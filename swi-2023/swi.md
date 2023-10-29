# SWI poznamky

## Prednaska 1

### Softverove inzinierstvo
- zaobera sa teoriami, metodami a nastrojmi pre profesionalny vyvoj softveru(sw)
- zaobera sa nakladovo-efektivnym vyvojom sw
- je inzinierska disciplina, ktora sa zaobera vsetkymi aspektmi vyroby softveru od pociatocnych stadii az po udrzbu po uvedeni do prevadzky

### Softver(sw)
- sw produkty mozu byt vyvinute pre konkretneho zakaznika alebo trh
- pc programy a suvisiaca dokumentacia

### SW produkty
- genericke => samostatne systemy, specifikaciu vlastni developer
- prisposobene => konkretny zakaznik, specifikaciu vlastni zakaznik

### Zakladne atributy sw
- **udrzatelnost** => sw napisany tak, aby vyhovoval meniacim sa potrebam zakaznikov
- **spolahlivost** a bezpecnost => zabezpecenie, nemal by sposobit fyzicke alebo ekonomicke skody v pripade zlyhania, pouzivatelia so zlymi umyslami by nemali mat pristup
- **efektivnost** => sw by nemal zbytocne vyuzivat systemove zdroje(ram, cpu time)
- **prijatelnost** => zrozumitelny, pouzitelny, kompatibilny, prijatelny pre typ pouzivatelov

### Inzinieska disciplina
- pouzivanie vhodnych teorii a metod na riesenie problemov s ohladom na organizacne a financne obmedzenia

### Zakladne procesne cinnosti swi
- **specifikacia** => definuje sa sw, obmedzenia jeho prevadzky
- **vyvoj** => sw je navrhnuty a naprogramovany
- **validacia** => kontrola sw, co zakaznik vyzaduje
- **evolucia** => uprava pre meniace sa poziadavky zakaznikov/trhu

### Spolahlivost systemu
- odraza mieru dovery pouzivatela v tento system
- **naklady** => drahsie vyvojove techniky, testovanie a overovanie systemu

### Priciny zlyhania
- **porucha hardveru**
- **zlyhanie sw**
- **prevadzkova porucha** => socialno-technicke systemy

### Hlavne vlastnosti spolahlivosti
- **dostupnost** => pravdepodobnost, ze system bude v prevadzke a bude schopny poskytovat sluzby uzivatelom
- **systemova spolahlivost** => system bude spravne poskytovat sluzby
- **ochrana zivota/prostredia** => posudenie ci system sposobi skody
- **informacna bezpecnost** => odolanie nahodnym/umyselnym prienikom
- **odolnost** => posudenie ako dobre system zachova kontinnitu svojich kritickyvh sluzieb(zlyhanie zariadenia, kyberutoky)

### Typy kritickych systemov
- **z hladiska bezpecnosti** => strata zivota, zranenie, poskodenie prostredia
- **systemy kriticke pre danu ulohu/ciel** => zlyhanie ma za nasledok zlyhanie nejakej cielovo orientovanej cinnosti(navigacny system vesmirnej lode)
- **pre podnikanie** => nasledok su vysoke ekonomicke straty

## Prednaska 2

### SW procesy
- **specifikacia**
  - definovanie toho, co ma system robit
  - poziadavky na inziniersky proces:
    - Ziskavanie a analyza
    - specifikacia
    - overenie
- **navrh a implementacia**
  - definovanie organizacie systemu a implementacia systemu(navrh)
  - dizajnerske cinnosti:
    - architektonicky navrh
    - navrh databazy
    - navrh rozhrania
    - vyber a dizajn komponentov

- **validacia**
  - kontrola ci system robi to, co zakaznik chce(**verifikacia, validacia V&V**)
  - testovanie systemu, **Fazy** testovania:
    - **testovanie komponentov**
    - **testovanie systemu**
    - **zakaznicke testovanie**

- **evolucia**
  - zmena systemu v reakcii na meniace sa potreby zakaznika

### Planom riadene procesy
- vsetky procesne cinnosti vopred naplanovane (**planovane procesy**)
- pokrok sa meria podla tohto planu

### agile procesy
- prirastokove planovanie, jednoduchsie zmenit proces tak, aby odrazal meniace sa poziadavky zakaznikov

