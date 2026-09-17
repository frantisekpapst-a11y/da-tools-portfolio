# Solution Design Cheatsheet

Stručná reference pro návrh přiměřeného datového a analytického řešení.

Cílem není použít co nejvíce technologií, ale zvolit řešení odpovídající business potřebě, dostupným datům, provozním požadavkům a možnostem týmu.

---

# Business potřeba

Než začnu vybírat technologie, musím pochopit business problém.

Prověřit:

- kdo bude výstup používat;
- jaké rozhodnutí má podporovat;
- jaký problém firma řeší;
- jaké KPI a úroveň detailu jsou potřeba;
- jak často má být výstup aktualizován;
- jak čerstvá a přesná musí být data;
- jakou historii potřebujeme;
- co se má stát při nekompletních datech.

KPI mají vycházet z business otázek:

```text
Business Question
→ Metric
```

Rozlišit:

```text
požadovaná frekvence
vs.
skutečná dostupnost dat
```

Úroveň přesnosti může být například:

- orientační reporting;
- management / controlling;
- účetní nebo regulatorní reporting.

Historii určit podle účelu:

- aktuální reporting;
- meziroční srovnání;
- trend a sezónnost;
- anomaly detection;
- forecasting.

Pro nekompletní data definovat:

```text
SUCCESS
WARNING
FAILED
```

Výstup:

```text
Who?
Why?
What?
When?
How accurate?
History?
Failure tolerance?
```

---

# Posouzení datových zdrojů

U každého zdroje prověřit:

```text
Availability
Structure
Granularity
History
Volume
Growth
Reliability
Ownership
Infrastructure
```

## SQL

Prověřit:

- frekvenci aktualizace;
- granularitu a strukturu;
- datové typy;
- historii;
- objem a růst;
- pokrytí entit;
- zpětné opravy;
- infrastrukturu;
- data ownera.

Reliability:

- úplnost;
- včasnost;
- duplicity;
- neplatné hodnoty;
- opožděná data;
- výpadky;
- potvrzení úspěšného loadu.

## API

Technická stránka:

- dostupnost;
- chybovost;
- latence;
- výpadky;
- stabilita endpointů;
- rate limits;
- autentizace.

Datová stránka:

- co API skutečně poskytuje;
- granularita;
- historie;
- formát;
- jednotky;
- časová zóna;
- úplnost;
- zpětné změny historie.

Pokud data závisí na poloze, ověřit správné mapování entity na lokalitu.

## Excel

Prověřit:

- stabilitu struktury;
- data ownera;
- administrátora;
- počet editorů;
- oprávnění ke změnám;
- schvalování změn;
- umístění;
- platnou verzi;
- zpětné přepisování historie.

Hlavní rizika:

```text
ruční změny
+ více editorů
+ více verzí
= vyšší provozní riziko
```

## Historical Files

U CSV, Parquet a dalších souborů prověřit:

- původ a ownership;
- časové pokrytí;
- pokrytí entit;
- konzistenci struktury;
- datové typy a jednotky;
- duplicity a mezery;
- překryv se současným zdrojem;
- zpětné opravy;
- objem.

Při překryvu určit:

```text
Source Priority
→ authoritative source
```

## Domain Knowledge Gap

Neznámá business pravidla nehádat.

```text
Unknown business rule
→ identifikovat
→ určit vlastníka
→ vyžádat definici
→ teprve potom implementovat
```

---

# Volba ingestion

Cílem je dostat data ze zdrojů do řešení spolehlivě a opakovatelně.

Posoudit:

- typ a počet zdrojů;
- frekvenci načítání;
- potřebu validace;
- error handling a logging;
- návaznost na transformace;
- automatizaci;
- provozní složitost.

Typické možnosti:

```text
SQL
→ direct query / Python / Power Query

API
→ Python / Power Query

Excel / CSV / Parquet
→ Python / Power Query

Multi-source workflow
→ ETL pipeline
```

Pokud je více heterogenních zdrojů, může být výhodná jednotná ingestion vrstva.

## ETL

```text
Extract
→ načtení

Transform
→ validace
→ čištění
→ sjednocení
→ business transformace

Load
→ cílové úložiště
```

## Task vs. Pipeline

```text
Task
→ jeden konkrétní krok

Pipeline
→ více navazujících kroků
```

Větší Python řešení lze rozdělit:

```text
main.py
→ orchestrace

modules
→ ingestion
→ validation
→ transformation
→ load
```

Power Query je vhodný hlavně pro jednodušší reportingové workflow.

Python dává větší smysl při více zdrojích, API, složitější validaci, logování, retry a automatizaci.

---

# Volba úložiště

Zvolit úložiště podle:

- objemu a růstu dat;
- počtu zdrojů;
- historie;
- auditovatelnosti;
- počtu uživatelů a reportů;
- provozní složitosti;
- dostupného týmu.

Typické možnosti:

