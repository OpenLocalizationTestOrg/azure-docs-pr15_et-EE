<properties 
    pageTitle="Pilveteenuse saidikogumi Azure RemoteApp loomiseks | Microsoft Azure'i" 
    description="Saate teada, kuidas luua Azure RemoteApp salvestab andmed Azure pilveteenuse kasutuselevõtt." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Kuidas luua pilve saidikogumi Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

On kahte tüüpi [Azure RemoteApp saidikogumid](remoteapp-collections.md). 

- Pilveteenuse: asub täielikult Azure. Soovi korral saate salvestada kõik andmed pilves (nii, et vaid saidikogumi) või luua ühendus oma saidikogumi on VNET ja andmed salvestada.   
- Hübriid: sisaldab virtuaalse võrgu kohapealse Accessi - on vaja Azure AD kasutuse ja kohapealse Active Directory keskkonnas.

Selles õpetuses juhendab teid cloud saidikogumi loomine. Pole järgmisi toiminguid: 

1.  Looge on Azure RemoteApp kogum.
2.  Soovi korral saate konfigureerida kataloogi sünkroonimine. Kui kasutate Azure AD + Active Directory, tuleb kasutajad, kontaktid ja paroolide kohapealse Active Directoryst oma Azure AD rentniku sünkroonida.
5.  Rakenduste avaldamine.
6.  Kasutajate juurdepääsu konfigureerimine.


**Enne alustamist**

Peate tegema enne selle saidikogumi loomine järgmine:

