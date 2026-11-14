# Meta SeamlessStreaming

**Recherche-Stand:** 20.10.2026  
**Bezugsform:** Self-hosted / Open-Weight-Modell  
**Architektur:** Direktes Speech-to-Speech / Speech-to-Text, simultane Streaming-Übersetzung

**Relevante Lösung:**

- Meta SeamlessStreaming

**Produkt-/Projektseite:**  
Meta Seamless Communication / SeamlessStreaming

**Modell:**  
SeamlessStreaming, 2,5 Milliarden Parameter

## Kurzüberblick

**SeamlessStreaming** ist ein von Meta entwickeltes multilingualer Streaming-Übersetzungsmodell für die simultane Verarbeitung von Sprache. Das Modell ist speziell darauf ausgelegt, Übersetzungen bereits während des laufenden Sprechens zu erzeugen, anstatt zunächst die vollständige Äußerung abzuwarten.

SeamlessStreaming unterstützt:

- Streaming Automatic Speech Recognition für 96 Sprachen
- simultane Übersetzung von Sprache für 101 Eingangssprachen
- simultane Übersetzung zu Text für 96 Zielsprachen
- simultane Speech-to-Speech-Übersetzung für 36 Zielsprachen

Das Modell besitzt laut offizieller Dokumentation **2,5 Milliarden Parameter**.

Im Gegensatz zu einer klassischen Pipeline wie

**Audio → STT → Übersetzung → TTS**

ist SeamlessStreaming als integriertes Modell für simultane Sprachübersetzung konzipiert. Meta verwendet dafür unter anderem **Efficient Monotonic Multihead Attention (EMMA)**, damit das Modell Übersetzungen erzeugen kann, ohne auf das Ende der vollständigen Eingabe warten zu müssen.

Das Modell kann lokal betrieben werden. Meta stellt dafür Modell-Checkpoints und den zugehörigen Code zur Verfügung. Die Modellgewichte von SeamlessStreaming stehen jedoch **nicht unter MIT**, sondern unter **CC-BY-NC 4.0**. Damit ist insbesondere die kommerzielle Nutzung eingeschränkt.

---

# Kriteriencheck Meta SeamlessStreaming

| Kriterium | SeamlessStreaming |
| --- | --- |
| **Live-Eignung** | Ja, speziell für simultane Streaming-Übersetzung entwickelt |
| **Architektur** | Direktes S2ST/S2TT; simultane Verarbeitung |
| **Streaming** | Ja |
| **Sprachen** | 101 Quellsprachen für Speech Input; 36 Zielsprachen für Speech Output |
| **Bidirektionalität** | Grundsätzlich für unterstützte Sprachpaare möglich; konkrete Richtung muss anhand der Sprachlisten geprüft werden |
| **Telefonie** | Keine direkte Telefonie-/G.711-Schnittstelle dokumentiert |
| **Schnittstellen** | Lokale Python-/CLI-Integration; kein vergleichbarer Cloud-API-Endpunkt |
| **WebSocket** | Kein offizieller WebSocket-Service als Bestandteil des Modells dokumentiert |
| **API/Testbarkeit** | Lokaler Betrieb, Python, Hugging Face und bereitgestellte Demo |
| **Verarbeitung** | Lokal / Self-hosted möglich |
| **EU-Verarbeitung** | Abhängig vom eigenen Serverstandort |
| **Self-Hosting** | Ja |
| **Hardware** | GPU empfohlen; Meta nennt für die Streaming-Demo eine NVIDIA T4 mit 16 GB VRAM |
| **Modellgröße** | 2,5 Mrd. Parameter |
| **Preis** | Keine API-Nutzungskosten; eigene Infrastrukturkosten |
| **Lizenz** | CC-BY-NC 4.0 für SeamlessStreaming-Modelle |
| **Kommerzielle Nutzung** | Nein bzw. durch die CC-BY-NC-Lizenz eingeschränkt |
| **Besonderheit** | Direkte simultane Speech-to-Speech-Übersetzung mit niedriger Latenz |

---

# 1. Eignung für Live Translation

SeamlessStreaming wurde speziell für **simultane Sprachübersetzung** entwickelt.

Im Gegensatz zu einer klassischen Pipeline wartet das System nicht darauf, dass ein Sprecher einen vollständigen Satz beendet. Das Modell kann bereits während der laufenden Sprache Übersetzungen erzeugen.

