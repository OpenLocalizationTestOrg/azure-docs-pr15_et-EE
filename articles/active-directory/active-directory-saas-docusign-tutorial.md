<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine DocuSign | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja DocuSign vahel."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Õpetus: Azure'i Active Directory integreerimine DocuSign

Selle õpetuse eesmärk on integreerimine Azure ja DocuSign kuvamiseks.
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

- Azure'i kehtiv tellimus
- Rentniku jaoks sisse DocuSign



Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1. [Rakenduste integreerimise jaoks DocuSign lubamine](#enabling-the-application-integration-for-docusign) 


2. [Ühekordse sisselogimise konfigureerimine](#configuring-single-sign-on) 


3. [Konto ettevalmistamise konfigureerimine](#configuring-account-provisioning) 


4. [Kasutajate määramine](#assigning-users) 

    ![Ühekordse sisselogimise konfigureerimine][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Rakenduste integreerimise jaoks DocuSign lubamine

Selle jaotise eesmärk on liigendamine DocuSign jaoks rakenduse integreerimise lubamise kohta.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Rakenduste integreerimise jaoks DocuSign lubamiseks tehke järgmist.

1. Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Ühekordse sisselogimise konfigureerimine][1]

2. Valige loendist Directory kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Ühekordse sisselogimise konfigureerimine][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake kohta, mida soovite dialoogiboksi, klõpsake käsku **Lisa rakendus galeriist**.

    ![Ühekordse sisselogimise konfigureerimine][4]


6. Tippige otsinguväljale **DocuSign**.

    ![Ühekordse sisselogimise konfigureerimine][5]

7. Tulemuste paanil valige **DocuSign**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Ühekordse sisselogimise konfigureerimine][6]


## <a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutajate autentimiseks DocuSign oma konto abil SAML protokolli federation Azure AD lubamise kohta.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1. Azure'i klassikaline portaalis lehel **DocuSign rakenduste integreerimise** nuppu **Konfigureeri ühekordse sisselogimise** konfigureerimine ühekordse sisselogimise dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine][7]

2. **Kuidas soovite kasutajad logida DocuSign** lehel Valige **Microsoft Azure AD ühekordse sisselogimise**ja klõpsake nuppu edasi.

    ![Ühekordse sisselogimise konfigureerimine][8]

3. Klõpsake lehel **Rakenduse sätete konfigureerimine** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine][61]

    lisamine. Tippige väljale **URL-i sisse logida** `https://account.docusign.com/*`.  

    b. Tippige tekstiväljale **identifikaator** `https://account.docusign.com/*`.  
   
    c. Klõpsake nuppu **edasi**. 


    > [AZURE.TIP] Logige sisse URL ja identifikaator väärtused on ainult kohatäited. Juhised, kuidas tegelike väärtuste keskkonna hankida asuvad käesoleva teema.
 

4. Lehel **konfigureerimine ühekordse sisselogimise DocuSign juures** nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine][10]


5. Erinevate web brauseriaknas, logige sisse oma **DocuSign haldusportaali** administraatorina.


6. Klõpsake vasakpoolses menüüs navigeerimine **Domeenid**.

    ![Ühekordse sisselogimise konfigureerimine][51]

7. Klõpsake parempoolsel paanil **Taotluste domeeni**.

    ![Ühekordse sisselogimise konfigureerimine][52]

8. **Taotluste domeeni** dialoogiboksis **Domeeni nimi** väljale Tippige oma ettevõtte domeen ja klõpsake **taotluste**. Veenduge, et teie domeeni kinnitamine ja olek on aktiivne.

    ![Ühekordse sisselogimise konfigureerimine][53]

9. Klõpsake vasakus servas menüü **Identiteedipakkujad**  

    ![Ühekordse sisselogimise konfigureerimine][54]

10. Klõpsake parempoolsel paanil **Lisamine identiteedipakkuja juures**. 
    
    ![Ühekordse sisselogimise konfigureerimine][55]

11. Klõpsake lehel **Identiteedi pakkuja sätted** tehke järgmist:

    ![Ühekordse sisselogimise konfigureerimine][56]


    lisamine. Tippige väljale **nimi** konfiguratsioonist kordumatu nimi. Ärge kasutage tühikuid.

    b. Azure'i klassikaline portaalis kopeerige väljaandja URL ja seejärel kleepige **Identiteedi pakkuja väljaandja** tekstiväli.

    c. Azure'i klassikaline portaalis, kopeerige **Remote sisselogimise URL-i**ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli.

    d. Azure'i klassikaline portaalis, kopeerige **Remote välju URL-i**ja seejärel kleepige **Identiteedi pakkuja välju URL-i** tekstiväli.

    e. Valige **Logi AuthN taotlus**.

    f. Nimega **AuthN saata koosolekukutse,**valige **POSTITA**.

    g. Nimega, **Saada taotlus välju**, valige **POSTITA**. 


12. Valige jaotises **Kohandatud atribuutide vastendamise** vastendamiseks Azure AD taotlusega soovitud välja. Selles näites on vastendatud **emailaddress** taotluste **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**väärtusega. See on vaikimisi taotluste nimi: Azure'i AD taotluste e-posti jaoks. 

    > [AZURE.NOTE] Vastendage Azure AD kasutajale Docusign kasutaja vastenduse sobiv **kasutaja ID** abil. Valige väli, proper ja sisestage vastavalt oma ettevõtte sätteid vastav väärtus.

    ![Ühekordse sisselogimise konfigureerimine][57]

13. **Identiteedi pakkuja serdi** jaotises nuppu **Serdi lisamine**ja laadige Azure AD klassikaline portaalist allalaaditud serdi.   

    ![Ühekordse sisselogimise konfigureerimine][58]

14. Klõpsake nuppu **Salvesta**.

15. Jaotises **Identiteedipakkujad** nuppu **toimingud**ja seejärel klõpsake nuppu **lõpp-punktid**.   

    ![Ühekordse sisselogimise konfigureerimine][59]



10. Azure'i klassikaline portaalis, minge tagasi lehele **Rakenduse sätete konfigureerimine** . 

16. **DocuSign haldusportaali**, klõpsake jaotises **Kuva SAML 2.0 lõpp-punktid** teha, toimige järgmiselt:

    ![Ühekordse sisselogimise konfigureerimine][60]

    lisamine. Kopeerige **Teenuse pakkuja väljaandja URL**ja seejärel kleepige see **identifikaator** tekstivälja Azure klassikaline portaalis.

    b. **Teenuse pakkuja sisselogimise URL-i**kopeerige ja kleepige **Logi sisse URL-i** tekstivälja Azure klassikaline portaalis.

    c.  Klõpsake nuppu **Sule**  


10. Azure'i klassikaline portaalis, klõpsake nuppu **edasi**. 


15. Azure'i klassikaline portaalis, valige **ühekordse sisselogimise konfigureerimine kinnituse**ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduste][14]

10. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.

    ![Rakenduste][15]
 

## <a name="configuring-account-provisioning"></a>Konto ettevalmistamise konfigureerimine

Selle jaotise eesmärk on liigendamine kasutaja ettevalmistamine Active Directory kasutajakontosid, et DocuSign lubamise kohta.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1. **Azure'i klassikaline portaalis**lehel **DocuSign rakenduste integreerimise** nuppu **Konfigureeri konto ettevalmistamise** konfigureerimine kasutaja ettevalmistamise dialoogiboksi avamiseks.

    ![Konto ettevalmistamise konfigureerimine][30]

2. Lehel **sätted ja administraatori identimisteave** lubamiseks ettevalmistamise automaatse kasutaja mandaat DocuSign konto piisavaid õigusi, ja klõpsake nuppu **edasi**. 

    ![Konto ettevalmistamise konfigureerimine][31]

3. Klõpsake dialoogiboksis **ühenduse testimiseks** klõpsake nuppu **testi alustage**ja pärast edukat test, klõpsake nuppu **edasi**.

    ![Konto ettevalmistamise konfigureerimine][32]

3. Klõpsake **kinnitamise** lehel nuppu **valmis**.

    ![Konto ettevalmistamise konfigureerimine][33]
 

## <a name="assigning-users"></a>Kasutajate määramine

Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>DocuSign kasutajate määramiseks tehke järgmist.

1. **Azure'i klassikaline portaali**testi konto loomine.

2. Klõpsake lehel **DocuSign rakenduste integreerimise** **määrata kasutajatele**.

    ![Kasutajate määramine][40]
 

3. Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Kasutajate määramine][41]


Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud DocuSign.

Esimese sammuna kinnitamine, saate ettevalmistamise olek, klõpsates armatuurlaua d Azure klassikaline portaalis lehel DocuSign rakenduse integreerimine.

![Kasutajate määramine][42]

Tsükkel ettevalmistamise edukalt lõpule viidud kasutaja on tähistatud seotud olek:

![Kasutajate määramine][43]


Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel.

Accessi paani kohta leiate lisateavet teemast Sissejuhatus Accessi paani.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png