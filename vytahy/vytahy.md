# Výtahy z doporučené literatury

## 1. The Semantic Scholar Open Data Platform


### O čem článek je

Článek popisuje platformu Semantic Scholar, která spravuje velkou otevřenou databázi odborných článků. V květnu 2025 obsahovala přibližně 225 mil. článků, 105 mil. autorů a 2,8 mld. citací. Data pocházejí z více než 50 zdrojů, takže je potřeba je čistit a odstraňovat duplicity. Stejný článek totiž může přijít z více míst a jeho název nemusí být všude stejný.

Platforma nabízí API, ke kterému lze získat klíč zdarma. K dispozici je relevance search pro běžné dotazy, bulk search pro velké množství výsledků a snippet search, který hledá přímo v textech článků. Poskytuje také krátké souhrny článků (TLDR) a embeddingy SPECTER. Plný text ale není dostupný zdaleka u všech článků. V informatice je to asi 810 tis. z 19 mil.

### Jak ho můžu využít v bakalářce

- Semantic Scholar API můžu použít jako hlavní zdroj dokumentů pro prototyp a popsat ho v části o rozhraní vědeckých databází.
- Podle účelu můžu zvolit typ vyhledávání: bulk search pro sběr kandidátů a snippet search pro hledání konkrétních pasáží.
- TLDR a embeddingy můžu využít při filtrování, takže je nebudu muset počítat sám.
- Postup deduplikace se dá zjednodušit pro prototyp, například porovnáním DOI a podobnosti názvů.
- U článků bez plného textu musí systém pracovat jen s metadaty a abstraktem.

## 2. BEIR


### O čem článek je

BEIR je benchmark pro porovnání vyhledávacích modelů. Obsahuje 18 datasetů z 9 různých úloh a autoři na něm testovali 10 systémů. Zaměřují se na to, jak modely fungují na datech z jiné oblasti, než na které byly natrénované. Ukázalo se, že dobrý výsledek na trénovací doméně nezaručuje dobrý výsledek jinde.

Klasická metoda BM25 je stále velmi silná a řada neuronových modelů ji nepřekonala. Nejlepší výsledky měla kombinace BM25 a cross-encoderu, který přeřazuje nalezené dokumenty. V průměru byla o 11 % lepší než samotné BM25, ale je výrazně pomalejší, přibližně 450 ms na dotaz s GPU a 6,1 s na CPU. Dense modely jsou rychlé, mimo svou doménu však často horší než BM25.

Autoři také upozorňují na zkreslení anotací. Hodnocené dokumenty pocházely hlavně z lexikálních metod, což znevýhodňuje ostatní modely. Po doplnění anotací v datasetu TREC-COVID se model ANCE zlepšil z 0,654 na 0,735.

### Jak ho můžu využít v bakalářce

- BM25 můžu použít jako základ, se kterým porovnám vlastní metodu hodnocení relevance.
- Cross-encoder se hodí jako druhý krok, který přeřadí menší počet nalezených článků. Při tomto množství dokumentů by jeho pomalost neměla vadit.
- Metriky nDCG@10 a Recall@100 můžu použít při ověření metody.
- Při tvorbě vlastní testovací sady je lepší nechat ohodnotit výsledky z více metod, aby sada nezvýhodňovala jednu z nich.
- Model je potřeba otestovat přímo na odborných textech. Datasety v BEIR jsou navíc jen anglické, takže výsledky nemusí platit pro česká zadání.

## 3. Retrieval-Augmented Generation


### O čem článek je

Tento článek zavedl pojem RAG. K dotazu se nejprve vyhledají relevantní dokumenty a jazykový model pak s jejich pomocí generuje odpověď. Autoři použili vyhledávání DPR nad 21 mil. úseky z Wikipedie a generátor BART. Porovnávají dvě varianty: RAG-Sequence používá pro celou odpověď jeden dokument, RAG-Token může pro každý token použít jiný.

V lidském hodnocení generování otázek byl RAG věcně lepší než samotný BART ve 42,7 % případů, BART jen v 7,1 %. Znalosti lze aktualizovat výměnou indexu a model není nutné znovu trénovat. RAG ale nezaručuje, že všechno ve výstupu pochází z nalezených dokumentů, protože část může pocházet ze znalostí modelu. U některých úloh se navíc vyhledávání zhroutilo a vracelo stále stejné dokumenty.

