# DeepL Voice API

# DeepL Voice API

**Produktdokumentation:**

- Überblick: <https://developers.deepl.com/docs/voice/overview> 
- Unterstützte Sprachen: <https://developers.deepl.com/docs/voice/supported-voice-languages> 
- Anforderungen und Limits: <https://developers.deepl.com/docs/voice/voice-api-requirements> 
- API-Referenz (Session anfordern): <https://developers.deepl.com/api-reference/voice/request-session> 
- Ablauf einer Session: <https://developers.deepl.com/docs/voice/understanding-voice-sessions> 
- Ergänzungen zur Leistungsbeschreibung: <https://developers.deepl.com/api-reference/voice/deepl-voice-api-service-specification-updates> 
- AGB und Leistungsbeschreibung: <https://www.deepl.com/en/pro-license> 
- Changelog: <https://developers.deepl.com/docs/resources/roadmap-and-release-notes> 

---

**Recherche-Stand:** 21.10.2026  
**Anbieter:** DeepL SE (Deutschland)  
**Bezugsform:** Proprietärer, cloudbasierter API-Dienst  
**Produktstatus:** General Availability (GA) für Voice API und Speech-to-Speech; einzelne Zusatzfunktionen Beta  
**Architektur:** Funktional kaskadiert (ASR → maschinelle Übersetzung → TTS)

