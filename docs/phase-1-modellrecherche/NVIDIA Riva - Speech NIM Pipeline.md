# NVIDIA Riva / Speech NIM Pipeline

**Recherche-Stand:** 20.10.2026  
**Dokumentationsversion:** NVIDIA Speech NIM 26.10.0  
**Bezugsform:** GPU-beschleunigte Microservices, unter anderem als selbst betriebene Container

**Relevante Komponenten:**

| Komponente | Modell | Funktion |
| --- | --- | --- |
| Speech-to-Text (ASR) | Nemotron ASR Streaming Multilingual | Kontinuierliche mehrsprachige Spracherkennung |
| Übersetzung (NMT) | Riva Translate 1.6B | Übersetzung von Text zwischen Sprachen |
| Text-to-Speech (TTS) | Magpie TTS Multilingual | Mehrsprachige synthetisierte Sprachausgabe |
| Alternative TTS | Chatterbox TTS Multilingual | Mehrsprachige Sprachausgabe, unter anderem Polnisch |

**Produktdokumentation:** <https://docs.nvidia.com/nim/speech/26.10.0/> 

<https://docs.nvidia.com/nim/speech/26.10.0/tts/index.html> 

## Kurzüberblick

NVIDIA Speech NIM stellt getrennte Microservices für Spracherkennung (ASR), Textübersetzung (NMT) und Sprachsynthese (TTS) bereit.

Diese Dienste können zu einer gemeinsamen Echtzeit-Speech-to-Speech-Pipeline verbunden werden.

**Pipeline:**

Eingehendes Audio → ASR NIM → erkannter Text → Riva Translate 1.6B → übersetzter Text → TTS NIM → übersetztes Audio

NVIDIA dokumentiert hierfür eine vollständige Streaming-Beispielimplementierung.

Der NMT-Microservice übernimmt dabei die Koordination der Gesamtpipeline und kommuniziert mit den ASR- und TTS-Diensten über gRPC.

Die offizielle Beispielimplementierung übersetzt **gesprochenes Englisch in lateinamerikanisches Spanisch**.

Dafür werden folgende Modelle eingesetzt:

- Parakeet 1.1B CTC English für die Spracherkennung
- Riva Translate 1.6B für die Textübersetzung
- Magpie TTS Multilingual für die gesprochene Ausgabe

Nemotron ASR Streaming Multilingual und Chatterbox TTS Multilingual sind ebenfalls dokumentierte Modelle, gehören jedoch nicht zur genannten Beispielkonfiguration.