### SW procesne modely
- **vodopadovy model(waterfall model)**
  - model riadeny planom, pre velke projeky
  - oddelene a odlisne fazy specifikacie a vyvoja:
    - **analyza a definicia poziadaviek**
    - **navrh systemu a sw**
    - **implementacia a testovanie jednotiek**
    - **integracia a testovanie systemu**
    - **prevadzka a udrzba**
  - NEVYHODY neflexibilne rozdelenie projektu na jednotlive etapy stazuje reakciu na zmenu
- **postupny vyvoj**
  - specifikacia, vyvoj a validacia su prepojene navzajom
  - moze byt riadeny planom alebo agile
  - VYHODY znizuju sa naklady na prisposobenie zmenam
  - VYHODY jednoduchsi feedback, rychlejsie dodanie
  - NEVYHODY proces nieje viditelny, struktura systemu ma tendenciu sa degradovat
- **integracia a konfiguracia**
  - system je zostaveny z existujucich konfig. komponentov
  - moze byt riadeny planom alebo agile
  - VYHODY znizene naklady a rizika, pretoze menej sw sa vyvija od nuly
  - VYHODY rychlejsie dodanie a nasadenie systemu
  - NEVYHODY strata kontroly nad vyvojom opatovne pouzitych prvkov

### Znizenie nakladov na prepracovanie
- **predvidanie zmien**
- **tolerancia zmien** => prisposob. relativne nizkym nakladom

### Vyrovnanie sa s meniacimi poziadavkami
- **Systemove prototypovanie**
  - rychlo sa vyvinie verzia systemu, aby sa preverili poziadavky
  - tento pristup podporuje predvidanie zmien
  - VYHODY vylepsena pouzitelnost systemu, kvalita dizajnu, udrzatelnost
  - NEVYHODY prototyp nemusi obsahovat kontrolu/obnovu chyb, co nie je dobre
- **inkrementalne dorucovanie**
  - systemove inkrementy sa dorucuju zakaznikovi na pripomienkovanie a experimentovanie => podporuje vyhybaniu sa zmenam a toleranciu
  - VYHODY prirastokove dourcovanie ma nizsie riziko celkoveho zlyhania projektu

### Procesy - urovne zrelosti sposobilosti
- **lvl 1: pociatocne** => v podstate nekontrolovatelne
- **lvl 2: opakovatelne** => postupy riadenia produktu
- **lvl 3: definovane a pouzivane postupy**
- **lvl 4: organizovane** => strategie manazerstva kvality
- **lvl 5: optimalizovane** => strategie zlepsovania procesov

## Prednaska 3

#### Rozdiely v sprave sw
- sw je **nehmotny produkt** => sw nieje vidiet
- mnohe sw projekty su **jednorazove projekty** => problem predvidania problemu

#### Univerzalne manazerske cinnosti
- **planovanie projektu**
  - za panovanie su zodpovedni projektovi manazeri
  - odhadovanie/planovanie vyvoja projektu a pridelovanie ludi k uloham
- **riadenie rizik**
  - projektovi manazeri posudzuju rizika ktore mozu ovlyvnit projekt
- **riadenie ludi**
  - projektovi manazeri musia vyberat ludi do svojho timu a zaviest sposoby prace ku efektivnemu timovemu vykonu

### Planovanie projektu
- zahrna rozdelenie prace na casti a pridelenie ich clenom projektoveho timu, predvidanie problemov a priprava predbeznych rieseni
- **projektovy plan:** vytvara sa na zaciatku projektu
- je proces rozhodovania o tom, ako bude praca na projekte organizovana

#### Etapy planovania
- **faza navrhu**
  - ked sa uchadzame o zakazku na vyvoj/poskytovanie sw systemu
- **faza spustenia projektu**
  - rozdelenie kto bude na projekte pracovat, ako bude rozdeleny
- **pocas celeho projektu**
  - z monitorovania postupu sa upravi plan

#### Etapy agilneho planovania
- **planovanie vydania**
  - pozera sa niekolko mesiacov dopredu, rozhoduje o funkciach ktore by mali byt sucastou
- **iteracne planovanie**
  - kratkodoby vyhlad, zameriava sa na planovanie dalsieho prirastku systemu
- **pristupy ku agilnemu planovaniu**
  - planovanie v scrume
  - zalozene na riadeni nevybavenych uloh
  - planovacia hra

