<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine NetSuite | Microsoft Azure'i"
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory NetSuite abil!"
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

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Õpetus: Kuidas NetSuite integreerimine Azure Active Directory

Selle õpetuse näitab teile, kuidas NetSuite keskkonna ühenduse loomiseks oma Azure Active Directory (Azure AD). Saate teada, ühekordse sisselogimise NetSuite konfigureerimine, lubamise automatiseeritud kasutaja ettevalmistamise kohta ja kuidas määrata kasutajatele juurdepääsu NetSuite. 

##<a name="prerequisites"></a>Eeltingimused

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)kaudu juurdepääsu Azure Active Directory, peab olema lubatud Azure tellimust.

2. Administraatori juurdepääs [NetSuite](http://www.netsuite.com/portal/home.shtml) tellimuse peab teil olema. Võite kasutada tasuta prooviversiooni konto kas teenus.

##<a name="step-1-add-netsuite-to-your-directory"></a>Samm 1: Kataloogi NetSuite lisamine

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com), klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Valige vasakpoolsel navigeerimispaanil Active Directory.][0]

2. Valige loendist **Directory** kataloogi, mida soovite lisada NetSuite abil.

3. Klõpsake ülemises menüüs **rakendused** .

    ![Klõpsake nuppu rakendused.][1]

4. Klõpsake lehe allosas **Lisa** .

    ![Lisamiseks klõpsake nuppu Lisa uus rakendus.][2]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Klõpsake käsku Lisa rakendus galeriist.][3]

6. Tippige **väljale Otsi** **NetSuite**. Seejärel valige **NetSuite** tulemustest ja **lõpuleviimine** rakenduse lisamiseks klõpsake nuppu.

    ![Lisage NetSuite.][4]

7. Nüüd peaks nähtaval Kiirkäivituse lehe NetSuite jaoks:

    ![NetSuite's Kiirkäivituse lehe Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Samm 2: Lubada ühekordse sisselogimise

1. Azure'i ad, Kiirkäivituse lehel NetSuite, klõpsake nuppu **Konfigureeri ühekordse sisselogimise** .

    ![Konfigureeri ühekordse sisselogimise nupp][6]

2. Avab dialoogiboksi ja teile kuvatakse kuva, mis küsib "Kuidas soovite kasutajate NetSuite logida?" Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Valige Azure AD ühekordse sisselogimise][7]

    > [AZURE.NOTE] Lisateavet erinevate ühekordse sisselogimise võimaluste kohta, [klõpsake siin](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. Lehel **Rakenduse sätete konfigureerimine** **Vasta URL** väljale Tippige oma NetSuite rentniku URL, kasutades ühte järgmistes vormingutes:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Tippige oma rentniku URL-is][8]

4. Lehel **konfigureerimine ühekordse sisselogimise NetSuite veebisaidil** klõpsake **metaandmete allalaadimine**ja seejärel salvestage serdi fail teie arvutile.

    ![Laadige alla metaandmeid.][9]

5. Avamine brauseris uuel vahekaardil ja saidile Netsuite ettevõtte administraatorina sisse logida.

6. Klõpsake lehe ülaosas tööriistaribal **häälestamise**, klõpsake käsku **Setup Manager**.

    ![Minge häälestamise haldur][10]

7. Valige loendist **Häälestustoimingute** **integreerimine**.

    ![Minge integreerimine][11]

8. Klõpsake jaotises **Halda autentimist** **SAML ühekordse sisselogimise**.

    ![Minge SAML Ühekordne sisselogimine][12]

9. **SAML häälestamise** lehel tehke järgmist.

    - Azure Active Directory, kopeerige **Remote sisselogimise URL-i** väärtus ja kleepige see NetSuite **Identiteedi pakkuja sisselogimislehe** välja.

        ![SAML häälestuslehele.][13]

    - Valige NetSuite, **Esmane autentimise meetodit**.

    - Valige üleslaaditavad **SAMLV2 identiteedi pakkuja metaandmete**välja **IDP metaandmete faili üleslaadimine**. Klõpsake nuppu **Sirvi** metaandmete etapis #4 allalaaditud faili üles laadida.

        ![Metaandmete üleslaadimine][16]

    - Klõpsake **esitada**.

9. Azure AD, valige sert, mida saate üles laadida NetSuite lubada ühekordse sisselogimise konfigureerimine kinnituse märkeruut. Klõpsake nuppu **edasi**.

    ![Märkeruudu kinnituse][14]

10. Dialoogiboks viimasel lehel tippige e-posti aadressi kui soovite saada meiliteatised tõrgete ja hoiatuste seotud selle ühekordse sisselogimise konfiguratsiooni säilitamine. 

    ![Tippige oma meiliaadress.][15]

11. Klõpsake dialoogiboksi sulgemiseks **lõpuleviimine** . Klõpsake lehe ülaosas **atribuute** .

    ![Klõpsake Atribuudid.][17]

12. Klõpsake **kasutaja atribuut lisada**.

    ![Klõpsake klõpsake nuppu Lisa kasutaja atribuut.][18]

