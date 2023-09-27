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
  - 

#### MAXVA
  - maximalna adresa 2^28 -1 = 0x3fffffffff
  
#### ECALL
  - vykonava prechod z neprivilegovaneho modu do privilegovaneho
  - systemove volanie v a7
  - argumenty v 

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