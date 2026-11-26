# Znalostní mapa AI agentů

**Úvodní analytická Page · verze 0.1 · 26. listopadu 2026**

**Účel:** dlouhodobá, kriticky udržovaná znalostní základna o architektuře a provozu AI agentů. Nejde o katalog názorů ani sbírku izolovaných shrnutí. Každý nový text má měnit, zpřesňovat nebo zpochybňovat společnou mapu poznatků.

**Výchozí otázka:** Co v agentních systémech skutečně funguje, za jakých podmínek, s jakými náklady a jak silné důkazy máme?

**Zásada:** *Zdroj není poznatek. Autorovo tvrzení není důkaz. Popularita není míra pravdivosti. Název „engineering“ sám o sobě nevymezuje novou disciplínu.*

**Stav zpracování:** první dva výchozí články zařazeny; jeden dostupný jako PDF s extrahovaným textem, druhý jako webový přepis. Základní slovník a počáteční evidence vytvořeny; experimentální ověřování dosud neprovedeno.

## 1. Architektura znalostního projektu

Doporučený model tvoří pět propojených registrů. Není nutné vytvářet pět izolovaných dokumentů: rozhodující jsou **stabilní identifikátory a vzájemné odkazy**, které později dovolí data přesunout do GitHubu, databáze nebo znalostního grafu.

1. **Zdroje [S]**: originální článek, jeho kopie, autor, datum, URL, druh, dostupný rozsah, změny a původ.

2. **Tvrzení [C]**: co přesně autor říká; jedno samostatně ověřitelné tvrzení na záznam, odkaz na pasáž.

3. **Pojmy [P]**: pracovní definice nezávislé na jednom autorovi, alternativní definice, hranice použití a vazby.

4. **Důkazy a testy [E]**: studie, kód, benchmark, metodika, lokální experiment, negativní výsledek, limit přenositelnosti.

5. **Rozpory a otázky [R, Q]**: střet definic či empirických výsledků, konkurenční vysvětlení, co by spor rozhodlo.

**Pravidlo normalizace:** Jeden zdroj může obsahovat mnoho tvrzení. Jedno tvrzení může mít více důkazů. Jedno téma může mít několik legitimních definic. Konsenzus se zapisuje až jako *syntéza*, nikdy jako tichý přepis původního výroku.

**Vyhledávací osy:** agentní architektura; kontext a paměť; běh a orchestrace; nástroje a oprávnění; kontrola a evaluace; náklady a výkon; autonomní a multiagentní systémy; bezpečnost; metodologie benchmarků.

## 2. Pracovní mapa pojmů a vztahů

Nejvhodnější výchozí model není ani čtyřpatrový, ani pětipatrový žebřík. Je to **systém komponent a procesů**, jejichž hranice se v konkrétních implementacích překrývají.

```text
ZADÁNÍ / CÍL / KRITÉRIA DOKONČENÍ
           │
           ▼
ORCHESTRACE / WORKFLOW (případně graf úloh)
           │ spouští a řídí
           ▼
AGENTNÍ SMYČKA: pozoruj → rozhodni → jednej → ověř → pokračuj/ukonči
           │
           ├─ CONTEXT ASSEMBLY ─► instrukce + vybrané znalosti + stav + historie
           │                                   │
           │                                   ▼
           ├─────────────────────────────────► MODEL
           │                                   │
           ├─ NÁSTROJE ◄───────────────────────┘
           │
           └─ KONTROLA: testy, limity, schvalování, logování, obnova

HARNESS = konkrétní provozní software a pravidla,
          které tyto části bezpečně spojují a vykonávají.

PAMĚŤ / RAG / ÚLOŽIŠTĚ STAVU zásobují kontext a obnovují běh.
EVALUACE měří kvalitu výstupu i celého systému.
GOVERNANCE (oprávnění, audit, rozpočty) protíná všechny vrstvy.
```

**Podstatné rozlišení:** *Loop* je chování a řídicí logika opakování. *Harness* je provozní aparát, v němž může být tato smyčka implementována. *Graph* je vyjádření závislostí mezi úlohami či běhy; může být realizován uvnitř harnessu nebo sloužit jako návrhový model orchestrace. Žádné z těchto slov automaticky neznamená vyšší vyspělost než ostatní.

