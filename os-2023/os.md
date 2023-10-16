## OS Poznamky

### Prednaska 1

#### Na co je dobry os?
- Aplikácie (izolácia ↔ zdieľanie)
- Služby (hw → app, spravodlivosť)
- Hardvér (výkon)
  
#### Co chcu aplikacie od os?
- Prístup k hardvéru
  - Abstrakciu hardvéru
  - Multiplexovanie hw medzi aplikáciami
- Izolovanie poškodených aplikácií od zvyšku OS
- Komunikáciu medzi sebou

#### Abstrakcia hw
- Prístup ku hw spravovaný jadrom na základe žiadosti aplikácie
- Jednotné rozhranie k rôznym zariadeniam toho istého typu (zbernica)
- Programátor ovládača sa môže sústrediť iba na jednu časť OS
- Aplikácie využívajú služby jadra cez tzv. systémové volania (syscalls)

#### Poziadavky na os
- Multiplex (Podpora viacerých vecí SÚČASNE. Zdieľanie a prerozdeľovanie zdrojov hw)
- Izolavanie (Izolácia procesov (aby nemohol jeden ničiť druhý))
- Interakcia (Komunikácia procesov je však tiež potrebná)


### kapitola 2

- Aplikacia beziacia v userspace moze vykonavat iba user mode instrukcie
- software beziaci v kernel space moze vykonavat privilegovane instrukcie

#### Kniznicny pristup
  - nevyhodou je ze ak je spustenych viac ako jedna aplikacia tak aplikacie musia byt spravne spravovane
  - aplikacia sa musi pravidelne vzdat cpu

#### monoliticke jadro
  - vsetky systemove volania bezia v privilegovanom
  - os bezi s plnymi hardverovymi  privilegiami

#### mikrokernel
  - co najmenej kodu v privilegovanom rezime
  - kod sa spustaju v user mode
  - na pouzivanie privilegovanych instrukcii sa pouzivaju servery

#### Proces
  - je jednotkou izolacie
  - Abstrakcia procesu bráni jednému procesu v zničení alebo špehovaní pamäte iného procesu, CPU, deskriptorov súborov
  - kazdy proces ma vlastne adresne miesto
  - kazdy proces ma vlastnu page table
  - kazdy proces ma vlakno (thread)
  - Dva zasobniky
    - uzivatelsky zasobnik
    - zasobnik jadra

#### Tabulka stranok(page table)
  - mapuje virtualnu adresu na fyzicku adresu

#### RISC V pamat
  - pointer 64bit
  - hardvér pri vyhľadávaní virtuálnych adries v tabuľkách stránok používa iba nízkych 39 bitov
  - xv6 používa iba 38 z týchto 39 bitov

#### Tramopoline
  - zabezpecuje prechod do jadra a spat

#### trapframe
  - sluzi na ulozenie a obnovenie stavu pouzivatelskeho procesu

#### MAXVA
  - maximalna adresa 2^28 -1 = 0x3fffffffff

#### [ECALL viz.](#ecall)

#### Entry
  - hovori kde sa zacne vykonavat programovy kod
  - nastavuje zasobnik aby sa vedel spustat c kod

#### Instrukcia mret
  - vstup do privilegovaneho rezimu
  
#### IO zariadenia
  - na 0x0 az 0x80 000 000
  
#### 0x80 000 000
  - zaciatok ram
  - zavadac sem nacita jadro

#### main
  - inicializuje niekolko zariadeni podsystemov
  - vytvori prvy proces volanim userinit

#### initcode.S
  - nacita cislo systemoveho volania exec(SYS_EXEC) do registra a7 a potom zavola ecall

#### Syscall 
  - nacita cislo systemoveho volania z registra a7 v trapframe pouziva ho na indexovanie do systemovych volani

#### argint, argaddr, argfd
  - ziskavaju n-ty argumenent systemoveho volania z ramca trap ako cele cislo, pointer alebo fd

#### copyinstr
  kopiruje max pocet bajtov do dst z virtualnej adresy srcva

### Kapitola 3

#### xv6
- umoznuje izolovat adresne priestory roznych procesov a multiplexovat ich do jedinej fyzickej pamate
- mapovanie rovnakej pamate(stranky trampoline) v niekolkych adresnych priestoroch
- strazenie zasobnikov jadra a pouzivatelov pomocou nezmapovanej stranky
- Sv39 RISC V (pouziva iba spodnych 39 bitov 64-bitovej virtualnej adresy)
- 2^27 page table entries(PTEs)
- velkost page table je 4096(2^12) bajtov

