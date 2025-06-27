# Dataset en preprocessing

Aangezien ons thema erg breed is en we elk standpunt en argument grondig willen onderbouwen, hebben we voor ieder perspectief aparte datasets geselecteerd. Hieronder beschrijven we per perspectief en per argument consequent welke datasets we gekozen hebben, hoe we deze hebben voorbewerkt en hoe de uiteindelijke visualisaties zijn opgebouwd.

---

## Perspectief 1: Verschillen in milieubeleid verklaren verschillen in landbouwuitstoot

### Argument 1 – Strenger milieubeleid resulteert in minder CO₂ per ton landbouwproduct

#### Stelling
Landen met een hoger Environmental Performance Index-cijfer (EPI) stoten gemiddeld minder CO₂-equivalenten uit per geproduceerde ton landbouwproduct.

#### Gebruikte datasets
- **FAOSTAT – Landbouwemissies**
- **FAOSTAT – Landbouwproductie**
- **Environmental Performance Index (EPI)**

#### Doel van de preprocessing
Niet de absolute uitstoot is doorslaggevend, maar de efficiëntie ervan. Door uitstoot te delen door productievolume kunnen landen eerlijker vergeleken worden, ongeacht schaal.

#### Stappen in het verwerkingsproces
1. **Aggregatie per land**  
   - Emissies opgeteld in *kiloton CO₂-eq.*  
   - Productie omgerekend van *ton* naar *kiloton*.
2. **Berekening van uitstoot per ton**  
   \[
   \text{uitstoot per ton} = \frac{\text{totale uitstoot (kt)}}{\text{totale productie (kt)}}
   \]
3. **Koppeling aan EPI-score**  
   EPI-scores toegevoegd om milieubeleid te relateren aan emissie-efficiëntie.
4. **Filtering van landen**  
   Alleen landen met ≥ 5 000 kt productie én ≥ 50 kt uitstoot zijn behouden.

#### Opbouw van de visualisatie
Een interactieve kaart met twee lagen:  
- **Achtergrondkleur (groentinten):** EPI-score.  
- **Stippen (percentielen 10–90 %):** CO₂ per ton landbouwproduct.

#### Resultaten
Landen met hoge EPI-scores vertonen doorgaans een lagere uitstoot per ton, wat suggereert dat streng milieubeleid effectief is.


### Argument 2 – Duurzaam gerichte subsidies verlagen landbouw-CO₂

#### Stelling
Landen die een groter deel van hun landbouwsubsidies aan duurzame doelen toewijzen, stoten relatief minder CO₂ uit binnen de landbouwsector.

#### Gebruikte datasets
- **FAOSTAT – Landbouwemissies**
- **OECD – Landbouwsubsidies (GSSE/TSE/PSE)**

#### Doel van de preprocessing
Onderzoeken of de verhouding \(\text{GSSE}/\text{TSE}\) (duurzaamheidsratio) samenhangt met de landbouwemissietrend.

#### Stappen in het verwerkingsproces
1. **Selectie relevante observaties**  
   - In FAOSTAT: *“Emissions (CO₂eq) (AR5)”*  
   - In OECD: GSSE, TSE en PSE.
2. **Landnaam-mapping**  
   Handmatige harmonisatie, bijv. *“China (People’s Republic of)” → “China”*.
3. **Aggregatie per land-jaar**  
   Subsidie- en emissiewaarden geaggregeerd en samengevoegd (inner join).
4. **Opschonen & export**  
   - Rijen met ontbrekende TSE verwijderd.  
   - Resultaat opgeslagen als *CO2.csv* met kolommen `Country`, `Year`, `CO2_Emission`, `GSSE`, `TSE`, `PSE`.

#### Opbouw van de visualisatie
Een gecombineerde grafiek:  
- **Blauwe staven:** duurzaamheidsratio (GSSE/TSE).  
- **Oranje lijn:** totale landbouw-CO₂ (kt) per jaar.

#### Resultaten
In de meeste landen correleert een stijgende duurzaamheidsratio met een daling of vertraging van de CO₂-groei; enkele afwijkingen bestaan.

---

## Perspectief 2: Economische welvaart en productiviteit beïnvloeden uitstoot

### Argument 1 – Rijkdom leidt niet automatisch tot schonere productie

#### Stelling
Een hoger bbp per hoofd correleert niet noodzakelijk met een lagere CO₂-uitstoot per ton voedselproductie.

#### Gebruikte datasets
- **FAOSTAT – Landbouwemissies**  
- **FAOSTAT – Landbouwproductie**  
- **World Bank – GDP per capita**  
- **Our World in Data – Population & Total CO₂**

#### Doel van de preprocessing
Twee kernindicatoren bepalen: (1) CO₂ per ton voedsel en (2) productie per hectare, gekoppeld aan welvaartsdata.

