<properties
    pageTitle="Microsoft Windows Hello ettevõtetele luba ettevõtte | Microsoft Azure'i"
    description="Juurutamise juhiseid, mis võimaldavad Microsoft Passporti teie asutuses."
    services="active-directory"
    documentationCenter=""
    keywords="Microsoft Passporti, Microsoft Windows Hello Business juurutamiseks konfigureerimine"
    authors="markusvi"
    manager="femila"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="markvi"/>


# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Microsoft Windows Hello ettevõtetele lubada teie asutuses

Pärast [ühenduse loomine Windows 10 domeeni ühendatud seadmete Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), tehke Microsoft Windows Hello ettevõtetele lubamiseks ettevõtte järgmist.

1. System Center Configuration Manager juurutamine  

2. Poliitika sätete konfigureerimine

3. Serdi profiili konfigureerimine  




## <a name="deploy-system-center-configuration-manager"></a>System Center Configuration Manager juurutamine 

Kasutaja serdid põhjal Windows Hello Business võtmed juurutamiseks peate järgmist:

- **System Center Configuration Manager praeguse haru** - on vaja installida versiooni 1606 või parem. Lisateavet leiate teemast [dokumentatsiooni System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) ja [System Center Configuration Manager meeskonna ajaveeb](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
- **Avaliku võtme infrastruktuuri (PKI)** – Microsoft Windows Hello for Business abil kasutaja serdid, peab teil olema PKI kohas. Kui teil pole või te ei soovi kasutada kasutaja serdid, saate uute domeenikontrolleri, mis on Windows Server 2016 koostamine 10551 (või parem) installitud juurutada. Järgige neid juhiseid installimiseks [olemasoleva domeeni domeenikontrolleri koopia](https://technet.microsoft.com/library/jj574134.aspx) või [uue Active Directory kogumis, kui loote uue keskkonna](https://technet.microsoft.com/library/jj574166)installimiseks. (Selle ISOs on [Signiant Media](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true)Exchange'i allalaadimiseks saadaval).


## <a name="configure-policy-settings"></a>Poliitika sätete konfigureerimine

Konfigureerimiseks on Microsoft Windows Hello Business poliitikasätted, on teil kaks võimalust:

- Rühmapoliitika Active Directorys 
- System Center Configuration Manager 


Rühmapoliitika Active Directory on soovitatav meetod konfigureerida Microsoft Windows Hello Business rühmapoliitika sätted. 



Abil System Center Configuration Manager on eelistatud meetod, kui te seda kasutada ka juurutamine serdid. Selle stsenaariumi:

- Tagab ühilduvuse uuem juurutamise stsenaariumid
- Nõuab kliendi poolel Windows 10 versiooni 1607 või parem.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Microsoft Windows Hello ettevõtetele konfigureerimine Active Directory rühmapoliitika kaudu

 
**Juhised**:

1.  Avage Server Manager ja liikuge **Tööriistad** > **Rühmapoliitika haldus**.
2.  Kaudu rühmapoliitika, liikuge domeeni sõlm, mis vastab, kus soovite lubada Azure AD liituda domeeni.
3.  Paremklõpsake **Rühmapoliitika objektid**ja valige **Uus**. Pange oma rühmapoliitika objekti nimi, näiteks lubamine Windows Hello ettevõtetele. Klõpsake nuppu **OK**.
4.  Paremklõpsake oma uue rühmapoliitika objekti ja seejärel valige **Redigeeri**.
5.  Liikuge **Arvuti konfiguratsioon** > **poliitikate** > **haldus mallide** > **Windowsi komponentide** > **Windows Hello ettevõtetele**.
6.  Paremklõpsake **Lubamine Windows Hello ettevõtetele**ja seejärel valige **Redigeeri**.
7.  Valige nupp **lubatud** , ja seejärel klõpsake nuppu **Rakenda**. Klõpsake nuppu **OK**.
8.  Nüüd saate linkida rühmapoliitika objekti soovitud kohta. Link sellele seadnud kõigi oma asutuse domeeni ühendatud Windows 10 seadmes lubamiseks domeeni rühmapoliitika. Näiteks:
 - Teatud organisatsiooniüksusega (OU) Active Directory, kuhu paigutatakse Windows 10 domeeni ühendatud arvutites
 - Turvalisuse rühm, mis sisaldab Windows 10 domeeni ühendatud arvutites automaatselt registreeritakse Azure AD


### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Windows Hello abil System Center Configuration Manager konfigureerimine


**Juhised**:


1. Avage **System Center Configuration Manager**, ja seejärel liikuge **varad ja nõuetele vastavus > vastavuse sätted > ettevõtte ressursside Accessi > Windows Hello Business profiilide**.

    ![Windowsi Tere tulemast ärirakenduse konfigureerimine](./media/active-directory-azureadjoin-passport-deployment/01.png)


2. Ülaosas tööriistaribal nuppu **Loomine Windows Hello ettevõtetele profiili**.

    ![Windowsi Tere tulemast ärirakenduse konfigureerimine](./media/active-directory-azureadjoin-passport-deployment/02.png)

2. **Üldine** dialoogiboksis tehke järgmist.

    ![Windowsi Tere tulemast ärirakenduse konfigureerimine](./media/active-directory-azureadjoin-passport-deployment/03.png)

    lisamine. Tippige väljale **nimi** , näiteks **WHfB minu profiil**profiili nimi.

    b. Klõpsake nuppu **edasi**.


2. Valige dialoogiboksis **Toetatud platvormid** platvormid, mis on ette valmistatud selle Windows Hello business profiili ja seejärel klõpsake nuppu **edasi**.

    ![Windowsi Tere tulemast ärirakenduse konfigureerimine](./media/active-directory-azureadjoin-passport-deployment/04.png)


2. Klõpsake dialoogiboksis **sätted** tehke järgmist.

    ![Windowsi Tere tulemast ärirakenduse konfigureerimine](./media/active-directory-azureadjoin-passport-deployment/05.png)

    lisamine. Kui **Konfigureerimine Windows Hello ettevõtetele**, valige **lubatud**.

    b. Kui **kasutada usaldusväärse platvormi mooduli (TPM)**, valige **nõutav**. 

    c. **Autentimise meetodit**, valige **sert-põhine**.

    d. Klõpsake nuppu **edasi**.



2. Klõpsake dialoogiboksis **Kokkuvõte** klõpsake nuppu **edasi**.

2. Klõpsake dialoogiboksis **lõpetamist** klõpsake nuppu **Sule**.


2. Klõpsake tööriistaribal ülaosas **Deploy**.

    ![Windowsi Tere tulemast ärirakenduse konfigureerimine](./media/active-directory-azureadjoin-passport-deployment/06.png)



## <a name="configure-the-certificate-profile"></a>Serdi profiili konfigureerimine 

Kui kasutate serdi autentimise kohapealse autentimine, peate serdi profiili juurutada ja konfigureerimiseks. Selle toimingu kasutamiseks peate mõne NDES serveri ja serdi registreerimise punkti saidi rolli System Center Configuration Manager. Lisateavet leiate teemast [eeltingimuste serdi profiilid Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).

1. Avage **System Center Configuration Manager**, ja seejärel liikuge **varad ja nõuetele vastavus > vastavuse sätted > ettevõtte ressursside Accessi > serdi profiilid**.


2. Valige mall, mis sisaldab kiipkaardiga sisselogimine laiendatud võtme kasutamine (EKU).

Serdi profiili lehel **SCEP registreerimine** peate valima **installi, et pass tööl, muidu ei suuda** **Klahvi salvestusruumi pakkuja**.



## <a name="next-steps"></a>Järgmised sammud
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Parooli Microsoft Passporti kaudu ilma autentimine](active-directory-azureadjoin-passport.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
