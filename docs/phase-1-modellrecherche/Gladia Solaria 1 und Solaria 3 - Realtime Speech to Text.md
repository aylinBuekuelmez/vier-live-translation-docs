# Gladia Solaria 1 und Solaria 3 \- Realtime Speech to Text

# Gladia Solaria-1 und Solaria-3: Detailblatt

**Kurz:** Solaria-1 ist Gladias breiter Generalist für **Echtzeit-Streaming, 100+ Sprachen und Code-Switching**. Solaria-3 ist das neuere, präzisere Modell für **voraufgezeichnete, realistische europäische Business-/Call-Audio** — aber aktuell **nicht für Live-Streaming**. 

Für uns aktuell relevant: **Solaria-1**

Solaria-3 wäre stark für Nachbearbeitung und Qualitätstests mit aufgezeichneten Anrufen.\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[docs.gladia](https://docs.gladia.io/chapters/pre-recorded-stt/quickstart)\]\[[gladia](https://www.gladia.io/solaria-3)\] → weiter unten Vergleich Solaria 1 und Solaria 3

## Architektur im Detail

ASR-/STT-Modelle mit optionalen Audio-Intelligence-Funktionen → eigene Kette bauen:

```
Telefon-Audio
   ↓
AEC / Noise Suppression / VAD
   ↓
Gladia Solaria-1 Realtime STT
   ↓
Externe MT: DeepL / Google Translate / Azure Translator
   ↓
Externe TTS: Aura-2, ElevenLabs o. Ä.
   ↓
Gegenüber
```

Gladia kann dabei bereits die **Speech-to-Text-Translation** übernehmen: Es übersetzt Äußerungen in über 100 Sprachen und behält dabei Timestamps und Speaker Labels. Das würde die separate MT-Stufe für Textübersetzung ersetzen; für gesprochene Ausgabe bleibt TTS aber notwendig.

```
Gladia Solaria-1
   ↓
Speech-to-Text Translation DE → EN
   ↓
Externe TTS EN
   ↓
Gegenüber
```

## Echtzeit- und Streaming-Verhalten

**Solaria-1** ist für euren Live-Fall der relevante Kandidat. Es unterstützt WebSocket-Streaming, VAD, automatische Spracherkennung und Code-Switching. Gladia gibt als selbst gemessene Werte etwa **103 ms optimale Partial-Latenz**, **270 ms durchschnittliche Partial-Latenz** und **rund 698 ms Final-Latenz** an.\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[gladia](https://www.gladia.io/blog/best-real-time-stt-models-for-meeting-assistants-2026)\]

**Solaria-3** ist dagegen bewusst als **Async-Modell** positioniert. Es verarbeitet aufgezeichnete Audio- oder Videodateien bis 135 Minuten bzw. 1.000 MB und liefert ein vollständiges, strukturiertes Transkript.\[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]

| Eigenschaft | Solaria-1 | Solaria-3 |
| --- | --- | --- |
| Live-Streaming | Ja | Nein |
| Async / Batch | Ja | Ja |
| Partial Results | Ja | Nein |
| Code-Switching | Ja | Nein |
| Typischer Einsatz | Live-Captioning, Live-Übersetzung, Voice Agents | Nachbearbeitung, Call-Analyse, Qualitätstests |
| Verarbeitungsgeschwindigkeit | Echtzeit | Ca. 1 Stunde Audio in ca. 60 Sekunden. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\] |

## Sprachunterstützung

### Solaria-1

Solaria-1 unterstützt **100+ Sprachen**, inklusive Deutsch und Englisch, mit automatischer Spracherkennung und nativem Code-Switching. Es kann in einem Gespräch Sprachwechsel erkennen und transkribieren.\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]

Für euer DE↔EN-Szenario heißt das:

```
„Ich habe dem customer eine email geschickt.“
   ↓
Solaria-1 erkennt DE/EN-Mix
   ↓
Transkript bzw. Übersetzung
```

### Solaria-3

Solaria-3 ist gezielt auf **EN, FR, DE, ES und IT** optimiert. Deutsch und Englisch sind also abgedeckt, aber es unterstützt kein Code-Switching. Sobald eine Sprache erkannt ist, bleibt sie für den Rest der Datei fixiert.\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[gladia](https://www.gladia.io/solaria-3)\]

Für reine deutsche oder reine englische Testaufnahmen ist Solaria-3 daher gut geeignet. Für gemischte DE/EN-Gespräche wäre Solaria-1 die richtige Wahl.\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]

## Genauigkeit und Benchmarks

