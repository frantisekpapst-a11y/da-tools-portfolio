# Case Study 07 — Energy Data Solution Design

## Účel případové studie

Tato case study představuje návrh datového a analytického řešení pro monitoring spotřeby energie, nákladů, rozpočtu a provozních odchylek napříč 12 pobočkami.

Cílem není implementovat konkrétní produkční platformu, ale navrhnout přiměřenou architekturu a zdůvodnit hlavní technologická rozhodnutí.

Hlavní důraz je na:

```text
Business Understanding
→ Source Assessment
→ Architecture Decision
→ ETL Design
→ Data Layers
→ Data Quality
→ Automation
→ Governance
→ Reporting
```

---

## Business scénář

Firma provozuje 12 poboček a potřebuje pravidelně sledovat spotřebu energie a související náklady.

Hlavním cílem je:
- sledovat plnění rozpočtu;
- identifikovat pobočky nad a pod rozpočtem;
- sledovat vývoj spotřeby a nákladů;
- odhalovat neobvyklé zvýšení spotřeby;
- umožnit přechod ze souhrnného pohledu do detailu pobočky.

Primární uživatel:
- top management.

Další uživatelé:
- controlling;
- facility / energy management;
- branch managers.

Hlavní KPI:
- skutečné náklady;
- rozpočet;
- odchylka od rozpočtu v Kč;
- odchylka od rozpočtu v %;
- spotřeba energie;
- počet poboček nad rozpočtem.

Požadovaná frekvence:
```text
Refresh
→ daily

Maximálně akceptované zpoždění:
→ 1 day
```

Požadovaná historie:
```text
Současný rok
+
Předchozí rok
```

---

## Datové zdroje

Navržené řešení pracuje s několika typy vstupů:
```text
SQL
→ odečty spotřeby

Energy API
→ cenová data

Weather API
→ počasí

Excel
→ rozpočty

CSV / Parquet
→ historická data
```

### SQL databáze s odečty

SQL představuje hlavní zdroj spotřebních dat.

Před zapojením do řešení je potřeba potvrdit zejména:
- frekvenci aktualizace;
- granularitu odečtů;
- dostupnou historii;
- objem a růst dat;
- pokrytí všech poboček a odběrných míst;
- zpětné opravy;
- vlastníka zdroje.

### Energy API

U API je potřeba ověřit:
- co přesně poskytuje;
- frekvenci aktualizace;
- granularitu;
- jednotky;
- časovou zónu;
- autentizaci;
- limity volání;
- dostupnost a chybovost služby.

Výpočet skutečných nákladů nebude zjednodušen pouze na:

```text
spotřeba × cena
```

Přesná business pravidla pro cenové, tarifní a případné fixní složky musí být potvrzena.

### Weather API

Weather API bude sloužit pro analýzu vztahu mezi počasím a spotřebou.

Vazba:

```text
Branch
→ Coordinates
→ Weather Data
```

Je potřeba ověřit správné souřadnice, granularitu, historii, jednotky a časovou zónu.

### Excel s rozpočty

U Excel zdroje je potřeba ověřit:
- stabilitu struktury;
- granularitu rozpočtu;
- data ownera;
- administrátora souboru;
- počet editorů;
- způsob správy verzí.

### Historické CSV / Parquet

Historické soubory budou použity pro doplnění starší historie.

Je potřeba ověřit:
- původ;
- časové pokrytí;
- konzistenci struktury;
- datové typy a jednotky;
- duplicity a mezery;
- překryv se současným SQL zdrojem.

Pokud se období překrývají, musí být určena priorita zdroje.

---

## Cílová architektura

Hlavní datový tok:
```text
SQL ────────────────┐
Energy API ─────────┤
Weather API ────────┤
Excel rozpočty ─────┼→ Python ETL
CSV / Parquet ──────┘
                         ↓
                 Analytická SQL databáze
                         ↓
                    Power Query
                         ↓
                 Power BI sem. model
                         ↓
                       DAX
                         ↓
                 Management reporting
```

Python je zvolen jako společná ETL a lehká orchestration vrstva.

SQL databáze je hlavní analytické úložiště.

Power BI představuje sémantickou a reportingovou vrstvu.

---

## Ingestion a ETL

Pro všechny hlavní zdroje je zvolena jednotná Python vrstva:
```text
SQL
→ Python

Energy API
→ Python

Weather API
→ Python

Excel
→ Python

Historical CSV / Parquet
→ Python
```

Důvody:
- více heterogenních zdrojů;
- potřeba práce s API;
- jednotná validace;
- error handling;
- logging;
- možnost automatizace;
- menší roztříštěnost logiky mezi nástroje.

Python pipeline bude rozdělena do samostatných tasků.

```text
SQL Task
Energy API Task
Weather API Task
Excel Task
History Task
        ↓
Transformation
        ↓
Data Quality
        ↓
Load
```

