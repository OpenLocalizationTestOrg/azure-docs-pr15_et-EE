<properties 
   pageTitle="Azure'i Külastajate OS juhend klienditoe ja aegumine poliitika | Microsoft Azure'i" 
   description="Teave mis Microsoft toetab osas Azure'i Külastajate OS kasutatavaid pilveteenustega." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure'i Külastajate OS klienditoe ja aegumine poliitika
Selle lehel olev teave on seotud Azure'i Külastajate operatsioonisüsteem ([Külastajate OS](cloud-services-guestos-update-matrix.md)) pilveteenustega töötaja ja web rollide (PaaS). See ei kehti Virtuaalmasinates (IaaS). 

Microsoft on avaldatud [tugiteenuste poliitika Külastajate OS](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Lehe loete nüüd kirjeldatakse, kuidas on rakendatud poliitika.

Poliitika 

1. Microsoft toetab **vähemalt uusima kaks peredele Külastajate OS**. Kui pere on kasutuselt kõrvaldatud, kliendid on 12 kuu ametlik aegumine uuem toetatud Külastajate OS pere värskendamine.
2. Microsoft toetab soovitud **vähemalt kaks viimast versiooni toetatud Külastajate OS peredele**. 
3. Microsoft toetab on **vähemalt kaks uusimad Azure'i SDK**juures. Kui SDK versioon on kasutuselt kõrvaldatud, kliendid on 12 kuu ametlik aegumine uuema versiooni värskendamiseks. 

Aeg-ajalt rohkem kui kaks peredele või väljalasete võib toetatud. Ametlik Külastajate OS tugiteabe kuvatakse [Azure'i Külastajate OS ja SDK ühilduvus maatriks](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Kui külaline OS pere või versioon on kasutuselt kõrvaldatud 


Külalisena OS **pere** kasutusele millalgi pärast uue ametliku versiooni Windows Server operatsioonisüsteemi versiooni. Iga kord, kui on kasutusele võetud uus külaline OS pere, Microsoft pensionile vanimast Külastajate OS pere. 

Uute Külastajate OS **versioonid** tutvustatakse iga kuu lisada MSRC uusimate värskenduste kohta. Tavaline igakuine värskendusi, kuna Külastajate Opsüsteemi versioon on tavaliselt keelatud 60 päeva pärast. See hoiab vähemalt kaks Külastajate OS versiooni iga pere kasutamiseks saadaval. 

### <a name="process-during-a-guest-os-family-retirement"></a>Protsessi ajal Külastajate OS pere aegumine 


Kui pensionile on teada, kliendid on 12 kuu "üleminek" aega enne vanemate pere eemaldatakse ametliku teenus. Sel ajal ülemineku pikendada Microsoft äranägemisel. [Azure'i Külastajate OS ja SDK ühilduvus maatriksi](cloud-services-guestos-update-matrix.md)postitatud värskendused.

Aegumine järkjärguline protsess algab perioodi ülemineku 6 kuud. Sel ajal:

1. Microsoft teavitab kliendid pensionile. 
2. Azure'i SDK uuem versioon ei toeta endine Külastajate OS pere.
3. Uue kasutuselevõttu ja ümberpaigutustega pilveteenuste ei tohi endine pere

Microsoft kasutab ka edaspidi tutvustada uue Külastajate Opsüsteemi versioon, mis sisaldavad MSRC uusimate värskenduste viimase päevani ülemineku perioodi, nimetatakse "aegumiskuupäeva". Sel ajal, kuvatakse kõik töötab pilveteenustega toetuseta jaotises Azure'i SLA. Microsoft on õigus jõustada uuele versioonile, kustutamine või peatada nende teenuste pärast seda kuupäeva.



### <a name="process-during-a-guest-os-version-retirement"></a>Protsessi ajal Külastajate Opsüsteemi versioon aegumine 
Kui kliendid nende Külastajate OS automaatselt värskendada, on neil kunagi muretsema tegelevad Külastajate OS versioonid. Need alati kasutamine Külastajate OS uusim versioon.

Külalisena OS versioonid on välja iga kuu. Tänu oma avaldatakse regulaarselt, iga versioon on fikseeritud eluiga.

Eluiga 60 päeva versioon on "*keelatud*". "Keelatud" tähendab, et versioon on eemaldatud Azure klassikaline portaalist. See saab enam saab määrata ka CSCFG konfigureerimise faili. Olemasoleva juurutuste on jäänud töötab, kuid uute juurutuste ning olemasoleva juurutuste kood ja konfiguratsiooni värskendused pole lubatud. 

Hiljem, on Külastajate OS versiooni "*aegub*" ja mis tahes installide töötab see versioon on täiendatud ja seadmine automaatseks värskendamiseks Külastajate OS tulevikus. Aegumise tehakse pakettidena nii, et perioodi jooksul väljalülitamine aegumise võivad erineda. 

Nende perioodide võidakse enam Microsofti äranägemisel kliendi üleminekud lihtsustamiseks. [Azure'i Külastajate OS ja SDK ühilduvus maatriksi](cloud-services-guestos-update-matrix.md)edastatakse kõik muudatused.



### <a name="notifications-during-retirement"></a>Teatiste ajal aegumine 

* **Pere aegumine** <br>Microsoft kasutab ajaveebipostituste ja Azure klassikaline portaali teatis. Kliendid, kes on ikka veel endine Külastajate OS pere teavitatakse otse määratud teenuseadministraatorid side (e-posti, portaali sõnumid, telefonikõne) kaudu. Kõik muudatused postitatakse sellele lehele ja RSS-kanali loendis selle lehe alguses. 


* **Versiooni aegumine** <br>Kõik muudatused postitatakse sellele lehele ja selle lehe, sh väljaanne, keelatud ja aegumiskuupäev alguses loetletud RSS-kanal. Teenuste administraatorid saavad e-kirju, kui neil on keelatud Külastajate Opsüsteemi versioon või pere juurutuste. Need meile ajastuse võivad erineda. Üldiselt nad on vähemalt kuu enne väljalülitamine, kuigi see ajastus ei ole ametlik SLA. 


## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Kuidas saan leevendada migreerimise mõju?**

Kasutage oma pilveteenustega kujundamiseks uusima Külastajate OS pere. 

1. Migreerimise uuem pere varakult plaanimise alustamine. 
2. Häälestada ajutine testi juurutuste Testige oma uut pere töötavate pilveteenuses. 
3. Teie külaline Opsüsteemi versioon **automaatse** määramine (osVersion = * failis [.cscfg](cloud-services-model-and-package.md#cscfg) ) nii, et uute Külastajate OS versioonide migreerimise toimub automaatselt.

**Mida teha, kui mu veebirakenduse nõuab süvitsi integreerimine OS?**

Kui teie web rakenduse arhitektuur nõuab süvitsi sõltuvus aluseks operatsioonisüsteemi, kasutage platvormi toetatud võimalusi, nt [käivitus tööülesannete](cloud-services-startup-tasks.md) või muid laiendatavuse vahendeid, mis võivad esineda tulevikus. Teise võimalusena saate ka [Azure'i Virtuaalmasinates](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – taristu teenus), kus teie vastutate säilitades aluseks operatsioonisüsteemi.
 
## <a name="next-steps"></a>Järgmised sammud
Vaadake üle uusima [Külastajate OS välja](cloud-services-guestos-update-matrix.md).
