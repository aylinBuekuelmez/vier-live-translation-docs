# OpenAI GPT\-Realtime\-Translate

**Recherche-Stand: **19.10.2026  
**Bezugsform**: Cloudbasierter API-Dienst

**Dokumentation:** <https://developers.openai.com/api/docs> 

**Überblick: **

#### **Eignung für Live-Translation**

“Realtime translation: Translate live speech with streaming audio and transcript output.  Realtime translation lets you stream source audio into a dedicated translation session and receive translated audio plus transcript deltas while the speaker is still talking. Use it for live interpretation, multilingual calls, broadcasts, meetings, lessons, and video rooms.  Use gpt-realtime-translate when your application should translate what a human says. If you need an assistant that answers questions, calls tools, and manages a conversation, use gpt-realtime-2.1 with a standard Realtime session instead.” Quelle: <https://developers.openai.com/api/docs/guides/realtime-translation>, 17.10.26

“`gpt-realtime-translate` is a live speech-to-speech translation model for building multilingual audio experiences across broadcasts, streams, calls, and video conversations. It accepts spoken input, automatically detects the source language, and returns translated speech plus text transcripts. Developers only need to specify the target output language.” Quelle: [https://developers.openai.com/cookbook/examples/voice\_solutions/realtime\_translation\_guide](https://developers.openai.com/cookbook/examples/voice_solutions/realtime_translation_guide), 17.10.26

#### **Architektur**

Speech-to-Speech: “GPT-Realtime-Translate is a streaming speech-to-speech translation model for live multilingual audio experiences. It uses a dedicated realtime translation endpoint and returns translated audio plus transcript deltas while source audio is still arriving. GPT-Realtime-Translate is priced by audio duration rather than text tokens.” Quelle: <https://developers.openai.com/api/docs/models/gpt-realtime-translate>, 17.10.26

#### **Realtime-/Streaming-Fähingkeit**

**“Continuous audio in**: Stream 24 kHz PCM16 audio with `session.input_audio_buffer.append`, including silence between phrases.” Quelle: [https://developers.openai.com/cookbook/examples/voice\_solutions/realtime\_translation\_guide#session-lifecycle](https://developers.openai.com/cookbook/examples/voice_solutions/realtime_translation_guide#session-lifecycle) 17.10.26

Auszug Tabelle: 

| Translation session |
| --- |
| Connects to `/v1/realtime/translations`. |
| The model acts as an interpreter. |
| Streams continuously from incoming audio. |
| Produces translated audio and transcript deltas. |

Translation starts from the audio stream itself. Keep appending audio, including silence between phrases, and handle output events as they arrive. Quelle: <https://developers.openai.com/api/docs/guides/realtime-translation#how-translation-sessions-differ>, 17.10.26

#### **Sprachunterstützung und Bidirektionalität**

“Realtime Translation currently supports 13 target output languages: Spanish, Portuguese, French, Japanese, Russian, Chinese, German, Korean, Hindi, Indonesian, Vietnamese, Italian, and English.

The model currently supports over 70 input languages. It can dynamically detect and translate from Arabic, Afrikaans, Azerbaijani, Belarusian, Bengali, Bosnian, Bulgarian, Catalan, Chinese, Croatian, Czech, Danish, Dutch, Dzongkha, English, Esperanto, Estonian, Basque, Persian / Farsi, Finnish, Filipino, French, Galician, German, Greek, Gujarati, Haitian Creole, Hawaiian, Hebrew, Hindi, Hungarian, Armenian, Indonesian, Italian, Japanese, Javanese, Georgian, Kazakh, Korean, Kurdish, Latin, Latvian, Lithuanian, Macedonian, Malay, Malayalam, Maori, Mongolian, Burmese / Myanmar, Nepali, Norwegian, Nynorsk, Polish, Portuguese, Punjabi, Romanian, Russian, Serbian, Shona, Slovak, Slovenian, Albanian, Spanish, Swahili, Swedish, Tagalog, Telugu, Thai, Turkish, Ukrainian, Uzbek, Vietnamese, Welsh, and Yoruba.”

Quelle: [https://developers.openai.com/cookbook/examples/voice\_solutions/realtime\_translation\_guide#supported-languages](https://developers.openai.com/cookbook/examples/voice_solutions/realtime_translation_guide#supported-languages), 17.20.26

#### **Telefonie-/Audio-Kompatibilität**

“Phone calls with Twilio

1. A caller joins and says their preferred output language.
2. Twilio streams that caller’s audio to your backend.
3. Your backend converts Twilio audio into the audio format expected by Realtime Translation.
4. For each translation direction, your backend opens a Realtime Translation session with the listener’s target language.
5. As translated audio comes back from OpenAI, your backend converts it back into Twilio media messages.
6. Twilio plays the translated audio to the other participant.” Quelle: [https://developers.openai.com/cookbook/examples/voice\_solutions/realtime\_translation\_guide](https://developers.openai.com/cookbook/examples/voice_solutions/realtime_translation_guide#how-it-works-1), 17.10.26

Twilio sends base64 `audio/x-mulaw` at 8 kHz. Realtime Translation expects base64 little-endian PCM16 at 24 kHz, so the bridge decodes u-law, resamples to 24 kHz, and appends the audio buffer. Keep sending audio continuously, including silence, since translation sessions are not turn-based and do not wait for `response.create`. Realtime Translation emits translated audio as base64 24 kHz PCM16 in `session.output_audio.delta`. Before sending it to Twilio, convert it back to 8 kHz u-law and wrap it in a Twilio media message. Quelle: [https://developers.openai.com/cookbook/examples/voice\_solutions/realtime\_translation\_guide#bridge-twilio-audio-into-realtime-translation](https://developers.openai.com/cookbook/examples/voice_solutions/realtime_translation_guide#bridge-twilio-audio-into-realtime-translation), 17.10.26

#### **Technische Evaluierbarkeit **

Der Zugang erfolgt über die API mit WebRTC oder WebSocket und über den Playground. Kein Self-Hosting. Free-Tier nicht unterstützt, Build-Tier mit 200 Audio-Minuten pro Minute. Preis 0,034 $ pro Minute. Quellen: <https://developers.openai.com/api/docs/models/gpt-realtime-translate>, <https://developers.openai.com/api/docs/guides/realtime-translation#test-quality-and-latency>, 17.10.26

#### **Produktreife / Entwicklungsstatus**

keine Infos

#### **Verarbeitungsort / Datenregion**

“Auf Anfrage und mit extra Kosten: “OpenAI uses sub-processors to provide its services. For requests sent to `us.api.openai.com` or `eu.api.openai.com`, OpenAI uses Cloudflare Regional Services so that TLS termination and HTTPS decryption occur within the selected processing region.” Quelle: <https://developers.openai.com/api/docs/guides/your-data#sub-processors-and-regional-request-processing>, 17.10.26

“Data residency controls: Data residency controls are a project configuration option that allow you to configure the location of infrastructure OpenAI uses to provide services. Contact our sales team to see if you’re eligible for using data residency controls. Data residency endpoints are charged a 10% uplift for models released on or after March 5, 2026, that are eligible for data residency.” Quelle: <https://developers.openai.com/api/docs/guides/your-data#data-residency-controls>, 17.10.26

#### **Lizenz und kommerzielle Nutzbarkeit**

“Wir halten es für wichtig, unsere Produkte und Forschungsarbeit einer eingehenden Prüfung durch die breite Öffentlichkeit zu unterziehen, damit wir mögliche Schwächen, Sicherheitsprobleme und Verzerrungen in unseren Modellen besser erkennen und beheben können. Daher laden wir aktiv zur Veröffentlichung von Forschungsarbeiten über die OpenAI API ein.”

Quelle: <https://openai.com/de-DE/policies/sharing-publication-policy/>, 20.10.26

#### **Terminologie-/Kontextanpassung**

“The model does not currently support custom prompts, glossaries, or pronunciation guides. If your use case depends on specific vocabulary, names, legal or medical terms, or other domain language, test those terms directly. The model can sometimes substitute incorrect names or entities while translating, so include these cases in your launch evaluation set.”

Quelle: [https://developers.openai.com/cookbook/examples/voice\_solutions/realtime\_translation\_guide#test-terminology-and-names-directly](https://developers.openai.com/cookbook/examples/voice_solutions/realtime_translation_guide#test-terminology-and-names-directly), 17.10.26

**Preismodell**

Prices per 1M tokens unless noted.

| Model | Modality | Input | Output / cost | Cached input |
| --- | --- | --- | --- | --- |
| gpt-realtime-2.1 | Audio | $32.00 | $64.00 | $0.40 |
| Text | $4.00 | $24.00 | $0.40 |  |
| Image | $5.00 | - | $0.50 |  |
| gpt-realtime-2.1-mini | Audio | $10.00 | $20.00 | $0.30 |
| Text | $0.60 | $2.40 | $0.06 |  |
| Image | $0.80 | - | $0.08 |  |

Quelle: <https://developers.openai.com/api/docs/pricing?latest-pricing=batch&latest-pricing-expanded=true#audio-tokens>, 20.10.26

0.0034 $ per minute → <https://developers.openai.com/api/docs/models/gpt-realtime-translate>, 18.10.26
