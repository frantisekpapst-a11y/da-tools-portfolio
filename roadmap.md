Blok 3 — Analytical Workflow Portfolio
Hlavní cíl

Samostatně navrhnout a realizovat analytické řešení od business problému až po automatizovaný a zdokumentovaný výstup.

Toto nebude pouze další lekce. Půjde o závěrečnou portfolio fázi.

Business Understanding
→ Architecture Decision
→ Data Acquisition
→ Data Quality
→ Data Preparation
→ Analysis
→ Data Model
→ Reporting
→ Interpretation
→ Automation
→ Delivery
→ Documentation

1 — Business Understanding
Témata
business kontext;
cílový uživatel;
rozhodnutí, které má analýza podpořit;
analytické otázky;
definice KPI;
rozsah projektu;
předpoklady;
omezení;
kritéria úspěchu.
Výstup
business-requirements.md

2 — Data Source Assessment
Témata
dostupné zdroje;
význam jednotlivých tabulek;
granularita;
datové typy;
objem dat;
historie;
frekvence změn;
kvalita;
přístupová omezení;
osobní a citlivé údaje.
Výstup
data-sources.md
data-dictionary.md

3 — Architecture Decision
Témata
výběr nástrojů;
role SQL;
role Pythonu;
role Power Query;
role Power BI;
forma úložiště;
datové vrstvy;
automatizace;
zamítnuté alternativy;
zdůvodnění přiměřenosti řešení.
Výstup
architecture.md

4 — Data Acquisition a Raw Layer
Témata
SQL extraction;
API;
CSV, JSON a Excel;
uchování původních dat;
timestamp načtení;
oddělení raw dat;
reprodukovatelnost;
dokumentace původu dat.
Výstup
data/raw/

5 — Data Quality, Cleaning a Validation
Témata
missing values;
duplicity;
datové typy;
neplatné hodnoty;
klíče;
referenční integrita;
časová návaznost;
business pravidla;
reconciliation;
audit změn;
validace před publikací.
Výstup
data-quality-report.md

6 — Transformation a Business Logic
Témata
filtrování;
joiny;
agregace;
výpočty;
business kategorizace;
rozdělení práce mezi SQL a Python;
příprava faktů a dimenzí;
Gold tabulky;
dokumentace transformačních pravidel.

7 — Exploratory Data Analysis
Témata
distribuce;
trendy;
porovnání skupin;
odchylky;
outliers;
vztahy mezi proměnnými;
segmentace;
formulace a ověřování hypotéz;
hledání relevantních business zjištění.

8 — Statistická analýza

Použije se pouze tehdy, když odpovídá business otázce.

Možná témata:

deskriptivní statistika;
korelace;
testování rozdílů;
intervaly spolehlivosti;
jednoduchá regrese;
interpretace statistického výsledku;
omezení a riziko nesprávného závěru.

Statistiku nebudeme přidávat pouze proto, aby projekt vypadal složitěji.

9 — Datový a sémantický model
Témata
granularita faktové tabulky;
faktové a dimenzní tabulky;
surrogate keys;
vztahy;
kardinalita;
kalendářní dimenze;
jednosměrné filtrování;
measures;
hierarchie;
formátování;
skrytí technických sloupců.

10 — KPI a DAX
Témata
základní míry;
poměrové ukazatele;
časové porovnání;
plán versus skutečnost;
dynamické filtrování;
správný kontext výpočtu;
popis business významu každé míry.

11 — Dashboard
Témata
cílová skupina;
informační hierarchie;
KPI karty;
trendy;
porovnání kategorií;
tabulkové detaily;
filtry a slicery;
tooltipy;
navigace;
čitelnost;
omezení počtu vizuálů;
podpora rozhodování.

12 — Interpretace a doporučení
Témata
hlavní zjištění;
business význam;
oddělení faktu od domněnky;
omezení analýzy;
rizika;
doporučení;
očekávaný přínos;
navržený další krok.
Hlavní zásada
Výsledek
≠ pouze číslo

Výsledek
= číslo + kontext + význam + doporučení

13 — Automation a Monitoring
Témata
převod procesu do .py;
scheduler;
pipeline;
validace vstupů;
error handling;
logging;
secrets;
bezpečná publikace;
Power BI refresh;
kontrola poslední aktualizace;
zachování posledního správného výstupu.

Automatizace se použije pouze u projektu, kde dává smysl opakované zpracování.

14 — Delivery a distribuce
Témata
Power BI;
Excel export;
CSV nebo Parquet;
databázová tabulka;
sdílení výsledku;
cílový uživatel;
frekvence distribuce;
oprávnění;
verze výstupu;
archivace.

15 — Dokumentace a GitHub
Povinné části README
business problém;
cílový uživatel;
datové zdroje;
použitá architektura;
role nástrojů;
datová kvalita;
transformační proces;
KPI;
analýza;
dashboard;
zjištění;
doporučení;
automatizace;
omezení;
návod ke spuštění;
struktura repozitáře.
Další dokumentace
requirements.txt;
.env.example;
.gitignore;
SQL skripty;
Python skripty;
datový slovník;
screenshoty dashboardu;
ukázkové výstupy.

16 — Finální kontrola projektu

Projekt zkontrolujeme z pohledu:

datového analytika;
BI specialisty;
hiring managera;
recruitera;
technické reprodukovatelnosti;
pravdivosti prezentovaných dovedností;
relevance pro juniorní pozice.

Prověříme:

zda každý nástroj má jasný účel;
zda projekt není zbytečně komplikovaný;
zda jsou KPI správně definována;
zda závěry vycházejí z dat;
zda lze projekt vysvětlit při pohovoru;
zda README odpovídá skutečné realizaci.
Portfolio projekty

Doporučuji vytvořit tři až čtyři větší end-to-end projekty. Každý nemusí používat všechny technologie.

Projekt 1 — SQL a Power BI

Hlavní důraz:
business analýza;
SQL;
datový model;
DAX;
management dashboard.

Projekt 2 — Python, API a automatizace

Hlavní důraz:
API;
Pandas;
validace;
automatické spuštění;
logging;
Power BI výstup.

Projekt 3 — Kombinovaný analytický projekt

Hlavní důraz:
více datových zdrojů;
SQL;
Python;
Power Query;
Power BI;
architektonické rozhodování;
automatizace.

Volitelný projekt 4 — Excel nebo business analýza

Hlavní důraz:
Excel;
Power Query;
analytická interpretace;
management reporting;
rychlé ad-hoc řešení bez zbytečné infrastruktury.
Konečný harmonogram
Modern Data Stack
→ dokončení současného bloku


3. Analytical Workflow

Dostane samostatné repo:

da-workflow
Úvodní teoretická část

Bude krátká a prakticky zaměřená:

postup od business otázky k řešení;
definice KPI;
posouzení zdrojů;
granularita;
výběr nástrojů;
datová kvalita;
analýza;
modelování;
reporting;
interpretace;
automatizace;
dokumentace;
kontrolní seznam dokončeného projektu.

Teorie nebude znovu podrobně vysvětlovat SQL, Pandas, DAX ani automatizaci. Bude fungovat jako metodika:

Jak správně vést celý analytický projekt?
Praktická část

Po metodice už budeme pracovat na jednotlivých projektech:

Projekt 1
→ zadání
→ řešení
→ review
→ dokončení

Projekt 2
→ zadání
→ řešení
→ review
→ dokončení

Projekt 3
→ zadání
→ řešení
→ review
→ dokončení


da-workflow
│
├── metodika Analytical Workflow
├── projektová šablona
└── praktické end-to-end projekty