SUMO使用大全
===
Hackmd 上的教學更新比較快 : 
[Hackmd Link](https://hackmd.io/@Q3rbDqtfQLurEkgTZfo5FA/B1IeJt0BN)

教學文件中的程式位置 : 
[Github Link](https://github.com/stanwang0222/SUMO-DEMO)

[TOC]
# 參考資料
[原文SUMO百科](http://sumo.sourceforge.net/userdoc/Sumo_at_a_Glance.html)
# 安裝 SUMO
安裝網址：http://sumo.sourceforge.net/

# SUMO 使用目的
SUMO 用來模擬車輛在一個網絡道路中的移動性，並且可以將道路中每一輛車的數值作為結果輸出，而透過 SUMO Tools 的工具 TraCI ，可以改變網絡道路中的參數，透過寫程式的方式來控制整個網絡道路元件或車輛的移動性。

SUMO 大致上需要至少<font color=red>**兩種 input 資訊**</font>才可以正常的模擬車輛的移動，第一種是模擬的地圖環境(net.xml)，他是最基礎的道路地圖，提供給車輛去行走，第二種是車輛的出現與軌跡(rou.xml)，他是記錄哪一輛車在模擬時間睇幾秒的時候會出現在這個網絡道路之中，並且要透過道路中的哪一條道路來醒走，一直走到終點才會離開網絡。
# SUMO 安裝（Windows為例）
* [下載最新版本](http://sumo.sourceforge.net/userdoc/Downloads.html)
* [下載其他版本](https://sourceforge.net/projects/sumo/files/sumo/)

在 Windows 環境下，個人偏好下載(.zip)的版本，解壓縮在自己想放的資料夾位置後即可開始使用。
## 環境變數設定
開啟 Windows 的環境參數配置頁面（在開始工具列直接搜尋「環境變數」即可）。

![](https://i.imgur.com/3BLrGjC.png)

在 Path 的環境路徑下新增 SUMO 資料夾內的 bin 路徑

![](https://i.imgur.com/K954pLx.png)

新增環境變數 SUMO_HOME，路徑指到 SUMO 的資料夾位置

![](https://i.imgur.com/33SzeMP.png)

設定完以上兩個之後，可以打開 cmd 打上 sumo ，如果成功看到類似以下的文字，即表示環境配置完成。
![](https://i.imgur.com/6ZvIqCk.png)





# 使用教學
## 地圖環境 net.xml
### 方法一、自行建點建邊做道路網絡
#### [Node file](https://github.com/stanwang0222/SUMO-DEMO/blob/master/cross.nod.xml)
node 有 id名稱、X軸座標、Y軸座標與點型態(常見：priority、traffic_light)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<nodes xmlns:xsi="http://www.w3.org/1301/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/nodes_file.xsd">

	<node id="1" x="-130.0" y="130.0" type="priority"/>
	<node id="2" x="-130.0" y="0.0" type="priority"/>
	<node id="3" x="-130.0" y="-130.0" type="priority"/>
	<node id="4" x="0.0" y="130.0" type="priority"/>	
	<node id="5" x="0.0" y="0.0" type="traffic_light"/>
	<node id="6" x="0.0" y="-130.0" type="priority"/>
	<node id="7" x="130.0" y="130.0" type="priority"/>
	<node id="8" x="130.0" y="0.0" type="priority"/>
	<node id="9" x="130.0" y="-130.0" type="priority"/>
   
</nodes>
```
#### [Edge file](https://github.com/stanwang0222/SUMO-DEMO/blob/master/cross.edg.xml)
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
#### [Connection file](https://github.com/stanwang0222/SUMO-DEMO/blob/master/cross.con.xml)
用來將道路中的 edge 做連線使其成為一個車輛可行走的路線(可以右轉、直走、左轉或迴轉)
```xml
<?xml version="1.0" encoding="iso-8859-1"?>
<connections xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://sumo.dlr.de/xsd/connections_file.xsd">
	<connection from="f41" to="f12"/>
	<connection from="f21" to="f14"/>

	<connection from="f12" to="f23"/>
	<connection from="f12" to="f25"/>
	<connection from="f52" to="f21"/>
	<connection from="f52" to="f23"/>
	<connection from="f32" to="f21"/>
	<connection from="f32" to="f25"/>

	<connection from="f23" to="f36"/>
	<connection from="f63" to="f32"/>

	<connection from="f14" to="f45"/>
	<connection from="f14" to="f47"/>
	<connection from="f54" to="f41"/>
	<connection from="f54" to="f47"/>
	<connection from="f74" to="f41"/>
	<connection from="f74" to="f45"/>

	<connection from="f65" to="f54"/>
	<connection from="f65" to="f52"/>
	<connection from="f65" to="f58"/>
	<connection from="f45" to="f52"/>
	<connection from="f45" to="f56"/>
	<connection from="f45" to="f58"/>
	<connection from="f25" to="f54"/>
	<connection from="f25" to="f56"/>
	<connection from="f25" to="f58"/>
	<connection from="f85" to="f54"/>
	<connection from="f85" to="f52"/>
	<connection from="f85" to="f56"/>

	<connection from="f36" to="f65"/>
	<connection from="f36" to="f69"/>
	<connection from="f56" to="f63"/>
	<connection from="f56" to="f69"/>
	<connection from="f96" to="f65"/>
	<connection from="f96" to="f63"/>

	<connection from="f47" to="f78"/>
	<connection from="f87" to="f74"/>

	<connection from="f78" to="f85"/>
	<connection from="f78" to="f89"/>
	<connection from="f58" to="f87"/>
	<connection from="f58" to="f89"/>
	<connection from="f98" to="f87"/>
	<connection from="f98" to="f85"/>

	<connection from="f69" to="f98"/>
	<connection from="f89" to="f96"/>
</connections>
```

#### 道路網絡生成指令
netconvert.exe 存在 <SUMO_HOME>/bin 的路徑下，必須引入 node file 與 edge file 才可建立
```
netconvert --node-files=[MyNodes.nod.xml] --edge-files=[MyEdges.edg.xml] --connect-files=[MyConnect.con.xml] --output-file=[MySUMONet.net.xml]
```
其中 connect file 可填可不填，填了道路會按照設定的形式來建立車輛可行走的方向，若不填則會直接假設車輛可以允許與相連的所有邊來行走
```
netconvert --node-files=[MyNodes.nod.xml] --edge-files=[MyEdges.edg.xml] --output-file=[MySUMONet.net.xml]
```

### 方法二、透過 OpenStreepMap，載入真實地圖
1. 開啟 OpenStreepMap 元件
```
python <SUMO_HOME>/tools/osmWebWizard.py
```
2. 調整引入的資訊(Polygon、Vehicle)
3. 選取地圖範圍

### 方法三、直接透過 SUMO 的圖形化介面自己拉圖


以上三個方法，執行完成後會產出一個 net.xml 檔，此為模擬地圖環境的檔案，是整個模擬中的第一個 input 資訊。



## Trips 生成指令
在 <SUMO_HOME>/tools/ 路徑下
```
randomTrips.py -n osm.net.xml -b 0 -e 3600 -p 0.6 -o osm.trips.xml
```
-b : begin time
-e : end time
-p : ((end time) - (begin time)) / (這段期間要加入的車輛數)

## Route 生成指令
在 <SUMO_HOME>/bin/ 路徑下
```xml
duarouter -n osm.net.xml -r osm.trips.xml -o osm.rou.xml --ignore-errors
```

## sumocfg 編寫
```xml
<?xml version="1.0" encoding="UTF-8"?>

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
## 執行 SUMO
如果想要看到執行的介面，觀察車輛移動性的話，指令需要下 sumo-gui，如果只是單純的想要執行，不想看到執行的使用者介面，只需要打 sumo 即可，"YOUR.sumocfg" 是自己定義的 sumocfg 檔案名稱，"--tripinfo-output" 這段指令用來要求模擬器在模擬結束的時候，輸出每一輛車輛在網絡中的模擬參數，包括旅行時間、等待時間、燃料消耗等等，"YOUR.tripinfo.xml" 是自己定義的輸出檔案名稱。
```
sumo-gui -c [YOUR.sumocfg] --tripinfo-output [YOUR.tripinfo.xml]
```
```
sumo -c [YOUR.sumocfg] --tripinfo-output [YOUR.tripinfo.xml]
```

## 