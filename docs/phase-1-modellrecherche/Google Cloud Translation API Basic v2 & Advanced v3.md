# Google Cloud Translation API Basic v2 & Advanced v3

Stand der Recherche: 26.10.2026

**Maximale Sprachabdeckung, Glossare und eine starke Cloud-Integration.** Die Advanced Edition (v3) unterstützt über 130 Sprachen, Glossare, Batch- und Dokumentübersetzung sowie optional LLM-basierte bzw. adaptive Übersetzung

| Kriterium | Google Cloud Translation API |
| --- | --- |
| **Modell** | Cloud Translation Basic v2 und Advanced v3; NMT, Custom Models, Translation LLM, Adaptive Translation. docs.cloud.google+1 |
| **Architektur** | Reiner MT-Baustein: Text → Text; kein STT, kein TTS. |
| **Eignung für Live Translation** | Sehr gut als Mittelstufe; segmentweise REST/gRPC-Aufrufe zwischen STT und TTS. |
| **Realtime-/Streaming-Fähigkeit** | Kein natives Audio-/MT-Streaming; Text-Segmente werden einzeln übersetzt. |
| **Sprachunterstützung** | Über 130 Sprachen; deutlich breiter als DeepL. |
| **Bidirektionalität** | Ja, für unterstützte Sprachpaare; automatische Source-Language-Detection möglich. |
| **Telefonie-/Audio-Kompatibilität** | Indirekt: nur Text; Audio muss vorher durch STT und danach durch TTS. |
| **Technische Evaluierbarkeit** | Sehr gut: COMET/chrF, Glossar-Trefferquote, Latenz, NMT-vs-LLM-Vergleich, Adaptive-Translation-Tests. |
| **Produktreife / Status** | Sehr reif; globales Google-Cloud-Produkt mit ausführlicher Doku und Client Libraries. |
| **Verarbeitungsort / Datenregion** | Google Cloud Regionen; Region und Datenresidenz über Projekt-/API-Konfiguration steuerbar. |
| **Datennutzung** | Google Cloud-Datenschutzbestimmungen und Projekt-Konfiguration maßgeblich; für sensible Gespräche DPA, Retention und Region vertraglich prüfen. |
| **Lizenz / kommerzielle Nutzung** | Kommerzielle Cloud-API; kein Open Source, kein Self-Hosting der Translation-Modelle. |
| **Terminologie-/Kontextanpassung** | Glossare für domänenspezifische Begriffe und Named Entities; Custom Models; Adaptive Translation mit Beispielen. |
| **Schnittstellen / Kompatibilität** | REST und gRPC; offizielle Client Libraries in acht Sprachen; IAM-Authentifizierung für Advanced v3. |
| **Preismodell** | Pro Zeichen; NMT ca. 20 USD / 1M Zeichen nach Freikontingent; LLM-Modelle separat nach Input- und Output-Zeichen. |

## Technische Aspekte

V**iele Sprachpaare** unterstützt

Advanced Edition - Glossare, Batch-Übersetzung und Custom Models

K**ein Streaming-MT-Dienst - **für Live-Übersetzung stabile Segmente aus STT an die API , Übersetzung dann an TTS weiterreichen


## Testing:

- klassische NMT-Übersetzung,
- LLM-basierte Übersetzung,
- Adaptive Translation mit eigenen Beispielen,
- Verhalten mit und ohne Glossar


## Referenzen

- API Overview: [https://docs.cloud.google.com/translate/docs/api-overviewdocs.cloud.google](https://docs.cloud.google.com/translate/docs/api-overview)
- Cloud Translation Docs: <https://docs.cloud.google.com/translate/docs> 
- Pricing: <https://cloud.google.com/products/translate/pricing> 
- FAQ: <https://docs.cloud.google.com/translate/faq> 
- Unabhängige Übersicht: <https://www.apibenchmarks.com/translation/google-cloud-translation-api>

---
