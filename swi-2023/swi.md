# SWI poznamky

## Prednaska 1

### Softverove inzinierstvo
- zaobera sa teoriami, metodami a nastrojmi pre profesionalny vyvoj softveru(sw)
- zaobera sa nakladovo-efektivnym vyvojom sw
- je inzinierska disciplina, ktora sa zaobera vsetkymi aspektami vyroby softveru od pociatocnych stadii az po udrzbu po uvedeni do prevadzky

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
- pokrok sa mera podla tohto planu

### agile procesy
- prirastokove planovanie, jednoduchsie zmenit proces tak, aby odrazal meniace sa poziadavky zakaznikov

### SW procesne modely
- **vodopadovy model(waterfall model)**
  - model riadeny planom, pre velke projeky
  - oddelene a odlisne fazy specifikacie a vyvoja:
    - **analyza a definicia poziadaviek**
    - **navrh systemy a sw**
    - **implementacia a testovanie jednotiek**
    - **integracia a testovanie systemu**
    - **prevadzka a udrzba**
  - neflexibilne rozdelenie projektu na jednotlive etapy stazuje reakciu na zmenu
- **postupny vyvoj**
  - specifikacia, vyvoj a validacia su prepojene navzajom
  - moze byt riadeny planom alebo agile
  - znizuju sa naklady na prisposobenie zmenam
  - jednoduchsi feedback, rychlejsie dodanie
  - proces nieje viditelny, struktura systemu ma tendenciu sa degradovat
- **integracia a konfiguracia**
  - system je zostaveny z existujucich konfig. komponentov
  - moze byt riadeny planom alebo agile
  - znizene naklady a rizika, pretoze menej sw sa vyvija od nuly
  - ryclejsie dodanie a nasadenie systemu
  - strata kontroly nad vyvojom opatovne pouzitych prvkov

### Znizenie nakladov na prepracovanie
- **predvidanie zmien**
- **tolerancia zmien** => prisposob. relativne nizkym nakladom

### Vyrovnanie sa s meniacimi poziadavkami
- **Systemove prototypovanie**
  - rychlo sa vyvinie verzia systemu, aby sa preverili poziadavky
  - tento pristup podporuje predvidanie zmien
  - vylepsena pouzitelnost systemu, kvalita dizajnu, udrzatelnost
  - prototyp nemusi obsahovat kontrolu/obnovu chyb co nie je dobre
- **inkrementalne dorucovanie**
  - systemove inkrementy sa dorucuju zakaznikovi na pripomienkovanie a experimentovanie => podporuje vyhybaniu sa zmenam a toleranciu
  - prirastokove dourcovanie ma nizsie riziko celkoveho zlyhania projektu

### Procesy - urovne zrelosti sposobilosti
- **lvl 1: pociatocne** => v podstate nekontrolovatelne
- **lvl 2: opakovatelne** => postupy riadenia produktu
- **lvl 3: definovane a pouzivane postupy**
- **lvl 4: organizovane** => strategie manazerstva kvality
- **lvl 5: optimalizovane** => strategie zlepsovania procesov

## Prednaska 3

#### Rozdiely v sprave sw
- **nehmotny produkt** => sw nieje vidiet
- **jednorazove projekty** => problem predvidania problemu

#### Univerzalne manazerske cinnosti
- **planovanie projektu**
  - za panovanie su zodpovedni projektovi manazeri
  - odhadovanie/planovanie vyvoja projektu a pridelovanie ludi k uloham
- **riadenie rizik**
  - projekt manazeri posudzuju rizika ktore mozu ovlyvnit projekt
- **riadenie ludi**
  - projekt manazeri musia vyberat ludi do svojho timu a zaviest sposoby prace ku efektivnemu timovemu vykonu

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
- je pristup k sw inzinierstvu, kde je proces vyvoja podrobne naplanovany
- **projektovy plan** stanovuje:
  - **zdroje** dostupne pre projekt
  - **rozpis prac**
  - **harmonogram** vykonavania prace
    - na zaklade kalendara
    - siete aktivit

### Riadenie rizik
- zaobera sa identifikaciou a zostavovanim planov na minimalizaciu rizik

#### Klasifikacia rizika
- **rizika projektu** => ovlyvnit plan alebo zdroje
- **rizika produktu** => ovlyvnit kvalitu alebo vykon SW
- **rizika podnikatelske** => ovlyvnuju organizaciu, ktora vyvyja SW

#### Proces riadenia rizik
- **identifikacia** => produktovych/obchodnych rizik
- **analyza** => posudenie pravdepodobnosti/dosledkov rizik
- **planovanie** => vypracovanie planu na minimalizaciu rizik
- **monitorovanie** => rizik pocas celeho projektu

#### Planovanie rizika
- **strategie vyhybania sa** => pravdepodobnost vzniku rizika je znizena
- **strategie minimalizacie** => znizi sa vlyz rizika na projekt
- **pohotovostne plany** => pre riesenie vznikuteho rizika

### Riadenie ludi
- doslednost, respekt, inkluzia, cestnost, motivacia ludi

#### typy osobnosti
- **orientovany na ulohy**
  - motivaciou je samotna praca (kazdy chce robit to svoje)
- **sebaorientovany**
  - motivaciou je praca na dosiahnutie individualneho ciela(zbohatnut)
- **orientovany na interakciu**
  - motivaciou je pritomnost spolupracovnikov
  - ludia chodia do prace, pretoze to maju radi

#### skupinova sudrznost
- kvalita skupiny vytvoria jej clenovia
- tim sa od sebe navzajom uci , spoznavanie prace toho druheho
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
- mozu uviest, co by system nemal/mal robit
- popisuju funkcnost alebo systemove sluzby

- **uplne poziadavky** => popisy vsetkych pozadovanych zariadeni
- **konzistentne** => popisoch by nemali byt konfliky/rozpory

#### Domenove poziadavky
- sluzby a obmedzenia systemu z oblasti prevadzky

#### Nefunkcionalne poziadavky(NP)
- obmedzenia sluzieb alebo funkcii ponukanych systemom(casove obmedzenia, obmedzenia procesu vyvoja, ...)
- definuju vlastnosti systemyu a obmedzenia **aky by mal system byt**


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
- mal by stanovit, co by mal system robit

#### Kontrolne kontroly
- **overitelnost** => je poziadavka testovatelna?
- **zrozumitelnost** => je poziadavka spravne pochopena?
- **vysledovatelnost** => je jasne uvedeny dovod poziadavky?
- **prisposobitelnost** => da sa poziadavka zmenit?

#### Planovanie Manazmentu poziadaviek
- identifikacia poziadaviek
- proces riadenia zmien
- politiky sledovatelnosti => difinuju vztahy medzi poziadavkami
- podpora nastrojov

#### Riadenei zmeny poziadaviek
- rozhodovanie, ci by sa mala akceptovat zmena poziadaviek
- analyza problemu a specifikacia zmeny
- analyza zmien a kalkulacia
- implementaci zmeny