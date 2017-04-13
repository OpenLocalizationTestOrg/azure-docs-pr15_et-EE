<properties
    pageTitle="Internet Exploreri rühmapoliitika kaudu juurdepääsu juhtpaneeli laiend juurutamise | Microsoft Azure'i"
    description="Kuidas kasutada rühmapoliitika juurutada Internet Exploreri lisandmooduli portaali minu rakendused."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Juurutamise Internet Exploreri rühmapoliitika kaudu juurdepääsu juhtpaneeli laiend

Selle õpetuse näitab, kuidas eemalt installimiseks Accessi juhtpaneeli laiend Internet Exploreri teie kasutajate arvutites rühmapoliitika abil. Sellele laiendile on vaja Internet Exploreri kasutajad, kellel on vaja rakendusi, mis on konfigureeritud, [parooli-põhiste ühekordse sisselogimise](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)abil sisse logida.

Soovitatav on, et administraatorid automatiseerida sellele laiendile kasutuselevõtt. Muul juhul kasutajaid on alla laadima ja installima laiendamine ise, mis on palju kasutaja vigu ja nõuab administraatoriõigusi. Selle õpetuse hõlmab üks meetod automatiseerimine tarkvara juurutuste rühmapoliitika abil. [Lisateavet rühmapoliitika.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Accessi juhtpaneeli laiend on ka [Chrome'i](https://go.microsoft.com/fwLink/?LinkID=311859) ja [Firefoxi](https://go.microsoft.com/fwLink/?LinkID=626998)neist vaja administraatoriõigusi installimiseks saadaval.

##<a name="prerequisites"></a>Eeltingimused

- Olete seadistanud [Active Directory domeeniteenused](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)ja teie kasutajate masinad liitunud, saate oma domeeni.
- Peab olema "Sätete redigeerimine" õiguste redigeerimiseks rühmapoliitika objektid (GPO). Vaikimisi on järgmine turberühmade liikmed see õigus: administraatorid, ettevõtte administraatorid ja rühma poliitika looja omanikud. [Lisateave.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Samm 1: Loo jaotuse punkt

Kõigepealt tuleb asetada installijapaketi võrgukoht, millele pääseb juurde kõik masinad, mida soovite installida kaugühenduse teel laiendamine. Selleks tehke järgmist.

1. Serveri administraatorina sisse logida

2. **Server Manager** aknas, avage **failide ja salvestusruumi teenused**.

    ![Failide avamine ja salvestusruumi teenused](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Minge menüüsse **ühtlasi** . Seejärel klõpsake käsku **toimingud** > **Uue ühiskasutus …**

    ![Failide avamine ja salvestusruumi teenused](./media/active-directory-saas-ie-group-policy/shares.png)

4. **Uue ühiskasutus viisardi** lõpuleviimine ja tagamaks, et see pääseb teie kasutajate masinad õiguste määramine. [Lugege lisateavet ühtlasi.](https://technet.microsoft.com/library/cc753175.aspx)

5. Järgmised Microsoft Windows Installeri pakett (MSI-faili) allalaadimine: [Accessi paani Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Kopeerige installijapaketi ühiskasutus soovitud asukohta.

    ![Kopeeri msi-faili teile olete ühiskasutus.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Veenduge, et teie klientarvutite on võimalik juurde pääseda installijapaketi ühiskasutus. 

##<a name="step-2-create-the-group-policy-object"></a>Samm 2: Looge rühmapoliitika objekti

1. Logige sisse oma Active Directory domeeniteenused (AD DS) installi majutavas serveris.

2. Server Manager valige **Tööriistad** > **Rühmapoliitika haldus**.

    ![Valige Tööriistad > rühma poliitika haldamine](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. **Rühmapoliitika** akna vasakul paanil saate vaadata oma ettevõtte üksus (OU) hierarhia ja kindlaks teha, millised ulatus soovite rakendada rühmapoliitika. Näiteks võib juhtuda, väike OU juurutada mõne kasutaja kontrollimise valimiseks või valite ülataseme OU kogu ettevõttes juurutada.

    > [AZURE.NOTE] Kui soovite luua või redigeerida teie asutuse üksused (organisatsiooniüksused), liikumine tagasi-Server Manager ja valige **Tööriistad** > **Active Directory kasutajad ja arvutid**.

4. Kui olete valinud Organisatsiooniüksusega, paremklõpsake seda ja valige **selle domeeni rühmapoliitika objekti loomine ja linkida selle siia...**

    ![Looge uus GPO](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. **Uus GPO** vastava viiba kuvamisel tippige nimi uue rühmapoliitika objekti.

    ![Uut GPO nimi](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Paremklõpsake vastloodud rühmapoliitika objekti ja valige **Redigeeri**.

    ![Uut GPO redigeerimine](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Samm 3: Määramine installipaketi

1. Määratlemine, kas soovite juurutada **Arvuti konfiguratsioon** või **Kasutaja konfiguratsioon**laiendamine. Kui kasutate [Arvuti konfiguratsioon](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), installitakse laiendamine sõltumata sellest, millised kasutajad logige arvutisse. Teisalt, kasutajate [Kasutaja konfiguratsioon](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), mis on installitud neid sõltumata sellest, millises arvutis logib laiendamine.

2. **Rühmapoliitika halduskonsoolis** akna vasakul paanil minge ühte järgmist kausta tee, sõltuvalt sellest, millist tüüpi konfiguratsiooni valitud.
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Paremklõpsake **tarkvara installimine**ja seejärel valige **Uus** > **paketi...**

    ![Uue tarkvara installimise paketi loomine](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Valige ühiskasutusse antud kausta, mis sisaldab Installeri pakett [Samm 1: luua jaotuse punkt](#step-1-create-the-distribution-point), valige msi-faili ja klõpsake nuppu **Ava**.

    > [AZURE.IMPORTANT] Kui ühiskasutus asub see sama server, veenduge, et kasutate selle MSI võrgu faili tee, mitte kohaliku faili tee kaudu.

    ![Valige installipaketi ühiskausta.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. **Tarkvara juurutamine** kui küsitakse, valige oma juurutamise meetodi **määratud** . Klõpsake nuppu **OK**.

    ![Valige määratud ja seejärel klõpsake nuppu OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

OU, valitud on nüüd juurutatud laiendamine. [Lisateave tarkvara install.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Samm 4: Automaatne lubamine Internet Exploreri laiend 

Lisaks töötab installiprogrammi, iga Internet Exploreri laiend peab olema selgesõnaliselt lubatud enne seda saab kasutada. Allpool Luba juurdepääs juhtpaneeli laiend rühmapoliitika abil tehke järgmist.

1. Avage aknas **Rühmapoliitika halduskonsoolis** kas järgmine teed, olenevalt sellest, millist tüüpi konfiguratsiooni valisite [Samm 3: määramine installipaketi](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Paremklõpsake **Lisandmoodulite loend**ja valige **Redigeeri**.
    ![Lisandmooduli loendit redigeerida.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. Valige aknas **Lisandmoodulite loendis** **lubatud**. Jaotises **Suvandid** valige **Kuva …**.

    ![Klõpsake nuppu Luba ja seejärel klõpsake käsku Kuva...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. **Kuva sisu** aknas, tehke järgmist.

    1. Esimese veeru (väljale **Nimi väärtus** ), kopeerige ja kleepige järgmised klassi ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Teise veeru (välja **väärtus** ), tippige järgmine väärtus:`1`

    3. Klõpsake nuppu **OK** , **Sisu kuvamine** akna sulgemiseks.

    ![Täitke kohal määratletud väärtused.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Klõpsake nuppu **OK** , et rakendada soovitud muudatused ja sulgege aken **Lisandmoodulite loend** .

Pikendamise peaks nüüd olema lubatud valitud OU masinad. [Lisateavet rühmapoliitika abil saate lubada või keelata Internet Exploreri lisandmoodulid.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Juhis 5 (valikuline): "Mäleta parooli" Küsi keelamine

Kui kasutajad sisse logida veebisaitide abil juurdepääsu juhtpaneeli laiend, Internet Explorer võidakse kuvada järgmine viip küsib "Kas soovite parooli salvestada?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Kui soovite takistada oma kasutajate selline küsimus kuvatakse, siis järgige allpool automaatteksti kaudu parooli meelespidamine tõkestamiseks:

1. Avage aknas **Rühmapoliitika halduskonsoolis** tee, mis on loetletud allpool. Võtke arvesse, et selle konfiguratsioon sätte ainult jaotises **Kasutaja**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Otsige üles säte nimega **kasutajanimed ja paroolid vormidel automaatteksti funktsiooni sisse lülitada**.

    > [AZURE.NOTE] Varasemate versioonide Active Directory võib loendis selle sätte **Luba, parool salvestada automaatteksti**nimi. Selle sätte konfiguratsioon erineb selles õpetuses kirjeldatud säte.

    ![Ärge unustage selle kasutaja sätted jaotises ilme.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Ülaltoodud säte paremklõps ja valige **Redigeeri**.

4. Pealkirjaga **kasutajanimed ja paroolid vormidel automaatteksti funktsiooni sisselülitamiseks**klõpsake aknas valige **keelatud**.

    ![Valige keelamine](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Klõpsake nuppu **OK** , et need muudatused rakendada ja sulgege aken.

Kasutaja saab enam identimisteabe või automaatteksti funktsiooni kasutamine juurdepääsu varem talletatud mandaatide talletamiseks. Selle poliitika siiski lubada kasutajatel automaatteksti kasutada muud tüüpi vormivälju otsingu väljadega.

> [AZURE.WARNING] Kui see poliitika on lubatud, kui kasutajad valinud mõne mandaat, see poliitika on *pole* tühjendage identimisteavet, mida juba salvestatud.

##<a name="step-6-testing-the-deployment"></a>Samm 6: Testimine juurutamise

Allpool laiend juurutamise õnnestus kinnitamiseks tehke järgmist.

1. Kui olete juurutanud abil **Arvuti konfiguratsioon**, logige sisse kliendi seadme, mis kuulub valitud OU [Samm 2: rühmapoliitika objekti loomine](#step-2-create-the-group-policy-object). Kui olete juurutanud abil **Kasutaja konfiguratsioon**, veenduge, et kasutaja, kes selle OU kuulub sisse logida.

2. Võib kuluda paar Logi lisandmoodulid rühmapoliitika muutub täielikult värskendada selle seadme abil. Jõustamine värskendus, avage **käsuviiba** aken ja käivitage järgmine käsk:`gpupdate /force`

3. Peate installi toimub arvuti taaskäivitama. Alglaadimisel võib kuluda märkimisväärselt rohkem aega, kui tavaline ajal laiendamine installib.

4. Pärast taaskäivitada, avage **Internet Explorer**. Klõpsake akna paremas ülanurgas klõpsake **Tööriistad** (hammasratta ikoon) ja seejärel valige **Halda lisandmooduleid**.

    ![Valige Tööriistad > Halda lisandmooduleid](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. Kontrollige aknas **Lisandmoodulite haldamine** **Accessi juhtpaneeli laiend** installitud ja kas selle **olek** on seatud **lubatud**.

    ![Veenduge, et Access juhtpaneeli laiend installitud ja lubatud.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Internet Exploreri juurdepääsu juhtpaneeli laiend tõrkeotsing](active-directory-saas-ie-troubleshooting.md)