### Jak ho můžu využít v bakalářce

- V teoretické části můžu z článku vysvětlit princip RAG a proč vyhledávání pomáhá proti vymyšleným informacím.
- Prototyp může používat stejné schéma: najít dokumenty, předat je modelu a vygenerovat text. Generátor nemusím trénovat, stačí existující jazykový model.
- Díky oddělení znalostí od modelu můžou do systému přicházet nové články bez dalšího trénování.
- Protože RAG nezaručuje věrnost zdrojům, musím v prototypu kontrolovat citace a věrnost textu měřit.
- Musím vyřešit i případ, kdy se k tématu nic relevantního nenajde.

## 4. OpenScholar


### O čem článek je

OpenScholar je otevřený systém, který odpovídá na vědecké dotazy delším textem s citacemi. Pracuje s 45 mil. článků. Hledá ve vlastním indexu, přes Semantic Scholar a na webu, výsledky přeřadí cross-encoderem a vygeneruje odpověď. Tu potom sám vylepšuje na základě vlastní zpětné vazby a nakonec zkontroluje citace.

Autoři vytvořili také benchmark ScholarQABench s 2 967 otázkami. GPT-4o bez vyhledávání si vymýšlelo 78–90 % citací. V hodnocení odpovědí z informatiky dosáhl OpenScholar s menším modelem 51,1 bodu, GPT-4o 45,0 a lidé 54,0. Experti dali přednost odpovědi OpenScholaru před odpovědí člověka v 51 % případů u menšího modelu a v 70 % u varianty s GPT-4o, hlavně kvůli většímu pokrytí tématu.

Kombinace zdrojů fungovala nejlépe a samotný web nejhůře. Mezi slabiny patří, že systém někdy nenajde nejvýznamnější články a nepracuje s placenými články. Autoři sami netvrdí, že lze rešerši plně automatizovat.

### Jak ho můžu využít v bakalářce

- Je to nejbližší existující řešení, takže ho můžu uvést v analýze existujících nástrojů a porovnat s ním svůj návrh.
- Architekturu můžu převzít ve zjednodušené podobě: vyhledání, přeřazení, generování textu a kontrola citací. Vlastní index 45 mil. článků nepotřebuji.
- Do Semantic Scholaru je lepší posílat klíčová slova vygenerovaná z dotazu, ne celou otázku.
- Experiment můžu postavit podobně a porovnat jazykový model bez vyhledávání, běžný RAG a svoje řešení.
- Sledovat můžu správnost, přesnost citací a pokrytí tématu. Existenci citovaných článků můžu ověřovat přes API.
- V diskusi můžu uvést stejné omezení jako autoři, tedy že systém rešerši podporuje, ale nenahrazuje.

## 5. Ragas


### O čem článek je

Ragas je nástroj pro hodnocení RAG systémů, který nepotřebuje referenční odpovědi od lidí. Hodnocení provádí jazykový model pomocí tří metrik:

- faithfulness: jaká část tvrzení v odpovědi je podložená nalezeným textem
- answer relevance: jestli odpověď řeší položenou otázku
- context relevance: jaká část nalezeného textu je k odpovědi opravdu potřeba

Autoři nástroj ověřili na vlastním datasetu WikiEval z 50 stránek Wikipedie. Shoda s lidským hodnocením byla 0,95, 0,78 a 0,70, nejlépe tedy vyšla faithfulness. Přímé hodnocení od ChatGPT bylo horší (0,72, 0,52 a 0,63). Dataset je ale malý a výsledek závisí na použitém jazykovém modelu. Metriky navíc neměří pravdivost, ale shodu s nalezeným textem, a neukážou, že se důležitý dokument vůbec nenašel.

### Jak ho můžu využít v bakalářce

- V experimentální části můžu Ragas použít k hodnocení vygenerovaných textů bez drahých expertních odpovědí.
- Faithfulness ukáže, jestli text vychází z nalezených článků, context relevance zase kvalitu vyhledávání a filtrování.
- Rozklad textu na tvrzení a jejich ověření proti zdroji můžu použít i přímo v prototypu při kontrole citací.
- Hodnocení je potřeba spustit vícekrát a uvést použitý model, protože výsledky se můžou mezi běhy lišit.
- Ragas neměří úplnost nalezených článků, proto ho doplním o hodnocení relevance dokumentů a menší ruční kontrolu.
