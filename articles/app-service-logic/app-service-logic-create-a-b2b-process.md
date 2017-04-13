<properties 
   pageTitle="Azure'i rakendust Service B2B protsessi loomine | Microsoft Azure'i" 
   description="Ülevaade sellest, kuidas luua Äriprotsess Business" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>B2B protsessi loomine

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Business stsenaarium 
Contoso ja põhjatuul on kaks äripartneritega. Contoso (selle edasimüüja) saadab tellimuste üle valdkonna taseme nt AS2 transport põhjatuul (tarnija). Põhjatuul talletab kõik sissetulevad tellimused oma salvestusruumi. Tellimuste on XML-i sõnumite need kaks partnerite vahel. Kui sõnum on talletatud põhjatuul 's salvestusruumi seejärel põhjatuul on sisemised protsessid toime sellega tellimuse kohta.
 
Selle õpetuse eesmärk on luua, kuidas põhjatuul saate luua äriprotsessi kaudu, mille saate vastu võtta sõnumeid (ostutellimuse XML-i) selle partneri Contoso üle AS2 ja püsivad see siis oma salvestusruumi.


## <a name="capabilities-demonstrated"></a>Näidanud võimalused 
Selle õpetuse aitab eksponeerivad järgmisi võimalusi. 

- **Sõnumi transport**: edasimüüjalt ja tarnija võib olla erinevad platvormide, kuid nad võivad endiselt saavutada kahe vahel. Selles õpetuses on üle AS2 suhtlemine (kohaldamine lause 2). AS2 on mugav transport andmete vahel kaubanduspartneritega business ettevõtte suhtlus.
- **Andmete püsimine**: Kui sõnumi saanud AS2 üle, seejärel põhjatuul soovib püsima see enne edasiseks töötlemiseks. Konnektori abil saate selle jää püsima sõnumite oma salvestusruumi. Selles õpetuses Azure plekid on on kasutada nimega pilvepõhist salvestusruumi põhjatuul jaoks.
- **Loomise äriprotsessi**: rakenduses on voog mitme API rakendusi saate õmmeldud kokku business tulemuse saavutamiseks, nagu on näidatud siin.


## <a name="before-you-begin"></a>Enne alustamist
Selle õpetuse eeldab, et eelnevalt Azure'i rakenduse teenuste, teada, kuidas luua API rakendusi ja vool liita.


## <a name="steps-to-achieve-the-business-scenario"></a>Juhised saavutamiseks business stsenaarium
**Loomine ja konfigureerimine vajalik API rakendused**

1. **Azure'i salvestusruumi bloobimälu konnektori**eksemplari loomine. Selleks on vaja Azure Storage konto mandaat. Veenduge, et see on valmis enne alustamist, et luua.
2. Looge **BizTalki börsipäev partnerite halduse**eksemplari. Selleks on vaja tühja SQL-andmebaasi funktsiooni. Veenduge, et see on valmis enne alustamist, et luua.
3. Looge **AS2 konnektori**eksemplari. Selleks on vaja ka tühja SQL-andmebaasi funktsiooni. Veenduge, et see on valmis enne alustamist, et luua. Lisaks, kui soovite arhiivida sõnumite osana AS2 töötlemine, mida võib ette identimisteave on Azure'i bloobimälu selle loomise ajal.
4. Konfigureerige TPM (börsipäev partnerite haldus) teenus, mis on loodud.  
    1. Liikuge sirvides eespool kirjeldatud toimingud osana loodud TPM teenuse eksemplari.
    2. Kasutage **partnerite** suvandit jaotises **Lisa** partnerilt uue nimega **Contoso** ja oma profiilis *komponendid* lisada nõutavad AS2 identiteedi.
    3. Kasutage **partnerite** suvandit jaotises **Lisa** partnerilt uue nimega **põhjatuul** ja oma profiilis *komponendid* lisada nõutavad AS2 identiteedi.
    4. Kasutage **lepingute** suvandit **Lisa** uus AS2 põhjatuul ja Contoso vaheline jaotises *komponendid* . Põhjatuul on majutatud partneri siin ja Contoso saab külaline partner. Sisestage asjakohane teave sisselogimise krüptimist, tihendamise ja kinnitused konfigureerimine selle lepingu loomise ajal. Serdid on vaja kasutada, kui ta saab üles laadida kaudu **serdid** valik sirvimisel TPM teenus, mis on loodud.


## <a name="create-a-flow--business-process"></a>Looge vool / business protsessi
1. Saate luua uue kulgemist, kus esimese asjana AS2. Lohistage ja kukutage **AS2 konnektor** ja valige juba loodud eksemplar. Valige päästik nimega funktsioone.  
    ![][1]  
2. Järgmiseks lohistage ja kukutage **Azure'i salvestusruumi bloobimälu konnektor** ja valige juba loodud eksemplar. Valige toiming nimega funktsioonid ja, valige **Bloobimälu üles** soovitud funktsioone. Saate konfigureerida vastavalt vajadusele.
3. Nüüd luua ja juurutada voogu.


## <a name="message-processing--troubleshooting"></a>Sõnumi töötlemine ja tõrkeotsing
1. On aeg katsetada oleme juurutanud voogu. Saada AS2 lõpp-punkti uurinud AS2Connector astme loodud XML-i sõnumite pakitud AS2 (ühe AS2 lepingu loodud kohal). Võimalik, et peate lõpp-punkti autentimise konfigureerimine nii, et see oleks avalikult kättesaadav.
2. Täitmise teavet voogu on uurinud sirvides voogu ja seejärel samm arvesse meilivoo eksemplar, mida sai täidetud
3. AS2 töötlemine teavet, liikuge AS2Connector eksemplari seotud ja samm arvesse jälitus osa, siis järgige. Saate kasutada filtreid, mis on seotud teabe, mis on soovitud kuvamise piiramine.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
