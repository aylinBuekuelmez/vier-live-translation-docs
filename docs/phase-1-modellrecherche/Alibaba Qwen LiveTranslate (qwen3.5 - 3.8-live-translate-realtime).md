# Alibaba Qwen LiveTranslate \(qwen3\.5 / 3\.8\-live\-translate\-realtime\)

**Recherche-Stand:** 21.10.2026 

**Mögliche Bezugsform:** gehostete Echtzeit-API über Alibaba Cloud Model Studio oder QwenCloud

**Entwickler:** Alibaba Cloud, Qwen Team 

**Architektur:** Multimodales Echtzeitmodell auf Basis der Thinker-Talker-/Interleave-Architektur. Eingabe: Audio und optional Bildmaterial/Videoframes; Ausgabe: Text und/oder Audio.


**Inhaltsverzeichnis:**

---

# 1. Kriterien-Schnellübersicht

| **Kriterium** | **Bewertung** |
| --- | --- |
| **Eignung Live-Translation** | **Ja.** Vollwertiges Speech-to-Speech-Streaming. |
| **Architektur** | **Direktes S2ST (multimodal).** Interleave-Architektur ("Thinker-Talker").**Input:** Audio und optional Bildmaterial/Videoframes → **Output:** Text und/oder Audio. |
| **Streaming & Latenz** | Streaming wird unterstützt. Qwen nennt für Qwen3.5 ca. 2,8 Sekunden und für Qwen3.8 eine LAAL von 2,3 Sekunden; die Werte sind nicht direkt vergleichbar. |
| **Sprachunterstützung & Bidirektionalität** | **Umfassend.** DE↔EN vollständig; 60 Input- / 29 Output-Sprachen. |
| **Telefonie- & Audio-Kompatibilität** | Telefonie-Audio muss getestet werden; 8-kHz-Audio muss laut Doku nicht grundsätzlich umgewandelt werden. |
| **Technische Evaluierbarkeit** | Cloud-Evaluation möglich; Self-hosting mit den geprüften Quellen nicht belegt. |
| **Verarbeitungsort / Datenregion** | **Prüfbedarf**. Frankfurt ist für andere DashScope-Modelle dokumentiert, aber nicht für LiveTranslate. Für LiveTranslate sind in der geprüften Modellseite Beijing und Singapore aufgeführt; Datenresidenz muss beim Anbieter geklärt werden. |
| **Lizenz & Kommerzielle Nutzung** | **Abrechnung:** Nutzungsbasierte API; kommerzielle Produktnutzung vertraglich noch zu klären. |
| **Terminologie-/Kontextanpassung** | Hotwords unterstützt; bis zu 1.000 Einträge empfohlen. |
| **Produktreife / Entwicklungsstatus** | Kostenpflichtige API; in der Modellübersicht als „Stable“ geführt. |

---

# 2. Qwen3.5 und Qwen3.8 im Vergleich 

- **Gemeinsamkeiten**: Beide unterstützen 60 Sprachen für Audioeingabe/Textausgabe und 29 Sprachen für Sprachausgabe.
-  **Qwen3.8**: Neuere Version mit Echtzeit-Sprechertrennung, synchronisierter Anzeige von Original und Übersetzung sowie verbessertem Umgang mit längerem Gesprächskontext. 
- **Qwen3.5**: Die API-Dokumentation nennt zusätzlich Unterstützung für 8-kHz-Audio. Bei Qwen3.8 ist diese Samplerate auf der geprüften Seite nicht ausgewiesen.
- **LAAL:** steht für *Length-Adjusted Average Lagging*. Die Kennzahl schätzt, wie weit die Übersetzung der gesprochenen Eingabe zeitlich hinterherhinkt.
- **Latenz**: Qwen nennt für Qwen3.5 etwa 2,8 Sekunden und für Qwen3.8 eine LAAL von 2,3 Sekunden. Da die Angaben unterschiedlich bezeichnet sind, sollte man sie nicht als direkten Vergleich behandeln.
- **Integration**: Die API-Parameter unterscheiden sich teilweise zwischen den Versionen.

**Quellen:**

- <https://qwen.ai/blog?id=qwen3.5-livetranslate> (21.10.26)
- <https://qwen.ai/blog?id=qwen3.8-livetranslate> (21.10.26)
- <https://help.aliyun.com/en/model-studio/qwen3-8-livetranslate-flash-realtime> (21.10.26)
- <https://help.aliyun.com/en/model-studio/live-translator-client-events> (21.10.26)

