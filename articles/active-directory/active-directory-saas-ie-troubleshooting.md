<properties
    pageTitle="Internet Exploreri juurdepääsu juhtpaneeli laiend tõrkeotsing | Microsoft Azure'i"
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

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Internet Exploreri juurdepääsu juhtpaneeli laiend tõrkeotsing

See artikkel aitab teil tõrkeotsing järgmist:

- Te ei saa juurdepääsu rakenduste minu rakendused portaali kaudu Internet Exploreri kasutamise ajal.
- Kuvatakse teade "Tarkvara installimine", isegi siis, kui teil on juba installitud tarkvara.

Kui olete administraator, vt ka: [juurutamise Internet Exploreri rühmapoliitika kaudu juurdepääsu juhtpaneeli laiend](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Käivitage diagnostika tööriist

Saate diagnoosimine installiprobleeme Accessi juhtpaneeli laiend, laadides ja kasutada paneeli diagnostika tööriista:

1. [Diagnostika tööriista allalaadimiseks klõpsake siin.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Avage fail ja klõpsake nuppu **ekstrakti kõik** .

    ![Vajutage Väljavõte kõik](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Vajutage jätkamiseks nuppu **ekstrakti** .

    ![Vajutage väljavõte](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Käivitage tööriist, paremklõpsake nimega **AccessPanelExtensionDiagnosticTool**faili ja seejärel valige **Ava > Microsoft Windowsi vastavalt skriptihosti**.

    ![Ava > vastavalt Microsoft Windowsi skriptihosti](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Seejärel kuvatakse järgmine diagnostika aken, kus kirjeldatakse, mis võib olla valesti installi.

    ![Valimi diagnostika akna](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Valige "**Jah**" lasta programmi, mis on leitud probleemide lahendamiseks.

7. Muudatuste salvestamiseks sulgege kõik Internet Exploreri aknas ja seejärel avage Internet Explorer.<br />Kui te ikka ei pääse oma rakendused, proovige järgmisi juhiseid.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Kontrollige, et Access juhtpaneeli laiend on lubatud

Kinnitamaks, et Internet Exploreris on lubatud juurdepääs juhtpaneeli laiend:

1. Klõpsake Internet Exploreri akna paremas ülanurgas **hammasrattaikoon** . Valige **Interneti-suvandid**.<br />(Vanemates versioonides Internet Exploreri leiate selle jaotise **Tööriistad > Interneti-suvandid**.

    ![Valige Tööriistad > Interneti-suvandid](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Klõpsake vahekaarti **programmid** ja seejärel klõpsake nuppu **Halda lisandmooduleid** .

    ![Klõpsake Lisandmoodulite haldamine](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. Selle dialoogiboksi valige **Accessi juhtpaneeli laiend** ja seejärel klõpsake nuppu **Luba** .

    ![Klõpsake nuppu Luba](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Muudatuste salvestamiseks sulgege kõik Internet Exploreri aknas ja seejärel avage Internet Explorer.

##<a name="enable-extensions-for-inprivate-browsing"></a>InPrivate-sirvimise laiendid lubamine

Kui kasutate InPrivate-sirvimise režiimi:

1. Klõpsake Internet Exploreri akna paremas ülanurgas **hammasrattaikoon** . Valige **Interneti-suvandid**.<br />(Vanemates versioonides Internet Exploreri leiate selle jaotise **Tööriistad > Interneti-suvandid**.

    ![Valimi diagnostika akna](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Minge **Privaatsus, siis **tühjendage** ruut **Keela tööriistaribad ja laiendid InPrivate-sirvimise käivitamisel** **</p>

    ![Tühjendage ruut Keela tööriistaribad ja laiendid InPrivate-sirvimise käivitamisel](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Muudatuste salvestamiseks sulgege kõik Internet Exploreri aknas ja seejärel avage Internet Explorer.

##<a name="uninstall-the-access-panel-extension"></a>Desinstallige Accessi juhtpaneeli laiend

Arvutist juurdepääs juhtpaneeli laiend desinstallimiseks tehke järgmist.

1. Klaviatuuril vajutage **Windowsi klahvi** menüü Start avamiseks. Kui menüü on avatud, saate tippida midagi ei Otsi. Tippige "Juhtpaneel" ja seejärel avage **Juhtpaneelil** otsingutulemite kuvamisel.

    ![Juhtpaneeli otsimine](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. Muutke Juhtpaneel paremas ülanurgas suvandi **Kuvamisalus** **suured**ikoonid. Seejärel otsige üles ja klõpsake nuppu **programmid ja funktsioonid** .

    ![Chang vaate kuvamiseks Suured ikoonid](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Valige loendist **Accessi juhtpaneeli laiend**, ja klõpsake nuppu **desinstalli** .

    ![Klõpsake nuppu desinstalli.](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Seejärel proovige installida pikendamise uuesti, et näha, kas probleem on lahendatud.

Kui teil tekib probleeme desinstallimine laiendamine, saate ka selle [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tööriista abil eemaldada.

## <a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Juurutamise Internet Exploreri rühmapoliitika kaudu juurdepääsu juhtpaneeli laiend](active-directory-saas-ie-group-policy.md)
