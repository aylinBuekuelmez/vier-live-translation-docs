# Microsoft Azure Speech Translation / Live Interpreter

**Recherche-Stand:** 20.10.2026  
**Bezugsform:** Cloudbasierter API-Dienst

**Relevante Lösungen:**

- Real-time Speech Translation (Standard)
- Live Interpreter

**Produktdokumentation:** [Microsoft – What is Speech Translation?](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-translation)

## Kurzüberblick

Microsoft Azure Speech stellt Funktionen für Spracherkennung (STT), Textübersetzung und Sprachsynthese (TTS) zur Verfügung.

Für die Echtzeit-Sprachübersetzung unterscheidet Microsoft zwei Varianten:

- **Speech Translation Standard:** Verarbeitet einen Audiostream aus einer festgelegten Eingangssprache und liefert übersetzten Text, synthetisierte Sprache oder beides.
- **Live Interpreter:** Erkennt die gesprochene Eingangssprache automatisch, unterstützt Sprachwechsel innerhalb einer Sitzung und erzeugt übersetzte Sprache unter Berücksichtigung des Sprechstils und Tonfalls.

Die Standardvariante verwendet eine mehrstufige Verarbeitung:

**Audio → Speech-to-Text → Azure Translator → Text-to-Speech → übersetztes Audio**

Die TTS-Stufe wird nur benötigt, wenn eine gesprochene Übersetzung ausgegeben werden soll.


**Quellen:** 

- [Microsoft – Speech Translation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-translation) 
- [Microsoft – Data and Privacy](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/speech-service/speech-to-text/data-privacy-security)


## Kriteriencheck Microsoft Azure

| Kriterium | Speech Translation Standard | Live Interpreter |
| --- | --- | --- |
| Live-Eignung | Ja, Echtzeitübersetzung mit Text- und Audioausgabe | Ja, Echtzeit-Speech-to-Speech |
| Architektur | Kaskadiert: STT → Translator → optional TTS | Interne Architektur nicht eindeutig dokumentiert |
| Streaming | Ja, Audiostreams und Zwischenergebnisse | Ja, kontinuierliche Echtzeitverarbeitung |
| Sprachen | DE, EN, BG, PL, FR, IT, ES für STT und Textübersetzung dokumentiert | Automatische Erkennung zahlreicher Eingangssprachen; gesprochene Ziele gesondert zu prüfen |
| Bidirektionalität | Textübersetzung grundsätzlich in beide Richtungen möglich; Audioausgabe separat prüfen | Sprachwechsel möglich; vollständige S2S-Unterstützung aller Sprachpaare nicht bestätigt |
| Telefonie | 8-/16-kHz-PCM-Eingabe über Speech SDK dokumentiert | Direkte Telefoniecodec-Unterstützung nicht eindeutig belegt |
| Schnittstellen | Speech SDK, Speech CLI, Streaming | Speech SDK, WebSocket-basierter v2-Endpunkt |
| API/Testbarkeit | Dokumentierte SDK-Beispiele und Speech Studio | Dokumentierte API und Beispielcode |
| Produktstatus | Etablierte Speech-Funktion; ausdrückliches Statuslabel in den untersuchten Auszügen nicht angegeben | GA |
| EU-Verarbeitung | Unter anderem Deutschland und West Europe | West Europe; Deutschland nicht in der Live-Interpreter-Regionentabelle aufgeführt |
| Self-Hosting | Keine vollständige lokale Speech-Translation-Pipeline als Produkt nachgewiesen | Kein Self-Hosting nachgewiesen |
| Terminologie | Azure bietet Phrase Lists, Custom Speech und Custom Translator; direkte Kombinierbarkeit prüfen | Direkte Unterstützung dieser Anpassungen nicht bestätigt |
| Preis | 2,50 USD je Audiostunde | Getrennte Preise für Eingabeaudio und Ausgaben |
| Lizenz | Microsoft-Azure-Produktbedingungen | Microsoft-Azure-Produktbedingungen |

---

## 1. Eignung für Live Translation

Microsoft dokumentiert für beide Varianten eine mehrsprachige Echtzeit-Sprachübersetzung.

**Speech Translation Standard** kann eingehende Sprache während einer laufenden Audioübertragung erkennen und übersetzen. Die Ergebnisse können als Text oder synthetisierte Sprache ausgegeben werden.

**Live Interpreter** ist insbesondere für mehrsprachige Gesprächssituationen ausgelegt. Microsoft nennt unter anderem Geschäftstreffen und Reiseübersetzungen als Anwendungsfälle.