#### Stappen in het verwerkingsproces
1. **Aggregatie per land** (emissies, productie, areaal).  
2. **Berekening indicatoren**  
   \[
   \text{CO₂ per ton} = \frac{\text{CO₂ (kt)} \times 1000}{\text{Productie (ton)}}
   \]  
   \[
   \text{Efficiëntie} = \frac{\text{Productie (ton)}}{\text{Area (ha)}}
   \]
3. **Koppeling GDP & bevolking**  
4. **Filtering op complete data** (o.a. India, Japan, Brazilië, Nigeria, Zuid-Afrika).  
5. **Normalisatie (min-max) voor radarplot**.

#### Opbouw van de visualisatie
- **Radarplot:** vergelijkt zes landen op CO₂ per ton, GDP per capita, productievolume en bevolkingsgrootte.

#### Resultaten
Welvarende landen tonen uiteenlopende emissie-efficiënties; Japan en Nederland scoren hoog op GDP maar niet per se op uitstoot per ton.


### Argument 2 – Hogere land-efficiëntie gaat vaak samen met lagere CO₂ per ton

#### Stelling
Landbouwsystemen met een hogere opbrengst per hectare behalen doorgaans een lagere uitstoot per ton geproduceerd voedsel.

*(Datasets en preprocessing identiek aan Argument 1.)*

#### Opbouw van de visualisatie
- **X-as:** efficiëntie (ton/ha).  
- **Y-as:** CO₂ per ton.  
- **Puntkleur:** inkomensgroep; **puntgrootte:** productievolume.

#### Resultaten
Landen met intensieve teelt (bijv. Nederland) combineren hoge efficiëntie met lage uitstoot per ton, terwijl extensieve systemen (Nigeria) het omgekeerde profiel laten zien.

---

## Perspectief 3: De invloed van klimaat & geografie op landbouw-CO₂

### Argument 1 – Exportgerichtheid verhoogt uitstoot per inwoner

#### Stelling
Emissies voor exportproductie worden volledig in het producerende land geboekt, wat de landbouw-CO₂ per inwoner doet stijgen.

#### Gebruikte datasets
- **FAOSTAT – Detailed Trade Matrix**
- **FAOSTAT – Landbouwemissies**
- **World Bank – Population**

#### Doel van de preprocessing
Emissies koppelen aan exportvolume en normaliseren per inwoner en per geëxporteerde ton.

#### Stappen in het verwerkingsproces
1. **Aggregatie per land-jaar** (export, emissies).  
2. **Normalisatie**  
   - Export / Population  
   - Emissions / Population  
   - Emissions / Export ton
3. **Datacleaning** (lege waarden behouden als hiaten).

#### Opbouw van de visualisatie
Interactieve lijngrafiek met een dropdown voor vijf metrieke lijnen: export per inwoner, uitstoot per inwoner, exportvolume, totale uitstoot, uitstoot per exportton.

#### Resultaten
Patroon is duidelijk voor Argentinië (meer export → hogere uitstoot per inwoner), maar wijkt af in Brazilië en de VS wegens ontbossing resp. bevolkingsgrootte.


### Argument 2 – Klimaatzone bepaalt productmix en emissieprofiel

#### Stelling
Het lokale klimaat dicteert de rendabele productmix en daarmee het karakteristieke uitstootpatroon.

#### Gebruikte datasets (2022)
- **FAOSTAT – Production**
- **FAOSTAT – Landbouwemissies**
- **World Bank – Population**
- **combined_2022.csv** (klimaatgegevens: temperatuur & neerslag)

#### Doel van de preprocessing
Emissies en productmix koppelen aan klimaatgegevens om te tonen hoe klimaat de productkeuze en uitstoot beïnvloedt.

#### Stappen in het verwerkingsproces
1. **Selectie van landen** (Brazilië, Nederland, VS, Kenia).  
2. **Mapping naar 10 hoofdcategorieën** van ~150 producten.  
3. **Berekening indicatoren**  
   - Productie per categorie  
   - Totale emissies  
   - Emissions / Capita
4. **Harmonisatie klimaatdata** (maandtelling, vertaling, aggregatie).

#### Opbouw van de visualisatie
Een figuur met drie rijen:  
1. Balkgrafiek CO₂ per inwoner  
2. Polar-diagram productmix  
3. Dubbelas: neerslag (bars) & temperatuur (lijn)  
Dropdown voor landkeuze.

#### Resultaten
- **Nederland:** kassen & melkvee → hoge uitstoot per inwoner.  
- **Kenia:** bloemen & thee → zeer lage uitstoot.  
- **Brazilië:** soja & suikerriet → gemiddelde uitstoot.  
- **Verenigde Staten:** graan & vee; schaalvoordelen drukken uitstoot.
