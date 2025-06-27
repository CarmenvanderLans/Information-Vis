# Dataset en preprocessing

Aangezien ons thema erg breed is en we elk standpunt en argument grondig willen onderbouwen, hebben we voor ieder perspectief aparte datasets geselecteerd. Hieronder beschrijven we per perspectief en argument uitgebreid welke datasets we gekozen hebben en hoe we deze hebben voorbewerkt.

---

## Perspectief 1: Verschillen in milieubeleid verklaren verschillen in landbouwuitstoot

### Argument 1 – Landen met strenger milieubeleid stoten gemiddeld minder CO₂ uit per ton landbouwproductie

#### Gebruikte datasets  
- *FAOSTAT – Landbouwemissies* – toont hoeveel broeikasgassen landen uitstoten binnen de landbouwsector.  
- *FAOSTAT – Landbouwproductie* – geeft inzicht in hoeveel landbouwproducten landen produceren (in tonnen).  
- *Environmental Performance Index (EPI)* – een internationale score die laat zien hoe goed landen presteren op het gebied van milieubeleid.

#### Doel van de preprocessing  
We wilden niet alleen kijken naar hoeveel CO₂ een land uitstoot, maar juist naar hoe duurzaam die uitstoot is in verhouding tot wat er geproduceerd wordt. Daarom focust deze visualisatie niet op de totale uitstoot, maar op de uitstoot per geproduceerde ton landbouwproduct. Zo zie je niet alleen wie veel produceert, maar vooral wie dat op een relatief schone of juist vervuilende manier doet. Dat maakt het mogelijk om landen van verschillende omvang eerlijk met elkaar te vergelijken.

#### Stappen in het verwerkingsproces  
1. *Aggregatie per land*  
   Zowel de emissie- als productiedata bevatten waarden per landbouwproduct of subcategorie. Deze zijn samengevoegd tot een totaal per land:  
   - Emissies opgeteld in kiloton CO₂-equivalenten.  
   - Productie omgerekend van ton naar kiloton, zodat beide datasets dezelfde schaal gebruiken.  

2. *Berekening van uitstoot per ton landbouwproductie*  
   Na aggregatie is per land de verhouding berekend tussen totale uitstoot en totale productie:  
   \[
   \text{uitstoot per ton} = \frac{\text{totale uitstoot (kt)}}{\text{totale productie (kt)}}
   \]  

3. *Koppeling aan de EPI-score*  
   De gecombineerde dataset is verrijkt met de EPI-score per land. Zo kan de relatie tussen milieubeleid en emissie-efficiëntie worden geanalyseerd.  

4. *Filtering van landen*  
   Om vertekening in de resultaten te voorkomen, zijn alleen landen meegenomen die:  
   - minstens *5.000 kiloton landbouwproductie* rapporteren, én  
   - minstens *50 kiloton landbouwuitstoot* hebben.  

#### Opbouw van de visualisatie  
De uiteindelijke kaart bevat twee informatielagen:  
- De *achtergrondkleur* (in groentinten) toont de EPI-score: hoe groener, hoe beter het milieubeleid.  
- De *gekleurde stippen* geven de CO₂-uitstoot per ton landbouwproductie aan, gebaseerd op percentielen (10%–90%).  

Door deze twee dimensies te combineren, wordt zichtbaar welke landen sterk milieubeleid combineren met efficiënte landbouw — en welke landen daar juist in achterblijven.

### Argument 2 – Landen die duurzaamheid belonen via subsidies, stoten relatief minder CO₂ uit binnen hun landbouwsector.

#### Gebruikte datasets  
- *FAOSTAT – Landbouwemissies* – toont hoeveel broeikasgassen landen uitstoten binnen de landbouwsector.  
- *OECD – Landbouwsubsidiedataset* – bevat informatie over landbouwsubsidies van verschillende landen.

#### Doel van de preprocessing  
Het doel is om te onderzoeken of landen die een groter deel van hun subsidies toewijzen aan duurzame doeleinden, zoals onderzoek, milieubeleid en infrastructuur, ook relatief minder landbouwgerelateerde CO₂ uitstoten.  
Door de verhouding tussen GSSE en TSE te berekenen, ontstaat een duurzaamheidsmaat die eerlijk tussen landen vergeleken kan worden. Deze wordt gekoppeld aan de jaarlijkse CO₂-uitstoot van landbouw om trends in beleid en uitstoot in kaart te brengen.

#### Stappen in het verwerkingsproces  
1. *Filteren van relevante observaties*  
   - Uit de FAO-dataset zijn alleen rijen behouden met de indicator “Emissions (CO2eq) (AR5)”  
   - Uit de OECD-dataset zijn alleen de indicatoren GSSE, TSE en PSE geselecteerd  

