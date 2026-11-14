# Google Cloud Speech\-to\-Text \+ Translation \+ Text\-to\-Speech

**Recherche-Stand:** 31.10.2026  
**Bezugsform:** Cloudbasierter API-Dienst

**Produktdokumentation: **

- [Cloud Speech-to-Text – Übersicht Documentation](https://docs.cloud.google.com/speech-to-text/docs/overview?hl=de)
- [Cloud Text-to-Speech Documentation](https://docs.cloud.google.com/text-to-speech/docs/basics?hl=de)
- [Cloud Translation Documentation](https://docs.cloud.google.com/translate/docs/overview?hl=de)   


**Übersicht:**


#### **Eignung für Live-Translation**

Gut geeignet. Die Kaskade aus Google-Cloud-Diensten kann zusammen funktionieren, dadurch kann eine Live-Translation-Pipeline aufgebaut werden, ohne dass noch weitere Anbieter hinzugefügt werden müssen. Echtzeit-streaming vorhanden. Es werden unterschiedliche Modelle für die Übersetzung angeboten. Die Modelle bieten fast alle Sprachen an, mit denen wir testen möchten (außer Estnisch).


#### **Architektur**

Kaskadierend

*Speech-to-Text:* bidirektionaler gRPC-Stream. “Bei Streaminganfragen erfolgt die Erkennung in Echtzeit, zum Beispiel bei der Aufnahme von Live-Audio mit einem Mikrofon. Die Streamingerkennung liefert während der Aufnahme der Audiodaten Zwischenergebnisse. So können Ergebnisse zum Beispiel bereits angezeigt werden, während ein Nutzer noch spricht.” Quelle:  <https://docs.cloud.google.com/speech-to-text/docs/overview?hl=de#recognition-requests>, 16.10.26

*Translation AI: “*Mit Cloud Translation können Ihre Websites und Anwendungen Text dynamisch und programmatisch über eine API übersetzen. Cloud Translation verwendet für das Übersetzen von Text ein von Google vortrainiertes oder ein benutzerdefiniertes Modell für maschinelles Lernen.” Quelle: <https://docs.cloud.google.com/translate/docs/overview?hl=de>,  16.10.26

*Text-to-Speech-KI: “*Text-to-Speech wandelt Text- oder SSML-Eingaben (Speech Synthesis Markup Language) in Audiodatenformate wie MP3 oder LINEAR16 um. Letzteres ist die in WAV-Dateien verwendete Codierung.” Quelle: <https://docs.cloud.google.com/text-to-speech/docs/basics?hl=de>, 16.10.26

#### **Realtime-/Streaming-Fähingkeit**

*Speech-to-Text:*bidirektionaler gRPC-Stream. “Bei Streaminganfragen erfolgt die Erkennung in Echtzeit, zum Beispiel bei der Aufnahme von Live-Audio mit einem Mikrofon. Die Streamingerkennung liefert während der Aufnahme der Audiodaten Zwischenergebnisse. So können Ergebnisse zum Beispiel bereits angezeigt werden, während ein Nutzer noch spricht.” Quelle:  <https://docs.cloud.google.com/speech-to-text/docs/overview?hl=de#recognition-requests> , 16.10.26

Chirp 3 ist das neueste Modell und unterstützt [Speech.StreamingRecognize](https://docs.cloud.google.com/speech-to-text/docs/reference/rpc/google.cloud.speech.v2?hl=de#google.cloud.speech.v2.Speech.StreamingRecognize) (geeignet für Audiostreams und Echtzeit-Audioinhalte). Quelle: [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#api\_methods](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#api_methods), 16.10.26 

Chirp 2 ist das ältere Modell und unterstützt auch `Speech.StreamingRecognize` (geeignet für Audiostreams und Echtzeit-Audioinhalte), allerdings ist hier die Sprachunterstützung begrenzt. Quelle: [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-2?hl=de#api\_methods](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-2?hl=de#api_methods), 16.10.26

*Translation AI:*

 Es gibt zwei Optionen: einmal Cloud Translation Basic API und einmal Cloud Translation Advanced API. Dieses benutzt unterschiedliche Modelle. 

| Modell | Beschreibung | Modell-ID |
| --- | --- | --- |
| [Translation LLM (TLLM)](https://docs.cloud.google.com/translate/docs/translation-llm?hl=de) | **Übersetzungsmodell mit höchster Qualität** : ein Large Language Model (LLM) das auf Gemini basiert und für die Übersetzung optimiert wurde, um Übersetzungen von höchster Qualität mit geringen Latenzen zu liefern (halbe Latenz im Vergleich zu Gemini 2.0 Flash) | `general/translation-llm` |
| [NMÜ-Modell (neuronale maschinelle Übersetzung)](https://docs.cloud.google.com/translate/docs/advanced/nmt-model?hl=de) | **Schnellstes Übersetzungsmodell** : ideal für Echtzeit- und latenz kritische Anwendungsfälle Dies ist eine aktualisierte Version der ursprünglichen neuronale maschinelle Übersetzung von Google, die über die Cloud Translation Basic API mit \[v2-Clientbibliotheken\](/translate/docs/reference/libraries/v2/overview-v2) verfügbar ist. Sie können dieses Modell sofort ohne Anpassung verwenden. | `general/nmt` |
| [Modell für adaptive maschinelle Übersetzung](https://docs.cloud.google.com/translate/docs/advanced/adaptive-translation?hl=de) | Diese Option umfasst die **einfache Echtzeitanpassung** des Translation LLM mithilfe der [adaptiven Übersetzung](https://docs.cloud.google.com/translate/docs/advanced/adaptive-translation?hl=de) auf Grundlage eines begrenzten Datasets mit Übersetzungsbeispielen. | `general/translation-llm-adaptive` |

Quelle: <https://docs.cloud.google.com/translate/docs/advanced/compare-models?hl=de> , 16.10.26

*Text-to-Speech-KI: *

“Mit bidirektionalem Streaming können Sie gleichzeitig Texteingaben senden und Audiodaten empfangen. Das bedeutet, dass Sie mit der Sprachsynthese beginnen können, bevor der vollständige Eingabetext gesendet wurde. Dadurch wird die Latenz verringert und Interaktionen in Echtzeit werden ermöglicht. Bei Sprachassistenten und interaktiven Spielen wird bidirektionales Streaming verwendet, um dynamischere Anwendungen zu erstellen, die schneller reagieren… Das Streamen von Text-to-Speech ist nur mit [Chirp 3: HD-Stimmen](https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de) kompatibel” Quelle: <https://docs.cloud.google.com/text-to-speech/docs/create-audio-text-streaming?hl=de>, 16.10.26  

*Meine Einschätzung: *GUT* weil die einzelnen Teile zusammenarbeiten können. Allerdings ist meine Einschätzung nicht gut, weil manche Features für bestimmte Modelle nur in der Preview sind oder nicht in der EU gehostet werden können. Latenz ist für die gesamte Kaskade auch noch unklar. *

StT: deu + eng, Wunschsprachen, Unsere Sprachen

#### **Sprachunterstützung und Bidirektionalität**

*Speech-to-Text:* 

Verfügbare Sprachen für die Transkription

Chirp 3 unterstützt die Transkription für `StreamingRecognize`, `Recognize` und `BatchRecognize` in den folgenden Sprachen: [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#language\_availability\_for\_transcription](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#language_availability_for_transcription) 

Verfügbare Sprachen für die Sprecherbestimmung

Chirp 3 unterstützt die Transkription und Sprecherbestimmung nur für `BatchRecognize` und `Recognize` in den folgenden Sprachen: [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#language\_availability\_for\_diarization](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#language_availability_for_diarization)  

*Translation AI: *

Sprachunterstützung. Es werden sehr viele Sprachen unterstützt. Muss einzeln geprüft werden. 

1. NMÜ-Modell (Neuronale maschinelle Übersetzung) → <https://docs.cloud.google.com/translate/docs/languages?hl=de#nmt> 
2. Benutzerdefinierte Modelle → <https://docs.cloud.google.com/translate/docs/languages?hl=de#automl> 
3. Vom Translation LLM unterstützte Sprachen → [https://docs.cloud.google.com/translate/docs/languages?hl=de#adaptive\_translation](https://docs.cloud.google.com/translate/docs/languages?hl=de#adaptive_translation) 

*Text-to-Speech-KI:*

Gemini-TTS unterstützt die folgenden Sprachen: [https://docs.cloud.google.com/text-to-speech/docs/gemini-tts?hl=de#available\_languages](https://docs.cloud.google.com/text-to-speech/docs/gemini-tts?hl=de#available_languages)

Chirp 3: HD-Stimmen werden in den folgenden Sprachen unterstützt: [https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#language\_availability](https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#language_availability)

Instant-Custom Voice wird in den folgenden Sprachen unterstützt: [https://docs.cloud.google.com/text-to-speech/docs/chirp3-instant-custom-voice?hl=de#available\_languages](https://docs.cloud.google.com/text-to-speech/docs/chirp3-instant-custom-voice?hl=de#available_languages)

16.10.26

#### **Telefonie-/Audio-Kompatibilität**

*Speech-to-Text:* in der Annahme, dass Telefoniecodes 8 kHz nutzen.

Auszug aus → 

Unterstützte Audiocodierungen für Cloud Speech-to-Text: Die Cloud Speech-to-Text API unterstützt verschiedene Codierungen. In der nachstehenden Tabelle sind die unterstützten Audio-Codecs aufgelistet:

| Codec | Name | Hinweise zur Verwendung |
| --- | --- | --- |
| `MULAW` | μ-law | 8-Bit-PCM-Codierung |
| `AMR` | Adaptive Multi-Rate Narrowband | Abtastrate muss 8.000 Hz sein |
| `OGG_OPUS` | Opus-codierte Audioframes in einem Ogg-Container | Abtastrate muss 8.000 Hz, 12.000 Hz, 16.000 Hz, 24.000 Hz oder 48.000 Hz sein. |
| `WEBM_OPUS` | WebM Opus | Abtastrate muss 8.000 Hz, 12.000 Hz, 16.000 Hz, 24.000 Hz oder 48.000 Hz sein. |

Quelle: <https://docs.cloud.google.com/speech-to-text/docs/encoding?hl=de#audio-encodings>, 16.10.26  

Es gibt ein Modell Telephony, das aber sonst nicht weiter erwähnt wird. [https://docs.cloud.google.com/speech-to-text/docs/transcription-model?hl=de#select\_the\_right\_transcription\_model](https://docs.cloud.google.com/speech-to-text/docs/transcription-model?hl=de#select_the_right_transcription_model) 

|  |  |
| --- | --- |
| `telephony` | Verwenden Sie dieses Modell für Audiodaten, die aus einem Telefonanruf stammen und in der Regel mit einer Abtastrate von 8 kHz aufgezeichnet wurden. Ideal für Kundenservice, Telefonkonferenzen und automatisierte Kioskanwendungen. |

*Translation AI:  nicht relevant*

*Text-to-Speech-KI: *

Es werden folgende Formate unterstützt: außer MP3 sind das die passenden Codes für Telefonie. Quelle: [https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#supported\_output\_formats](https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#supported_output_formats), 16.10.16

| API-Methode | Format |
| --- | --- |
| `streaming` | ALAW, MULAW, OGG\_OPUS und PCM |
| `batch` | ALAW, MULAW, MP3, OGG\_OPUS und PCM |


#### **Technische Evaluierbarkeit**

*Speech-to-Text:*  Es gibt eine Konsole und ein Colab-Notebook zum Ausprobieren.

*Translation AI: *

*Text-to-Speech-KI: *Kostenloses Konto in Vertex Ai Studio zum Testen und Colab-Notebook .



#### **Produktreife / Entwicklungsstatus**

*Speech-to-Text:*  Chirp 3 in EU multi-region als GA. Quelle: [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#regional\_availability](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#regional_availability), 16.10.26

*Translation AI:  *Advanced Modell empholen. Status Unbekannt.

*Text-to-Speech-KI:*

Das Streamen von Text-to-Speech ist nur mit Chirp 3: HD-Stimmen kompatibel. 

Chirp 3: HD-Stimmen sind jeweils in den folgenden Google Cloud -Regionen verfügbar: Auszug aus der Tabelle. Manche Features sind noch in Preview. Quelle: [https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#regional\_availability](https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#regional_availability) 16.10.26

| Google Cloud -Zone | Einführungsreife |
| --- | --- |
| `global` | GA |
| `eu` | GA |
| `europe-west2` | GA |

#### **Verarbeitungsort / Datenregion**

*Speech-to-Text:*  Auszug aus der Tabelle: 

| Google Cloud -Zone | Modell | Einführungsreife |
| --- | --- | --- |
| `us (multi-region)` | Chirp 3 | GA |
| `eu (multi-region)` | Chirp 3 | GA |
| `europe-west2` und `-west3` | Chirp 3 | Vorschau |
| `northamerica-northeast1` | Chirp 3 | Vorschau |
| `europe-west4` | Chirp 2 | GA |

Quelle: [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#regional\_availability](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-3?hl=de#regional_availability), und  [https://docs.cloud.google.com/speech-to-text/docs/models/chirp-2?hl=de#regional\_availability](https://docs.cloud.google.com/speech-to-text/docs/models/chirp-2?hl=de#regional_availability), 16.10.26 


*Translation AI:  *

“Cloud Translation Advanced bietet einen globalen Endpunkt sowie multiregionale EU- und US-Endpunkte:

- `translate.googleapis.com` (global)
- `translate-eu.googleapis.com`
- `translate-us.googleapis.com`

Wenn Sie einen multiregionalen Endpunkt verwenden, bleiben Ihre ruhenden Daten und die Verarbeitung für maschinelles Lernen innerhalb der kontinentalen Grenzen der EU oder der USA. Diese multiregionalen Endpunkte sind wichtig, wenn der Standort Ihrer Daten lokalen gesetzlichen Anforderungen genügen muss. Wenn Sie keinen Endpunkt angeben, verwendet Cloud Translation Advanced standardmäßig den globalen Endpunkt.” 

Quelle: <https://docs.cloud.google.com/translate/docs/advanced/endpoints?hl=de>, 16.10.26

*Text-to-Speech-KI: Auszug aus der Tabelle:*

| Google Cloud -Zone | Modell | Einführungsreife |
| --- | --- | --- |
| `global` | Chirp 3 | GA |
| `eu` | Chirp 3 | GA |
| `europe-west2` | Chirp 3 | GA |

Quelle: [https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#regional\_availability](https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd?hl=de#regional_availability),  16.10.26

#### **Lizenz und kommerzielle Nutzbarkeit**

**Benchmarking.** Customer may itself (but may not permit a third party to): (a) conduct benchmark tests of the Services (each a "Test"); and (b) publicly disclose the results of such Tests only if (i) the public disclosure includes all necessary information to replicate the Tests, and (ii) Customer allows Google to conduct benchmark tests of Customer's publicly available products or services and publicly disclose the results of such tests. Notwithstanding the foregoing, Customer may not do either of the following on behalf of a hyperscale public cloud provider without Google's prior written consent: (A) conduct (directly or through a third party) any Test or (B) disclose the results of any such Test.

Quelle: <https://cloud.google.com/terms/service-terms> General Service Terms, 7. 20.10.26


#### **Terminologie-/Kontextanpassung**

*Speech-to-Text:* 

*Translation AI: *

“Beide Versionenv2 und v3  unterstützen Spracherkennung und Übersetzung. Außerdem verwenden sie für das Übersetzen von Inhalten dasselbe vortrainierte Modell von Google. Cloud Translation Advanced bietet außerdem Features wie Batchanfragen, benutzerdefinierte AutoML-Modelle und Glossare. Wenn es beispielsweise Begriffe gibt, die für Ihren Anwendungsfall spezifisch sind und einheitlich übersetzt werden müssen, können Sie dafür Glossare verwenden.”

 Quelle: [https://docs.cloud.google.com/translate/docs/overview?hl=de#translation\_editions](https://docs.cloud.google.com/translate/docs/overview?hl=de#translation_editions), 16.10.26

*Text-to-Speech-KI:*

#### **Preismodell**

S2T: <https://cloud.google.com/speech-to-text/pricing?authuser=1&hl=de>

T: <https://cloud.google.com/translate/pricing?hl=de>

T2S: <https://cloud.google.com/text-to-speech/pricing?hl=de>
