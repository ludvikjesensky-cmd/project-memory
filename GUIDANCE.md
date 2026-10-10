# Project Memory Guidance

**Status:** CANONICAL  
**Purpose:** Centrální pravidla pro organizaci, čtení, údržbu a konsolidaci dlouhodobé paměti projektů.  
**Audience:** Ludvík, ChatGPT/Codex a další AI nebo lidé pracující na projektech.

---

## 1. Základní princip

Konverzace je pracovní prostor. Project Memory je destilát.

Dlouhodobá kontinuita projektu nesmí záviset na délce jedné konverzace ani na paměti konkrétní AI instance. Každý projekt musí být pochopitelný z vlastních zdrojů pravdy a své Project Memory.

Nová AI instance musí být schopna:
1. najít projekt;
2. zjistit, kde je jeho Source of Truth;
3. přečíst jeho vstupní dokument `PROJECT.md`;
4. obnovit potřebný kontext;
5. pokračovat bez nutnosti číst staré konverzace.

Project Memory není archiv všeho. Je to udržovaná, autoritativní znalost projektu.

---

## 2. Hierarchie

Centrální vrstva:
- `GUIDANCE.md` — tato pravidla;
- `PROJECT_STANDARD.md` — formální standard struktury projektu;
- `CONSOLIDATION.md` — postup konsolidace konverzace;
- `projects.yaml` — strojově čitelný registr projektů.

Projektová vrstva:
- každý projekt má svůj vlastní Source of Truth;
- hlavní vstupní dokument projektu je vždy `PROJECT.md`;
- kanonická dlouhodobá paměť projektu je v `project-memory/`;
- detailní dokumentace může být v `docs/`;
- provozní data mohou žít mimo Git, typicky v Supabase.

Centrální repository `project-memory` není Source of Truth jednotlivých projektů. Je to control plane: říká, jak projekty najít, číst, vytvářet a udržovat.

---

## 3. Jediný vstup do projektu

Každý projekt musí mít právě jeden kanonický vstupní dokument:

`PROJECT.md`

Při vstupu do neznámého projektu čti nejdříve tento soubor.

PROJECT.md je mapa, nikoli encyklopedie. Má stručně odpovědět:
- co projekt je;
- proč existuje;
- kde je Source of Truth;
- kde je aktuální stav;
- jaké další dokumenty číst podle typu práce;
- kde jsou provozní data;
- kde jsou zdrojové, produkční a výstupní soubory;
- jaké jsou kritické zásady a zákazy.

README může existovat, ale jeho primární účel je technický nebo veřejný: instalace, spuštění, použití repository apod. README nesmí nahrazovat PROJECT.md jako vstup do dlouhodobé projektové paměti.

---

## 4. Kanonická Project Memory

Standardní adresář `project-memory/` obsahuje:

### SCOPE.md
Co projekt je a co není. Produktový účel, cíle, hranice a publikum.

### CURRENT_STATE.md
Pouze současný ověřený stav.

Obsahuje:
- kde právě jsme;
- co funguje;
- co nefunguje;
- aktivní blockery;
- nejbližší kroky.

CURRENT_STATE nesmí být historický deník. Jakmile informace přestane být aktuální, odstraní se nebo se její trvalá hodnota přesune do History, Decisions, Failures nebo Lessons.

Smí existovat pouze jeden kanonický CURRENT_STATE.

Nevytvářej paralelní STATUS, HANDOFF, NEW_HANDOFF, CURRENT_STATUS nebo datované PROGRESS soubory jako konkurenční zdroje současného stavu.

### ARCHITECTURE.md
Popisuje současnou platnou architekturu projektu.

Neobsahuje kompletní historii architektury ani seznam všech experimentů. Historické změny patří do HISTORY a jejich důvody do DECISIONS.

### DECISIONS.md
Dlouhodobý Decision Log.

Každé významné rozhodnutí má:
- identifikátor;
- datum;
- rozhodnutí;
- důvod;
- důsledky, pokud jsou důležité;
- stav.

Doporučené stavy:
- ACTIVE;
- SUPERSEDED;
- REVOKED.