#### Tabulky stranok(page tables PT)
- mechanizmus ktorym os poskytuje kazdemu procesu vlastny sukromny adresny priestor a pamat
- urcuju co znamenaju pamatove adresy(memory adresses)
- ku ktorym castiam fyzickej pamate je mozne pristupovat
- dava os kontrolu nad prekladmi virtualnych adries na fyzicku 

#### RISC V
- instrukcie(user aj kernel) manipuluju s virtualnymi adresami
- mapuje virtualne adresy na fyzicku adresu
  

#### Page table entry (PTE)
- kazde PTE obsahuje 44-bitove cislo fyzickej stranky(PPN) a niektore priznaky
- priestor na rast 10 bitov
- kazde PTE obsahuje flag bits ktore informuju strankovaci hardver ako je povolene pouzivat virtualnu adresu
  - PTE_V indikuje ci je PTE je pritomne
  - PTE_R riadi ci mozu instrukcie citat zo stranky
  - PTE_W riadi ci mozu instrukcie zapisovat do stranky
  - PTE_X riadi ci cpu moze interpretovat obsah stranky ako instrukcie a vykonat ich
  - PTE_U riadi ci maju instrukcie(v usermode) pristup ku stranke


#### Preklad na fyzicku adresu v troch krokoch
  - Page table je ulozena vo fyzickej pamati ako troj-urovnovy strom
    - koren stromu je tabulka stranok s velkostou 4096bajtov
    - kazda stranka tabulky stranok obsahuje 512 PTE
    - PTEs obsahuju fyzicke adresy pre stranky tabulky stranko v dalsej urovni stromu
  - hardver strankovania
    - hornych 9 bitov z 27 bitov vyber PTE
    - prostrednych 9 bitov vyber PTE na stranke tabulky stranok
    - spodnych 9 bitov vyber konecneho PTE
  - ak niektory z troch PTE pozadovanych na preklad nieje pritomny strankovaci hardver vyvola page-fault exception
  - nevyhodou je ze CPU musi nacitat 3 PTE z pamate aby vykonal preklad virtualnej adresy v nacitancej/ukladanadacej instrukcii do fyzickej adresy
  - aby sa predislo nacitavaniu PTE z pamate CPU uklada PTEs do cache Translation Look-aside Buffer(TLB)

#### treba dokoncit


### Kapitola 4

#### Pasce a systemove volania(traps and syscalls)
  - tri druhy udalosti ktore sposobia ze CPU odloží bežné vykonávanie inštrukcií a vynúti prenos kontroly na špeciálny kód, ktory udalost spracuje
    - systemove volanie
    - instrukcia ktora robi nieco nepovolene(je jedno ci kernel alebo user)
    - prerusenie zariadenia(ked zariadenie signalizuje ze potrebuje pozornost)

#### Pasca (trap)
  - akýkoľvek kód spustený v čase pasce sa bude musieť neskôr obnoviť a nemal by si byť vedomý toho, že sa stalo niečo zvláštne
  - chceme aby boli transparentne(dolezite hlavne pre prerusenia zariadenia)
  - postupnost
    - pasca vynúti prenos kontroly do jadra
    - jadro ukladá registre a iný stav, aby bolo možné obnoviť vykonávanie
    - jadro vykoná príslušný kód obsluhy 
    - jadro obnoví uložený stav a vráti sa z pasce
    - povodny kod pokracuje tam kde skoncil
  - vsetky pasce sa vykonavaju v jadre
  - kod ktory spracova pascu sa nazyva handler
  - spracovanie pasce v xv6 prebieha v 4 fazach:
    -   hardvérové akcie vykonávané procesorom RISCV 
    -   niektoré pokyny na zostavenie, ktoré pripravujú cestu pre kód C jadra 
    -   funkcia C, ktorá rozhoduje,čo robiť s pascou
    -   systémové volanie alebo ovládač zariadenia

