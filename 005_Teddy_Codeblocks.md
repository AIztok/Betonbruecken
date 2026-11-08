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
