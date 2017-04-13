<properties
   pageTitle="Azure identiteedi haldamine | Microsoft Azure'i"
   description="Selgitatakse ja võrreldakse toodud erinevad meetodid, hübriid spikritest kohta – ruumide/cloud äärist Azure ulatuvad identiteedi haldamine."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Azure identiteedi haldamine

Ettevõtte süsteemides Windows põhjal, kasutage Active Directory (AD) identiteedi halduse teenuste osutamise rakenduste. AD toimib hästi asutusesiseses keskkonnas, kuid saate laiendada võrgu taristule pilveteenusesse teil on mõned olulised otsuseid teha kohta, kuidas hallata identiteedi. Peaks teil laiendada oma kohapealse domeene lisada pilveteenuses VMs? Peaks loote uue domeenide pilves ja kui jah, kuidas? Tuleks rakendada oma mets pilveteenuses või teete, Azure Active Directory (AAD) kasutada?

Selles artiklis kirjeldatakse mõned levinud võimalused koosoleku selle stsenaariumi probleemidega ja aitab teil kindlaks teha, milline lahendus kõige paremini teie vajadustele, oma vajaduste järgi.

## <a name="using-azure-active-directory"></a>Azure Active Directory abil

Saate AAD pilveteenuses AD domeeni luua ja linkida selle asutusesisese AD domeeni. AAD abil saate konfigureerida ühekordse sisselogimise (SSO) rakenduste kaudu pilveteenuses kasutajate jaoks.

[! [0]][0]

AAD on lihtne viis rakendada turvalisus domeeni pilveteenuses. See on kasutatud paljudes Microsoft rakendustes, näiteks Microsoft Office 365. 

AAD kasutamise eelised:

- Ei ole vaja säilitada Active Directory infrastruktuur pilveteenuses. AAD on täiesti hallatava ja haldab Microsoft.

- AAD pakub sama identiteedi teavet, mis on saadaval kohapealse.

- Autentimine võib juhtuda Azure, vähendamine välistes rakendustes ja kasutajate kohapealse domeeni ühendust võtta.

Millega tuleks arvestada AAD kasutamisel punkte.

- Identiteedi teenused on piiratud kasutajad ja rühmad. On puudub võimalus autentida teenuse ja arvuti kontod.

- Ühenduvus tuleb konfigureerida kohapealse domeeni säilitada AAD kataloogi sünkroonitud. 

- Teie vastutate avaldamise rakendused, mida kasutajad pääsevad pilves AAD kaudu.

