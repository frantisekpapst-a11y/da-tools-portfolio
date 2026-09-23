# 🧰 Data Analytics Tools Portfolio

Portfolio zaměřené na **praktické používání nástrojů, datových architektur a návrhových principů v datové analytice**.

Repozitář obsahuje případové studie, technické taháky, praktické notebooky, datové podklady a krátké ověřovací testy. Hlavním cílem je ukázat schopnost vybrat vhodný nástroj pro konkrétní problém, navrhnout přiměřený datový tok a rozumět návaznosti mezi zdroji, transformacemi, úložištěm, analytickou vrstvou a reportingem.

Repozitář není prezentován jako produkční enterprise platforma. Případové studie slouží především k praktickému procvičení **datového návrhu, architektonického rozhodování a volby nástrojů** v realistických business scénářích.

Hlavní oblasti:
- návrh moderního datového prostředí;
- volba nástrojů podle business a technických požadavků;
- práce s většími datasety;
- data warehouse, data lake a lakehouse;
- vrstvy Raw / Bronze / Silver / Gold;
- cloudové platformy a Microsoft Fabric;
- Databricks, Spark a PySpark;
- orchestrace datových procesů;
- datová kvalita, validace a monitoring;
- Jupyter Notebook a efektivní práce s Pandas;
- Git, GitHub a VS Code;
- návrh řešení od business požadavku po Power BI.

---

# 📂 Struktura repozitáře

```text
da-tools-portfolio/

├── tools-case-studies/
│   ├── 01_data_tools_selection.md
│   ├── 02_large_data_strategy.md
│   ├── 03_warehouse_design.md
│   ├── 04_lakehouse_layers.md
│   ├── 05_cloud_platform_selection.md
│   ├── 06_inventory_reporting_orchestration.md
│   └── 07_energy_data_solution_design.md
│
├── tools-datasets/
│   ├── generated/
│   ├── processed/
│   └── raw/
│
├── tools-knowledge-base/
│   ├── cloud-platforms-cheatsheet.md
│   ├── data-layers-cheatsheet.md
│   ├── data-stack-cheatsheet.md
│   ├── databricks-cheatsheet.md
│   ├── git-cheatsheet.md
│   ├── jupyter-cheatsheet.md
│   ├── large-data-cheatsheet.md
│   ├── orchestration-cheatsheet.md
│   ├── solution-design-cheatsheet.md
│   ├── spark-cheatsheet.md
│   ├── vs-code-cheatsheet.md
│   └── warehouse-lake-lakehouse-cheatsheet.md
│
├── tools-mini-tests/
│   └── mini-tests.md
│
├── tools-notebooks/
│   ├── jupyter-workflow-notebook.ipynb
│   └── large-data-efficiency-notebook.ipynb
│
├── .gitignore
└── README.md
```

---

# 🎯 Zaměření portfolia

Repozitář je postavený kolem jednoduchého principu:
```text
Business Requirement
→ Data Sources
→ Tool Selection
→ Data Architecture
→ Transformation
→ Analytical Layer
→ Reporting
→ Business Decision
```

Důraz není na použití co největšího počtu technologií.

Každý návrh má odpovědět zejména na otázky:
```text
Co potřebuje business?
Jaká data jsou dostupná?
Jaký objem a granularitu mají?
Kde má probíhat transformace?
Jaké úložiště dává smysl?
Jak zajistit kvalitu a dohledatelnost?
Jaký nástroj má kterou roli?
Jak má výsledek sloužit reportingu a rozhodování?
```

---

# 📁 Case Studies

## Case Study 01 — Modern Data Environment and Tools Selection

Návrh přehledného a udržitelného datového prostředí pro pravidelný management reporting.

Business scénář propojuje:
```text
SQL Server
+
Excel
+
REST API
+
Power BI
```

Hlavní témata:
- odstranění ručních exportů a kopírování;
- sjednocení transformační a business logiky;
- rozdělení rolí mezi SQL Server, Python, Power Query, Power BI a DAX;
- centrální analytická vrstva;
- orchestrace aktualizace;
- monitoring a zpracování chyb;
- posouzení alternativ jako CSV, Parquet, Pandas, Spark nebo Databricks.

