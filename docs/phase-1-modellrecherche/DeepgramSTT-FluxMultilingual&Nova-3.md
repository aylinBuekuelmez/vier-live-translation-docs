# Deepgram STT \- Flux Multilingual & Nova\-3




About Deepgram: Deepgram offers both, STT-TTS, and Voice Agent (for this, see other column in table, and resp. document. 

Testing:

offers free credit of $200 to test: to run a standard voice agent for more than 40 hours. No credit card needed. apprently for both , STT, TTS, Voice Agent

(Deepgram website: ​<https://console.deepgram.com/signup>  , 16.10.2026)


# STT

## Kurzes Fazit STT: 

| **Flux** | **Nova 3** |
| --- | --- |
| gut für Voice und Turnwechsel, also Unterhaltungen | akurate Transkription und viele Sprachen |
| höhere WER, weniger Sprachen | mehrere Sprecher\*innen werden dokumentiert, Turnwechsel nicht wie bei Flux, andere Technik, siehe weiter unten |


## Flux multilingual

Deepgram:

“Streaming-Modell der neuesten Generation, das erstklassige ASR mit modellnativer Dreherkennung vereint. Empfohlen für Echtzeit-Agenten, Kundensupport-Bots und interaktive, rundenbasierte Erlebnisse.”

spezialisiertes, niedriglatentes Streaming-STT-Modell für Voice Agents. Es ist besonders stark bei Turn Detection, Barge-in und Code-Switching, bietet aber kein Batch-Transkription und kein Smart Formatting


**Verfügbare Sprachen: **

en, es, fr, de, hi, ru, pt, ja, it, nl


**Technische Aspekte:**

Flux wird angepriesen als sehr gut in Turn-Recognition, konfigurierbaren Turn-Taking Dynamics, sehr geringer Latenz optimiert für Voice Agent Pipelines 


**Testing**:

Flux ist ein Streaming-STT-Modell und bietet keinen klassischen Batch-Endpoint, bei dem man eine komplette Datei hochlädt und ein fertiges Transkript zurückbekommt; voraufgezeichnete WAV-Testdateien trotzdem verwenden: Runner kleine Audio-Chunks zerlegt + WebSocket an Flux streamen;

\- vereint Transkription, End-of-Turn-Erkennung, Interruption-/Barge-in-Handling und Code-Switching in einem Modell und läuft ausschließlich über den WebSocket-Endpunkt `/v2/listen`. Für  Live-Telefonübersetzung interessant, wegen schneller Reaktionen auf Sprecherwechsel und Unterbrechungen \[[developers.deepgram](https://developers.deepgram.com/docs/flux-self-hosted)\]\[[developers.deepgram](https://developers.deepgram.com/docs/voice-agent-stt-models)\]

Das Multilingual-Modell unterstützt zehn Sprachen, darunter Deutsch und Englisch, und kann Sprachwechsel innerhalb eines Gesprächs erkennen --\> für reine DE- bzw. EN-Pfade als auch für gemischte Gespräche testbar

! Wichtig: Flux bietet kein Smart Formatting; Zahlen, Interpunktion und Formatierung → gegebenenfalls selbst normalisieren.\[[developers.deepgram](https://developers.deepgram.com/docs/voice-agent-stt-models)\]

\- interessant: **Latenz, Turn Detection, Barge-in und Stabilität**; WER-/Entity-Qualitätsvergleiche  wegen Smart Formatting, Entity Detection und Batch-Unterstützung eher mit  Nova-3 \[[developers.deepgram](https://developers.deepgram.com/docs/flux-self-hosted)\]

**Empfohlene Testkonfiguration:**  
`flux-general-multi` mit `language_hints` für DE/EN, WebSocket-Streaming, Partial Results protokollieren, Latenz und Revisionsverhalten im Runner messen.\[[developers.deepgram](https://developers.deepgram.com/docs/flux-self-hosted)\]


| Kriterium | Bewertung |
| --- | --- |
| **Modell** | `flux-general-en`, `flux-general-de`, `flux-general-multi`. |
| **Architektur** | Conversational Streaming-STT: Audio → Text, mit integriertem End-of-Turn und Interruption Handling. |
| **Eignung für Live Translation** | Sehr gut für Live-Telefonie; MT und TTS extern. |
| **Realtime-/Streaming-Fähigkeit** | Ja, ausschließlich WebSocket `/v2/listen`; kein Batch-Modus. |
| **Sprachunterstützung / Bidirektionalität** | DE und EN über Multilingual-Modell; Code-Switching integriert. |
| **Telefonie-/Audio-Kompatibilität** | Sehr gut: Turn Detection, Barge-in, VAD-ähnliche Gesprächslogik eingebaut. |
| **Technische Evaluierbarkeit** | Sehr gut für Latenz, Turns, Barge-in, Code-Switching; WER/CER mit jiwer möglich. |
| **Produktreife / Status** | Relativ neu, aber gezielt für Voice Agents entwickelt; Multilingual seit 2026. |
| **Verarbeitungsort / Datenregion** | Deepgram Cloud oder Self-Hosted; Self-Hosting benötigt Lizenzserver-Verbindung. [developers.deepgram](https://developers.deepgram.com/docs/self-hosted-deployment-environments) |
| **Datennutzung** | Gleiche Deepgram-Datenarchitektur: MIP standardmäßig aktiv, Opt-out mit `mip_opt_out=true`. [developers.deepgram](https://developers.deepgram.com/trust-security/your-data.md) |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle API-/Self-Hosting-Lizenz; kein Open Source. [developers.deepgram](https://developers.deepgram.com/docs/self-hosted-deployment-environments) |
| **Terminologie-/Kontextanpassung** | Keyterm Prompting bis 100 Begriffe; Language Prompting im Multilingual-Modell. |
| **Preismodell** | Pro Audio-Minute: EN ca. 0,0065–0,0077 USD/min, Multilingual ca. 0,0078 USD/min; Keyterms ca. 0,0013 USD/min. |

**Referenzen:**

- Flux Overview: <https://developers.deepgram.com/docs/flux-self-hosted> 
- Flux Multilingual: <https://developers.deepgram.com/docs/flux/language-prompting> 
- Flux Quickstart: <https://developers.deepgram.com/docs/flux/quickstart> 
- Voice Agent STT Models: <https://developers.deepgram.com/docs/voice-agent-stt-models> 
- Latenz-Architektur: <https://deepgram.com/learn/real-time-voice-ai-stack-agents-architecture-guide> 
- Datenutzung: <https://developers.deepgram.com/trust-security/your-data> [developers.deepgram](https://developers.deepgram.com/trust-security/your-data.md)
- Flux Overview: <https://developers.deepgram.com/docs/flux-self-hosted> \[[developers.deepgram](https://developers.deepgram.com/docs/flux-self-hosted)\]
- Flux Multilingual: <https://developers.deepgram.com/docs/flux/language-prompting> 
- Voice Agent STT Models: <https://developers.deepgram.com/docs/voice-agent-stt-models> \[[developers.deepgram](https://developers.deepgram.com/docs/voice-agent-stt-models)\]


## Nova-3

**Deepgram Beschreibung: **

**Nova-3** ist Deepgrams breiteres Flaggschiff-STT-Modell für Batch- und Echtzeit-Transkription. Es unterstützt Streaming mit Interim Results und Endpointing sowie Multilingual-Transkription mit Code-Switching; Deepgram gibt für Streaming eine Latenz unter 300 ms an


**Verfügbare Sprachen: Nove 3 oder nova general: **

nova-3 oder nova-3-general, Mehrsprachig (Englisch, Spanisch, Französisch, Deutsch, Hindi, Russisch, Portugiesisch, Japanisch, Italienisch und Niederländisch): multi, Afrikaans: af, af-ZA, Arabisch: ar, ar-AE, ar-SA, ar-QA, ar-KW, ar-SY, ar-LB, ar-PS, ar-JO, ar-EG, ar-SD, ar-TD, ar-MA, ar-DZ, ar-TN, ar-IQ, ar-IR, Armenier: hy, Assamese: as, as-IN, Weißrussisch: be, Bengalisch: bn, Bosnisch: bs, Bulgarisch: bg, Katalanisch: ca, Chinesisch (Kantonisch, Traditionell): zh-HK, Chinesisch (Mandarin, Vereinfacht): zh, zh-CN, zh-Hans, Chinesisch (Mandarin, Traditionell): zh-TW, zh-Hant, Kroatisch: hr, Tschechisch: cs, cs-CZ, Dänisch: da, da-DK, Niederländisch: nl, Englisch: en, en-US, en-AU, en-GB, en-IN, en-NZ, Estnisch: et, Finnisch: fi, Flämisch: nl-BE, Französisch: fr, fr-CA, Georgisch: ka, ka-GE, Deutsch: de, Deutsch (Schweiz): de-CH, Griechisch: el, Gujarati: gu, gu-IN, Hebräisch: he, Hindi: hi, Ungarisch: hu, Indonesisch: id, Italienisch: it, Japanisch: ja, Kannada: kn, Kasachisch: kk, kk-KZ, Koreanisch: ko, ko-KR, Lettisch: lv, Litauisch: lt, Mazedonisch: mk, Malaiisch: ms, Marathi: mr, Mongolisch: mn, Nepalesisch: ne, Norwegisch: no, Paschtu: ps, ps-AF, Persisch: fa, Polnisch: pl, Portugiesisch: pt, pt-BR, pt-PT, Punjabi: pa, pa-IN, Rumänisch: ro, Russisch: ru, Serbisch: sr, Slowakisch: sk, Slowenisch: sl, Spanisch: es, es-419, Schwedisch: sv, sv-SE, Tagalog: tl, Tamilisch: ta, Telugu: te, Thai: th, th-TH, Türkisch: tr, tr-TR, Ukrainisch: uk, Urdu: ur, Vietnamesisch: vi


**Technische Aspekte:**

54.2% Reduktion in  word error rate (WER) for streaming im Vergleich zu anderen Anbietern, 47,4 % für batch processing, also schon aufgenommene Gespräche

Stärke Nova3: mehrsprachige Konversationstranskription in Echtzeit, ein verbessertes Verständnis der domänenspezifischen Terminologie und optionale Schwärzung personenbezogener Daten. Insbesondere ist es das erste Voice-AI-Modell, das eine Selbstbedienung bietet und eine sofortige Anpassung des Wortschatzes ohne Modellumschulung ermöglicht. In mehrsprachigen Tests zeigte Nova-3 eine überlegene Leistung im Vergleich mit OpenAI Whisper in allen sieben getesteten Sprachen, wobei besonders starke Ergebnisse bis zu 8:1-Präferenzverhältnisse in bestimmten Sprachen aufwiesen.


**Testing**

Gegenüber Flux bietet Nova-3 mehr Transkriptions-Features: **Smart Formatting**, **Entity Detection**, **Keyterm Prompting** und eine breitere Sprachabdeckung → Tests zu Zahlen, Eigennamen, Beträgen, Negationen und Fachbegriffen \[[deepgram](https://deepgram.com/voice-ai-platform)\]

Nova-3 Multilingual für gemischte Gespräche oder automatische Spracherkennung  
`nova-3` mit `language=multi`, `interim_results=true` und `endpointing=300`; WER/CER mit jiwer, Latenz aus Audio- und Transcript-Cursor, Entity- und Keyterm-Treffer separat auswerten.


| Kriterium | Bewertung |
| --- | --- |
| **Modell** | Nova-3, Nova-3 Multilingual, Nova-3 Medical. [developers.deepgram](https://developers.deepgram.com/docs/measuring-streaming-latency.mdx) |
| **Architektur** | Cascadierter STT-Baustein: Audio → Text; Batch über REST, Streaming über WebSocket. [developers.deepgram](https://developers.deepgram.com/docs/measuring-streaming-latency.mdx) |
| **Eignung für Live Translation** | Sehr gut als Eingangsstufe: `Audio → Text`; MT und TTS extern. developers.deepgram+1 |
| **Realtime-/Streaming-Fähigkeit** | Ja: WebSocket, Interim Results, Endpointing; Deepgram nennt sub-300-ms-Streaming-Latenz. [developers.deepgram](https://developers.deepgram.com/docs/measuring-streaming-latency.mdx) |
| **Sprachunterstützung / Bidirektionalität** | DE und EN; Multilingual-Modell für Code-Switching. [developers.deepgram](https://developers.deepgram.com/docs/language-detection) |
| **Telefonie-/Audio-Kompatibilität** | Gut: Streaming-Audio, VAD, Endpointing; für Telefonie mit AEC/Codec-Handling kombinierbar. [developers.deepgram](https://developers.deepgram.com/docs/measuring-streaming-latency.mdx) |
| **Technische Evaluierbarkeit** | Sehr gut: WER/CER mit jiwer, Interim Results, Latenz, Keyterms, Smart Formatting, Entity Detection. [developers.deepgram](https://developers.deepgram.com/docs/stt-streaming-feature-overview) |
| **Produktreife / Status** | Reif; Flaggschiff-Modell mit breitem Feature-Set. developers.deepgram+1 |
| **Verarbeitungsort / Datenregion** | Deepgram Cloud (global/regional) oder Self-Hosted: Docker, Kubernetes, SageMaker, Bare Metal. [developers.deepgram](https://developers.deepgram.com/docs/self-hosted-deployment-environments) |
| **Datennutzung** | Standardmäßig Teil des Model Improvement Program; mit `mip_opt_out=true` Zero Retention nach der Antwort. [developers.deepgram](https://developers.deepgram.com/trust-security/your-data.md) |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle API-/Self-Hosting-Lizenz; kein Open Source. [developers.deepgram](https://developers.deepgram.com/docs/self-hosted-deployment-environments) |
| **Terminologie-/Kontextanpassung** | Keyterm Prompting bis 100 Begriffe; Smart Formatting; Entity Detection. [developers.deepgram](https://developers.deepgram.com/docs/stt-streaming-feature-overview) |
| **Preismodell** | Pro Audio-Minute: Monolingual ca. 0,0043–0,0077 USD/min, Multilingual ca. 0,0052–0,0092 USD/min; Keyterms ca. 0,0013 USD/min. |

**Referenzen:**

- Deepgram Nova-3 Streaming-Latenz: <https://developers.deepgram.com/docs/measuring-streaming-latency> [developers.deepgram](https://developers.deepgram.com/docs/measuring-streaming-latency.mdx)
- Interim Results: <https://developers.deepgram.com/docs/interim-results> 
- Endpointing: <https://developers.deepgram.com/docs/understand-endpointing-interim-results> 
- Sprachunterstützung: [https://developers.deepgram.com/docs/language-detectiondevelopers.deepgram](https://developers.deepgram.com/docs/language-detection)
- Self-Hosting/Datenregion: [https://developers.deepgram.com/docs/self-hosted-deployment-environmentsdevelopers.deepgram](https://developers.deepgram.com/docs/self-hosted-deployment-environments)
- Datenutzung: <https://developers.deepgram.com/trust-security/your-data> [developers.deepgram](https://developers.deepgram.com/trust-security/your-data.md)
- Preise: <https://deepgram.com/voice-ai-platform> 

- Nova-3 Streaming-Latenz: <https://developers.deepgram.com/docs/measuring-streaming-latency> 
- Interim Results: <https://developers.deepgram.com/docs/interim-results> 
- Endpointing: <https://developers.deepgram.com/docs/understand-endpointing-interim-results> 
- Sprachen: <https://developers.deepgram.com/docs/language-detection> 
- Keyterms/Preise: <https://deepgram.com/voice-ai-platform> \[[deepgram](https://deepgram.com/voice-ai-platform)\]


## Deepgram eigener Vergleich Flux - Nova 3 (<https://developers.deepgram.com/docs/flux/flux-nova-3-comparison> , 16.10.2026)

### Use Cases

| Feature | Flux | Nova-3 |
| --- | --- | --- |
| Voice Agents | ✅ | ✅ |
| IVR Systems | ✅ | ✅ |
| Agent Assist | ✅ | ✅ |
| Real-time Transcription | ✅ | ✅ |
| Meeting Transcription | 🚫 | ✅ |
| Event Captioning | 🚫 | ✅ |
| Pre-recorded Audio | 🚫 | ✅ |
| Call Analytics | 🚫 | ✅ |

### Language Support

| Feature | Flux | Nova-3 |
| --- | --- | --- |
| English | ✅ | ✅ |
| Multilingual (10 languages) | ✅ `flux-general-multi` ([Language Prompting](/docs/flux/language-prompting)) | ✅ [See language support](/docs/models-languages-overview#nova-3) |
| Monolingual languages | 🚫 | ✅ [Full list](/docs/models-languages-overview#nova-3) |
| Language control | `language_hint` parameter | `language` parameter |
| Code-switching | ✅ Native with `flux-general-multi` | ✅ With `language=multi` |

### Technical Details

| Feature | Flux | Nova-3 |
| --- | --- | --- |
| Self-hosted Availability | ✅ | ✅ |
| Endpoint | `/v2/listen` | `/v1/listen` |

### Conversation Features

| Feature | Flux | Nova-3 |
| --- | --- | --- |
| Start of Turn | ✅ | 🚫 |
| Speech Resumed | ✅ | 🚫 |
| End of Turn | ✅ | 🚫 |
| Eager End of Turn | ✅ | 🚫 |

### Transcription Features

| Feature | Flux | Nova-3 |
| --- | --- | --- |
| Word Times | ✅ | ✅ |
| Smart Formatting | 🚫 | ✅ |
| Numerals | ✅ | ✅ |
| Speaker Diarization | 🚫 | ✅ |
| Filler Words | ✅ Transcribed by default | 🚫 |
| Profanity Filtering | 🚫 | ✅ |
| Redaction | ✅ Numbers only | ✅ |
| Find and Replace | 🚫 | ✅ |
| Keyterm Prompting | ✅ | ✅ |
| Search | 🚫 | ✅ |
| Turn-based Transcription | ✅ | 🚫 |

### Connection Management

| Feature | Flux | Nova-3 |
| --- | --- | --- |
| Endpointing + Utterance End | ✅ (Replaced by model integrated turn detection) | ✅ |
| Interim Results | ✅ (Replaced by Update Messages) | ✅ |
| Keep Alive | ✅ (Replaced by pings, 60s timeout) | ✅ (12s timeout) |

## Vergleich Turnwechsel Flux - Nova 3

von Claude verglichen

**Turnwechsel: Flux und Nova-3 unterscheiden sich in der Art der Erkennung:** 

Laut Deepgrams Vergleichstabelle hat nur Flux die modellbasierten Gesprächs-Events Start of Turn, Speech Resumed, End of Turn und Eager End of Turn, während Nova-3 diese nicht bietet. Nova-3 kennt aber weiterhin Endpointing und Utterance End. Das Endpointing arbeitet mit einem Voice Activity Detector und löst aus, wenn eine ausreichend lange Pause erkannt wird. Nova-3 erkennt also "die Person hat aufgehört" nur über Stille, nicht über den Inhalt des Satzes. Die Pausendauer lässt sich in Millisekunden einstellen, und der Standardwert liegt laut Doku bei sehr kurzen 10 Millisekunden, müsste also ggf. angepasst werden.

**Mehrsprecher: **

Nova-3 unterstützt Speaker Diarization, Flux nicht (auch keine Wortzeitstempel). Deepgram empfiehlt Nova-3 ausdrücklich für Meetings, Event-Captioning, Mehrsprecher-, mehrsprachige, laute oder Fernfeld-Audioquellen. Flux ist dagegen gezielt für Voice Agents gebaut, also für das Gespräch zwischen Mensch und KI. Diarization ist dabei kein Turn-Detection-Ersatz, sondern gibt an, wer spricht, nicht, wann eine Äußerung zu Ende ist. Wenn jede\*n Sprecher\*in auf einem eigenen Kanal bekommt, eventuell unnötig, denn der Streaming-Endpunkt kann mit Multichannel jeden Audiokanal unabhängig transkribieren.

**Übersetzungen betreffend: **

"wo werden Segmente geschnitten, die an die Übersetzung gehen?". Das ist bei einer Kaskade der wichtigste Stellhebel:

- zu früh (kurzes Endpointing): Latenz sinkt, aber die Übersetzung bekommt halbe Sätze und wird schlechter
- zu spät: Gespräch, wirkt träge
- Interim Results zusätzlich nutzen: selbst entscheiden, wann ein Segment für die Übersetzung reif ist, zum Beispiel am Satzende → bei Nova-3 selbst bauen, bei Flux im Modell teilöweise vorhanden
