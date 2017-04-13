<properties
    pageTitle="Kuidas luua hübriid saidikogumi Azure RemoteApp | Microsoft Azure'i"
    description="Saate teada, kuidas RemoteApp, mis loob ühenduse oma sisevõrgus juurutamise loomiseks."
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

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Kuidas luua hübriid saidikogumi Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

On kahte tüüpi Azure RemoteApp saidikogumid.

- Pilveteenuse: asub täielikult Azure. Soovi korral saate salvestada kõik andmed pilves (nii, et vaid saidikogumi) või luua ühendus oma saidikogumi on VNET ja andmed salvestada.   
- Hübriid: sisaldab virtuaalse võrgu kohapealse Accessi - on vaja Azure AD kasutuse ja kohapealse Active Directory keskkonnas.

Ei tea, mida te ei vaja? Vaadake, [mis tüüpi saidikogumi jaoks vaja läheb Azure RemoteApp](remoteapp-collections.md).

Selles õppeteemas tutvustatakse hübriid saidikogumi loomine. On kaheksa toimingut.

1.  Otsustage, milliseid [pilt](remoteapp-imageoptions.md) oma saidikogumi jaoks kasutada. Saate luua kohandatud pildi või kasutada ühte kaasas tellimuse Microsoft pildid.
2. Virtuaalse võrgu häälestamine. Vaadake [VNET kavandamise](remoteapp-planvnet.md) ja [suuruse muutmise](remoteapp-vnetsizing.md) teavet.
2.  Looge kogum.
2.  Domeeni kohaliku oma saidikogumi liituma.
3.  Saate lisada oma saidikogumi malli pilti.
4.  Kataloogisünkroonimise konfigureerimine. Azure RemoteApp nõuab integreerimine Azure Active Directory, kas 1) konfigureerimine Azure Active Directory Sync suvandiga parooli sünkroonimise või (2) konfigureerimine Azure Active Directory sünkroonimine ilma suvandi parooli sünkroonimise, kuid domeen, mis on ühendatud, et AD FS-i abil. Tutvuge [jaoks Active Directory RemoteAppi konfiguratsiooni teave](remoteapp-ad.md).
5.  RemoteApp rakenduste avaldamine.
6.  Kasutajate juurdepääsu konfigureerimine.

**Enne alustamist**

Peate tegema enne selle saidikogumi loomine järgmine:

