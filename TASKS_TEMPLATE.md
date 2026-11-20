# Tasks Map

**Status:** DRAFT  
**Purpose:** Volitelná mapa zdroje projektových úkolů pro lidi a Kancelář.  
**Authority:** Definice mapování v Git; živé úkoly v uvedeném provozním systému.

> Toto je šablona. Nahraď TBD až po ověření skutečného schématu. Neukládej hesla, tokeny, connection strings ani SQL příkazy.

```yaml
schema_version: 1
project_id: TBD
tasks:
  provider: TBD # např. supabase
  connection_id: TBD # reference na bezpečně spravované připojení Kanceláře
  source: TBD # autoritativní tabulka, pohled nebo API resource
  fields:
    id: TBD
    title: TBD
    status: TBD
    assignee: TBD
    priority: TBD
    due_at: TBD
    work_item_id: TBD # volitelné
    workflow_run_id: TBD # volitelné
    updated_at: TBD
  status_mapping: {} # skutečné hodnoty projektu -> společné zobrazovací stavy
  priority_mapping: {} # volitelné
```

## Pravidla a interpretace

Vysvětli, které úkoly do zdroje patří, jak se přiřazují lidem nebo agentům, jak se vážou na pracovní objekty či běhy workflow a jak se rozpoznává zastaralý stav. Neexistující pole označ TBD nebo v projektovém souboru vynech, pokud je volitelné.

## Bezpečnost a oprávnění

Kancelář čte úkoly autorizovaně přes serverový adaptér. Přítomnost TASKS.md neuděluje právo úkol vytvořit, změnit ani uzavřít. Změnové operace vyžadují zvláštní autorizovaný kontrakt. Jeden úkol má právě jeden autoritativní systém.
