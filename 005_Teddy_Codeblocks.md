Im folgenden werden Blöcke von Cadinp (Programmiersprache SOFiSTiK) die bei der Übung verwendet werden, angeführt.

# Einwirkungen und Lasten



## Ständige Lasten


```
+prog sofiload
head

    LC NO 1 FACD 1.0 TYPE G_1 TITL 'Eigengewicht'

!*!Label Ausbalasten
    $ Fahrbahn definiert
    $                ----->y
    $ x_____                   _______x
    $ YLA  x_________x_________x      YRA
    $      WL        AX        WR
    $
    LANE NO 'AX' type EC YLA -5.2 WL -4.23 WR 4.23 YRA 5.2
    $ Lasten als Lastenzüge definiert
    LC NO 950 TYPE none TITL 'RBK'
            TRAI USER P5 25*0.3 WIDT 1.0 $ Wichte * Dicke / WIDT = breite
    LC NO 951 TYPE none TITL 'Belag'
            TRAI USER P5 24*0.13 $ wicht * Dicke
    LC NO 952 TYPE none TITL 'Linienlast 1kN/m Z'
            TRAI USER
            trbl 1  $ linienlast 1 kN/m

    LC NO 3 TYPE G_2 TITL 'Randbalken'
          $ am überbau
          COPY NO 950 FACT  1.0  TYPE ALL REF 'AX.0' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0
          $ am überbau - schurze(überstand) rechts
          COPY NO 952 FACT  0.55*0.35  TYPE ALL REF 'AX.0' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0 DY +5.2
          $ am überbau - schurze links
          COPY NO 952 FACT  0.55*0.35  TYPE ALL REF 'AX.0' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0 DY -5.2

    LC NO 4 TYPE G_2 TITL 'Geländer'
          $ rechts
          COPY NO 952 FACT  1  TYPE ALL REF 'AX.0' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0 DY 5.2-0.2
          $ links
          COPY NO 952 FACT  1  TYPE ALL REF 'AX.0' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0 DY -(5.2-0.2)

    LC NO 5 TYPE G_2 TITL 'Asfalt Belag'
          COPY NO 951 FACT  1.0  TYPE ALL REF 'AX.10' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0
          COPY NO 951 FACT  1.0  TYPE ALL REF 'AX.11' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0
          COPY NO 951 FACT  1.0  TYPE ALL REF 'AX.12' DX 0 FROM (0 3 1) TO - INC 0 WIDE 0

end      
```


## Veränderliche Lasten

### Setzungen
```
+PROG SOFILOAD
HEAD Setzungen

!*! Setzungen
LC  201  TITL 'Setzungen Achse WL1' TYPE F
node  no 1 TYPE WZZ  P1 (0.5*10)[mm]

LC  202  TITL 'Setzungen Achse ST2' TYPE F
node  no 3 TYPE WZZ  P1 (0.5*10)[mm]

LC  203  TITL 'Setzungen Achse WL3' TYPE F
node  no 2 TYPE WZZ  P1 (0.5*10)[mm]

end   
```

### Temperatur
```
+PROG SOFILOAD urs:29.1
HEAD Temperaturbedingte Belastung


!*!Label Temperatur Überbau Kombinationen
    let#DeltaTnCon -28
    let#DeltaTnExp 20
    let#DeltaTmHeat 10
    let#DeltaTmCool -5

    let#OmegaN 0.35
    let#OmegaM 0.75

    LC NO 241 TYPE T TITL 'Temp. TnExp'     $ orig titl: 'Temp. TnExp'
           BEAM grp 101,102,201,202  TYPE DT   PA    20

    LC NO 242 TYPE T TITL 'Temp. TnCon'     $ orig titl: 'Temp. TnCon'
           BEAM grp 101,102,201,202  TYPE DT   PA    -28

    LC NO 243 TYPE T TITL 'Temp. DeltaTmHeat'     $ orig titl: 'Temp. DeltaTmHeat'
           BEAM grp 101,102,201,202  TYPE DTZ   PA  10

    LC NO 244 TYPE T TITL 'Temp. DeltaTmCool'     $ orig titl: 'Temp. DeltaTmCool'
           BEAM grp 101,102,201,202  TYPE DTZ   PA  -5

    LC NO 251 TYPE T TITL 'T Sommer wn*TNexp+dT_heat'     $ orig titl: 'T Sommer wn*TNexp+dT_heat'
        COPY 241 FACT #OmegaN
        COPY 243

    LC NO 252 TYPE T TITL 'T Sommer wn*TNexp+dT_cool'     $ orig titl: 'T Sommer wn*TNexp+dT_cool'
        COPY 241 FACT #OmegaN
        COPY 244

    LC NO 253 TYPE T TITL 'T Winter wn*TNcon+dT_heat'     $ orig titl: 'T Winter wn*TNcon+dT_heat'
        COPY 242 FACT #OmegaN
        COPY 243

    LC NO 254 TYPE T TITL 'T Winter wn*TNcon+dT_cool'     $ orig titl: 'T Winter wn*TNcon+dT_cool'
        COPY 242 FACT #OmegaN
        COPY 244

    LC NO 255 TYPE T TITL 'T Sommer TNexp+wm*dT_heat'     $ orig titl: 'T Sommer TNexp+wm*dT_heat'
        COPY 241
        COPY 243 FACT #OmegaM

    LC NO 256 TYPE T TITL 'T Sommer TNexp+wm*dT_cool'     $ orig titl: 'T Sommer TNexp+wm*dT_cool'
        COPY 241
        COPY 244 FACT #OmegaM

    LC NO 257 TYPE T TITL 'T Winter TNcon+wm*dT_heat'     $ orig titl: 'T Winter TNcon+wm*dT_heat'
        COPY 242
        COPY 243 FACT #OmegaM

    LC NO 258 TYPE T TITL 'T Winter TNcon+wm*dT_cool'     $ orig titl: 'T Winter TNcon+wm*dT_cool'
        COPY 242
        COPY 244 FACT #OmegaM

end  
```