#### Strojove zariadenie pasce RISC V
  - kazdy RISC V ma sadu riadiacich registrov
    - jadro do nich zapisuje aby povedalo CPU ako zaobchadzat s pascami
    - jadro ich moze citat aby zistilo ktora pasca sa vyskytla
    - riscv.h obsahuje definicie
  - najdolezitejsie registre:
    - stvec 
      - Jadro sem zapíše adresu svojho obslužného programu pasce
      - RISC-V skoci na adresu v stvec aby handlol trap
      - musi mat platne mapovanie v user page table
    - sepc
      - Keď sa vyskytne trap, RISC-V sem uloží počítadlo programu (keďže pc sa potom prepíše hodnotou v stvec). 
      - Inštrukcia sret (návrat z pasce) skopíruje sepc do PC. 
      - Jadro môže zapisovať sepc, aby kontrolovalo, kam ide sret.
    - scause
      - RISC-V tu uvádza číslo, ktoré popisuje dôvod pasce
    - sscratch
      - Kód obsluhy pasce používa sscratch, aby zabránil prepísaniu užívateľských registrov
    - sstatus
      - Bit SIE v sstatus riadi, či sú povolené prerušenia zariadenia. 
      - Ak jadro vymaže SIE, RISC-V odloží prerušenia zariadenia, kým jadro nenastaví SIE. 
      - Bit SPP udáva, či pasca prišla z užívateľského režimu alebo z režimu supervízora, a riadi, do ktorého režimu sa vráti príkaz sret.
  - ked CPU potrebuje vynutit trap hardver urobi nasledovne pre vsetky typy pasci okrem prerusenia casovacom:
    1. Ak je pasca prerušením zariadenia a bit sstatus SIE je čistý, nevykonávaj nasledujúce.
    2. Zakážte prerušenia vymazaním bitu SIE v sstatus.
    3. Skopírujte pc(program counter) do sepc
    4. Ulož aktuálny režim (používateľ alebo supervízor) do bitu SPP v sstatus
    5. Nastav scause tak, aby odrážal príčinu pasce.
    6. Nastavte režim na privilegovany
    7. Skopírujte stvec do pc
    8. Začni vykonavat od noveho pc

#### Pasce(vynimky) v user space
  - pasca sa moze vyskytnut ak:
    -  program zavola syscall 
    -  urobi nieco ilegalne 
    -  ak ho zariadenie prerusi
  - stranka trampoline
    - namapovana v tabulke stranok kazdeho procesu na adrese **TRAMPOLINE**(vo virtualnom adresnom priestore uplne na vrchu)
    - namapovana v tabulke stranok jadra na adrese **TRAMPOLINE**
    - uservec
      - instrukcia na zaciatku (csrw) ulozi a0 do sscratch
      - ulozi vsetky uzivatelske registre(je ich 32) do **trapframe** vratane uzivatelskeho a0
      - nacita adresu **TRAPFRAME** do a0
      - kod na obsluhu pasce je v trampoline.S
      - ziska hodnoty z **trapframe** a prepne satp na PT jadra a zavola **usertrap**
  - xv6 neprepina tabulky stranok ked vynuti trap
  - high-level path trapu z userspace:
    1. uservec
    2. usertrap
    3. usertrapret
    4. userret
#### Trapframe
  - trapframe kazdeho procesu sa mapuje sa na adresu **TRAPFRAME**(tesne pod **TRAMPOLINE**)(v priestore uzivatelskych adries)
  - p->trapframe tiez ukazuje na trapframe(ale fyzicku ale moze ho vyuzit jadro cez PT jadra)
  - obsahuje:
    - adresu zasobnika jadra aktualneho procesu
    - hartid aktualneho CPU
    - adresu funkcie usertrap
    - adresu tabulky stranok jadra

#### Usertrap
  - jeho ulohou je:
    1. urcit pricinu pasce
    2. spracovat ju
    3. return

  - najprv zmeni stvec tak ze pascu v jadre bude skor riesit **kerneltrap** ako **usertrap**
  - ulozi register **sepc**
  - ak nastane:
    - trap syscallom usertrap zavola **syscall** aby to spracoval
    - device interupt tak **devintr**
    - inak je to exception a kernel killne chybny proces
  - cesta syscallu prida k PC 4(lebo by stale ukazoval na ecall a nastal by nekonecny cyklus)
  - na ceste von:
    - kontroluje ci bol proces zabity alebo mal poskytnut CPU(ak je trap timer interupt)
    1. zavola **usertrapret**


#### Usertrapret
  - nastavuje riadiace registre na pripravu na buducu trap z userspace
  - to zahrna:
    - zmenu **stvec** na **uservec** 
    - prípravu polí **trapframe**, na ktoré sa **uservec** spolieha 
    - nastavenie **sepc** na predtým uložené **user PC**
  - na konci zavola **userret** na stranke trampilny(ta je mapovana v user page aj kernel page)
    - pretoze **userret** prepne tabuky stranok
    - odovzda pointer na user page table procesu do a0
    - prepne **satp** na adresu user page table procesu
    - nacita **TRAFRAME** do a0
    - obnovi user registre pomocou a0
    - obnovi ulozeny user obsah registra a0
    - vykona **sret**