#### Planom riadeny vyvoj
- je pristup ku sw inzinierstvu, kde je proces vyvoja podrobne naplanovany
- **projektovy plan** stanovuje:
  - **zdroje** dostupne pre projekt (KTO)
  - **rozpis prac** (CO)
  - **harmonogram** vykonavania prace (KEDY)
    - na zaklade kalendara
    - siete aktivit

### Riadenie rizik
- zaobera sa identifikaciou a zostavovanim planov na minimalizaciu rizik

#### Klasifikacia rizika
- **rizika projektu** => ovplyvnit plan alebo zdroje
- **rizika produktu** => ovplyvnit kvalitu alebo vykon SW
- **rizika podnikatelske** => ovplyvnuju organizaciu, ktora vyvyja SW

#### Proces riadenia rizik
- **identifikacia** => produktovych/obchodnych rizik
- **analyza** => posudenie pravdepodobnosti/dosledkov rizik
- **planovanie** => vypracovanie planu na minimalizaciu rizik
- **monitorovanie** => rizik pocas celeho projektu

#### Planovanie rizika
- **strategie vyhybania sa** => pravdepodobnost vzniku rizika je znizena
- **strategie minimalizacie** => znizi sa vplyv rizika na projekt
- **pohotovostne plany** => pre riesenie vznikuteho rizika

### Riadenie ludi
- doslednost, respekt, inkluzia, cestnost, motivacia ludi

#### typy osobnosti
- **orientovany na ulohy**
  - motivaciou je samotna praca (kazdy chce robit to svoje)
- **sebaorientovany**
  - motivaciou je praca na dosiahnutie individualneho ciela (zbohatnut)
- **orientovany na interakciu**
  - motivaciou je pritomnost spolupracovnikov
  - ludia chodia do prace, pretoze to maju radi :D

#### skupinova sudrznost
VYHODY
- kvalitu skupiny vytvoria jej clenovia
- tim sa od seba navzajom uci, spoznavanie prace toho druheho
- delia sa vedomosti, podporuje sa retaktoriny

#### Efektivnost timu
- **ludia v skupine** => potreba roznych ludi, pre rozne ulohy
- **organizacia skupiny** => tak, aby kazdy mohol prispievat podla svojich schopnosti
- **technicka/manazerska komunikacia** => nevyhnutna komunikacia medzi clenmi

#### skupinova organizacia
- **male skupiny**
- **velke projekty**
- **agilny vyvoj**

## Prednaska 4

#### Inzinierstvo s poziadavkami
- proces vytvarania sluzieb, ktore zakaznik od systemu vyzaduje a obmedzenia, za ktorych system funguje a je vyvijany

### Poziadavky
- by mali uvadzat, **co** ma system robit a **aky** ma byt + **navrh** by mal popisovat, **ako** to ma system robit
- moze byt zakladom pre ponuku na zakazku alebo pre samotnu zmluvu

#### Druhy
- **Poziadavky pouzivatelov**
  - prikazy v prirodzenom jazyku + diagramy sluzieb + prevadzkove obmedzenia
- **Poziadavky na system**
  - strukturovany dokument s podrobnym popisom funkcii, sluzieb ...
  - definuje, co sa ma realizovat