Microsoft führt außerdem Contact Center als Anwendungsbereich von Azure Speech auf, beispielsweise für die Echtzeittranskription von Telefongesprächen. Eine unmittelbare Telefonanlagenintegration von Live Interpreter wird in der Übersichtsquelle jedoch nicht ausdrücklich beschrieben.

**Quellen:** [Speech Translation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-translation) · [Azure Speech Overview](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/overview)

## 2. Architektur

**Speech Translation Standard – kaskadierte Architektur:**

1. **Speech-to-Text:** Wandelt eingehendes Audio in Text um.
2. **Azure Translator:** Übersetzt den erkannten Text in die gewünschte Zielsprache.
3. **Text-to-Speech:** Erzeugt optional eine gesprochene Ausgabe.

Die Verarbeitung erfolgt somit über mehrere hintereinandergeschaltete Funktionen.

**Live Interpreter:** Microsoft beschreibt die Lösung als integrierte Echtzeit-Speech-to-Speech-Übersetzung. Die genaue interne Modellarchitektur, insbesondere ob ein direktes Audio-zu-Audio-Modell oder mehrere interne Verarbeitungsschritte verwendet werden, wird nicht eindeutig angegeben.

**Quellen:** [Speech Translation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-translation) · [Data and Privacy](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/speech-service/speech-to-text/data-privacy-security)

## 3. Realtime-/Streaming-Fähigkeit

Azure Speech unterstützt die Verarbeitung kontinuierlicher Audiostreams.

Bei **Speech Translation Standard** werden während der Spracherkennung vorläufige Transkriptions- und Übersetzungsergebnisse bereitgestellt. Nach der Verarbeitung eines Sprachabschnitts können endgültige Ergebnisse ausgegeben und in Sprache umgewandelt werden.

Die SDK-Dokumentation beschreibt dafür unter anderem die Ereignisse `recognizing` und `recognized`. Bei einer bestimmten mehrsprachigen Übersetzungskonfiguration sind vorläufige Übersetzungsergebnisse laut Microsoft allerdings nicht verfügbar.

**Live Interpreter** ermöglicht kontinuierliche Sprachübersetzung mit automatischer Erkennung und Wechsel der Eingangssprache. Microsoft beschreibt die Übersetzung als latenzarm.

Eine garantierte vollständige Ende-zu-Ende-Latenz in Millisekunden ist in den untersuchten Quellen nicht angegeben.

**Quellen:** [Speech Translation](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-translation) · [How to Translate Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-translate-speech)

## 4. Sprachunterstützung und Bidirektionalität

Microsoft veröffentlicht getrennte Sprachlisten für Spracherkennung, Textübersetzung und Sprachausgabe.

Die priorisierten Projektsprachen sind in der Speech-to-Text-Liste und als Ziele der Textübersetzung aufgeführt.

| Sprache | Spracherkennung (STT) | Textübersetzung |
| --- | --- | --- |
| Deutsch | `de-DE` | `de` |
| Englisch | `en-US` | `en` |
| Bulgarisch | `bg-BG` | `bg` |
| Polnisch | `pl-PL` | `pl` |
| Französisch | `fr-FR` | `fr` |
| Italienisch | `it-IT` | `it` |
| Spanisch | `es-ES` | `es` |

Weitere dokumentierte Spracherkennungssprachen sind unter anderem Türkisch, Russisch, Vietnamesisch, Bengalisch, Estnisch sowie mehrere regionale Varianten des Arabischen.

**Bidirektionalität:** Die aufgeführten Sprachen können grundsätzlich als Ausgangs- und Textzielsprachen verwendet werden. Für die vollständige gesprochene Übersetzung müssen zusätzlich geeignete TTS-Stimmen beziehungsweise die entsprechenden Audioausgabefunktionen verfügbar sein.

Bei Live Interpreter verweist Microsoft auf die Speech-to-Text-Liste für Eingabesprachen und die Speech-Translation-Liste für Zielsprachen. Aus der Unterstützung der Textübersetzung lässt sich nicht automatisch jede bidirektionale Speech-to-Speech-Kombination bestätigen.

**Quelle:** [Microsoft – Language and Voice Support](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/language-support)

## 5. Telefonie-/Audio-Kompatibilität

Das Azure Speech SDK unterstützt benutzerdefinierte Audiostreams mit folgenden Eigenschaften:

| Eigenschaft | Dokumentiertes Format |
| --- | --- |
| Codierung | Unkomprimiertes PCM |
| Bittiefe | 16 Bit |
| Kanäle | Mono |
| Abtastrate | 8.000 oder 16.000 Hz |
| Byte-Reihenfolge | Little-Endian |

Damit kann die dokumentierte Eingabeschnittstelle auch 8-kHz-Audio verarbeiten. Abweichende Audioformate müssen für diese Schnittstelle gegebenenfalls umgewandelt werden.

