# Prírodou inšpirované počítanie

Témou tohto projektu je optimalizovať spotrebu elektriny v mikrosieti kde sa zameriavame na maximalizovanie životnosti batérie. Na životnosť batérie  negatívne vplýva množstvo vybitej energie a hĺba vybitia, čo sa snažíme minimalizovať. Okrem predĺženia životnosti sa zameriavame aj na minimalizovanie importovanej energie z hlavnej siete. Riešime tak viacrozmernú optimalizáciu, kde si používateľ môže nastaviť váhu jednotlivých objektívnych funkcií. Najlepšie výsledky dosahuje optimalizácia pomocou Firefly algoritmu, ktorú porovnávame a inými algoritmami (PSO, BA, heuristika).

# Spôsob implementácie modelu
Model je implementovaný pomocou štyroch algoritmov (heuristika, FA, BA a PSO). Pre každú implementáciu modelu je vytvorený osobitný jupyter notebook.

Jednotlivé jupyter notebooky sa delia na nasledovné bunky:
- Bunka metód vypočítania objektívnych funkcií
- Bunka triedy ```class Battery()```
- Bunka triedy použitého optimalizačného algoritmu algoritmu -- napr. ```class FireflyAlgorithm()```
- Bunka triedy konkrétnej implementácie daného modelu -- napr. ```class FAOptimizer()```
- Bunka pre spustenie optimalizácie

# Spustenie optimalizácie
Pre spustenie optimalizácie je potrebné spustiť v príslušnom jupyter notebooku všetky bunky. Posledná bunka slúži na vytvorenie objektu modelu a spustenie optimalizácie. Po vytvorení objektu modelu sa optimalizácia spúšťa metódou ```
optimize()```.


# Spustenie optimalizácie pomocou FA algoritmu.
vytvorenie modelu
```
fa = FAOptimizer(Nparticles=15, Niter=25, start=1, end=8760, windowSize=48, const1=1, const2=0, const3=2,
                 batCapacity=13, batSOC=0, houseNumber=93)
fa.optimize()

```

Spustenie optimalizácie heuristikou.
```
he = HeuristicOptimizer(start=1, end=8760, batCapacity=13, batSOC=0, 
                        houseNumber=1169)
he.optimizeHeuristic()
```
| parameter | význam |
| ------ | ------ |
| Nparticles | počet jedincov |
| Niter | počet iterácií |
| start | začiatočná hodina hodina optimalizácie |
| end | koncová hodina optimalizácie |
| windowSize | počet hodín počas ktorých prebieha výpočet optimalizácie |
| const1 | váha pre minimalizovanie importu |
| const2 | váha pre minimalizovanie stavov nečinnosti batérie |
| const3 | váha pre minimalizovanie straty životnosti batérie |
| batCapacity | kapacita batérie (kWh) |
| batSOC | počiatočné nabitie batérie |
| houseNumber | ID domácnosti z datasetu |

# Vstupné a výstupné dáta
Po dokončení optimalizácie sa ku kópii pôvodných .csv dát z použitého datasetu vypočíta nový import energie zo siete v danej hodine, ktorý sa ku nim doplní. Tak isto sa doplnia aj údaje o stave nabitia batérie, jej akcie v danej hodine, využitie vyprodukovanej energie a cena energie.

ukážka vstupných dát