Üksikasjalikku teavet, lugege [Rakendamise Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Active Directory abil pilves ühendatud mõne kohapealse mets

Saate majutada kohapealse AD Directory Services (AD DS), kuid rakenduse elemendid asukohta Azure'i hübriidjuurutuse stsenaarium võib olla tõhusam kopeerida see funktsioon ja AD hoidla pilveteenusesse. Seda moodust saab vähendada, saates autentimine põhjustada latentsus ja kohalik luba kutsed pilvest tagasi AD DS kohapealse töötab. 

[! [1]][1]

Seda moodust jaoks on vaja luua oma domeeni pilveteenuses ning liituda asutusesiseses kogumis. Saate luua VMs majutada AD DS teenuseid.

Pilveteenuses eraldi domeeni kasutamise eelised:

- Pakub võimalust autentida kasutaja, teenuse ja arvuti kontod asutusesisese ja pilve.

- Saab kasutada sama identiteedi teavet, mis on saadaval kohapealse.

- Ei ole vaja hallata eraldi AD mets; domeeni pilveteenuses saate kuuluvad asutusesiseses kogumis.

- Saate rakendada rühmapoliitika kohapealse GPO objektide domeeni pilves määratletud.

Kaalutlused pilveteenuses eraldi domeeni kasutamise jaoks

- Peab teil luua ja hallata oma AD DS-i serverid ja domeeni pilveteenuses.

- Võib olla mõni sünkroonimise latentsus domeeni serverid pilves ja kohapealse serverites vahel.

See arhitektuur konfigureerimise kohta leiate teemast [Ulatub Active Directory Directory Services (LISAB) Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Active Directory abil eraldi mets

Ettevõte, mis töötab kohapealse Active Directory (AD) võib olla mets, mis sisaldab palju erinevaid domeene. Domeenide abil saate hoida eraldi turvalisuse põhjustel võib-olla otstarbekas alade vahelise eraldustaseme esitada, kuid vahel domeenide usalda seosed luues saate ühiskasutusse anda.

[! [2]][2]

Ettevõte, mis kasutab eraldi domeenid saate ära Azure'i paigutades ühte või mitut domeenid sisse eraldi mets pilveteenuses. Teise võimalusena võib ettevõtte soovi säilitada kõik cloud ressursid loogiliselt erinevad need hoitakse kohapeal ja talletada teavet cloud ressursid oma kataloogi osana ka pilveteenuses hoitavate mets.

Pilveteenuses eraldi mets kasutamise eelised:

- Saate rakendada kohapealse identiteedid ja eraldada ainult Azure identiteedid.

- Ei ole vaja kopeerida: asutusesisene AD mets Azure, võrgu latentsuse mõju vähendamiseks.

Kaalutlused

- Kohapealse identiteedid pilveteenuses autentimise sooritab asutusesisese eest võrgu *humala* reklaami serverites.

- Peab teil rakendada oma AD DS-i serverid ja mets pilves, ja metsade vastav usalda seoseid luua.

Dokumendi [loomine Azure Active Directory Directory Services (LISAB) ressursi mets] [ adds-forest-in-azure] kirjeldatakse, kuidas rakendada seda moodust üksikasjalikumalt.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Kasutades Active Directory Federation Services (ADFS) Azure

ADFS-i käitamise kohapealse, kuid hübriidjuurutuse stsenaariumi, kus rakendused asuvad Azure võib olla tõhusam rakendada selle funktsiooni pilves, nagu allpool näidatud.

[! [3]][3]

See arhitektuur on eriti kasulik:

- Lahendusi, mis kasutavad ühendatud autoriseerimine esitamist veebirakenduste partneri ettevõtted.

- Veebibrauserid töötab väljaspool ettevõtte tulemüüri juurdepääsu toetavatest meilisüsteemidest.

- Süsteemid, mis võimaldavad kasutajatel juurdepääs veebirakenduste, ühendades volitatud välise seadmest, nt kaugarvutite, märkmike ja muude mobiilseadmete jaoks. 

Azure ADFS-i kasutamise eelised:

- Saate kasutada taotluste-rakendusi.

- Pakub võimalus usaldada välispartnereid autentimiseks.

- Suure hulga autentimise Protokollid ühilduvuse pakub.

Kaalutlused Azure'i ADFS-i abil

- See peab teil rakendada oma ADFS-i veebiteenuse rakenduse puhverserver lisatakse ning ADFS-i serverid pilveteenuses.

- See arhitektuur võib olla keeruline konfigureerida.

Üksikasjalikku teavet, lugege [Rakendamise Active Directory Federation Services (ADFS) Azure][adfs-in-azure].

## <a name="next-steps"></a>Järgmised sammud

Ressursside allpool selgitatakse, kuidas rakendada arhitektuurides, mis on selles artiklis kirjeldatud.

- [Azure Active Directory rakendamine][implementing-aad]
- [Active Directory kataloogiteenuste ulatub (LISAB) Azure][extending-adds]
- [Azure Active Directory Directory Services (LISAB) ressursi mets loomine][adds-forest-in-azure]
- [Azure Active Directory Federation Services (ADFS) rakendamine][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Pilveteenuse identiteedi arhitektuur Azure Active Directory abil"
[1]: ./media/guidance-identity/figure2.png "Turvaline hübriid võrgu arhitektuur Active Directoryga"
[2]: ./media/guidance-identity/figure3.png "Turvaline hübriid võrgu arhitektuur eraldi AD domeenid ja metsade"
[3]: ./media/guidance-identity/figure4.png "Turvaline hübriid võrgu arhitektuur ADFS-i abil"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md