```text
Source
→ bez dalšího ukládání

CSV / Excel / Parquet
→ soubory, archiv, historie

SQL Database
→ relační analytické řešení

Data Warehouse
→ centrální analytická platforma

Data Lake
→ raw a různorodá data ve velkém objemu

Lakehouse
→ lake storage + analytická vrstva
```

Raw data uchovávat, pokud je potřeba:

- audit;
- reprocessing;
- kontrola původního vstupu;
- historie API odpovědí;
- ochrana proti změně zdroje.

Hlavní princip:

```text
Current need
→ simplest sufficient storage

Future scale
→ architecture can evolve
```

---

# Volba transformačního nástroje

Transformaci dělat tam, kde bude efektivní, kontrolovatelná a opakovatelná.

## Python

Vhodný pro:

- ingestion;
- validaci a cleaning;
- standardizaci;
- propojení zdrojů;
- business transformace;
- automatizaci a logging.

## SQL

Vhodný pro:

- filtrování;
- joins;
- views;
- integritu;
- stabilní agregace;
- reportingovou vrstvu.

## Power Query

Vhodný pro:

- načtení do Power BI;
- datové typy;
- drobné technické úpravy;
- jednoduché statické transformace.

## Power BI model

Patří sem:

- fact / dimension struktura;
- relationships;
- date table;
- hierarchie.

## DAX

Použít pro dynamické výpočty podle filter contextu:

- KPI;
- variance;
- variance %;
- YoY;
- podíly.

## PySpark

Použít až tehdy, když objem nebo charakter dat vyžaduje distribuované zpracování.

Hlavní princip:

```text
SQL / Python
→ stabilní a opakovatelná datová logika

Power BI / DAX
→ analytická logika závislá na kontextu reportu
```

---

# Návrh datových vrstev

```text
Raw / Bronze
→ původní data

Staging
→ technické sjednocení

Silver / Curated
→ vyčištěná a validovaná data

Gold / Analytical
→ data připravená pro analytiku

Semantic Model
→ vztahy, measures, KPI

Reporting
→ finální výstup
```

## Raw / Bronze

Účel:

- auditovatelnost;
- reprocessing;
- zachování původního vstupu.

## Staging

Typicky:

- názvy sloupců;
- datové typy;
- datum a čas;
- business keys;
- identifikace entit;
- jednotky;
- sjednocení starších a novějších struktur.

## Silver / Curated

Typicky:

- validace struktury a dat;
- cleaning;
- business rules;
- duplicity;
- missing values;
- kontrola klíčů a vazeb.

## Gold / Analytical

Typicky:

- joins;
- agregace;
- fact tables;
- dimension tables;
- reporting tables.

## Semantic Model a Reporting

Semantic Model:

- relationships;
- hierarchie;
- measures;
- KPI;
- business logika.

Reporting:

- management dashboard;
- drill-down;
- trend;
- odchylky;
- detailní analýza.

---

# Data Quality

Cílem je určit, zda jsou data dostatečně důvěryhodná pro další zpracování a publikaci.

Prověřit:

- povinné hodnoty;
- duplicity;
- klíče a vazby;
- rozsahy;
- business pravidla;
- reconciliation;
- podmínky publikace.

## Mandatory Fields

Definovat podle konkrétní tabulky a jejího business významu.

## Duplicity

Jedinečnost určovat podle business key, ne pouze podle shodného celého řádku.

```text
Business Key
→ určuje jedinečnost záznamu
```

## Klíče a vazby

Kontrolovat:

- primární klíče;
- cizí klíče;
- referenční integritu;
- vazby fact → dimension.

## Rozsahy a logika

Prověřit:

- NULL;
- neplatné záporné hodnoty;
- neobvyklé skoky;
- hodnoty mimo rozsah;
- porušení business pravidel.

Neobvyklá hodnota může být `WARNING`, ne automaticky chyba.

## Reconciliation

Ověřit, že se data neztratila, nezdvojila nebo nezměnila bez vysvětlení.

Porovnat například:

- vstupní a výstupní počty;
- rejected rows;
- duplicates;
- důležité součty;
- očekávané a načtené období.

## Provozní stavy

```text
SUCCESS
→ data jsou v pořádku

WARNING
→ omezení existuje, ale výstup lze použít

FAILED
→ data nejsou dostatečně důvěryhodná
```

Publikace:

```text
SUCCESS / allowed WARNING
→ publish

FAILED
→ stop
```

---

# Automatizace a monitoring

Navrhnout:

- trigger;
- scheduler;
- pořadí a závislosti;
- paralelní kroky;
- retry;
- error handling;
- logging;
- secrets;
- monitoring;
- refresh reportingu.

## Trigger a Scheduler

```text
Trigger
→ časový / událostní

Scheduler
→ kdy se proces spustí
```

## Orchestration

```text
Orchestration
→ co se spustí a v jakém pořadí
```

Nezávislé tasky mohou běžet paralelně.