**Audioausgabe:** Die separate Text-to-Speech-REST-API unterstützt unter anderem `raw-8khz-8bit-mono-alaw`, `raw-8khz-8bit-mono-mulaw` und PCM-Ausgabe mit 8 beziehungsweise 16 kHz.

A-law und μ-law sind telefonietypische Audioformate. Die Unterstützung durch die eigenständige TTS-REST-API bestätigt jedoch keine direkte G.711-Eingabe oder -Ausgabe innerhalb von Live Interpreter.

**Quellen:** [Audio Input Streams](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-use-audio-input-streams) · [TTS REST API](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/rest-text-to-speech)

## 6. Technische Evaluierbarkeit

Microsoft stellt das **Speech SDK**, die Speech CLI, Speech Studio sowie GitHub-Beispiele für die Integration und Erprobung der Sprachfunktionen bereit.

Für die Konfiguration können eine Azure-Ressource, ein API-Schlüssel und eine Region verwendet werden. Die Sprachübersetzung unterstützt kontinuierliche Erkennung über das SDK.

**WebSocket:** Für Live Interpreter dokumentiert Microsoft folgenden v2-Endpunkt:

`wss://<resource-name>.cognitiveservices.azure.com/stt/speech/universal/v2`

Die API kann mit dem Speech SDK angesprochen werden. Für die Verwendung persönlicher Stimmen gelten zusätzliche Zugangsvoraussetzungen; vorgefertigte Stimmen sind davon zu unterscheiden.

**Nutzungsgrenzen:** Für Echtzeit-Speech-to-Text und Speech Translation gelten gemeinsam standardmäßig eine gleichzeitige Anfrage im F0-Tarif beziehungsweise 100 gleichzeitige Anfragen im S0-Tarif. Das S0-Limit kann auf Antrag erhöht werden.

Ein gesondertes Limit für Live-Interpreter-Sitzungen wird durch diese allgemeine Angabe nicht nachgewiesen.

**Quellen:** [How to Translate Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-translate-speech) · [Quotas and Limits](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-services-quotas-and-limits)

## 7. Produktreife / Entwicklungsstatus

**Live Interpreter:** Microsoft hat die allgemeine Verfügbarkeit (*General Availability, GA*) in einer offiziellen Veröffentlichung vom Dezember 2025 bestätigt.

**Speech Translation Standard:** Die Funktion wird als regulärer Bestandteil von Azure Speech dokumentiert. Ein gesondertes GA-Einführungsdatum ist in den untersuchten Dokumenten nicht angegeben.

Die ebenfalls angebotene Funktion Azure LLM Speech ist ein eigenständiger Dienst und nicht mit Live Interpreter gleichzusetzen.

