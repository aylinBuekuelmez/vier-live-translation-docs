# Google Gemini 3\.5 Live Translate \(Preview\)

Longlist-Recherche – Softwareentwicklungsprojekt VIER

Recherche- und Prüfstand: 20.10.2026 

Anbieter: Google / Google DeepMind

Modell-ID: gemini-3.5-live-translate-preview

Bezugsform: Proprietärer, cloudbasierter API-Dienst

Architektureinordnung: Integrierte Audio-to-Audio-/Speech-to-Speech-Übersetzung; interne Verarbeitung nicht vollständig offengelegt

Produktstatus: Public Preview (API)

## Kurzüberblick

Gemini 3.5 Live Translate ist ein auf kontinuierliche Echtzeit-Sprach-zu-Sprach-Übersetzung spezialisiertes Modell. Über die Gemini Live API werden eingehende Audiostücke während des Sprechens verarbeitet und als übersetzte Audiostücke ausgegeben; optional sind Eingangs- und Ausgangstranskripte verfügbar. Die offizielle Modell-ID lautet gemini-3.5-live-translate-preview. Anders als ein Sprachagent ist das Modell ausdrücklich auf die Übersetzungsfunktion beschränkt. Batch API, freie Instructions und Tools werden für diese Modellvariante nicht unterstützt.

Quellen: Q1 Live Translation Guide · Q2 Modellseite — Abruf 20.10.2026.

## Kriteriencheck

| Kriterium | Bewertung | Kernbeleg / Einschränkung |
| --- | --- | --- |
| Eignung für Live Translation | Erfüllt | Kontinuierliche Speech-to-Speech-Übersetzung, keine Batch-Verarbeitung |
| Architektur | Integriertes S2ST (Audio-to-Audio) | Keine separat zu orchestrierende ASR→MT→TTS-Kette; interne Architektur nicht vollständig offengelegt |
| Realtime/Streaming | Erfüllt | Bidirektionaler WebSocket, 100-ms-Eingabechunks; numerische Latenz nicht belegt |
| Sprachen/Bidirektionalität | Sprachen erfüllt; simultane Gegenrichtungen zu evaluieren | DE, EN, BG, PL, FR, IT, ES; ein Zielsprachcode pro Sitzung |
| Telefonie/Audio | Teilweise erfüllt | 16-kHz-PCM-Eingang/24-kHz-PCM-Ausgang; G.711-/8-kHz-Transcoding erforderlich |
| Evaluierbarkeit | Erfüllt | Gemini API, AI Studio, Python/JavaScript und WebSocket; Zugang/Quoten beachten |
| Produktreife | Preview | Public Preview; kein GA-Status für die konkrete API-Modellversion |
| Datenregion | EU/DE nicht nachgewiesen | Agent Platform listet für dieses Modell nur global; keine EU-Residenzzusage |
| Lizenz/kommerzielle Nutzung | Grundsätzlich möglich; vertraglich begrenzt | Proprietäre Cloud-API; EWR-Paid-Service-Regel; kein Self-Hosting; Benchmarkveröffentlichung nicht ausdrücklich untersagt in geprüften API-Terms |
| 10. Terminologie/Kontext | Native Anpassung nicht unterstützt | Keine System Instructions, Glossare, Custom Vocabulary oder Tuning dokumentiert |

## 1. Eignung für Live Translation

Bewertung: erfüllt.

- Google beschreibt das Modell als Live-Übersetzer: kontinuierliches Audio-Streaming und übersetzte Audioausgabe während des Sprechens, ohne auf abgeschlossene Sprecherturns zu warten. Quelle: Q1, Abruf 20.10.2026.
- Audioeingabe und übersetzte Audioausgabe sind der primäre Einsatzzweck; Eingangs- und Ausgangstranskripte können zusätzlich ausgegeben werden. Quelle: Q1, Abruf 20.10.2026.
- Die Modellseite weist die Live API als unterstützt und die Batch API als nicht unterstützt aus. Quelle: Q2, Abruf 20.10.2026.
- Google nennt mehrsprachige Anrufe, Meetings und Übertragungen als Anwendungsfälle; Grab testet das Modell bei der Verständigung von Fahrern und Reisenden. Die von Google genannten über 10 Mio. Grab-Sprachanrufe pro Monat sind keine nachgewiesene Anzahl produktiv über das Modell übersetzter Anrufe. Quelle: Q4 Google-Blog, 09.06.2026; Abruf 20.10.2026.

