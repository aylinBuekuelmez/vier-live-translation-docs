# AWS Transcribe \+ Translate \+ Polly

**Recherche-Stand:** 20.10.2026  
**Bezugsform:** Cloudbasierte API-Dienste

**Relevante Lösungen:**

- Amazon Transcribe
- Amazon Translate
- Amazon Polly

**Produktdokumentation:**

- AWS – Amazon Transcribe
- AWS – Amazon Translate
- AWS – Amazon Polly

## Kurzüberblick

Amazon Web Services (AWS) bietet mit **Amazon Transcribe**, **Amazon Translate** und **Amazon Polly** drei getrennte Cloud-Dienste, die zu einer vollständigen Sprachübersetzungspipeline kombiniert werden können.

Die Verarbeitung erfolgt nach folgendem Schema:

**Audio → Speech-to-Text → Übersetzung → Text-to-Speech → übersetztes Audio**

Dabei übernimmt:

1. **Amazon Transcribe** die Umwandlung des gesprochenen Audios in Text.
2. **Amazon Translate** die Übersetzung des erkannten Textes.
3. **Amazon Polly** die Umwandlung des übersetzten Textes in gesprochene Sprache.

Die Architektur ist damit **kaskadiert**. Im Gegensatz zu einem direkten Speech-to-Speech-Modell werden die einzelnen Verarbeitungsschritte über getrennte Dienste realisiert.

Amazon Transcribe unterstützt Echtzeit-Streaming über HTTP/2 und WebSockets. Amazon Translate bietet Echtzeitübersetzung von Text. Amazon Polly kann daraus anschließend wieder Audio erzeugen und unterstützt verschiedene Audioformate, darunter PCM, μ-law und A-law.

## Kriteriencheck AWS

| Kriterium | Amazon Transcribe + Translate + Polly |
| --- | --- |
| **Live-Eignung** | Ja, durch Streaming von Amazon Transcribe und Echtzeitverarbeitung der nachfolgenden Schritte |
| **Architektur** | Kaskadiert: STT → Übersetzung → TTS |
| **Streaming** | Ja, Amazon Transcribe unterstützt Streaming; Polly unterstützt Echtzeit-Synthese |
| **Sprachen** | Große Sprachabdeckung; für die vollständige Pipeline muss die gemeinsame Sprachschnittmenge betrachtet werden |
| **Bidirektionalität** | Grundsätzlich möglich, sofern Quell-/Zielsprache von allen drei Komponenten unterstützt wird |
| **Telefonie** | Amazon Polly unterstützt μ-law und A-law; konkrete Telefonieintegration muss separat umgesetzt bzw. geprüft werden |
| **Schnittstellen** | AWS APIs und SDKs; Amazon Transcribe unterstützt HTTP/2 und WebSockets |
| **API/Testbarkeit** | Sehr gut dokumentierte APIs, SDKs und Beispiele |
| **Verarbeitungsort** | AWS Cloud; unter anderem Region Frankfurt (`eu-central-1`) |
| **EU-Verarbeitung** | Ja, mehrere europäische AWS-Regionen |
| **Self-Hosting** | Keine vollständige lokale Transcribe-/Translate-/Polly-Pipeline als AWS-Produkt |
| **Preis** | Pay-as-you-go; Transcribe nach Audiomenge, Translate und Polly nach Zeichen |
| **Lizenz** | Kommerzielle AWS-Dienste unter den AWS-Servicebedingungen |

---

## 1. Eignung für Live Translation

Die Kombination aus Amazon Transcribe, Amazon Translate und Amazon Polly kann für **Echtzeit-Sprachübersetzung** eingesetzt werden.

Der wichtigste Bestandteil für die kontinuierliche Audioverarbeitung ist Amazon Transcribe. Der Dienst kann Audiodaten als Streaming-Eingabe verarbeiten und während des Streams Transkriptionsergebnisse zurückgeben.

Amazon Transcribe unterstützt für Streaming sowohl **HTTP/2 als auch WebSockets**. Eine Streaming-Transkription ist damit für kontinuierlich eintreffendes Audio möglich.

Der Ablauf kann beispielsweise folgendermaßen aussehen:

**Mikrofon → Amazon Transcribe → Amazon Translate → Amazon Polly → Audio**

Beispiel:

**„Hello, how are you?“**  
↓  
Amazon Transcribe  
↓  
**„Hello, how are you?“**  
↓  
Amazon Translate  
↓  
**„Hallo, wie geht es dir?“**  
↓  
Amazon Polly  
↓  
**gesprochenes deutsches Audio**

