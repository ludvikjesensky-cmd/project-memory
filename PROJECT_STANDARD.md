# Project Memory Standard

**Status:** CANONICAL  
**Version:** 1.0  
**Purpose:** Formální standard dlouhodobé paměti každého projektu.

## 1. Project a Repository

Project je logická jednotka dlouhodobé práce. Repository je fyzické Git úložiště.

Typický Project má jedno **primary repository**. V jeho kořeni žije `PROJECT.md` a v něm také kanonická `project-memory/`.

PROJECT.md popisuje celý Project, nikoli pouze obsah repository. Musí tedy umět odkázat i na další repositories, Supabase, Railway, produkční soubory, externí assets a další systémy.

Jeden Project může mít více repositories. Právě jedno má být označeno jako primary memory repository, pokud není explicitně zdokumentována jiná architektura.

GitHub Project (planning board) není součástí tohoto standardu a nesmí být zaměňován s Projectem v Project Memory.

## 17. Povinný vstup

Každý dlouhodobý projekt musí mít v kořeni `PROJECT.md`.

`PROJECT.md` je jediný kanonický bootstrap projektu. Má být krátký a navigační. Nesmí se z něj stát druhá encyklopedie.

Musí obsahovat alespoň:
- název a účel projektu;
- aktuální generaci/verzi, pokud existuje;
- Source of Truth;
- umístění Project Memory;
- doporučené pořadí čtení podle typu práce;
- umístění provozních dat;
- mapu úložišť KNOWLEDGE / SOURCE / PRODUCTION / ARTIFACTS / EXTERNAL;
- kritická pravidla a zákazy;
- odkazy na relevantní detailní dokumentaci.

## 2. Kanonická struktura

Doporučené minimum:

`PROJECT.md`

`project-memory/SCOPE.md`  
`project-memory/CURRENT_STATE.md`  
`project-memory/ARCHITECTURE.md`  
`project-memory/DECISIONS.md`  
`project-memory/FAILURES.md`  
`project-memory/LESSONS.md`  
`project-memory/HISTORY.md`  
`project-memory/OPEN_QUESTIONS.md`

Volitelně:

`project-memory/GLOSSARY.md`

Detailní dokumentace patří do `docs/` nebo jiné jasně pojmenované projektové vrstvy.

## 3. Jedna odpovědnost, jeden kanonický domov

- Definice a hranice → SCOPE
- Současnost → CURRENT_STATE
- Platná architektura → ARCHITECTURE
- Důvody významných voleb → DECISIONS
- Slepé cesty → FAILURES
- Přenositelné poznání → LESSONS
- Vývoj projektu → HISTORY
- Nerozhodnuté otázky → OPEN_QUESTIONS
- Provozní události → provozní systém
- Verze textu/kódu → Git

Nevytvářej paralelní kanonické soubory stejné odpovědnosti.

## 4. CURRENT_STATE

CURRENT_STATE je přepisovaný snapshot, nikoli append-only log.

Má obsahovat:
- datum posledního ověření;
- současnou fázi;
- ověřeně funkční části;
- známé problémy;
- blockery;
- nejbližší konkrétní kroky;
- odkazy na živý provozní Source of Truth.

Historické informace se z něj průběžně odstraňují a podle významu přesouvají do HISTORY, DECISIONS, FAILURES nebo LESSONS.

Datované `PROGRESS_*.md`, nové HANDOFF soubory nebo alternativní STATUS soubory se nemají používat jako náhrada CURRENT_STATE.

## 5. DECISIONS

Každé významné rozhodnutí má doporučeně:
- ID, například D001;
- datum;
- stav ACTIVE / SUPERSEDED / REVOKED;
- stručný název;
- rozhodnutí;
- důvod;
- důsledky;
- odkaz na nahrazující rozhodnutí, pokud existuje.

Experiment není automaticky rozhodnutí. Rozhodnutí musí být vědomě přijato.

## 6. FAILURES

Failure záznam má doporučeně:
- ID, například F001;
- datum nebo období;
- cíl;
- zvolený přístup;
- pozorovaný výsledek;
- příčinu;
- cenu/riziko, pokud je relevantní;
- poučení;
- podmínky, za kterých lze přístup znovu zvážit.

Neúspěšný přístup nemaž jen proto, že už není v kódu. Historie slepé cesty chrání budoucí práci.