Staré rozhodnutí se nemaže jen proto, že už neplatí. Změní se jeho stav a uvede se rozhodnutí, které jej nahradilo.

Experimentální implementace není automaticky architektonickým rozhodnutím.

### FAILURES.md
Evidence významných slepých cest a neúspěšných přístupů.

U každého významného failure zaznamenej:
- co jsme chtěli dosáhnout;
- co jsme zkusili;
- co se skutečně stalo;
- proč přístup nefungoval;
- náklady nebo rizika, pokud jsou relevantní;
- zda a za jakých nových podmínek má smysl pokus opakovat.

Failure neznamená „navždy zakázáno“. Znamená „neopakuj bez znalosti předchozí zkušenosti“.

Před návrhem zásadní architektonické změny musí AI zkontrolovat FAILURES.md.

### LESSONS.md
Přenositelné poznatky získané během projektu.

Failure říká, co nefungovalo. Lesson říká, co jsme se díky práci naučili.

Lessons mají zachycovat principy, které mají hodnotu i mimo konkrétní incident.

### HISTORY.md
Stručný příběh vývoje projektu.

Zachycuje významné generace, pivoty, milníky a změny směru. Neopisuje historii commitů a není provozním logem.

U vícegeneračních projektů má vysvětlit:
- proč vznikla nová generace;
- co změnila;
- co ze starší generace zachovala;
- co vědomě opustila.

### OPEN_QUESTIONS.md
Otázky, které ještě nejsou rozhodnuté.

Je důležité odlišit:
„zatím jsme nerozhodli“
od
„dokumentace něco zapomněla“.

AI nesmí otevřenou otázku svévolně prezentovat jako existující rozhodnutí.

### GLOSSARY.md
Volitelný dokument pro projekty s vlastní terminologií, komponentami, zkratkami nebo doménovým jazykem.

---

## 5. Detailní dokumentace

Adresář `docs/` je volný prostor pro detailní dokumentaci.

Může obsahovat libovolný počet specializovaných dokumentů: technické kontrakty, popis komponent, editorial playbook, právní poznámky, produktové koncepty, API dokumentaci, výzkum apod.

Důležité pravidlo:

Detailní dokumentace nesmí soutěžit s kanonickou Project Memory o autoritu.

Project Memory říká, co je platné a kam se pro detail podívat.

Před vytvořením nového Markdown souboru nejdříve zjisti, zda daná znalost už nemá přirozený domov v existujícím kanonickém dokumentu.

**Nevytvářej nový kanonický dokument, pokud jeho odpovědnost už existuje.**

---

## 6. Markdown versus provozní data

Markdown odpovídá především na otázky:

**Co víme? Proč to tak je? Co platí? Co jsme se naučili?**

Databáze a observability odpovídají především:

**Co se skutečně stalo? Kdy? V jakém běhu? S jakým výsledkem?**

Do Markdownu nepatří tisíce provozních eventů, jobů, retry, tokenových statistik a logů.

Pokud projekt používá Supabase nebo jinou provozní databázi, živý provozní stav a event log mají být tam.

Project Memory pouze vysvětluje:
- kde tato data jsou;
- které tabulky/dashboardy jsou autoritativní;
- jak je interpretovat.

Významné provozní zjištění se může stát Decision, Failure nebo Lesson. Samotný raw event však zůstává v provozním systému.

---

## 7. Typy projektových souborů

Každý projekt má podle potřeby definovat pět vrstev.

### KNOWLEDGE
Znalost o projektu: PROJECT.md, Project Memory, dokumentace.

### SOURCE
Materiál, ze kterého produkt vzniká. Například Markdown rukopis, zdrojový kód, data nebo scénář.

Textové zdroje vhodné pro Git se verzují historií Gitu. Nevytvářej zbytečně soubory typu FINAL, FINAL2, REALLY_FINAL.

### PRODUCTION
Pracovní soubory nástrojů, například InDesign, Blender, PowerPoint nebo jiné binární formáty.

Mohou být součástí projektu, pokud to dává smysl, ale Git není automaticky vhodný archiv každé jejich pracovní verze.

### ARTIFACTS
Výsledné výstupy konkrétní verze: PDF, EPUB, finální video, export apod.

