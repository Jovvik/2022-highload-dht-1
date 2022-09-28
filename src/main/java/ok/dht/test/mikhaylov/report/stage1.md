# Отчет по первой задаче

Используемая БД - LSM дерево, референсная реализация.

## Нагрузочное тестирование

### PUT

Тестирование производилось на пустой БД, с помощью скрипта [put.lua](scripts/put.lua).

#### Меньший объем значений

Подбором было установлено, что при 40000 запросах в секунду нагрузка на БД
является стабильной (все запросы успевают выполняться), а при 50000 запросах
в секунду нагрузка на БД становится нестабильной (выполняется только 49750 запросов).

Результат выполнения:
```
❯ wrk2 -d 60 -t 1 -c 1 -R 40000 -L -s scripts/put.lua "http://localhost:19234/"
Running 1m test @ http://localhost:19234/
  1 threads and 1 connections
  Thread calibration: mean lat.: 0.897ms, rate sampling interval: 10ms
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   565.35us  321.29us   3.11ms   59.04%
    Req/Sec    42.26k     2.56k   50.33k    65.42%
  Latency Distribution (HdrHistogram - Recorded Latency)
 50.000%  564.00us
 75.000%  836.00us
 90.000%    1.00ms
 99.000%    1.14ms
 99.900%    1.71ms
 99.990%    2.76ms
 99.999%    3.09ms
100.000%    3.11ms

  Detailed Percentile spectrum:
       Value   Percentile   TotalCount 1/(1-Percentile)

       0.014     0.000000           56         1.00
       0.126     0.100000       200722         1.11
       0.236     0.200000       401451         1.25
       0.345     0.300000       600296         1.43
       0.454     0.400000       800112         1.67
       0.564     0.500000      1001442         2.00
       0.618     0.550000      1100173         2.22
       0.673     0.600000      1200943         2.50
       0.727     0.650000      1300006         2.86
       0.782     0.700000      1401399         3.33
       0.836     0.750000      1500817         4.00
       0.863     0.775000      1550664         4.44
       0.890     0.800000      1600494         5.00
       0.917     0.825000      1650549         5.71
       0.944     0.850000      1700207         6.67
       0.971     0.875000      1750039         8.00
       0.985     0.887500      1776019         8.89
       0.998     0.900000      1800116        10.00
       1.012     0.912500      1826342        11.43
       1.025     0.925000      1850604        13.33
       1.039     0.937500      1876478        16.00
       1.045     0.943750      1887816        17.78
       1.052     0.950000      1901044        20.00
       1.059     0.956250      1914108        22.86
       1.065     0.962500      1925432        26.67
       1.072     0.968750      1937697        32.00
       1.078     0.971875      1944105        35.56
       1.086     0.975000      1949771        40.00
       1.097     0.978125      1956347        45.71
       1.108     0.981250      1962689        53.33
       1.119     0.984375      1968695        64.00
       1.125     0.985938      1971699        71.11
       1.132     0.987500      1975212        80.00
       1.138     0.989062      1978120        91.43
       1.145     0.990625      1981392       106.67
       1.152     0.992188      1984556       128.00
       1.155     0.992969      1985923       142.22
       1.159     0.993750      1987624       160.00
       1.162     0.994531      1988919       182.86
       1.166     0.995313      1990655       213.33
       1.170     0.996094      1991976       256.00
       1.174     0.996484      1992838       284.44
       1.181     0.996875      1993546       320.00
       1.208     0.997266      1994310       365.71
       1.255     0.997656      1995082       426.67
       1.321     0.998047      1995864       512.00
       1.361     0.998242      1996253       568.89
       1.422     0.998437      1996636       640.00
       1.525     0.998633      1997029       731.43
       1.631     0.998828      1997418       853.33
       1.723     0.999023      1997809      1024.00
       1.771     0.999121      1998003      1137.78
       1.857     0.999219      1998200      1280.00
       1.963     0.999316      1998395      1462.86
       2.061     0.999414      1998591      1706.67
       2.129     0.999512      1998787      2048.00
       2.177     0.999561      1998883      2275.56
       2.237     0.999609      1998980      2560.00
       2.349     0.999658      1999079      2925.71
       2.449     0.999707      1999179      3413.33
       2.479     0.999756      1999276      4096.00
       2.505     0.999780      1999321      4551.11
       2.539     0.999805      1999372      5120.00
       2.575     0.999829      1999419      5851.43
       2.611     0.999854      1999470      6826.67
       2.667     0.999878      1999518      8192.00
       2.721     0.999890      1999541      9102.22
       2.765     0.999902      1999565     10240.00
       2.817     0.999915      1999591     11702.86
       2.847     0.999927      1999614     13653.33
       2.867     0.999939      1999643     16384.00
       2.875     0.999945      1999651     18204.44
       2.919     0.999951      1999663     20480.00
       2.973     0.999957      1999675     23405.71
       3.015     0.999963      1999687     27306.67
       3.037     0.999969      1999699     32768.00
       3.043     0.999973      1999708     36408.89
       3.049     0.999976      1999714     40960.00
       3.053     0.999979      1999719     46811.43
       3.063     0.999982      1999724     54613.33
       3.075     0.999985      1999730     65536.00
       3.081     0.999986      1999733     72817.78
       3.087     0.999988      1999737     81920.00
       3.091     0.999989      1999739     93622.86
       3.095     0.999991      1999742    109226.67
       3.101     0.999992      1999746    131072.00
       3.103     0.999993      1999748    145635.56
       3.103     0.999994      1999748    163840.00
       3.105     0.999995      1999750    187245.71
       3.107     0.999995      1999752    218453.33
       3.109     0.999996      1999756    262144.00
       3.109     0.999997      1999756    291271.11
       3.109     0.999997      1999756    327680.00
       3.109     0.999997      1999756    374491.43
       3.109     0.999998      1999756    436906.67
       3.111     0.999998      1999758    524288.00
       3.111     0.999998      1999758    582542.22
       3.111     0.999998      1999758    655360.00
       3.111     0.999999      1999758    748982.86
       3.111     0.999999      1999758    873813.33
       3.113     0.999999      1999760   1048576.00
       3.113     1.000000      1999760          inf
#[Mean    =        0.565, StdDeviation   =        0.321]
#[Max     =        3.112, Total count    =      1999760]
#[Buckets =           27, SubBuckets     =         2048]
----------------------------------------------------------
  2399954 requests in 1.00m, 153.35MB read
Requests/sec:  39999.26
Transfer/sec:      2.56MB
```

