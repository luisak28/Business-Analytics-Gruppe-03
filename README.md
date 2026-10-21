# Lieferverzögerung & Kundenzufriedenheit im E-Commerce

**Datenprojekt im Modul Business Analytics** – DHBW Bad Mergentheim, Digital Business Management  
**Gruppe 3:** Luisa Krenz & Sophia Menzel · **Dozent:** Thomas Burghardt · **Abgabe:** 26.10.2026

---

## Forschungsfrage

> Welchen Zusammenhang gibt es zwischen Lieferverzögerungen und Kundenbewertungen im E-Commerce und in welchen Regionen treten Verzögerungen besonders häufig auf?

| | Hypothese | Ergebnis |
|---|---|---|
| H1 | Verspätete Lieferungen werden schlechter bewertet als pünktliche. | ✅ bestätigt |
| H2 | Mit zunehmender Verzögerung sinkt die Bewertung. | ✅ bestätigt |
| H3 | Die Verspätungsquote unterscheidet sich zwischen den brasilianischen Bundesstaaten. | ✅ bestätigt |
| H4 | Auch die Gesamtlieferzeit steht mit der Kundenbewertung in Zusammenhang. | ✅ bestätigt (schwächer) |

## Wichtigste Ergebnisse

- **Pünktlich** gelieferte Bestellungen erhalten im Schnitt **4,29 Sterne**, **verspätete** nur **2,27 Sterne**. 54 % der verspäteten Bestellungen werden mit 1 Stern bewertet (pünktlich: 7 %).
- Die Bewertung sinkt mit jedem Tag Verspätung: 1–3 Tage zu spät → 3,3 Sterne, mehr als eine Woche zu spät → unter 2 Sterne.
- Die **Verspätungsquote** liegt insgesamt bei 6,7 %, schwankt aber je Bundesstaat zwischen **2,8 % (AM)** und **20,8 % (AL)**. Besonders betroffen sind der **Nordosten** und **Rio de Janeiro**.
- Die **Lieferzeit** hängt ebenfalls mit der Bewertung zusammen, aber schwächer. Entscheidend ist vor allem, dass der **angekündigte Termin eingehalten** wird.

**Empfehlung für Olist:** realistische Liefertermine ankündigen, Logistik im Nordosten und in RJ prüfen, Kunden bei Verzögerungen proaktiv informieren, Verspätungsquote je Bundesstaat als KPI überwachen.

## Daten

| | |
|---|---|
| Datensatz | Brazilian E-Commerce Public Dataset by Olist |
| Quelle | Olist (2018), Kaggle: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce |
| Lizenz | CC BY-NC-SA 4.0 |
| Zeitraum | 09/2016 – 10/2018, Brasilien, ca. 100.000 anonymisierte Bestellungen |
| Erhebungsart | Sekundärdaten |

Genutzt werden **3 von 9 Tabellen**:

| Datei | Inhalt | Schlüssel |
|---|---|---|
| `mgh-dbm25-gruppe03-olist_orders_dataset.csv` | Status, Kaufdatum, tatsächliches & geschätztes Lieferdatum | `order_id`, `customer_id` |
| `mgh-dbm25-gruppe03-olist_order_reviews_dataset.csv` | Bewertung (1–5 Sterne), Kommentar | `order_id` |
| `mgh-dbm25-gruppe03-olist_customers_dataset.csv` | Stadt, Bundesstaat | `customer_id` |

```
customers ──(customer_id)── orders ──(order_id)── order_reviews
```

**Datenbereinigung:** nur Bestellungen mit Status `delivered` und vorhandenem Lieferdatum (96.470 von 99.441); bei mehrfach bewerteten Bestellungen nur die zuletzt beantwortete Bewertung. Analysedatensatz: **95.824 Bestellungen**.

**Neu berechnete Variablen:**

