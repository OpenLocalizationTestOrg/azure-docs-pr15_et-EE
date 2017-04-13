<properties
 pageTitle="Loogika rakendusemallid | Microsoft Azure'i"
 description="Siit saate teada, mis aitavad teil alustada varem loodud loogika appi mallide kasutamise kohta"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Loogika rakendusemallid

## <a name="what-are-logic-app-templates"></a>Mis on loogika rakenduse malle

Loogika rakenduse mall on valmismallide loogika rakendus, mille abil saate kiiresti alustada oma töövoo loomine. 

Need mallid on hea võimalus leida erinevate mustrite, mida saab ehitada loogika rakendused. Võite kasutada neid malle nimega-on või neid vastavalt stsenaariumile muuta.

## <a name="overview-of-available-templates"></a>Saadaolevad Mallid ülevaade

On palju saadaolevad Mallid, mis on praegu avaldatud loogika rakenduse platvorm. Näide kõigis kategooriates ning kasutada neid konnektorid tüüp on loetletud allpool.

### <a name="enterprise-cloud-templates"></a>Pilveteenuse ettevõttemallid.
Mallid, integreerida Dynamics CRM-iga, Salesforce, väljale, Azure'i bloobimälu ja teiste teie ettevõtte cloud vajadustele konnektorid. Näited, mida saab teha nende mallid sisaldavad oma viib korraldamine ja oma ettevõtte faili andmeid varundada.

### <a name="enterprise-integration-pack-templates"></a>Keelepaketi ettevõttemallid Integreerimine
VETER konfiguratsioone (kinnitage, eraldada, transformatsioon, rikastamine, marsruutimine) torujuhtmete vastu ka X12 EDI dokumendi üle AS2 ja muudab selle XML, samuti X12 ja AS2 sõnumi töötlemine.

### <a name="protocol-pattern-templates"></a>Protokoll mustri Mallid
Need Mallid koosnevad loogika rakendusi, mis sisaldavad protokolli mustrid näiteks päringu vastuse üle HTTP kui ka integratsioon FTP ja SFTP. Kasutage neid need on olemas või alusena keerukamaid protokolli mustrite loomise kohta.  

### <a name="personal-productivity-templates"></a>Tööviljakust Mallid
Mustrite täiustamiseks tööviljakust kaasata malle, mida päevas meeldetuletusi seada, muuta olulised tööüksusi ülesandeloenditele ja järjena automatiseerida allapoole ühe kasutaja kinnitamise samm.

### <a name="consumer-cloud-templates"></a>Tarbija cloud Mallid
Lihtsa Mallid, sotsiaalmeedia teenused, nt Twitteri, vaikne ja e-posti, lõpuks võimeline tugevdamine sotsiaalmeedia turundus algatused integreerida. Nende hulka kuuluvad näiteks pilves kopeerimine, malle, mis aitavad tootlikkuse kulunud aja säästmiseks traditsiooniliselt korduvate ülesannete kohta. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Kuidas luua loogika rakenduse malli abil 

Loogika rakenduse malli kasutamise alustamine, lähevad loogika rakenduse designer. Kui sisestate designer, avades olemasoleva loogika rakendus, laadib loogika rakenduse kujundaja vaate automaatselt. Juhul, kui loote uue loogika rakenduse, kuvatakse ekraani alla.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Selle kuval Kas soovite, et alustada tühja loogika rakenduse või valmismallide malli. Kui valite mõne malli, on varustatud täiendavat teavet. Selles näites me kasutame *uue faili loomisel Dropboxi, kopeerige see OneDrive'i* mall.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Kui otsustate kasutada malli, valige lihtsalt nupp *seda malli kasutada* . Küsitakse sisselogimiseks põhiste kontode kohta, millised ühendused Mall kasutab. Või kui olete varem need ühendused ühenduse loonud, saate valida jätkata, nagu allpool näha.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Pärast ühenduse loomisel ja valides *edasi*, avatakse loogika rakendust päringukujundaja vaade.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

Ülaltoodud näites on nii palju malle, mõni kohustuslik atribuudiväljad täidetakse sees konnektorid; siiski mõned võib siiski nõuda väärtus enne õigesti juurutada loogika rakendus. Kui proovite kasutada sisestamata mõned puuduvad väljad, teavitatakse teid tõrketeate.

Kui soovite tagastada malli vaataja, valige nupp *Mallid* ülemisel navigeerimisribal. Saate aktiveerida tagasi malli viewer, lähevad kaotsi kõik salvestamata edenemist. Enne vahetada uuesti sisse malli viewer, kuvatakse hoiatusteade, mis teavitab teid sellest.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Kuidas malli põhjal loodud loogika minirakenduse juurutamine

Kui olete teinud soovitud muudatused ja laaditud malli, valige Salvesta ülemises vasakus nurgas nuppu. See salvestab ja avaldab loogika rakenduse.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Kui soovite lisateavet kohta lisada rohkem sammud olemasoleva loogika rakenduse malli või teha muudatused üldiselt Loe [loogika rakenduse loomine](app-service-logic-create-a-logic-app.md).