---

# 3. Detaillierte Kriterien-Analyse

## 3.1. Eignung für Live Translation

Qwen LiveTranslate ist primär auf die simultane Übersetzung laufender Audiosignale ausgelegt und geht über reine Textübersetzung oder Batch-Verarbeitung weit hinaus. Das System verarbeitet eingehendes Audio kontinuierlich und stellt die Übersetzung simultan als parallele Audio- und Textausgabe bereit. Die tatsächliche Eignung muss mit repräsentativen Telefonaten geprüft werden.

**Quellen: **

- <https://help.aliyun.com/en/model-studio/qwen3-5-livetranslate-flash-realtime> 
- <https://onegen.ai/a-proven-qwen-livetranslate-in-risky-race-for-data/>  (21.10.26)
- <https://www.mindstudio.ai/blog/qwen3-8-livetranslate-hands-on> (21.10.26)


## 3.2. Architektur

Die Lösung verzichtet auf eine klassische, dreistufige Kaskade (ASR → MT → TTS). Stattdessen ist Qwen LiveTranslate ein direktes, **multimodales Speech-to-Speech-Modell (S2ST)**.

- **Multimodaler Input & Output:** Das System ist für multimodales Streaming konzipiert und verarbeitet als Input parallel Audio und optional Bilder/Videoframes. Als Output generiert es simultan eine direkte "Audio+Text-Ausgabe".


**Interleave-Architektur ("Thinker-Talker"):** 

Das Modell arbeitet im Grunde wie ein menschlicher Simultandolmetscher, der gleichzeitig zuhört, den Sinn versteht und sofort übersetzt.

- **Der "Thinker" (Zuhörer/Denker):** Der Thinker verarbeitet Audio und kann zusätzlich visuelle Informationen aus Bildern oder Videoframes berücksichtigen.
- **Der "Talker" (Sprecher):** Generiert parallel dazu bereits die gesprochene Übersetzung.

Statt starrer Einzel-Schritte (erst den kompletten Satz abwarten, dann als Text übersetzen, dann vorlesen) laufen diese Prozesse komplett ineinandergreifend (interleaved) ab. Audio- und visuelle Informationen können bei der Übersetzung berücksichtigt werden; die Verarbeitung erfolgt mit einer messbaren Latenz.

**Quellen:**

- <https://qwen.ai/blog?id=qwen3.5-livetranslate> (21.10.26)
- <https://qwen.ai/blog?id=qwen3.8-livetranslate> (21.10.26)
- <https://docs.qwencloud.com/developer-guides/speech/realtime-translation> (21.10.26)
- <https://onegen.ai/a-proven-qwen-livetranslate-in-risky-race-for-data/> (21.10.26)


## 3.3. Realtime-/Streaming-Fähigkeit

Das System übersetzt den laufenden Audiostrom im Streaming-Verfahren. Qwen nennt für Qwen3.8 eine LAAL von 2,3 Sekunden. Das ist ein Anbieter-Messwert und keine garantierte Gesprächslatenz.

**Quellen:**

- <https://qwen.ai/blog?id=qwen3.8-livetranslate> (21.10.26)


## 3.4. Sprachunterstützung und Bidirektionalität

Die geforderte Kernkombination Deutsch und Englisch (DE↔EN) wird vollständig und bidirektional für die Audio- und Textausgabe unterstützt. Zudem deckt das Modell wichtige europäische Sprachen (wie Französisch, Spanisch, Italienisch, Polnisch, Bulgarisch, Russisch) sowie internationale Sprachen (u. a. Vietnamesisch, Arabisch, Türkisch) über ein Spektrum von bis zu 60 Eingabe- und 29 Ausgabesprachen ab.


Die folgende Übersicht zeigt die Abdeckung für typische Kern- und Wunschsprachen in europäischen Contact-Center-Szenarien:

| **Sprache** | **Audio-Input (Verstehen)** | **Audio-Output (S2S-Sprachausgabe)** | **Status für bidirektionale Telefonie** |
| --- | --- | --- | --- |
| **Deutsch** | Ja | Ja | Voll unterstützt |
| **Englisch** | Ja | Ja | Voll unterstützt |
| **Französisch** | Ja | Ja | Voll unterstützt |
| **Spanisch** | Ja | Ja | Voll unterstützt |
| **Italienisch** | Ja | Ja | Voll unterstützt |
| **Polnisch** | Ja | Ja | Voll unterstützt |
| **Türkisch** | Ja | Ja | Voll unterstützt |
| **Russisch** | Ja | Ja | Voll unterstützt |
| **Estnisch** | Ja | Nein | nicht in der Liste der 29 Audio-Ausgabesprachen aufgeführt |
| **Bengalisch** | Ja | Nein | nicht in der Liste der 29 Audio-Ausgabesprachen aufgeführt |
| **Vietnamesisch** | Ja | Ja | Voll unterstützt |
| **Arabisch** | Ja | Ja | Voll unterstützt |
| **Bulgarisch** | Ja | Nein | nicht in der Liste der 29 Audio-Ausgabesprachen aufgeführt |