Damit lässt sich eine Live-Übersetzung mit gesprochener Ausgabe realisieren.

Da die Lösung aus drei nacheinandergeschalteten Diensten besteht, entsteht allerdings zusätzlicher Verarbeitungsaufwand. Das Audio muss zunächst transkribiert werden, anschließend wird der Text übersetzt und danach wieder in Sprache umgewandelt.

Eine garantierte Ende-zu-Ende-Latenz für die komplette Pipeline wird von AWS in den untersuchten Quellen nicht als feste Millisekundenangabe angegeben.

**Quellen:** Amazon Transcribe Streaming und Amazon Translate Dokumentation.

---

## 2. Architektur

Die Architektur von Amazon Transcribe + Translate + Polly ist eindeutig **kaskadiert**.

### 1. Speech-to-Text

**Amazon Transcribe**

Amazon Transcribe wandelt die eingehende Sprache in Text um.

### 2. Machine Translation

**Amazon Translate**

Amazon Translate übersetzt den von Transcribe erzeugten Text in die gewünschte Zielsprache.

### 3. Text-to-Speech

**Amazon Polly**

Amazon Polly wandelt den übersetzten Text wieder in gesprochene Sprache um.

Damit ergibt sich:

**Audio → STT → Machine Translation → TTS → Audio**

Die drei Komponenten sind eigenständige AWS-Dienste und können unabhängig voneinander eingesetzt werden.

Ein Vorteil dieser Architektur ist die klare Trennung der Verarbeitungsschritte. Die einzelnen Komponenten können separat konfiguriert und getestet werden.

Ein Nachteil gegenüber einem direkten Speech-to-Speech-Modell besteht darin, dass mehrere Verarbeitungsschritte nacheinander ausgeführt werden müssen.

**Quelle:** AWS-Dokumentationen zu Amazon Transcribe, Amazon Translate und Amazon Polly.

---

## 3. Realtime-/Streaming-Fähigkeit

### Amazon Transcribe

Amazon Transcribe unterstützt Streaming-Transkription in Echtzeit.

AWS dokumentiert dafür:

- **HTTP/2**
- **WebSockets**
- AWS SDKs

WebSockets können für Streaming-Transkriptionen verwendet werden. Bei HTTP/2 wird ebenfalls eine Streaming-Verbindung verwendet.

Damit können Audiodaten kontinuierlich an Amazon Transcribe übertragen und Transkriptionsergebnisse während der Verarbeitung zurückgegeben werden.

### Amazon Translate

Amazon Translate unterstützt **Echtzeitübersetzung**. Text kann synchron an die API übergeben und die Übersetzung anschließend zurückgegeben werden. Amazon Translate unterstützt sowohl Standardtextübersetzung als auch weitere Übersetzungsfunktionen.

### Amazon Polly

Amazon Polly unterstützt Echtzeit-Sprachsynthese. Die erzeugte Sprache kann als Audiostream ausgegeben werden.

Amazon Polly unterstützt unter anderem:

- MP3
- OGG
- PCM
- μ-law
- A-law

Die Unterstützung von μ-law und A-law ist insbesondere für Anwendungen mit Telefonie relevant.

### Bewertung

**Streaming:** Ja

**WebSocket:** Ja, für Amazon Transcribe eindeutig dokumentiert.

Wichtig ist jedoch, dass nicht alle drei Dienste über **dieselbe WebSocket-Verbindung** kommunizieren. Die Pipeline besteht aus mehreren getrennten API-Aufrufen:

**Transcribe → Translate → Polly**

---

## 4. Sprachunterstützung und Bidirektionalität

AWS bietet bei den drei Diensten jeweils eigene Sprachlisten.

Für die Bewertung einer vollständigen Sprachübersetzung ist deshalb nicht die Anzahl aller unterstützten Sprachen entscheidend. Relevant ist vielmehr die **gemeinsame Schnittmenge** von Transcribe, Translate und Polly.

Amazon Transcribe unterstützt zahlreiche Sprachen sowohl für Batch- als auch für Streaming-Transkription. Für die hier relevanten Sprachen sind unter anderem Deutsch, Englisch, Bulgarisch, Französisch, Italienisch, Polnisch und Spanisch dokumentiert.

Amazon Translate unterstützt ebenfalls Deutsch, Englisch, Bulgarisch, Französisch, Italienisch, Polnisch und Spanisch.