## 2. Architektur

Klassifikation: integriertes Speech-to-Speech / Audio-to-Audio; intern nicht vollständig dokumentiert.

- Google bezeichnet die Modellvariante als Audio-to-Audio-Modell, optimiert für latenzarme Live-Übersetzung. Das Modell basiert laut DeepMind-Model-Card auf Gemini 3 Pro. Quellen: Q2 Modellseite · Q3 Model Card, Abruf 20.10.2026.
- Die Nutzerschnittstelle erfordert keine getrennt bereitzustellende ASR-, Übersetzungs- und TTS-Pipeline. Transkripte können optional zugeschaltet werden. Daraus folgt nicht der Nachweis, dass intern keine Textrepräsentationen oder mehrstufigen Verarbeitungsschritte existieren. Quelle: Q1, Abruf 20.10.2026.
- Modelllimits laut Gemini API: 131.072 Input-Token, 65.536 Output-Token; laut Model Card gerundet 128K/64K. Diese Tokenlimits sind keine Garantie einer bestimmten Sitzungsdauer. Quellen: Q2 · Q3, Abruf 20.10.2026.
- Dokumentationsabweichung: Die Model Card nennt „Audio und Text“ als Eingaben der Modellfamilienvariante; der spezifische Live-Translate-API-Guide gestattet für Übersetzung ausschließlich Audioeingaben, keinen Texteingang. Für die praktische API-Evaluation ist die konkrete API-Vorgabe maßgeblich. Quellen: Q3 · Q1, Abruf 20.10.2026.

## 3. Realtime-/Streaming-Fähigkeit

Bewertung: Streaming erfüllt; numerische Latenz nicht belegt.

- Streaming über persistente bidirektionale WebSocket-Verbindung (BidiGenerateContent); Audioeingang, übersetzte Audiochunks und optional Transkripte laufen innerhalb der Verbindung. Quellen: Q1 · Q5 Cloud-Modellseite, Abruf 20.10.2026.
- Entwickler-WebSocket-Endpunkt: wss://generativelanguage.googleapis.com/ws/google.ai.generativelanguage.v1beta.GenerativeService.BidiGenerateContent. Quelle: Q1, Abruf 20.10.2026.
- Empfohlene Audio-Chunk-Dauer: 100 ms. Das ist die Übertragungsstückgröße, keine garantierte Übersetzungsverzögerung. Quelle: Q1, Abruf 20.10.2026.
- Google beschreibt qualitativ eine Verzögerung von „wenigen Sekunden“ gegenüber dem Sprecher. Eine verbindliche durchschnittliche oder P95-End-to-End-Latenz wird nicht angegeben. Quelle: Q4, Abruf 20.10.2026.
- Die offizielle Evaluationsmethodik beschreibt Initial-Latenz (Start Quellrede bis Start übersetzte Rede, gemessen per Voice Activity Detection) und Wortlatenz (durchschnittliche Zeit zwischen Ende eines Quellworts und Beginn des zugeordneten Zielworts), nennt jedoch keine numerischen Messergebnisse. Quelle: Q6 Evaluationsmethodik, PDF, Abruf 20.10.2026.
- Allgemeine Live-API-Dokumentation: Verbindungslebensdauer ca. 10 Minuten, reine Audiositzung ohne Kontextkomprimierung 15 Minuten; Session Resumption und Kontextkomprimierung sind für die allgemeine Live API dokumentiert. Nicht belegt: Ob alle Mechanismen für genau diese Translate-Preview-Version verwendbar sind. Quelle: Q7 Session Management, Abruf 20.10.2026.
- Bekannte Grenzen laut Google: Stimmen können nach längeren Pausen wechseln oder bei schnellen Mehrsprecherwechseln an einer Stimme hängen bleiben; die automatische Spracherkennung kann bei (nicht-muttersprachlichen) Akzenten, ähnlichen Sprachen und schnellen Sprachwechseln Probleme haben; nicht alle Hintergrundgeräusche werden herausgefiltert. Quellen: Q1 · Q3, Abruf 20.10.2026.