### Wind

```
+prog sofiload urs:29.5
head Wind

LC NO 221 TYPE W TITL 'Wind +Y'
    line sln 1005  type py p1 -1.5*1.1      x1 s 0[-] p2 -1.5*1.5       x2 s 1[-]
    line sln 1005  type mx p1  1.5*1.1^2/2  x1 s 0[-] p2  1.5*1.5^2/2   x2 s 1[-]
    line sln 1009  type py p1 -1.5*1.5      x1 s 0[-] p2 -1.5*1.4       x2 s 1[-]
    line sln 1009  type mx p1  1.5*1.5^2/2  x1 s 0[-] p2  1.5*1.4^2/2   x2 s 1[-]
    line sln 1013  type py p1 -1.5*1.4      x1 s 0[-] p2 -1.5*1.1       x2 s 1[-]
    line sln 1013  type mx p1  1.5*1.4^2/2  x1 s 0[-] p2  1.5*1.1^2/2   x2 s 1[-]
     

end      
```

# Berechnung

## Berechnung Einzellastfälle
```
+prog ase
head Berechnung

#define Calc_param
syst prob line
grp -
grp2 401,402 quea 0.001 qemx 0.001
#enddef

#include Calc_param

lc all

end
```

## Berechnung Straßenverkehrslasten mittels ELLA GUI
Task `Verkehrslasten` einfügen.

## Berechnung Eisenbahnlasten mittels ELLA Teddy Eingabe
Lastenzüge für Eisenbahnlasten können auch mittels ELLA berechnet werden, jedoch nur über die textuelle Eingabe.

**Wird noch ergänzt**

# Bauphasen

```

+prog csm urs:37.1
head Bauphasen

CTRL CANT 2 ! neue segmente tangential zu vorherigen ergänzen

ctrl beam aqb ! kriechen und schwinden über AQB Modul berechnet
ctrl crep rcre ! kriechwert für jede Bauphase neu berechnet
ctrl relz auto ! relaxation von Spannstahl berücksichtigt
ctrl emod auto ! E-Modulus ist Zeitabhängig

ctrl ase text 'ctrl warn 43'
ctrl ase2 text 'GRP2 401,402 quea 0.001 qemx 0.001'

CS 10   TYPE G_1    TITL 'Unterbau'
CS 15   TYPE C_1    TITL 'K+S 28d' T 28 RH 80 ncre 3

CS 19   TYPE B      TITL 'Überbau BA1 aktiviert'
CS 20   TYPE G_1    TITL 'Überbau BA1 Gewicht'

CS 21   TYPE P      TITL 'Vorspannung BA1'
CS 25   TYPE C_1    TITL 'K+S 21d' T 21 RH 80 ncre 2

CS 29   TYPE B      TITL 'Überbau BA2 aktiviert'
CS 30   TYPE G_1    TITL 'Überbau BA2 Gewicht'

CS 31   TYPE P      TITL 'Vorspannung BA2'
CS 35   TYPE C_1    TITL 'K+S 21d' T 21 RH 80 ncre 2

CS 100  TYPE G_2  TITL 'G_2 Aufbau'
CS 105  TYPE C_1  TITL 'K+S bis Verkehrsöffnung'  T 100               RH 80 ncre 3
CS 115  TYPE C_2  TITL 'K+S bis unendlich'        T 365*70-21-21-100  RH 80 ncre 5

GRP    NO                 ICS1  ICSD    ATIL   QEMX   QUEA    T0  TITL PHIF=0.00
       -                  10    10      -     -      -       28  'Fundierung'
       1                  10    10      -     -      -       28  'WL01'
       2                  10    10      -     -      -       28  'ST02'
       3                  10    10      -     -      -       28  'WL03'
       19                 19    20      -     -      -       28  'WL01 Lager'
       21,22              10    10      -     -      -       28  'ST02'
       30,31              10    10      -     -      -       28  'WL03'
       39                 29    30      -     -      -       28  'WL03 Lager'
       101,201,401        19    20      -     -      -       14  'Überbau BA1'
       102,202,402        29    30      -     -      -       14  'Überbau BA2'

LC    NO     TYPE  ICS1 ATIL
       3     G_2    100   -      $ Randbalken
       4     G_2    100   -      $ Geländer
       5     G_2    100   -      $ Asphalt


CAMB CS 119

end

$ Dann die datei $(NAME)_csm.dat laufen lassen
$ Start file $(NAME)_csm.dat
$ Wird hier automatisch über +apply erledigt
+apply $(NAME)_csm.dat   

```

# Einwirkungskombinationen
Einfügen Task `CSM Bridge Design - Überlagerungen`

# Bemessung
Einfügen Task `CSM Bridge Design - Stäbe`
Einfügen Task `CSM Bridge Design - Flächenelemente`

