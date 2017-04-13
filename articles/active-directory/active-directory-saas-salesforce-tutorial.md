<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Salesforce'i | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Salesforce'i abil!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Õpetus: Kuidas Salesforce'i integreerimine Azure Active Directory

Selle õpetuse näitab teile, kuidas oma Azure Active Directory Salesforce'i keskkonna loomiseks. Saate teada, ühekordse sisselogimise Salesforce'i konfigureerimine, lubamise automatiseeritud kasutaja ettevalmistamise kohta ja kuidas määrata kasutajatele juurdepääsu Salesforce'i.

##<a name="prerequisites"></a>Eeltingimused

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)kaudu juurdepääsu Azure Active Directory, peab olema lubatud Azure tellimust.

2. [Salesforce.com](https://www.salesforce.com/)peab teil olema kehtiv rentnik.

> [AZURE.IMPORTANT] Kui kasutate Salesforce.com **prooviversiooni** konto, siis te ei saa konfigureerida automatiseeritud kasutaja ettevalmistamise. Prooviversiooni kontod ei ole vaja API juurdepääs lubatud seni, kuni on ostetud.
> 
> Saate avada see piirang selles õpetuses lõpuleviimiseks [tasuta arendaja konto](https://developer.salesforce.com/signup) abil.

Kui kasutate Salesforce'i Liivakasti keskkonnas, leiate [Salesforce'i Liivakasti integreerimine õpetuse](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Video õpetused

Võib-olla järgige selles õpetuses alltoodud videote abil.

**Video õpetuse esimene osa: ühekordse sisselogimise lubamine**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Video õpetuse osa: Kuidas automatiseerida kasutajate ettevalmistamine**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Samm 1: Kataloogi Salesforce'i lisamine

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com), klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Valige vasakpoolsel navigeerimispaanil Active Directory.][0]

2. Valige loendist **Directory** kataloogi, mida soovite lisada Salesforce'i abil.

3. Klõpsake ülemises menüüs **rakendused** .

    ![Klõpsake nuppu rakendused.][1]

4. Klõpsake lehe allosas **Lisa** .

    ![Lisamiseks klõpsake nuppu Lisa uus rakendus.][2]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Klõpsake käsku Lisa rakendus galeriist.][3]