Hlavní princip:
```text
nevybírat technologii podle popularity
→ vybírat ji podle konkrétní role v datovém toku
```

➡️ [Otevřít Case Study 01](tools-case-studies/01_data_tools_selection.md)

---

## Case Study 02 — Large Data Strategy

Návrh efektivnějšího zpracování rozsáhlých prodejních dat uložených v SQL Serveru.

Výchozí scénář:
```text
80 milionů řádků
+
30 sloupců
+
5 milionů nových řádků měsíčně
+
16 GB RAM
```

Případová studie řeší zejména:
- proč není vhodné exportovat celou databázovou tabulku do CSV;
- proč filtrovat a agregovat data co nejblíže zdroji;
- výběr pouze potřebných sloupců a období;
- rozdělení práce mezi SQL Server, Power Query, Power BI a DAX;
- rozdíl mezi management agregací a detailem posledních 90 dní;
- proč větší dataset automaticky neznamená potřebu Sparku.

Hlavní princip:
```text
nejdříve snížit množství dat
→ optimalizovat datový tok
→ teprve potom zvažovat distribuované zpracování
```

➡️ [Otevřít Case Study 02](tools-case-studies/02_large_data_strategy.md)

---

## Case Study 03 — Warehouse Design

Návrh analytického řešení pro obchodní reporting a praktické rozlišení rolí data lake, data warehouse a lakehouse.

Případová studie zahrnuje:
- zachování původních dat;
- návrh Sales datamartu;
- definici granularity faktové tabulky;
- návrh `fact_sales`;
- návrh zákaznické, produktové, časové a regionální dimenze;
- přirozené a technické klíče;
- hvězdicové schéma;
- referenční integritu;
- rozdělení výpočtů mezi datovou vrstvu a DAX;
- validaci analytického modelu.

Navržený princip:
```text
zdrojová data
→ data lake
→ transformace
→ data warehouse / datamart
→ Power BI
```

➡️ [Otevřít Case Study 03](tools-case-studies/03_warehouse_design.md)

---

## Case Study 04 — Customer Support Lakehouse Layers

Návrh opakovatelného zpracování dat zákaznické podpory pomocí lakehouse vrstev.

Zdrojový scénář kombinuje:
```text
CSV tickety
+
JSON průzkumy
+
relační data operátorů
+
textové přepisy
```

Hlavní témata:
- ETL vs. ELT;
- Bronze vrstva pro zachování vstupů;
- Silver vrstva pro technicky připravená data;
- Gold vrstva pro business-ready výstupy;
- validace stavů, časů, hodnocení a referenčních vazeb;
- karanténa problematických záznamů;
- výpočet SLA;
- reconciliation;
- idempotence;
- data lineage;
- quality gate před publikací.

Hlavní tok:
```text
Source
→ Bronze
→ Silver
→ Gold
→ Power BI
```

➡️ [Otevřít Case Study 04](tools-case-studies/04_lakehouse_layers.md)

---

## Case Study 05 — Cloud Platform Selection

Porovnání dvou přístupů k návrhu cloudového analytického řešení:
```text
samostatné služby Microsoft Azure

vs.

Microsoft Fabric
```

Business scénář je zaměřený na logistický reporting a každodenní aktualizaci dat.

Případová studie řeší:
- existující technologie a schopnosti týmu;
- datové zdroje a jejich charakter;
- zabezpečení;
- správu prostředí;
- náklady a výpočetní kapacitu;
- bezpečný přístup k lokálnímu SQL Serveru;
- identity a oprávnění;
- oddělení vývoje, testu a produkce;
- Bronze, Silver a Gold vrstvy;
- Gold hvězdicové schéma;
- automatizaci a monitoring;
- situace, kdy dává větší smysl Fabric a kdy samostatné Azure služby.

➡️ [Otevřít Case Study 05](tools-case-studies/05_cloud_platform_selection.md)

---

## Case Study 06 — Inventory Reporting Orchestration

Návrh řízeného datového procesu pro každodenní aktualizaci skladového reportingu.

