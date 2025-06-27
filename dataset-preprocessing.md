# Dataset en preprocessing

Aangezien ons thema erg breed is en we elk standpunt en argument grondig willen onderbouwen, hebben we voor ieder perspectief aparte datasets geselecteerd. Hieronder beschrijven we per perspectief en per argument welke datasets we gekozen hebben, hoe we deze hebben voorbewerkt en hoe de uiteindelijke visualisaties zijn opgebouwd.


## Perspectief 1: Verschillen in milieubeleid verklaren verschillen in landbouwuitstoot

### Argument 1 – Strenger milieubeleid resulteert in minder CO₂ per ton landbouwproduct

**Stelling**  
Landen met een hoger Environmental Performance Index‑cijfer (EPI) stoten gemiddeld minder CO₂‑equivalenten uit per geproduceerde ton landbouwproduct.

**Gebruikte datasets**  
- FAOSTAT – Landbouwemissies  
- FAOSTAT – Landbouwproductie  
- Environmental Performance Index (EPI)

**Doel van de preprocessing**  
Niet de absolute uitstoot is doorslaggevend, maar de efficiëntie ervan. Door de uitstoot te delen door het productievolume kunnen landen eerlijker worden vergeleken.

**Stappen in het verwerkingsproces**  
1. **Aggregatie per land**: emissie-, productie- en areaaldata samengevoegd.  
2. **Berekening van CO₂ per ton**:  
   $
   \text{CO₂ per ton} = \frac{\text{totale uitstoot (kt)} \times 1000}{\text{totale productie (ton)}}
   $
3. **Berekening van efficiëntie**:  
   $
   \text{Efficiëntie (ton/ha)} = \frac{\text{totale productie (ton)}}{\text{area (ha)}}
   $
4. **Koppeling aan EPI-score**: EPI-scores toegevoegd om milieubeleid te relateren aan emissie-efficiëntie.  
5. **Filtering van landen**: alleen landen met ≥ 5 000 kt productie én ≥ 50 kt uitstoot behouden.

**Opbouw van de visualisatie**  
Een interactieve kaart met twee lagen: achtergrondkleur in groentinten voor de EPI‑score en stippen (percentielen 10–90 %) voor CO₂ per ton.

**Resultaten**  
Landen met hoge EPI‑scores vertonen doorgaans een lagere uitstoot per ton, wat wijst op effectiviteit van streng milieubeleid.

### Argument 2 – Duurzaam gerichte subsidies verlagen landbouw‑CO₂

**Stelling**  
Landen die een groter deel van hun landbouwsubsidies aan duurzame doelen toewijzen, stoten relatief minder CO₂ uit.

**Gebruikte datasets**  
- FAOSTAT – Landbouwemissies  
- OECD – Landbouwsubsidies (GSSE, TSE en PSE)

**Doel van de preprocessing**  
Onderzoeken of de duurzaamheidsratio samenhangt met de trend in landbouwemissies.

**Stappen in het verwerkingsproces**  
1. **Selectie relevante observaties** uit FAOSTAT en OECD.  
2. **Harmonisatie van landnamen**.  
3. **Aggregatie per land-jaar** via inner join.  
4. **Opschonen & export**: verwijderen van rijen met ontbrekende subsidies.

**Opbouw van de visualisatie**  
Gecombineerde grafiek met blauwe staven voor de GSSE/TSE-ratio en een oranje lijn voor de totale CO₂-uitstoot per jaar.

**Resultaten**  
In de meeste landen correleert een hogere duurzaamheidsratio met een afname of vertraging van de CO₂‑groei.

---

## Perspectief 2: Economische welvaart en productiviteit beïnvloeden uitstoot

### Argument 1 – Rijkdom leidt niet automatisch tot schonere productie

**Stelling**  
Een hoger bbp per hoofd correleert niet noodzakelijk met een lagere CO₂‑uitstoot per ton voedselproductie.

**Gebruikte datasets**  
- FAOSTAT – Landbouwemissies  
- FAOSTAT – Landbouwproductie  
- World Bank – GDP per capita  
- Our World in Data – Population & Total CO₂

**Doel van de preprocessing**  
Bepalen van CO₂ per ton voedsel en productie per hectare, gekoppeld aan welvaartsdata.