Artefakt není automaticky Source of Truth. Významná vydání mohou být uchovávána jako Releases nebo v určeném archivu.

### EXTERNAL
Velká data, databáze, raw video, audio mastery, cloudové archivy, externí služby apod.

Nemusí být fyzicky v GitHubu. PROJECT.md však musí říkat, kde jsou.

Zásada:

**GitHub nemusí skladovat všechno. Projekt ale musí vědět, kde všechno je.**

---

## 8. Source of Truth

Pro každý druh informace musí existovat jasný autoritativní zdroj.

Typické mapování:

- Co projekt dělá? → SCOPE.md
- Kde jsme teď? → CURRENT_STATE.md
- Jak systém nyní funguje? → ARCHITECTURE.md
- Proč jsme něco rozhodli? → DECISIONS.md
- Co jsme zkusili a nefungovalo? → FAILURES.md
- Co jsme se naučili? → LESSONS.md
- Jak jsme se sem dostali? → HISTORY.md
- Co ještě není rozhodnuté? → OPEN_QUESTIONS.md
- Co se stalo v konkrétním běhu? → provozní databáze / observability
- Jak se změnil text nebo kód? → Git historie

Pokud dva dokumenty tvrdí rozdílné věci, nejprve určuj jejich roli a autoritu. Nevytvářej třetí dokument jako další „pravdu“.

---

## 9. Pravidla pro AI

Při vstupu do projektu:
1. přečti PROJECT.md;
2. přečti pouze Project Memory a detailní dokumenty relevantní pro aktuální úkol;
3. pokud je důležitý živý provozní stav, ověř jej v autoritativním provozním systému.

Před architektonickou změnou:
- přečti ARCHITECTURE.md;
- přečti DECISIONS.md;
- přečti FAILURES.md;
- zkontroluj OPEN_QUESTIONS.md.

Před opakováním dříve zkoušeného přístupu:
- zkontroluj FAILURES.md a HISTORY.md;
- nevracej projekt do staré slepé uličky bez nové skutečnosti, která mění předchozí závěr.

Při debuggingu:
- CURRENT_STATE poskytuje kontext;
- skutečné runy, logy a eventy ověřuj v provozním Source of Truth;
- starý Markdown nepovažuj za důkaz současného runtime stavu.

Při návrhu:
- odděluj ověřenou skutečnost, hypotézu a návrh;
- před nákladným nebo destruktivním krokem definuj schopnost, failure modes, cenu a riziko;
- preferuj nejmenší reverzibilní test;
- automatizuj až workflow, jehož hodnotu jsme ověřili.

Po významné práci:
- aktualizuj odpovídající Project Memory;
- nevytvářej nový handoff, pokud stačí aktualizovat CURRENT_STATE;
- provozní události zapisuj do provozního systému, ne do Markdown deníku.

---

## 10. Vytvoření nového projektu

Nový projekt zakládej pouze tehdy, když jde skutečně o samostatnou dlouhodobou iniciativu, nikoli o jednorázové téma nebo dílčí úkol existujícího projektu.

Při vytvoření projektu:
1. urč Source of Truth;
2. vytvoř PROJECT.md;
3. vytvoř `project-memory/`;
4. inicializuj potřebné kanonické soubory;
5. definuj KNOWLEDGE, SOURCE, PRODUCTION, ARTIFACTS a EXTERNAL podle potřeby;
6. urč provozní Source of Truth, pokud existuje;
7. zaregistruj projekt v centrálním registru;
8. nikdy neukládej secrets, API keys nebo hesla do Project Memory.

---

## 11. Konsolidace konverzace

Konverzace není dlouhodobá projektová paměť.

Po významné práci lze zadat například:

**„Udělej konsolidaci této konverzace.“**

Konsolidace znamená:
1. projít relevantní konverzaci;
2. určit dotčené projekty;
3. destilovat nové znalosti;
4. aktualizovat správné kanonické soubory;
5. zaznamenat nová rozhodnutí;
6. zaznamenat významné failures;
7. přenést přenositelné poznatky do Lessons;
8. aktualizovat Current State;
9. aktualizovat Architecture pouze pokud se skutečně změnila;
10. aktualizovat History pouze při významném milníku nebo změně směru;
11. uzavřít nebo přidat Open Questions;
12. vytvořit nový projekt pouze pokud téma skutečně splňuje kritéria samostatného projektu.