2. *Mapping maken van landnamen*
   Omdat FAO en OECD verschillende naamconventies gebruiken, zijn landnamen gematcht via een handmatige mapping , bijvoorbeeld “China (People’s Republic of)” → “China”.

3. *Samenvatten per land en jaar*  
   - Subsidie- en emissiewaarden zijn per land en jaar geaggregeerd  
   - Dataframes zijn samengevoegd via een inner join op Mapped_Country en Year

5. *Opschonen en exporteren*  
   - Waarden met ontbrekende of nulwaardes in TSE zijn verwijderd  
   - De uiteindelijke dataset bevat:  
     - 'Mapped_Country, 'Year'
     - 'CO2_Emission'  
     - 'GSSE_Subsidy', 'TSE_Subsidy', 'PSE_Subsidy'
   - De dataset is opgeslagen als 'CO2.csv'

#### Opbouw van de visualisatie  
De uiteindelijke visualisatie combineert twee informatielagen in één interactieve grafiek:
- De blauwe staven tonen de duurzaamheidsratio (GSSE/TSE) — hoe hoger de balk, hoe groter het aandeel van subsidies dat naar duurzame ondersteuning gaat.
- De oranje lijn toont de totale CO₂-uitstoot uit landbouw (in ton CO₂-equivalent) per jaar.
Door deze twee lagen te combineren, wordt zichtbaar of een toename in duurzame subsidiëring samenhangt met een afname of juist toename van de landbouwuitstoot en welke landen hierin voor- of achterlopen.

---

# Perspectief 2: Economische welvaart en productiviteit beïnvloeden uitstoot

Voor dit perspectief zijn vier datasets gecombineerd om inzicht te geven in de relatie tussen landbouw, uitstoot, economische welvaart en efficiëntie:

* *FAOSTAT Landbouwemissies* – toont CO₂-uitstoot door landbouw per land (in kiloton).
* *FAOSTAT Landbouwproductie* – bevat per land hoeveel voedsel geproduceerd wordt (in ton).
* *World Bank GDP per capita* – laat de economische welvaart zien per hoofd van de bevolking.
* *OWID Data (Our World in Data)* – biedt aanvullende info zoals bevolkingsomvang en totale CO₂-uitstoot.

### Doel van de preprocessing

We wilden niet alleen weten hoeveel een land uitstoot, maar vooral hoe efficiënt en duurzaam dat gebeurt. Daarom zijn twee centrale indicatoren berekend:

1. *CO₂-uitstoot per ton voedsel*
   → Hoe vervuilend is de productie per geproduceerde hoeveelheid?

2. *Productie per hectare landbouwgrond*
   → Hoe efficiënt wordt het beschikbare land benut?

Deze maatstaven zijn gecombineerd met welvaartsdata (GDP per capita), zodat kan worden onderzocht of rijkere landen efficiënter en duurzamer produceren.

### Stappen in het verwerkingsproces

1. *Aggregatie per land*

   * Landbouwemissies: totale CO₂-uitstoot per land (uit energiegerelateerde landbouwactiviteiten).
   * Voedselproductie: som van alle gewassen en dierlijke productie per land, in ton.
   * Oogstoppervlak: totaal aantal hectares landbouwgrond per land.

2. *Berekening van kernindicatoren*

   * *CO₂ per ton*:

     $$
     \text{CO₂ per ton} = \frac{\text{CO₂ (kt)} \times 1000}{\text{Productie (ton)}}
     $$
   * *Efficiëntie (ton per hectare)*:

     $$
     \text{Efficiëntie} = \frac{\text{Productie (ton)}}{\text{Area (ha)}}
     $$

3. *Koppeling aan GDP en bevolkingsdata*
   Door GDP per capita en bevolkingsgrootte toe te voegen, ontstaat een breder profiel per land: rijkdom, schaal, uitstoot en efficiëntie.

4. *Filtering en selectie*
   Alleen landen met consistente data voor alle variabelen zijn opgenomen, waaronder India, Japan, Brazilië, Nigeria en Zuid-Afrika. Deze landen bieden een goede spreiding qua inkomen, schaal en productiemodel.

5. *Normalisatie & visualisatie*
   Voor de radarplot (Argument 1) zijn de variabelen geschaald (min-max) en gecombineerd in een polar chart.
   Voor het tweede argument is een interactieve scatterplot gemaakt (2015–2023) waarin efficiëntie wordt afgezet tegen CO₂ per ton. Daarbij     is per land ook de inkomensgroep weergegeven op basis van GDP per capita.

### Opbouw van de visualisaties