## 4. Sprachunterstützung und Bidirektionalität

Bewertung: geforderte Sprachen erfüllt; gleichzeitige Gegenrichtungen bedürfen eigener Sitzungsarchitektur.

| Projektsprache | BCP-47-Code | In offizieller Live-Translate-Liste |
| --- | --- | --- |
| Deutsch | de | Ja |
| Englisch | en | Ja |
| Bulgarisch | bg | Ja |
| Polnisch | pl | Ja |
| Französisch | fr | Ja |
| Italienisch | it | Ja |
| Spanisch | es | Ja |

Quelle Sprachmatrix: Q1 – Supported Languages, Abruf 20.10.2026.

- Google gibt mehr als 70 unterstützte Sprachen an; weitere Beispiele sind Türkisch, Arabisch, Russisch, Ukrainisch, Rumänisch und Vietnamesisch. Automatische Identifikation der gesprochenen Quellsprache wird beworben. Quellen: Q1 · Q4, Abruf 20.10.2026.
- Übersetzungen können grundsätzlich in beide Sprachrichtungen konfiguriert werden (z. B. DE→BG und BG→DE), da die gewünschte Zielsprache pro Session über targetLanguageCode gesetzt wird. Standard-Zielsprache ist Englisch (en). Quelle: Q1, Abruf 20.10.2026.
- Simultane bidirektionale Telefonie: Pro Session ist nur eine Zielsprachenkonfiguration dokumentiert. Zwei getrennte, parallel geführte Sessions mit separaten Audiokanälen sind daher ein technischer Integrationsansatz, nicht eine ausdrücklich garantierte fertige Zweiwege-Telefoniefunktion. Quelle: Q1, Abruf 20.10.2026.
- echoTargetLanguage: false (Standard) unterdrückt die erneute Sprachausgabe von Eingangssprache, die bereits der Zielsprache entspricht. Quelle: Q1, Abruf 20.10.2026.
- Einschränkungen der automatischen Spracherkennung bei starken Akzenten, ähnlichen Sprachen (z. B. Spanisch/Portugiesisch) und schnellen Sprachwechseln sind offiziell dokumentiert. Laut Google soll dies nur das Eingangstranskript betreffen; Sprachcodes und finale Übersetzung sollen korrekt bleiben. Für Telefonie-Audio nicht verifiziert. Quellen: Q1 · Q3, Abruf 20.10.2026.

## 5. Telefonie-/Audio-Kompatibilität

Bewertung: teilweise erfüllt – nur mit Anpassung der Telefonie-Audioformate.

| Eigenschaft | Offizielle Vorgabe |
| --- | --- |
| Audioeingabe | Rohes PCM, 16 Bit, 16 kHz, Mono, Little-endian |
| Audioausgabe | Rohes PCM, 16 Bit, 24 kHz, Mono, Little-endian |
| Empfohlene Chunk-Dauer | 100 ms |
| Native G.711 A-law/μ-law-Eingabe | Nicht dokumentiert |
| Native 8-kHz-Telefonieeingabe | Nicht dokumentiert |
| SIP-/RTP-Telefonieschnittstelle | Nicht dokumentiert |

Quelle: Q1 – Sending Audio, Abruf 20.10.2026.

- Für übliche G.711-A-law/μ-law-Telefonie mit 8 kHz ist ein Medienadapter erforderlich: G.711 dekodieren, für den API-Eingang auf 16-kHz-PCM resamplen; den 24-kHz-PCM-Ausgang für die Telefonanlage passend umwandeln. Eigene technische Ableitung aus den dokumentierten Formaten, kein nativer Google-G.711-Support. Quellen: Q1 · Q5, Abruf 20.10.2026.
- Google empfiehlt die strikte Einhaltung des 16-kHz-Eingabeformats, um Audioartefakte und Tonhöhenfehler zu vermeiden. Quelle: Q5, Abruf 20.10.2026.
- Eine direkte Qualitätsmessung für 8-kHz-G.711-Telefonieaudio nach Konvertierung, Paketverlust oder stark verrauschte Leitung ist nicht belegt. Die dokumentierte allgemeine Geräuschfilterung ersetzt diesen Test nicht. Quellen: Q1 · Q3, Abruf 20.10.2026.