#### Funkcionalne poziadavky
- vypisy sluzieb, ktore ma system poskytovat, ako ma reagovat na konkretne vstupy
- mozu uviest, co by system nemal/**mal robit**
- popisuju funkcnost alebo systemove sluzby

- **uplne poziadavky** => popisy vsetkych pozadovanych zariadeni
- **konzistentne** => v popisoch by nemali byt konfliky/rozpory

#### Domenove poziadavky
- sluzby a obmedzenia systemu z oblasti prevadzky

#### Nefunkcionalne poziadavky(NP)
- obmedzenia sluzieb alebo funkcii ponukanych systemom (casove obmedzenia, obmedzenia procesu vyvoja, ...)
- definuju vlastnosti systemu a obmedzenia **aky by mal system byt**


### Kvalifikacia Nefunkcionalnych poziadaviek

##### Poziadavky na produkt
- specifikuju ze sa produkt musi spravat urcitym sposobom

##### Organizacne poziadavky
- su dosledkom organizacnych politik a postupov

##### Externe poziadavky
- vyplyvaju z faktorov, ktore su externe pre system a proces jeho vyvoja

- **ciel** => vseobecny zamer pouzivatela
- **overitelna nefunkcionalna poziadavka**

#### Metriky na specifikovanie NP
- **rychlost**
- **velkost**
- **jednoduchost pouzivania**
- **spolahlivost**
- **robustnost**
- **prenosnost**


#### Inziniersky proces na specifikaciu poziadaviek
- **ziskavanie poziadaviek**
- **analyza poziadaviek**
- **validacia poziadaviek**
- **riadenie poziadaviek**

#### Dokument s poziadavkami na SW
- nieje dizajnovany dokument
- **mal by stanovit, co** by mal system robit

#### Kontrolne kontroly
- **overitelnost** => je poziadavka testovatelna?
- **zrozumitelnost** => je poziadavka spravne pochopena?
- **vysledovatelnost** => je jasne uvedeny dovod poziadavky?
- **prisposobitelnost** => da sa poziadavka zmenit?

#### Planovanie Manazmentu poziadaviek
- identifikacia poziadaviek
- proces riadenia zmien
- politiky sledovatelnosti => definuju vztahy medzi poziadavkami
- podpora nastrojov

#### Riadenie zmeny poziadaviek
- rozhodovanie, ci by sa mala akceptovat zmena poziadaviek
- analyza problemu a specifikacia zmeny
- analyza zmien a kalkulacia
- implementacia zmeny

## Prednaska 5


### **Modelovanie systému**

1. *Systémové modelovanie* je proces vytvárania abstraktných modelov systému, pričom každý model predstavuje iný pohľad na daný systém.
2. *UML* (Unified Modeling Language) je vizuálny grafický jazyk, vyvinutý Boochom, Rumbaughom a Jacobsonom. UML slúži k vizualizácii, špecifikácii, konštrukcii a dokumentácii softvérových systémov. Podľa definície Object Management Group (OMG) je UML štandardizovaným spôsobom zápisu plánov systému, ktorý zahŕňa koncepčné prvky a konkrétne prvky.

### UML

- Unified (Booch , Rumbaugh , Jacobson)
- Modeling (vizuálny, grafický)
- Language (gramatika, syntax, sémantika)

### **Systémové perspektívy:**

- *Externá perspektíva:* Modeluje kontext alebo prostredie systému z vonkajšieho hľadiska.
- *Interakčná perspektíva:* Modeluje interakcie medzi systémom a jeho prostredím.
- *Štrukturálna perspektíva:* Modeluje organizáciu systému a štruktúru údajov, ktoré systém spracováva.
- *Perspektíva správania:* Modeluje dynamické správanie systému a jeho reakcie na udalosti.

### **Typy diagramov UML:**

- *Diagramy aktivít:* Zobrazujú činnosti spojené s procesmi a spracovaním údajov.
- *Diagramy prípadov použitia:* Zobrazujú interakcie medzi systémom a jeho používateľmi.
- *Sekvenčné diagramy:* Zobrazujú interakcie medzi aktérmi a systémom.
- *Diagramy tried:* Zobrazujú triedy objektov v systéme a vzťahy medzi nimi.
- *Stavové diagramy:* Ukazujú, ako systém reaguje na udalosti.

### **Použitie grafických modelov:**

- Uľahčujú diskusiu o existujúcom alebo navrhovanom systéme, aj keď nie sú úplné.
- Slúžia ako dokumentácia existujúceho systému, aj keď nemusia byť úplné.
- Môžu byť použité na podrobný popis systému a generovanie implementácie, ak sú správne a úplné.

### **(1) Externá perspektíva - Kontextové modely**

- Kontextové modely sa používajú na ilustráciu operačného kontextu systému a ukazujú, čo sa nachádza mimo jeho hraníc.
- Architektonické modely ukazujú systém a jeho vzťahy s ostatnými systémami.

### **(2) Procesná perspektíva**

- Procesné modely odhaľujú, ako sa vyvíjaný systém používa v širších obchodných procesoch.
- Na definovanie sa môžu použiť diagramy aktivít UML, ako sú modely podnikových procesov (workflow) a modely transformácií údajov (dataflow).

### **(3) Interakčné modely**

- Modelovanie interakcie používateľov - je dôležité pre identifikáciu požiadaviek používateľov.
- Modelovanie interakcie medzi systémami - pomáha identifikovať komunikačné problémy.
- Modelovanie interakcie komponentov - nám pomáha posúdiť, či navrhovaná štruktúra systému pravdepodobne poskytne požadovaný výkon a spoľahlivosť.
- Na modelovanie interakcií sa používajú UML diagramy prípadov použitia a diagramy sekvencií a komunikácie.

#### **Sekvenčné diagramy**

- Sekvenčné diagramy sú súčasťou UML a používajú sa na modelovanie interakcií medzi aktérmi a objektmi v rámci systému.

### **(4) Štrukturálne modely**

- Štrukturálne modely softvéru zobrazujú organizáciu systému z hľadiska komponentov, ktoré tvoria tento systém a ich vzťahy.

#### **Diagramy tried**

- Diagramy tried sa používajú pri vývoji objektovo orientovaného modelu systému na zobrazenie tried v systéme a asociácií medzi týmito triedami.

### **(5) Modely správania**

- Behaviorálne modely opisujú dynamické správanie systému pri jeho vykonávaní a ukazujú, čo sa deje pri reakcii systému na vstupy z okolia.
- Tieto stimuly sa rozdeľujú na *údaje*, ktoré musí systém spracovať, a *udalosti*, ktoré spúšťajú akcie v systéme.
- Modely stavových strojov zobrazujú stavy systému ako uzly a udalosti ako prechody medzi týmito stavmi. Stavové diagramy sú súčasťou UML a slúžia na reprezentáciu modelov stavových strojov.

#### **Modelom riadené inžinierstvo**

- Modelom riadené inžinierstvo (MDE) je prístup k vývoju softvéru, kde hlavnými výstupmi vývojového procesu sú skôr modely ako programy.

#### **Použitie modelom riadeného inžinierstva**

- Modelom riadené inžinierstvo je stále v ranom štádiu vývoja a nie je jasné, či bude mať významný vplyv na prax softvérového inžinierstva.
- VÝHODY zahŕňajú schopnosť posúdiť systémy na vyšších úrovniach abstrakcie a možnosť automatického generovania kódu, čo ulahčuje prispôsobenie systému novým platformám.
- NEVÝHODY zahŕňajú to, že modely nie sú vždy vhodné na implementáciu a náklady na vývoj nových prekladačov pre nové platformy môžu vyvážiť úspory z generovania kódu.

#### **Architektúra riadená modelom**

- Architektúra riadená modelom (MDA) bola predchodcom obecnejšieho modelom riadeného inžinierstva.

## Prednaska 6

### **Architektonický dizajn**

- Architektonický dizajn zaoberá sa organizáciou softvérového systému a jeho štruktúrou.
- Výstupom architektonického návrhu je model architektúry, ktorý opisuje, ako je systém organizovaný a komunikujú jeho komponenty.

### **Výhody explicitnej architektúry**

- Komunikácia so zainteresovanými stranami:
    - Architektúra sa používa na diskusiu medzi zainteresovanými stranami systému.
- Systémová analýza:
    - Umožňuje analýzu, či systém môže splniť svoje nefunkčné požiadavky.
- Opätovné použitie vo veľkom meradle:
    - Architektúra môže byť opakovane použiteľná v rôznych systémoch.

### **Použitie architektonických modelov**

- Uľahčuje diskusiu o návrhu systému:
    - Abstraktný pohľad na systém je užitočný pre komunikáciu a plánovanie projektov.
- Dokumentovanie architektúry:
    - Cieľom je vytvoriť model systému, ktorý zobrazuje komponenty, ich rozhrania a prepojenia.

### **Architektonické reprezentácie**

- *Krabicové a čiarové diagramy* sú veľmi abstraktné, ale užitočné pre komunikáciu a plánovanie projektov.
- Niektorí tvrdia, že *UML* je vhodnou notáciou pre popis architektúr.
- *Architektonické popisné jazyky* (ADL) boli vyvinuté, ale nie sú široko používané.

### **Opätovné použitie architektúry**

- Systémy v rovnakej doméne často majú podobné architektúry, ktoré odrážajú koncepty domény.
- Aplikačné produktové rady sú postavené na základnej architektúre s variantmi podľa požiadaviek zákazníkov.

### **Architektonické pohľady**

- 4 + 1 pohľadový model softvérovej architektúry obsahuje:
    - *Logický pohľad* (objekty alebo triedy objektov).
    - *Procesný pohľad* (interagujúce procesy).
    - *Vývojový pohľad* (rozloženie softvéru počas vývoja).
    - *Fyzický pohľad* (hardvér systému a distribúcia softvérových komponentov medzi procesormi).
    - Súvisiace prípady použitia alebo scenáre (+1).

### **Architektonické vzory**

- Architektonický vzor je štylizovaný popis osvedčenej dizajnérskej praxe, ktorý bol overený v rôznych prostrediach.

### **(1) Organizácia systému**

- Odráža základnú stratégiu štrukturovania systému.
- Najčastejšie sa používajú tri organizačné štýly:
    - Štýl *zdieľaného úložiska údajov*
    - Štýl *zdieľaných služieb a serverov*
    - Abstraktný *strojový alebo vrstvený* štýl.

#### **(1.1) Vrstvená architektúra**

- Používa sa na modelovanie rozhrania podsystémov.
- Organizuje systém do vrstiev, pričom každá vrstva poskytuje rôzne služby.
- Podporuje postupný vývoj podsystémov v rôznych vrstvách.
- V niektorých prípadoch môže byť umelé.

#### **(1.2) Zdieľané úložisko údajov**

- Subsystémy zdieľajú údaje cez *centrálnu databázu* alebo *vlastné distribuované úložiská*.
- Model zdieľania úložiska sa používa na zdieľanie veľkého množstva údajov.

#### **(1.3) Zdieľané služby a servery**

- Model distribuovaného systému, ktorý zahrňuje údaje a spracovanie medzi rôznymi komponentmi.
- Môže byť implementovaný na jednom počítači.
- Obsahuje samostatné *servery* poskytujúce špecifické služby a *klientov* využívajúcich tieto služby.

### **(2) Modulárne štýly rozkladu**

- Štýly rozkladu (pod)systémov na moduly.
- Modul je komponent systému, ktorý poskytuje služby iným komponentom, ale nepovažuje sa za samostatný systém.
- Dva modulárne modely rozkladu:
    - *Objektový model*, kde je systém rozložený na interagujúce objekty.
    - *Model potrubia alebo toku údajov*, kde je systém rozložený na funkčné moduly transformujúce vstupy na výstupy.

#### **(2.2) Architektúra potrubia a filtra**

- Varianty tohto prístupu sú časté, najmä v systémoch na spracovanie údajov.
- *Dávkový sekvenčný model* sa používa, keď sú transformácie postupné.

#### **(3) Štýly ovládania**

- Zaoberajú sa riadiacim tokom medzi podsystémami, na rozdiel od modelu rozkladu systému.

#### (3.1) Centralizované ovládanie:

- Jeden podsystém riadi ostatné.
- (3.1.1) Model odovzdania a vrátenia riadenia: Riadenie sa pohybuje zhora nadol, vhodné pre sekvenčné systémy.
- (3.1.2) Manažérsky model: Použiteľný pre súbežné systémy, kde jeden komponent riadi ostatné procesy.

#### **(3.2) Ovládanie založené na udalostiach**

- Udalosti sú externe generované a riadenie je založené na nich.
- Dva hlavné modely riadené udalosťami:
    - (3.2.1) Vysielací model: Udalosť je vysielaná do všetkých podsystémov, ktoré majú záujem.
    - (3.2.2) Modely riadené prerušením: Používa sa v systémoch v reálnom čase s rýchlym spracovaním udalostí.

#### **(3.2.1) Vysielací model**

- Efektívny pri integrácii podsystémov na rôznych počítačoch v sieti.
- Podsystémy registrujú záujem o konkrétne udalosti, ktoré sú im zasielané.
- Riadiaca politika nie je vložená do správy udalostí, ale rozhoduje o nich samotné podsystémy.

#### **(3.2.2) Riadenie prerušením**

- Používa sa v systémoch v reálnom čase, kde je rýchla reakcia nevyhnutná.
- Existujú typy prerušení s preddefinovaným spracovaním.
- Každý typ prerušenia je spojený s umiestnením pamäte a prepínač prerušení vedie k spracovaniu udalosti.
- Zabezpečuje rýchlu odozvu, ale vyžaduje zložité programovanie a overenie.
