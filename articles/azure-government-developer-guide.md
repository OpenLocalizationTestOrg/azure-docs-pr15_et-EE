<properties 
    pageTitle="Azure'i Government arendajate juhend" 
    description="See pakub võrdlus funktsioonid ja juhiseid Azure'i valitsuse rakenduste arendamise kohta" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Microsoft Azure'i Government arendaja juhend 

<p> Microsoft Azure'i Government on mõne füüsilise võrgu isoleeritakse eksemplari Microsoft Azure'i ja.  Juhendi arendajad pakkuda üksikasjad erinevused selle rakenduste arendajatele ja administraatoritele oleks vaja suhelda ja Azure eraldi regioonides töötamine.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>Selles teemas


+ [Ülevaade](#Overview)
+ [Arendajatele mõeldud juhised](#Guidance)
+ [Funktsioonid, mis on praegu saadaval Microsoft Azure Governmenti](#Features)
+ [Lõpp-punkti kaardistamine](#Endpoint)
+ [Järgmised sammud](#next)


## <a name="Overview"></a>Ülevaade

Microsoft Azure'i Government on eraldi eksemplari Microsoft Azure'i teenuse turvalisus ja nõuetele vastavus ja USA, riigi kohaliku- ja nende lahenduste pakkujaid vajadustega. Azure'i Government pakub füüsilise ja võrgu eraldi, mitte-USA valitsuse juurutuste ja läbivaadatud USA töötajad. 

Microsoft pakub mitmesuguseid tööriistu, luua ja juurutada pilv rakendusi Microsofti globaalne Microsoft Azure'i teenus ("globaalne") ja Microsoft Azure Governmenti teenused.

Kui loomine ja juurutamine rakenduste arendajatele Azure Governmenti teenuste asemel globaalne teenuse teadma kahte teenuste Peamised erinevused.  Täpsemalt häälestamise ja konfigureerimise oma programmeerimise keskkonda, ümber konfigureerida kirjalikult rakenduste ning rakendades neile teenustele Azure'i Government lõpp-punktid.

Selles dokumendis sisalduvat teavet Kokkuvõte nende erinevusi ja täiendab teavet [Azure Governmenti](http://www.azure.com/gov "Azure Governmenti") saidi ja [Microsoft Azure'i tehniline teek](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN-i") MSDN-is saadaval. Ametlik teave võite kasutada paljudes muudes kohtades manustamiseks nagu funktsiooni [Microsoft Azure'i Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure'i Trust Center" /), [Azure'i dokumentatsiooni keskmist](https://azure.microsoft.com/documentation/) ja [Azure ajaveebide] (https://azure.microsoft.com/blog/ "Azure ajaveebide" /). 

See sisu on mõeldud partnerid ja arendajad, kes on Microsoft Azure'i valitsuse.



## <a name="Guidance"></a>Arendajatele mõeldud juhised
Kuna enamik tehnilise sisu, mis on praegu saadaval eeldab, et rakenduste globaalne teenuse, mitte Microsoft Azure Governmenti väljatöötatud, on teil tagada, et arendajad teadlikud Peamised erinevused Azure'i valitsuse töötaks välja jaoks oluline.

- Esmalt on teenuste ja funktsioonide erinevused, see tähendab, et teatud funktsioonid, mis on määratud piirkondades globaalne teenuse ei pruugi olla saadaval Azure Government.

- Teiseks funktsioonid, mis on saadaval Azure Government, on globaalne teenuse konfigureerimine erinevused.  Seega peaksite oma proovi kood, konfiguratsioone ja tagamaks, et on koostamise ja täitmise Azure Governmenti pilveteenustega keskkonnas.


## <a name="Features"></a>Funktsioonid, mis on praegu saadaval Microsoft Azure Governmenti
Azure'i Government on praegu saadaval järgmisi teenuseid meile gov – IOWA nii meile gov – VIRGINIA piirkondades.

- Virtuaalmasinates
- Pilveteenused
- Salvestusruumi
- Active Directory
- Ajasti
- Virtuaalne võrgunduse
- SQL-andmebaas
- Azure'i failid
- Media Services
- Liikluse haldur
- Teenuse siini
- StorSimple
- Redis vahemälu
- Azure'i varukoopiad
- Automatiseerimine
- ExpressRoute
- jne.

Muud teenused on saadaval, ja rohkem teenuseid lisatakse pidevalt.  Kõige värskemate teenuste loendit, leiate [piirkondade lehe](https://azure.microsoft.com/regions/#services) , mis toob esile iga saadaval piirkonna- ja nende teenuste.  

Praegu meile gov – Iowa ja meile gov – Virginia on andmekeskuste, mis toetavad Azure'i Government.  Vaadake piirkondade leht praegune andmekeskuste ja pakutavaid kohal.

## <a name="Endpoint"></a>Lõpp-punkti kaardistamine

Kasutage järgmises tabelis, juhendab teid Microsoft Azure'i ja SQL-andmebaasi avaliku lõpp-punktid vastendamisel Azure Governmenti kindla lõpp-punktid.


Teenuse tüüp|Azure'i avaliku|Azure'i Government
---|---|---
Haldusportaal|Manage.windowsazure.com|Manage.windowsazure.us
Üldine|*. windows.net|*. usgovcloudapi.net
Core|*. core.windows.net|*. core.usgovcloudapi.net
Arvuta|*. cloudapp.net|*. usgovcloudapp.net
Bloobimälu|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Järjekorda salvestusruum|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Tabelimälu|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Teenuste haldus|Management.Core.Windows.net|Management.Core.usgovcloudapi.net
SQL-andmebaas|*. database.windows.net|*. database.usgovcloudapi.net
ARM koormusetasakaalustusega lõpp-punkti|https://Management.Windows.net|https://Management.usgovcloudapi.net  

* ARM autentimiseks kaudu Azure AD palun viide [Azure ressursihaldur taotlused autentimine](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Järgmised sammud

Kui olete huvitatud õppe rohkem ja Azure Governmenti kohta leiate kasutada osa saamiseks allpool olevaid linke.

- **[Prooviversiooni kasutajaks registreerumine](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Hankimine ja Azure Governmenti juurdepääs](http://azure.com/gov)**

- **[Azure'i Government ülevaade](/azure-government-overview)**

- **[Azure'i Government ajaveeb](http://blogs.msdn.com/b/azuregov/)**

- **[Azure'i nõuetele vastavus](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