Typický tok:

```text
Source Tasks
→ Transformation
→ Data Quality
→ Load
→ Reporting Refresh
```

## Retry

Použít pouze pro dočasné technické chyby:

- API timeout;
- dočasný výpadek spojení;
- krátkodobě nedostupný soubor;
- transient database error.

Retry nepomůže při logické nebo datové chybě.

## Error Handling

```text
WARNING
→ proces může pokračovat

FAILED
→ navazující kroky zastavit
```

## Logging

Sledovat minimálně:

```text
start_time
end_time
status
message
error_code
exit_code
rows_processed
```

## Secrets

Secrets neukládat do kódu ani repozitáře.

Použít například:

```text
environment variables
.env
Secrets Manager / Vault
```

`.env` musí být v `.gitignore`.

## Monitoring

Sledovat zejména:

```text
last_run_time
last_success_time
pipeline_status
failed_task
warning_count
rows_processed
```

## Power BI Refresh

Refresh provést až po úspěšném dokončení pipeline.

```text
SQL load completed
Data Quality = SUCCESS / allowed WARNING
Data freshness within limit
→ refresh allowed
```

Při `FAILED` refresh neprovádět.

---

# Governance a odpovědnosti

Určit jasné vlastnictví dat, systémů a výstupů.

```text
Data Owner
→ business význam a správnost dat

Source System Owner
→ technický provoz zdrojového systému

Pipeline Owner
→ ETL a orchestrace

Data Quality Owner
→ pravidla a kvalita

Semantic Model Owner
→ model, relationships, DAX

Report Owner
→ business odpovědnost za výstup
```

Data Quality bývá sdílená:

```text
Business
→ business pravidla

Data Engineer / Analyst
→ technické kontroly

Data Analyst / BI
→ analytická konzistence
```

## Přístupy

Použít princip:

```text
least privilege
```

Rozlišit:

- read;
- write;
- admin;
- přístup ke zdroji;
- přístup k pipeline;
- přístup k analytické databázi;
- přístup k modelu a reportu.

## Ownership vs. Access

```text
Owner
→ odpovídá

Contributor
→ vytváří / upravuje

Consumer
→ používá
```

## Řešení chyb

```text
Source issue
→ Source System Owner

Pipeline issue
→ Pipeline Owner

Business data issue
→ Data Owner

Semantic model / DAX issue
→ Semantic Model Owner

Report issue
→ Report Administrator / Report Owner
```

---

# Přiměřenost architektury

Ověřit:

- zda má každý nástroj jasnou roli;
- zda se nástroje zbytečně nepřekrývají;
- zda lze filtrovat už ve zdroji;
- zda ETL načítá jen potřebná data;
- zda je potřeba cloud;
- zda je potřeba Spark;
- zda je potřeba Data Lake / Lakehouse;
- zda řešení zvládne dostupný tým;
- zda složitost odpovídá business hodnotě;
- zda lze řešení zjednodušit.

## Filter Pushdown

```text
Source
→ only relevant data
→ ETL
```

Nenačítat celý zdroj bez důvodu.

## Hlavní princip

```text
Simplest sufficient architecture
→ preferovat

Unnecessary complexity
→ odstranit

Advanced technology
→ použít až při skutečné potřebě
```

---

# Zamítnuté alternativy

Zdokumentovat, které varianty byly posouzeny a proč nebyly zvoleny.

U každé významné alternativy určit:

- proč byla posouzena;
- proč nebyla vybrána;
- proč je zvolená varianta vhodnější;
- kdy by se rozhodnutí změnilo.

Typické oblasti:

```text
Power Query vs. Python
SQL Database vs. Data Warehouse
SQL vs. Data Lake / Lakehouse
Python vs. Spark / PySpark
Local scheduler vs. cloud orchestration
Direct Power BI connection vs. analytical layer
```

Doporučení přehodnotit při změně:

- objemu dat;
- granularita dat;
- frekvence zpracování;
- počtu zdrojů;
- počtu uživatelů a reportů;
- SLA;
- počtu business domén;
- governance;
- infrastruktury;
- možností týmu.

Hlavní princip:

```text
Current requirement
→ simplest sufficient solution

Higher complexity
→ only when justified
```

---

# Solution Design — rychlá kontrola

```text
Business
→ komu řešení slouží a proč

Sources
→ odkud data pocházejí

Ingestion
→ jak se data načítají

Storage
→ kde jsou uložena

Transformation
→ kde probíhá datová logika

Layers
→ jak jsou data oddělena

Data Quality
→ jak se ověřuje důvěryhodnost

Automation
→ jak se proces spouští a sleduje

Governance
→ kdo za co odpovídá

Architecture Fit
→ zda je řešení přiměřené

Alternatives
→ proč nebyly zvoleny jiné varianty
```

Hlavní zásada:

```text
Business need
→ simplest sufficient architecture
→ clear ownership
→ repeatable process
→ controlled output
```