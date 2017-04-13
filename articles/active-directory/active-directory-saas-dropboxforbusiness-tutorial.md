<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Dropbox for Business | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Dropbox for Business koos Azure Active Directory lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Õpetus: Azure'i Active Directory integreerimine Dropbox for Business
  
Selle õpetuse eesmärk on integreerimine Azure ja Dropbox for Business kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Testi rentniku rakenduses Dropbox for Business
  
Pärast selle õpetuse, olete määranud Dropbox for Business on võimalik ühe logige sisse oma Dropbox for Business rakenduse Azure AD kasutajate ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Dropbox for Business rakenduse integreerimise lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Stsenaarium")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Dropbox for Business rakenduse integreerimise lubamine
  
Selle jaotise eesmärk on liigendamine Dropbox for Business rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Dropbox for Business rakenduse integreerimise lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Dropbox for Business**.

    ![Rakenduse Galerii] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Dropbox for Business**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Dropbox for Business] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox for Business")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Dropbox for Business oma konto abil SAML protokolli federation Azure AD lubamise kohta.

Selle toimingu käigus on vaja oma Dropbox for Business rentniku base – 64 encoded serdi üleslaadimine. Kui te pole seda toimingut juba tuttav, vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Azure'i klassikaline portaalis lehel **Dropbox for Business** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajate Dropbox for Business logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Konfigureerimine Ühekordne sisselogimine")

3.  **Rakenduse URL-i konfigureerimine** lehel tehke järgmist.

    lisamine. Sisselogimine oma Dropboxi business rentniku jaoks. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Konfigureerimine Ühekordne sisselogimine")

    b. Klõpsake vasakul navigeerimispaanil **Halduskonsoolis**. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Konfigureerimine Ühekordne sisselogimine")

    c. **Halduskonsoolis**klõpsake vasakpoolsel navigeerimispaanil **autentimist** . 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Konfigureerimine Ühekordne sisselogimine")

    d. **Ühekordse sisselogimise** jaotises **ühekordse sisselogimise lubamiseks**valige ja seejärel klõpsake nuppu **veel** jaotise laiendamiseks.  

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Konfigureerimine Ühekordne sisselogimine")

    e. Kopeerige URL **kasutajad saavad sisse logida, sisestades oma e-posti aadress või toimuvad otse**kõrval. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Konfigureerimine Ühekordne sisselogimine")

    f. Azure'i klassikaline portaalis tekstiväljale **DropBox for business sisse logida** URL, kleepige URL. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Konfigureerimine Ühekordne sisselogimine")  



4. Lehel **Konfigureeri ühekordse sisselogimise Dropbox for Business veebisaidil** klõpsake nuppu **Laadi alla serdi**ja salvestage serdi faili oma arvutist.  

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Konfigureerimine Ühekordne sisselogimine")


5. Klõpsake oma Dropbox for Business rentniku **autentimine** lehe jaotises **ühekordse sisselogimise** , tehke järgmist. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfigureerimine Ühekordne sisselogimine")

    lisamine. Klõpsake **nõutav**.

    b. Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Dropbox for Business** dialoogiboksi kopeerige **sisselogimise lehe URL-i** väärtus ja seejärel kleepige **URL-i sisselogimine** tekstiväli.


    c. Teie allalaaditud serdi **Base-64-kodeeritud** faili loomine. 

    > [AZURE.TIP] Lisateabe saamiseks vaadake, [Kuidas teisendada teksti faili kahendarvu sert](http://youtu.be/PlgrzUZ-Y1o).


    d. Klõpsake nuppu **"Vali sertifikaat"** ja liikuge sirvides oma **base-64-kodeeritud serdifail**.


    e. Klõpsake oma Dropbox for Business rentniku konfigureerimise lõpuleviimiseks nuppu **"Salvesta muudatused"** .


6. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks. 

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Konfigureerimine Ühekordne sisselogimine")



##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selle jaotise eesmärk on liigendamine lubamine kasutaja ettevalmistamine Active Directory kasutajakontode Dropbox for Business.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1. Klõpsake lehel **Dropbox for Business** rakenduse integreerimine Azure klassikaline portaalis **konfigureerimine kasutaja ettevalmistamise** **Konfigureerimine kasutaja ettevalmistamise** dialoogiboksi avamiseks.

2. Soovitud luba kasutaja ettevalmistamise Dropbox for Business lehe, klõpsake nuppu Luba kasutaja ettevalmistamise avamiseks logige sisse Dropboxi link: Azure'i AD dialoogiboksi abil.  

    ![Kasutajate ettevalmistamine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Kasutajate ettevalmistamine")

3. Teie Dropbox for Business rentniku sisselogimiseks klõpsake dialoogiboksis **Dropboxi Azure AD link Logi sisse** . 

    ![Kasutajate ettevalmistamine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Kasutajate ettevalmistamine")



4. Klõpsake nuppu **Luba** Azure AD Dropboxi juurdepääsu anda. 

    ![Kasutajate ettevalmistamine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Kasutajate ettevalmistamine")



5. Konfiguratsiooni lõpetamiseks klõpsake nuppu **valmis** .  

    ![Kasutajate ettevalmistamine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Kasutajate ettevalmistamine")




##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Dropbox for Business kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel **Dropbox for Business **rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Jah")
  


Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Dropbox for Business.

Esimese sammuna kinnitamine, saate ettevalmistamise olek, klõpsates **armatuurlaua** **Dropbox for Business** rakenduse integreerimine Azure klassikaline portaalis lehel.

![Kasutajate määramine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Kasutajate määramine")


Tsükkel ettevalmistamise edukalt lõpule viidud kasutaja on tähistatud seotud olek.

![Kasutajate määramine] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Kasutajate määramine")


Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paneel.
Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)