Vazby zapisujeme v podobě **SUBJEKT — VZTAH — OBJEKT**: `harness — vykonává — loop`; `loop — skládá — context snapshot`; `context snapshot — obsahuje — prompt`; `workflow graph — koordinuje — úlohy/běhy`; `verifier — kontroluje — výstup či stav`; `memory — dodává — relevantní minulý stav`.

## 3. Základní slovník a počáteční stav poznání

| ID | Pojem | Pracovní definice | Stav definice |
|---|---|---|---|
| P-01 | **Prompt engineering** | Návrh instrukcí, příkladů a požadovaného formátu pro interakci s modelem. | **Vysoká** shoda na jádru; hranice vůči context engineering jsou neostré. |
| P-02 | **Context engineering** | Výběr, organizace a sestavení informací, nástrojových popisů, historie a instrukcí dostupných modelu při konkrétním kroku. | **Vysoká** shoda na principu; rozsah pojmu se mezi autory liší. |
| P-03 | **RAG / retrieval** | Vyhledání podkladů z externí báze a jejich vložení do kontextu pro daný krok. | **Vysoká** technická ustálenost; je to mechanismus, nikoli celá agentní architektura. |
| P-04 | **Agentní smyčka (loop)** | Opakovaná posloupnost rozhodování, akce, pozorování výsledku a kontroly cíle. | **Vysoká** shoda na jádru; terminologie „loop engineering“ je méně ustálená. |
| P-05 | **Harness** | Software a konfigurace kolem modelu: nástroje, orchestrátor, stav, oprávnění, retry, kontroly a telemetrie. | **Střední** terminologická shoda; reálné implementace i případové studie existují. |
| P-06 | **Workflow / graph orchestration** | Řízení úloh jako uzlů a závislostí; umožňuje větvení, paralelismus, spojování a schvalovací brány. | **Vysoká** shoda na principu grafového workflow; „Graph Engineering“ jako pátý stupeň není standard. |
| P-07 | **Paměť agenta** | Mechanismy uchování a opětovného využití stavu, rozhodnutí a zkušeností mezi kroky či běhy. | **Střední** shoda; výraz „memory“ označuje více různých technik. |
| P-08 | **Evaluace / verification** | Měření výsledků a ověřování podmínek dokončení mimo samotné prohlášení modelu. | **Vysoká** shoda na významu; obtížné je zvolit validní metriku. |
| P-09 | **Multi-agent** | Více oddělených rolí či instancí, které spolupracují na úlohách. | **Střední**; přínos vůči jednomu agentovi s nástroji závisí na úloze. |
| P-10 | **Autonomie** | Míra, v jaké systém spouští, provádí a vyhodnocuje kroky bez průběžného zásahu člověka. | **Střední**; není jediné univerzální měřítko. |

**„Vysoká/střední“ zde vyjadřuje důvěru ve *stabilitu pracovní definice*, nikoli univerzální důkaz o efektivitě příslušné techniky.**

## 4. Dva výchozí články: shoda a skutečný rozdíl

