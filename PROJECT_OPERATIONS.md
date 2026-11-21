# Project Operations Standard

**Status:** CANONICAL — metodický kontrakt, nikoli potvrzení nasazené implementace.
**Purpose:** Jednotný význam pracovních uzlů a jejich řízení napříč projekty; živá data patří do centrální Supabase, společný pohled poskytuje Kancelář i projektové aplikace.

## 1. Oddělení odpovědností
Project Memory uchovává trvalé znalosti, rozhodnutí, architekturu a význam workflow. Centrální Supabase je zamýšlený jediný autoritativní zdroj pro pracovní uzly, jejich vlastníky, stavy, termíny, vazby a provozní historii. Kancelář a projektové aplikace jsou různé pohledy nad stejnými daty, nikoli paralelní evidence. Projektové databáze nadále spravují odborná/produkční data (např. skeny, stránky, články, geologická měření). Zápis tohoto standardu sám o sobě neznamená, že schéma či integrace již existují.

## 2. Univerzální model
Maximální pojmová hloubka: **Project → L1 → L2 → L3 → Workflow → Stage → Task**. Project je dlouhodobá identita dle PROJECT_STANDARD; ostatní úrovně jsou **volitelné**, opakovatelné a lze je přeskočit. Workflow i samostatný Task lze navázat přímo na Project nebo na existující L1/L2/L3; Stage lze vynechat. Nenutíme projekt vytvářet prázdné mezistupně. L1–L3 jsou obecné pracovní objekty, Workflow je konkrétní průběh procesu, Stage jeho významová fáze, Task vykonatelná jednotka práce. Workflow může existovat vícekrát nad různými objekty. Odlišuj definici opakovatelného procesu od jeho konkrétní instance.

Projekty si samy určují názvy, význam, použité úrovně, odborná pravidla a případná rozšíření. Kancelář zobrazuje pouze skutečně naplněné úrovně se stejnými pravidly zobrazení, filtrování a řízení. Typy uzlů zůstávají standardní: `L1`, `L2`, `L3`, `WORKFLOW`, `STAGE`, `TASK`; projekt mění jejich názvy a význam, nikoli základní kontrakt. Vazby mohou překračovat strom: úkol uvnitř QA může odkazovat na konkrétní stránku či článek, aniž by byl jejím hierarchickým potomkem.

## 3. Společná struktura work node
Každý provozní uzel sdílí obecný kontrakt (fyzické SQL schéma není tímto textem předepsáno):
- **Identita:** stabilní `id`, `project_id`, standardní `type`.
- **Umístění:** volitelný `parent_id`, pořadí `position`; vztah rodič–potomek musí respektovat smysluplnou hierarchii, bez cyklů.
- **Význam:** `title`, `description`, projektové zobrazované označení úrovně.
- **Odpovědnost:** právě jeden odpovědný `owner_id` (člověk či agent); případní další `assignees` jsou spolupracovníci, nikoli sdílená nejasná odpovědnost.
- **Řízení:** `status`, `priority`.
- **Čas:** `start_at`, `due_at`, `completed_at` dle povahy a stavu uzlu.
- **Dokončení:** `acceptance_criteria`, `result` a případné doklady schválení.
- **Provenience:** `created_at`, `updated_at`, vazby na zdroje, výsledky a relevantní externí identifikátory.
- **Rozšíření:** projektová `metadata`, která nenahrazují společná pole.

Identita, typ, projekt, název, vlastník a stav jsou povinné pro všechny aktivně řízené uzly. Popis a kritéria dokončení musí být dostatečně určité pro posouzení výsledku; u Tasku jsou povinné. Aktivní Task musí mít termín; backlog může zůstat bez termínu. U L1–L3, Workflow a Stage je termín podle povahy objektu volitelný. Všechny úrovně mají vlastníka a stav. Při dokončení se eviduje výsledek a skutečný čas; odborné schválení není automaticky totéž co technické dokončení.

## 4. Stavy, priority, průběh
Společné základní stavy: `BACKLOG`, `READY`, `IN_PROGRESS`, `BLOCKED`, `IN_REVIEW`, `DONE`, `CANCELLED`. Ne každá práce musí projít všemi stavy; `IN_REVIEW` je volitelná etapa životního cyklu. Konkrétní přechody, oprávnění a automatické podmínky budou specifikovány až při implementaci. Priorita má jednotnou srovnatelnou stupnici definovanou centrálním provozním systémem; projekt může doplnit odborné třídění.

**Stav**, **vypočtený pokrok** a **schválení** jsou tři různé veličiny. Stav rodiče se automaticky neodvozuje ze stavů potomků bez výslovného pravidla. `DONE` znamená splněná kritéria, nikoli jen vyplněné procento.

## 5. Vazby
Odděluj:
1. **Hierarchii** (`parent_id`): kam uzel patří.
2. **Závislosti**: co musí předcházet nebo co blokuje dokončení.
3. **Reference**: související uzly, odborné entity, zdroje, artefakty a rozhodnutí.
Závislosti a reference mohou být mnohonásobné; jeden `parent_id` je nenahradí. Vhodná je samostatná evidence typovaných vazeb (pracovně `work_node_links`). Nevytvářej cyklické blokující závislosti. Externí reference používají stabilní identifikátory a označení zdrojového systému, ne kopie odborných dat.

## 6. WORKFLOW.md v projektové paměti
Volitelný `project-memory/WORKFLOW.md` je **volný významový popis**, bez povinné šablony a bez provozních instancí. Vysvětluje účel workflow, význam fází, návaznosti, odborné podmínky, kritéria kvality a rozdíl mezi technickým dokončením a odborným schválením. Není strojovou definicí procesu, seznamem tasků, runtime logem ani duplikátem Supabase. Pokud workflow neexistují, soubor není potřeba. Změny významu procesů se konsolidují do WORKFLOW.md, významná rozhodnutí do DECISIONS, provozní stavy pouze do Supabase.

## 7. Příklad PALME Pilot
Project = PALME Pilot; L1 může být konkrétní vydání novin, L2 stránka, L3 obsahová jednotka. Workflow = Editorial a Quality Assurance; Stage = odborně pojmenovaná fáze; Task = konkrétní práce. Workflow lze spustit nad vydáním nebo stránkou; úkol může odkazovat na článek. Stránky a články jako odborné entity zůstávají v projektovém systému. Toto je ilustrace modelu, nikoli tvrzení o nasazeném schématu PALME.

## 8. Implementační hranice
Tento dokument definuje **významový a minimální datový kontrakt**. Nezavádí fyzické tabulky, automatizace, migrační postupy, oprávnění, API ani konkrétní workflow běhy. Tyto věci je třeba samostatně navrhnout a ověřit před nasazením. Centrální registr projektů v `projects.yaml` nadále slouží k nalezení Git Source of Truth; centrální provozní evidence se s ním propojí stabilní identitou projektu, ne duplicitním ručním opisováním.