Meta beschreibt SeamlessStreaming als Modell für **low-latency target translations without waiting for complete source utterances**. Dafür wird die Architektur mit der **Efficient Monotonic Multihead Attention (EMMA)** verwendet.

Das ist für Live-Gespräche besonders relevant.

Der vereinfachte Ablauf ist:

**Mikrofon → SeamlessStreaming → übersetztes Audio**

beziehungsweise bei Textausgabe:

**Mikrofon → SeamlessStreaming → übersetzter Text**

Es sind damit nicht zwingend drei getrennte Dienste für STT, Übersetzung und TTS erforderlich.

Meta beschreibt die Lösung als Streaming-System mit einer Latenz von ungefähr **zwei Sekunden**.

Die tatsächliche Latenz hängt allerdings von Hardware, Sprache, Audioübertragung und konkreter Konfiguration ab. Die von Meta angegebene Größenordnung sollte deshalb nicht als garantierte Ende-zu-Ende-Latenz für jede Umgebung verstanden werden.

**Bewertung:** Sehr hohe Eignung für Live Translation.

---

# 2. Architektur

SeamlessStreaming unterscheidet sich deutlich von einer klassischen kaskadierten Architektur.

Eine klassische Pipeline wäre:

**Audio → Speech-to-Text → Machine Translation → Text-to-Speech → Audio**

Bei SeamlessStreaming werden die benötigten Verarbeitungsschritte dagegen in einem zusammenhängenden multilingualen Modell beziehungsweise einer integrierten Modellarchitektur realisiert.

SeamlessStreaming basiert auf **SeamlessM4T v2** und verwendet eine für simultane Übersetzung angepasste Streaming-Architektur. Meta nennt dabei insbesondere die **Efficient Monotonic Multihead Attention (EMMA)**.

Das Modell ist deshalb als **direktes bzw. integriertes Speech-to-Speech-System** einzuordnen.

### Vereinfachte Darstellung

**Kaskadierte Lösung:**

```
Audio
  ↓
STT
  ↓
Text
  ↓
Übersetzung
  ↓
Text
  ↓
TTS
  ↓
Audio
```

**SeamlessStreaming:**

```
Audio
  ↓
SeamlessStreaming
  ↓
übersetztes Audio
```

Für euren Architekturvergleich ist dieser Unterschied besonders relevant.

---

# 3. Realtime-/Streaming-Fähigkeit

Streaming ist eine der wichtigsten Eigenschaften von SeamlessStreaming.

Das Modell wurde ausdrücklich für **simultane Übersetzung** entwickelt. Es kann eingehende Sprache fortlaufend verarbeiten und Übersetzungen erzeugen, bevor die vollständige Eingabe vorliegt.

Meta beschreibt SeamlessStreaming als ein System mit ungefähr **zwei Sekunden Latenz** für die Streaming-Übersetzung.

Damit ist SeamlessStreaming deutlich stärker auf Echtzeitkommunikation ausgerichtet als ein gewöhnliches Offline-Speech-to-Speech-Modell.

### Streaming-Schnittstelle

Hier muss zwischen **Modell-Streaming** und **Netzwerk-Streaming-API** unterschieden werden.

Das Modell unterstützt Streaming auf Verarbeitungsebene.

Es wird jedoch von Meta **kein fertiger Cloud-WebSocket-Service** angeboten, der mit beispielsweise Azure Speech oder Amazon Transcribe vergleichbar wäre.

Für einen eigenen Echtzeitdienst müsste daher selbst eine Streaming-Infrastruktur aufgebaut werden, beispielsweise:

```
Mikrofon
   ↓
Audio-Streaming
   ↓
eigener Server
   ↓
SeamlessStreaming
   ↓
Audio-Streaming
   ↓
Client
```

Das ist ein wichtiger Unterschied zu den Cloud-Diensten aus den anderen Analysen.

---

# 4. Sprachunterstützung und Bidirektionalität

SeamlessStreaming besitzt eine sehr große Sprachabdeckung.

Die offizielle Streaming-Dokumentation nennt:

- **101 Quellsprachen für Spracheingabe**
- **96 Zielsprachen für Textausgabe**
- **36 Zielsprachen für Sprachausgabe**

Für die vollständige Speech-to-Speech-Anwendung ist deshalb insbesondere die kleinere Zahl von **36 Zielsprachen für Audioausgabe** relevant.

Die von euch betrachteten Sprachen sind in der Sprachliste von SeamlessM4T beziehungsweise SeamlessStreaming weitgehend vertreten.