**S-001 · @beamnxw: „Context vs Prompt vs Loop vs Harness Engineering: The Four-Layer Agent Stack“**  
Dostupný je uživatelem dříve vložený PDF export (11 stran), ze kterého bylo načteno 203 řádků strojově rozpoznaného textu. Textové pasáže jsou dostupné, avšak některé obrázkové diagramy a jejich detaily nemusí být zachyceny v textové vrstvě. [Odkaz na původní příspěvek](https://x.com/beamnxw/status/2119806541821776215). Autorův model je **vnořená architektura**: harness realizuje loops; kroky smyčky sestavují context; context obsahuje prompts. Silné stránky: diagnostika poruch podle místa vzniku, konkrétní příklady nástrojů a kontrol. Slabiny: neostré hranice mezi pojmy, rétorika pokroku a zobecňování některých benchmarkových závěrů.

**S-002 · @L1vsun: „Prompt → Context → Harness → Loop → Graph: The 5 Levels of AI Engineering“**  
Původní příspěvek: [X, 31. 10. 2026](https://x.com/L1vsun/status/2116519560631205914). Byl načten [webový textový přepis na teapaw.org](https://www.teapaw.org/L1vsun/article/2116519560631205914), nikoli autentizovaný originální obsah X se všemi vizuálními přílohami. Autorův model je **postupová mapa pracovní vyspělosti**: od ručního promptu přes kontext a harness k automatizovaným loopům a rozvětvenému grafu. Přidává čitelné příklady Stop hooků, rozpočtových limitů a `split → fan-out → verify → merge`. Slabiny: zaměňuje didaktické pořadí osvojování dovedností s hierarchií softwarových vrstev a používá silné formulace o automatizaci a úsporách, které je nutné testovat.

**Společný základ:** dobrý výsledek neurčuje pouze formulace promptu; význam mají relevantní podklady, nástroje, opakování, kontrola dokončení, stav a bezpečné provozní mechanismy.

**Hlavní neshoda:** @beamnxw píše `Harness → Loop → Context → Prompt` (implementační obsahování), zatímco @L1vsun píše `Prompt → Context → Harness → Loop → Graph` (výuková posloupnost). Nejde nutně o empirický rozpor. **Směšují se dvě různé otázky: „co je uvnitř čeho?“ a „co se člověk naučí nejdřív?“** Obě mapy jsou heuristiky, nikoli obecně přijaté technické normy.

**Pracovní syntéza v0.1:** Zachovat jejich užitečné pojmy, ale odmítnout pevný počet „úrovní“. Pro výzkum používáme komponentovou mapu, pro výuku lze použít postupné stupně, jsou-li jasně označeny jako pedagogická pomůcka.

## 5. Registr zdrojů · počáteční korpus

| ID | Pramen | Typ a dostupnost | Jak jej používáme |
|---|---|---|---|
| S-001 | [@beamnxw · Four-Layer Agent Stack](https://x.com/beamnxw/status/2119806541821776215), 9. 11. 2026 dle PDF záhlaví | Popularizační příspěvek; **PDF text načten**, obrazové části neúplně v textovém extraktu | Zdroj autorových definic, diagnostických příkladů a zobecnění. |
| S-002 | [@L1vsun · 5 Levels](https://x.com/L1vsun/status/2116519560631205914), 31. 10. 2026 | Popularizační příspěvek; [textový webový přepis](https://www.teapaw.org/L1vsun/article/2116519560631205914) načten; původní obrázky nejsou považovány za nezávisle ověřené | Zdroj alternativní taxonomie a návrhů workflow. |
| S-003 | [Vivek Trivedy / LangChain · Improving Deep Agents with harness engineering](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering), 17. 2. 2026 | Primární technická případová studie s popisem benchmarku; **autorem reportovaný experiment** | Konkrétní empirický doklad, který je třeba interpretovat v mezích dané sestavy. |
| S-004 | [Ryan Lopopolo / OpenAI · Harness engineering](https://openai.com/index/harness-engineering/), 11. 2. 2026 | Technická zkušenost z vlastního produkčního vývoje; **case study, nikoli kontrolovaný pokus** | Praktické mechanismy: krátký navigační dokument, testovatelné architektonické invarianty, dohledatelnost, „doc gardening“. |
| S-005 | [Addy Osmani · Loop Engineering](https://addyosmani.com/blog/loop-engineering/), 7. 6. 2026 | Autorská koncepce a praktický výklad, s vlastní výhradou k ranosti směru a nákladům | Přímý kontext pro užití termínu „loop engineering“. |
| S-006 | [Wang et al. · Rethinking the Evaluation of Harness Evolution for Agents](https://arxiv.org/abs/2607.12227), 14. 7. 2026 | Výzkumný preprint; metodologická kritika automatické optimalizace harnessu | Kontrapunkt: přeučení na benchmark, férové rozpočty, přenos výsledků. |

**Stav provenience:** S-001 a S-002 jsou první dva analyzované *výchozí* články. S-003 až S-006 tvoří doplňující ověřovací a kritické prameny. Záznam přítomnosti v korpusu neznamená, že byly nezávisle zopakovány všechny jejich experimenty. Dřívější konverzace jsou zdrojem *výzkumných otázek a interpretací*, nikoli nezávislým důkazem.

## 6. Registr tvrzení · co víme a co jen někdo říká

Legenda typu: **F** = ověřitelný popis architektury či zdroje, **E** = empirické tvrzení, **H** = hypotéza / generalizace, **M** = marketingové či normativní tvrzení. Typ nevyjadřuje sám o sobě pravdivost. Důkazní stupně: **D0** bez dokladu; **D1** zdroj uvádí příklad či argument; **D2** existuje konkrétní studie s metodou; **D3** nezávislá replikace napříč podmínkami.

| ID | Výrok (vlastní parafráze) | Typ · stupeň | Následující krok |
|---|---|---|---|
| C-001 | Model v jednom volání reaguje na dodaný kontext; změna kontextu může zásadně změnit výsledek. | F · D1 | Stanovit přesné hranice podle rozhraní modelu; měřit na stejných úlohách. |
| C-002 | Harness spojuje model, nástroje, řízení stavu, kontroly a běh. | F · D1 | Srovnat definice v kódu a dokumentaci několika frameworků. |
| C-003 | Pro agentní úkol lze kontrolovat „hotovo“ nezávislým testem, nikoli jen deklarací modelu. | F · D1 | Rozlišit binární strojové testy od otevřených kvalitativních úloh. |
| C-004 | LangChain zvýšil úspěšnost svého coding agenta na Terminal-Bench 2.0 z **52,8 % na 66,5 %** při fixním `gpt-5.2-codex`; změnil harness. | E · D2, **autorem reportováno** | Ověřit protokol, běhy, rozptyl a přenos na jiné úlohy; neredukovat na obecné pravidlo. |
| C-005 | „Lepší harness je často důležitější než lepší model.“ | H · D1 | Faktoriální experiment: stejný model × různé harnessy, pak různé modely × stejný harness. |
| C-006 | Pět kroků Prompt→Context→Harness→Loop→Graph je univerzální vývojový žebřík. | M/H · D0 | Zachovat pouze jako autorovu didaktickou metaforu, ne jako konsenzus. |
| C-007 | Paralelní graf s nezávislým ověřením může snížit latenci a zlepšit pokrytí rozložitelné úlohy. | H · D1 | Porovnat náklady, koordinaci, kvalitu a faktickou nezávislost úkolů. |
| C-008 | Automaticky evolvovaný harness vykáže stabilní zlepšení i na dosud neviděných úlohách. | H · **sporné** | S-006 uvádí slabou generalizaci a nutnost kontrolovat test-time compute. |
| C-009 | Stručný rozcestník do projektové dokumentace je pro agenta praktičtější než monolitický soubor instrukcí. | H/E · D1 (S-004 případová zkušenost) | A/B test relevantnosti kontextu, oprav a ceny na konkrétním projektu. |

**Výklad C-004 vs. C-008:** Zlepšení v jednom reportovaném benchmarku a slabší generalizace v jiné studii si neodporují přímo. Testují odlišné zásahy a protokoly. Rozpor se týká *šíře zobecnění*. Názvy benchmarků a číselné rozdíly vždy ponechávat s konkrétním modelem, sestavou a verzí benchmarku.

## 7. Evidence rozporů a otevřených sporů

| ID | Spor | Konkurenční vysvětlení | Co jej rozhodne |
|---|---|---|---|
| R-001 | **Harness vs. Loop: co je „nad“ čím?** | S-001 popisuje implementační vnoření; S-002 pořadí dovedností; S-005 jiný řez automatizací. | Oddělené diagramy: runtime architektura vs. výuková posloupnost. Spíše terminologický střet. |
| R-002 | **Je přínos harnessu větší než přínos modelu?** | S-003 ukazuje významný výsledek se stejným modelem; nelze z něj vyvodit převahu pro všechny modely a úlohy. | Kontrolovaný benchmark s totožnými daty, rozpočty, časem a více modely; analýza rozptylu. |
| R-003 | **Je Graph „pátou úrovní“?** | S-002 chápe graf jako vyšší fázi; standardní workflow může mít graf i při malém počtu agentů. | Rozlišit reprezentaci závislostí, paralelní orchestraci a skutečně vícerolové systémy. |
| R-004 | **Zlepšení benchmarku vs. generalizace.** | Optimalizace může využívat specifika testu; S-006 ukazuje rizika sdílených testovacích úloh a dodatečného výpočetního rozpočtu. | Oddělené trénovací/validační/testovací úlohy, fixní rozpočty, nezávislé evaluace. |
| R-005 | **Autonomní dokončení vs. prokazatelná kvalita.** | Úspěšné testy mohou potvrdit jen omezený soubor invariantů, nikoli všechny vlastnosti výsledku. | Vícevrstvá evaluace: automatické testy, adversariální kontroly, lidský audit, následné chyby. |

U každého sporu ukládáme: **datum zjištění, ID dotčených tvrzení, přesnou povahu neshody, alternativy, rozhodující důkazy, stav (otevřený / vysvětlený / rozhodnutý / znovu otevřený)**. Rozdílné definice nebudeme předstírat jako rozdílné výsledky experimentů.

## 8. Jednotná metodika hodnocení zdrojů

### Dvě nezávislé osy

**A. Hodnocení *zdroje*, nikdy ne automaticky pravdivosti všech jeho vět.** Pět kritérií po 0–3 bodech (celkem 0–15): **P** původnost a identita autora; **T** transparentnost metod a konfigurace; **O** ověřitelnost důkazů (data, kód, logy); **R** reprodukovatelnost; **N** nezávislost na komerčním zájmu či nezávislé kontroly. Používat „nehodnoceno“, pokud materiál nestačí. Součet je *priorita k dalšímu prověření*, nikoli procento pravdy. Text vysvětlující pojmy může být užitečný i při nízkém skóre experimentální doloženosti.

**B. Hodnocení *jednotlivého tvrzení*.** U každého výroku zvlášť zapisovat:

- **povahu**: definice / popis systému / empirický výsledek / kauzální závěr / prognóza / návod / reklama;

- **podklad** D0–D3; **jistotu** nízká / střední / vysoká; **přenositelnost** úzká / neznámá / opakovaně doložená;

- **závislosti**: model, benchmark, sestava harnessu, dataset, nákladový limit, datum a případné střety zájmů;

- **negativní důkazy** a známé protiklady, nikoli jen citace potvrzující výrok.

### Jak odhalit marketingový balast

Indikátory ke kontrole: „jediná správná cesta“, „nová úroveň“, „vždy“, „10× lepší“, univerzální rady bez výchozí metriky, extrapolace z jednoho benchmarku, zaměňování úspor tokenů za kvalitu, anekdoty vydávané za replikaci a přesměrování k produktu. Přítomnost těchto znaků není důkaz nepravdy. Znamená **oddělit jádro tvrzení od rétoriky a vyžádat měření**.

### Pravidlo citace

Ke každému tvrzení uchovat: `source_id`, přesné místo v textu (sekce/odstavec/strana), krátkou parafrázi, případně krátký doslovný citát, datum přístupu a informaci, **zda byl dostupný originál, kopie, textový extrakt, nebo pouze výstřižek**. Citace na druhotný výtah není automaticky citací původní studie.

## 9. Postup zpracování každého nového článku

**Krok 1 · Příjem.** Uložit URL nebo soubor, datum, původ, verzi a rozsah dostupného textu. Spojit duplikáty, ale nepřepisovat odlišné verze.

**Krok 2 · Rozložení.** Vytěžit 3–10 atomických tvrzení. Každé musí být formulováno tak, aby se dalo potvrdit, vyvrátit nebo přesně omezit.

**Krok 3 · Napojení.** Pro každé tvrzení určit existující pojem [P], potvrzované či zpochybňované tvrzení [C] a případný rozpor [R]. Novou kategorii zavést jen tehdy, když staré nepostačují.

**Krok 4 · Kritika.** Označit autorovu definici, praktický návod, empirický výsledek, nepodloženou kauzalitu a rétoriku. U benchmarků kontrolovat: fixní model? stejné zdroje? rozpočet? data? počet běhů? otevřený kód? oddělený test?

**Krok 5 · Syntéza.** Aktualizovat *jednu společnou mapu* a příslušné registry; nepsat další volně stojící shrnutí. Původní názor autora zachovat i při nesouhlasu.

**Krok 6 · Revize.** Zapsat, která definice či úroveň jistoty se změnila, proč, a co zůstává sporné. Staré závěry nezmizí: budou nahrazeny novou verzí s odůvodněním.

**Krok 7 · Akce.** Tam, kde poznatek ovlivní návrh vlastního systému, připojit testovatelnou hypotézu a praktické kritérium úspěchu.

### Jednotná karta zdroje (kopírovat při každém dalším příjmu)

```text
S-XXX | Název | Autor | Datum publikace | URL | Datum přístupu
Forma: primární článek / preprint / benchmark / case study / názor / reklamní text
Dostupnost: originál / zrcadlo / PDF text / obrázky / výtah / pouze metadata
Co je nové oproti mapě?:
Tvrzení: C-... (jedno na řádek; s místem v prameni)
Co zdroj skutečně dokládá?:
Co jen předpokládá nebo slibuje?:
Konflikty / alternativy: R-...
Hodnocení zdroje: P_/3 T_/3 O_/3 R_/3 N_/3 (nebo nehodnoceno)
Co je třeba ověřit?:
Změněná místa mapy a datum revize:
```

### Jednotná karta tvrzení

```text
C-XXX | Přesný výrok / parafráze | P-... | S-... a pasáž
Typ: definice / architektura / empirické / kauzální / hypotéza / marketing
Důkazy: E-... | D0–D3 | jistota | rozsah platnosti
Protiargument / alternativní hypotéza:
Co by výrok vyvrátilo?:
Závislé proměnné a kontext experimentu:
Stav: otevřené / předběžně podpořené / opakovaně podpořené / vyvrácené / zastaralé
Historie změn:
```

## 10. Otevřené výzkumné otázky a návrhy testů

**Q-001 · Kde přesně končí „context engineering“ a začíná harness?** Pojmová analýza několika frameworků a jejich skutečných runtime komponent. Hypotéza: nejde o striktní vrstvy; context assembly je služba uvnitř harnessu.

**Q-002 · Jak velký je přínos strukturované projektové paměti?** Srovnat (A) celý chat, (B) stručná pravidla + ručně vybraný kontext, (C) selektivní retrieval. Měřit kvalitu odpovědí, počet oprav, latenci, náklady a chyby z irelevantních informací.

**Q-003 · Co přesně přidá verifier?** Na sadě stejných úloh porovnat běh bez kontroly, s modelovou sebekontrolou a s externím strojovým testem. Vyhodnocovat i falešné „hotovo“.

**Q-004 · Kdy má smysl graf a multiagentní práce?** Porovnat jeden agent v sekvenčním běhu proti paralelnímu `split–verify–merge` na rozdělitelných i silně závislých úlohách; měřit kvalitu, cenu, latenci a režii sloučení.

**Q-005 · Zlepší harness výkon i mimo benchmark, na němž se ladil?** Použít skrytý testovací soubor a fixní rozpočet. Jediný leaderboard nesmí rozhodovat.

**Q-006 · Jaký je minimální užitečný harness pro vlastní výzkumnou aplikaci?** Hypotéza: vstupní událost → výběr kontextu → model/nástroje → verifikace → zápis stavu → audit + lidské schválení rizikových změn. To je návrh k ověření, nikoli hotová technologie.

**Q-007 · Jak předcházet „epistemickému driftu“?** Jak poznat, že nová populární terminologie pouze přejmenovává staré návrhové principy? Test: přiřadit nové názvy k již existujícím mechanismům a identifikovat skutečně novou predikci nebo měřitelný přínos.

## 11. Provozní pravidla a první backlog

**Při každé další práci s korpusem:**

1. Začít načtením této Page a dosavadních registrů, nikoli pouze posledním článkem.

2. Nový článek nejprve porovnat s [P], [C] a [R]; uvádět, *co přidává*, *co opravuje* a *co opakuje*.

3. Každé tvrzení označit jeho proveniencí a mírou jistoty; nedostupný plný text výslovně přiznat.

4. Chránit staré identifikátory, záznamy důkazů a negativní výsledky.

5. Po aktualizaci vytvořit krátké **„Co se změnilo v mapě“** a přidat záznam do changelogu.

**První tři praktické úkoly:** (a) projít obrázkové diagramy v PDF S-001 a porovnat je s textem; (b) založit první skutečné záznamy [E] pro S-003 a S-006 včetně metodických limitů; (c) navrhnout malý opakovatelný experiment na vlastním agentním workflow, kde se oddělí přínos kontextu, loopu a verifikace.

**Rytmus revize:** po každém novém zdroji inkrementální aktualizace; po 5–10 zdrojích tematická syntéza; jednou měsíčně audit rozporů, neověřených významných tvrzení, zastaralých odkazů a otevřených experimentů. Automatizace může práci připravit, ale nemá potichu přepisovat ověřený stav bez kontroly.

**Budoucí datový model:** S/P/C/E/R/Q lze ukládat v Markdownu s YAML front matter a verzovat v GitHubu; později převést do relačního schématu či znalostního grafu. Tato Page je výzkumný rozcestník a metodický standard, nikoli zatím implementovaná databáze.

### Changelog

- **2026-11-26 · v0.1:** založena mapa; zavedena pracovní taxonomie, pravidla evidence, výchozí korpus S-001 až S-006, tvrzení C-001 až C-009, rozpory R-001 až R-005 a otázky Q-001 až Q-007.

- **Další revize:** vždy datum, konkrétní změna, odkaz na dotčené ID, nový důkaz a důvod revize.