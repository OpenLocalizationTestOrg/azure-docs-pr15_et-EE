<properties
  pageTitle="Azure'i asjade komplekti KKK | Microsoft Azure'i"
  description="Korduma kippuvad küsimused asjade komplekti"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Korduma kippuvad küsimused asjade komplekti

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Mis vahe on kustutamise ressursirühma Azure portaali ja klõpsake käsku Kustuta eelkonfigureeritud lahenduse azureiotsuite.com rakenduses?

- Kui kustutate eelkonfigureeritud lahendus [azureiotsuite.com][lnk-azureiotsuite], kustutate ressursse, mis on ette valmistatud, kui olete loonud eelkonfigureeritud lahendus; Kui lisasite lisaressursid ressursirühma, need kustutatakse ka. 

- Kui kustutate ressursirühm [Azure portaali][lnk-azure-portal], kustutate ressursside kohta selle ressursirühma; peate ka seotud eelkonfigureeritud lahendus [Azure klassikaline portaalis]Azure Active Directory rakenduse kustutamiseks[lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Kui palju asjade jaoturi eksemplari saate tellimuse sättega? 

Kümme. Saate luua ka [Azure tugiteenuste Piletite] [ link-azuresupportticket] tõsta piiri, kuid vaikimisi olete saate ainult ette kümme asjade jaoturi kohta tellimus, nagu on kirjeldatud [Azure tellimuse piirangud][link-azuresublimits]. Seetõttu, kuna iga eelkonfigureeritud lahenduse sätted uue asjade jaoturi, saate ainult säte kuni kümme eelkonfigureeritud lahenduste antud tellimus. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Kui palju DocumentDB eksemplari saate tellimuse sättega?

Viiskümmend. Saate luua ka [Azure tugiteenuste Piletite] [ link-azuresupportticket] tõsta piiri, kuid vaikimisi ainult ettevalmistamise viiskümmend DocumentDB eksemplari tellimuse kohta. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Kui palju tasuta Bingi kaartide API saate tellimuse sättega?

Kaks. Saate luua ainult kahe sisemise tehingud tase 1 Bingi kaartide Suurettevõttelepinguga klientidele Azure tellimuse. Remote jälgimise lahendus on ette valmistatud vaikimisi plaaniga sisemise tehingud tase 1. Seetõttu saab ainult ettevalmistamise kahe remote jälgimisega seotud lahenduste tellimuse koos muudatusi ei.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Mul on vastuseks Kaug jälgimisega seotud lahenduse juurutamine staatiline kaart, kuidas lisada Bingi Interaktiivne kaart? 
1. [Azure'i]portaalis Enterprise QueryKey oma Bingi kaartide API saamiseks[lnk-azure-portal]: 
 1. Liikuge ressursirühm, kus teie ettevõtte Bingi kaartide API on [Azure portaali][lnk-azure-portal].
 2. Klõpsake nuppu kõik sätted, seejärel Key Management. 
 3. Märkate, et kahe klahvid: peavõtmega ja QueryKey. Kopeerige väärtuse jaoks QueryKey.

     > [AZURE.NOTE] Bingi kaartide API Enterprise konto ei ole? Luua [Azure portaali] [ lnk-azure-portal] mõõt klõpsates + uus Bingi kaartide API ettevõtte otsimine ja järgige viipasid loomiseks.

2. Rippmenüü uusima koodi [Asjade Azure Remote jälgimise][lnk-remote-monitoring-github].

3. Käivitage kohaliku või pilvepõhises juurutamise juhiste käsureal juurutamise hoidla /docs/ kausta. 

4. Pärast olete käivitanud kohalikule või pilvepõhises juurutus, otsige oma juurkausta jaoks soovitud *. tehke ühte loodud käigus juurutamist. Avage tekstiredaktoris seda faili. 

5. Järgmine rida lisada oma QueryKey kopeeritud väärtuse muutmiseks tehke järgmist. 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Eelkonfigureeritud lahenduse saate luua kui mul on Microsoft Azure'i DreamSpark?
Sel ajal, ei saa luua mõne [Microsoft Azure'i jaoks DreamSpark] eelkonfigureeritud lahenduse[ lnk-dreamspark] konto. Siiski saate luua ka [tasuta prooviversiooni konto Azure] [ lnk-30daytrial] vaid paar minutit, mis võimaldab teil luua eelkonfigureeritud lahenduse.

### <a name="how-do-i-delete-an-aad-tenant"></a>Kuidas kustutada rentnikku AAD?

Leiate Eric Golpe ajaveebipostitusest [samm-sammult juhendi kustutamine on Azure AD rentniku][lnk-delete-aad-tennant].

## <a name="next-steps"></a>Järgmised sammud

Samuti saate uurida muude funktsioonide ja võimaluste kohta asjade komplekti eelkonfigureeritud lahendused:

- [Eelkonfigureeritud sõnastikupõhise hooldustööd lahenduse ülevaade][lnk-predictive-overview]
- [Asjade turvalisuse kohalt kaudu][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
