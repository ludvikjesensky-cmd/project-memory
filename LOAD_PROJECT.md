# Load Project Protocol

**Status:** CANONICAL  
**Version:** 1.0  
**Purpose:** Standardní postup pro obnovení kontextu projektu v nové konverzaci.

## 1. Standardní příkaz

Uživatel může říct:

**„LOAD PROJECT: Business Brief“**

nebo přirozeně:

**„Načti Business Brief a pokračujme.“**

## 2. Najdi projekt

1. Otevři centrální `projects.yaml`.
2. Najdi projekt podle ID, názvu nebo jednoznačného aliasu.
3. Použij registrovaný Source of Truth.
4. Každý záznam v registru odpovídá jednomu Repository/Projectu. Pokud uživatel zadá family místo konkrétní generace, preferuj aktivní Project v dané family.

Pokud projekt není v registru, nejdříve ověř, zda existuje známý Source of Truth. Nevytvářej jej automaticky jen proto, že nebyl nalezen.

## 3. Bootstrap

V projektu vždy nejdříve přečti `PROJECT.md`.

Poté načti minimálně:
- SCOPE;
- CURRENT_STATE.

Další paměť čti podle práce.

Pro architekturu:
- ARCHITECTURE;
- DECISIONS;
- FAILURES;
- OPEN_QUESTIONS.

Pro strategii, změnu směru nebo návrat ke starším přístupům:
- HISTORY;
- LESSONS;
- relevantní DECISIONS a FAILURES.

Pro terminologii:
- GLOSSARY.

Detailní docs načítej selektivně podle PROJECT.md a aktuálního úkolu.

## 4. Runtime reality

Pokud otázka závisí na současném provozním stavu, Markdown nestačí.

Použij operational Source of Truth definovaný v PROJECT.md, například Supabase, dashboard, Railway nebo jiný systém.

Rozlišuj:
- documented current state;
- live verified runtime state.

Pokud se rozcházejí, runtime skutečnost je důvodem ke konsolidaci/aktualizaci CURRENT_STATE, ne k ignorování rozdílu.

## 5. Project families a starší generace

Starší Project/Repository ze stejné family načítej jen když:
- je potřeba provenance;
- řeší se staré rozhodnutí;
- hrozí opakování failure;
- současná Project Memory na ně explicitně odkazuje.

Starší Project není současný Project jen proto, že obsahuje více dokumentace.

## 6. Výstup LOAD

Po načtení není nutné uživateli reprodukovat dokumentaci.

Stačí být připraven pokračovat. Pokud je užitečné shrnutí, uveď stručně:
- účel;
- současný stav;
- kritické omezení;
- nejbližší krok.

## 7. Definition of Done

LOAD je hotový, když AI zná dostatek kanonického kontextu pro konkrétní práci a ví, kde ověřit další potřebné skutečnosti.

LOAD neznamená „načti všechno“. Znamená „načti správnou autoritativní znalost pro tuto práci“.
