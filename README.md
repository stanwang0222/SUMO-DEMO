SUMO使用大全
===
[TOC]
# 參考資料
[原文SUMO百科](http://sumo.sourceforge.net/userdoc/Sumo_at_a_Glance.html)
# 安裝 SUMO
安裝網址：http://sumo.sourceforge.net/

# SUMO 使用目的
SUMO 用來模擬車輛在一個網絡道路中的移動性，並且可以將道路中每一輛車的數值作為結果輸出，而透過 SUMO Tools 的工具 TraCI ，可以改變網絡道路中的參數，透過寫程式的方式來控制整個網絡道路元件或車輛的移動性。
# SUMO 安裝（Windows為例）
* [下載最新版本](http://sumo.sourceforge.net/userdoc/Downloads.html)
* [下載其他版本](https://sourceforge.net/projects/sumo/files/sumo/)

在 Windows 環境下，個人偏好下載(.zip)的版本，解壓縮在自己想放的資料夾位置後即可開始使用。
## 環境變數設定
開啟 Windows 的環境參數配置頁面（在開始工具列直接搜尋「環境變數」即可）。
![](https://i.imgur.com/3BLrGjC.png)


# 使用教學
## 自行建點建邊做道路網路
### Node file
{%gist stanwang0222/13a36fc85f59a070b23d10018d37496f%}
### Edge file
{%gist stanwang0222/4783c110b21a1e7dcee8f0ac6265b0aa%}
### Connection file
用來將edge做連線成為一個路口連線
```xml
<?xml version="1.0" encoding="iso-8859-1"?>
<connections xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/connections_file.xsd">
	<connection from="1i" to="2o"/>
	<connection from="2i" to="1o"/>
	<connection from="3i" to="4o"/>
	<connection from="4i" to="3o"/>
</connections>
```

### Net生成指令
```xml
netconvert --node-files=MyNodes.nod.xml --edge-files=MyEdges.edg.xml --output-file=MySUMONet.net.xml
```
### Trips生成指令
在sumo/tools/路徑下
```xml
randomTrips.py -n osm.net.xml -b 0 -e 3600 -p 0.6 --binomial=6000 -o osm.trips.xml
```
-b : begin time
-e : end time
-p : ((end time) - (begin time)) / (這段期間要加入的車輛數)
--binorial=N : 使用exponential的方式放數N輛車

### Rou生成指令
在sumo/bin/路徑下
```xml
duarouter -n osm.net.xml -r osm.trips.xml -o osm.rou.xml --ignore-errors
```

### sumocfg編寫
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- generated on 02/28/19 17:23:22 by Eclipse SUMO Version 1.0.1
-->

<configuration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/sumoConfiguration.xsd">

    <input>
        <net-file value="osm.net.xml"/>
        <route-files value="osm.passenger.trips.xml"/>
    </input>

    <processing>
        <ignore-route-errors value="true"/>
    </processing>

    <routing>
        <device.rerouting.adaptation-steps value="180"/>
    </routing>

    <time>
      <begin value="0"/>
      <end value="1000"/>
      <time-to-teleport value="-1"/>
      <srand value="23423"/>
      <route-steps value="-1"/>
    </time>

    <report>
        <verbose value="true"/>
        <duration-log.statistics value="true"/>
        <no-step-log value="true"/>
    </report>

</configuration>

```

## 透過OpenStreepMap，載入真實地圖
1. 開啟OpenStreepMap元件
```xml
python <SUMO_HOME>/tools/osmWebWizard.py
```
### 環境變數設定
加入兩個環境變數
1. PATH : C:\Program Files\sumo-1.1.0\bin
2. SUMO_HOME : C:\Program Files\sumo-1.1.0
:::warning
C:\Program Files 為SUMO路徑，請自行更改
:::