|    | date     | time     | dateTime       | dayOfWeek | holiday | dayId | loadCons           | loadProd           | loadGrid            |
|----|----------|----------|----------------|-----------|---------|-------|--------------------|--------------------|---------------------|
| 1  | 1.1.2014 | 0:00:00  | 1.1.2014 0:00  | 3         | 1       | 1     | 1.74876666666667   | 0.0                | 1.74876666666667    |
| 2  | 1.1.2014 | 1:00:00  | 1.1.2014 1:00  | 3         | 1       | 1     | 1.4943333333333302 | 0.0                | 1.4943333333333302  |
| 3  | 1.1.2014 | 2:00:00  | 1.1.2014 2:00  | 3         | 1       | 1     | 0.9989             | 0.0                | 0.9989              |
| 4  | 1.1.2014 | 3:00:00  | 1.1.2014 3:00  | 3         | 1       | 1     | 0.608133333333333  | 0.0                | 0.608133333333333   |
| 5  | 1.1.2014 | 4:00:00  | 1.1.2014 4:00  | 3         | 1       | 1     | 1.15493333333333   | 0.0                | 1.15493333333333    |
| 6  | 1.1.2014 | 5:00:00  | 1.1.2014 5:00  | 3         | 1       | 1     | 0.642783333333333  | 0.0                | 0.642783333333333   |
| 7  | 1.1.2014 | 6:00:00  | 1.1.2014 6:00  | 3         | 1       | 1     | 0.602566666666667  | 0.0                | 0.602566666666667   |
| 8  | 1.1.2014 | 7:00:00  | 1.1.2014 7:00  | 3         | 1       | 1     | 0.9443             | 0.0                | 0.939483333333333   |
| 9  | 1.1.2014 | 8:00:00  | 1.1.2014 8:00  | 3         | 1       | 1     | 0.5443             | 0.801383333333333  | -0.257033333333333  |
| 10 | 1.1.2014 | 9:00:00  | 1.1.2014 9:00  | 3         | 1       | 1     | 0.943              | 2.00053333333333   | -1.05758333333333   |
| 11 | 1.1.2014 | 10:00:00 | 1.1.2014 10:00 | 3         | 1       | 1     | 0.839716666666667  | 3.93106666666667   | -3.09141666666667   |
| 12 | 1.1.2014 | 11:00:00 | 1.1.2014 11:00 | 3         | 1       | 1     | 1.13373333333333   | 2.7197833333333303 | -1.5859666666666699 |
| 13 | 1.1.2014 | 12:00:00 | 1.1.2014 12:00 | 3         | 1       | 1     | 1.01551666666667   | 1.26571666666667   | -0.250216666666667  |

ukážka výstupných dát

| newLoadGrid            | batterySOC         | batteryActions          | pvUsage            |
|------------------------|--------------------|-------------------------|--------------------|
| 4.355935401698769 | 3.844595363491342  | 2.6071687350320993      | 0.0                |
| 1.5174961459905802 | 2.6303315476893494 | 0.023162812657250065    | 0.0                |
| 0.9997592477067446 | 2.631190795396094  | 0.0008592477067446147   | 0.0                |
| 0.608109153815215 | 2.631166615877976  | -2.4179518117950494e-05 | 0.0                |
| 1.1549042394493676 | 2.6311375219940136 | -2.9093883962438838e-05 | 0.0                |
| 0.642653933554397 | 2.6310081222150776 | -0.0001293997789360013  | 0.0                |
| 0.6019384452239728 | 2.6303799007723834 | -0.0006282214426942545  | 0.0                |
| 0.9434110187640398 | 2.629490919536423  | -0.0008889812359602622  | 0.0                |
| 2.1617759829006822e-05 | 2.886595870629585  | 0.257104951093162       | 0.801383333333333  |
| -0.0065748443103994525 | 3.9375543596525158 | 1.0509584890229307      | 1.9939584890229307 |
| -0.0018785443080422404 | 7.0270258153444765 | 3.0894714556919607      | 3.9291881223586276 |
| -0.0006290003576394643 | 8.612446814986837  | 1.585420999642361       | 2.719154332975691  |
okrem uvedenýh stĺpcoch sú ku .csv dátam pridané aj finálne hodnoty ako súčet importu, exportu, finálna strata životnosti batérie, priemerná hĺbka vybitia, a iné hodnoty za celé optimalizované obdobie.

V jupyter notebooku sa zobrazí výpis výsledkov:
```
FINAL:
minut:  8.5418
Total demand:  117.22166666666666
Total price:  1.2618601718476863
Total Import:  42.08359360884748
Total Export:  49.523506300747876
pv to demand:  83.13777499095735

Charging cycles
num CharChanges  55
num Charges  28
num Discharges  28
num NullStates  3
batLifeLoss  20.15271309703101
avgDOD  0.4470222978384793
avgSOC  0.5529777021615204
avgDischAction  38.259819520941846
totalBat:  85.86476342163775
```