**Quellen:**

- <https://qwen.ai/blog?id=qwen3.5-livetranslate> (21.10.26)
- <https://onegen.ai/a-proven-qwen-livetranslate-in-risky-race-for-data/> (21.10.26)


## 3.5. Telefonie-/Audio-Kompatibilität

- SIP-Audio ist der Ton, der über eine Telefonanlage oder einen Telefonieanbieter übertragen wird. 
- Ein **PoC** („Proof of Concept“) ist ein kleiner Praxistest. Dabei sollte geprüft werden, ob LiveTranslate die Audioqualität unserer Telefonate zuverlässig verarbeitet. Für Qwen3.5 nennt Alibaba sowohl 8 kHz als auch 16 kHz als unterstützte Sampleraten

**Quellen:**

- <https://help.aliyun.com/en/model-studio/live-translator-client-events> (21.10.26)


## 3.6. Technische Evaluierbarkeit

Die geprüften Quellen dokumentieren den Zugriff über die Cloud-API. Öffentlich verfügbare Gewichte für diese LiveTranslate-Varianten habe ich nicht gefunden; Self-hosting ist daher nicht belegt.

- **Cloud-API**: Zugriff über die Alibaba Cloud Model Studio Realtime API; unterstützt werden WebSocket, WebRTC und AOQ. Die Nutzung wird nach Ein- und Ausgabetokens abgerechnet.

**Quellen:**

- <https://help.aliyun.com/en/model-studio/qwen3-5-livetranslate-flash-realtime> (21.10.26)
- <https://onegen.ai/a-proven-qwen-livetranslate-in-risky-race-for-data/>  (21.10.26)


## 3.7. Produktreife / Entwicklungsstatus

Qwen LiveTranslate wird kostenpflichtig über eine Echtzeit-API angeboten. In der separaten API-Dokumentation führt Alibaba die empfohlenen Versionen als „Stable“. Auf der Modell-Informationsseite selbst ist dieser Status nicht angegeben.

**Quellen:**

- <https://help.aliyun.com/en/model-studio/qwen3-5-livetranslate-flash-realtime> (21.10.26)
- <https://help.aliyun.com/en/model-studio/qwen3-8-livetranslate-flash-realtime> (21.10.26)


## 3.8. Verarbeitungsort / Datenregion

Die geprüften Modellinformationen führen Beijing und Singapore als Regionen für LiveTranslate auf. Eine Bereitstellung in Frankfurt ist dort nicht ausgewiesen. Vor einer Nutzung müssen die konkrete Datenverarbeitung und mögliche Drittlandtransfers anhand der für den gewählten Dienst geltenden Verträge geprüft werden.

**Quellen:**

- <https://help.aliyun.com/en/model-studio/qwen3-8-livetranslate-flash-realtime> (21.10.26)


## 3.9. Lizenz und kommerzielle Nutzbarkeit

- **Bereitstellung**: Qwen3.5- und Qwen3.8-LiveTranslate-Realtime sind in den offiziellen Quellen als API-Modelle dokumentiert.
- **Gewichte und Self-Hosting**: In den geprüften offiziellen Quellen habe ich keine öffentlichen Gewichte oder Self-hosting-Lizenz für diese Varianten gefunden.
- **Kommerzielle Nutzung**: Noch ungeklärt. Die Quellen zeigen, dass die Modelle als APIs angeboten werden, enthalten aber keine eindeutige LiveTranslate-spezifische Freigabe für die kommerzielle Produktnutzung. 
- **Evaluation und Benchmarks**: Qwen veröffentlicht eigene Benchmark-Ergebnisse. Ob Dritte die Modelle evaluieren und eigene Ergebnisse veröffentlichen dürfen, ist in den geprüften Quellen nicht ausdrücklich geregelt. 
- **Testzugänge**: Bestimmte Testfunktionen im Model Studio sind laut Sonderbedingungen auf die reine Modellbewertung beschränkt. Die dabei erzeugten Inhalte dürfen nicht kommerziell genutzt werden. Diese Regel lässt sich jedoch nicht automatisch auf die reguläre API-Nutzung übertragen. 
- **Noch zu klären (falls die Modelle eingesetzt werden sollen)**: Geltende Vertragsbedingungen, die Erlaubnis zur kommerziellen Nutzung und die Regeln zur Veröffentlichung eigener Benchmarks. Der konkrete Zugangsweg und die dafür geltenden kontospezifischen Bedingungen sind noch nicht festgelegt.

