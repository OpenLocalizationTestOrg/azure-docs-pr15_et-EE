<properties
    pageTitle="Web Appi kloonimine Azure'i portaalis"
    description="Saate teada, kuidas oma veebirakenduste uute veebirakenduste Azure'i portaalis klooni."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure'i rakendust Service rakenduse kloonimine Azure portaali kaudu#

[Azure'i rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=529714) veebirakendustes kloonimise funktsiooni saate hõlpsalt klooni olemasoleva veebirakenduste vastloodud rakendusse teises regioonis või piirkonna. See võimaldab klientide juurutamiseks rakenduste arvust eri piirkondade kiire ja lihtne.

Rakenduse kloonimine pole praegu toetatud ainult premium taseme rakenduse teenuse lepingud. Uus funktsioon kasutab sama piirangud Web Appsi varundamise funktsioon, leiate [Azure'i rakendust Service veebirakenduse varundada](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Olemasoleva rakenduse kloonimine ##

Veebirakenduse peab töötama **Premium** režiimis, et te klooni web appi loomiseks.

1. [Azure portaali](https://portal.azure.com/), avage oma veebirakenduse tera.
2. Klõpsake menüü **Tööriistad**. Seejärel klõpsake **Tööriistad** labale **Klooni rakendus**.

    ![][1]

    > [AZURE.NOTE]
    > Kui web app ei ole juba **Premium** režiimis, saate toetatud režiimide rakenduse kloonimiseks teade. Sel hetkel, on teil võimalus valida **täiendamine**.
    
3. Sisestage tera **Klooni rakenduse** nimi uue veebirakenduse, ressursirühm ja rakenduse teenuse kavandamine. Lisaks kasutaja saab valida, kas arv allika web appi sätete klooni või mitte.

    ![][2]

4. Pärast nupu **Loo** platvormi alustada tööd klooni veebirakenduse andmeallika loomise kohta.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloonimine olemasoleva rakenduse App teenuse keskkonnas##

**Klooni rakenduse** tera on kliendil võimalus rakenduse kohapeal on valida olemasolev rakenduse teenuse keskkonnas.

## <a name="current-restrictions"></a>Praeguse piirangud ##

See funktsioon on praegu eelvaade, tegeleme lisada uusi võimalusi aja jooksul, järgmises loendis on praeguse tugi rakenduse kloonimine Azure'i portaalis teadaolevad piirangud:

- Azure'i liikluse haldur sätted on pole kloonitud
- Automaatne skaala sätteid on pole kloonitud
- Varunduse ajakava sätted on pole kloonitud
- VNET sätted on pole kloonitud
- Rakenduse ülevaateid ei ole automaatselt seadistatud sihtkoha web app
- Lihtne Auth sätted on pole kloonitud
- Kudu laiend on pole kloonitud
- Näpunäide reeglid on pole kloonitud
- Andmebaasi sisu on pole kloonitud


### <a name="references"></a>Viited ###
- [Web Appi kloonimine PowerShelli abil](app-service-web-app-cloning.md)
- [Azure'i rakendust Service veebirakenduse varundamine](web-sites-backup.md)
- [Kuidas luua rakenduse teenuse keskkonnas](app-service-web-how-to-create-an-app-service-environment.md)
- [Luua veebirakenduse rakenduse teenuse keskkonnas](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Sissejuhatus rakenduse teenuse keskkonnas](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png