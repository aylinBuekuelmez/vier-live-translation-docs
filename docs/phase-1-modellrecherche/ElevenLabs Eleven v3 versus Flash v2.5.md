# ElevenLabs Eleven v3 versus Flash v2\.5

**Kurz:** 

maximale Mehrsprachigkeit →  **Eleven v3**

- unterstützt 74 Sprachen und ist das expressivste Modell der Familie

niedriglatente Live-Telefonie → **Flash v2.5** 

- spricht 32 Sprachen, liefert Latenz unter 75 (laut elevenlabs) 


Stand der Recherche: 26.10.2026

| Kriterium | Eleven v3 | Flash v2.5 |
| --- | --- | --- |
| **Modell** | `eleven_v3` | `eleven_flash_v2_5` |
| **Architektur** | Cascadierter TTS-Baustein: Text → Audio. | Cascadierter TTS-Baustein: Text → Audio. |
| **Eignung für Live Translation** | Sehr gut, wenn Sprachabdeckung und Ausdrucksstärke wichtiger sind als minimale Latenz. | Sehr gut für Live-Telefonie und Voice Agents mit vielen Sprachen. |
| **Realtime-/Streaming-Fähigkeit** | Ja, aber weniger auf ultra-niedrige Latenz optimiert. | Ja; Speech-Generierung in unter 75 ms. |
| **Sprachunterstützung** | **74 Sprachen**, inklusive Deutsch, Englisch, Französisch, Spanisch, Arabisch, Hindi, Japanisch, Koreanisch, Portugiesisch, Russisch und vielen weiteren. | **32 Sprachen**, inklusive Deutsch, Englisch, Französisch, Spanisch, Arabisch, Hindi, Japanisch, Koreanisch, Portugiesisch, Russisch und weiteren. |
| **Mehrsprachigkeit** | Beste ElevenLabs-Wahl, wenn ihr möglichst viele Zielsprachen abdecken wollt. | Guter Kompromiss: deutlich mehr Sprachen als ältere Flash-Modelle, bei sehr niedriger Latenz. |
| **Telefonie-/Audio-Kompatibilität** | Streaming-Audio; Codec-, Sample-Rate- und Telefon-Integration im Orchestrator prüfen. | Streaming-Audio; für Telefonie besonders interessant wegen niedriger Latenz. |
| **Technische Evaluierbarkeit** | Sehr gut: TTFB, MOS/UTMOS, Stabilität, Aussprachetests, Hörtest. | Sehr gut: TTFB, MOS/UTMOS, Stabilität, Hörtest, Aussprachetests. |
| **Produktreife / Status** | Aktuellste, ausdrucksstärkste ElevenLabs-TTS-Generation. | Aktuelle Low-Latency-Generation für Entwickler- und Echtzeit-Fälle. |
| **Verarbeitungsort / Datenregion** | Cloud-first; regionale Datenresidenz und Zero Retention Mode als Enterprise-Optionen. | Cloud-first; regionale Datenresidenz und Zero Retention Mode als Enterprise-Optionen. |
| **Datennutzung** | Keine Nutzung für Modelltraining laut ElevenLabs; ZRM per `enable_logging=false`. | Keine Nutzung für Modelltraining laut ElevenLabs; ZRM per `enable_logging=false`. |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle API-Lizenz; kein Open Source. | Kommerzielle API-Lizenz; kein Open Source. |
| **Terminologie-/Kontextanpassung** | Stark bei Emotion, Pacing und natürlichem Kontext; Fachbegriffe über Textnormalisierung bzw. phonetische Schreibweise im Orchestrator. | Gleiches Prinzip; Fokus auf Geschwindigkeit und Mehrsprachigkeit. |
| **Preismodell** | Pro Zeichen; Multilingual ca. 0,08 USD / 1.000 Zeichen. | Pro Zeichen; ca. 0,04 USD / 1.000 Zeichen, also 0,5 Credit pro Zeichen. |

# Eleven v3

ElevenLabs’ vorherige Flaggschiff-TTS-Generation für besonders natürliche, ausdrucksstarke Sprache. Es ist auf emotionale Nuancen, Kontextverständnis und mehrsprachige Dialoge ausgelegt und unterstützt laut Help-Center **74 Sprachen**; die aktuelle Modell-Doku nennt „70+ languages“.

stark in: **Sprachabdeckung, natürliche Ausdrucksstärke**

nicht so: minimale Latenz

→  kein Echtzeit-First-Modell, mehr für hochwertige Dialoge, Medien und mehrsprachige Inhalte


## Technische Eigenschaften

| Eigenschaft | Eleven v3 |
| --- | --- |
| **Primärer Zweck** | Ausdrucksstarke, natürliche mehrsprachige Sprache. |
| **Sprachen** | 74 laut Help Center, „70+“ laut aktueller Modell-Doku. |
| **Maximale Textlänge** | 5.000 Zeichen pro Request, ungefähr 5 Minuten Audio. |
| **Multi-Speaker-Dialog** | Ja, über Text-to-Dialogue. |
| **Emotionale Steuerung** | Stark; Modell reagiert auf Textkontext und Audio Tags. |
| **Echtzeit-Eignung** | Möglich, aber nicht der Hauptfokus; ElevenLabs empfiehlt für Echtzeit eher Flash-Modelle |
| **Kosten** | 1 Credit pro Zeichen, ohne Credit-Multiplikatoren. |

