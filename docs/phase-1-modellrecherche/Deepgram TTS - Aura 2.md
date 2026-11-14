# Deepgram TTS \- Aura 2

About Deepgram: Deepgram offers both, STT-TTS, and Voice Agent (for this, see other column in table, and resp. document. 


### Testing:

offers free credit of $200 to test: to run a standard voice agent for more than 40 hours. No credit card needed. apprently for both , STT, TTS, Voice Agent

(Deepgram website: <https://console.deepgram.com/signup> , 16.10.2026)


- single, unified conversational AI API 
- ready for building in own LLM or TTS


Für die Transparenz: es gibt auch Flux TTS, aber das war bei Weitem weniger geeignet als TTS.

# AURA 2

**Aura-2** (Endpunkt `/v1/speak`) ist Deepgrams Modell mit den meisten Sprachen, mit Stimmen in Englisch, Spanisch, Deutsch, Französisch, Niederländisch, Italienisch und Japanisch. Spanisch kam im Juni 2025 mit zehn Stimmen, die fünf weiteren Sprachen folgten im Dezember 2025. Dazu gibt es regionale Akzente: Englisch in amerikanisch, britisch, australisch und philippinisch, Spanisch in mexikanischer, kastilischer, kolumbianischer, argentinischer und lateinamerikanischer Variante. Ausgewählte spanische Stimmen beherrschen Englisch-Spanisch-Codeswitching. Deepgram sagt selbst, Aura-2 nähere sich schrittweise einer globalen Abdeckung, nennt aber keine weiteren Sprachen. Ein Preis laut OpenRouter-Listing (nicht Deepgram selbst): 30 Dollar pro Million Zeichen. [Models & Languages Overview +6](https://developers.deepgram.com/docs/tts-models-languages-overview)


| Kriterium | **Deepgram Aura-2 TTS** |
| --- | --- |
| **Technische Evaluierbarkeit** | Sehr gut: Streaming über WebSocket sowie Batch über REST; Audio-Ausgabe als PCM, mu-law oder A-law; für Batch zusätzlich MP3, Opus, FLAC und AAC. Ihr könnt Latenz, Time-to-First-Byte, Verständlichkeit und MOS mit eurem Runner messen. [anchorterminal](https://www.anchorterminal.com/tools/deepgram-tts) |
| **Produktreife** | Hoch: Aura-2 ist Deepgrams breiteste TTS-Familie, unterstützt mehrere Sprachen und wird für Enterprise-Voice-Applikationen positioniert. deepgram+1 |
| **Verarbeitungsort** | Deepgram Cloud (global oder regional) oder Self-Hosted: Docker/Podman, Kubernetes, SageMaker, Dedicated Environment oder Bare Metal. [aws.amazon](https://aws.amazon.com/marketplace/pp/prodview-53fbbb6iqhkim) |
| **Datennutzung** | Standardmäßig nimmt die Anfrage am Model Improvement Program teil; mit `mip_opt_out=true` gilt Zero Retention für Audio, Text, Transkripte und synthetisiertes Audio nach der Antwort. developers.deepgram+1 |
| **Lizenz** | Kommerzielle Deepgram-Lizenz, kein Open Source; Self-Hosting erfordert Deepgram-Lizenzierung. |
| **Terminologie-Anpassung** | Aussprache einzelner Wörter kann per IPA-Notation überschrieben werden; Aura-2 unterstützt zudem entity-aware Textnormalisierung für Adressen, Telefonnummern und Kontonummern. developers.deepgram+1 |
| **Kontextanpassung** | Aussprache-Overrides benötigen Satzkontext, besonders bei Homonymen; Deepgram empfiehlt, diese im LLM/Orchestrator zu setzen, weil dort der Gesprächskontext vorliegt. [developers.deepgram](https://developers.deepgram.com/docs/voice-agent-tts-controls) |
| **Preismodell** | Cloud: nutzungsbasiert, ca. 0,030 USD pro 1.000 Zeichen; Self-Hosted: Lizenz plus Infrastruktur, bei AWS z. B. Stundenpreis für Inferenz-Host plus Preis pro Request. aws.amazon+1 |

**Aura-2** ist als breite Enterprise-TTS-Familie positioniert und bietet Streaming über WebSocket sowie Batch über REST; für Telefonie kann es PCM, mu-law und A-law ausgeben.

**Aura-2 TTS unterstützt aktuell 7 Sprachen** (bzw. 15 Locales): Englisch, Spanisch, Deutsch, Französisch, Niederländisch, Italienisch und Japanisch. Für euer DE↔EN-Szenario sind damit beide benötigten Sprachen abgedeckt.\[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\]\[[deepgram](https://deepgram.com/learn/best-text-to-speech-apis-2026)\]

| Sprache | Locales / Akzente |
| --- | --- |
| Englisch | 5 Locales: US, UK, Australien, Irland, Philippinen \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |
| Spanisch | 5 Locales, u. a. Mexiko, Spanien, Kolumbien, Lateinamerika \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |
| Deutsch | Deutschland \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |
| Französisch | Frankreich \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |
| Niederländisch | Niederlande \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |
| Italienisch | Italien \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |
| Japanisch | Japan \[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\] |

Insgesamt werden je nach Quelle etwa **88–102 Voices** genannt; die Anzahl kann sich durch neue Voices ändern. Deutsch hat dabei etwa 7 Voices.\[[docs.cognigy](https://docs.cognigy.com/voice-gateway/references/tts-and-stt-vendors)\]\[[api-docs.ollang](https://api-docs.ollang.com/apis/tts-apis/deepgram-aura)\]


**Testing:**  
Aura-2 mit deutschen und englischen Voices (Empfehlung), WebSocket-Streaming, mu-law/A-law-Ausgabe für Telefon-Simulation; TTFB, UTMOS, Aussprache von Glossarbegriffen und Verständlichkeit bei Telefonqualität messen.

Besonders relevant ist die **Aussprachesteuerung**: Aura-2 erlaubt Overrides per IPA-Notation, etwa für Produktnamen, Firmennamen oder Fachbegriffe. Zusätzlich unterstützt es entity-aware Textnormalisierung für Adressen, Telefonnummern und Kontonummern → Terminologie-Tests


**Referenzen:**

- Aura-2 Voices/Languages: <https://developers.deepgram.com/docs/tts-models> 
- Aura-2 Sprachausbau: <https://deepgram.com/learn/aura-2-now-speaks-dutch-french-german-italian-japanese> 
- Audio-Streaming: <https://developers.deepgram.com/docs/streaming-the-audio-output> 
- Datenutzung: <https://developers.deepgram.com/trust-security/your-data> 
- Self-Hosting: <https://developers.deepgram.com/docs/self-hosted-deployment-environments>
