# OS Poznamky

### Obsah
 - [Kapitola 1](os.md#prednaska-1)
 - [Kapitola 2](os.md#kapitola-2)
   - [Kniznicny pristup](os.md#kniznicny-pristup)
   - [Monoliticke jadro](os.md#monoliticke-jadro)
   - [Mikrokernel](os.md#mikrokernel)
   - [Proces](os.md#proces)
 - [Kapitola 3](os.md#kapitola-3)
   - [Tabulky stranok](os.md#tabulky-stranokpage-tables-pt)
   - [Page table entry (PTE)](os.md#page-table-entry-pte)
   - [Preklad na fyzicku adresu](os.md#preklad-na-fyzicku-adresu-v-troch-krokoch)
 - [Kapitola 4](os.md#kapitola-4)
   - [Pasca](os.md#pasca-trap)
     - [V user space](os.md#pascevynimky-v-user-space)
     - [V kernel space](os.md#pasce-z-kernel-space)
   - [Trapframe](os.md#trapframe-1)
   - [Usertrap](os.md#usertrap)
   - [Usertrapret](os.md#usertrapret)
   - [Ecall](os.md#ecall)
   - [Kernelvec](os.md#kernelvec)
   - [Kerneltrap](os.md#kerneltrap)
 - [Kapitola 4.6](os.md#kapitola-46)
   - [COW fork](os.md#copy-on-write-cow-fork)
   - [Chyby stranok](os.md#chyby-stranok)
   - [Leniva alokacia](os.md#lazy-alocation)
 - [Kapitola 7](os.md#kapitola-7)
   - [Scheduler](os.md#schedulerplanovac)
   - [Swtch](os.md#swtch)
   - [Sched](os.md#sched)
   - [mycpu() a myproc()](os.md#mycpu-myproc)
   - [Sleep a wakeup](os.md#sleep-and-wakeup-1)
   - [Wait, exit, kill](os.md#kod-wait-exit-kill)
   - [p->lock](os.md#p-lock)

### Prednaska 1   ===============================

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



### kapitola 2   ===============================

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



### Kapitola 3   ===============================

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



### Kapitola 4   ===============================

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
  - na konci zavola **userret** na stranke trampoliny(ta je mapovana v user page aj kernel page)
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
  - ak trap nieje device interupt ide o **exception a to je vzdy fatal error**
    - kernel **vyvola paniku a prestane vykonavat**

  - ak bol zavolany kvoli timer interuptu tak poskytne ostatnym vlaknam sancu bezat
    - v urcitom case sa niektore vlakno podvoli a nechaju nase vlakno a jeho kerneltrap sa obnovit
  - ked sa vykonavanie kerneltrap dokonci vrati sa ku kodu ktory trap prerusila
    - obnovi riadiace registre a vrati sa do kernelvec
    - potom kernelvec vyberie ulozene registre zo stacku a zavola sret ten skopiruje sepc do PC a obnovi preruseny kod

#### navrat z trapu ak bol vyvolany timer interuptom
- xv6 nastavi stvec na kernelvec
- Existuje časové okno, keď sa jadro začalo vykonávať, ale stvec je stále nastavený na uservec a je dôležité, aby počas tohto okna nenastalo žiadne prerušenie zariadenia. Našťastie RISC-V vždy deaktivuje prerušenia, keď začne zachytávať pascu, a xv6 ich znova povolí, kým nenastaví stvec.



### Kapitola 4.6   ===============================

#### Reakcia xv6 na vynimky
  - v user mode
    - jadro zabije proces kde chyba nastala
  - v kernel mode
    - jadro panikari

#### Copy-on-write (COW) fork
  - rodic aj dieta mozu zdielat fyzicku pamat vhodnym pouzivanim vhodnych povoleni tabulky stranok a chyb stranok
  - COW je transparentna (niesu treba zmeny v aplikacii aby z COW benefitovali)
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
  - **instruction page fault**(keď sa adresa v počítadle programu nemoze prelozit)

  - register **scause** oznacuje typ chyby a **sval** obsahuje adresu ktoru nebolo mozne prelozit
  - CPU vyvolá výnimku chyby stránky, keď sa pouzije virtualna adresa ktora:
    - nema ziadne mapovanie v PT
    - ma mapovanie ktoreho priznak **PTE_V** je prazdny
    - mapovanie ktoreho bity priznakov **(PTE_R, PTE_W, PTE_X, PTE_U)** zakazuju danu operaciu

#### Lazy alocation
  -  alokuje pamat az ked ju aplkacia potrebuje
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
  - ak program ktory bezi potrebuje viac pamate ako ma pocitac RAM
  - Myšlienkou je uložiť iba zlomok používateľských stránok do pamäte RAM a zvyšok uložiť na disk v **paging area**
  - kernel oznaci priznaky PTE ktore oznacuju pamat ulozenu v **paging** area ako neplatnu
  - ak sa aplikacia pokusi pouzit stranku ktora bola **paged out** na disk tak sa vyvola **page fault** a stranka musi byt **paged in** do RAM
    - kernel trap handler alokuje fyzicku stranku pamate RAM
    - nacita stranku z disku do RAM
    - upravi prislusny PTE aby ukazoval na RAM
  - ak je treba nacitat stranku z disku do RAM ale nieje dostatok RAM:
    - jadro musi najprv uvolnit fyzicku pamat bud pomocou **paging out** alebo ju odlozi do **paging area** na disku a oznaci PTE ukazujuce na tuto pamat za neplatne
    - odlozenie je drahe takze strankovanie usiluje o to aby ho bolo treba robit co najmanej
  - dobra refernecna lokalita:
    - ak aplikácie používajú iba podmnožinu svojich pamäťových stránok a spojenie podmnožín sa zmestí do pamäte RAM
  - pocitace casto operuju s malou alebo ziadnou volnou fyzickou pamatou(bez ohladu na to kolko je hardverovej RAM)

- Lazy alocation a paging to disk je velmi vyhodne ak je nedostatok volnej pamate

####  automatically extending stacks and memory-mapped files
  - dalsia funkcionalita ktora kombinuje vynimky strankovania a chyby stranok

#### Terminologia z prednasky:
  - Exception (výnimka)
    - výnimočný stav vyvolaný inštrukciou vykonávaného programu
  - Trap (presun riadenia)
    -  synchrónny prenos riadenia do kódu obsluhy spôsobený výnimočným stavom, ktorý zapríčinil vykonávaný program
  - Interrupt (prerušenie)
    - externá udalosť, ktorá sa vyskytne asynchrónne voči vykonávanému kódu



### Kapitola 7   ===============================

#### Multiplexing
  - Vytvara iluziu ze kazdy proces ma svoje CPU

#### Sleep and wakeup
  - umoznuje procesu vzdat sa CPU a cakat kym ho zobudi iny proces alebo prerusenie

#### Context switching
  - Prepnutie z jedného vlákna do druhého zahŕňa uloženie registrov CPU starého vlákna a obnovenie predtým uložených registrov nového vlákna(CPU prepne zásobníky a prepne, aký kód vykonáva)


#### Scheduler(Planovac)
  - ma vyhradene vlakno(ulozene registre a stack) pretoze nieje bezpecne aby sa scheduler vykonal na stacku stareho procesu
  - existuje vo forme specialneho jadra(na ktorom bezi scheduler) pre kazde CPU
  - existuje jeden pripad ked volanie swtch z schedulera nekonci v sched:
    - **allocproc** nastavi contextovy register **ra** na adresu **forkret** takze prvy swtch sa vrati na adresu tej funkcie
    - **forkret** existuje aby uvolnil **p->lock**
    - novy proces sa musi vratit do user space ako keby sa vracal z forku a namiesto toho moze zacat usertrapret
  - bezi loop, najde proces ktory moze spustit, spusti proces dokial sa nevykona, opakuje
  - loop prechadza procesy v tabulke procesov a hlada proces ktoreho **p->state == RUNNABLE**
  - ked takyto proces najde nastavi pre kazde CPU aktualnu premenu procesu p->proc, oznaci proces za **RUNNING** potom zavola swtch aby ho spustil
  - struktura sheduling kodu, set invariantov o kazdom procese a drzi **p->lock** vzdy ked invarianty niesu true
  - lock musi byt drzany pokial invarianty niesu obnovene(spravny release point je ked scheduler vymaze c->proc)

#### swtch
  - vykonáva uloženie a obnovenie pre prepínač vlákien jadra
  - ked sa proces vzdava CPU zavola swtch aby ulozil svoj context(struct context) a vratil sa ku contextu scheduleru
  - dva argumenty
    - struct context *old
    - struct context *new
  - ulozi aktualne registre v old a nacita nove z new
  - uklada iba callee-saved registre
  - pozna offset kazdeho registra zo struct context
  - neuklada program counter namiesto neho uklada **ra** register ktory obsahuje navratovu adresu z ktorej bol **swtch** volany

#### Code: Scheduling
  - proces ktory sa chce vzdat CPU musi: 
    - ziskat vlastny process lock **p->lock**(kontrola locku sa prenasa na switchnuty proces)
    - uvolnit vsetky ostatne zamky
    - updatnut svoj state (p->state)
    - a potom zavolat **sched**
  - kedze je proces zamknuty(p->lock) prerusenia by mali byt vypnute
  - ak by **p->lock** nebol podrzany pocas swtch ine CPU by sa mohlo rozhodnut spustit proces potom ako yield nastavil svoj state na **RUNNABLE** ale predtym ako by swtch zapricinil prestatie pouzivania vlastneho kernel stacku => 2CPU beziace na rovnakom kernel stacku

#### sched
  - vola swtch aby ulozil aktualny context do **p->context** a switch to scheduler context do **cpu->context**
  - **swtch** vracia na stack scheduleru ako keby switch planovaca vracal(celkom confusing)
  - scheduler pokracuje svoj for loop, najde proces, prepne na neho a cyklus sa opakuje
  - jedine miesto kde sa kernel thread vzdava CPU(a vzdy prepina na rovnake miesto v scheduleri ktore skoro vzdy prepina na kernel thread ktory volal **sched**)

#### mycpu() myproc()
  - xv6 uchovava **struct cpu** pre kazde CPU, ta zaznamenava:
    -  aktualne beziaci proces na danom CPU, 
    -  ulozene registre scheduler threadu
    -  pocet nested spinlockov potrebnych na riadenie deaktivacie interuptov
  - xv6 prideluje kazdemu cpu **hartid**(kazdy hartid je ulozeny v **tp** registry daneho CPU pokial je v jadre)
  - **start** nastavuje **tp** v zaciatkoch bootovania(v machine mode)
  - **usertrapret** uklada **tp** v PT trampoliny
  - **uservec** obnovuje **tp** ked vstupuje do kernelu z user space
  - kompilator garantuje ze nikdy nepouzije **tp**

#### mycpu()
  - vracia pointer na **struct cpu** daneho CPU
  - pouziva **tp** aby indexovalo **array of struct cpu**
  - xv6 vyzaduje aby volajuci vypol interupty a zapol ich az po dokonceni prace so **struct cpu**

#### myproc()
  - vracia pointer na **struct proc** aktualneho procesu ktory bezi na danom CPU
  - vypne prerusenia vyvola **mycpu()**, spracuje aktualny process pointer(c->proc) zo **struct cpu**, zapne prerusenia
  - vracana hodnota je bezpecne pouzitelna aj ked su interupts povolene

#### Sleep and wakeup
  - casto nazyvane **sequence coordination** alebo **conditional synchronization mechanisms**
  - scheduling a locks pomahaju skryt akcie jedneho vlakna pred druhym
  - spanok umožňuje vláknu jadra čakať na konkrétnu udalosť
  - iné vlákno môže zavolať prebudenie, aby naznačilo, že vlákna čakajúce na udalosť by sa mali obnoviť
  - mozu ako wait channel pouzit akekolvek im vyhovujuce cislo(Xv6 často používa adresu dátovej štruktúry jadra zapojenej do čakania)
  - spiaci proces drzi bud condition lock alebo p->lock alebo obidva(predtym ako je vyhodnotena podmienka a je oznaceny za **SLEEPING**)

#### sleep
  - sleep oznaci aktualny proces ako **SLEEPING** a potom zavola **shed** aby sa vzdal CPU
  - drzi **p->lock** a pusti ho az ked je proces **SLEEPING**

#### wakeup
  - hlada spiaci proces na danom wait channely a oznaci ho za **RUNNABLE**
  - V určitom bode proces získa stavový zámok, nastaví stav, na ktorý spánok čaká, a zavolá wakeup(chan).
  - je dolezite aby sa **wakeup volal ked je condition lock v drzani**
  - prechadza cez procesy v tabulke procesov ziskava p->lock daneho procesu(aby sa so sleepom neminuli a preto ze bude menit state procesu), ak najde **SLEEPING** process so zhodnym channelom tak ho oznaci za **RUNNABLE**
  - v loope drzi conditional lock aj p->lock

#### kod: pipe
  - **pipewrite**
    - zacina ziskanim pipe locku **pi->lock**(ktory chrani pocty, data a ich asociovane invarianty), potom cykluje cez zapisovane bajty(medzitym **piperead caka spinovanim v acquire**) pricom kazdy pridava do pipe, pocas cyklu sa moze buffer naplnit v tomto pripade pipewrite zavola **wakeup** aby upozornil spiacich readerov na to ze data cakaju v bufferi
    - potom sleep uspi pipewrite na **&pi->nwrite** aby cakal kym reader zoberie nejake bajty z buffera
    - sleep vypusti **pi->lock** pocas uspavania pipewritu
  - **piperead**
    - ked ma **pi->lock** k dispozicii tak ho acquirne a vstupi do **kritickej sekcie**
    - for loop, kopiruje data z pipe a incrementuje nread o pocet precitanych bajtov, tolko bajtov je teraz dostupnych na zapis, piperead zavola **wakeup** aby zobudil ktorehokolvek spiaceho writera, predtym ako vracia
    - wakeup najde spiaceho writera na adrese **&pi->nwrite** a oznaci ho za **RUNNABLE**
  - kazda pipe je reprezentovana **struct pipe** ta obsahuje lock a data buffer
  - pouziva oddelene sleep channely pre **read a write(pi->nread a pi->nwrite)**

#### kod: wait, exit, kill
  - exit prevedie volajuceho do **ZOMBIE** state dokial si to rodicov wait nevsimne a zmeni state childu na **UNUSED** skopiruje childov exit status a vrati rodicovi pid childu
  
#### exit
  - zaznamenava exit status, 
  - uvolni niektore zdroje, 
  - vola **reparent** aby dalo child **init procesu**, 
  - zobudi rodica ak je vo wait, nastavi status volajuceho na **ZOMBIE**
  - trvalo odvzdava CPU
  - drzi  **wait_lock**(pretoze jeho podmienka pre wakeup) aj **p->lock**(aby zabranil waitu vydiet **ZOMBIE** state predtym ako child zavola swtch) pocas tejto sekvencie
  - ak rodic exitne skorek ako child tak da child **init procesu** (aby mal kazdy child nad sebou proces ktory ponom 'poupratuje')

#### wait
  - zacina ziskanim **wait_lock**
    - **wait_lock** funguje ako stavovy zamok, pomáha zabezpečiť, aby rodič nezmeškal prebudenie od odchádzajúceho potomka
  - potom wait skenuje tabulku procesov a ak najde child so statom **ZOMBIE** tak uvolni prostriedky childu a jeho proc struct
  - a skopiruje childov exit status na adresu dodanu do wait a vrati childov proces id
  - ak nenajde ziadne ukoncene childy tak zavola sleep a caka az niektory exitne a potom scanuje znova
  - casto drzi dva zamky **wait_lock** a nejakeho procesu **pp->lock**
  - vyhýbanie sa **deadlocku je najprv wait_lock a potom pp->lock**

#### kill
  - nastavi **p->killed** obete a ak spi tak ju zobudi, nakoniec obet opusti kernel a v tom momente kod v **usertrap** zavola exit ak je **p->killed** nastaveny
  - Ak obeť beží v používateľskom priestore, čoskoro vstúpi do jadra vykonaním systémového volania alebo preto, že nastane timer(alebo nejaké iné zariadenie) interupt
  - Niektoré cally sleepu tiež testujú p->killed v slučke a opustia aktuálnu aktivitu, ak je nastavená

#### process locking
  - **p->parent** je chraneny globalnym **wait_lockom** (iba rodic procesu ho modifikuje)
  - ucelom **wait_locku** je spravat sa ako conditional lock pocas toho ako **wait** spi a caka na to az sa ukonci nejaky child
  - wait_lock je globalny zamok pretoze kým ho proces nezíska, nemôže vedieť, kto je jeho rodič.

#### p->lock
  - najkomplexnejsi zamok v xv6
  - musí byt drzany čítaní alebo zapisovani ktoréhokoľvek z nasledujúcich polí struct proc : **p->state, p->chan, p->killed, p->xstate a p ->pid**
  - Väčšina použití **p->lock** však chráni aspekty vyššej úrovne štruktúry procesných údajov xv6 a algoritmy
  - veci ktore robi **p->lock**
    - s p->state zabranuje pretekom pri prideľovaní proc[] slotov pre nové procesy
    - Ukrýva proces pred zrakom, kým sa vytvára alebo ničí
    - zabranuje rodicovskemu wait aby collectol proces ktory je v state **ZOMBIE** predtym ako sa vdal CPU
    - zabranuje scheduleru ineho jadra spustit yielding proces potom co bol proces oznaceny za **RUNNABLE** ale predtym ako bol volany **swtch** 
    - Zabezpečuje, že iba jeden plánovač jadra sa rozhodne spustiť RUNNABLE procesy
    -  Zabraňuje tomu, aby prerušenie časovača spôsobilo uvoľnenie procesu, kým je vo **swtch**
    -  Spolu s condition lockom pomáha zabrániť wakeupu od prehliadnutia procesu, ktorý vola sleep ale nedokoncil vzdavanie sa CPU
    -  Zabraňuje tomu, aby obet killu skoncila a bola realocovana medzitym ako kill overuje **p->pid** a nastavenim **p->killed**
    -  donuti kill overit a zapisat **p->state** atomicky

#### round robin
  - jednoduchá politika plánovania, ktorá postupne spúšťa každý proces