Nekopíruj konverzaci wholesale do Project Memory.

**Conversation is the workspace. Project Memory is the distillate.**

Podrobný postup definuje CONSOLIDATION.md.

---

## 12. Historie a nové generace projektu

Nová verze nebo generace projektu nesmí automaticky zahodit znalost předchozí verze.

Při vzniku nové generace:
- popiš důvod změny;
- urč, které principy se dědí;
- urč, která rozhodnutí jsou superseded;
- přenes relevantní failures a lessons;
- zachovej provenance historických rozhodnutí;
- neudržuj starou implementaci jako skrytou code dependency, pokud má být pouze reference.

Nová generace má zdědit znalost, ne nutně starou architekturu.

---

## 13. Central Vault

Budoucí Central Vault je projekce, nikoli Source of Truth.

Může agregovat vybrané Markdown dokumenty z více projektů pro vyhledávání, navigaci a lidské čtení.

Pravidla:
- Source → Central Vault je jednosměrný tok;
- změny ve Vaultu se nepropagují zpět do zdrojových projektů;
- Vault musí být rebuildable;
- jeho ztráta nesmí znamenat ztrátu kanonické projektové znalosti;
- Obsidian je pouze možná prezentační vrstva nad Vaultem, nikoli součást autority projektu.

---

## 14. Bezpečnost a záloha

Logická kopie ve stejném účtu není skutečná bezpečnostní záloha.

Dlouhodobě odděluj:
- pracovní Source of Truth;
- případný Central Vault;
- skutečný backup v jiném trust domainu / účtu / poskytovateli.

Automatizační služby mají dostávat pouze minimální potřebná oprávnění. Builder Vaultu má například číst zdroje a zapisovat Vault, nikoli měnit zdrojové projekty.

---

## 15. Zásady odvozené z Business Brief v1–v4

Zkušenost Business Briefu ukázala několik obecných pravidel:

1. Více dokumentace samo o sobě neznamená lepší paměť.
2. Nejdůležitější je jasná hierarchie autority.
3. Handoff soubory vznikají jako symptom toho, že není jasný jediný Current State a vstupní mapa.
4. Provozní log a dlouhodobé rozhodnutí jsou různé věci.
5. Historická rozhodnutí se nemají mazat, ale musí být zřejmé, zda stále platí.
6. Nová generace projektu musí explicitně převzít důležité dědictví starší generace.
7. Failure je cenná znalost. Jeho zapomenutí vede k opakování drahých slepých cest.
8. Architektura má následovat ověřený produkt nebo potřebu, nikoli naopak.
9. Více automatizace automaticky neznamená lepší systém.
10. Před drahým nebo destruktivním krokem proveď nejmenší reverzibilní test.
11. Zdrojový systém musí zůstat autoritativní. Agregace a prezentační vrstvy jsou odvozené.
12. Dokumentace musí novému člověku nebo AI říct nejen „co existuje“, ale hlavně „co mám číst teď a proč“.

---

## 16. Nejvyšší pravidlo

Když vznikne nová informace, neptej se nejdříve:

**„Jaký nový dokument mám vytvořit?“**

Ptej se:

**„Kam tato znalost podle své povahy patří?“**

Teprve pokud pro ni neexistuje správný domov, vzniká nový dokument.

Cílem Project Memory není mít co nejvíce Markdown souborů.

Cílem je, aby nový člověk nebo nová AI instance dokázali rychle obnovit správný kontext, pochopit současnost, respektovat minulá rozhodnutí a pokračovat bez opakování starých chyb.

---

## 17. Životní cyklus

Základní cyklus práce:

**READ → WORK → LEARN → CONSOLIDATE → READ**

Nejdříve načti existující znalost.  
Potom pracuj.  
Z práce vytěž rozhodnutí, failures a lessons.  
Konsoliduj je do Project Memory.  
Příští práce začíná z této aktualizované paměti.