Für die sieben priorisierten Projektsprachen ergibt sich:

| Sprache | Speech Input | Speech Output | Vollständige S2ST-Pipeline |
| --- | --- | --- | --- |
| **Deutsch** | ✓ | ✓ | ✓ |
| **Englisch** | ✓ | ✓ | ✓ |
| **Bulgarisch** | ✓ | prüfen | prüfen |
| **Polnisch** | ✓ | ✓ | ✓ |
| **Französisch** | ✓ | ✓ | ✓ |
| **Italienisch** | ✓ | ✓ | ✓ |
| **Spanisch** | ✓ | ✓ | ✓ |

Die offizielle SeamlessM4T-Sprachliste weist beispielsweise Deutsch, Englisch, Bulgarisch, Französisch, Italienisch, Polnisch und Spanisch als unterstützte Quellsprachen aus. Für Speech Output ist jedoch die kleinere Menge der tatsächlich unterstützten Zielsprachen entscheidend.

Für eine endgültige Bewertung der Sprachpaare muss deshalb nicht nur geprüft werden, ob eine Sprache grundsätzlich unterstützt wird, sondern ob das konkrete **Quell-Ziel-Paar für Speech-to-Speech** verfügbar ist.

### Bidirektionalität

Eine Übersetzung kann grundsätzlich in beide Richtungen erfolgen, wenn beide Richtungen vom Modell für Speech-to-Speech unterstützt werden.

Beispielsweise:

**Deutsch → Englisch**

und

**Englisch → Deutsch**

sind innerhalb der unterstützten Sprachkombinationen grundsätzlich möglich.

Die Unterstützung von Sprache A und Sprache B allein garantiert jedoch nicht automatisch jedes mögliche S2ST-Sprachpaar. Für die endgültige Projektbewertung sollten deshalb die konkreten Sprachpaare getestet werden.

---

# 5. Telefonie-/Audio-Kompatibilität

SeamlessStreaming ist primär für allgemeine Sprachverarbeitung und nicht speziell für Telefonie entwickelt.

Es gibt daher keine vergleichbare Aussage wie bei Amazon Polly, dass beispielsweise **G.711, μ-law oder A-law** als direkte Telefonieausgabe unterstützt werden.

Für eine Telefonieintegration muss das Audio zunächst in das von SeamlessStreaming beziehungsweise der verwendeten Audio-Schnittstelle erwartete Format gebracht werden.

Damit ergibt sich:

**Telefonieaudio → Audio-Konvertierung → SeamlessStreaming → Audio-Konvertierung → Telefonie**

Die konkrete Integration hängt deshalb von der verwendeten Telefonieplattform ab.

### Bewertung

**Direkte Telefonieintegration:** nicht dokumentiert

**G.711 / μ-law / A-law als direkte SeamlessStreaming-Schnittstelle:** nicht nachgewiesen

**Eigene Konvertierung:** wahrscheinlich erforderlich, abhängig von der Audioquelle und dem verwendeten Setup.

Für euer Projekt ist dies ein wichtiger Nachteil gegenüber Lösungen, die bereits auf Cloud-Telefonie oder standardisierte Telefonie-Audioformate ausgerichtet sind.

---

# 6. Technische Evaluierbarkeit

Ein großer Vorteil von SeamlessStreaming ist, dass Meta die Modelle und den Code öffentlich bereitstellt.

Das Repository enthält unter anderem:

- SeamlessStreaming
- SeamlessM4T
- Evaluationswerkzeuge
- CLI-Werkzeuge
- Demo-Anwendungen

Meta stellt außerdem eine **SeamlessStreaming-Demo auf Hugging Face** bereit. Die Demo kann auch lokal ausgeführt beziehungsweise dupliziert werden.

Für einen eigenen Test kann SeamlessStreaming daher lokal installiert und ausgeführt werden.

Der offizielle Code verwendet unter anderem:

- Python
- PyTorch
- fairseq2
- torchaudio
- simuleval

und weitere Abhängigkeiten.

Damit kann ein eigener Prototyp aufgebaut werden:

```
Mikrofon
   ↓
Audio-Stream
   ↓
SeamlessStreaming
   ↓
übersetztes Audio
   ↓
Lautsprecher
```

Für die Evaluation können beispielsweise folgende Werte gemessen werden:

- Ende-zu-Ende-Latenz
- Average Lagging
- Übersetzungsqualität
- Speech-to-Speech-Qualität
- Wortfehlerrate
- Audioqualität
- GPU-Auslastung
- VRAM-Verbrauch
- CPU-Auslastung
- Durchsatz

Meta stellt für die Streaming-Modelle eigene Evaluationsdaten und Metriken bereit.

---

# 7. Hardware und Self-Hosting

Self-Hosting ist eine der wichtigsten Eigenschaften von SeamlessStreaming.

Die Modelle können heruntergeladen und auf eigener Hardware ausgeführt werden.

Das SeamlessStreaming-Modell besitzt **2,5 Milliarden Parameter**.

Für die offizielle Streaming-Demo nennt Meta beziehungsweise das Seamless-Team eine **NVIDIA T4 mit 16 GB VRAM** als empfohlene Hardware.

Damit ist ein Betrieb auf einer einzelnen GPU grundsätzlich möglich.

Allerdings bedeutet Self-Hosting nicht, dass keine Infrastruktur benötigt wird.

Für einen produktiven Einsatz müssen unter anderem berücksichtigt werden:

- GPU
- VRAM
- CPU
- RAM
- Audio-Ein-/Ausgabe
- Streaming-Server
- Netzwerk
- parallele Nutzer
- Modell-Ladezeit
- Monitoring
- Skalierung

Für mehrere gleichzeitig aktive Gespräche kann daher zusätzliche GPU-Hardware notwendig werden.

### Bewertung

**Self-Hosting:** Ja

**GPU erforderlich/empfohlen:** Ja

**NVIDIA T4 16 GB:** als Referenz für die offizielle Streaming-Demo dokumentiert.

**Produktionsbetrieb mit vielen Nutzern:** zusätzliche Infrastruktur und Skalierung erforderlich.

---

# 8. Verarbeitungsort / Datenregion

Bei SeamlessStreaming gibt es keinen zentralen Cloud-Dienst von Meta, über den die Audiodaten zwingend verarbeitet werden müssen.

Das Modell kann **lokal auf eigener Hardware** betrieben werden.

Damit kann der Betreiber selbst bestimmen, wo die Daten verarbeitet werden.

Beispielsweise kann die komplette Verarbeitung auf einem Server innerhalb Deutschlands erfolgen:

```
Mikrofon
   ↓
Server in Deutschland
   ↓
SeamlessStreaming
   ↓
übersetztes Audio
```

Damit besteht gegenüber Cloud-APIs eine wesentlich größere Kontrolle über den Verarbeitungsort.

Allerdings liegt dadurch auch die Verantwortung für:

- Datenschutz
- Zugriffskontrolle
- Verschlüsselung
- Protokollierung
- Speicherung
- Löschung
- Serversicherheit

beim Betreiber.

### Bewertung

**EU-Verarbeitung:** Ja, wenn eigene Hardware innerhalb der EU verwendet wird.

**Deutschland:** Ja, bei eigener Infrastruktur in Deutschland.

**Cloud-Zwang:** Nein.

**Self-hosted:** Ja.

---

# 9. Lizenz und kommerzielle Nutzbarkeit

- **Modelllizenz:** CC-BY-NC 4.0 (Namensnennung, nicht kommerziell).
- **Modellgewichte herunterladen:** Ja, öffentlich verfügbar.
- **Self-Hosting:** Ja, für lizenzkonforme Zwecke.
- **Private und nicht kommerzielle Forschung:** Grundsätzlich erlaubt, sofern die Lizenzbedingungen eingehalten werden.
- **Kommerzielle Nutzung:** Durch die NC-Klausel nicht erlaubt, soweit die Nutzung kommerziell ist.
- **Modell verändern und anpassen:** Für nicht kommerzielle Zwecke grundsätzlich erlaubt, unter Einhaltung der Lizenzbedingungen.
- **Weitergabe des Modells oder bearbeiteter Versionen:** Möglich, wenn die Lizenzbedingungen eingehalten werden, insbesondere Namensnennung und Lizenzhinweis.
- **Namensnennung:** Bei Weitergabe müssen Urheber- und Lizenzhinweise erhalten bleiben.
- **MIT-Lizenz:** Gilt für Teile des Codes, aber nicht für die SeamlessStreaming-Modellgewichte.
- **Kommerzieller Produktivbetrieb:** Ohne eine zusätzliche Genehmigung oder Lizenz, die dies erlaubt, nicht geeignet.