- Azure RemoteApp [registreeruda](https://azure.microsoft.com/services/remoteapp/) . 
- Koguge teavet kasutajatele, mille soovite anda juurdepääsu. See võib olla Microsofti konto teavet või Active Directory töö kontoteave kasutajate jaoks.
- See toiming eeldab, et teil on, kas saab kasutada ühte mallide piltide tellimuse osana või teil on juba üles laaditud malli pilti, mida soovite kasutada. Kui soovite üles laadida muu malli pilti, saate seda teha mallide piltide lehe kaudu. Klõpsake **malli pilti üles laadida** ja järgige viisardis kuvatavaid juhiseid. 
- Soovite kasutada Office 365 ProPlusi pilt? Vaadake teave [siin](remoteapp-officesubscription.md).
- Kas soovite kohandatud rakendused või Kassa programmide? Uue [pildi](remoteapp-imageoptions.md) loomine ja kasutamine oma saidikogumi pilve.
- Aru saada, kas vajate mõne VNET ühendamiseks. Kui valite mõne VNET ühenduse, veenduge, et see vastab [suuruse suunised](remoteapp-vnetsizing.md) ja see, et [saate luua ühenduse RemoteApp](remoteapp-vnet.md). Vaadake lisateavet [VNET kavandamise artikkel ](remoteapp-planvnet.md).
- Kui kasutate mõnda VNET, otsustada, kas teie kohaliku Active Directory domeeni ühendamiseks.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Samm 1: Pilveteenuste kogumi - loomine koos või ilma on VNET##


Kasutage **vaid saidikogumi loomiseks**tehke järgmist.

1. Haldusportaal, minge lehele RemoteApp.
2. Klõpsake **uus > kiire loomine**.
3. Sisestage oma saidikogumi nimi ja valige oma piirkond.
4. Valige leping, et soovite kasutada – standard- või tavaline.
5. Valige mall, mida soovite selle saidikogumi jaoks kasutada. 

    **Näpunäide:** Tellimuse jaoks RemoteApp kaasas [malli pilte](remoteapp-images.md) , mis sisaldavad Office 365 või Office 2013 (prooviversiooni kasutamiseks) programmid, mõned (nt Word) ja teistele avaldada. Saate luua uus [pilt](remoteapp-imageoptions.md) ja kasutamine oma saidikogumi pilve.


1. Klõpsake **saidikogumi loomine RemoteApp**.
    
    **Oluline:** Võib kuluda kuni 30 minutit oma saidikogumi ette valmistada.

Kui teie saidikogumi Azure RemoteApp on loodud, topeltklõpsake selle saidikogumi nime. Mis avab lehe **Kiirkäivituse** – see on koht, kus olete selle saidikogumi konfigureerimine.

Järgmiste juhiste abil saate koostada on **pilv + VNET saidikogumi**:

1. Haldusportaal, minge lehele Azure RemoteApp.
2. Klõpsake nuppu **Uus** > **VNET loomine**.
3. Sisestage oma saidikogumi nimi.
4. Valige leping, et soovite kasutada – standard- või tavaline.
5. Valige VNET, mis on juba loodud. Ei tea, kuidas seda teha? Praegu on juhiseid teemas [hübriid](remoteapp-create-hybrid-deployment.md) .
6. Otsustage, kas soovite oma saidikogumi liituma domeeni. Kui jah, peate kasutama AD-ühendus integreerida Azure AD ja keskkonna Active Directory. Mis on kaetud all **Samm**2.
6. Klõpsake **saidikogumi loomine RemoteApp**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Samm 2: Konfigureerimine Active Directory Kataloogisünkroonimise (valikuline) ##

Kui soovite kasutada Active Directory, on vaja Azure RemoteApp Kataloogisünkroonimise Azure Active Directory ja teie kohapealse Active Directory sünkroonimine kasutajaid, kontaktide ja paroolide oma Azure Active Directory rentniku vahel. Kavandamise teavet teemast [Konfigureerimise Active Directory Azure RemoteApp](remoteapp-ad.md) . Samuti saate avada otse teavet [AD-ühendus](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) .

## <a name="step-3-publish-apps"></a>Samm 3: Rakenduste avaldamine ##

Azure RemoteApp rakendus on rakenduse või programmi, mis teie pakkumiseks. See asub saate üles laadida kogumiseks malli pilti. Kui kasutaja kasutab rakendust, rakendus kuvatakse käivitamiseks kohaliku keskkonnas, kuid see töötab tõesti Azure virtuaalse masina. 

Enne, kui teie kasutajad pääsevad rakendused, peate need avaldada – avaldamise apps saate kasutajate juurdepääs rakenduste kaudu kaugtöölaua klient.
 
Mitme rakendusi saate avaldada oma Azure RemoteApp saidikogumi. Avaldamislehe, klõpsake nuppu **Avalda** programmi lisamiseks. Kas saate avaldada menüü **Start** kaudu pildi malli või määrates tee rakenduse malli pilti. Kui otsustate lisada menüü **Start** , valige rakendus, avaldada. Kui valite rakenduse tee, sisestage nimi, rakenduse ja kui see on installitud malli pilti teed.

## <a name="step-4-configure-user-access"></a>Samm 4: Kasutaja juurdepääsu konfigureerimine ##

Nüüd, kui olete oma saidikogumi loonud, peate lisama kasutajad, mida soovite kasutada oma remote ressursse. Kui kasutate Active Directory, annate juurdepääsu tuleb olemas tellimusega seostatud Active Directory rentniku kasutajate loomiseks kasutatud selle saidikogumi.

1.  Klõpsake lehel Kiirkäivituse **konfigureerimine kasutajate juurdepääsu**. 
2.  Sisestage töö konto (Active Directory) või Microsofti konto, mille soovite anda juurdepääsu.

    **Märkmed:** 

    Veenduge, et kasutate selle “user@domain.com” vorming.

    Kui te kasutate teenusekomplekti Office 365 ProPlus oma saidikogumi, kasutage Active Directory identiteedid kasutajate jaoks. See aitab kinnitada litsentsimine. 

3.  Pärast seda, kui kasutajad on kinnitatud, klõpsake nuppu **Salvesta**.


## <a name="next-steps"></a>Järgmised sammud ##

See on õige – on nüüd loodud ja juurutatud oma Azure RemoteApp cloud saidikogumi. Järgmiseks on kasutajaid, laadige alla ja installige kaugtöölaua klient. URL-i leiate Azure'i RemoteApp Kiirkäivituse lehel kliendi jaoks. Seejärel on kasutajate logige kliendi ja avaldatud rakenduste juurde.

### <a name="help-us-help-you"></a>Aidake meil aidata teil 
Kas teadsite, et lisaks selles artiklis hindamine ja muutes kommentaaride alla all, saate muuta ise artiklit? Midagi puudu? Midagi on valesti? Kas midagi, mis on lihtsalt selgem kirjutada? Liikuge kerides üles ja klõpsake nuppu **Redigeeri github** muudatuste tegemiseks – need tulevad meile läbivaatamiseks, ja siis, kui me logige välja need, näete oma muudatused ja täiustused siinsamas.