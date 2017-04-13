<properties
    pageTitle="Azure Active Directory domeeniteenused: Liitumine Windows Server VM hallatava domeeni | Microsoft Azure'i"
    description="Liituda virtuaalse masina Windows Server Azure'i AD domeeniteenused"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Windows Server virtuaalse masina hallatava domeeni liitumine

> [AZURE.SELECTOR]
- [Azure'i klassikaline portaali - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShelli - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

Selles artiklis kirjeldatakse, kuidas liituda töötab Windows Server 2012 R2 Azure'i AD domeeniteenused hallatavate domeenile, Azure klassikaline portaalis.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Samm 1: Loo virtuaalse masina Windows Server
[Loo virtuaalse masina opsüsteemi Windows Azure'i klassikaline portaalis](../virtual-machines/virtual-machines-windows-classic-tutorial.md) õpetuse kirjeldatud juhiseid. On oluline, et tagada, et see äsja loodud virtuaalse masina on liitunud, mille te lubatud Azure AD domeeniteenused virtuaalse samasse võrku. Kiire loomine suvand võimaldavad teil liituda virtuaalse masina virtual võrku. Seetõttu peate: Galerii suvandi abil saate luua virtuaalse masina.

Järgmiste toimingute loomine Windowsi virtuaalse masina liitunud olete lubanud Azure AD domeeniteenused virtuaalse võrku.

1. Azure'i klassikaline portaalis käsuribal akna allosas nuppu **Uus**.

2. Klõpsake jaotises **arvutada**, **virtuaalse masina**, klõpsake käsku **Galeriist**.

3. Esimesel ekraanil võimaldab teil **Vali pilt** oma virtuaalse masina loendist saadaolevad pildid. Valige sobiv pilt.

    ![Valige pilt](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Teisel kuval saate valida arvuti nimi, suurus ja haldus kasutajanime ja parooli. Kasutage taseme- ja oma rakenduse või töökoormus käitamiseks nõutavad suurus. Saate valida siin kasutajanimi on kohaliku administraatori kasutaja arvutis. Sisestage siia domeeni kasutajakonto kasutajanimi ja parool.

    ![Virtuaalse masina konfigureerimine](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Kolmas ekraan laseb teil konfigureerida ressursid võrgunduse ja salvestusruumi kättesaadavus. Veenduge, et olete valinud virtuaalse võrgu, mille te lubatud Azure AD domeeniteenused **Piirkond/osaleja rühma/Virtual Network** rippmenüüst. Määrake vastavalt kasutatavale virtuaalse masina **Pilvepõhise teenuse DNS-i nimi** .

    ![Valige virtuaalse võrgu jaoks virtuaalse masina](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Veenduge, et liituda virtuaalse masina, mille olete lubatud Azure AD domeeniteenused virtuaalse samasse võrku. Selle tulemusena virtuaalse masina saate teemast domeeni ja toimingute jaoks nagu liitumist domeeni. Kui valite teise virtuaalse võrgu virtuaalse masina loomiseks, võrguga selle virtuaalse võrgu virtuaalse olete lubanud Azure AD domeeniteenused.

6. Neljas Kuva võimaldab teil VM Agent installida ja häälestada saadaval laiendid.

    ![Valmis](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Pärast virtuaalse masina on loodud, on loetletud klassikaline portaali uue virtuaalse masina **Virtuaalmasinates** sõlme all. Nii virtuaalse masina ja pilveteenuses käivitatakse automaatselt ja nende olek on loetletud **töötab**.

    ![Virtuaalse masina on tööks](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Samm 2: Ühenduse loomine kohaliku administraatorikonto Windows Server virtuaalse masina
Nüüd saame ühenduse äsja loodud Windows Server virtuaalse masina, domeeni ühendamiseks. Kohaliku administraatori identimisteabe määratud virtuaalse masina ühenduse loomisel kasutada.

Järgmiste toimingute ühenduse, virtuaalse masina.

1. Liikuge **Virtuaalmasinates** sõlm klassikaline portaalis. Valige virtuaalse masina loodud samm 1 ja käsuribal akna allosas käsku **Ühenda** .

    ![Ühenduse loomine Windows virtuaalse masina](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klassikaline portaali küsib, kas soovite avada või salvestada faili laiendiga "RDP", virtuaalse masina ühenduse loomiseks kasutatava. Kui see allalaadimine on lõppenud, avage fail, klõpsake nuppu.

3. Login kuvatakse vastav viip, sisestage oma **kohaliku administraatori identimisteave**, mis määratud virtuaalse masina loomisel. Näiteks kasutasime 'localhost\mahesh' selles näites.

Selles etapis teil peaks sisse logima vastloodud Windowsi virtuaalse masina kohaliku administraatori mandaadiga. Järgmiseks on liituda virtuaalse masina domeeni.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Samm 3: Liitu AAD-DS hallatava domeeni virtuaalse masina Windows Server
Järgmiste toimingute AAD-DS hallatava domeeni Windows Server virtuaalse masina ühendamiseks.

1. Ühenduse loomine Windows Server, nagu on näidatud etapp 2. Avage avakuva, **Server Manager**.

2. Klõpsake **Kohaliku Server** Server Manager akna vasakul paanil.

    ![Käivitage Server Manager virtual arvutisse](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Klõpsake jaotises **Atribuudid** **on töörühm** . Klõpsake lehe **Süsteemiatribuudid** atribuudi **muutmine** domeeniga liituda.

    ![Süsteemi lehe atribuudid](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Määrake Azure AD domeeniteenuste domeeni nime hallatavate domeeni tekstiväli **Domeen** ja klõpsake nuppu **OK**.

    ![Määrake domeeni ühendatakse](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Teil palutakse sisestama oma mandaadi domeeniga liituda. Veenduge, et selle saate **määrata AAD näiteks Põhiliselt administraatorid kuuluvate kasutaja mandaat** rühma. Selle rühma liikmetel on liituda masinad hallatava domeeni õigused.

    ![Määrake mandaat Domeeniga liitumine](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Saate määrata mandaadi ühel järgmisel viisil:

    - UPN-vormingus: määrake UPN-i järelliite kasutajakonto, nagu on konfigureeritud Azure AD. Selles näites on UPN-i järelliite "bob" kasutaja 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName vorming: saate määrata konto nime SAMAccountName vormingus. Selles näites oleks vaja kasutaja "bob" sisestage "CONTOSO100\bob".

        > [AZURE.NOTE] **Soovitame kasutada UPN-vormingus mandaadi määramiseks.** Funktsiooni SAMAccountName võib olla automaatselt loodud, kui kasutaja UPN-i eesliide on liiga pikk (nt "joereallylongnameuser"). Kui mitu kasutajat on sama UPN-i eesliide (nt bob) oma Azure AD rentniku, võib nende SAMAccountName vorming automaatselt loodud teenus. Sellisel juhul saab UPN-vormingus usaldusväärselt domeeni sisse logida.

7. Pärast domeeni ühendus on edukalt, kuvatakse teid domeeni meie järgmine teade. Taaskäivitage virtuaalse masina domeeni liitumine lõpuleviimiseks.

    ![Tere tulemast domeeni](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Domeeni ühenduse tõrkeotsing
### <a name="connectivity-issues"></a>Ühenduvusprobleemide
Kui virtuaalse masina ei leia domeeni, lugege järgmisi tõrkeotsingujuhiseid:

- Veenduge, et virtuaalne seade on ühendatud sama virtuaalse võrku, kui olete lubanud domeeni teenused. Kui ei, virtuaalse masina ei saa luua ühendust domeeni ja seetõttu ei saa liituda domeeni.

- Kui virtual seade on ühendatud muu virtuaalse võrguga, veenduge, et selle virtuaalse võrgu on ühendatud, mille olete lubatud domeeniteenused virtuaalse võrku.

- Proovige ping domeeni kasutavad seda domeeninime hallatava domeeni (nt ping contoso100.com). Kui te ei saa seda teha, proovige ping IP-aadressid, kui olete lubanud Azure AD domeeniteenused kuvatakse lehel domeeni (nt "ping 10.0.0.4"). Kui te pole võimalik ping IP-aadress, kuid mitte domeeni, DNS-i võib valesti konfigureeritud. Pole olete konfigureerinud IP-aadressid domeeni DNS-i serverid virtuaalse võrgu jaoks.

- Proovige õhetus DNS-i Resolveri vahemälu virtuaalse masina ("ipconfig/flushdns").

Kui kuvatakse dialoogiboks, mis küsib mandaati liituda domeeni, ei ole ühenduvusprobleemide.


### <a name="credentials-related-issues"></a>Mandaadi seotud probleemid
Vaadake järgmist, kui teil on probleeme identimisteabe ja ei saa liituda domeeni.

- Proovige kasutada UPN-vormingus mandaadi määramiseks. Teie konto jaoks SAMAccountName ei pruugi automaatselt loodud, kui on mitu kasutajat samal UPN-i eesliide oma rentniku või kui teie UPN-i eesliide on liiga pikk. Seetõttu SAMAccountName vorming teie konto jaoks võib olla erinev mida oodata või kasutage oma kohapealse domeeni.

- Proovige kasutada kasutajakonto, mis 'AAD näiteks Põhiliselt administraatorid' rühma masinad ühendamiseks hallatava domeeni identimisteabe.

- Veenduge, et teil on [lubatud parooli sünkroonimise](active-directory-ds-getting-started-password-sync.md) vastavalt alustamine juhend kirjeldatud juhised.

- Veenduge, et kasutate selle kasutaja UPN-i konfigureeritud Azure AD (nt 'bob@domainservicespreview.onmicrosoft.com') sisse logida.

- Veenduge, et olete oodanud piisavalt kaua parooli sünkroonimise täielik juhend alustamine.


## <a name="related-content"></a>Seotud teemad

- [Domeeniteenused Azure'i AD - kasutusjuhend](./active-directory-ds-getting-started.md)

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](./active-directory-ds-admin-guide-administer-domain.md)