13. Välja **Atribuudi nimi** tippige `account`. **Atribuudi väärtus** väljale Tippige oma NetSuite konto. Allpool on toodud juhised, kuidas leida oma konto ID:

    ![Lisage oma NetSuite konto ID.][19]

    - NetSuite, klõpsake ülemise navigeerimismenüü **häälestus** .
    - Klõpsake vasakpoolsel navigeerimispaanil valikut jaotise **Häälestustoimingute** , valige jaotises **integreerimine** ja klõpsake **Web Services eelistused**.
    - Kopeerige oma NetSuite konto ID ja kleepige see välja **Atribuudi väärtus** Azure AD.

        ![Oma konto ID hankimine][20]

14. Klõpsake Azure AD, **Complete** SAML atribuut lisamise lõpuleviimiseks. Klõpsake alumises menüüs **Muudatusi rakendada** .

    ![Salvestage muudatused.][21]

15. Enne kasutajad saavad teha ühekordse sisselogimise üheks NetSuite, need tuleb esmalt määrata NetSuite asjakohased õigused. Järgige alltoodud juhiseid nende õiguste määramiseks.

    - Klõpsake ülemise navigeerimismenüü **häälestus**, klõpsake käsku **Setup Manager**.

        ![Minge häälestamise haldur][10]

    - Klõpsake vasakpoolsel navigeerimispaanil valikut, valige **Kasutajad/rollid**ja klõpsake nuppu **Rollide haldamine**.

        ![Minge rollide haldamine][22]

    - Klõpsake nuppu **uus roll**.

    - Tippige väljale **nimi** uue rolli, ja märkige ruut **Ühekordse sisselogimise ainult** .

        ![Uude rolli nimi.][23]

    - Klõpsake nuppu **Salvesta**.

    - Menüü üleval, klõpsake nuppu **õigused**. Klõpsake nuppu **häälestus**.

        ![Minge õigused][24]

    - Valige **Määra üles SAM ühekordse sisselogimise**ja seejärel klõpsake nuppu **Lisa**.

    - Klõpsake nuppu **Salvesta**.

    - Klõpsake ülemise navigeerimismenüü **häälestus**, klõpsake käsku **Setup Manager**.

        ![Minge häälestamise haldur][10]

    - Klõpsake vasakpoolsel navigeerimispaanil valikut, valige **Kasutajad/rollid**ja klõpsake nuppu **Kasutajate haldamine**.

        ![Minge kasutajate haldamine][25]

    - Valige kasutaja test. Klõpsake nuppu **Redigeeri**.

        ![Minge kasutajate haldamine][26]

    - Klõpsake dialoogiboksis rollid valige roll, et olete loonud ja klõpsake nuppu **Lisa**.

        ![Minge kasutajate haldamine][27]

    - Klõpsake nuppu **Salvesta**.

19. Teie konfiguratsiooni testimiseks vaadake allpool olevat jaotist [NetSuite kasutajatele määrata](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Samm 3: Luba automaatne kasutajate ettevalmistamine

> [AZURE.NOTE] Vaikimisi lisatakse ettevalmistatud kasutajate root esinduse oma NetSuite keskkonna.

1. Azure Active Directory, Kiirkäivituse lehel NetSuite, klõpsake **konfigureerimine kasutaja ettevalmistamise**.

    ![Kasutaja ettevalmistamise konfigureerimine][28]

2. Avanevas dialoogiboksis tippige administraatori identimisteave jaoks NetSuite ja seejärel klõpsake nuppu **edasi**.

    ![Tippige NetSuite administraatori identimisteave.][29]

3. Klõpsake lehel confirmation Tippige e-posti aadressi saada teateid ettevalmistamise ebaõnnestumist.

    ![Kinnitage.][30]

4. Klõpsake dialoogiboksi sulgemiseks **lõpuleviimine** .

##<a name="step-4-assign-users-to-netsuite"></a>Samm 4: NetSuite kasutajate määramine

1. Teie konfiguratsiooni testimiseks käivitage test uue konto loomine kataloogis.

2. NetSuite Kiirkäivituse lehel klõpsake nuppu **Määra kasutajad** .

    ![Klõpsake nuppu Määra kasutajatele][31]

3. Valige oma testkasutaja ja klõpsake Kuva allosas nuppu **määramine** :

 - Kui te pole seda lubada ettevalmistamise automatiseeritud kasutaja, siis kuvatakse järgmine viip, et kinnitada.

        ![Confirm the assignment.][32]

 - Kui olete lubanud ettevalmistamise automatiseeritud kasutaja, siis kuvatakse viip, et määratleda, millist rolli kasutaja peaks olema NetSuite. Äsja ettevalmistatud kasutajatele kuvatakse teie NetSuite keskkonnas mõne minuti pärast.

4. Ühekordse sisselogimise sätete testimiseks avage Accessi paneeli [https://myapps.microsoft.com](https://myapps.microsoft.com/), testi kontole sisse logida ja klõpsake **NetSuite**.

##<a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Õpetused kuidas integreerida SaaS rakenduste loend](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