**Stappen in het verwerkingsproces**  
1. **Aggregatie per land**: emissie-, productie- en areaaldata samengevoegd.  
2. **Berekening van CO₂ per ton**:  
   $
   \text{CO₂ per ton} = \frac{\text{totale uitstoot (kt)} \times 1000}{\text{totale productie (ton)}}
   $
3. **Berekening van efficiëntie**:  
   $
   \text{Efficiëntie (ton/ha)} = \frac{\text{totale productie (ton)}}{\text{area (ha)}}
   $
4. **Koppeling van GDP en bevolkingsdata**.  
5. **Filtering op complete datasets**.  
6. **Normalisatie voor radarplot**.

**Opbouw van de visualisatie**  
Radarplot dat zes landen vergelijkt op CO₂ per ton, bbp per hoofd, productievolume en bevolkingsgrootte.

**Resultaten**  
Welvarende landen laten uiteenlopende emissie‑efficiënties zien.

### Argument 2 – Hogere land‑efficiëntie gaat vaak samen met lagere CO₂ per ton

**Stelling**  
Landbouwsystemen met een hogere opbrengst per hectare behalen doorgaans een lagere uitstoot per ton.

**Gebruikte datasets**  
Identiek aan Argument 1.

**Opbouw van de visualisatie**  
Scatterplot met de X-as als efficiëntie (ton/ha), de Y-as als CO₂ per ton, puntkleur voor inkomensgroep en puntgrootte voor productievolume.

**Resultaten**  
Intensieve landbouwlanden combineren vaak hoge efficiëntie met lage uitstoot per ton.

---

## Perspectief 3: De invloed van klimaat & geografie op landbouw‑CO₂

### Argument 1 – Exportgerichtheid verhoogt uitstoot per inwoner

**Stelling**  
Emissies voor exportproductie worden volledig in het producerende land geboekt, wat de CO₂‑uitstoot per inwoner verhoogt.

**Gebruikte datasets**  
- FAOSTAT – Detailed Trade Matrix  
- FAOSTAT – Landbouwemissies  
- World Bank – Population

**Doel van de preprocessing**  
Normaliseren van emissies en exportvolumes per inwoner.

**Stappen in het verwerkingsproces**  
1. **Aggregatie per land-jaar**: exportvolumes en emissies samengevoegd.  
2. **Berekening van export per capita**:  
   $
   \text{Export per capita} = \frac{\text{exportvolume (ton)}}{\text{bevolking}}
   $
3. **Berekening van emissies per capita**:  
   $
   \text{Emissies per capita} = \frac{\text{uitstoot (kt)} \times 1000}{\text{bevolking}}
   $
4. **Berekening van emissies per exportton**:  
   $
   \text{Emissies per exportton} = \frac{\text{uitstoot (kt)} \times 1000}{\text{exportvolume (ton)}}
   $
5. **Behoud van hiaten** (lege waarden).

**Opbouw van de visualisatie**  
Interactieve lijngrafiek met dropdown voor export per inwoner, uitstoot per inwoner, exportvolume, totale uitstoot en uitstoot per exportton.

**Resultaten**  
Patronen verschillen per land; bijvoorbeeld Argentinië toont een duidelijke samenhang, terwijl Brazilië en de VS hiervan afwijken.

### Argument 2 – Klimaatzone bepaalt productmix en emissieprofiel

**Stelling**  
Het lokale klimaat bepaalt de rendabele productmix en daarmee het uitstootpatroon.

**Gebruikte datasets**  
- FAOSTAT – Production  
- FAOSTAT – Landbouwemissies  
- World Bank – Population  
- combined_2022.csv (klimaatgegevens)

**Doel van de preprocessing**  
Koppelen van emissies en productmix aan klimaatgegevens.

**Stappen in het verwerkingsproces**  
1. **Selectie van vier casuslanden**.  
2. **Mapping naar hoofdproductcategorieën**.  
3. **Berekening van productie, emissies en emissies per capita**.  
4. **Aggregatie van klimaatgegevens per jaar**.

**Opbouw van de visualisatie**  
Drie rijen in één figuur: balkgrafiek CO₂ per inwoner, polar-diagram productmix en dubbelas-grafiek voor neerslag en temperatuur.

**Resultaten**  
- Nederland: kassen en melkvee → hoge uitstoot per inwoner.  
- Kenia: bloemen en thee → zeer lage uitstoot.  
- Brazilië: soja en suikerriet → gemiddelde uitstoot.  
- Verenigde Staten: graan en vee → lagere uitstoot door schaalvoordelen.