Gladia veröffentlicht eigene, laut Unternehmen offene und reproduzierbare Benchmarks. Die Zahlen sollten ihr trotzdem mit euren eigenen Telefonaten validieren, aber sie geben eine gute Richtung.\[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[gladia](https://www.gladia.io/competitors/benchmarks)\]

| Benchmark | Solaria-3 | Solaria-1 | Interpretation |
| --- | --- | --- | --- |
| **Earnings22 Cleaned AA** | 6,4% WER | 8,1% WER | Solaria-3 deutlich besser bei Business-/Earnings-Audio. \[[gladia](https://www.gladia.io/solaria-3)\] |
| **Switchboard, Telefon-/Konversationsaudio** | 33,9% WER | 37,3% WER | Solaria-3 besser bei schwerem, natürlichem Gesprächsaudio. \[[gladia](https://www.gladia.io/solaria-3)\] |
| **Noisy Audio** | 1,4% WER | 1,9% WER | Solaria-3 besser bei Rauschen. \[[gladia](https://www.gladia.io/solaria-3)\] |
| **Common Voice 24** | 6,9% WER | 8,2% WER | Solaria-3 besser. \[[gladia](https://www.gladia.io/solaria-3)\] |
| **FLEURS** | 3,7% WER | 3,9% WER | Beide stark; Solaria-3 leicht besser. \[[gladia](https://www.gladia.io/solaria-3)\] |
| **VoxPopuli Cleaned AA** | schlechter | besser | Solaria-1 besser bei formeller, institutioneller Sprache. \[[gladia](https://www.gladia.io/solaria-3)\] |
| **Multilingual LibriSpeech** | schlechter | besser | Solaria-1 besser bei sauberer, vorgelesener Sprache. \[[gladia](https://www.gladia.io/solaria-3)\] |

### Deutsch im Vergleich

Für **German** berichtet Gladia eine Verbesserung von Solaria-3 gegenüber Solaria-1 um **3% auf echtem Kundenaudio** und **13% auf Common Voice 24**. Das heißt: Solaria-3 ist bei deutschem Real-World-Audio etwas besser, aber der Vorsprung ist kleiner als bei Englisch oder Französisch.\[[gladia](https://www.gladia.io/solaria-3)\]

**Solaria-3 nur interessant als guter Offline-Benchmark-Kandidat für DE**, aber **Solaria-1 bleibt der einzige Gladia-Kandidat für den Live-Pfad**.\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[gladia](https://www.gladia.io/solaria-3)\]

## Unabhängige Einordnung

Externe Quellen zu Gladias eigenen ausführlichen Benchmark:

- **AssemblyAI-Benchmarks** listen Solaria-1 mit 13,7% WER und Deepgram Nova-3 mit 14,4% WER; das zeigt, dass die Rangfolge stark vom Testset und der Normalisierung abhängt.\[[assemblyai](https://www.assemblyai.com/benchmarks?ref=implicator.ai)\]
- **APIbenchmarks** bestätigt Solaria-3 als starkes Modell für Business- und Telefon-Audio: 6,4% WER auf Earnings22 und 33,9% auf Switchboard; gleichzeitig wird die Dokumentation als solide, aber weniger umfassend als bei Deepgram oder AssemblyAI beschrieben.\[[apibenchmarks](https://apibenchmarks.com/stt/gladia)\]
- **Voibe** ordnet Solaria-3 speziell für europäisches, verrauschtes Business-Audio ein und sieht Deepgram Nova-3 eher als Option für schnelles Streaming.\[[getvoibe](https://www.getvoibe.com/resources/openai-whisper-alternatives/)\]

Bestätigt bisheriges Bild: **Gladia ist stark bei Offline-Qualität und mehrsprachiger Analyse; Deepgram ist stark bei Streaming-Infrastruktur und Voice-Agent-Integration.**\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[getvoibe](https://www.getvoibe.com/resources/openai-whisper-alternatives/)\]

## Verarbeitungsort und Datenschutz

Gladia bietet laut eigener Enterprise-Dokumentation **EU- und US-Datenresidenz** an. Für ein deutsches/EU-Telefonprojekt wäre die EU-Region der relevante Modus.

Wichtige Punkte:

- **Kein On-Premise-Deployment:** Gladia ist eine SaaS-Lösung; Modelle nicht selbst in  Infrastruktur betreibbar.
- **Datenresidenz ist konfigurierbar:** EU oder USA, abhängig von Vertrag bzw. Enterprise-Setup.
- **Compliance:** Gladia wirbt mit GDPR, HIPAA und SOC 2 Type 2.\[[apibenchmarks](https://apibenchmarks.com/stt/gladia)\]
- **Retention:** Live-Einsatz: explizit klären, wie lange Audio, Transkripte, Übersetzungen und Metadaten gespeichert werden und ob ein Zero-Retention-/Opt-out-Modus verfügbar ist. Gefundenen Quellen keine klare, dokumentierte `mip_opt_out`-ähnliche Option wie bei Deepgram.

## Lizenz und kommerzielle Nutzung

Gladia ist **kommerzielle SaaS-Software**, kein Open-Source-Modell und kein Self-Hosting-Produkt. Erwerb von API-Zugang bzw. Enterprise-Vertrag; die Modellgewichte bleiben bei Gladia:

- Kein Betrieb in eigener Infrastruktur.
- Keine freie Modellgewicht-Nutzung.
- Kein klassisches Fine-Tuning der ASR-Modelle.
- Anpassung primär über **Custom Vocabulary**, Sprachkonfiguration und optionale Audio-Intelligence-Features.\[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\]

## Terminologie und Kontext

Gladia unterstützt **Custom Vocabulary**: Übergabe domänenspezifischer Begriffe, Produktnamen, Firmennamen oder Fachausdrücke \[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\]

```
Glossar:
- „Anfrage“
- „Kundennummer“
- „Vorgangs-ID“
- „Rechnung“
   ↓
Gladia Custom Vocabulary
   ↓
präziseres Transkript
   ↓
MT / TTS
```

Gladia bietet außerdem Translation, Named Entity Recognition, Diarization und Sentiment als Audio-Intelligence-Features, z.B. **Translation**, **NER** und **Diarization** interessant.\[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\]

## Preismodell

Gladia rechnet primär nach **Audio-Stunde** bzw. Minute. Die aktuellen Self-Serve-Preise sind:

| Nutzung | Solaria-1 | Solaria-3 | Translation-Add-on |
| --- | --- | --- | --- |
| **Async / Batch** | 0,27 EUR/Stunde bzw. 0,0045 EUR/Minute | 0,16 EUR/Stunde bzw. 0,0027 EUR/Minute | 0,05 EUR/Stunde |
| **Realtime** | 0,68 EUR/Stunde bzw. 0,0113 EUR/Minute | Nicht verfügbar | 0,05 EUR/Stunde |

Für Live-Übersetzung relevante Kostenbasis:

```
Solaria-1 Realtime: 0,68 EUR/Stunde
+ Translation: 0,05 EUR/Stunde
+ TTS-Kosten
+ Telefonie/Infrastruktur
```

Das ergäbe allein für Gladia-STT plus Textübersetzung etwa **0,73 EUR/Stunde**, bevor TTS, MT-Fallback, Telefonie und eigener Betrieb dazukommen.

## Vergleich Solaria 1 versus Solaria 3

| Kriterium | **Gladia Solaria-1** | **Gladia Solaria-3** |
| --- | --- | --- |
| **Modell** | Solaria-1, Gladias Standard-/Generalist-Modell. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] | Solaria-3, Gladias neuestes ASR-Modell für europäisches Real-World-Audio. \[[gladia](https://www.gladia.io/solaria-3)\]\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] |
| **Architektur** | Cloud-STT-Dienst mit REST für Async und WebSocket für Realtime; Audio → strukturiertes Transkript. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] | Cloud-STT-Dienst ausschließlich für vorab aufgezeichnetes Audio; Audio → strukturiertes Transkript. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[docs.gladia](https://docs.gladia.io/chapters/pre-recorded-stt/quickstart)\] |
| **Eignung für Live Translation** | **Ja, als STT-Baustein**: Live-Transkript kann an eine externe MT- und TTS-Komponente weitergegeben werden. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] | **Nein, nicht für Live-Übersetzung**: Kein Realtime-Modus; nur für Nachbearbeitung aufgezeichneter Anrufe. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[docs.gladia](https://docs.gladia.io/chapters/pre-recorded-stt/quickstart)\] |
| **Realtime-/Streaming-Fähigkeit** | **Ja.** WebSocket-Streaming, Partial Results, VAD; laut Gladia ca. 103 ms optimale bzw. 270 ms durchschnittliche Partial-Latenz. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[gladia](https://www.gladia.io/blog/best-real-time-stt-models-for-meeting-assistants-2026)\] | **Nein.** Explizit „Pre-recorded only“; kein Live-/Streaming-Modus. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[docs.gladia](https://docs.gladia.io/chapters/pre-recorded-stt/quickstart)\] |
| **Sprachunterstützung** | 100+ Sprachen, inklusive Deutsch und Englisch; automatische Spracherkennung. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] | Optimiert für EN, FR, DE, ES und IT; Deutsch und Englisch sind enthalten. \[[gladia](https://www.gladia.io/solaria-3)\]\[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] |
| **Bidirektionalität** | Nicht inhärent: Gladia liefert Transkript/Übersetzung; für DE↔EN-Sprachausgabe braucht ihr externe TTS. | Nicht inhärent: Nur Transkription, keine Übersetzung als Kernfunktion; MT/TTS extern. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[docs.gladia](https://docs.gladia.io/chapters/pre-recorded-stt/quickstart)\] |
| **Code-Switching** | **Ja**, nativ in Async und Realtime; erkennt Sprachwechsel innerhalb eines Gesprächs. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\] | **Nein.** Einmal erkannte Sprache bleibt für die restliche Datei fixiert; keine Mid-File-Sprachwechsel. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] |
| **Telefonie-/Audio-Kompatibilität** | Gut: Realtime-WebSocket, VAD, Speaker Labels; für Telefon-Codecs und euren Audio-Runner nutzbar. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\] | Sehr gut für Call-Center-/Meeting-Audio, aber nur offline; Dateien bis 135 Minuten und 1.000 MB. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[gladia](https://www.gladia.io/solaria-3)\] |
| **Technische Evaluierbarkeit** | Sehr gut: WER/CER, Word-Timestamps, Diarization, Translation, NER, Custom Vocabulary; mit jiwer reproduzierbar testbar. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\] | Sehr gut: WER/CER, Word-Timestamps, Diarization, Translation, NER; besonders gut für Offline-Benchmarks. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[gladia](https://www.gladia.io/solaria-3)\] |
| **Produktreife / Entwicklungsstatus** | Produktionsreif und Standardmodell; breit für Live- und Async-Workloads ausgelegt. \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]\[[docs.gladia](https://docs.gladia.io/chapters/pre-recorded-stt/quickstart)\] | Neu (Juni 2026), produktionsreif für Async, aber noch ohne Realtime-Unterstützung. \[[gladia](https://www.gladia.io/solaria-3)\]\[[getvoibe](https://www.getvoibe.com/resources/openai-whisper-alternatives/)\] |
| **Verarbeitungsort / Datenregion** | Cloud mit EU- und US-Datenresidenz; kein On-Premise. | Gleiche Cloud-Infrastruktur; EU- oder US-Datenresidenz je nach Konfiguration. |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle SaaS-API; kein Open Source, kein Self-Hosting. | Kommerzielle SaaS-API; kein Open Source, kein Self-Hosting. |
| **Terminologie-/Kontextanpassung** | Custom Vocabulary für domänenspezifische Begriffe; Translation, Diarization und NER als API-Features. \[[gladia](https://www.gladia.io/blog/automating-lead-enrichment-with-ai-speech-to-text)\] | Custom Vocabulary und Translation verfügbar; Fokus auf hohe Erkennungsgenauigkeit in fünf europäischen Sprachen. \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]\[[docs.gladia](https://docs.gladia.io/api-reference/v2/pre-recorded/init)\] |
| **Preismodell** | Realtime: 0,68 EUR/Stunde bzw. 0,0113 EUR/Minute; Translation-Add-on: 0,05 EUR/Stunde. | Async: ab 0,16 EUR/Stunde bzw. 0,0027 EUR/Minute; Translation-Add-on 0,05 EUR/Stunde. |


## Referenzen

- Gladia Solaria-3 Produktseite und Benchmark-Tabellen: <https://www.gladia.io/solaria-3> \[[gladia](https://www.gladia.io/solaria-3)\]
- Gladia-Dokumentation zu Solaria-1 und Solaria-3: <https://docs.gladia.io/chapters/introduction/models> \[[docs.gladia](https://docs.gladia.io/chapters/introduction/models)\]
- Gladia Async-STT-Dokumentation: <https://www.gladia.io/product/speech-to-text-api/async> \[[gladia](https://www.gladia.io/product/speech-to-text-api/async)\]
- Gladia Realtime-STT-Dokumentation: <https://www.gladia.io/product/real-time> 
- Gladia Speech-to-Text-Translation: <https://www.gladia.io/product/speech-to-text-api> 
- Gladia Pricing: <https://www.gladia.io/pricing> 
- Gladia Open Benchmark: <https://www.gladia.io/competitors/benchmarks> \[[gladia](https://www.gladia.io/competitors/benchmarks)\]
- Gladia Enterprise-/On-Premise-/Datenresidenz-Artikel: <https://www.gladia.io/blog/enterprise-and-on-premise-speech-to-text-security-sla-and-data-residency> 
- Unabhängige Vergleichsquelle: AssemblyAI Benchmarks: <https://www.assemblyai.com/benchmarks> \[[assemblyai](https://www.assemblyai.com/benchmarks?ref=implicator.ai)\]
- Unabhängige Vergleichsquelle: APIbenchmarks zu Gladia: <https://apibenchmarks.com/stt/gladia> \[[apibenchmarks](https://apibenchmarks.com/stt/gladia)\]
- Unabhängige Vergleichsquelle: Voibe, „OpenAI Whisper Alternatives“: <https://www.getvoibe.com/resources/openai-whisper-alternatives/> \[[getvoibe](https://www.getvoibe.com/resources/openai-whisper-alternatives/)\]