- Azure RemoteApp [registreeruda](https://azure.microsoft.com/services/remoteapp/) .
- Kasutajakonto loomine Active Directory Azure RemoteApp teenusekonto kasutamiseks. Piira õigusi selle konto nii, et see ainult liituda masinad domeeni.
- Koguge teavet kohapealse võrgu: IP-aadress ja VPN seadme üksikasju.
- Installige [Azure'i PowerShelli](../powershell-install-configure.md) moodul.
- Koguge teavet kasutajatele, mille soovite anda juurdepääsu. Peate Azure Active Directory kasutaja turvasubjektinimi (nt name@contoso.com) iga kasutaja jaoks. Veenduge, et UPN-i vastab vahel Azure AD ja Active Directory.
- Valige oma malli pilti. Pildi Azure RemoteApp Mall sisaldab rakendused ja programmid, mille soovite avaldada oma kasutajate jaoks. Lisateavet leiate [Azure'i RemoteApp pildi suvandid](remoteapp-imageoptions.md) .
- Soovite kasutada Office 365 ProPlusi pilt? Vaadake teave [siin](remoteapp-officesubscription.md).
- [Active Directory RemoteApp jaoks konfigureerida](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Samm 1: Virtuaalse võrgu häälestamine
Saate juurutada hübriid saidikogumi, mis kasutab olemasolevaid Azure virtuaalse võrgu või saate luua uue virtuaalse võrgu. Virtuaalse võrgu võimaldab kasutajate Accessi andmeid teie kohalikus võrgus RemoteApp remote ressurssidest. Mõne Azure virtuaalse võrgu kaudu annab teie saidikogumi otsese võrgupääs teiste Azure'i teenuste ja selle virtuaalse võrgu juurutatud virtuaalmasinates.

Veenduge, et [VNET plaanimine](remoteapp-planvnet.md) ja [VNET suurus](remoteapp-vnetsizing.md) teave läbi, enne kui saate luua oma VNET.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Mõne Azure'i VNET loomine ja oma Active Directory juurutuse liituda

Alustuseks [virtuaalse võrgu](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)loomine. Seda Azure'i portaalis menüü **võrku** . Peate virtuaalse võrgu ühenduse loomine Active Directory juurutuse, mida sünkroonitakse teie Azure Active Directory rentniku.

Lisateavet leiate [Azure'i portaalis virtuaalse võrgu loomine](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Veenduge, et teie virtuaalse on valmis Azure RemoteApp
Enne, kui loote oma saidikogumi, vaatame veenduge, et uue virtuaalse võrgu on valmis. Saate selle kinnitada, tehes järgmist:

1. Saate luua mõne Azure virtuaalse masina vastloodud RemoteApp virtuaalse võrgu alamvõrgu sees.
2. Kaugtöölaua ühenduse, virtuaalse masina kasutamine. (Valige **ühenduse**).
3. Liitumiseks sama Active Directory juurutuse, mida soovite kasutada RemoteApp.

See ei toimi? Oma virtuaalse võrgu ja alamvõrgu on Azure RemoteApp valmis!

Leiate Azure'i virtuaalmasinates loomise ja nende Remote'i töölaua [siin](https://msdn.microsoft.com/library/azure/jj156003.aspx)ühenduse kohta lisateavet.

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Samm 2: Looge on Azure RemoteApp saidikogumi ##



1. [Azure portaali](http://manage.windowsazure.com), avage leht Azure RemoteApp.
2. Klõpsake **uus > Loo VNET**.
3. Sisestage oma saidikogumi nimi.
4. Valige leping, et soovite kasutada – standard- või tavaline.
5. Valige rippmenüüst loend ja seejärel teie alamvõrku oma VNET.
6. Valige oma domeeni ühendamiseks.
5. Klõpsake **saidikogumi loomine RemoteApp**.

Kui teie saidikogumi Azure RemoteApp on loodud, topeltklõpsake selle saidikogumi nime. Mis avab lehe **Kiirkäivituse** – see on koht, kus olete selle saidikogumi konfigureerimine.

Kas midagi valesti minna? Vaadake [hübriidjuurutuse saidikogumi tõrkeotsingu teavet](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Samm 3: Kohaliku Domeen oma saidikogumi linkimine ##


1. Klõpsake lehel **Kiirkäivituse** **ühendus kohaliku Domeen**.
2. Azure RemoteApp teenuse konto lisamine teie kohaliku Active Directory domeeni. Peate selle domeeni nimi, Organisatsiooniüksus, teenuse konto kasutajanimi ja parool.

    See on teil kogutud, kui järgisite juhiseid [konfigureerida Active](remoteapp-ad.md)Directory Azure RemoteApp.


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Samm 4: Link Azure RemoteApp pilt ##

Pildi Azure RemoteApp Mall sisaldab programme, mida soovite ühiskasutusse anda kasutajatele. Saate luua uue [malli pilti](remoteapp-imageoptions.md) või lingi olemasoleva pilt (üks juba imporditud või laaditakse üles Azure RemoteApp). Saate ka linkida ühte Azure RemoteApp [malli pilte](remoteapp-images.md) , mis sisaldavad Office 365 või Office 2013 (prooviversiooni kasutamiseks) programmid.

Uue pildi üleslaadimiseks, peate sisestage nimi ja valige asukoht, pilt. Viisardi järgmisel lehel kuvatakse lugege teemat PowerShelli cmdlet-käskude - Kopeeri ja käivitage järgmised cmdlet-käsud on tõstetud Windows PowerShelli käsuviibas määratud pildi üleslaadimise.

Kui lingite olemasoleva malli pilti, määrake lihtsalt pildi nime, asukoha ja seotud Azure'i tellimus.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Juhis 5: Konfigureerimine Active Directory kataloogi sünkroonimine ##

Azure RemoteApp nõuab integreerimine Azure Active Directory, kas 1) konfigureerimine Azure Active Directory Sync suvandiga parooli sünkroonimise või (2) konfigureerimine Azure Active Directory Sync ilma suvandi parooli sünkroonimise, kuid domeen, mis on ühendatud, et AD FS-i abil.

Tutvuge [AD-ühendus](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – see artikkel aitab teil häälestada kataloogi integreerimise 4 juhiseid.

Teavet ja üksikasjalikud juhised leiate jaotisest [Kataloogisünkroonimise teejuht](http://msdn.microsoft.com//library/azure/hh967642.aspx) .

## <a name="step-6-publish-apps"></a>Samm 6: Rakenduste avaldamine ##

Azure RemoteApp rakendus on rakenduse või programmi, mis teie pakkumiseks. See asub saate üles laadida kogumiseks malli pilti. Kui kasutaja kasutab rakendust, tundub, et käivitada oma kohaliku keskkonnas, kuid see töötab tõesti Azure.

Enne, kui teie kasutajad pääsevad rakendused, peate need avaldada – see võimaldab juurde kasutajate rakenduste kaudu kaugtöölaua klient.

Mitme rakendusi saate avaldada oma saidikogumi. Lehe avaldamine nuppu **Avalda** lisa rakendus. Kas saate avaldada menüü **Start** kaudu pildi malli või määrates tee rakenduse malli pilti. Kui otsustate lisada menüü **Start** , valige programm lisada. Kui valite rakenduse tee, sisestage nimi, rakenduse ja kui see on installitud malli pilti teed.

## <a name="step-7-configure-user-access"></a>Juhis 7: Kasutaja juurdepääsu konfigureerimine ##

Nüüd, kui olete oma saidikogumi loonud, peate lisama kasutajad, mida soovite kasutada oma remote ressursse. Annate juurdepääsu tuleb olemas tellimusega seostatud Active Directory rentniku kasutajate loomiseks kasutatud selle saidikogumi Azure RemoteApp.

1.  Klõpsake lehel Kiirkäivituse **konfigureerimine kasutajate juurdepääsu**.
2.  Sisestage töö konto (Active Directory) või Microsofti konto, mille soovite anda juurdepääsu.

    **Märkmed:**

    Veenduge, et kasutate selle “user@domain.com” vorming.

    Kui te kasutate teenusekomplekti Office 365 ProPlus oma saidikogumi, kasutage Active Directory identiteedid kasutajate jaoks. See aitab kinnitada litsentsimine.


3.  Kui kasutajad on kinnitatud, klõpsake nuppu **Salvesta**.


## <a name="next-steps"></a>Järgmised sammud ##
See on õige – on nüüd loodud ja juurutatud oma Azure RemoteApp hübriid saidikogumi. Järgmiseks on kasutajaid, laadige alla ja installige kaugtöölaua klient. URL-i leiate Azure'i RemoteApp Kiirkäivituse lehel kliendi jaoks. Seejärel on kasutajate logige kliendi ja avaldatud rakenduste juurde.



### <a name="help-us-help-you"></a>Aidake meil aidata teil
Kas teadsite, et lisaks selles artiklis hindamine ja muutes kommentaaride alla all, saate muuta ise artiklit? Midagi puudu? Midagi on valesti? Kas midagi, mis on lihtsalt selgem kirjutada? Liikuge kerides üles ja klõpsake nuppu **Redigeeri github** muudatuste tegemiseks – need tulevad meile läbivaatuseks ja siis, kui me logige välja need, näete oma muudatused ja täiustused siinsamas.