Zdrojový scénář:
```text
ERP databáze
+
dodavatelský CSV
+
kurzovní API
+
Excel s cílovými hodnotami
```

Hlavní témata:
- rozdělení procesu na samostatné úlohy;
- závislé a nezávislé kroky;
- paralelní načítání zdrojů;
- povinné a podmíněné vstupy;
- validace před transformací;
- chování při selhání;
- blokování publikace nevalidního výsledku;
- závěrečná kontrola;
- spuštění Power BI refresh až po úspěšném dokončení povinných kroků.

Výsledný princip:
```text
Load
→ Validate
→ Transform
→ Update Analytical Tables
→ Final Validation
→ Power BI Refresh
→ Log Result
```

➡️ [Otevřít Case Study 06](tools-case-studies/06_inventory_reporting_orchestration.md)

---

## Case Study 07 — Energy Data Solution Design

Komplexní návrh datového a analytického řešení pro monitoring spotřeby energie, nákladů, rozpočtu a provozních odchylek napříč 12 pobočkami.

Datové zdroje:
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

Případová studie propojuje:
- business understanding;
- posouzení datových zdrojů;
- architektonické rozhodování;
- ETL návrh;
- Raw / Staging / Silver / Gold vrstvy;
- datovou kvalitu;
- analytický model;
- rozdělení transformačních rolí mezi Python, SQL, Power Query a DAX;
- automatizaci a monitoring;
- governance;
- zamítnuté alternativy;
- podmínky, za kterých by bylo vhodné architekturu přehodnotit.

Cílem není použít nejkomplexnější technologii, ale navrhnout řešení odpovídající reálným požadavkům.

➡️ [Otevřít Case Study 07](tools-case-studies/07_energy_data_solution_design.md)

---

# 🧩 Data Tools Skills

Repozitář pokrývá především schopnost orientovat se v širším datovém prostředí a rozhodovat, **který nástroj má v konkrétní situaci smysl**.

Hlavní oblasti:
- business a technické posouzení datového problému;
- role SQL, Pythonu, Power Query, Power BI a DAX;
- relační analytické modelování;
- fact a dimension tabulky;
- granularita a kardinalita;
- hvězdicové schéma;
- data lake, warehouse a lakehouse;
- ETL a ELT;
- Raw / Bronze / Silver / Gold vrstvy;
- práce s většími objemy dat;
- Pandas a efektivní načítání;
- Parquet;
- Spark a distribuované zpracování;
- Databricks;
- Azure a Microsoft Fabric;
- cloudové identity, oprávnění a prostředí;
- orchestrace;
- datová kvalita a validační brány;
- lineage, governance a monitoring;
- Jupyter Notebook workflow;
- Git, GitHub a VS Code.

Důraz je kladen na tento princip:
```text
Business Need
+ Data Characteristics
+ Existing Stack
+ Team Capabilities
+ Operational Requirements
→ Appropriate Tool and Architecture
```

---

# 🔄 Technologie a principy v datovém workflow

## SQL a relační databáze

SQL a relační databáze jsou v návrzích využívány zejména pro:
- práci se strukturovanými daty;
- filtrování a agregaci u zdroje;
- relační transformace;
- integritu klíčů;
- reportingové pohledy;
- data warehouse a datamart vrstvy.

---

## Python a Pandas

Python a Pandas mají roli zejména tam, kde je potřeba:
- pracovat se soubory nebo API;
- kombinovat různorodé zdroje;
- provádět nestandardní validace;
- zkoumat data v notebooku;
- automatizovat opakovatelné zpracování.

Repozitář zároveň zdůrazňuje, že Python není automaticky nejlepší nástroj pro každou transformaci.

---

## Power Query, Power BI a DAX

Power BI představuje analytickou a reportovací vrstvu.

```text
připravená data
→ Power Query
→ datový model
→ DAX
→ vizualizace
→ business interpretace
```

Transformace mají být rozdělené tak, aby se stejná logika zbytečně neopakovala v několika vrstvách.

---

## Cloud, Fabric a Databricks