Eleven v3, wenn

- viele Zielsprachen anbieten
- hohe Natürlichkeit und emotionale Ausdrucksstärke
- Dialoge mit mehreren Sprechern erzeugen
- eher für **Nachbearbeitung, aufgezeichnete Anrufe oder hochwertige Audioausgabe**

(Für ein Live-Telefongespräch ist es aber nicht die erste Wahl, weil ElevenLabs selbst für Echtzeit- und Agent-Fälle die Flash-Modelle empfiehlt. Die 5.000-Zeichen-Grenze ist für einzelne Übersetzungssegmente kein Problem, aber für lange Texte müsste chunking eingesetzz werden.[elevenlabs](https://elevenlabs.io/docs/capabilities/text-to-speech))

## Sprachabdeckung

Eleven v3 unterstützt 74 Sprachen, darunter:

Afrikaans, Arabisch, Bengalisch, Chinesisch (Mandarin), Dänisch,  Deutsch, Englisch, Finnisch, Französisch, Griechisch, Hebräisch, Hindi, Indonesisch, Italienisch, Japanisch, Koreanisch, Niederländisch, Norwegisch, Polnisch, Portugiesisch, Russisch, Schwedisch, Somali, Spanisch, Swahili, Tamil, Telugu, Tschechisch, Türkisch, Ukrainisch, Ungarisch, Vietnamesisch, Yoruba


**Referenzen:**

- ElevenLabs-Modelle: <https://elevenlabs.io/docs/overview/intro> 
- Eleven v3 und Flash v2.5 Sprachen: <https://help.elevenlabs.io/hc/en-us/articles/17883183930129-What-models-do-you-offer-and-what-is-the-difference-between-them> 
- TTS-Produktübersicht: <https://start.elevenlabs.io/text-to-speech/text-to-speech-v4> 
- Zero Retention Mode: <https://elevenlabs.io/docs/eleven-api/resources/zero-retention-mode> 
- Preise: <https://elevenlabs.io/pricing/api> 


# Flash v2.5

ElevenLabs’ schnelles, kostengünstiges TTS-Modell für Echtzeit-Anwendungen:

- niedrige Latenz
- skalierbare API-Nutzung
- konsistente Stimmen über mehrere Sprachen
- für Voice Agents sowie interaktive Anwendungen optimiert

ElevenLabs nennt eine Modell-Inferenz-Latenz von etwa **75 ms**; Netzwerk- und Applikationslatenz kommen davon unabhängig hinzu.


## Sprachabdeckung

Flash v2.5 unterstützt 32 Sprachen, darunter:

Arabisch, Bulgarisch, Chinesisch (Mandarin), Dänisch, Deutsch, Englisch, Filipino, Finnisch, Französisch, Griechisch, Hindi, Indonesisch, Italienisch, Japanisch, Koreanisch, Kroatisch, Malay, Niederländisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, Schwedisch, Slowakisch, Spanisch, Tamil, Tschechisch, Türkisch, Ukrainisch, Ungarisch, Vietnamesisch


## Technische Eigenschaften

| Eigenschaft | Flash v2.5 |
| --- | --- |
| **Primärer Zweck** | Echtzeit-TTS für Agents, interaktive Apps und große Mengen Text. |
| **Sprachen** | 32. |
| **Latenz** | Ca. 75 ms Modell-Inferenz; ohne Netzwerk-/App-Latenz. |
| **Maximale Textlänge** | 40.000 Zeichen pro Request, ungefähr 40 Minuten Audio. |
| **Kosten** | 0,5 Credit pro Zeichen, ohne Credit-Multiplikatoren. |
| **Textnormalisierung** | Standardmäßig deaktiviert, um Latenz zu sparen; Enterprise kann `apply_text_normalization="on"` setzen. |
| **Audio-Formate** | MP3, PCM, mu-law, A-law, Opus. |
| **Telefonie** | mu-law und A-law mit 8 kHz sind explizit für Telefonie optimiert. |

## Testing


- niedrige End-to-End-Latenz 
- viele parallele Gespräche 
- viele Sprachen, nicht unbedingt alle 74 Eleven-v3-Sprachen
- Kosten pro Zeichen
- Telefon-Audio direkt in mu-law oder A-law ausgeben

### Zahlen und Normalisierung

Flash v2.5 normalisiert Zahlen standardmäßig **nicht** → Telefonnummern, Daten und Währungen können unklar ausgesprochen sein --\>

transkript in klare sprechbare Form bringen, e.g. Deepgram Nova-3  „0176 12345678“ korrekt transkribiert - bevor an Flash v2.5:

```
text
```

`„null eins sieben sechs, eins zwei drei vier …“`

ODER

Enterprise `apply_text_normalization="on"` aktivieren ABER eigener Orchestrator - eigene Normalisierung für Zahlen, Daten, Währungen und IDs --\> vermutlich zuverlässiger.

## Referenzen

- Modellvergleich und 32 Sprachen: <https://help.elevenlabs.io/hc/en-us/articles/17883183930129-What-models-do-you-offer-and-what-is-the-difference-between-them> 
- Aktuelle Modell-Doku: <https://elevenlabs.io/docs/overview/models> 
- TTS-Doku, Audio-Formate und Telefonie: <https://elevenlabs.io/docs/capabilities/text-to-speech>
