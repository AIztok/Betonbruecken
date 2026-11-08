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
    line sln 1005       type py p1 -1.5*1.1 x1 sx 0 p2 -1.5*1.5 x2 sx 1
    line sln 1005       type mx p1 1.5*1.1^2/2 x1 s 0[-] p2 1.5*1.5^2/2 x2 s 1[-]
    line sln 1009,1013  type py p1 -1.5*1.1 x1 sx 0 p2 -1.5*1.5 x2 sx 1
    line sln 1009,1013  type mx p1 1.5*1.1^2/2 x1 s 0[-] p2 1.5*1.5^2/2 x2 s 1[-]

end      
```