## 7. LESSONS

Lesson má zachytit zobecnitelné poznání, ne pouze popis incidentu.

Doporučeně:
- ID, například L001;
- původ zkušenosti;
- princip;
- praktický dopad na budoucí práci.

## 8. HISTORY

HISTORY zaznamenává pouze významné milníky, generace a pivoty.

U nové generace vždy zachyť:
- proč vznikla;
- co zdědila;
- co změnila;
- co opustila;
- které Decisions byly superseded;
- které Failures a Lessons zůstávají relevantní.

Git commit history nenahrazuje projektovou historii a projektová historie nemá opisovat commity.

## 9. OPEN_QUESTIONS

Každá otevřená otázka má mít:
- ID;
- otázku;
- proč je důležitá;
- známé varianty, pokud existují;
- co je potřeba zjistit pro rozhodnutí;
- stav OPEN / BLOCKED / RESOLVED.

Po vyřešení se závěr podle povahy přesune do DECISIONS a otázka se označí RESOLVED nebo odstraní z aktivního seznamu s odkazem na rozhodnutí.

## 10. ARCHITECTURE

ARCHITECTURE popisuje pouze současný platný systém:
- hlavní komponenty;
- odpovědnosti;
- datové toky;
- hranice;
- externí služby;
- autoritativní data;
- kritické kontrakty.

Historické varianty patří do HISTORY/DECISIONS/FAILURES.

## 11. SCOPE

SCOPE chrání projekt před postupným rozpliznutím.

Má definovat:
- problém;
- uživatele/publikum;
- cíle;
- non-goals;
- hranice;
- základní produktové principy.

## 12. Storage model

Každý projekt podle potřeby explicitně mapuje:

**KNOWLEDGE** — znalost a dokumentace.  
**SOURCE** — editovatelný zdroj produktu.  
**PRODUCTION** — pracovní nativní soubory nástrojů.  
**ARTIFACTS** — výsledné release/export soubory.  
**EXTERNAL** — data a služby mimo hlavní repository.

Git je primární pro text, kód a znalost. Velké nebo často měněné binární soubory se ukládají do Git pouze tehdy, když to dává provozní smysl. Pro velké binární zdroje lze použít Git LFS nebo externí storage. Významné výsledky mají mít explicitní release/version identitu.

U publikací je doporučený princip:
- textový rukopis = content master;
- produkční sazba = production/release master;
- PDF/EPUB = immutable release artifact.

Pokud se po sazbě opraví obsah, změna se má podle možností promítnout zpět do content masteru, aby nevznikly dvě divergentní pravdy.

## 13. Operational state

Raw logy, joby, retry, telemetry, usage a runtime eventy nepatří do Project Memory.

Pokud existuje Supabase/Postgres nebo jiný provozní systém, je autoritativní pro skutečný běh systému. Markdown pouze vysvětluje význam, umístění a dlouhodobé závěry.

## 14. Čtení projektu AI

Default:
1. PROJECT
2. SCOPE
3. CURRENT_STATE

Pro architekturu navíc:
4. ARCHITECTURE
5. DECISIONS
6. FAILURES
7. OPEN_QUESTIONS

Pro historické nebo strategické rozhodnutí navíc:
8. HISTORY
9. LESSONS

Detailní docs čti selektivně podle mapy v PROJECT.md.

Nečti automaticky celé repository, pokud to není potřeba.

## 15. Údržba

Po významné práci proveď konsolidaci.

Nezakládej nový Markdown proto, že je pohodlnější něco odložit do nového souboru. Nejdříve najdi existující kanonický domov.

Kanonická Project Memory má být malá, čitelná a aktuální. Detail může růst v docs a provozních systémech.

## 16. Kvalitativní test

Projekt splňuje standard, pokud nová AI instance bez staré konverzace dokáže po přečtení PROJECT.md a relevantní Project Memory správně odpovědět:

- Co tento projekt dělá?
- Kde právě jsme?
- Jak systém funguje?
- Proč jsme se sem dostali?
- Co už jsme zkusili a nemáme slepě opakovat?
- Co ještě není rozhodnuto?
- Kde ověřím skutečný provozní stav?
- Co mám udělat dál?

Pokud ne, Project Memory není dostatečně konsolidovaná.