Bei Amazon Polly ist jedoch zu beachten, dass **Bulgarisch aktuell nicht in der unterstützten Sprachliste aufgeführt ist**. Deutsch, Englisch, Französisch, Italienisch, Polnisch und Spanisch werden dagegen unterstützt.

Für die priorisierten Projektsprachen ergibt sich deshalb:

| Sprache | Amazon Transcribe | Amazon Translate | Amazon Polly | Vollständige Audio-Pipeline |
| --- | --- | --- | --- | --- |
| **Deutsch** | ✓ | ✓ | ✓ | ✓ |
| **Englisch** | ✓ | ✓ | ✓ | ✓ |
| **Bulgarisch** | ✓ | ✓ | ✗ | ✗ |
| **Polnisch** | ✓ | ✓ | ✓ | ✓ |
| **Französisch** | ✓ | ✓ | ✓ | ✓ |
| **Italienisch** | ✓ | ✓ | ✓ | ✓ |
| **Spanisch** | ✓ | ✓ | ✓ | ✓ |

Damit ist **Bulgarisch ein wichtiger Einschränkungsfall**: Transcribe und Translate unterstützen Bulgarisch, Amazon Polly bietet aktuell jedoch keine bulgarische TTS-Stimme. Eine vollständige gesprochene Übersetzungskette für Bulgarisch kann mit dieser Standardkombination daher nicht realisiert werden.

Die übrigen sechs betrachteten Sprachen können grundsätzlich die komplette Pipeline durchlaufen.

**Bidirektionalität:**  
Eine Übersetzung in beide Richtungen ist grundsätzlich möglich, sofern die jeweilige Quell- und Zielsprache von allen drei Diensten unterstützt wird. Die reine Unterstützung einer Sprache bei Amazon Translate bedeutet dabei nicht automatisch, dass auch die vollständige gesprochene Pipeline in beide Richtungen möglich ist.

**Quelle:** AWS Language Support für Transcribe, Translate und Polly.

---

## 5. Telefonie-/Audio-Kompatibilität

Amazon Polly unterstützt verschiedene Audioformate für die Sprachsynthese.

Für Anwendungen mit Telefonie sind insbesondere folgende Formate relevant:

- **μ-law**
- **A-law**
- **PCM**

AWS beschreibt μ-law und A-law ausdrücklich als mögliche Ausgabeformate für Telefonieanwendungen.

Damit kann Amazon Polly beispielsweise Audio erzeugen, das anschließend in einer Telefonieumgebung verwendet werden kann.

Die Unterstützung dieser Audioformate bedeutet allerdings nicht automatisch, dass eine vollständige Telefonieanlage direkt an die gesamte AWS-Pipeline angeschlossen werden kann. Für die konkrete Telefonieintegration müssen Audioformat, Samplingrate, Übertragungsprotokoll und die verwendete Telefonieplattform berücksichtigt werden.

Für die Bewertung sollte deshalb zwischen **Audioformat-Unterstützung** und einer **direkten Telefonieintegration** unterschieden werden.

---

## 6. Technische Evaluierbarkeit

AWS bietet umfangreiche Möglichkeiten, die drei Dienste praktisch zu testen.

Amazon Transcribe kann über APIs, AWS SDKs sowie Streaming über HTTP/2 und WebSockets angesprochen werden.

Dadurch kann beispielsweise ein eigener Prototyp aufgebaut werden:

**Mikrofon**

↓

**Amazon Transcribe**

↓

**erkannter Text**

↓

**Amazon Translate**

↓

**übersetzter Text**

↓

**Amazon Polly**

↓

**Audioausgabe**

Die Pipeline kann dadurch praktisch untersucht werden.

Für eine technische Evaluation können unter anderem folgende Werte gemessen werden:

- Ende-zu-Ende-Latenz
- Transkriptionslatenz
- Übersetzungslatenz
- TTS-Latenz
- Erkennungsqualität
- Übersetzungsqualität
- Audioqualität
- Kosten pro Gesprächsminute

Damit ist die AWS-Lösung gut für einen praktischen Vergleich mit anderen Cloud- und Speech-to-Speech-Lösungen geeignet.

---

## 7. Produktreife / Entwicklungsstatus

Amazon Transcribe, Amazon Translate und Amazon Polly sind etablierte, kommerziell verfügbare AWS-Dienste.

Es handelt sich daher nicht um ein einzelnes Forschungsmodell, sondern um eine Kombination mehrerer produktiver Cloud-Dienste.

Die Architektur kann somit als:

**Produktive Cloud-Service-Pipeline**

eingeordnet werden.