**Quelle:** Creative Commons, *Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)*: <https://creativecommons.org/licenses/by-nc/4.0/>   
**Modellquelle:** Meta, *Seamless Communication Repository*: [https://github.com/facebookresearch/seamless\_communication](https://github.com/facebookresearch/seamless_communication) 

---

# 10. Preise

## 10. Preise

SeamlessStreaming hat keinen offiziellen Preis pro Audiominute, da Meta das Modell nicht als kostenpflichtige zentrale Cloud-API anbietet. Die Modellgewichte sind öffentlich verfügbar; die laufenden Kosten entstehen beim eigenen Betrieb durch GPU-Rechenleistung, Server, Strom und Wartung. Die Gewichte stehen unter der Lizenz **CC-BY-NC 4.0**, die kommerzielle Nutzung einschränkt. Daher bedeutet die kostenlose Verfügbarkeit nicht, dass das Modell ohne Weiteres kostenlos in einem kommerziellen Produkt eingesetzt werden darf.

**Konkrete Kostenpunkte:**

- **Modellgewichte:** öffentlich verfügbar, kein ausgewiesener Preis pro API-Aufruf.
- **GPU- und Serverkosten:** abhängig vom gewählten Hardwareanbieter, der GPU, der Laufzeit und der Auslastung.
- **Betriebskosten:** zusätzliche Kosten für Strom, Speicher, Netzwerk, Wartung und Monitoring.
- **Kosten pro Audiominute:** erst berechenbar, wenn die GPU-Kosten pro Stunde und die tatsächlich verarbeitbaren Audiominuten pro Stunde gemessen wurden.
- **Lizenz:** keine pauschale kommerzielle Nutzungserlaubnis durch die kostenlose Bereitstellung; für einen kommerziellen Einsatz muss die Lizenzfrage vorab geklärt werden.

**Beispielrechnung (nur zur Veranschaulichung):** Kostet der GPU-Server 1,00 € pro Stunde und verarbeitet er in dieser Stunde 60 Audiominuten, ergeben sich reine GPU-Kosten von rund **0,0167 € pro Audiominute**. Bei 120 verarbeiteten Audiominuten pro Stunde wären es rund **0,0083 € pro Audiominute**. Das sind Rechenbeispiele, keine gemessenen Leistungswerte von SeamlessStreaming; weitere Betriebskosten sind nicht enthalten.

**Bewertung:** SeamlessStreaming kann bei hoher Auslastung wirtschaftlich interessant sein, weil keine Gebühr pro API-Aufruf anfällt. Dafür müssen Infrastruktur und Betrieb selbst organisiert werden. Ein belastbarer Kostenvergleich mit Cloud-Diensten setzt einen Test auf vergleichbarer Hardware und mit identischem Audiomaterial voraus.

---

# 11. Produktreife / Entwicklungsstatus

SeamlessStreaming wurde von Meta als Forschungs- und Open-Source-Modell veröffentlicht.

Die zugrunde liegende Arbeit **„Seamless: Multilingual Expressive and Streaming Speech Translation“** wurde 2023 veröffentlicht.

Das Modell ist öffentlich verfügbar und kann praktisch getestet werden.

Im Vergleich zu Azure Speech oder AWS handelt es sich jedoch nicht um einen vollständig verwalteten Cloud-Produktdienst.

Das bedeutet:

**Modell verfügbar:** Ja

**Lokaler Betrieb:** Ja

**Fertiger Managed-Service:** Nein

**Eigene Integration notwendig:** Ja

Für einen Forschungs- oder Prototyping-Vergleich ist das Modell deshalb besonders interessant. Für einen produktiven kommerziellen Einsatz müssen dagegen Lizenz, Hardware, Skalierung und Betrieb genauer geprüft werden.

---

# 12. Besonderheiten der Architektur

Eine besondere Eigenschaft von SeamlessStreaming ist die Kombination aus **multilingualer Übersetzung und simultaner Verarbeitung**.

Meta verwendet die **Efficient Monotonic Multihead Attention (EMMA)**, um die Entscheidung zu steuern, wann genügend Eingabe vorhanden ist, um bereits eine Übersetzung zu erzeugen.

Das Modell wartet dadurch nicht zwingend auf den vollständigen Satz.

Dies ermöglicht den Kompromiss zwischen:

**mehr Kontext → bessere Übersetzung**

und

**weniger Wartezeit → niedrigere Latenz**

Die Architektur ist damit speziell für simultane Kommunikation interessant.

---

# Hauptstärken Meta SeamlessStreaming

- **Direkte Speech-to-Speech-Übersetzung**
- Speziell für **simultane Streaming-Übersetzung** entwickelt
- Keine klassische STT → MT → TTS-Kaskade notwendig
- **Sehr große Sprachabdeckung**
- Unterstützt 101 Quellsprachen für Speech Input
- Unterstützt 36 Zielsprachen für Speech Output
- **Niedrige Latenz**, von Meta mit ungefähr zwei Sekunden beschrieben
- **Self-hosting möglich**
- Verarbeitung kann vollständig auf eigener Infrastruktur erfolgen
- Keine zwingende Cloud-API
- Öffentliche Modellgewichte und öffentlich zugänglicher Code
- Gute Möglichkeit für eigene technische Experimente und Benchmarks
- Offizielle Demo und Evaluationswerkzeuge vorhanden

# Haupteinschränkungen

- **Hoher Hardwarebedarf** im Vergleich zu kleinen klassischen Speech-Diensten
- GPU für einen sinnvollen Streaming-Betrieb erforderlich beziehungsweise empfohlen
- Eigene Streaming-Infrastruktur muss aufgebaut werden
- Kein fertiger Managed-Cloud-Service wie Azure Speech oder AWS
- **Keine direkte WebSocket-Cloud-API** wie bei klassischen Speech-APIs dokumentiert
- Direkte Telefonieintegration nicht dokumentiert
- G.711/μ-law/A-law nicht als direkte SeamlessStreaming-Schnittstelle nachgewiesen
- Speech-to-Speech-Ausgabe auf **36 Zielsprachen** beschränkt
- Konkrete Sprachpaare müssen separat geprüft werden
- **CC-BY-NC-4.0-Lizenz schränkt kommerzielle Nutzung ein**
- Datenschutz und Sicherheit der Infrastruktur müssen selbst umgesetzt werden
- Skalierung auf viele parallele Gespräche erfordert zusätzliche GPU-Infrastruktur

## Gesamtbewertung

SeamlessStreaming ist besonders interessant, weil es einen **echten Gegenpol zu den Cloud-Kaskaden von AWS und Azure** darstellt.

Während AWS Transcribe + Translate + Polly beispielsweise

**Audio → STT → Übersetzung → TTS**

verwendet, ist SeamlessStreaming auf **simultane, integrierte Speech-to-Speech-Übersetzung** ausgelegt.

Damit bietet SeamlessStreaming insbesondere Vorteile bei:

- niedriger Latenz,
- Self-Hosting,
- Kontrolle über den Verarbeitungsort,
- direkter Speech-to-Speech-Verarbeitung.

Dem stehen insbesondere gegenüber:

- höherer Infrastrukturaufwand,
- GPU-Bedarf,
- fehlende fertige Telefonieintegration,
- eigener Betriebsaufwand,
- und vor allem die **CC-BY-NC-4.0-Lizenz**, die eine kommerzielle Nutzung der Modellgewichte einschränkt.

Für einen **technischen Forschungs- und Vergleichstest** ist SeamlessStreaming daher sehr interessant. Für einen **kommerziellen Produktivbetrieb** müssen insbesondere Lizenz und Self-Hosting-Aufwand vor einer Entscheidung geprüft werden.

## Quellen

- Meta / Facebook Research – Seamless Communication: [https://github.com/facebookresearch/seamless\_communication](https://github.com/facebookresearch/seamless_communication) 
- Meta – SeamlessStreaming Streaming Documentation: [https://github.com/facebookresearch/seamless\_communication/blob/main/docs/streaming/README.md](https://github.com/facebookresearch/seamless_communication/blob/main/docs/streaming/README.md) 
- Meta – SeamlessM4T Documentation: [https://github.com/facebookresearch/seamless\_communication/blob/main/docs/m4t/README.md](https://github.com/facebookresearch/seamless_communication/blob/main/docs/m4t/README.md) 
- Meta AI Research – Seamless: Multilingual Expressive and Streaming Speech Translation: <https://ai.meta.com/research/publications/seamless-multilingual-expressive-and-streaming-speech-translation/> 
- Hugging Face – Meta SeamlessStreaming: <https://huggingface.co/facebook/seamless-streaming> 
- Hugging Face – Meta SeamlessM4T v2: <https://huggingface.co/facebook/seamless-m4t-v2-large> 
- arXiv – Seamless: Multilingual Expressive and Streaming Speech Translation: <https://arxiv.org/abs/2312.05187>
