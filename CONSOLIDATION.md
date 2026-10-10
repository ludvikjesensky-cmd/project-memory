# Conversation Consolidation

**Status:** CANONICAL  
**Version:** 1.0  
**Purpose:** Převést hodnotu z pracovní konverzace do dlouhodobé Project Memory bez archivování konverzačního šumu.

## 1. Spouštěcí formulace

Typický pokyn:

**„Udělej konsolidaci této konverzace.“**

Lze použít i užší variantu, například „konsoliduj dnešní práci do Business Briefu“.

## 2. Princip

**Conversation is the workspace. Project Memory is the distillate.**

Cílem není uložit konverzaci. Cílem je zachovat to, co bude důležité v nové konverzaci za týden, měsíc nebo rok.

## 3. Postup

### Krok 1 — Identifikuj dotčené projekty

Projdi relevantní konverzaci a urč:
- existující projekty, kterých se práce týká;
- témata, která jsou pouze jednorázová;
- případné kandidáty na nový samostatný projekt.

Nezakládej projekt pro každé téma.

### Krok 2 — Načti současnou paměť

Pro každý dotčený projekt nejdříve přečti:
- PROJECT.md;
- CURRENT_STATE;
- podle potřeby SCOPE, ARCHITECTURE, DECISIONS, FAILURES, LESSONS, HISTORY a OPEN_QUESTIONS.

Konsolidace musí navazovat na existující znalost, nikoli ji přepisovat z neznalosti.

### Krok 3 — Extrahuj pouze trvalou hodnotu

Hledej zejména:
- nové ověřené skutečnosti;
- změny aktuálního stavu;
- přijatá rozhodnutí a jejich důvody;
- změny architektury;
- neúspěšné přístupy;
- zobecnitelné lessons;
- významné milníky;
- nové nebo vyřešené otevřené otázky;
- změny umístění Source of Truth;
- nové důležité pojmy.

Ignoruj:
- zdvořilostní konverzaci;
- brainstorming, který nebyl přijat;
- hypotézy prezentované pouze jako možnost;
- dočasné pracovní mezikroky bez budoucí hodnoty;
- opakování již uložených informací.

### Krok 4 — Aktualizuj správný kanonický domov

- současnost → CURRENT_STATE;
- platná architektura → ARCHITECTURE;
- rozhodnutí → DECISIONS;
- slepá cesta → FAILURES;
- přenositelné poznání → LESSONS;
- významný milník/pivot → HISTORY;
- nevyřešená otázka → OPEN_QUESTIONS;
- hranice/účel → SCOPE;
- terminologie → GLOSSARY.

Nevytvářej nový HANDOFF nebo STATUS dokument.

### Krok 5 — Provozní data

Nevkládej raw runtime historii do Markdownu.

Pokud je provozní systém dostupný, ověř důležité skutečnosti v něm. Do Project Memory přenes pouze dlouhodobý význam, například:
- služba byla definitivně odstraněna;
- určitý failure změnil architekturu;
- nákladový limit vedl k rozhodnutí zastavit automatizaci.

### Krok 6 — Rozhodnutí versus návrhy

Do DECISIONS zapisuj jen skutečně přijaté volby.

Pokud konverzace skončila variantami bez rozhodnutí, patří do OPEN_QUESTIONS, případně do detailního design dokumentu.

### Krok 7 — Failure versus bug

Ne každý bug patří do FAILURES.

Failure zaznamenej, pokud zkušenost chrání budoucí práci před opakováním významné slepé cesty, nákladu, rizika nebo mylného předpokladu.

Běžný opravený bug může zůstat pouze v Git historii a provozních datech.

### Krok 8 — History

HISTORY aktualizuj pouze při významném milníku:
- nová generace;
- pivot;
- významná změna produktu;
- změna základní architektury;
- dokončení důležité fáze.

Každodenní práci do HISTORY nezapisuj.

### Krok 9 — Nový projekt

Nový projekt vytvoř pouze pokud:
- má vlastní dlouhodobý cíl;
- bude mít vlastní Source of Truth;
- očekává se opakovaná práce;
- zaslouží vlastní Project Memory.

Pokud ano:
1. vytvoř/zvol Source of Truth;
2. vytvoř PROJECT.md;
3. inicializuj project-memory podle PROJECT_STANDARD;
4. zaregistruj projekt v projects.yaml;
5. urč storage vrstvy a případný operational Source of Truth.

Pokud ne, informace začleň do existujícího projektu nebo ji neukládej.

### Krok 10 — PROJECT.md

PROJECT.md měň jen pokud se změnila mapa projektu:
- Source of Truth;
- struktura;
- čtecí instrukce;
- storage;
- kritické pravidlo;
- provozní umístění.

Nepoužívej PROJECT.md jako denní status.

### Krok 11 — Kontrola konzistence

Před dokončením ověř:
- nevznikly dvě autoritativní verze stejné informace;
- CURRENT_STATE neobsahuje zbytečnou historii;
- staré Decision bylo označeno SUPERSEDED, pokud je nahrazeno;
- vyřešené Open Question odkazuje na výsledek;
- Failure není v rozporu s novým aktivním Decision bez vysvětlení;
- odkazy a názvy souborů odpovídají realitě.

### Krok 12 — Report

Na konci konsolidace stručně sděl:
- které projekty byly aktualizovány;
- které kanonické soubory se změnily;
- jaká nová Decisions/Failures/Lessons vznikla;
- zda vznikl nový projekt;
- co zůstává otevřené.

Nekopíruj uživateli celý obsah souborů, pokud o něj nepožádá.

## 4. Bezpečnost

Nikdy nepřenášej do Markdownu:
- API keys;
- hesla;
- session secrets;
- privátní tokeny;
- jiné credentials.

U citlivých provozních údajů ukládej pouze bezpečný odkaz/popisek umístění.

## 5. Co konsolidace není

Konsolidace není:
- export chatu;
- automatický zápis každé věty;
- meeting minutes;
- Git commit log;
- runtime log;
- nový handoff dokument.

Je to kurátorský proces.

## 6. Definition of Done

Konsolidace je hotová, když nová AI instance může otevřít projekt bez této konverzace a neztratí žádné rozhodnutí, failure, lesson nebo změnu stavu, která by mohla materiálně změnit její další postup.