Die einzelnen Dienste werden unabhängig voneinander weiterentwickelt und besitzen jeweils eigene Funktionen, Sprachlisten, Preise und regionale Verfügbarkeiten.

Für den Vergleich ist deshalb zu beachten, dass „AWS Transcribe + Translate + Polly“ kein einzelnes Modell mit einer gemeinsamen Modellversion ist.

---

## 8. Verarbeitungsort / Datenregion

AWS stellt die Dienste in verschiedenen AWS-Regionen bereit.

Für Amazon Transcribe ist beispielsweise die Region **Europe (Frankfurt) – **`eu-central-1` sowohl für Batch- als auch für Streaming-Transkription verfügbar. Weitere europäische Regionen sind unter anderem Irland und London.

Auch Amazon Polly stellt Standardstimmen unter anderem in der Region **Europe (Frankfurt) – **`eu-central-1` bereit. Weitere europäische Regionen umfassen beispielsweise Irland, London, Paris, Spanien und Stockholm.

Damit ist eine Verarbeitung innerhalb der EU grundsätzlich möglich.

Für datenschutzrelevante Anwendungen müssen jedoch die konkreten Datenverarbeitungsbedingungen jedes verwendeten AWS-Dienstes berücksichtigt werden. AWS verwendet hierfür unter anderem das Shared-Responsibility-Modell. Die Konfiguration und der Schutz der eigenen Inhalte liegen dabei teilweise in der Verantwortung des Kunden.

**Self-Hosting:**  
Eine vollständige lokale Bereitstellung von Amazon Transcribe + Translate + Polly als eigene lokale Modellpipeline wird von AWS nicht als gleichwertiges Self-Hosting-Produkt angeboten. Die Lösung ist grundsätzlich als Cloud-Service konzipiert.

---

## 9. Lizenz und kommerzielle Nutzbarkeit

Amazon Transcribe, Amazon Translate und Amazon Polly sind proprietäre AWS-Cloud-Dienste. Es gelten das AWS Customer Agreement und die AWS Service Terms.

- **Open Source:** Nein, die Dienste selbst sind nicht Open Source.
- **Eigene Anwendung:** Ja, Integration über AWS-APIs und SDKs ist möglich.
- **Kommerzielle Nutzung:** Ja, unter den geltenden AWS-Vertragsbedingungen.
- **Nutzung der Ergebnisse:** Generierte Texte und Audiodateien können grundsätzlich für eigene Zwecke verwendet werden; weitere rechtliche und vertragliche Anforderungen sind zu beachten.
- **Self-Hosting:** Nein, die AWS-Dienste können nicht als vollständige Modelle lokal installiert und betrieben werden.
- **Reverse Engineering:** Vertraglich eingeschränkt, soweit gesetzlich zulässig.
- **Weiterverkauf:** Der Weiterverkauf der AWS-Dienste ist vertraglich eingeschränkt.
- **Datenschutz:** Bei der Verarbeitung von Gesprächsdaten müssen Datenschutzvorgaben und die AWS-Regeln zur Datenverarbeitung eingehalten werden. Die Bedingungen zur Nutzung von Inhalten für die Verbesserung von KI-Diensten sollten ebenfalls geprüft werden.
- **Voraussetzung:** Für die reguläre Nutzung ist ein AWS-Konto erforderlich.

**Quellen:**

1. AWS Customer Agreement: <https://aws.amazon.com/agreement/> 
2. AWS Service Terms, insbesondere Abschnitt 50: <https://aws.amazon.com/service-terms/> 
3. AWS Data Privacy: <https://aws.amazon.com/compliance/data-privacy/> 

---

## 10. Preise

Die Kosten der Lösung entstehen durch **drei getrennte AWS-Dienste**.

### Amazon Transcribe

Amazon Transcribe verwendet ein **Pay-as-you-go-Modell**. Die Abrechnung basiert auf der Menge des transkribierten Audiomaterials. AWS rechnet aktuell in präzisen Ein-Sekunden-Schritten ohne Mindestdauer ab. Die konkreten Preise hängen unter anderem von Region und verwendeter Transcribe-Funktion ab.

### Amazon Translate

Amazon Translate wird nach der Anzahl der übersetzten Textzeichen abgerechnet.

Für die Standard-Textübersetzung nennt AWS aktuell:

**15 USD pro 1 Million Zeichen**

Für neue Nutzer ist ein kostenloses Kontingent von bis zu 2 Millionen Zeichen pro Monat für zwölf Monate angegeben.

### Amazon Polly

