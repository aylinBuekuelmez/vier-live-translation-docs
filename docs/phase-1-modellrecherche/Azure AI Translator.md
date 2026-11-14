# Azure AI Translator

Stand der Recherche: 26.10.2026

**Wenn bereits Azure in Nutzung ODER EU-/Enterprise-Anforderungen im Vordergrund **

Es ist ein Text-zu-Text-MT-Dienst mit REST-API, Glossaren, Custom Translator und breiter Sprachabdeckung.

| Kriterium | Azure AI Translator |
| --- | --- |
| **Modell** | Azure AI Translator; Custom Translator als Erweiterung. |
| **Architektur** | Reiner MT-Baustein: Text → Text; kein STT, kein TTS. |
| **Eignung für Live Translation** | Sehr gut als Mittelstufe; segmentweise REST-Aufrufe zwischen STT und TTS. |
| **Realtime-/Streaming-Fähigkeit** | Kein natives MT-Streaming; Text-Segmente werden einzeln übersetzt. |
| **Sprachunterstützung** | Breite Sprachabdeckung, inklusive DE/EN und vielen weiteren Sprachen. |
| **Bidirektionalität** | Ja, für unterstützte Sprachpaare; DE↔EN wird unterstützt. |
| **Telefonie-/Audio-Kompatibilität** | Indirekt: nur Text; Audio muss vorher durch STT und danach durch TTS. |
| **Technische Evaluierbarkeit** | Sehr gut: COMET/chrF, Glossar-Trefferquote, Latenz, Custom-Translator-Vergleiche. |
| **Produktreife / Status** | Sehr reif; etablierter Enterprise-MT-Dienst. |
| **Verarbeitungsort / Datenregion** | Azure-Regionen; für EU-/DSGVO-Anforderungen Region, Data Residency und DPA prüfen. |
| **Datennutzung** | Azure-Datenschutz- und Vertragsbedingungen maßgeblich; für sensible Gespräche DPA, Retention und Region vertraglich klären. |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle Cloud-API; kein Open Source, kein Self-Hosting der Standardmodelle. |
| **Terminologie-/Kontextanpassung** | Glossare und Custom Translator für domänenspezifische Terminologie. |
| **Schnittstellen / Kompatibilität** | REST API; SDKs für gängige Sprachen; gute Integration in Azure-Ökosystem. |
| **Preismodell** | Pro Zeichen bzw. Volumen; genaue Preise hängen von Region, Tier und Custom Translator ab. |

## Technische Aspekte

Sinnvoll, wenn bereits **Azure Communication Services, Azure Speech, Azure OpenAI oder ein Azure-Enterprise-Abkommen**

EU-Datenresidenz priorisiert,

Custom Translator für Fachdomäne, Glossare für wiederkehrende Fachbegriffe braucht

**Kein Streaming-MT-Dienst - ** stabile Segmente aus STT an die API und Übersetzung dann an TTS weiterreichen.


## Referenzen

- Azure AI Translator: <https://azure.microsoft.com/products/ai-services/ai-translator> 
- Unabhängige Marktübersicht: <https://www.apibenchmarks.com/translation/google-cloud-translation-api>

---
