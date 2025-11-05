
# Querschnitt

Polylinie für den Querschnitt

```
_PL
0,0
2700,0
2700,300
1900,300
600,450
600,1100
-600,1100
-600,450
-2500,250
-2500,0
C
```

# Unterbausteifigkeiten

## Widerlager Ersatzfedern

Abmessungen Fundament:
B = 12 m
L = 8 m
A = 96 m2
Ix = L x B^3 / 12 = 1152 m4
Iy = B x L^3 / 12 = 512 m4

Bettungszahl:
ks = 25.000 kN/m3

Vertikale Feder:
Kz = A x ks = 2,4e6 kN/m

Tangentiale Federn:
Kx, Ky = 2 / 3 Kz = 1,6e6 kN/m

Rotationsfeder um die globale X-Achse:
Rx = ks x Ix = 28,8e6 kNm/rad

Rotationsfeder um die globale Y-Achse:
Ry = ks x Iy = 12,8e6 kNm/rad


## Stützenfundament Ersatzfedern

Abmessungen Fundament:
B = 12 m
L = 4 m
A = 48 m2
Ix = L x B^3 / 12 = 576 m4
Iy = B x L^3 / 12 = 64 m4

Bettungszahl:
ks = 30.000 kN/m3

Vertikale Feder:
Kz = A x ks = 1,44e6 kN/m

Tangentiale Federn:
Kx, Ky = 2 / 3 Kz = 0,96e6 kN/m

Rotationsfeder um die globale X-Achse:
Rx = ks x Ix = 17,28e6 kNm/rad

Rotationsfeder um die globale Y-Achse:
Ry = ks x Iy = 1,92e6 kNm/rad
