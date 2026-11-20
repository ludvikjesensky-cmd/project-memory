# Workflow Map

**Status:** DRAFT  
**Purpose:** Volitelná mapa opakovatelných procesů projektu pro lidi a Kancelář.  
**Authority:** Definice workflow v Git; skutečné stavy běhů v uvedeném provozním systému.

> Nahraď příklady skutečnými údaji. Hodnoty TBD znamenají dosud neověřené propojení. Nevkládej credentials ani přímé SQL dotazy.

```yaml
schema_version: 1
project_id: TBD
workflows:
  - id: report-production
    name: Tvorba odborné zprávy
    description: Příklad; uprav podle projektu.
    steps:
      - id: intake
        name: Přijetí zadání
      - id: sources
        name: Shromáždění podkladů
      - id: draft
        name: Zpracování návrhu
      - id: review
        name: Odborná kontrola
      - id: release
        name: Finalizace
    runtime:
      provider: TBD # např. supabase; dokud není ověřeno, ponech TBD
      connection_id: TBD # bezpečná reference v Kanceláři, nikoli tajný klíč
      runs_source: TBD # název tabulky/pohledu nebo API resource
      steps_source: TBD
      run_id_field: TBD
      step_run_id_field: TBD
      step_definition_id_field: TBD
      status_field: TBD
      status_mapping: {} # skutečné hodnoty projektu -> společné zobrazovací stavy
```

## Popis procesů

### report-production

Zde vysvětli účel, podmínky zahájení, význam kroků, případné závislosti a schvalovací body. YAML slouží jako strojově čitelný přehled; podrobný popis zůstává v Markdownu nebo odkazované dokumentaci.

## Provozní pravda a dostupnost

Uveď, kdo vlastní provozní data, kde lze ověřit jejich schéma, zda je připojení skutečně funkční a jak se rozlišuje neznámý či zastaralý stav. Kancelář smí stav číst pouze autorizovaně přes serverový adaptér. Žádný zápis či spuštění workflow nevyplývá jen z tohoto souboru.