**Quellen:**

- <https://docs.qwencloud.com/developer-guides/speech/realtime-translation> (21.10.26)
- <https://docs.qwencloud.com/developer-guides/getting-started/model-prices> (21.10.26)
- <https://help.aliyun.com/en/model-studio/bailian-service-notes> (21.10.26)
- <https://help.aliyun.com/en/model-studio/related-agreements> 
- <https://www.qwencloud.com/legal/agreement> (21.10.26)


## 3.10. Terminologie- und Kontextanpassung

- **Glossare und Fachbegriffe**: Unterstützt. Begriffe wie Namen oder Produktbezeichnungen können mit einer gewünschten Übersetzung hinterlegt werden; Qwen empfiehlt bis zu 1.000 Einträge.
- **Kontext**: Visuelle Hinweise aus Bildern oder Videoframes können laut Doku helfen, mehrdeutige Begriffe und Eigennamen besser zu verstehen. Eine frei formulierbare Kontextbeschreibung, etwa mit Vertrags- oder Produktwissen, ist für LiveTranslate nicht ausdrücklich dokumentiert.
- **Grenzen**: Die Funktion soll die Übersetzung verbessern, garantiert aber keine exakte Übernahme. Für Vertragsnummern oder andere kritische Angaben sollte die Genauigkeit separat geprüft werden.

**Quellen:**

- <https://docs.qwencloud.com/developer-guides/speech/realtime-translation> (21.10.26)
- <https://docs.qwencloud.com/api-reference/speech-translation/livetranslate-realtime/client-events> (21.10.26)
- <https://qwen.ai/blog?id=qwen3.5-livetranslate> (21.10.26)


## 3.11. Preismodell

Qwen LiveTranslate wird **nutzungsbasiert **über die Cloud-API abgerechnet. Berechnet werden Audio- und Bild-Eingaben sowie Text- und Audio-Ausgaben, jeweils pro Million Tokens. Die Preise unterscheiden sich je nach Region; in Beijing ist laut Preisliste ein zeitlich begrenztes Freikontingent aufgeführt, in Singapore nicht. Preise und Freikontingente vor einer Evaluierung im Alibaba-Cloud-Konto prüfen.

**Quellen:**

<https://help.aliyun.com/en/model-studio/model-pricing>  (21.10.26)

<https://help.aliyun.com/en/model-studio/qwen3-8-livetranslate-flash-realtime> (21.10.26)

---

# 4. Management Summary (Fazit)

**Technisches Potenzial:** 

Qwen LiveTranslate ist für synchrone Sprachübersetzung ausgelegt und unterstützt gestreamte Text- und Sprachausgabe. Qwen nennt für Qwen3.8 eine LAAL von 2,3 Sekunden. Hotwords können Fachbegriffe und Namen gezielt berücksichtigen. Diese Anbieterangaben sollten in einem Test mit typischen Telefonaten überprüft werden. 


**Architektur und Bereitstellung: **

Die geprüften Quellen dokumentieren LiveTranslate als Cloud-Dienst über die Realtime API. Öffentlich verfügbare Modellgewichte habe ich nicht gefunden; Self-hosting ist daher nicht belegt. 


**Prüfbedarf: **

Die LiveTranslate-Modellinformationen führen Beijing und Singapore als Regionen auf; Frankfurt ist für diese Modelle nicht ausgewiesen. Vor einem Einsatz sind Datenverarbeitung und mögliche Drittlandtransfers sowie die Vertragsbedingungen für die kommerzielle Nutzung zu klären. 


**Empfehlung: **

Qwen LiveTranslate kommt für einen ersten Test mit typischen Telefongesprächen infrage. Dabei sollten Übersetzungsqualität, Verzögerung und Audioqualität bewertet werden. Datenschutz und Vertragsbedingungen sind vor einer späteren Nutzung im Produktivbetrieb zu prüfen.
