<properties
   pageTitle="Loogika rakenduse stsenaarium: Azure'i funktsioonide teenuse siini lisamispäästiku loomine | Microsoft Azure'i"
   description="Azure'i funktsioonide abil saate luua teenuse siini päästik loogika rakenduse"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Loogika rakenduse stsenaarium: Azure'i teenus siini lisamispäästiku loomine Azure funktsioonide abil

Azure'i funktsioonide abil saate luua loogika rakenduse käivitamiseks, kui teil on vaja juurutada pikaajalisi kuulajale või tööülesande. Näiteks saate luua funktsioon, mis järjekorda kuulata ja seejärel kohe tule loogika rakenduse tõuketeatised käivitamiseks.

## <a name="build-the-logic-app"></a>Loogika rakenduse koostamine

Selles näites on teil töötab iga loogika rakenduse, mis tuleb käivitada funktsiooni. Esmalt looge loogika rakendus, mis sisaldab lisamispäästiku HTTP taotluse. Funktsiooni kõned selle lõpp-punkti iga kord, kui järjekorda sõnumi saabumisel.  

1. Luua uue loogika rakenduse; Valige **käsitsi – kui an HTTP päring on saanud** päästik.  
   Soovi korral saate määrata JSON skeemi kasutada sõnumiga järjekorra tööriista nagu [jsonschema.net](http://jsonschema.net)abil. Kleepige skeemiga käivitada. See aitab mõista andmete kuju designer ja Lisateavet meilivoo hõlpsalt atribuudid töövoo kaudu.
1. Lisage lisatoimingud, soovitud pärast järjekorra sõnumi saabumisel. Näiteks saada e-posti kaudu Office 365.  
1. Loogika rakenduse loomiseks tagasihelistamise URL-i päästik selle loogika rakenduse salvestamine. URL-i kuvatakse päästik kaart.

![Funktsiooni tagasihelistamise URL-i kuvatakse päästik kaardil][1]

## <a name="build-the-function"></a>Funktsiooni koostamine

Järgmiseks peate looma funktsioon, mis käivitab toimivad ja kuulake järjekorda.

1. [Azure'i funktsioonide portaalis](https://functions.azure.com/signin)valige **Uus funktsioon**ja valige mall **ServiceBusQueueTrigger - C#** .

    ![Azure'i funktsioonide portaal][2]

2. Teenuse siini järjekorda konfigureeriks (mis kasutab Azure teenuse siini SDK `OnMessageReceive()` kuulajale).
3. Kirjutage lihtsa funktsiooni helistamiseks loogika rakenduse lõpp-punkti (varem loodud), kasutades järjekorda sõnumi käivitamiseks. Siin on täielik näide funktsiooni. Näide kasutab mõnda `application/json` sõnumi sisutüübi, kuid te saate seda muuta vajaduse korral.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Katsetada, saate lisada järjekorda sõnumi tööriista nagu [Teenuse siini Exploreri](https://github.com/paolosalvatori/ServiceBusExplorer)kaudu. Vt loogika rakenduse tule kohe pärast funktsiooni saab teate.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
