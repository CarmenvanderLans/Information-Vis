# Dataset and preprocessing

Aangezien ons thema erg breed is en we elk standpunt en argument grondig willen onderbouwen, hebben we voor ieder perspectief aparte datasets geselecteerd. Hieronder beschrijven we per perspectief en argument uitgebreid welke datasets we gekozen hebben en hoe we deze hebben voorbewerkt.


## Perspectief 1: Verschillen in milieubeleid verklaren verschillen in landbouwuitstoot

### Argument 1: Landen met strenger milieubeleid stoten gemiddeld minder CO₂ uit per ton landbouwproductie

Voor deze visualisatie hebben we drie datasets uit 2022 gecombineerd die samen een goed beeld geven van landbouw en milieubeleid wereldwijd:

FAOSTAT Landbouwemissies: toont per land hoeveel broeikasgassen er in de landbouwsector worden uitgestoten.

FAOSTAT Landbouwproductie – geeft inzicht in hoeveel landbouwproducten landen produceren (in tonnen).

Environmental Performance Index (EPI) – een internationale score die laat zien hoe goed landen het doen op het gebied van milieubeleid.

### Doel van de preprocessing

We wilden niet alleen kijken naar hoeveel CO₂ een land uitstoot, maar juist naar hoe duurzaam die uitstoot is in verhouding tot wat er geproduceerd wordt. Daarom focust deze visualisatie niet op de totale uitstoot, maar op de uitstoot per geproduceerde ton landbouwproduct. Zo zie je niet alleen wie veel produceert, maar vooral wie dat op een relatief schone of juist vervuilende manier doet. Dat maakt het mogelijk om landen van verschillende omvang op een eerlijke manier met elkaar te vergelijken.

### Stappen in het verwerkingsproces

1. **Aggregatie per land**  
   Zowel de emissie- als productiedata bevatten waarden per landbouwproduct of subcategorie. In de eerste stap zijn deze gegevens samengevoegd tot een totaal per land:
   - Emissies zijn opgeteld in *kiloton CO₂-equivalenten*.
   - Productie is omgerekend van *ton* naar *kiloton*, zodat beide datasets dezelfde schaal gebruiken.

2. **Berekening van uitstoot per ton landbouwproductie**  
   Na aggregatie is per land de verhouding berekend tussen totale uitstoot en totale productie:
   \[
   \text{uitstoot per ton} = \frac{\text{totale uitstoot (kt)}}{\text{totale productie (kt)}}
   \]
   Deze indicator biedt een genuanceerder beeld dan absolute cijfers: een groot landbouwland met hoge uitstoot én hoge productie kan per ton relatief schoon zijn.

3. **Koppeling aan de EPI-score**  
   De gecombineerde dataset is vervolgens verrijkt met de EPI-score per land. Deze score geeft inzicht in hoe goed een land scoort op aspecten als milieubeleid, luchtkwaliteit, biodiversiteit en klimaatadaptatie. Zo kan de relatie tussen **duurzaam beleid en efficiëntie** van landbouwemissies worden geanalyseerd.

4. **Filtering van landen**  
   Om vertekening in de resultaten te voorkomen, zijn alleen landen meegenomen die:
   - minstens **5.000 kiloton aan landbouwproductie** rapporteren, én
   - minstens **50 kiloton aan landbouwuitstoot** hebben.
   Zo worden kleine landbouwlanden zoals Qatar uitgesloten, omdat die bij relatief kleine absolute cijfers tot extreem vertekende ratios kunnen leiden.

### Opbouw van de visualisatie

De uiteindelijke kaart bevat twee informatielagen:

- De **achtergrondkleur** (in groentinten) laat de EPI-score van elk land zien: hoe groener, hoe beter het milieubeleid.
- De **gekleurde stippen** geven de CO₂-uitstoot per ton landbouwproductie aan. Deze kleuren zijn gebaseerd op percentielen (10%–90%) om uitschieters minder dominant te maken.

Door deze twee dimensies te combineren, wordt zichtbaar welke landen erin slagen om sterk milieubeleid te combineren met een efficiënte landbouwsector — en welke landen daar juist in achterblijven.

