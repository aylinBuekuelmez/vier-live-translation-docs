# DeepL Translation API v3

Stand der Recherche: 26.10.2026


Es ist ein reiner Text-zu-Text-Übersetzungsdienst, unterstützt DE↔EN vollständig und bietet Glossare sowie Formality — genau die Funktionen, die ihr für Terminologie- und Kontexttests braucht.developers.

| Kriterium | DeepL API |
| --- | --- |
| **Modell** | DeepL Translation API; aktuelle API-Version v3, v2 ist deprecated. |
| **Architektur** | Reiner MT-Baustein: Text → Text; kein STT, kein TTS. |
| **Eignung für Live Translation** | Sehr gut als Mittelstufe; für Live-Übersetzung segmentweise aufrufen, z. B. nach `speech_final` bzw. Committed Transcript. |
| **Realtime-/Streaming-Fähigkeit** | Kein natives WebSocket-Streaming; REST-basiert, pro Segment/Chunk aufrufen. |
| **Sprachunterstützung** | Deutsch und Englisch vollständig; weitere Sprachen über die Languages API abrufbar. |
| **Bidirektionalität** | Ja |
| **Telefonie-/Audio-Kompatibilität** | Indirekt: DeepL verarbeitet nur Text; Audio muss vorher durch STT und danach durch TTS. |
| **Technische Evaluierbarkeit** | Sehr gut: COMET/chrF gegen Referenzübersetzungen, Glossar-Trefferquote, Latenz pro Segment, Formality-Tests. |
| **Produktreife / Status** | Sehr reif; etablierter kommerzieller MT-Anbieter mit dokumentierter REST-API. |
| **Verarbeitungsort / Datenregion** | Cloud-Dienst; für EU-/DSGVO-Anforderungen Vertrag, Datenverarbeitung und Region mit DeepL klären. |
| **Datennutzung** | DeepL API Pro: Texte werden laut DeepL unmittelbar nach der Übersetzung gelöscht; Free API hat ein 500k-Zeichen-Monatslimit. |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle SaaS-API; kein Open Source, kein Self-Hosting der MT-Modelle. |
| **Terminologie-/Kontextanpassung** | Glossare für DE↔EN; Formality für deutsche Zieltexte; Tag Handling für strukturierten Text. developers.deepl+1 |
| **Schnittstellen / Kompatibilität** | REST API; offizielle SDKs u. a. für Python, .NET, Java, PHP, Ruby, Go; einfache Integration in Python-Runner. |
| **Preismodell** | Free: 500.000 Zeichen/Monat; Pro: monatliche Grundgebühr plus nutzungsbasierte Kosten pro Zeichen; Cost Control möglich. |

Technische Aspekte:

**Terminologie verlässlich steuerbar: **Glossar mit Begriffen wie „Rechnung → invoice“, „Vorgangsnummer → case ID“ oder Produktnamen pflegen; DeepL verwendet diese Zuordnungen dann konsistent

**Formality** steuern - für deutsche Zieltexte, z.B. etwa förmliches „Sie“ statt informellem „du“

**Kein Streaming-MT-Dienst - ** nicht das komplette Gespräch, sondern stabile Segmente — etwa nach Endpointing von Nova-3, Committed Transcript von Scribe oder erkanntem Satzende.


## Referenzen

- Sprachen und Features: <https://developers.deepl.com/docs/languages/using-the-languages-api> 
- Unterstützte Sprachen: <https://developers.deepl.com/docs/getting-started/supported-languages> 
- Glossare: [https://support.deepl.com/hc/en-us/articles/360021634540-About-the-glossarysupport.deepl](https://support.deepl.com/hc/en-us/articles/360021634540-About-the-glossary)
- Glossar-Sprachpaare: [https://developers.deepl.com/api-reference/multilingual-glossaries/list-language-pairs-supported-by-glossariesdevelopers.deepl](https://developers.deepl.com/api-reference/multilingual-glossaries/list-language-pairs-supported-by-glossaries)
- Usage/Limits: <https://developers.deepl.com/docs/resources/usage-limits> 
- API-Pläne: <https://support.deepl.com/hc/en-us/articles/360021200939-DeepL-API-plans> 
- Cost Control: <https://support.deepl.com/hc/en-us/articles/360020685580-API-usage-and-cost-control> 
- Python SDK: <https://github.com/DeepL/deepl-python>