* *Radarplot (Argument 1)* – vergelijkt zes landen op vier assen: uitstoot per ton, GDP per capita, productievolume en bevolkingsgrootte. Hiermee wordt zichtbaar dat rijkdom niet altijd leidt tot schonere productie.
* *Scatterplot (Argument 2)* – laat per jaar (2015–2023) zien of een hogere productiviteit per hectare samenhangt met lagere uitstoot. Landen met een efficiëntere landbouw blijken meestal ook schoner te produceren, wat duidt op het belang van technologie en kennisdeling.

---

## Perspectief 3: De invloed van klimaat & geografie op landbouw-CO₂  

### Argument 1 – Internationale handels­stromen verhogen de uitstoot per inwoner  

#### Stelling  
Een land dat veel landbouwproducten exporteert, registreert relatief meer landbouw-CO₂ per inwoner, omdat alle emissies voor exportproductie in het producerende land worden geboekt.

#### Gebruikte datasets  
- *FAOSTAT – Detailed Trade Matrix* – exportvolume (ton) per land en jaar.  
- *FAOSTAT – Landbouwemissies* – totale landbouwemissies (kt CO₂-eq).  
- *World Bank – Population* – inwoners per land en jaar.

#### Doel van de preprocessing  
We wilden emissies koppelen aan de mate waarin een land voor de wereldmarkt produceert, en dat normaliseren per inwoner en per geëxporteerde ton.

#### Stappen in het verwerkingsproces  
1. *Aggregatie per land-jaar*  
   - Exporttonnages en emissies opgeteld.  

2. *Normalisatie*  
   - Export per Capita = export / population  
   - Emissions per Capita = emissions / population  
   - Emissions per Export Tonne = emissions (kg) / export (ton)  

3. *Schoonmaak*  
   - Jaren zonder waarden verwijderd.  
   - Databrekingen (bijv. Argentinië 1999) zichtbaar gelaten als lege segmenten.

#### Opbouw van de visualisatie  
Een interactieve dropdown-lijngrafiek met vijf metrieken:  
- Export per inwoner  
- Uitstoot per inwoner  
- Exportvolume  
- Totale landbouwuitstoot  
- Uitstoot per exportton  

Hiermee wordt zichtbaar dat het verwachte patroon (meer export leidt tot meer uitstoot per inwoner) wel opgaat voor Argentinië, maar afwijkt in Brazilië (ontbossing) en in de VS (verdunning door grote bevolking).


### Argument 2 – Klimaatzone bepaalt productmix → emissieprofiel  

#### Stelling  
Het lokale klimaat bepaalt welke gewassen en dierlijke producten rendabel zijn. Dit vertaalt zich in een kenmerkende landbouw-CO₂-uitstoot per inwoner.

#### Gebruikte datasets (2022)  
- *FAOSTAT – Production* – gewas- en veehouderijopbrengst.  
- *FAOSTAT – Landbouwemissies* – totale landbouwemissies.  
- *World Bank – Population* – inwoners per land.  
- *combined_2022.csv* – gemiddelde temperatuur en neerslag per maand.

#### Doel van de preprocessing  
We koppelden emissies en productmix aan klimaatgegevens om te tonen hoe klimaat de productkeuze en dus de uitstoot beïnvloedt.

#### Stappen in het verwerkingsproces  
1. *Selectie van landen*  
   Brazilië, Nederland, Verenigde Staten en Kenia – vier landen met contrasterende klimaten.  

2. *Productcategorie-mapping*  
   Ongeveer 150 FAO-producten herleid naar 10 hoofdcategorieën (Fruit, Granen, Vee, enz.).  

3. *Aggregatie en berekening van indicatoren*  
   - Productie per categorie.  
   - Totale emissies per land.  
   - Emissions per Capita = emissies (kt) / population.  

4. *Klimaatdata harmoniseren*  
   - Maand → datetime.  
   - Maandnamen vertaald.  
   - Berekening van temperatuurbereik en neerslagsom per jaar.

#### Opbouw van de visualisatie  
Eén gecombineerd figuur met drie rijen:  
1. *Balkgrafiek* – CO₂-uitstoot per inwoner.  
2. *Polar-diagram* – verdeling van de productmix per categorie.  
3. *Dubbelas-grafiek* – maandelijkse neerslag (bars) en temperatuur (lijn).  

Een dropdown maakt het mogelijk om te schakelen tussen de vier landen.

#### Resultaten  
- *Nederland* (koel, korte zomers) → kassen & melkvee ⇒ hoge CO₂-uitstoot per inwoner.  
- *Kenia* (mild, bimodale regen) → open-veld bloemen & thee ⇒ extreem lage uitstoot.  
- *Brazilië* (tropisch nat/droog) → soja & suikerriet ⇒ gemiddelde uitstoot.  
- *Verenigde Staten* (gematigd) → granen + vee; schaalvoordelen drukken CO₂-uitstoot per inwoner.