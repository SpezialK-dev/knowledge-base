z
# Some artikels that are worth a read

- https://kleine-technikwelt.de/alte-telefone-an-der-fritzbox.php

# Goals of this project 

The goals of this project are to convert the FetAp 611-2 into a normal working telephon for that we need to convert its dialing type from impulse to Multifrequenz dial mode. To do that we want to build a simple.

# Pictures taken from the web
since I didnt want to break the seal so far I am using some pictures from the web

![[Inside connector.png]]
[Source](http://www.ptt-apparate.ch/DBP/FeTAp_61/611-1_kieselgrau_1/thumb/05.jpg)


![[Circut.png]]
[Source](https://www.telefoonmuseum.eu/images/1960-1980/67/0067-schema.jpg)

### Pinout

https://de.wikipedia.org/wiki/Verbinderdose#VDo_7


in my case I have VDo_7 connection

**VDo 7**  

| Anschluss-  <br>belegung | Name                                                                                               | Bedeutung                                                                                                                                                                                                                                                 | Farbe des Drahtes (bei Regelbeschaltung) bei |                                            |         |
| ------------------------ | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------ | ------- |
| Stecker-  <br>belegung   | Dose*)  <br>(„[Telekom](https://de.wikipedia.org/wiki/Deutsche_Telekom "Deutsche Telekom")-Kabel“) | Dose*)  <br>(Standardkabel)                                                                                                                                                                                                                               |                                              |                                            |         |
| 1                        | a2                                                                                                 | Leitung „a“ wird durch das Gerät geschleift                                                                                                                                                                                                               | 1 weiß                                       | rot mit Doppelring (Ringabstand 34 mm)     | weiß    |
| 2                        | b2                                                                                                 | Leitung „b“ wird durch das Gerät geschleift                                                                                                                                                                                                               | 2 braun                                      | rot mit Doppelring (Ringabstand 17 mm)     | gelb    |
| 3                        | a                                                                                                  | Leitung „a“ der [a/b-Schnittstelle](https://de.wikipedia.org/wiki/A/b-Schnittstelle "A/b-Schnittstelle") der [Teilnehmeranschlussleitung](https://de.wikipedia.org/wiki/Teilnehmeranschlussleitung "Teilnehmeranschlussleitung")                          | 3 grün                                       | rot ohne Ring                              | rot     |
| 4                        | b                                                                                                  | Leitung „b“ der ab-Schnittstelle der Teilnehmeranschlussleitung                                                                                                                                                                                           | 4 gelb                                       | rot mit einfachem Ring (Ringabstand 17 mm) | schwarz |
| 5                        | W                                                                                                  | Wecker des zweiten Telefones, Zweitwecker oder [Tonrufzweitgerät](https://de.wikipedia.org/wiki/Tonrufzweitger%C3%A4t "Tonrufzweitgerät")                                                                                                                 | 5 grau                                       |                                            |         |
| 6                        | E                                                                                                  | „[Erde](https://de.wikipedia.org/wiki/Erdung "Erdung")“ für [Nebenstelle](https://de.wikipedia.org/wiki/Nebenstelle_\(Telefonanlage\) "Nebenstelle (Telefonanlage)") und [DEV](https://de.wikipedia.org/wiki/Dioden-Erd-Verfahren "Dioden-Erd-Verfahren") | 6 blau                                       |                                            |         |
| 7                        | G                                                                                                  | Externer [Gebührenanzeiger](https://de.wikipedia.org/wiki/Geb%C3%BChrenanzeiger "Gebührenanzeiger")                                                                                                                                                       | 7 rot                                        |                                            |         |
the table is taken form [wikipedia](https://de.wikipedia.org/wiki/Verbinderdose#VDo_7)
