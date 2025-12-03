# MasteryProjekt


# **Erweiterte Projektdokumentation: TravelTide – Kundensegmentierung für personalisierte Treueprogramme**

## **1. Einleitung**
TravelTide plant den Ausbau eines personalisierten Treueprogramms, das auf individuelle Reiseverhalten und Nutzungsprofile zugeschnitten ist.  
Notebook 1 übernimmt die Datenbereinigung und Feature-Entwicklung aus Millionen von Sessions, Buchungen und Nutzerdaten. Notebook 2 baut darauf auf und entwickelt eine Kundensegmentierung mittels verschiedener Clustering-Verfahren.

**Ziel:** Identifikation handlungsrelevanter Kundengruppen, die strategisch mit maßgeschneiderten Angeboten angesprochen werden können.

---

## **2. Methodische Vorgehensweise**

### **2.1 Stufe 1 – Datenaufbereitung (Notebook 1)**  
Bereits ausführlich dokumentiert; hier eine Zusammenfassung:

- Konsolidierung aller Datenquellen (Nutzer, Sessions, Flüge, Hotels)  
- Bereinigung fehlerhafter Einträge (z. B. doppelte Trip-IDs, negative Aufenthaltsdauern)  
- Definition aktiver Nutzer (mind. 7 Sessions seit 4.1.2023)  
- Entwicklung zentraler Verhaltensmerkmale:
  - reale Trips, Flüge, Hotels  
  - Stornoquote  
  - internationale vs. nationale Reisen  
  - durchschnittliche Klicks & Sitzungsdauer  
  - Hotelkosten, Flugkilometer  
  - Planungshorizont (Tage zwischen Buchung und Reise)  
- Export eines harmonisierten Datensatzes (**users_real.csv**) für die Clusteranalyse  

Damit liegen erstmals vergleichbare Kundenmerkmale vor, die eine Segmentierung ermöglichen.

---

### **2.2 Stufe 2 – Segmentierung mittels Clustering (Notebook 2)**

#### **2.2.1 Datenbereinigung & Feature Engineering**
- **Ausreißerbehandlung**  
  - Entfernung extremer Hotel-Ausgaben (`real_avg_money_spent_hotel ≤ 190 €`)
- **Feature-Auswahl**  
  - Verwendung ausschließlich numerischer und binär kodierter Variablen  
  - Ausschluss kategorialer Daten wie Herkunftsort  
- **One-Hot-Encoding**
  - `has_children`, `married`  
- **Skalierung**
  - StandardScaler für alle Features zur Vermeidung von Verzerrungen

---

## **3. Clustering-Methoden**

### **3.1 KMeans-Clustering**

#### **Bestimmung der Clusteranzahl**
- Elbow-Methode ergibt ein deutliches Knickverhalten bei **k = 4**

#### **Mit und ohne PCA**
- PCA reduziert korrelierte Dimensionen und erklärt ~95 % der Varianz mit wenigen Komponenten  
- Auch hier ergibt die Elbow-Methode **4 Cluster**

#### **Vergleich der Modelle**
| Modell              | Silhouette | Resultat             |
|---------------------|------------|-----------------------|
| KMeans ohne PCA     | 0.131      | gute Struktur         |
| KMeans mit PCA      | 0.140      | bestes Gesamtmodell   |

→ Ergebnis: **4 stabile und gut interpretierbare Cluster**

---

### **3.2 DBSCAN-Clustering**
- **Ohne PCA:** 9 Kleinstcluster  
- **Mit PCA:** 22 Cluster, starke Übersegmentierung  

→ **Für marktorientierte Segmentierung ungeeignet**

---

### **3.3 Auswahl des besten Modells**
**Gewinner:** **KMeans mit PCA (4 Cluster)**  
Begründung: Höchster Silhouette-Wert + beste Interpretierbarkeit.

---

## **4. Ergebnis: Identifizierte Kundensegmente**

### **Cluster 1 – „Junge Sparfüchse”**
**Verhalten**
- niedrigste Buchungsaktivität (Ø 1,8 Trips)  
- höchste Rabattnutzung  
- kurze Sessions & wenige Klicks  
- 0 % internationale Reisen  
- keine Hotelkosten  
- spontane Buchungen (Ø 0 Tage Vorlauf)