**Quelle:** [NVIDIA – Run Streaming Speech-to-Speech Translation](https://docs.nvidia.com/nim/speech/26.10.0/nmt/speech-to-speech-translation.html)

## Kriteriencheck NVIDIA Speech NIM

| Kriterium | Ergebnis |
| --- | --- |
| Live-Eignung | Ja, vollständige Streaming-Speech-to-Speech-Beispielpipeline dokumentiert |
| Architektur | Kaskadiert: ASR → NMT → TTS |
| Streaming | Ja, ASR und TTS unterstützen Streaming; integrierte S2S-API dokumentiert |
| Schnittstellen | gRPC, HTTP und WebSocket, je nach Microservice und Endpunkt |
| Sprachen | ASR und Riva Translate unterstützen alle sieben priorisierten Projektsprachen |
| Bidirektionalität | Textübersetzung durch Riva Translate in beide Richtungen; vollständiges S2S abhängig von der TTS-Sprache |
| Telefonie | ASR-Schnittstelle dokumentiert PCM, G.711 μ-law und A-law sowie weitere Formate |
| API/Testbarkeit | Dokumentierte Python-Clients, Docker-Compose-Beispiel und Jupyter-Notebook |
| Produktstatus | Speech NIM 26.10.0 als offizielle Release dokumentiert; Modellstatus separat zu betrachten |
| EU-Verarbeitung | Bei eigener Bereitstellung auf EU-Infrastruktur möglich |
| Self-Hosting | Ja, Docker- und Kubernetes-/Helm-Bereitstellung dokumentiert |
| Datenschutz | Verarbeitungsort bei Eigenbetrieb kontrollierbar; Telemetrie- und Lizenzdatenflüsse beachten |
| Lizenz | NVIDIA-Produktbedingungen; Community-Modell Chatterbox zusätzlich MIT-lizenziert |
| Terminologie | Word Boosting für ASR und Custom Dictionaries für die Übersetzung |
| Preis | GPU-basierte Software- und Infrastrukturkosten; kein allgemeiner fester Preis je übersetzter Audiostunde |
| Benchmark-Veröffentlichung | Grundsätzlich eingeschränkt; vorherige schriftliche Zustimmung oder einschlägige Ausnahme erforderlich |

---

## 1. Eignung für Live Translation

NVIDIA dokumentiert eine Streaming-Speech-to-Speech-Pipeline, bei der eingehendes Audio erkannt, der resultierende Text übersetzt und anschließend als Sprache ausgegeben wird.

Die Verarbeitung erfolgt über drei miteinander verbundene Microservices.

Die offizielle Beispielimplementierung zeigt den vollständigen Ablauf für Englisch → Spanisch. Andere Sprachrichtungen hängen von der Sprachunterstützung der verwendeten ASR-, NMT- und TTS-Modelle ab.

Damit ist eine funktionierende Streaming-Speech-to-Speech-Architektur dokumentiert. Die Verwendung alternativer Modelle innerhalb der vollständigen Pipeline ist nicht für jede Kombination durch ein offizielles Beispiel belegt.

**Quelle:** [NVIDIA – Streaming Speech-to-Speech Translation](https://docs.nvidia.com/nim/speech/26.10.0/nmt/speech-to-speech-translation.html)

## 2. Architektur

NVIDIA Speech NIM verwendet eine **kaskadierte Microservice-Architektur**:

1. **ASR – Automatic Speech Recognition:** Wandelt gesprochene Sprache in Text um.
2. **NMT – Neural Machine Translation:** Übersetzt den erkannten Text.
3. **TTS – Text-to-Speech:** Erzeugt aus der Übersetzung gesprochenes Audio.

Die Komponenten werden als eigenständige Container betrieben.

In der integrierten Speech-to-Speech-Pipeline koordiniert der NMT-Dienst die Verarbeitung. Die Kommunikation mit den anderen Diensten erfolgt über gRPC.

Dafür werden unter anderem die Umgebungsvariablen `NMT_REMOTE_ASR_SERVICE` und `NMT_REMOTE_TTS_SERVICE`konfiguriert.

NVIDIA stellt eine Docker-Compose-Konfiguration für den gemeinsamen Betrieb bereit. Bei Bedarf können die Komponenten auf unterschiedliche GPUs verteilt werden.

**Quellen:** [How Speech NIM Works](https://docs.nvidia.com/nim/speech/26.10.0/about/how-it-works.html) · [Streaming S2S](https://docs.nvidia.com/nim/speech/26.10.0/nmt/speech-to-speech-translation.html)

## 3. Realtime-/Streaming-Fähigkeit

Alle drei Verarbeitungsschritte verfügen über dokumentierte Streaming-Funktionen.

**Nemotron ASR Streaming:** Verarbeitet laufende Audiostreams und gibt Spracherkennungsergebnisse zurück. Die mehrsprachige Variante `type=multi` unterstützt automatische Sprachidentifikation.

**Riva Translate 1.6B:** Die NMT-Schnittstelle unterstützt neben Textübersetzung eine integrierte Streaming-Speech-to-Speech-Funktion.

**Magpie und Chatterbox:** Beide TTS-Modelle sind laut NVIDIA-Supportmatrix für Streaming und Offline-Verarbeitung verfügbar.

Für die separate Echtzeit-Sprachausgabe dokumentiert NVIDIA außerdem eine WebSocket-Schnittstelle, über die laufend Text gesendet und generierte Audiodaten empfangen werden können.

Die NVIDIA-Dokumentation enthält Performance-Messungen für einzelne Modelle. Ein allgemeiner garantierter Ende-zu-Ende-Latenzwert für eine beliebige vollständige ASR-NMT-TTS-Pipeline wird jedoch nicht angegeben.

**Quellen:** [ASR Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/asr.html) · [TTS Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/tts.html) · [Realtime TTS API](https://docs.nvidia.com/nim/speech/26.10.0/reference/api-references/tts/realtime-tts.html)

## 4. Sprachunterstützung und Bidirektionalität

Die Sprachunterstützung muss für alle drei Komponenten getrennt betrachtet werden.

**Nemotron ASR Streaming Multilingual** unterstützt 40 Sprachvarianten. NVIDIA unterscheidet die Kategorien *Transcription-ready*, *Broad-coverage* und *Adaptation-ready*.

**Riva Translate 1.6B** unterstützt laut Supportmatrix Textübersetzungen zwischen 36 dokumentierten Sprachcodes in unterschiedlichen Richtungen (*any-to-any*). Die Übersetzung aus dem Englischen wird außerdem in der offiziellen Beispielpipeline gezeigt.

Bei der Sprachausgabe unterscheiden sich Magpie und Chatterbox.

| Sprache | Nemotron ASR | Riva Translate | Magpie TTS | Chatterbox TTS |
| --- | --- | --- | --- | --- |
| Deutsch | Ja | Ja | Ja | Ja |
| Englisch | Ja | Beispiel dokumentiert | Ja | Ja |
| Bulgarisch | Ja | Ja | Nein | Nein |
| Polnisch | Ja | Ja | Nein | Ja |
| Französisch | Ja | Ja | Ja | Ja |
| Italienisch | Ja | Ja | Ja | Ja |
| Spanisch | Ja | Ja | Ja | Ja |

**Magpie TTS Multilingual** unterstützt zwölf Sprachvarianten, darunter Deutsch, Englisch, Französisch, Italienisch und Spanisch.

**Chatterbox TTS Multilingual** unterstützt 23 Sprachen, darunter zusätzlich Polnisch, Russisch und Türkisch.

**Bidirektionalität:** Für Deutsch ↔ Englisch sind die benötigten Sprachfunktionen in allen drei Verarbeitungsschritten dokumentiert. Für eine polnische Sprachausgabe kann Chatterbox eingesetzt werden. Bulgarische Sprachausgabe wird von keinem der beiden untersuchten TTS-Modelle aufgeführt.

Die erfolgreiche Kombination aller Modelle muss bei der eigenen Implementierung geprüft werden.

**Quellen:** [ASR Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/asr.html) · [NMT Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/nmt.html) · <https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/tts.html> 

## 5. Telefonie-/Audio-Kompatibilität

Die NVIDIA-ASR-gRPC-Schnittstelle dokumentiert verschiedene Audioeingabeformate:

| Format | Beschreibung |
| --- | --- |
| `LINEAR_PCM` | Unkomprimiertes PCM-Audio |
| `MULAW` | G.711 μ-law |
| `ALAW` | G.711 A-law |
| `FLAC` | Verlustfrei komprimiertes Audio |
| `OGGOPUS` | Opus-Audio |

G.711 μ-law und A-law sind für klassische Telefonie relevant.

Die Audio-Abtastrate wird über `sample_rate_hertz` angegeben. NVIDIA dokumentiert außerdem eine interne Anpassung der Abtastrate auf das vom Modell benötigte Format.

Für die TTS-Ausgabe sind separate Audioformate, darunter PCM und Opus, dokumentiert.

Die G.711-Unterstützung auf der ASR-Eingabeseite bedeutet nicht automatisch, dass die gesamte Speech-to-Speech-Pipeline G.711 ohne zusätzliche Konvertierung ausgeben kann.

**Quellen:** [ASR gRPC API](https://docs.nvidia.com/nim/speech/26.10.0/reference/api-references/asr/protos.html) · [TTS API References](https://docs.nvidia.com/nim/speech/26.10.0/reference/api-references/tts/index.html)

## 6. Technische Evaluierbarkeit

NVIDIA stellt verschiedene Schnittstellen und Testmöglichkeiten zur Verfügung.

| Schnittstelle | Verwendung |
| --- | --- |
| gRPC | ASR, NMT, TTS und integrierte Streaming-S2S-Pipeline |
| WebSocket | Echtzeit-ASR beziehungsweise Echtzeit-TTS |
| HTTP | Weitere API-Endpunkte |
| Python-Clients | Programmatischer Zugriff auf die Dienste |

Zusätzlich dokumentiert NVIDIA Docker-Compose-Beispiele, Python-Skripte und ein Jupyter-Notebook für die Streaming-Sprachübersetzung.

Für den Zugriff auf NVIDIA-Container und Modellressourcen wird ein NGC-Zugang benötigt.

### Hardwareanforderungen

NVIDIA nennt für die Speech-NIM-Dienste grundsätzlich eine kompatible NVIDIA-GPU mit Compute Capability mindestens 8.0 und mindestens 16 GB VRAM.

Der tatsächliche Speicherbedarf hängt vom Modellprofil ab.

| Modell / Profil | GPU-Modellspeicher |
| --- | --- |
| Nemotron ASR Streaming Multilingual, Batch 32 | 4 GB |
| Riva Translate 1.6B | 9,5 GB |
| Magpie TTS Multilingual, Batch 8 | 12,58 GiB |
| Chatterbox TTS Multilingual, Batch 8 | 44,61 GiB |

Die angegebenen Werte beziehen sich auf einzelne Modellprofile. Für den gemeinsamen Betrieb müssen die Ressourcen für alle verwendeten Dienste bereitgestellt werden.

**Quellen:** [Streaming S2S Tutorial](https://docs.nvidia.com/nim/speech/26.10.0/nmt/speech-to-speech-translation.html) · [ASR Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/asr.html) · [NMT Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/nmt.html) · [TTS Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/tts.html)

## 7. Produktreife / Entwicklungsstatus

Die untersuchten Komponenten werden in der offiziellen NVIDIA-Speech-NIM-Dokumentation der Version **26.10.0**aufgeführt.

**Nemotron ASR Streaming:** Unterstütztes NVIDIA-Modell für kontinuierliche Spracherkennung.

**Riva Translate 1.6B:** Unterstütztes Modell zur neuronalen Textübersetzung.

**Magpie TTS Multilingual:** NVIDIA-Modell für Streaming- und Offline-Sprachausgabe.

**Chatterbox TTS Multilingual:** Community-Modell von Resemble AI, das über NVIDIA NIM bereitgestellt wird.

NVIDIA dokumentiert für Chatterbox bekannte Einschränkungen bei nichtenglischen Sprachen, beispielsweise Aussprachefehler, gemischte Sprachausgaben und unerwünschte Änderungen der Sprechgeschwindigkeit.

Ein einheitlicher GA-Status für jede selbst zusammengestellte Gesamtpipeline wird nicht ausdrücklich angegeben.

**Quellen:** [NVIDIA Release Notes](https://docs.nvidia.com/nim/speech/26.10.0/about/release-notes.html) · [TTS Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/tts.html)

## 8. Verarbeitungsort / Datenregion

NVIDIA Speech NIM unterstützt Self-Hosting auf eigener oder angemieteter GPU-Infrastruktur.

Für die Bereitstellung dokumentiert NVIDIA:

- **Docker:** Betrieb einzelner Speech-NIM-Container.
- **Kubernetes/Helm:** Verwaltung mehrerer containerisierter Dienste.

Der Verarbeitungsort hängt beim Self-Hosting vom Standort der eingesetzten Infrastruktur ab. Eine Verarbeitung auf eigenen Servern innerhalb Deutschlands oder der EU ist damit technisch möglich.

### Datenschutz und Datenflüsse

Bei einer selbst betriebenen Pipeline können Audiodaten, Transkripte und Übersetzungen innerhalb der eigenen Infrastruktur verarbeitet werden.

NVIDIA beschreibt in seinen Produktbedingungen mögliche Telemetriedaten, unter anderem zu Konfiguration, Nutzung und Hardware.

Die dokumentierte NIM-Telemetrie kann über folgende Einstellung deaktiviert werden:

`NIM_TELEMETRY_MODE=0`

Weitere Datenflüsse, etwa durch Modell-Downloads, Authentifizierung oder eingebundene externe Dienste, hängen von der konkreten Bereitstellung ab.

**Quellen:** [Speech NIM Deployment](https://docs.nvidia.com/nim/speech/26.10.0/deployment/index.html) · [NVIDIA Product Specific Terms](https://www.nvidia.com/en-us/agreements/enterprise-software/product-specific-terms-for-ai-products/)

## 9. Lizenz und kommerzielle Nutzbarkeit

Für NVIDIA Speech NIM gelten die jeweiligen NVIDIA-Software- und Produktbedingungen.

Bei Self-Hosting entstehen Kosten insbesondere durch Softwarelizenzen, GPU-Hardware beziehungsweise Cloud-GPUs sowie den Betrieb der Infrastruktur.

### Preise

Die NVIDIA-AI-Enterprise-Preisliste nennt folgende Listenpreise:

| Lizenzmodell | Listenpreis |
| --- | --- |
| Self-managed, 1 Jahr | 4.500 USD/GPU |
| Self-managed, 3 Jahre | 13.500 USD/GPU |
| Self-managed, 5 Jahre | 18.000 USD/GPU |

Für qualifizierte Bildungs- und Forschungseinrichtungen bietet NVIDIA gesonderte Konditionen an.

Die Beträge sind Lizenzpreise und stellen keinen festen Preis pro übersetzter Audiostunde dar. Die tatsächlichen Kosten hängen von Hardware, Modellkonfiguration, Auslastung und Betriebsdauer ab.

### Chatterbox-Lizenz

Chatterbox TTS Multilingual wurde von Resemble AI entwickelt. Das Ursprungsmodell wird von NVIDIA als MIT-lizenziert beschrieben. Bei Nutzung über NVIDIA-Dienste gelten zusätzlich die entsprechenden Nutzungsbedingungen.

### Benchmark-Veröffentlichung

Die NVIDIA Software License Agreement enthält eine ausdrückliche Einschränkung hinsichtlich der Weitergabe von Benchmark-, Wettbewerbsanalyse- und Leistungsdaten.

Die Weitergabe an Dritte setzt grundsätzlich eine vorherige schriftliche Zustimmung von NVIDIA oder eine ausdrücklich vorgesehene Ausnahme voraus.

Für die öffentliche Veröffentlichung von Ergebnissen des VIER-Projekts müssen deshalb die Bedingungen der tatsächlich verwendeten Lizenz geprüft werden.

**Quellen:** [NVIDIA Pricing](https://docs.nvidia.com/ai-enterprise/planning-resource/licensing-guide/latest/pricing.html) · [NVIDIA Software License Agreement](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-software-license-agreement/) · [TTS Support Matrix](https://docs.nvidia.com/nim/speech/26.10.0/reference/support-matrix/tts.html)

## 10. Terminologie-/Kontextanpassung

NVIDIA dokumentiert mehrere Möglichkeiten zur Anpassung der einzelnen Verarbeitungsschritte.

### ASR – Word Boosting

Nemotron ASR Streaming unterstützt Word Boosting, um bestimmte Wörter oder Ausdrücke bei der Spracherkennung stärker zu berücksichtigen.

Typische Anwendungsfälle sind Firmennamen, Produktbezeichnungen und technische Fachbegriffe.

Die Gewichtung kann dynamisch pro Anfrage beziehungsweise Audiostream erfolgen. NVIDIA dokumentiert für Nemotron ASR Streaming einen Wertebereich von `-200` bis `200`.

### NMT – Custom Translation Dictionaries

Riva Translate unterstützt benutzerdefinierte Übersetzungswörterbücher.

Damit lassen sich bestimmte Übersetzungen vorgeben oder einzelne Wörter von der Übersetzung ausschließen.

Dokumentiertes Format:

`source_word##target_word`

Zusätzlich können `<dnt>`-Tags („do not translate“) verwendet werden, um einzelne Begriffe unverändert zu übernehmen.

### TTS – Ausspracheanpassung

**Magpie TTS Multilingual** unterstützt dokumentierte Ausspracheanpassungen über SSML, beispielsweise mit `<phoneme>`, sowie benutzerdefinierte Aussprachewörterbücher.

**Chatterbox TTS Multilingual** unterstützt diese SSML- und Wörterbuchfunktionen laut Supporttabelle nicht. Es bietet stattdessen unter anderem einen Parameter zur Steuerung der emotionalen Ausdrucksstärke (`exaggeration_factor`).

**Quellen:** [ASR Customization](https://docs.nvidia.com/nim/speech/26.10.0/asr/customization/customization.html) · [Custom Translation Dictionaries](https://docs.nvidia.com/nim/speech/26.10.0/nmt/custom-dictionaries.html) · [TTS Customization](https://docs.nvidia.com/nim/speech/26.10.0/tts/customization.html)

## Hauptstärken NVIDIA Speech NIM

- **Echtzeit-Sprachübersetzung:** Vollständige Streaming-Speech-to-Speech-Pipeline aus ASR, NMT und TTS offiziell dokumentiert.
- **Flexible Bereitstellung:** Self-Hosting auf eigener GPU-Infrastruktur möglich, einschließlich Verarbeitung innerhalb Deutschlands oder der EU.
- **Mehrsprachigkeit:** Alle sieben priorisierten Projektsprachen für Spracherkennung und Textübersetzung dokumentiert.
- **Telefonie und Schnittstellen:** Telefonietypische Audioeingabeformate wie G.711 A-law/μ-law sowie gRPC-, HTTP- und WebSocket-Schnittstellen vorhanden.
- **Terminologieanpassung:** Word Boosting für Fachbegriffe und benutzerdefinierte Übersetzungswörterbücher verfügbar.

## Haupteinschränkungen

- **Technischer Aufwand:** Eigene GPU-Infrastruktur sowie Einrichtung und Betrieb mehrerer Microservices erforderlich.
- **Sprachausgabe:** Bulgarisch wird von den untersuchten TTS-Modellen nicht unterstützt; Polnisch nur durch Chatterbox.
- **Sprachqualität:** NVIDIA dokumentiert bei Chatterbox Einschränkungen bei der nichtenglischen Sprachausgabe.
- **Latenz und Kosten:** Keine allgemein garantierte Ende-zu-Ende-Latenz und kein einheitlicher Preis pro übersetzter Audiostunde.
- **Lizenzbedingungen:** Die Veröffentlichung von Benchmark-Ergebnissen unterliegt Einschränkungen und kann eine Genehmigung erfordern.