**Produktdokumentation:** [Voice API Overview](https://developers.deepl.com/docs/voice/overview) · [Sprachen](https://developers.deepl.com/docs/voice/supported-voice-languages) · [Requirements](https://developers.deepl.com/docs/voice/voice-api-requirements) · [Session API](https://developers.deepl.com/api-reference/voice/request-session) · [Changelog](https://developers.deepl.com/docs/resources/roadmap-and-release-notes)

## Kurzüberblick

DeepL Voice API verarbeitet gesprochene Sprache kontinuierlich über WebSockets und liefert Originaltranskriptionen, Übersetzungen in mehrere Zielsprachen und optional synthetisierte übersetzte Sprachausgabe. Die Lösung ist ausdrücklich auf geschäftliche Sprachkommunikation, einschließlich Contact Center und BPO, ausgerichtet. [Quelle: Voice API Overview](https://developers.deepl.com/docs/voice/overview) · [DeepL-Pressemitteilung](https://www.deepl.com/en/press-release/deepl_launches_voice_api_for_real_time_speech_transcription_and_translation)

## Kriteriencheck

| Kriterium | Bewertung / Kernergebnis |
| --- | --- |
| 1. Eignung für Live Translation | **Erfüllt:** kontinuierliche Speech-to-Speech-Übersetzung für Gespräche |
| 1. Architektur | **Funktional kaskadiert:** ASR → MT → TTS; genaue interne Modellarchitektur nicht vollständig veröffentlicht |
| 1. Realtime/Streaming | **Erfüllt:** WebSocket, inkrementelle Segmente, Reconnect; **End-to-End-Latenz nicht belegt** |
| 1. Sprachen/Bidirektionalität | **Erfüllt:** DE, EN, BG, PL, FR, IT, ES; für zwei simultane Audiogegenrichtungen z. B. zwei Sessions |
| 1. Telefonie-/Audio-Kompatibilität | **Codec-Ebene erfüllt:** G.711 A-law/μ-law, 8 kHz, nativ für Ein- und Ausgabe; Telefoniequalität nicht belegt |
| 1. Technische Evaluierbarkeit | **Erfüllt bei Zugang:** REST/WebSocket, Python-Quickstart; Voice-fähiger API-Tarif erforderlich |
| 1. Produktreife | **GA:** Voice API seit 15.04.2026, S2ST seit 17.07.2026; Spoken Terms Beta |
| 1. Verarbeitungsort/Datenregion | **Teilweise erfüllt:** EU-Residenz vertraglich möglich, nicht automatisch für alle Voice-Komponenten garantiert |
| 1. Lizenz/kommerziell | **Teilweise erfüllt:** kommerzielle Cloud-Nutzung; keine Modellgewichte; Benchmark-Klausel beachten |
| 10. Terminologie/Kontext | **Erfüllt:** bis zu 10 Glossare pro Sitzung; Spoken Terms mit 300 Zeichen je sprachbezogener Liste |

## 1. Eignung für Live Translation

**Bewertung: erfüllt**

- Echtzeittranskription und -übersetzung von kontinuierlich übertragenem Audio via WebSocket; kein vollständiger Audio-Dateiupload notwendig.
- Ausgaben: Originaltranskript, Übersetzungen in bis zu fünf Zielsprachen und optional synthetisierte übersetzte Sprache.
- Speech-to-Speech explizit als Funktion für Live-Gespräche dokumentiert.
- Contact Center und Business-Process-Outsourcing (BPO) sind ausdrücklich genannte Anwendungsbereiche.
- Fertige Telefonanlagen-Integration wird in der API-Dokumentation nicht als Standardkomponente ausgewiesen; Anbindung über eigene Medien-/Telefonieintegration erforderlich.
- Tatsächliche Verzögerung und Qualität unter Telefoniebedingungen nicht durch verbindliche, öffentlich dokumentierte Kennzahlen belegt.

**Quellen:** [Voice API Overview](https://developers.deepl.com/docs/voice/overview) · [DeepL Voice API – Pressemitteilung](https://www.deepl.com/en/press-release/deepl_launches_voice_api_for_real_time_speech_transcription_and_translation) · [Produktseite](https://www.deepl.com/en/products/voice/campaign/speak-your-language)

## 2. Architektur

**Klassifikation: funktional kaskadiert (ASR → MT → TTS)**

- Dokumentierte Funktionsfolge: automatische Spracherkennung (ASR), maschinelle Übersetzung (MT) und bei Audioausgabe Sprachsynthese (TTS).
- Ein integrierter Cloud-API-Dienst stellt alle Funktionen bereit; separate Ausgabeströme für Quelltranskript, Zieltranskript und synthetisierte Audio-Chunks.
- Übersetzung durch DeepL selbst; Transkription bzw. Sprachsynthese bestimmter Sprachen über externe Dienstleister.
- DeepL nennt in Voice-Leistungsergänzungen Speechmatics (Cantab Research Ltd.) für Spracherkennung und ElevenLabs für Sprachsynthese als Subunternehmer.
- Vollständige interne Modellarchitektur (konkrete Modellanzahl und interne Kopplung) nicht offengelegt. Die Klassifikation ist funktional; getrennte API-Nachrichten allein beweisen keine drei unabhängig betriebenen Modelle.
- Aussagen über „durchgängige“ Echtzeit-Audioübersetzung in Marketing-/AI-Labs-Texten belegen für sich allein kein rein direktes Audio-to-Audio-Modell ohne Textrepräsentation.

**Quellen:** [Understanding Voice Sessions](https://developers.deepl.com/docs/voice/understanding-voice-sessions) · [Supported Voice Languages](https://developers.deepl.com/docs/voice/supported-voice-languages) · [Service Specification Updates](https://developers.deepl.com/api-reference/voice/deepl-voice-api-service-specification-updates) · [TechCrunch, 16.04.2026 (Sekundärquelle)](https://techcrunch.com/2026/04/16/deepl-known-for-text-translation-now-wants-to-translate-your-voice/)

## 3. Realtime-/Streaming-Fähigkeit

**Bewertung: Streaming erfüllt; numerische Latenz nicht belegt**

- `POST /v3/voice/realtime` konfiguriert eine Sitzung und gibt eine temporäre Streaming-URL mit einmal nutzbarem Token zurück; anschließend WebSocket-Übertragung.
- Laufende Ergebnisse mit **tentative** (vorläufig) und **concluded** (abgeschlossen) Textsegmenten; separate Ausgabe von Audio-Chunks einschließlich Text-/Dauerzuordnung.
- Audio-Chunk: maximal **100 KB bzw. 1 Sekunde**; empfohlen **50–250 ms**.
- Streaminggeschwindigkeit: maximal **2× Echtzeit**.
- Inaktivitäts-Timeout nach **30 Sekunden ohne eingehende Daten**.
- Verbindungen können abbrechen; Wiederverbindung mit neuem Token unterstützt, Sitzungskonfiguration und Übersetzungskontext können erhalten bleiben. Token nicht mehrfach verwenden.
- Übersetzter Ausgabestream enthält nach Dokumentation keine künstlich eingefügte Stille bzw. Padding.
- Bei bekannter Quellsprache kann `source_language_mode: fixed` die automatische **Sprachidentifikation** vermeiden; die eigentliche ASR-Transkription entfällt **nicht**.
- Für längere Pausen sind Inaktivitäts-/Reconnect-Strategien zu testen. Gesendete Stille-Audiodaten können zur abrechenbaren Quell-Audiodauer zählen; sie sind keine generell vorgeschriebene Lösung.
- **Nicht belegt:** verbindliche mittlere End-to-End-Latenz, Time-to-First-Translated-Audio und P95 in Millisekunden. Audio-Chunk-Dauer ist keine Übersetzungslatenz.

**Quellen:** [Understanding Voice Sessions](https://developers.deepl.com/docs/voice/understanding-voice-sessions) · [Voice API Requirements](https://developers.deepl.com/docs/voice/voice-api-requirements) · [Real-Time Voice Quickstart](https://developers.deepl.com/docs/voice/real-time-voice-quickstart) · [Billing](https://support.deepl.com/hc/en-us/articles/360020685720-Usage-count-and-billing-in-DeepL-API)

## 4. Sprachunterstützung und Bidirektionalität

**Bewertung: Sprachunterstützung und sprachliche Bidirektionalität erfüllt; simultane Gegenrichtungen integrationsabhängig**

**Offizielle Voice-Sprachmatrix (Auszug):** ✓ DeepL-eigene Funktion; *extern* externe Partnerfunktion.

| Sprache | Transkription | Übersetzung | Sprachausgabe |
| --- | --- | --- | --- |
| Deutsch | ✓ | ✓ | ✓ |
| Englisch (US/GB) | ✓ | ✓ | ✓ |
| **Bulgarisch** | **extern** | ✓ | **extern** |
| Polnisch | ✓ | ✓ | ✓ |
| Französisch | ✓ | ✓ | ✓ |
| Italienisch | ✓ | ✓ | ✓ |
| Spanisch | ✓ | ✓ | ✓; Lateinamerika: extern |
| Türkisch | ✓ | ✓ | ✓ |
| Russisch | ✓ | ✓ | ✓ |
| Ukrainisch | ✓ | ✓ | extern |
| Arabisch | extern | ✓ | extern |

- Unterstützte Quellsprachen können grundsätzlich in unterstützte Zielsprachen übersetzt werden; DE ↔ EN, DE ↔ BG, DE ↔ PL, DE ↔ FR, DE ↔ IT und DE ↔ ES möglich.
- Maximal **fünf Textzielsprachen**, davon maximal **eine Zielsprache mit synthetisierter Audioausgabe** pro Sitzung.
- Für ein simultanes bidirektionales Telefonat mit zwei getrennten Audiokanälen sind beispielsweise **zwei Sitzungen** sinnvoll (Integrationsentwurf, keine fertige DeepL-Telefoniefunktion).
- Bei extern bereitgestellter Transkription (insbesondere BG) keine automatische Quellsprachidentifikation; Quellsprache explizit festlegen (`fixed`).
- `auto` verwendet die angegebene Quellsprache als Hinweis; `fixed` setzt sie verbindlich.
- **Speaker Match** nur bei DeepL-eigener TTS-Unterstützung; nicht für bulgarische Zielsprachausgabe.

**Quellen:** [Supported Voice Languages](https://developers.deepl.com/docs/voice/supported-voice-languages) · [Request Session](https://developers.deepl.com/api-reference/voice/request-session) · [Voice API Requirements](https://developers.deepl.com/docs/voice/voice-api-requirements)

## 5. Telefonie-/Audio-Kompatibilität

**Bewertung: Formate erfüllt; Erkennungs-/Übersetzungsqualität bei 8 kHz nicht belegt**

- Native G.711-Unterstützung mit **8.000 Hz** für Ein- und Ausgabe: **A-law** und **μ-law**.
- PCM **16 Bit / 8 kHz** als Eingang dokumentiert; PCM **16 Bit / 16 bzw. 24 kHz** als Ausgabemöglichkeiten.
- Mono-Audio erforderlich.
- Weitere Eingabeformate gemäß Codec-/Containerkombinationen: PCM, Opus, AAC, FLAC und MP3.
- Standard-Audioausgabe laut API-Referenz: WebM/Opus; alternative telefonietypische Audioausgaben konfigurierbar.
- Herstellerempfehlung für Eingang: PCM mit 16 kHz; Opus mit ca. 32 kbit/s bei begrenzter Bandbreite.
- Bei kompatiblen 8-kHz-G.711-Streams **kein zwangsläufiges Resampling auf 16 kHz** erforderlich; Telefonie-/WebSocket-Bridge für SIP/RTP- bzw. CCaaS-Anbindung kann dennoch notwendig sein.
- In geprüfter API-Dokumentation keine integrierte SIP-Schnittstelle oder allgemein bereitgestellte fertige Genesys-/Twilio-/Amazon-Connect-Anbindung bestätigt.
- **Nicht belegt:** Qualität und Zuverlässigkeit bei 8-kHz-Telefonieaudio, Paketverlusten, Rauschen, Akzenten und Sprecherüberlappung.

**Quellen:** [Request Session – Medienformate](https://developers.deepl.com/api-reference/voice/request-session) · [Voice API Requirements](https://developers.deepl.com/docs/voice/voice-api-requirements)

## 6. Technische Evaluierbarkeit

**Bewertung: erfüllt, jedoch abhängig von API-Berechtigung und Tarif**

- Öffentlich dokumentierte REST-/WebSocket-API; Authentifizierung mit API-Key und temporären Verbindungstokens.
- Python-Quickstart, API-Referenz und maschinenlesbare Streaming-Spezifikation verfügbar; Voice API laut Dokumentation noch nicht vollständig in den allgemeinen offiziellen DeepL-SDKs integriert.
- Kostenloser **API Developer**-Tarif: kein regulärer Speech-to-Text-/Speech-to-Speech-Zugang.
- **API Growth:** laut Support-Angaben bei monatlicher Abrechnung **10 STT-Stunden pro Monat**, bei jährlicher Abrechnung **120 STT-Stunden pro Jahr**; genannte maximale STT-Nutzung **300 Stunden/Monat** (tarifbezogene Angabe, keine generelle S2ST-Grenze).
- S2ST-Zugriff nur bei dafür berechtigtem Tarif; aus dem GA-Status allein folgt keine Freischaltung für jedes Konto.
- Individueller projektbezogener Testzugang kann angefragt werden, wird öffentlich nicht garantiert.
- DeepL behält sich Limits für parallele Voice-Sitzungen vor; konkreten Grenzwert bzw. SLA für gewünschten Vertrag klären.
- Kein Self-Hosting und kein öffentlicher Modellgewichtszugriff.

**Quellen:** [Voice API Overview](https://developers.deepl.com/docs/voice/overview) · [Real-Time Voice Quickstart](https://developers.deepl.com/docs/voice/real-time-voice-quickstart) · [API-Pläne](https://support.deepl.com/hc/en-us/articles/360021200939-DeepL-API-plans) · [Nutzung und Abrechnung](https://support.deepl.com/hc/en-us/articles/360020685720-Usage-count-and-billing-in-DeepL-API) · [AGB](https://www.deepl.com/en/pro-license)

## 7. Produktreife / Entwicklungsstatus

**Bewertung: GA für Voice API und Speech-to-Speech; bestimmte Zusatzfunktionen Beta**

- Erste Voice-API-Veröffentlichung laut Changelog im November 2025.
- **15.04.2026:** Voice API laut Changelog allgemein verfügbar.
- **17.07.2026:** synthetisierte Sprachausgabe/S2ST verlässt Closed Beta und wird GA.
- **Spoken Terms:** in der API-Referenz weiterhin als **Beta** markiert.
- Für zusätzliche Stimmen und Sprachvarianten wurden 2026 schrittweise neue Verfügbarkeiten veröffentlicht, darunter Speaker Match.
- **Dokumentationsabweichung:** Ankündigung vom Februar 2026 spricht bereits von allgemeiner Verfügbarkeit, während der Changelog die GA-Einführung im April datiert; ältere Leistungsergänzungen enthalten teilweise Beta-Bezüge.
- Ebenfalls zu beachten: möglicherweise abweichende Beta-Kennzeichnungen einzelner Sprachfeatures trotz GA der allgemeinen S2ST-Funktion.
- Für Alpha-/Beta-Funktionen gelten zusätzliche Änderungsvorbehalte nach Vertragsbedingungen.
- Allgemeine DeepL-Verfügbarkeitsaussagen sind nicht mit einem Voice-spezifischen, individuell garantierten SLA gleichzusetzen; öffentlich keine garantierte End-to-End-Latenz gefunden.

**Quellen:** [Roadmap and Release Notes](https://developers.deepl.com/docs/resources/roadmap-and-release-notes) · [Request Session](https://developers.deepl.com/api-reference/voice/request-session) · [Voice API Pressemitteilung](https://www.deepl.com/en/press-release/deepl_launches_voice_api_for_real_time_speech_transcription_and_translation) · [AGB](https://www.deepl.com/en/pro-license)

## 8. Verarbeitungsort / Datenregion

**Bewertung: teilweise erfüllt; EU-exklusive Verarbeitung nur mit geeigneter vertraglicher Absicherung beurteilbar**

- Unternehmenssitz DeepL SE in Deutschland; **Unternehmenssitz allein ist keine Zusicherung des Verarbeitungsorts**.
- DeepL nutzt eigene Infrastruktur und Drittanbieter-Cloud-Infrastruktur (einschließlich AWS); Verarbeitung nicht mehr grundsätzlich auf Europa beschränkt.
- Regionale Endpunkte dokumentiert: EU-Standard `https://api.deepl.com`, USA `https://api-us.deepl.com`, Japan `https://api-jp.deepl.com`.
- Vertraglich vereinbarte **Datenresidenz** bzw. *Regional Deployment Addendum* für geeignete Kunden möglich; der Standardendpunkt allein garantiert keine ausschließliche EU-Verarbeitung sämtlicher Voice-Schritte.
- Externe Voice-Subprozessoren, insbesondere Speechmatics (ASR) und ElevenLabs (TTS), müssen bei Datenresidenzbewertung berücksichtigt werden.
- **Für Bulgarisch besonders relevant:** ASR und TTS extern; konkrete Verarbeitungsorte dieser Partner innerhalb der DeepL-Leistung nicht öffentlich eindeutig nachgewiesen.
- In einer früheren Dokumentversion wurde Amazon Transcribe genannt; später Speechmatics: Subprozessorenlisten vor Vertragsabschluss erneut prüfen.
- DeepL beschreibt für bezahlte API-/Geschäftskunden grundsätzlich eine Nichtverwendung der Kundeninhalte zum allgemeinen Training; dies ist keine Datenresidenzgarantie.
- **Nicht belegt:** Deutschland-exklusive Voice-Datenverarbeitung; vollständige EU-Exklusivität aller Subprozessoren ohne Individualvereinbarung.

**Quellen:** [DeepL Infrastruktur-Blog](https://www.deepl.com/en/blog/expanding-deepl-data-infrastructure) · [Regional Endpoints](https://developers.deepl.com/docs/getting-started/regional-endpoints) · [Voice Service Specification Updates](https://developers.deepl.com/api-reference/voice/deepl-voice-api-service-specification-updates) · [DeepL-AGB](https://www.deepl.com/en/pro-license) · [Dokumentänderung Subprozessoren (GitHub)](https://github.com/DeepLcom/api-docs/pull/182/files)

## 9. Lizenz und kommerzielle Nutzbarkeit

**Bewertung: teilweise erfüllt; kommerziell nutzbar, mit vertraglich relevanten Einschränkungen**

- Proprietärer Cloud-Dienst für geschäftliche Nutzung; kein Self-Hosting, keine offenen Modellgewichte.
- Kundenseitige Rechte an bereitgestellten Inhalten bleiben grundsätzlich erhalten; Bedingungen zu Übersetzungen und synthetisierten Stimmen richten sich nach dem jeweiligen Vertrag.
- **Benchmark-Klausel, AGB 8.1.1(e):** Benchmarks bzw. Kapazitätstests der technischen DeepL-Infrastruktur ohne ausdrückliche schriftliche Genehmigung eingeschränkt.
- Ob reine Übersetzungsqualitätsvergleiche darunterfallen, ist rechtlich nicht abschließend geklärt; insbesondere **Latenz-, Last- und Kapazitätsmessungen** vorab schriftlich abstimmen.
- Keine pauschale verifizierte Freigabe zur Veröffentlichung beliebiger Benchmarkergebnisse; konkrete Veröffentlichung und Kennzeichnung vertraglich prüfen.
- Weitere vertragliche Einschränkungen betreffen bestimmte konkurrierende ML-/MT-Anwendungen sowie unzulässige Stimmenimitation, Deepfakes und nicht autorisierte Verwendungen generierter Stimmen.
- **Kennzeichnungspflicht:** Für bestimmte unveränderte DeepL-generierte Inhalte muss gegenüber Endnutzern die DeepL-Erzeugung kenntlich gemacht werden; genauen Geltungsbereich der AGB für die geplante Anwendung prüfen.
- Erforderliche Einwilligungen bzw. Rechtsgrundlagen für Erhebung und Verarbeitung von Sprachdaten sowie Sprachsynthese sind vom Kunden sicherzustellen.
- Vertragsrecht/Gerichtsstand gemäß den konkret geltenden DeepL-Bedingungen prüfen; keine pauschale Übertragung einzelner Vertragsfassungen auf alle Angebote.

**Quelle:** [DeepL Pro Terms and Conditions](https://www.deepl.com/en/pro-license) (insb. Abschnitte 8.1.1(e) und 8.4; genaue aktuelle Vertragsfassung maßgeblich)

## 10. Terminologie-/Kontextanpassung

**Bewertung: erfüllt, mit Einschränkungen**

- **Glossare:** definierte Fachübersetzungen; maximal **10 Glossare je Voice-Sitzung** mit Prioritätsreihenfolge.
- Sprachpaarspezifische Wörterbücher erforderlich. Bei automatischer Sprachidentifikation ohne passendes Glossar-Wörterbuch erfolgt keine Anwendung für das erkannte Sprachpaar.
- **Streaming-Einschränkung:** Mehrteilige Glossarbegriffe können über getrennte Transkriptionssegmente verteilt werden und deshalb verfehlt werden. DeepL beschreibt dies als prinzipbedingte Einschränkung des Streamingverfahrens.
- **Spoken Terms:** einsprachige ASR-Begriffslisten zur verbesserten Transkription von Eigennamen, Produktbezeichnungen und Abkürzungen; Einfluss auf Erkennung, nicht unmittelbar auf Übersetzungsregeln.
- Spoken Terms beachten Groß-/Kleinschreibung; maximal **300 Zeichen insgesamt je sprachbezogener Liste**, nicht 300 Begriffe.
- Seit Juni 2026 laut Changelog auch bei extern transkribierten Sprachen verfügbar, etwa Bulgarisch; Status der Funktion weiterhin Beta.
- Steuerung der **Förmlichkeit** (formell/informell) über die Session-Konfiguration möglich.
- **Nicht belegt:** garantierte exakte Übertragung unbekannter Vertrags-/Kundennummern, freies Systemprompting, umfangreiche Kontextdokumente oder eigenes Fine-Tuning der Voice-Modelle.

**Quellen:** [Spoken Terms](https://developers.deepl.com/docs/customize/improving-transcription-with-spoken-terms) · [Voice API Overview](https://developers.deepl.com/docs/voice/overview) · [Voice API Requirements](https://developers.deepl.com/docs/voice/voice-api-requirements) · [Request Session](https://developers.deepl.com/api-reference/voice/request-session) · [Changelog](https://developers.deepl.com/docs/resources/roadmap-and-release-notes)

## Ergänzung: Preis / Abrechnung

- Abrechnung nach **gestreamter Quell-Audiominute**, nicht allein nach WebSocket-Verbindungsdauer.
- S2ST umfasst Transkription, Übersetzung und synthetisierte Sprachausgabe; STT ohne übersetzte Audioausgabe ist gesondert bepreist.
- Angefangene verbrauchte Audiominuten werden nach DeepL-Abrechnungsregeln kumuliert und gerundet.
- **AWS Marketplace – konkretes Jahresangebot:** API Growth **312 USD/12 Monate**, mit **120 h STT** und **60 h S2ST** pro Jahr.
- **AWS Marketplace – konkretes Jahresangebot:** API Enterprise **15.000 USD/12 Monate**, mit Enterprise-Konditionen einschließlich Datenresidenz; enthaltene Stunden gemäß jeweils aktuellem Angebot prüfen.
- Diese Marketplace-Angebote sind **keine allgemeingültigen DeepL-Direktvertragspreise**.
- **STT-Mehrverbrauch im AWS-Angebot:** voneinander abweichende Preisangaben (u. a. 1,85 USD/h und rechnerisch ca. 2,85 USD/h in unterschiedlichen Angebotsstellen); kein eindeutiger Wert für die Longlist ohne Klärung.
- **S2ST-Mehrverbrauch:** **3,75 USD/h** nur aus externer Sekundärquelle; kein verifizierter offizieller allgemeiner DeepL-Preis. Daher nicht als gesicherter Kostenwert verwenden.
- Zwei parallel laufende Richtungen können zwei Quell-Audioströme und entsprechend zusätzliche abrechenbare Audiominuten bedeuten. Rechenbeispiele nur mit expliziten Annahmen vergleichbar.

**Quellen:** [DeepL – Nutzung und Abrechnung](https://support.deepl.com/hc/en-us/articles/360020685720-Usage-count-and-billing-in-DeepL-API) · [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-2fii5e2hmuneo) · [DeepL API-Pläne](https://support.deepl.com/hc/en-us/articles/360021200939-DeepL-API-plans) · [usagepricing.com (nicht verifizierte Sekundärquelle)](https://www.usagepricing.com/blueprint/deepl)

## Dokumentierte Unklarheiten und offene Prüfpunkte

1. **Latenz und Qualität:** E2E-Latenz/P95 sowie Übersetzungsqualität bei G.711 8 kHz, insbesondere DE ↔ BG und DE ↔ PL, empirisch vergleichen; vorher Benchmark-Berechtigung klären.
2. **Voice-GA-Kennzeichnung:** GA in Release Notes versus einzelne Beta-Labels in Feature-/Leistungsspezifikationen abgleichen.
3. **Datenresidenz:** Schriftliche Zusicherung zur EU-Verarbeitung einschließlich Speechmatics/ElevenLabs anfordern; aktuelle Subprozessorenliste prüfen.
4. **Preis und Zugang:** S2ST-Berechtigung, S2ST-/STT-Mehrverbrauch, parallele Sessionlimits und Vertragskosten verbindlich bestätigen lassen.
5. **Nutzungsrechte:** Umfang der Benchmark-Klausel, Publikation von Ergebnissen und DeepL-Kennzeichnungspflichten schriftlich klären.
6. **Terminologie:** Glossare mit mehrteiligen Begriffen und Spoken Terms mit Produkt- und Vertragsbezeichnungen testen; Zahlenfolgen gesondert evaluieren.
7. **Sitzungsbetrieb:** 30-Sekunden-Inaktivität, längere Haltephasen und Wiederverbindung testen.

## Hauptstärken

- Echtzeit-Speech-to-Speech mit WebSocket-Streaming und inkrementellen Ergebnissen.
- Alle sieben priorisierten Sprachen unterstützt.
- Native G.711-A-law-/μ-law-Unterstützung bei 8 kHz im Ein- und Ausgang.
- Glossare und Spoken Terms für Fachterminologie.
- GA-Status der Kernfunktionen; dokumentierter API-Zugang und Python-Quickstart.
- Vertraglich mögliche EU-Datenresidenz.

## Haupteinschränkungen

- Keine belastbaren veröffentlichten E2E-Latenz-/Telefoniequalitätswerte.
- Externe Sprachkomponenten unter anderem für Bulgarisch und damit zusätzlich zu prüfende Datenverarbeitung.
- EU-Residenz nicht ohne geeigneten Vertrag garantiert; kein Self-Hosting.
- S2ST-Zugang, Mehrverbrauchspreise und Parallelitätslimits tarifabhängig.
- Benchmark-Klausel mit Klärungsbedarf für die VIER-Evaluation.
- Terminologiegrenzen und teils Beta-Status von Spoken Terms.

**Longlist-Einordnung:** **Aufnehmen.** Technisch relevanter Kandidat für Telefonie-S2ST mit nativen Telefoniecodecs und Terminologieanpassung. Vor einer Shortlist- bzw. Produktionsentscheidung müssen Datenschutzregion, Preis, Benchmark-Rechte und Leistung unter realistischen Telefoniebedingungen geklärt werden.
