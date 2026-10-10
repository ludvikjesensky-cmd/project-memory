# Create Project Protocol

**Status:** CANONICAL  
**Version:** 1.0  
**Purpose:** Standardní postup pro vytvoření nového dlouhodobého projektu a jeho Project Memory.

## 1. Standardní příkaz

Uživatel může říct například:

**„Vytvoř z tohoto nový projekt.“**

nebo

**„CREATE PROJECT: <název>“**

Pokud je z kontextu jasné, co má být projektem, není nutné znovu žádat informace, které už konverzace obsahuje.

## 2. Nejdříve ověř, zda je nový projekt oprávněný

Nový projekt typicky:
- má vlastní dlouhodobý cíl;
- bude mít opakovanou práci;
- má nebo potřebuje vlastní Source of Truth;
- potřebuje vlastní rozhodnutí, historii a aktuální stav;
- nelze jej přirozeně začlenit do existujícího projektu.

Před vytvořením vždy přečti `projects.yaml` a ověř, zda projekt nebo jeho přirozený rodič už neexistuje.

Pokud jde pouze o úkol, experiment, dokument nebo dílčí workstream existujícího projektu, nevytvářej nové repository automaticky.

## 3. Urči Project a Repository

Nejdříve vytvoř logickou identitu Projectu. Potom urč jeho fyzický domov.

Standardně platí:

**CREATE PROJECT → zajisti primary repository → inicializuj Project Memory v repository → zaregistruj Project.**

Project není GitHub Project board. Repository není Project. Repository je pouze jeden z fyzických zdrojů Projectu.

Pokud nový Project potřebuje nové GitHub repository a aktuální GitHub konektor neumí repository fyzicky vytvořit, vyžádej pouze tento jeden ruční krok od uživatele: založení prázdného repository se sjednaným názvem. Jakmile repository existuje, pokračuj automaticky vytvořením všech potřebných souborů a registrací.

Pokud Project používá existující repository, nové nevytvářej.

Jeden Project může mít více repositories. Urči primary repository pro PROJECT.md a Project Memory a role ostatních repositories popiš v PROJECT.md a registru.

## 4. Identita

Stanov:
- lidský název;
- stabilní ID;
- repository slug;
- stručný účel;
- vztah k existujícím projektům;
- vlastníka/source-of-truth platformu.

Preferuj krátké stabilní názvy. Verzi produktu nevkládej do názvu repository, pokud nejde skutečně o oddělenou generaci vyžadující samostatný Source of Truth.

## 13. Zajisti Repository a Source of Truth

Pokud dostupný GitHub konektor podporuje vytvoření repository a má potřebná oprávnění, může jej AI vytvořit. Pokud tuto schopnost nepodporuje, repository musí fyzicky založit uživatel. AI nesmí tvrdit, že repository vytvořila.

Po založení repository AI ověří jeho existenci a pokračuje inicializací souborů. V aktuálně používaném GitHub konektoru je vytváření a úprava souborů v existujících repositories podporována, ale vytvoření nového repository nemusí být dostupné; schopnost se má vždy ověřit podle aktuálních nástrojů.

Nevytvářej paralelní repository, pokud už existuje vhodný Source of Truth.

## 5. Inicializuj Project Memory

Nový projekt dostane:

`PROJECT.md`

`project-memory/SCOPE.md`  
`project-memory/CURRENT_STATE.md`  
`project-memory/ARCHITECTURE.md`  
`project-memory/DECISIONS.md`  
`project-memory/FAILURES.md`  
`project-memory/LESSONS.md`  
`project-memory/HISTORY.md`  
`project-memory/OPEN_QUESTIONS.md`

`GLOSSARY.md` pouze pokud jej projekt potřebuje.

Nevytvářej prázdné soubory bezmyšlenkovitě. Pokud projekt vzniká z existující práce nebo konverzace, naplň je již známými ověřenými informacemi. U oblastí bez obsahu použij stručné „None recorded yet“ nebo ekvivalent, aby bylo zřejmé, že oblast byla vědomě zkontrolována.

## 6. PROJECT.md

PROJECT.md musí být použitelný jako bootstrap nové konverzace.

Obsahuje alespoň:
- Purpose;
- Source of Truth;
- Current generation/status;
- Project Memory map;
- Reading instructions;
- Storage map;
- Operational truth;
- Critical rules;
- relevantní detailní docs.

## 7. Storage map

Explicitně rozhodni, kde budou:

- KNOWLEDGE;
- SOURCE;
- PRODUCTION;
- ARTIFACTS;
- EXTERNAL;
- OPERATIONAL;
- BACKUP, pokud je již definován.

Neznámé umístění označ jako TBD. Nevymýšlej fyzickou lokaci.

## 8. Konsoliduj vznik projektu

Pokud projekt vzniká z aktuální konverzace nebo existujících materiálů, proveď při vytvoření první konsolidaci.

Zachyť:
- Scope;
- Current State;
- již přijatá Decisions;
- známé Failures;
- Lessons;
- dosavadní History;
- Open Questions;
- Architecture pouze v rozsahu, který je skutečně znám.

Nekopíruj chat jako raw transcript.

## 9. Registrace

Po úspěšném vytvoření přidej projekt do centrálního `projects.yaml`.

Registr musí uvádět minimálně:
- id;
- name;
- status;
- source_of_truth.type;
- source_of_truth location/repository;
- entrypoint.

Podle potřeby:
- current_generation;
- historical_repositories;
- operational;
- notes.

Do registru nezapisuj projekt, jehož Source of Truth ve skutečnosti neexistuje, pokud není explicitně označen jako planned.

## 10. Bezpečnost

Nikdy neukládej:
- API keys;
- passwords;
- tokens;
- secrets;
- credentials.

Při vytváření repository nepovoluj zbytečně širší přístup, než projekt vyžaduje.

## 11. Definition of Done

CREATE PROJECT je hotový pouze pokud:
1. existuje Project a jeho primary Source of Truth; pokud nové repository čeká na ruční založení uživatelem, operace ještě není plně dokončena;
2. existuje PROJECT.md;
3. existuje základní Project Memory;
4. známá hodnota z dosavadní práce byla zkonsolidována;
5. projekt je registrován v projects.yaml;
6. nová AI instance jej dokáže přes registr najít a přes PROJECT.md načíst.

## 12. Report

Na konci stručně sděl:
- co bylo vytvořeno;
- kde je Source of Truth;
- co bylo zkonsolidováno;
- co zůstalo TBD nebo OPEN;
- zda je potřeba nějaký fyzický krok uživatele.