Cloudová část portfolia se zaměřuje na pochopení rolí služeb, ne na memorování produktů.

Řešená témata zahrnují:
- ingestion;
- storage;
- compute;
- transformace;
- orchestrace;
- governance;
- serving layer;
- sémantický model;
- Azure;
- Microsoft Fabric;
- Databricks;
- lakehouse přístup.

---

## Spark a větší datasety

Repozitář pracuje s principem:
```text
nejdříve optimalizovat data a dotaz
→ potom optimalizovat formát a načítání
→ až následně zvažovat Spark
```

Spark je chápán jako nástroj pro distribuované zpracování, nikoliv jako automatická odpověď na každý větší dataset.

---

## Orchestrace

Orchestrace určuje:
```text
co se spouští
→ v jakém pořadí
→ na čem jednotlivé kroky závisí
→ co se stane při chybě
→ kdy lze publikovat výsledek
```

Samotná orchestrace data nemusí transformovat. Řídí nástroje a úlohy, které jednotlivé kroky provádějí.

---

# 📚 Technické materiály

## Knowledge Base

Složka `tools-knowledge-base` obsahuje referenční materiály pro jednotlivé oblasti:
- [Cloud Platforms Cheatsheet](tools-knowledge-base/cloud-platforms-cheatsheet.md)
- [Data Layers Cheatsheet](tools-knowledge-base/data-layers-cheatsheet.md)
- [Modern Data Stack Cheatsheet](tools-knowledge-base/data-stack-cheatsheet.md)
- [Databricks Cheatsheet](tools-knowledge-base/databricks-cheatsheet.md)
- [Git Cheatsheet](tools-knowledge-base/git-cheatsheet.md)
- [Jupyter Notebook Workflow Cheatsheet](tools-knowledge-base/jupyter-cheatsheet.md)
- [Large Data Cheatsheet](tools-knowledge-base/large-data-cheatsheet.md)
- [Orchestration Cheatsheet](tools-knowledge-base/orchestration-cheatsheet.md)
- [Solution Design Cheatsheet](tools-knowledge-base/solution-design-cheatsheet.md)
- [Spark a PySpark Cheatsheet](tools-knowledge-base/spark-cheatsheet.md)
- [VS Code Cheatsheet](tools-knowledge-base/vs-code-cheatsheet.md)
- [Warehouse / Lake / Lakehouse Cheatsheet](tools-knowledge-base/warehouse-lake-lakehouse-cheatsheet.md)

Materiály slouží jako praktická reference k případovým studiím a notebookům.

---

## Praktické notebooky

Složka `tools-notebooks` obsahuje praktické Jupyter notebooky:
- [Jupyter Workflow Notebook](tools-notebooks/jupyter-workflow-notebook.ipynb)
- [Large Data Efficiency Notebook](tools-notebooks/large-data-efficiency-notebook.ipynb)

Notebooky doplňují koncepční část repozitáře o praktickou práci s daty a reprodukovatelným analytickým postupem.

---

## Datové podklady

Složka `tools-datasets` odděluje datové soubory podle jejich role:
```text
raw
→ původní vstupy
generated
→ synteticky vytvořená data
processed
→ připravené nebo zpracované výstupy
```

Toto oddělení podporuje přehlednější práci s daty a reprodukovatelnost praktických cvičení.

---

## Mini Tests

Složka `tools-mini-tests` obsahuje krátké otázky a odpovědi určené k ověření porozumění hlavním principům práce s analytickými nástroji a architekturou.

➡️ [Otevřít Mini Tests](tools-mini-tests/mini-tests.md)

---

# 🛠 Technologie a koncepty

```text
SQL
SQL Server
Python
Pandas
Jupyter Notebook
CSV
JSON
Parquet
REST API
Power Query
Power BI
DAX
Data Warehouse
Data Lake
Lakehouse
ETL
ELT
Raw / Staging
Bronze / Silver / Gold
Star Schema
Fact / Dimension Modeling
Data Quality
Data Lineage
Governance
Orchestration
Monitoring
Spark
PySpark
Databricks
Microsoft Azure
Microsoft Fabric
Git
GitHub
VS Code
```