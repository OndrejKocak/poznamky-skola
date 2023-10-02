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
  
#### ECALL
  - vykonava prechod z neprivilegovaneho modu do privilegovaneho
  - zvyšuje úroveň privilégií hardvéru a mení počítadlo programu na vstupný bod definovaný jadrom
  - systemove volanie v a7
  - argumenty v a0 a a1
  - vstupuje do jadra v jadrom specifikovanom vstupnom bode
  - navratova hodnota syscallu sa ulozi do a0

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

#### Tabulky stranok(page tables)
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
  - PTE_X 


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