## 6. Technische Evaluierbarkeit

Bewertung: erfüllt; Zugang und Preview-Quoten beachten.

- Test über Google AI Studio, Gemini Developer API und offiziellen Python-/JavaScript-Beispielcode möglich; direkte WebSocket-Anbindung dokumentiert. Quelle: Q1, Abruf 20.10.2026.
- Google bietet eine Beispiel-App (GitHub) sowie einen Einstieg über Google Cloud / Agent Platform mit Einführungs-Notebook an. Quellen: Q1 · Q4 (Beispiel-App) · Q5 (Einführungs-Notebook), Abruf 20.10.2026.
- Widerspruch in der Cloud-Modellseite: Die Einleitung beschreibt eine bidirektionale Live-/WebSocket-API, die Capability-Tabelle derselben Seite kennzeichnet „Gemini Live API“ hingegen als nicht unterstützt (während GenerateContent API als unterstützt erscheint). Der Gemini-Developer-API-Live-Translate-Guide und die spezifische Modellseite dokumentieren den Live-Zugang eindeutig. Der konkrete Zugang über Agent Platform sollte mit einem realen API-Aufruf überprüft werden. Quellen: Q5 · Q1 · Q2, Abruf 20.10.2026.
- Die Preisseite weist einen Free Tier und einen Paid Tier aus. Verfügbare Quoten und gleichzeitige Verbindungen sind account-/tierabhängig; ein bestimmtes Concurrency-Limit für dieses Preview-Modell ist hier nicht bestätigt. Quelle: Q8 Preise, Abruf 20.10.2026.
- Für Client-Anwendungen unterstützt Google kurzlebige Zugriffstokens (Ephemeral Tokens), bei denen bestimmte Konfigurationsfelder gesperrt werden können. Quelle: Q1, Abruf 20.10.2026.

## 7. Produktreife / Entwicklungsstatus

Klassifikation: Public Preview (nicht GA).

- Offizielle API-Modell-ID: gemini-3.5-live-translate-preview. Die Agent-Platform-Modellseite nennt die Launch Stage Preview und ein Cloud-Release-Datum August 2026. Quelle: Q5, Abruf 20.10.2026.
- Die öffentliche Entwickler-Preview wurde laut Google-Blog bereits am 09.06.2026 vorgestellt. Das Cloud-Release-Datum August 2026 bezeichnet einen anderen Veröffentlichungs-/Bereitstellungskontext; beide Angaben sind deshalb nicht gleichzusetzen. Quelle: Q4, Abruf 20.10.2026.
- Die Nutzung des Modells in Google Translate oder eine separate Google-Meet-Preview bedeutet keinen GA-Status der Entwickler-API. Quelle: Q4, Abruf 20.10.2026.
- Offiziell genannte Einschränkungen: inkonsistente Stimmreplikation (Stimmwechsel nach langen Pausen, falsches Geschlecht abhängig vom Sprechbeginn, Festhängen auf einer Stimme bei schnellen Mehrsprecherwechseln), Schwächen der Spracherkennung bei Akzenten sowie unvollständige Filterung von Hintergrundgeräuschen. Quellen: Q1 · Q3, Abruf 20.10.2026.
- Kein modellspezifisch zugesichertes SLA oder verbindliche Aufrechterhaltung dieser Preview-Modellversion nachgewiesen. Quellen: Q2 · Q5, Abruf 20.10.2026.

## 8. Verarbeitungsort / Datenregion

Bewertung: EU-/DE-exklusive Verarbeitung nicht nachgewiesen; für eine harte EU-Residenzanforderung derzeit nicht geeignet ohne gesonderte Zusage.