Amazon Polly wird ebenfalls nach der Anzahl der verarbeiteten Zeichen abgerechnet.

Aktuelle Preise außerhalb des kostenlosen Kontingents:

| Polly-Engine | Preis pro 1 Mio. Zeichen |
| --- | --- |
| Standard | 4 USD |
| Neural | 16 USD |
| Long-Form | 100 USD |
| Generative | 30 USD |

AWS stellt für die verschiedenen Engines außerdem unterschiedliche kostenlose Kontingente bereit.

### Gesamtkosten

Die Kosten einer vollständigen Übersetzung lassen sich daher vereinfacht als Summe der drei Verarbeitungsschritte betrachten:

**Gesamtkosten = Transcribe + Translate + Polly**

Die konkrete Höhe hängt davon ab,

- wie viel Audio verarbeitet wird,
- wie viel Text entsteht,
- welche Sprache übersetzt wird,
- welche Polly-Stimme beziehungsweise Engine verwendet wird,
- und in welcher AWS-Region die Dienste genutzt werden.

Ein wichtiger Nachteil der kaskadierten Architektur besteht damit darin, dass **drei separate Dienste Kosten verursachen**.

---

## Hauptstärken AWS Transcribe + Translate + Polly

- **Klare Architektur:** Audio → STT → Übersetzung → TTS
- **Echtzeitverarbeitung möglich**
- **Streaming-Unterstützung**
- **WebSocket-Unterstützung bei Amazon Transcribe**
- **Große Sprachabdeckung**
- **EU-Regionen verfügbar**, unter anderem Frankfurt
- **Telefoniegeeignete Audioformate** wie μ-law und A-law bei Amazon Polly
- **Gut dokumentierte APIs und SDKs**
- **Sehr gut praktisch testbar**
- Die einzelnen Komponenten können unabhängig voneinander eingesetzt und getestet werden
- Gut geeignet als **Referenz für eine klassische kaskadierte Speech-Translation-Architektur**

## Haupteinschränkungen

- **Kaskadierte Architektur:** mehrere Dienste statt eines direkten Speech-to-Speech-Modells
- **Zusätzliche Latenz** durch die aufeinanderfolgenden Verarbeitungsschritte
- **Drei separate Dienste** müssen konfiguriert und bezahlt werden
- Die gemeinsame Sprachschnittmenge ist kleiner als die Summe der Sprachlisten der einzelnen Dienste
- **Bulgarisch** kann mit der Standardkombination nicht als vollständige Audio-zu-Audio-Pipeline umgesetzt werden, da Amazon Polly keine bulgarische TTS-Sprache anbietet.
- Keine vollständige lokale/self-hosted Pipeline als AWS-Produkt
- Datenschutz und Datenverarbeitung müssen für die einzelnen AWS-Dienste berücksichtigt werden
- Eine direkte Telefonieintegration ist nicht allein durch die vorhandenen Audioformate garantiert
- Eine feste Ende-zu-Ende-Latenz für die komplette Pipeline wird nicht garantiert

## Quellen

- AWS – Amazon Transcribe: <https://docs.aws.amazon.com/transcribe/latest/dg/>
- AWS – Amazon Transcribe Streaming / HTTP / WebSockets: <https://docs.aws.amazon.com/transcribe/latest/dg/getting-started-http-websocket.html> 
- AWS – Unterstützte Transcribe-Sprachen: <https://docs.aws.amazon.com/transcribe/latest/dg/supported-languages.html> 
- AWS – Amazon Translate: <https://docs.aws.amazon.com/translate/latest/dg/>
- AWS – Unterstützte Translate-Sprachen: <https://docs.aws.amazon.com/translate/latest/dg/what-is-languages.html> 
- AWS – Amazon Polly: <https://docs.aws.amazon.com/polly/latest/dg/>
- AWS – Unterstützte Polly-Sprachen: <https://docs.aws.amazon.com/polly/latest/dg/supported-languages.html> 
- AWS – Amazon Polly Audioformate: <https://docs.aws.amazon.com/polly/latest/dg/how-text-to-speech-works.html> 
- AWS – Amazon Transcribe Preise: <https://aws.amazon.com/transcribe/pricing/> 
- AWS – Amazon Translate Preise: <https://aws.amazon.com/translate/pricing/> 
- AWS – Amazon Polly Preise: <https://aws.amazon.com/polly/pricing/>
- AWS – Datenschutz Amazon Transcribe: <https://docs.aws.amazon.com/transcribe/latest/dg/data-protection.html>
