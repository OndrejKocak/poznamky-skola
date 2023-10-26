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