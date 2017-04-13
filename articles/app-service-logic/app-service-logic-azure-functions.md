<properties
   pageTitle="Loogika rakendustega Azure funktsioonide abil | Microsoft Azure'i"
   description="Vaadake, kuidas loogika rakenduste Azure funktsioonide kasutamine"
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
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Loogika rakendustega Azure funktsioonide abil

Loogika rakenduses Azure'i funktsioonide abil saate käivitada kohandatud pikad C# või node.js.  [Azure'i funktsioonid](../azure-functions/functions-overview.md) pakub server-tasuta arvuti Microsoft Azure. See on kasulik läbimiseks järgmisi toiminguid:

* Täpsem vorming või Arvuta väljade loogika rakenduses
* Töövoo sees arvutuste tegemine
* Funktsiooni loogika rakenduste funktsioonid, mis on toetatud C# või node.js laiendamine

## <a name="create-a-function-for-logic-apps"></a>Funktsiooni loogika rakenduste loomine

Soovitame luua **Üldise Webhook - sõlm** või **Üldise Webhook - C#** mallide abil Azure'i funktsioonide portaalis uue funktsiooni. See automaatne – kuvab malli, mida aktsepteerib `application/json` loogika rakenduse kaudu.  Kui valite menüü **integreerida** Azure'i funktsioonid pole **režiimi** **Webhook** ja **Üldise JSON** **Webhook tüüp** .  Funktsioonid, mida kasutada neid malle automaatselt avastanud ja jaotises Designeris loogika rakenduste loendis **Azure funktsioonide minu piirkonnas.**

Webhook funktsioonid on taotluse vastu ja lähevad kaudu meetod on `data` muutujana. Pääsete oma last atribuutide abil dot märkega nagu `data.foo`.  Näiteks lihtsa JavaScripti funktsioon, mis teisendab stringi kuupäeva ajaväärtuseks näeb välja nagu järgmises näites:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Kõne Azure'i funktsioonide loogika rakenduse kaudu

Designer, kui klõpsate menüü **toimingud** , saate valida **Azure'i funktsioonide minu piirkonnas**.  See on loetletud ümbriste teie tellimus ja võimaldab teil valida funktsiooni, millele soovite helistada.  

Valige funktsiooni, palutakse teil määrata objekti Sisestuskeel last. See on sõnum, mis saadab loogika rakenduse funktsiooni ja peab olema JSON objekt. Näiteks kui soovite edastada **Viimati muudetud** kuupäev Salesforce'i päästik, funktsioon last võib näeb välja järgmine:

![Viimase riskijuhtimissüsteemi kuupäev][1]

## <a name="trigger-logic-apps-from-a-function"></a>Funktsiooni päästik loogika rakendused

Samuti on võimalik loogika rakenduse funktsioonis käivitamiseks.  Selle tegemiseks lihtsalt luua loogika rakenduse käsitsi käivitada. Lisateabe saamiseks vaadake [loogika rakendused nimega sissenõutav lõpp-punktid](app-service-logic-http-endpoint.md).  Klõpsake oma funktsioonis luua HTTP POSTITUSKOHT käsitsi käivitada URL-i last, mida soovite saata loogika rakenduse abil.

### <a name="create-a-function-from-the-designer"></a>Funktsiooni kujundaja loomine

Saate luua ka node.js webhook funktsiooni kaudu designer. Esmalt valige **Azure funktsioonide minu ala** ja seejärel valige oma funktsioon ümbris.  Kui teil pole veel ümbris, peate looma ühte [Azure'i funktsioonide portaalis](https://functions.azure.com/signin). Valige **Loo uus**.  

Luua malli põhjal andmed, mida soovite arvutada, määrake kontekstis objekt, mille kavatsete lähevad funktsiooni. See peab olema JSON objekt. Näiteks, kui te kaotate faili sisu FTP toimingu, konteksti last näeb välja järgmine:

![Konteksti last][2]

>[AZURE.NOTE] Kuna seda objekti ei olnud oma hääle stringina, lisatakse sisu otse JSON last. Siiski siis tõrge välja, kui see pole märgiks JSON (ehk teisisõnu öeldes stringi või mõne JSON objekti/massiiv). Selle siirata stringina, lisage lihtsalt hinnapakkumised nagu on näidatud esimese pildil selle artikli.

Designer loob Tekstisisese saate luua malli funktsioon. Eelnevalt luuakse sõltuvalt sellest, mida plaanite lähevad funktsiooni.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
