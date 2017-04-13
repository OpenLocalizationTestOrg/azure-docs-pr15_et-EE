## <a name="send-messages-to-event-hubs"></a>Sündmuse jaoturi sõnumeid saata

Java kliendi teek sündmuse jaoturi jaoks on saadaval kasutada Maven projektides [Maven keskses hoidlas](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)ja viidatakse kasutamine järgmine sõltuvus deklaratsioon Maven projekti faili.    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Erinevat tüüpi Koosta keskkonnas, saate otseselt hankida uusim välja JAR failid [Maven keskses hoidlas](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) või [väljaanne jaotuse punkti github](https://github.com/Azure/azure-event-hubs/releases).  

Lihtsa sündmuse Publisheri, importida *com.microsoft.azure.eventhubs* alla sündmuse jaoturi kliendi tunnid ja *com.microsoft.azure.servicebus* paketi jaoks kasuliku tunnid, nt levinud erandid Azure'i teenus siini meilikliendi ühiskasutusse antud. 

Järgmises näites jaoks kõigepealt looma uue projekti Maven konsooli/shell rakenduse lemmik Java arenduskeskkond. Klassi nimetatakse ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Asendage nimeruum ja sündmuse keskuses nimed sündmuse keskuses loomisel kasutatavaid väärtusi.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Looge ainsuses sündmus, muutes selle UTF-8 bait kodeeringuga stringi. Seejärel luua uue eksemplari sündmuste jaoturi kliendi ühendusstringi ja saatke sõnum.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
