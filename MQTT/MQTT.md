#MQTT (Message Queuing Telemetry Transport)

##目的
MQTT 是為了M2M(machine-to-machine)/IoT(Internet of things)所制定的通訊協定.為IBM和Eurotech
所共同制訂.  
由於協定目的在於IoT上使用，所以在設計上對於網路頻寬的要求以及所需的硬體資源也是相對較低。

##原理
MQTT採用Publish/Subscribe的機制來互通訊息。如下圖所示:  

![MQTT 架構](http://www.eclipse.org/community/eclipse_newsletter/2014/february/images/febarticle2.1.png )  

架構上主要就兩個角色:Client & Broker  
  
Broker: 為Clients之間彼此傳送訊息的橋樑，負責訊息的接收與推播機制。  
Client: 訊息的產生者與接收者皆為所定義的client。  
訊息傳遞的過程如下圖所示:  

  ![MQTT 訊息傳遞](http://4.bp.blogspot.com/-ZVDTkvwJCFM/UuTeb9DNwzI/AAAAAAAAAAc/Rl1TwX0q5jE/s1600/publishsubscribe.png)  
  
  開頭會在Broker上訂一個Topic。所有對這Topic有興趣的clients即連接上Broker，並對這Topic做訂閱的動作，以告知Broker未來若在這Topic上有收到任何訊息的話需要轉傳訊息給此client。而之後若任何Client有對此Topic有更新的消息想要推播給所有對此Topic有註冊的clients時，就將訊息publish到Borker的Topic上，Broker會負責後續的轉傳動作。
  Client在與Broker連接時可以指定一些參數如下圖所示:
  
  ![MQTT 鏈結參數](http://www.hivemq.com/wp-content/uploads/connect.png)
  
  其中的clientId是Broker用來區別各個連接session所用。Broker可能會根據其他的連線設定來對各session做特定動作。
  Username & password是用來做連線時的認證動作。在Broker上可以管理合法的使用者，並且指定各個使用者是否有Read/Write的權限。需特別提的是不同的Session可以使用同樣的Username & Password來做連接。  
  MQTT只是定義了溝通的協定。只要實踐了MQTT的溝通流程，就可以與同樣實踐MQTT的裝置間彼此溝通。

##特色
1. Publish/Subscribe的訊息推播機制
2. TCP/IP連線
3. 不論在Publish->Broker 或者 Subscribe->Broker 可指定Message的傳送/接收的QoS (quality of service)  
	a) At most once
	b) At least once
	c) Exactly once
4. Header 只需2bytes，減少頻寬浪費
5. 使用最後遺囑(Last Will and Testament)的機制。在Connect到Broker時可以指定Last will的訊息來指示，當Client與Broker異常斷線時Broker可以自動發送Last will訊息給所有註冊相關Topic的Clients

##工具
MQTT Android Library:  
    a. Mqtt client library - org.eclipse.paho.client.mqttv3-1.0.2.jar
    b. Mqtt Service library -  org.eclipse.paho.android.service-1.0.2.jar
  在Android的project中加入此兩個Library、即可利用其所提供的method來與MQTT的Broker溝通。(在我們的情境中即為在Heroku上所建的MQTT broker)  
  
  Heroku:  
  Heroku為一PaaS(Platform as a service)的平台。在其上開個Application後可利用嵌入其所提供的免費CloudMQTT服務來做為我們應用中的Broker角色。
  https://dashboard.heroku.com/apps/android-team-mqtt
  點選上面連結中的CloudMQTT後，即可到MQTT的控管頁面。

##參考
MQTT的介紹:  
http://www.hivemq.com/mqtt-essentials/  
MQTT for Android - set up:  
http://www.hivemq.com/blog/mqtt-client-library-enyclopedia-paho-android-service  
MQTT Sample code:  
https://github.com/bytehala/android-mqtt-quickstart  
http://www.hivemq.com/blog/mqtt-client-library-enyclopedia-paho-android-service  
http://www.longdw.com/mqtt-server-client-android/