Nezávislé source tasky mohou běžet paralelně.

---

## Volba úložiště

Hlavním analytickým úložištěm bude SQL databáze.
```text
Zdroje
→ Python ETL
→ Analytical SQL Database
→ Power BI
```

SQL bude obsahovat logicky oddělené vrstvy:
```text
Raw
→ Staging
→ Silver
→ Analytical
```

Raw odpovědi z API budou zároveň archivovány v původním JSON formátu pro auditovatelnost a případné znovuzpracování.

CSV a Parquet mohou být využity pro historická nebo archivní data.

Pro současný rozsah nejsou potřeba:
```text
Data Warehouse
Data Lake
Lakehouse
```

---

## Rozdělení transformačních rolí

Jednotlivé nástroje mají jasně oddělenou odpovědnost.

### Python
```text
→ ingestion
→ validation
→ cleaning
→ standardization
→ source integration
→ business transformations
→ logging
→ orchestration
```

### SQL
```text
→ storage
→ views
→ filtering
→ simple joins
→ stable aggregations
→ reporting layer
```

### Power Query
```text
→ load z SQL views
→ kontrola datových typů
→ minimální technické úpravy
```

### Power BI model
```text
→ fact / dimension struktura
→ vztahy
→ datová tabulka
→ hierarchie
```

### DAX
```text
→ KPI
→ odchylky
→ YoY
→ context-dependent measures
```

PySpark nebude použit, protože současný objem dat nevyžaduje distribuované zpracování.

---

## Datové vrstvy

Datový tok bude rozdělen na několik vrstev:
```text
Raw
→ Staging
→ Silver
→ Gold / Analytical
→ Semantic Model
→ Reporting
```

### Raw

Obsahuje data co nejblíže původním vstupům.

### Staging

Technické sjednocení:
- názvy sloupců;
- datové typy;
- datum a čas;
- business klíče;
- identifikace poboček a odběrných míst;
- jednotky.

### Silver / Curated

Obsahuje vyčištěná a validovaná data.

Proběhne zde:
- validace struktury;
- validace hodnot;
- cleaning;
- kontrola business pravidel;
- řešení duplicit;
- kontrola klíčů a vazeb.

### Gold / Analytical

Vrstva obsahuje:
- faktové tabulky;
- dimension tabulky;
- potřebné joiny;
- stabilní agregace;
- reportingové tabulky.

### Semantic Model

Power BI model bude obsahovat:
- vztahy;
- hierarchie;
- míry;
- KPI;
- business logiku.

---

## Návrh analytického modelu

Jedna pobočka může mít více odběrných míst, proto musí být tato úroveň explicitně zachována.

Základní návrh:
```text
DimBranch
→ branch_id
→ branch_name
→ location

DimMeteringPoint
→ metering_point_id
→ branch_id
→ další atributy odběrného místa

FactMeterReading
→ metering_point_id
→ reading_date
→ VT_reading
→ NT_reading
```

Další analytické entity mohou zahrnovat:
```text
FactBudget
FactEnergyCost

DimDate
DimPriceType
```

Přesná struktura bude záviset na potvrzených business pravidlech a granularitě dostupných dat.

---

## Data Quality

Data Quality kontroly budou definovány podle konkrétních tabulek a zdrojů.

U odečtů budou kontrolovány zejména:
- `metering_point_id`;
- datum odečtu;
- VT / NT hodnoty;
- duplicity podle business klíče;
- platná vazba odběrného místa na pobočku;
- neplatné nebo podezřelé hodnoty.

Business klíč odečtu může být například:
```text
metering_point_id
+
reading_date
```

Reconciliation bude sledovat například:
```text
Raw rows
vs.
Valid + Rejected rows
```

a rozdíly důležitých součtů před a po transformaci.

Publikační stavy:
```text
SUCCESS
→ publikace povolena

WARNING
→ publikace povolena s viditelným omezením

FAILED
→ publikace zastavena
```

---

## Automatizace a monitoring

Pipeline bude spuštěna denním časovým triggerem.
```text
Trigger
→ denně

Plánovač
→ Windows Task plánovač
```

Windows Task plánovač určuje čas spuštění.

Python řídí pořadí tasků a jejich závislosti.

Základní pořadí:
```text
Zdrojové tasky
→ transformace
→ kvalita dat
→ SQL load
→ Kontrola stavu
→ Power BI aktualizace
```

Retry bude použit pouze pro dočasné technické chyby, například:
- API timeout;
- krátkodobý výpadek připojení;
- dočasně nedostupný zdroj.

Při kritické chybě:
```text
FAILED
→ zastaví následné kroky
→ log error
→ blokuje Power BI aktualizaci
```

Log bude obsahovat minimálně:
```text
start_time
end_time
status
message
error_code
rows_processed
exit_code
```

Secrets budou uloženy mimo kód, například v `.env` nebo environment variables.