- Die Google-Cloud-Modellseite führt für diese konkrete Modell-ID ausschließlich global als unterstützten Bereitstellungsstandort. Keine spezifische EU- oder Deutschlandregion ausgewiesen. Quelle: Q5, Abruf 20.10.2026.
- Laut Google verarbeiten globale Endpunkte Daten weltweit, ohne Bindung an eine bestimmte geografische Region; eine In-Region-Verarbeitung oder Datenresidenz ist damit nicht garantiert. Für Gemini 3.5 Live Translate ist keine ML-Processing-Zusage für EU/DE ausgewiesen. Quelle: Q9 Endpoints/Locations und Data Residency, Abruf 21.10.2026.
- Für die Gemini Developer API gelten vertragliche Regeln zur Datenverarbeitung: Bei Paid Services werden Prompts/Responses laut Google nicht zur Produktverbesserung genutzt. Prompts und Antworten (hier: Gesprächsaudio/Transkripte) werden jedoch zeitlich begrenzt zur Erkennung von Richtlinienverstößen protokolliert und können dabei vorübergehend in jedem Land gespeichert oder gecacht werden, in dem Google oder seine Beauftragten Einrichtungen betreiben. Quelle: Q10 Gemini API Terms, Abruf 20.10.2026.
- Für Personen im EWR, in der Schweiz und im Vereinigten Königreich gelten die Paid-Datenverwendungsregeln laut Google auch bei kostenlosem Zugang. Diese Regel betrifft die Datennutzung, nicht die regionale Verarbeitung. Quelle: Q10, Abruf 20.10.2026.
- Die Existenz von europäischen Regionen für andere Gemini-Modelle belegt keine EU-Verfügbarkeit von Live Translate. Quellen: Q5 · Q9, Abruf 20.10.2026.

## 9. Lizenz und kommerzielle Nutzbarkeit

Bewertung: proprietär und grundsätzlich geschäftlich nutzbar; spezielle Bedingungen beachten.

- Nutzung als Google-Cloud-/Gemini-API-Service für professionelle und geschäftliche Entwickleranwendungen vorgesehen. Keine öffentlich bereitgestellten Modellgewichte und keine dokumentierte Self-Hosting-Option. Quellen: Q10 Gemini API Terms · Q2 Modellseite, Abruf 20.10.2026.
- Für API-Clients, die Nutzern im EWR, in der Schweiz oder in Großbritannien bereitgestellt werden, verlangen die Gemini API Additional Terms ausdrücklich Paid Services. Ein vorhandener kostenloser Testtarif ist daher nicht pauschal für einen produktiven Endnutzerbetrieb im EWR nutzbar. Quelle: Q10, Abruf 20.10.2026.
- Untersagt sind unter anderem die Entwicklung konkurrierender Modelle auf Basis des Dienstes und das Reverse Engineering bzw. Extrahieren der Modellkomponenten. Google beansprucht laut Zusatzbedingungen kein Eigentum an generierten Inhalten. Quellen: Q10 · Q11 Google APIs Terms, Abruf 20.10.2026.
- In den geprüften Gemini API Additional Terms und den allgemeinen Google APIs Terms wurde kein ausdrückliches generelles Verbot gefunden, selbst erstellte Qualitäts-/Latenzbenchmarks zu veröffentlichen. Daraus folgt keine uneingeschränkte Veröffentlichungsfreigabe: Produkt-, Cloud- und ggf. individuell vereinbarte Vertraulichkeits- und Nutzungsbedingungen sind zu prüfen. Quellen: Q10 · Q11, Abruf 20.10.2026.
- Google erklärt, dass durch das Modell generierte Audioausgaben mit dem unhörbaren SynthID-Wasserzeichen markiert werden. Quelle: Q4, Abruf 20.10.2026.

## Preis / Abrechnung

Gemini Developer API (öffentlich ausgewiesener Standardtarif):

| Leistung | Free Tier | Paid Tier |
| --- | --- | --- |
| Audioeingabe | kostenlos innerhalb verfügbarer Limits | 3,50 USD / 1 Mio. Input-Audiotoken ≈ 0,0053 USD/Inputminute (Google-Angabe; eigene Rechnung 0,00525 USD) |
| Audioausgabe | kostenlos innerhalb verfügbarer Limits | 21,00 USD / 1 Mio. Output-Audiotoken ≈ 0,0315 USD/Outputminute |

