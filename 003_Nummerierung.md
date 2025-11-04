
# Nummerierung von Materialien

Die Nummerierung der Materialien ist frei wählbar, aber es ist sinnvoll eine klare Nummerierung zu haben um Änderungen und einfügen von zusätzlichen Materialen ins gleiche Schema leicht zu ermöglichen.
Gewählte Materialnummerierung:

	Beton: 1 bis 99
	Bewehrungsstahl: 101 bis 199
	Spannstahl: 201 bis 299
	Stahl: 301 bis 399
	Holz: 401 bis 499
	Mauerwerk: 501 bis 599
	Rest: 601+

Materialien nach Bauteilen sollten getrennt definiert werden, auch wenn es das gleiche Material ist, aus folgenden Gründen:
- Kriechen und Schwinden in den Bauphasen - notwendig getrennte Materiale zu haben da Kriech- und Schwindkurven ggf. anders (Zeitpunkt der Bauteilaktivieren, Erstbelastung)
- Bewehrung / Spannstahl: ggf. Abweichende Eigenschaften
- Bei Änderungen im Projekt (Änderung der Güte) ist diese einfach durchführbar wenn bereits getrennt definiert

# Gruppen

## Logik

Anbei eine Nummerierung die die meisten Fälle abdeckt.

Fundierung: 
- Gruppe 0 (Pfähle) - diese werden i.d.R. vorab hergestellt
- Gruppe 1 bis 9 - Fundamente je nach Achse, wenn <= 9 Achsen

Unterbau Aufgehendes: 
- Gruppe 10 bis 90, pro Achse x0 bis x9, somit 9 Gruppen für Bauteile pro Achse für Unterbau

Überbau:
- Gruppe 100 bis 999

## Fundierung & Aufgehendes & Lager

|                                  Bauteil | Achse | Gruppennr.          |
| ---------------------------------------: | :---: | :------------------ |
|                         Federn Fundament | WL01  | 1                   |
|                   Kopplung zu den Lagern | WL01  | 1                   |
|                             Federn Lager | WL01  | 19                  |
|                         Federn Fundament | ST02  | 202                 |
| Kopplung Federn Fundament zu den Stützen | ST02  | 311                 |
|                                  Stützen | ST02  | 21 (West), 22 (Ost) |
|                         Federn Fundament | WL03  | 3                   |
|                   Kopplung zu den Lagern | WL03  | 3                   |
|                             Federn Lager | WL03  | 39                  |

## Überbau

|                             Bauteil | Bauabschnitt | Gruppennr. |
| ----------------------------------: | :----------: | :--------- |
|                    Längsträger West |     BA 1     | 101        |
|                    Längsträger West |     BA 2     | 102        |
|                     Längsträger Ost |     BA 1     | 201        |
|                     Längsträger Ost |     BA 2     | 202        |
|                  Endquerträger WL01 |     BA1      | 311        |
|                  Endquerträger WL03 |     BA2      | 331        |
|                      Fahrbahnplatte |     BA1      | 401        |
|                      Fahrbahnplatte |     BA2      | 402        |
| Kopplung Überbaustab zu Lagern WL01 |     BA1      | 110        |
|        Kopplung Überbaustab zu ST02 |     BA1      | 120        |
| Kopplung Überbaustab zu Lagern WL03 |     BA2      | 130        |

# Lastfälle

In der Anwendervorlage ist im Task: `002_TEMPLATE: LFs` eine möglich Nummerierung der Lastfälle und Speicherlastfällen von Umhüllenden angegeben. Diese ist aktuell zu halten, damit bei neuen Lastfällen keine bestehenden Lastfälle überschrieben werden.