**Demografie**
- jung (~36 Jahre)  
- überwiegend unverheiratet  
- wenige Kinder  

**Strategie / Angebot**
- Last-Minute-Deals  
- starke Rabatte  
- „Buch jetzt – flieg morgen“-Kampagnen  

---

### **Cluster 2 – „Vielfliegende Geschäftsreisende“**
**Verhalten**
- höchste Aktivität: Ø 4,7 Trips, Ø 8,1 Flüge  
- hoher Hotelbedarf (Ø 4,5 Buchungen)  
- internationale Reisen (5,6 %)  
- planungsorientiert (~3,2 Tage Vorlauf)  
- lange Sessions & viele Klicks  

**Demografie**
- älter (~45 Jahre)  
- überwiegend Single/geschieden  
- geringe Kinderquote  

**Strategie / Angebot**
- Business-Upgrades  
- Lounge-Zugang  
- Firmenkundenpakete  
- Priority-Service  

---

### **Cluster 3 – „Familien-Heimurlauber“**
**Verhalten**
- moderate Trips (Ø 2,1)  
- kaum internationale Reisen  
- geringe Hotelausgaben  
- sehr kurzfristige Buchungen (0 Tage Vorlauf)

**Demografie**
- 100 % verheiratet (binäre Kodierung > 0,5)  
- 48 % mit Kindern  
- älter (~49 Jahre)

**Strategie / Angebot**
- Familienrabatte  
- kinderfreundliche Packages  
- flexible Umbuchungen  

---

### **Cluster 4 – „Gelegenheitsreisende“**
**Verhalten**
- ausgewogenes Reiseverhalten  
- moderate Rabatte, mittlere Internationalität  
- lange Sessions & viele Klicks  
- leichter Planungsvorlauf (~0,5 Tage)

**Demografie**
- durchschnittlich 44 Jahre  
- gemischter Familienstatus  

**Strategie / Angebot**
- Hotel+Flug-Kombideals  
- Wochenendtrips  
- flexible Preispakete  

---

## **5. Visualisierung & Profilanalyse**
Erstellte Visualisierungen:

- t-SNE-Darstellungen (2D)  
- Barplots der Cluster-Mittelwerte  
- Boxplots zentraler Verhaltensmerkmale  
- Heatmaps (z. B. Anteile verheiratet / mit Kindern)  
- umfassende Clusterprofile  

Diese bestätigen eine klare Trennung zwischen:

- preissensitiven jungen Nutzern,  
- reiseintensiven Geschäftsreisenden,  
- familienorientierten Kurzurlaubern,  
- gelegentlichen, vielseitigen Reisenden.

---

## **6. Handlungsempfehlungen**

### **1. Hochpersonalisierte Angebote**
- segmentbasierte Rabatte und Werbeaktionen  
- zielgruppenspezifische Kampagnen  

### **2. Struktur des Treueprogramms**
- „Miles Booster“ für Vielflieger  
- „Family Perks“ für Familien  
- „Youth Deals“ für sparorientierte Nutzer  

### **3. Produktstrategie**
- All-Inclusive-Pakete für Familien  
- flexible Tarife für spontane Reisende  
- attraktive B2B-Modelle für Geschäftsreisen  

### **4. Weitere Schritte**
- regelmäßiges Re-Clustering  
- Predictive Modeling (z. B. Churn)  
- Integration ins CRM für automatisierte Zielgruppensteuerung  

---

## **7. Fazit**
Durch die Kombination aus gründlicher Datenaufbereitung (Notebook 1) und einer leistungsfähigen Clustering-Analyse (Notebook 2) konnten vier klar unterscheidbare Kundensegmente identifiziert werden.

Diese ermöglichen:
- gezielte Marketingkampagnen  
- höhere Conversion im Treueprogramm  
- bessere Kundenzufriedenheit  
- datenbasierte Produktentwicklung  

Damit liefert das Projekt nicht nur analytische Erkenntnisse, sondern direkt umsetzbare Business-Strategien.

---