#### initcode.S
  - nacita cislo systemoveho volania exec(SYS_EXEC) do registra a7 a potom zavola ecall
  - umiestni argumenty syscallu do a0 a a1

#### Syscall 
  - nacita cislo systemoveho volania z registra a7 v trapframe pouziva ho na indexovanie do systemovych volani

#### argint, argaddr, argfd
  - ziskavaju n-ty argumenent systemoveho volania z **trapframe** ako cele cislo, pointer alebo fd

#### Exec
  - pouziva fetchstr na získanie argumentov názvu súboru reťazca z užívateľského priestoru
    - fetchstr volá copyinstr, aby vykonal ťažkú prácu 
#### copyinstr
  - kopiruje max pocet bajtov do dst z virtualnej adresy srcva

#### Walkaddr
  - kontroluje, či je virtuálna adresa zadaná používateľom súčasťou priestoru používateľských adries procesu

#### Copyout
  - kopíruje dáta z jadra na užívateľom zadanú adresu

#### ECALL
  - vykonava prechod z neprivilegovaneho modu do privilegovaneho
  - zvyšuje úroveň privilégií hardvéru a mení počítadlo programu na vstupný bod definovaný jadrom
  - systemove volanie v a7
  - argumenty v a0 a a1
  - vstupuje do jadra v jadrom specifikovanom vstupnom bode
  - navratova hodnota syscallu sa ulozi do a0
  - vyvola vynimku(tym sposobi spustenie uservec, usertrap a potom syscall)

#### Pasce z kernel space
  - xv6 konfiguruje trap registre inak v zavislosti ci je spusteny user alebo kernel kod
  - ked je jadro spustene na CPU tak kernel ukazuje na stvec na assembler kod v kernelvec

#### kernelvec
  - moze sa spoliehat na to ze **satp** ukazuje kernel page table pretoze xv6 je v jadre
  - vlozi vsetkych 32 registrov do stacku, odkial ich neskor obnovi aby sa mohol kod jadra neskor obnovit
  - ukladá registre do zásobníka prerušeného vlákna jadra(toto je dolezite ak trap vyvola prepnutie do ineho vlakna)
  - preskoci na **kerneltrap** po ulozeni registrov

#### Kerneltrap
  - pripraveny na dva typy preruseni:
    - device interupt
    - exception
  - pri spusteni ulozi sepc a predchadzjuci rezim v sstatus
  - vola **devintr** aby checkol a popripade hadlol device iterupt
  - ak trap nieje device interupt ide o **excetion a to je vzdy fatal error**
    - kernel **vyvola paniku a prestane vykonavat**

  - ak bol zavolany kvoli timer interuptu tak poskytne ostatnym vlaknam sancu bezat
    - v urcitom case sa niektore vlakno podvoli a nechaju nase vlakno a jeho kerneltrap sa obnovit
  - ked sa vykonavanie kerneltrap dokonci vrati sa ku kodu ktory trap prerusila
    - obnovi riadiace registre a vrati sa do kernelvec
    - potom kernelvec vyberie ulozene registre zo stacku a zavola sret ten skopiruje sepc do PC a obnovi preruseny kod

#### navrat z trapu ak bol vyvolany timer interuptom
- xv6 nastavi stvec na kernelvec
- Existuje časové okno, keď sa jadro začalo vykonávať, ale stvec je stále nastavený na uservec a je dôležité, aby počas tohto okna nenastalo žiadne prerušenie zariadenia. Našťastie RISC-V vždy deaktivuje prerušenia, keď začne zachytávať pascu, a xv6 ich znova povolí, kým nenastaví stvec.

### Kapitola 4.6

#### Reakcia xv6 na vynimky
  - v user mode
    - jadro zabije proces kde chyba nastala
  - v kernel mode
    - jadro panikari