Google verwendet für die Abrechnung 25 Audiotoken pro Sekunde und nennt einen effektiven Preis von ca. 0,0368 USD pro Minute (eigene Rechnung bei jeweils einer Minute Eingabe und Ausgabe: ca. 0,03675 USD). Die reale Abrechnung erfolgt anhand der tatsächlich verbrauchten Input- und Output-Audiotoken, nicht anhand einer pauschalen Gesprächsminute. Quelle: Q8 Preisübersicht, Abruf 20.10.2026.

Rechenbeispiel, keine Preisgarantie: 10-Minuten-Telefonat mit zwei durchgehend gestreamten Eingangskanälen (zusammen 20 Minuten Input) und insgesamt 10 Minuten synthetisierter Übersetzung: 20 × 0,00525 + 10 × 0,0315 = 0,42 USD. Infrastruktur, Telefoniekosten, abweichende Sprechzeit und tatsächliche Tokenzählung nicht berücksichtigt. Eigene Modellrechnung anhand Q8, Abruf 20.10.2026.

Für Agent Platform / Enterprise können andere Vertrags- und Abrechnungsbedingungen gelten; die obigen Werte sind ausdrücklich Gemini Developer API-Preise. Quellen: Q8 · Q5, Abruf 20.10.2026.

## 10. Terminologie-/Kontextanpassung

Bewertung: native Terminologie-/Kontextanpassung für dieses Modell nicht unterstützt.

- Google grenzt Live Translation ausdrücklich vom Live-Agent-Betrieb ab: nur Übersetzung, keine Tools oder Instructions. Über die dokumentierte translationConfig werden u. a. targetLanguageCode und echoTargetLanguage gesetzt. Quelle: Q1, Abruf 20.10.2026.
- Keine Unterstützung für System Instructions, Tuning, RAG Engine oder Context Caching ist in der Cloud-Modellübersicht für die konkrete Live-Translate-Modell-ID ausgewiesen. Quelle: Q5, Abruf 20.10.2026.
- Native Glossare, Vocabulary Boosting, terminologiespezifische Wortlisten oder ein freier Kontextprompt sind für das konkrete Live-Translate-Modell in den geprüften Herstellerunterlagen nicht dokumentiert. Quellen: Q1 · Q5, Abruf 20.10.2026.
- Das separate Modell Gemini 3.5 Transcribe unterstützt Custom Vocabulary Biasing; dieses Merkmal ist nicht auf Gemini 3.5 Live Translate übertragbar. Quelle: Q8 – Preisseite, Abschnitt Gemini 3.5 Transcribe, Abruf 20.10.2026.
- Die fehlerfreie Übertragung von Produktnamen, Namen, Vertragsnummern, IBAN und Fachterminologie ist daher nicht garantiert und benötigt separate Qualitätstests. Prüfpunkt/technische Ableitung, kein bereits gemessener Qualitätsmangel.

## Dokumentationsabweichungen und offene Punkte

- Texteingabe: DeepMind-Model-Card nennt „Audio und Text“; Live-Translate-API erlaubt für Übersetzung ausschließlich Audio. Für die Anwendung zählt die API-Spezifikation. Q3 · Q1.
- Agent-Platform-Schnittstelle: Cloud-Seite beschreibt Live-WebSockets, kennzeichnet „Gemini Live API“ in ihrer Capability-Tabelle aber als nicht unterstützt. Zugang über diesen Bereitstellungsweg nicht allein aus der Tabelle ableiten. Q5.
- Release-Daten: Entwickler-Preview seit Juni 2026 angekündigt; Agent-Platform-Modellseite nennt August 2026. Unterschiedlicher Veröffentlichungs-/Bereitstellungskontext. Q4 · Q5.
- Latenz: „Wenige Sekunden“ als qualitative Herstellerbehauptung, keine veröffentlichten verifizierbaren Durchschnitts-/P95-Werte in der Evaluation. Q4 · Q6.
- Session-Limits: Ca. 10 Minuten Verbindung und 15 Minuten unkomprimiertes Audio nur allgemein für Live API dokumentiert; Translate-Kompatibilität der Verlängerungsmechanismen nicht separat bestätigt. Q7.
- Region / Datenschutz: Keine EU-/DE-exklusive Verarbeitung belegt; vor Tests mit echten Kundendaten gesonderte Datenschutz- und Vertragsprüfung. Q5 · Q9 · Q10.
- Telefonie: G.711-Adapter, 8-kHz-Qualität, Unterbrechungen und zwei parallele Richtungen noch nicht anhand eigener Messungen getestet. Q1.