#### Больший размер значений

Если повысить размер значений до приблизительно 1Кб
(`wrk.body = "value#" .. index` было заменено на `wrk.body = string.rep("value#" .. index, 100)`)
, то 40000 запросов/сек перестало быть стабильной нагрузкой.
Максимальной стабильной нагрузкой стало 25000 запросов/сек.

Результат выполнения `wrk2` для значений размера 1Кб (без Detailed Percentile spectrum для краткости)
```
❯ wrk2 -d 60 -t 1 -c 1 -R 25000 -L -s scripts/put.lua "http://localhost:19234/"
Running 1m test @ http://localhost:19234/
  1 threads and 1 connections
  Thread calibration: mean lat.: 1.182ms, rate sampling interval: 10ms
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     0.95ms  664.30us  19.74ms   71.10%
    Req/Sec    26.31k     2.60k   49.78k    69.32%
  Latency Distribution (HdrHistogram - Recorded Latency)
 50.000%    0.90ms
 75.000%    1.34ms
 90.000%    1.76ms
 99.000%    2.07ms
 99.900%    5.15ms
 99.990%   18.06ms
 99.999%   19.66ms
100.000%   19.76ms

#[Mean    =        0.945, StdDeviation   =        0.664]
#[Max     =       19.744, Total count    =      1249889]
#[Buckets =           27, SubBuckets     =         2048]
----------------------------------------------------------
  1499970 requests in 1.00m, 95.84MB read
Requests/sec:  24998.84
Transfer/sec:      1.60MB
```

#### Выводы

- При стабильной нагрузке (40000 запросов/сек) (для небольшого размера значений) на сервер нет "длинного хвоста" в
  распределении latency, максимальное значение не превышает 3.1 мс.
- При более высокой нагрузке (50000 запросов/сек) наблюдается "длинный хвост" в распределении latency,
  90-й перцентиль превышает 100мс, что уже неприемлемо:
  ```
    50.000%  738.00us
    75.000%    1.11ms
    90.000%  116.35ms
    99.000%  292.10ms
    99.900%  322.56ms
    99.990%  327.42ms
    99.999%  327.68ms
    100.000%  327.68ms
  ```
- При бóльших объемах данных стабильная нагрузка понижается, сервер способен обработать только 25000 запросов/сек.
  При этом также повышается стандартное отклонение latency и 99.99-й перцентиль достигает 18.06мс, что может быть неприемлемо.
  Кроме того, за единицу времени обрабатывается меньше данных (по сравнению с 40000 запросов/сек и малым размером значений)
  \- 1.6Мб вместо 2.56Мб, т.е. понижение пропускной способности сервера не связано с тем, что данные долго ходят по сети. 

### GET

БД заполнялась с помощью той же команды `wrk2` и того же скрипта [put.lua](scripts/put.lua) со значениями размера 1Кб.
Всего в БД 1.5 млн записей _(на самом деле чуть-чуть меньше, т.к. несколько запросов не прошли)_ и 1.804 Гб данных.
Этого достаточно, чтобы данные не помещались в оперативную память.