6. Tippige **väljale Otsing**, **Salesforce**. Seejärel valige **Salesforce'i** tulemustest ja **lõpuleviimine** rakenduse lisamiseks klõpsake nuppu.

    ![Lisage Salesforce'i.][4]

7. Nüüd peaks nähtaval Kiirkäivituse lehe Salesforce'i jaoks:

    ![Salesforce'i 's Kiirkäivituse lehe Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Samm 2: Lubada ühekordse sisselogimise

1. Enne kui saate konfigureerida ühekordse sisselogimise, peate häälestamine ja juurutada kohandatud domeeni Salesforce'i keskkonnas. Juhised selle kohta, kuidas seda teha, leiate teemast [Domeeninime häälestamiseks](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Salesforce'i 's Kiirkäivituse lehe Azure AD nuppu **Konfigureeri ühekordse sisselogimise** .

    ![Konfigureeri ühekordse sisselogimise nupp][6]

3. Avab dialoogiboksi ja teile kuvatakse kuva, mis küsib "Kuidas soovite kasutajate Salesforce'i logida?" Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Valige Azure AD ühekordse sisselogimise][7]

    > [AZURE.NOTE] Lisateavet erinevate ühekordse sisselogimise võimaluste kohta, [klõpsake siin](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. Klõpsake lehel **Rakenduse sätete konfigureerimine** täitmine **Logi sisse URL-i** , tippides Salesforce'i domeeni URL järgmises vormingus:
 - Ettevõtte konto:`https://<domain>.my.salesforce.com`
 - Arendaja konto:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Tippige oma Logi sisse URL][8]

5. Lehel **konfigureerimine ühekordse sisselogimise Salesforce'i veebisaidil** klõpsake **serdi allalaadimine**ja seejärel salvestage serdi fail teie arvutile.

    ![Laadige alla serdi][9]

6. Avage brauser ja logige sisse oma Salesforce'i administraatorikontoga uuel vahekaardil.

7. Klõpsake navigeerimispaanil **administraatori** **Turbemeetmed** seotud jaotise laiendamiseks. Seejärel klõpsake **Ühekordse sisselogimise sätteid**.

    ![Klõpsake ühekordse sisselogimise sätted jaotises Turve juhtelemendid][10]

8. **Ühekordse sisselogimise sätete** lehel klõpsake nuppu **Redigeeri** .

    ![Klõpsake nuppu Redigeeri][11]

    > [AZURE.NOTE] Kui te ei saa lubada ühekordse sisselogimise sätete Salesforce'i konto, peate Salesforce'i 's tugiteenuste selleks, et funktsioon on sisse lülitatud, saate.

9. Valige **SAML lubatud**ja klõpsake siis nuppu **Salvesta**.

    ![Valige SAML lubatud][12]

10. SAML ühekordse sisselogimise sätete konfigureerimiseks, klõpsake nuppu **Uus**.

    ![Valige SAML lubatud][13]

11. Klõpsake lehel **SAML ühekordse sisselogimise sätete redigeerimine** teha järgmised:

    ![Veenduge, kuvatakse kuvatõmmis][14]

 - Tippige **nimi** väljale sõbralik nimi selle konfiguratsiooni puhul. Anda **nimi** automaatselt asustada **API nimi** rühmitusaluse väärtus.

 - Azure AD, **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **väljaandja** välja Salesforce'i.

 - **Üksuse Id tekstivälja**, tippige oma Salesforce'i domeeninime, kasutades järgmist mustrit:
     - Ettevõtte konto:`https://<domain>.my.salesforce.com`
     - Arendaja konto:`https://<domain>-dev-ed.my.salesforce.com`

 - Klõpsake nuppu **Sirvi** või **Faili valimine** avage dialoogiboksis **Vali fail üles** , valige oma Salesforce'i sert ja seejärel klõpsake nuppu **Ava** serdi üleslaadimine.

 - **SAML identiteedi tüüp**, valige **argument sisaldab kasutaja salesforce.com kasutajanimi**.

 - **SAML identiteedi asukoht**, valige **teema lause NameIdentifier element on**

 - Azure AD, kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige Salesforce'i **Identiteedi pakkuja sisselogimise URL-i** välja.

 - **Teenuse pakkuja algatatud taotlemine Köitmine**, valige **HTTP Redirect**.

 - Lõpuks klõpsake **Salvesta** SAML ühekordse sisselogimise sätete rakendada.

12. Salesforce'i Vasakpoolsel navigeerimispaanil **Domeenihaldus** seotud jaotise laiendamiseks klõpsake nuppu ja seejärel käsku **Minu domeeni**.

    ![Klõpsake nuppu minu domeeniga][15]

13. Liikuge kerides jaotiseni **Autentimise konfigureerimine** ja klõpsake nuppu **Redigeeri** .

    ![Klõpsake nuppu Redigeeri][16]

14. **Autentimisteenus** jaotises Valige konfiguratsioonile SAML SSO sõbralik nimi ja klõpsake siis nuppu **Salvesta**.

    ![Valige SSO konfiguratsioon][17]

    > [AZURE.NOTE] Kui valitud on rohkem kui üks autentimisteenus, siis kui kasutajad proovivad algatada ühekordse sisselogimise oma Salesforce'i keskkonnas, pakutakse valimiseks mis autentimisteenus soovite sisselogimiseks. Kui te ei soovi, et see juhtub, siis peaksite **jätke kõik muud autentimisteenuste märkimata**.

15. Azure AD, valige sert, mida saate üles laadida Salesforce'i lubada ühekordse sisselogimise konfigureerimine kinnituse märkeruut. Klõpsake nuppu **edasi**.

    ![Märkeruudu kinnituse][18]

16. Dialoogiboks viimasel lehel tippige e-posti aadressi kui soovite saada meiliteatised tõrgete ja hoiatuste seotud selle ühekordse sisselogimise konfiguratsiooni säilitamine. 

    ![Tippige oma meiliaadress.][19]

17. Klõpsake dialoogiboksi sulgemiseks **lõpuleviimine** . Teie konfiguratsiooni testimiseks vaadake allpool olevat jaotist [Salesforce'i kasutajatele määrata](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Samm 3: Luba automaatne kasutajate ettevalmistamine

1. Azure'i AD Lühijuhend lehe Salesforce, klõpsake nuppu **Konfigureeri kasutaja ettevalmistamise** .

    ![Klõpsake nuppu kasutaja ettevalmistamise konfigureerimine][20]

2. Tippige dialoogiboksi **konfigureerimine kasutaja ettevalmistamise** Salesforce'i administraatori kasutajanime ja parooliga.

    ![Tippige oma administraatori kasutajanime ja parooli][21]

    > [AZURE.NOTE] Kui olete konfigureerinud tootmiskeskkonna, on parim administraator uue konto loomist Salesforce'i spetsiaalselt selle etapi. Sellel kontol peab olema omistatud Salesforce'i profiili **Süsteemiadministraatori poole** .

3. Salesforce'i turvalisuse Turbeloa saamiseks avage uuel vahekaardil ja logida Salesforce'i administraator samale kontole. Klõpsake lehe paremas ülanurgas nuppu oma nime ja seejärel klõpsake **Minu sätted**.

    ![Klõpsake oma nime ja seejärel klõpsake minu sätted][22]

4. Klõpsake vasakpoolsel navigeerimispaanil **isikliku** seotud jaotise laiendamiseks klõpsake nuppu ja seejärel klõpsake **Lähtesta minu Turvalisus Turbeloa**.

    ![Klõpsake oma nime ja seejärel klõpsake minu sätted][23]

5. **Lähtesta minu Turvalisus Turbeloa** lehel klõpsake nuppu **Lähtesta turvalisus Turbeloa** .

    ![Lugege hoiatusi.][24]

6. Märkige ruut selle administraatori kontoga seotud e-posti sisendkausta. Otsige e-posti Salesforce.com, mis sisaldab uue Turbeloa.

7. Luba kopeerida, minge oma Azure AD aknasse ja kleepige see **Kasutaja turvalisus Turbeloa** välja. Klõpsake nuppu **edasi**.

    ![Turbeloa kleepimine][25]

8. Klõpsake lehel confirmation saate teavituste e-posti jaoks ettevalmistamise tõrkeid ilmneda. Klõpsake dialoogiboksi sulgemiseks **lõpuleviimine** .

    ![Tippige oma meiliaadress, kui soovite saada teatisi][26]

##<a name="step-4-assign-users-to-salesforce"></a>Samm 4: Salesforce'i kasutajate määramine

1. Teie konfiguratsiooni testimiseks Alustuseks testi uue konto loomine kataloogis.

2. Salesforce'i Kiirkäivituse lehel klõpsake nuppu **Määra kasutajad** .

    ![Klõpsake nuppu Määra kasutajatele][27]

3. Valige oma testkasutaja ja klõpsake Kuva allosas nuppu **määramine** :

 - Kui te pole seda lubada ettevalmistamise automatiseeritud kasutaja, siis kuvatakse järgmine viip, et kinnitada.

        ![Confirm the assignment.][28]

 - Kui olete lubanud ettevalmistamise automatiseeritud kasutaja, siis kuvatakse viip, et määratleda, millist tüüpi Salesforce'i profiili kasutaja peaks olema. Äsja ettevalmistatud kasutajatele kuvatakse teie Salesforce'i keskkonnas mõne minuti pärast.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Kui teil on ettevalmistamise Salesforce'i **arendaja** keskkonnas, on teil väga piiratud arv litsentside iga reegli puhul saadaval. Seetõttu on parim **Jutuvadin tasuta kasutaja** profiili, mis on saadaval 4,999 litsentside kasutajaid ette valmistada.

4. Ühekordse sisselogimise sätete testimiseks avage Accessi paneeli [https://myapps.microsoft.com](https://myapps.microsoft.com/), siis testi kontole sisse logida ja klõpsake **Salesforce'i**.

##<a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Õpetused kuidas integreerida SaaS rakenduste loend](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