#### Copy-on-write (COW) fork
  - rodic aj dieta mozu zdielat fyzicku pamat vhodnym pozivanim vhodnych povoleni tabulky stranok a chyb stranok
  - COW je transparentna (niesu treba zmeny v aplikacii aby z COW benefitovali)
  -  CPU vyvolá výnimku chyby stránky, keď sa pouzije virtualna adresa ktora:
     - nema ziadne mapovanie v PT
     - ma mapovanie ktoreho priznak **PTE_V** je prazdny
     - mapovanie ktoreho bity **(PTE_R, PTE_W, PTE_X, PTE_U)** zakazuju danu operaciu
  - rodič a dieťa budú spočiatku zdieľať všetky fyzické stránky, ale pre každého ich bude mapovať iba na čítanie (s jasným príznakom **PTE_W**)
  - ak niektory z nich zapise danu stranku risc v vyvola vynimku page fault
    - Obsluha pasce jadra odpovedá pridelením novej stránky fyzickej pamäte a skopírovaním fyzickej stránky, na ktorú sa mapuje chybná adresa
    - Jadro zmení príslušné PTE v tabuľke stránok chybujúceho procesu tak, aby ukazovalo na kópiu a umožňovalo zapisovanie aj čítanie
    - potom obnoví chybný proces podľa inštrukcie, ktorá spôsobila chybu
    - Pretože PTE umožňuje zápis, opätovne vykonaná inštrukcia sa teraz vykoná bez chyby.
  - vyžaduje vedenie zanamov, ktoré pomôžu rozhodnúť, kedy môžu byť fyzické stránky uvoľnené, pretože na každú stránku môže odkazovať rôzny počet tabuliek stránok v závislosti od histórie rozvetvení, chýb stránok, execov a výstupov
  - vedenie zaznamov umoznuje optimalizaciu : ak sa v procese vyskytne **store page fault** a na fyzickú stránku sa odkazuje iba z tabuľky stránok tohto procesu, nie je potrebná žiadna kópia.
  - COW urychluje fork pretoze nemusi kopirovat pamat(iba ked sa nieco zapise musi kopirovat)
#### Chyby stranok
  - **load page fault**(keď inštrukcia načítania nemôže preložiť svoju virtuálnu adresu)
  - **store page fault**(keď inštrukcia uloženia nemôže preložiť svoju virtuálnu adresu)
  - **instruction page fault**(keď adresa v počítadlo programu neprekladá)

  - register **scause** oznacuje typ chyby a **sval** obsahuje adresu ktoru nebolo mozne prelozit

#### Lazy alocation
  -  jadro nemusí robiť vôbec žiadnu prácu pre stránky, ktoré aplikácia nikdy nepoužíva
  -  ak aplikácia požaduje veľké rozšírenie adresného priestoru, tak sbrk je bez lazy alocation drahý
  -  umoznuje rozlozit naklady v case
  -  spôsobuje ďalšiu réžiu chýb stránok, ktoré zahŕňajú prechod medzi kernelom a userom
     -  **OS** môže znížiť tieto náklady pridelením dávky po sebe nasledujúcich stránok na chybu stránky namiesto jednej stránky a špecializáciou vstupného/výstupného kódu jadra pre takéto chyby stránky 

#### Demand paging
  - Na zlepšenie času odozvy moderné jadro vytvára tabuľku stránok pre priestor užívateľských adries, ale označuje PTE pre stránky ako neplatné. 
  - Pri chybe stránky jadro načíta obsah stránky z disku a namapuje ho do priestoru adries používateľa. 
  - Podobne ako COW fork a lenivá alokácia môže jadro implementovať túto funkciu transparentne do aplikácií.

#### Paging to disk
  - ak prokgram ktory bezi potrebuj viac pamate ako ma pocitac RAM
  - Myšlienkou je uložiť iba zlomok používateľských stránok do pamäte RAM a zvyšok uložiť na disk v **paging area**
  - kernel oznaci priznaky PTE ktore oznacuju pamat ulozenu v **paging** area ako neplatnu
  - ak sa aplikacia pokusi pouzit stranku ktora bola **paged out** na disk tak sa vyvola **page fault** a stranka musi byt **paged in** do RAM
    - kernel trap handler alokuje fyzicku stranku pamate RAM
    - nacita stranku z disku do RAM
    - upravi prislusny PTE aby ukazoval na RAM
  - ak je treba nacitat stranku z disku do RAM ale nieje dostatok RAM:
    - jadro musi najprv uvolnit fyzicku pamat bud pomocou **paging out** alebo ju evictuje do **paging area** na disku a oznaci PTE ukazujuce na tuto pamat za neplatne
    - eviction je drahe takze strankovanie usiluje o to aby ho bolo treba robit co najmanej
  - dobra refernecna lokalita:
    - ak aplikácie používajú iba podmnožinu svojich pamäťových stránok a spojenie podmnožín sa zmestí do pamäte RAM
  - pocitace casto operuju s malou alebo ziadnou volnou fyzickou pamatou(bez ohladu na to kolko je hardverovej RAM)

- Lazy alocation a paging to disk je velmi vyhodne ak je nedostatok volnej pamate

####  automatically extending stacks and memory-mapped files
  - dalsia funkcionalita ktora kombinuje vynimky strankovania a chyby stranok