**Quelle:** [Microsoft – What's New in Microsoft Foundry](https://devblogs.microsoft.com/foundry/whats-new-in-microsoft-foundry-oct-nov-2025/)

## 8. Verarbeitungsort / Datenregion

Microsoft dokumentiert die regionalen Verfügbarkeiten für Speech Translation Standard und Live Interpreter getrennt.

| Azure-Region | Standard | Live Interpreter |
| --- | --- | --- |
| Germany West Central | Ja | Nicht aufgeführt |
| West Europe | Ja | Ja |
| North Europe | Ja | Nicht aufgeführt |
| France Central | Ja | Nicht aufgeführt |

Beide Varianten verfügen somit über eine dokumentierte EU-Region. Eine deutsche Region ist in der funktionsspezifischen Tabelle für die Standardvariante ausgewiesen.

Microsoft gibt an, dass Azure Speech Sprachdaten grundsätzlich in der Region der jeweiligen Azure-Speech-Ressource verarbeitet und speichert. Für die dokumentierte Echtzeit-Spracherkennung und Sprachübersetzung wird außerdem beschrieben, dass Kundendaten nach der Verarbeitung nicht dauerhaft gespeichert werden.

**Self-Hosting:** Microsoft bietet Container für bestimmte Speech-Funktionen an. Eine vollständige lokale Bereitstellung von Live Interpreter wird in den untersuchten Dokumenten nicht bestätigt.

**Quellen:** [Supported Regions](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/regions?tabs=speech-translation) · [Data and Privacy](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/speech-service/speech-to-text/data-privacy-security)

## 9. Lizenz und kommerzielle Nutzbarkeit

Azure Speech ist ein kommerzieller Cloud-Dienst unter den Microsoft-Azure-Produktbedingungen.

Die Bedingungen beschreiben unter anderem kommerzielle Nutzungsrechte für mit vorgefertigten neuronalen Stimmen erzeugte Audioausgaben. Für benutzerdefinierte Stimmen gelten zusätzliche Anforderungen, insbesondere hinsichtlich der Zustimmung der betroffenen Sprecher.

### Preise

| Leistung | Preis laut Microsoft |
| --- | --- |
| Speech Translation Standard | 2,50 USD pro Audiostunde |
| Live Interpreter – Audioeingabe | 1,00 USD pro Audiostunde |
| Live Interpreter – Textausgabe | 10,00 USD pro 1 Mio. Zeichen |
| Live Interpreter – Audioausgabe, Standardstimme | 1,50 USD pro Audiostunde |
| Live Interpreter – Audioausgabe, Custom Voice | 2,00 USD pro Audiostunde |

Der Standardpreis umfasst eine Audioeingabe, eine Audioausgabe und bis zu zwei Textübersetzungszielsprachen.

Bei Live Interpreter umfasst der Preis für Audioausgabe bereits die Textausgabe. Diese wird bei entsprechender Konfiguration nicht zusätzlich abgerechnet.

Im kostenlosen F0-Tarif stehen für Speech Translation Standard fünf Audiostunden pro Monat zur Verfügung. Ein kostenloses Live-Interpreter-Kontingent wird in der vorliegenden Preisliste nicht ausgewiesen.

**Benchmark-Veröffentlichung:** Eine ausdrückliche allgemeine Freigabe zur öffentlichen Veröffentlichung von Live-Interpreter-Benchmarks ist in den untersuchten Produktbedingungen nicht eindeutig nachgewiesen.

**Quellen:** [Azure Speech Pricing](https://azure.microsoft.com/en-us/pricing/details/speech/) · [Microsoft Azure Product Terms](https://www.microsoft.com/licensing/terms/de-DE/product/MicrosoftAzure/all)

## 10. Terminologie-/Kontextanpassung

Microsoft bietet verschiedene Anpassungsmechanismen für die Spracherkennung und Übersetzung.

**Phrase Lists:** Ermöglichen die bevorzugte Erkennung bestimmter Begriffe, beispielsweise Unternehmensnamen, Produktnamen und Fachausdrücke. Für unterstützte Echtzeit-Spracherkennung kann die Gewichtung zwischen `0.0` und `2.0` eingestellt werden.

**Custom Speech:** Ermöglicht die Anpassung der Spracherkennung mit eigenen Text-, Audio- und Aussprachedaten. Der verfügbare Funktionsumfang unterscheidet sich nach Sprache.

**Custom Translator:** Ermöglicht die Anpassung der Textübersetzung mit eigenen Übersetzungsdaten, beispielsweise für Fachgebiete und unternehmensspezifische Terminologie.

Diese Anpassungsfunktionen sind für bestimmte Azure-Speech- und Translator-Dienste dokumentiert. Ihre direkte Verwendbarkeit innerhalb von Live Interpreter wird durch die untersuchten Quellen nicht vollständig bestätigt.

**Quellen:** [Phrase Lists](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/improve-accuracy-phrase-list) · [Custom Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/custom-speech-overview) · [Microsoft Translator FAQ](https://www.microsoft.com/de-de/translator/business/faq/)

## Hauptstärken Microsoft Azure

- **Echtzeit-Sprachübersetzung:** Speech Translation Standard und Live Interpreter unterstützen Echtzeitübersetzung mit gesprochener Ausgabe.
- **Automatische Spracherkennung:** Live Interpreter erkennt die Eingangssprache automatisch und unterstützt Sprachwechsel während einer Sitzung.
- **Mehrsprachigkeit:** Alle sieben priorisierten Projektsprachen sind für Spracherkennung und Textübersetzung dokumentiert.
- **EU-Verarbeitung:** Europäische Bereitstellungsregionen vorhanden; Deutschland wird für Speech Translation Standard unterstützt.
- **Schnittstellen und Kosten:** Speech SDK, Streaming und ein WebSocket-basierter Endpunkt dokumentiert; nutzungsabhängiges Preismodell.

## Haupteinschränkungen

- **Modellarchitektur:** Die interne Architektur von Live Interpreter ist nicht eindeutig dokumentiert.
- **Sprachunterstützung:** Vollständige bidirektionale Speech-to-Speech-Unterstützung nicht für alle priorisierten Sprachpaare nachgewiesen.
- **Telefonie:** Direkte G.711-Unterstützung und Telefonanlagenintegration von Live Interpreter nicht bestätigt.
- **Latenz:** Keine garantierte Ende-zu-Ende-Latenz für die vollständige gesprochene Übersetzung angegeben.
- **Bereitstellung und Anpassung:** Keine vollständige lokale Live-Interpreter-Bereitstellung dokumentiert; direkte Terminologieanpassung innerhalb von Live Interpreter nicht eindeutig bestätigt.
