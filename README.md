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
### [Node file](https://github.com/stanwang0222/SUMO-DEMO/blob/master/cross.nod.xml)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<nodes xmlns:xsi="http://www.w3.org/1301/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/nodes_file.xsd">

	<node id="1" x="-130.0" y="130.0" type="priority"/>
	<node id="2" x="-130.0" y="0.0" type="priority"/>
	<node id="3" x="-130.0" y="-130.0" type="priority"/>
	<node id="4" x="0.0" y="130.0" type="priority"/>	
	<node id="5" x="0.0" y="0.0" type="trafficlight"/>
	<node id="6" x="0.0" y="-130.0" type="priority"/>
	<node id="7" x="130.0" y="130.0" type="priority"/>
	<node id="8" x="130.0" y="0.0" type="priority"/>
	<node id="9" x="130.0" y="-130.0" type="priority"/>
   
</nodes>
```
### [Edge file](https://github.com/stanwang0222/SUMO-DEMO/blob/master/cross.edg.xml)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<edges xmlns:xsi="http://www.w3.org/33.301/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/edges_file.xsd">
   <edge id="f12" from="1" to="2" priority="78" numLanes="1" speed="33.3" />
   <edge id="f21" from="2" to="1" priority="78" numLanes="1" speed="33.3" />

   <edge id="f23" from="2" to="3" priority="78" numLanes="1" speed="33.3" />
   <edge id="f32" from="3" to="2" priority="78" numLanes="1" speed="33.3" />

   <edge id="f14" from="1" to="4" priority="78" numLanes="1" speed="33.3" />
   <edge id="f41" from="4" to="1" priority="78" numLanes="1" speed="33.3" />

   <edge id="f25" from="2" to="5" priority="78" numLanes="1" speed="33.3" />
   <edge id="f52" from="5" to="2" priority="78" numLanes="1" speed="33.3" />

   <edge id="f36" from="3" to="6" priority="78" numLanes="1" speed="33.3" />
   <edge id="f63" from="6" to="3" priority="78" numLanes="1" speed="33.3" />

   <edge id="f45" from="4" to="5" priority="78" numLanes="1" speed="33.3" />
   <edge id="f54" from="5" to="4" priority="78" numLanes="1" speed="33.3" />

   <edge id="f56" from="5" to="6" priority="78" numLanes="1" speed="33.3" />
   <edge id="f65" from="6" to="5" priority="78" numLanes="1" speed="33.3" />
   
   <edge id="f47" from="4" to="7" priority="78" numLanes="1" speed="33.3" />
   <edge id="f74" from="7" to="4" priority="78" numLanes="1" speed="33.3" />

   <edge id="f58" from="5" to="8" priority="78" numLanes="1" speed="33.3" />
   <edge id="f85" from="8" to="5" priority="78" numLanes="1" speed="33.3" />

   <edge id="f69" from="6" to="9" priority="78" numLanes="1" speed="33.3" />
   <edge id="f96" from="9" to="6" priority="78" numLanes="1" speed="33.3" />

   <edge id="f78" from="7" to="8" priority="78" numLanes="1" speed="33.3" />
   <edge id="f87" from="8" to="7" priority="78" numLanes="1" speed="33.3" />

   <edge id="f89" from="8" to="9" priority="78" numLanes="1" speed="33.3" />
   <edge id="f98" from="9" to="8" priority="78" numLanes="1" speed="33.3" />
</edges>
```
### [Connection file](https://github.com/stanwang0222/SUMO-DEMO/blob/master/cross.con.xml)
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