Monitoring bude sledovat:
```text
last_run_time
last_success_time
pipeline_status
failed_task
warning_count
rows_processed
```

Power BI refresh proběhne v plánovaný čas s rezervou po ETL pipeline a pouze pokud:
```text
SQL load completed
Kvalita dat = SUCCESS / případně WARNING
Aktualizace dat je acceptable
```

---

## Governance a odpovědnosti

Navržené rozdělení rolí:
```text
Data Owner
→ Facility / Energy Management

Source System Owner
→ IT

Pipeline Owner
→ Data Analyst / Analytics Engineer

Data Quality
→ společná odpovědnost

Semantický Model Owner
→ Data Analyst / BI Developer

Report Owner
→ Top Management

Report Administrator
→ Data Analyst / BI Developer
```

Kvalita dat je rozdělena mezi business a technické role:
```text
Facility / Energy Management
→ business pravidla

Data Analyst / Analytics Engineer
→ technické validace a pipeline

Data Analyst / BI
→ analytická konzistence
```

Data Analyst bude mít podle potřeby přístup k:
- zdrojovým datům;
- Python pipeline;
- analytické SQL databázi;
- sémantickému modelu;
- Power BI reportu.

Přístupy budou řízeny podle principu least privilege.

---

## Přiměřenost architektury

Navržené řešení používá:
```text
Python
SQL
Power Query
Power BI
Windows Task Scheduler
JSON / CSV / Parquet archive
```

Každý nástroj má jasnou roli.
```text
Windows Task plánovač
→ scheduling

Python
→ ETL + orchestrace

SQL
→ uložení analytické vrstvy

Power Query
→ lehká příprava

Power BI + DAX
→ model + reporting
```

Data ze zdrojového SQL nebudou automaticky načítána celá. Kde je to vhodné, bude použito filtrování už ve zdroji.

Současný rozsah nevyžaduje:
```text
Cloud
Spark
Data Lake
Lakehouse
```

Architektura je navržena jako nejjednodušší řešení, které pokrývá současné business a provozní požadavky.

---

## Zamítnuté alternativy

### Power Query jako hlavní ETL

Zamítnuto, protože:
- řešení kombinuje více heterogenních zdrojů;
- obsahuje API;
- vyžaduje logging, retry a error handling;
- Python lépe sjednotí celý ETL proces.

### Data Warehouse

Pro současný rozsah zatím není potřeba. Analytická SQL databáze pokryje současné potřeby s nižší provozní složitostí.

### Data Lake / Lakehouse

Nejsou potřeba vzhledem k současnému objemu, charakteru a frekvenci dat.

### Spark / PySpark

Nebude použit, protože SQL a Python výkonově dostačují.

### Cloud orchestrace

Nebyla zvolena, protože současný návrh počítá s lokálním / on-prem prostředím a jednoduchým denním batch procesem.

### Direct Power BI připojení ke všem zdrojům

Zamítnuto kvůli:
- rozptýlení transformační logiky;
- horší auditovatelnosti;
- horšímu error handlingu;
- složitější správě.

---

## Kdy architekturu přehodnotit

Doporučení by bylo vhodné znovu posoudit při:
- výrazném růstu počtu poboček a odběrných míst;
- vyšší granularitě odečtů;
- výrazném růstu objemu historie;
- větším počtu reportů a uživatelů;
- požadavku na téměř real-time zpracování;
- vyšších SLA požadavcích;
- zapojení více business domén;
- zavedení centrální cloudové datové platformy.

---

## Výsledný návrh

Finální architektura:
```text
SQL čtení ──────────┐
Energy API ─────────┤
Weather API ────────┤
Excel rozpočty ─────┼→ Python ETL / Orchestration
CSV / Parquet ──────┘
                         ↓
                  Raw / Staging
                         ↓
                       Silver 
                         ↓
                  Gold / Analytical
                         ↓
               Analytical SQL Database
                         ↓
                     Power Query
                         ↓
                 Power BI Model
                         ↓
                        DAX
                         ↓
              Management Dashboard
```

Automatizace:
```text
Windows Task Scheduler
→ Python pipeline
→ Data Quality
→ SQL Load
→ Monitoring
→ conditional Power BI refresh
```

---

## Závěr

Case study ukazuje návrh end-to-end datového řešení bez nutnosti použít všechny dostupné technologie.

Výsledná architektura staví na:
- Pythonu jako ETL a orchestration vrstvě;
- SQL jako hlavním analytickém úložišti;
- oddělených datových vrstvách;
- data kvality kontrolách;
- řízené automatizaci a monitoringu;
- jasném governance modelu;
- Power BI jako sémantické a reportingové vrstvě.

Součástí návrhu je také vědomé odmítnutí pokročilejších technologií, které by pro současný rozsah nepřinesly odpovídající business hodnotu.

Projekt je koncepční solution design. Nejde o tvrzení, že byla popsaná architektura nasazena v produkčním prostředí.