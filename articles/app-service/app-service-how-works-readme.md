<properties 
    pageTitle="Kuidas töötab Azure rakenduse teenus" 
    description="Siit saate teada, kuidas töötavad rakenduse teenuse" 
    keywords="rakenduse, azure rakenduse teenus, skaala scalable, rakenduse teenusleping, rakenduse teenuse kulu"
    services="app-service" 
    documentationCenter="" 
    authors="yochay" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="02/10/2016" 
    ms.author="yochay"/>

# <a name="how-app-service-works"></a>Rakendust Service tööpõhimõtted

Azure'i rakenduse teenus on loodud ees insenerid täna praktiline probleemide lahendamiseks pilveteenus. Rakenduse teenuse keskendub esitada ülemuse arendaja tööviljakuse tõstmiseks ilma kompromisse on vaja esitamisel rakenduste cloud tasandil. Rakenduse teenus pakub funktsioone ja raamistiku tuleb koostada ettevõtte ärivaldkonna rakenduste ajal toetavad arendajatele kõige populaarsemate arengu keelte (.NET, Java, PHP, Node.JS ja Python).
Rakenduse teenusega arendajad teha järgmist.

* Väga paindlik veebirakenduste koostamine
* Kiiresti koostada Mobile'i rakendus tagasi-lõpeb väärtusega lihtsa mobiilsideseadmete võimalusi, nagu andmete tagasi otstega kogumi, kasutaja autentimise ja push teatised mobiilirakenduste kohta. 
* Rakendada, juurutada ja avaldada API-de API rakendustega.
* Siduda ärirakenduste koos töövood ja muuta andmete loogika rakendustega.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

Kõik rakenduse tüübid sõltuvad scalable ja paindlikus veebirakenduse platvormi, mis võimaldab arendajatel on optimeeritud kogu elutsükli kasutusvõimalusi rakenduse kujundus rakenduse hooldustööd. Elutsükli võimaluste jaoks lubamiseks tehke järgmist.

* Kiirülevaate rakenduse loomine - alustada nullist või valige mõni OSS paketi Azure'i turuplatsilt. 
* Pidev juurutus - automaatselt uue koodi populaarsed andmeallika juhtelemendi lahendusi, nt salvestusruum võrgus teenused, nt OneDrive'i ja Dropboxi TFS, GitHub ja BitBucket ja sünkroonimine sisu juurutamine.
* Valmistamisel testimiseks – sujuvalt tootmiseelse keskkonnas loomine ja haldamine liiklus neile läheb osa. Silumine pilves, kui vaja ja tööks tagasi siis, kui probleemid ei leitud.
* Pakett-töö - ja töötab asünkroonne tööülesannete koodi käivitada tausta protsessi või aktiveerimine oma kood (nt sõnumid kuvataks Azure'i salvestusruumi järjekorras) põhjal ja plaanitud ajal (CRON).
* Skaleerimist rakendus – kasutage ühte mastaapimiseks horisontaalselt ja vertikaalselt automaatselt vastavalt liikluse ja ressursi kasutamine teenust palju võimalusi. Privaatne keskkonnas pühendunud rakenduste konfigureerimine   
* Rakenduse - võimendada säilitades paljud silumine ja diagnostika peatada enne probleemide ja tõhusalt nende lahendamiseks ühte reaalajas (nt automaatne probleemilahendus ja live silumine funktsioonidega) või pärast tegelikult analüüsib logid ja mälu puistab
 
Panna, rakenduse teenuse võimaluste võimaldab arendajatel keskenduda oma koodi ja ühed ja väga paindlik olekus kiiresti ühendust võtta. Funktsioonidega API ja loogika rakenduse arendajatel luua tegelike enterprise rakendusi teisendamine takistused Ärilahendused ning eeldusel cloud integreerimine.  

[AZURE.INCLUDE [app-service-blueprint-how-app-service-works](../../includes/app-service-blueprint-how-app-service-works.md)]
