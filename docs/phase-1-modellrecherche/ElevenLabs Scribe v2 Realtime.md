# ElevenLabs Scribe v2 Realtime

Scribe ist ElevenLabs’ STT-Linie mit Batch- und Realtime-Transkription. Scribe v2 Realtime ist ein ernstzunehmender Vergleichskandidat zu Deepgram Nova-3 und Flux, besonders wegen Word-Timestamps, Diarization und Keyterm Prompting.

Stand der Recherche: 26.10.2026  

| Kriterium | Scribe v2 Realtime |
| --- | --- |
| **Modell** | `scribe_v2_realtime` |
| **Architektur** | Cascadierter STT-Baustein: Audio → Text; WebSocket-Streaming |
| **Eignung für Live Translation** | Sehr gut als STT-Eingangsstufe; MT und TTS extern |
| **Realtime-/Streaming-Fähigkeit** | Ja; WebSocket, Partial/Committed Transcripts, ca. 150 ms Latenz |
| **Sprachunterstützung** | 90+ Sprachen, inkl. DE/EN |
| **Bidirektionalität** | Nicht inhärent; Richtung wird durch externe MT bestimmt |
| **Telefonie-/Audio-Kompatibilität** | WebSocket-Streaming; AEC/VAD/Resampling/Codec-Mapping im Orchestrator |
| **Technische Evaluierbarkeit** | WER/CER, Word-Timestamps, Entity Detection, Transcript Editing |
| **Produktreife / Status** | Produktionsorientierte Echtzeit-STT-Linie |
| **Verarbeitungsort / Datenregion** | Cloud-first; regionale Residenz und ZRM als Enterprise-Optionen |
| **Datennutzung** | Kein Training mit Kundendaten; ZRM per `enable_logging=false` |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle API-Lizenz; kein Open Source |
| **Terminologie-/Kontextanpassung** | Keyterm Prompting: max. 50 Begriffe, max. 20 Zeichen |
| **Preismodell** | Ca. 0,39 USD pro Audio-Stunde; Keyterms ca. 0,05 USD/h |

## Technische Aspekte

- 90+ Sprachen
- Echtzeit-Transkription über WebSocket
- ca. 150 ms Latenz
- Word-Level-Timestamps
- Entity Detection mit 65 Entity-Typen
- Transcript Editing mit natürlichsprachlichen Anweisungen
- Keyterm Prompting mit bis zu 50 Begriffen à 20 Zeichen
- No-Verbatim-Modus


Scribe v2 Realtime - liefert **Partial Transcripts** während des Sprechens und **Committed Transcripts**, wenn ein Abschnitt stabil genug ist. Für die Übersetzungskette: idealerweise erst Committed Segmente an MT und TTS weitergeben 

```
text
```

`Live-/WAV-Audio ↓ Scribe v2 Realtime ↓ Committed Transcript ↓ DeepL / Google Translate / Azure Translator ↓ ElevenLabs Flash v2.5 oder Eleven v3 ↓ Telefon-Audio`


## Testing:

- **WER/CER** gegen manuell korrigierte Referenztranskripte, mit jiwer.
- **Latenz**: Zeit zwischen Audio-Chunk und erstem Partial bzw. Committed Transcript.
- **Stabilität**: Wie oft ändern sich Partial Transcripts, bevor sie committed werden?
- **Entity-Treffer**: Zahlen, Namen, Daten, Beträge und weitere Entitäten separat auswerten.
- **Keyterm-Treffer**: Ob Glossarbegriffe, Produktnamen oder Eigennamen korrekt erkannt werden.


## Entity Detection

Scribe v2 Realtime kann Entitäten mit exakten Zeitstempeln erkennen → relevant für kritischen Inhaltstests: Namen, Kreditkartennummern, medizinische Angaben oder Sozialversicherungsnummern.

Entitäten direkt aus dem STT-Output extrahieren und mit der MT-Ausgabe abgleichen, statt ausschließlich eigene NER-Regeln auf den Text anzuwenden.  

## Transcript Editing

Natürlichsprachliche Anweisung mitgeben, z.B. „Normalisiere Daten und Uhrzeiten“ oder „Schreibe Maßeinheiten aus“ → Modell gibt neben Original-Transkript ein `edited_transcript` zurück, Zeitstempel und Sprecherlabels bleiben erhalten.

--\> experimentell, aber in Tests: prüfen, ob automatische Nachbearbeitung die Übersetzungsqualität verbessert — zum Beispiel bei Daten, Uhrzeiten, Maßeinheiten oder Zahlenformaten.


## No-Verbatim-Modus

 `no_verbatim`

-  entfernt das Modell Füllwörter, 
- falsche Satzanfänge und 
- Disfluencies

→ saubereren Text für Untertitel, Zusammenfassungen oder Weiterverarbeitung.

! ev. nicht für Telefonat geeignet: Für die Übersetzung kann „ähm“, eine Korrektur oder ein abgebrochener Satzanfang relevant sein → A versus B Test, jeweils mit COMET/chrF und menschlicher Stichprobe.


## Sprachqualität

ElevenLabs ordnet die Sprachen nach WER-Bereichen ein:

- **Exzellent, WER ≤ 5%:** Deutsch, Englisch, Französisch, Spanisch, Italienisch, Portugiesisch, Niederländisch, Polnisch, Russisch, Ukrainisch, Türkisch, Schwedisch, Dänisch, Finnisch, Griechisch, Ungarisch, Japanisch, Indonesisch und weitere.
- **Hohe Genauigkeit, 5–10% WER:** Hindi, Mandarin, Kantonesisch, Filipino, Tamil, Telugu, Bengali, Persisch, Serbisch, Slowenisch und weitere
- **Gut, 10–20% WER:** Arabisch, Koreanisch, Thai, Urdu, Walisisch und weitere.

! ElevenLabs-eigene Werte → für Telefonqualität, Rauschen und konkreten Sprecher mit eigenen Testdaten validieren.


## Referenzen

- **Scribe v2 Realtime — offizielle Feature-Doku:**  
[https://elevenlabs.io/docs/capabilities/speech-to-textelevenlabs](https://elevenlabs.io/docs/capabilities/speech-to-text)
- **Scribe-Produktseite:**  
<https://elevenlabs.io/speech-to-text> 
- **Scribe v2 Realtime Produktseite:**  
<https://elevenlabs.io/realtime-speech-to-text-api> 
- **ElevenLabs-Modellübersicht:**  
<https://elevenlabs.io/docs/overview/intro> 
- **Zero Retention Mode:**  
<https://elevenlabs.io/docs/eleven-api/resources/zero-retention-mode> 
- **Preise:**  
<https://elevenlabs.io/pricing/api> 
- **Python SDK:**  
<https://github.com/elevenlabs/elevenlabs-python> 

## Wichtigster Unterschied zu Scribe v2 Batch

| Feature | Scribe v2 Batch | Scribe v2 Realtime |
| --- | --- | --- |
| 90+ Sprachen | Ja | Ja |
| Word-Level-Timestamps | Ja | Ja |
| Entity Detection | Ja | Ja |
| Keyterm Prompting | Bis 1.000 Begriffe, 50 Zeichen | Bis 50 Begriffe, 20 Zeichen |
| Speaker Diarization | Ja, bis 32 Sprecher | Nein |
| Multichannel | Ja, bis 5 Kanäle | Nein |
| Transcript Editing | Ja | Ja |
| Live-Streaming | Nein | Ja |