| Variable | Bedeutung |
|---|---|
| `verzoegerung_tage` | tatsächliches minus geschätztes Lieferdatum (Kalendertage; negativ = früher) |
| `verspaetet` | `True`, wenn an einem späteren Tag als angekündigt geliefert wurde |
| `lieferzeit_tage` | Zeit vom Kauf bis zur Zustellung |
| `region` | Bundesstaat zugeordnet zu Norden, Nordosten, Mittelwesten, Südosten, Süden |

## Methodik

Aufbau nach den fünf Phasen einer statistischen Untersuchung (Vorbereitung, Datenerhebung, Aufbereitung, Analyse, Ergebnispräsentation).

| Hypothese | Verfahren |
|---|---|
| H1 | Gruppenvergleich, Mann-Whitney-U-Test, Effektstärke r |
| H2 | Klassenbildung, Spearman-Rangkorrelation, Kruskal-Wallis-Test |
| H3 | Verspätungsquote je Bundesstaat/Region, Chi²-Unabhängigkeitstest, Cramérs V |
| H4 | Klassenbildung, Spearman-Rangkorrelation (alle und nur pünktliche Bestellungen) |

Da die Bewertung ordinal skaliert und nicht normalverteilt ist, werden nichtparametrische Tests verwendet.

## Projektstruktur

```
├── README.md                                          ← diese Datei
├── Lieferverzoegerung_Kundenzufriedenheit.ipynb       ← Analyse (Jupyter Notebook)
├── requirements.txt                                   ← benötigte Python-Pakete
├── mgh-dbm25-gruppe03-Expose.pdf                      ← Exposé
├── mgh-dbm25-gruppe03-olist_orders_dataset.csv        ← Daten: Bestellungen
├── mgh-dbm25-gruppe03-olist_order_reviews_dataset.csv ← Daten: Bewertungen
└── mgh-dbm25-gruppe03-olist_customers_dataset.csv     ← Daten: Kunden
```

## Ausführen

1. Python 3.10 oder neuer installieren (oder Google Colab / Anaconda nutzen).
2. Repository klonen und in den Ordner wechseln:
   ```bash
   git clone https://github.com/luisak28/Business-Analytics-Gruppe-03.git
   cd Business-Analytics-Gruppe-03
   ```
3. Pakete installieren:
   ```bash
   pip install -r requirements.txt
   ```
4. Notebook starten und **Kernel → Restart & Run All** ausführen:
   ```bash
   jupyter notebook Lieferverzoegerung_Kundenzufriedenheit.ipynb
   ```

Die drei CSV-Dateien müssen im selben Ordner wie das Notebook liegen.

## Vibe Coding

Der Python-Code wurde mit der Methode **Vibe Coding** erstellt: Wir haben unsere Anforderungen (Forschungsfrage, Hypothesen, gewünschte Auswertungen und Diagramme) in Alltagssprache an ein Sprachmodell (**Claude**, Anthropic) gegeben. Den erzeugten Code haben wir ausgeführt, die Ergebnisse geprüft, die Interpretation überarbeitet und alles mit Git/GitHub versioniert.

**Erfahrungen:**
- ➕ Sehr schneller Einstieg in pandas, statistische Tests und Diagramme, die über unsere bisherigen Python-Kenntnisse hinausgehen.
- ➕ Der Code ist kommentiert und folgt dem Aufbau aus der Vorlesung.
- ➖ KI-Ergebnisse müssen kritisch geprüft werden, z. B. die Definition von „verspätet“, der Umgang mit doppelten Bewertungen und ob die Interpretation zu den Zahlen passt.
- ➖ Man muss den Code selbst verstehen, um Fehler zu erkennen und Fragen dazu beantworten zu können.

## Grenzen

- Korrelation ≠ Kausalität; Einflüsse wie Produktqualität, Verkäufer oder Preis wurden nicht untersucht.
- Nur zugestellte Bestellungen; stornierte/nicht gelieferte Bestellungen fehlen.
- Daten von 2016–2018 aus Brasilien – nur eingeschränkt übertragbar.
- Kleine Bundesstaaten (z. B. RR, AP) haben wenige Bestellungen, ihre Quoten sind unsicherer.