## Hauptstärken

- Kontinuierliche Speech-to-Speech-Übersetzung ohne getrennt aufzubauende ASR-/MT-/TTS-Services.
- 70+ Sprachen, darunter alle sieben VIER-Kernsprachen; automatische Quellsprachidentifikation.
- Dokumentierte Live-/WebSocket-Schnittstelle, Google AI Studio, Entwicklerbeispiele und öffentlicher Preis.
- Preview-Kandidat ausdrücklich passend zur Aufnahme neuer, technisch interessanter Ansätze.

## Haupteinschränkungen

- Kein bestätigter EU-/DE-exklusiver Verarbeitungsstandort für diese konkrete Modell-ID.
- Preview-Status, keine belastbaren veröffentlichten End-to-End-Latenzwerte.
- Keine native G.711-/8-kHz-Telefonieeingabe; Medienkonvertierung erforderlich.
- Keine dokumentierte native Glossar- oder Promptsteuerung; kein Self-Hosting.
- Sitzungsdauer, stabile Parallelität und simultane Zweiwege-Telefonie modellbezogen zu testen.

Longlist-Entscheidung: Aufnehmen. Funktionale Live-S2ST-, Sprach- und Streaming-Anforderungen erfüllt. Für eine produktive Contact-Center-Auswahl bleiben EU-Verarbeitung, Telefoniequalität, Latenz, Session-Stabilität und Fachterminologie offene Entscheidungspunkte.

## Quellenverzeichnis

| ID | Offizielle Quelle | Verwendung |
| --- | --- | --- |
| Q1 | [Google AI for Developers – Live Translation Guide](https://ai.google.dev/gemini-api/docs/live-api/live-translate) | Funktion, Schnittstelle, Sprachen, Audio, Konfiguration |
| Q2 | [Google – Modellseite Gemini 3.5 Live Translate](https://ai.google.dev/gemini-api/docs/models/gemini-3.5-live-translate-preview) | Modell-ID, Fähigkeiten, Limits, Batch/Live |
| Q3 | [Google DeepMind – Gemini 3.5 Audio Model Card (Fassung 26.08.2026)](https://deepmind.google/models/model-cards/gemini-3-5-audio/) | Basisarchitektur, Einschränkungen, Modalitäten |
| Q4 | [Google Blog – Gemini 3.5 Live Translate (09.06.2026)](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-live-3-5-translate/) | Veröffentlichung, Einsatzzweck, Pilot, SynthID |
| Q5 | [Google Cloud – Gemini 3.5 Live Translate](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-live-translate) | Cloud-Verfügbarkeit, Preview, Regionen, Funktionen |
| Q6 | [Google DeepMind – Evaluationsmethodik, PDF](https://storage.googleapis.com/deepmind-media/gemini/gemini_3-5_live-translate_model_evaluation.pdf) | Definition der Latenzmetriken und Testmethodik |
| Q7 | [Google – Live API Session Management](https://ai.google.dev/gemini-api/docs/live-session) | Allgemeine Session-Grenzen und Resumption |
| Q8 | [Google – Gemini API Pricing](https://ai.google.dev/gemini-api/docs/pricing) | Free/Paid, Audio-Tokenpreise, Abrechnung |
| Q9 | [Google Cloud – Deployments and Endpoints](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations); [Data Residency](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency) | Global-Endpunkt, Datenregion und Residenz |
| Q10 | [Google – Gemini API Additional Terms](https://ai.google.dev/gemini-api/terms) | Paid/Unpaid, EWR, Datennutzung, kommerzielle Regeln |
| Q11 | [Google – Google APIs Terms of Service](https://developers.google.com/terms) | Übergeordnete API-Nutzungsbedingungen |

Quellenabruf: 20.10.2026 (Nachprüfung 21.10.2026). Quellenstatus: Offizielle Herstellerunterlagen; eigene technische Ableitungen und offene Punkte sind gekennzeichnet. Herstellerangaben stellen keine unabhängigen Telefoniebenchmarks dar.
