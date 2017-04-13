<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine väljale | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure Active Directory ruut lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Õpetus: Azure'i Active Directory integreerimine ruut


  
Selle õpetuse eesmärk on integreerimine Azure ja väljale kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Väljale rentniku test
  
Pärast selle õpetuse, saab väljale määratud Azure AD kasutajate ühekordse sisselogimise rakendusse väljale ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Väljale rakenduse integreerimise lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ja rühma ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-box-tutorial/IC769537.png "Stsenaarium")



##<a name="enabling-the-application-integration-for-box"></a>Väljale rakenduse integreerimise lubamine
  
Selle jaotise eesmärk on liigendamine väljale rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Rakenduste integreerimise väljale lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-box-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-box-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-box-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  **Tippige **väljale Otsing**.**

    ![Rakenduse Galerii] (./media/active-directory-saas-box-tutorial/IC701023.png "Rakenduse Galerii")

7.  Tulemuste paanil märkige **ruut**ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Väljale] (./media/active-directory-saas-box-tutorial/IC701024.png "Väljale")



##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks väljale oma konto abil SAML protokolli federation Azure AD lubamise kohta. Selle toimingu käigus on vaja metaandmete Box.com üles laadida.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  **Konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks klõpsake lehel **ruut** rakenduste integreerimine Azure klassikaline portaalis.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-box-tutorial/IC769538.png "Konfigureerimine Ühekordne sisselogimine")

2.  Lehel **Kuidas soovite kasutajad logida väljal** valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-box-tutorial/IC769539.png "Konfigureerimine Ühekordne sisselogimine")

3.  Lehel **Rakenduse URL-i konfigureerimine** **Ruut rentniku URL** väljale Tippige väljale rentniku URL-i (nt: https://<mydomainname>. box.com), ja seejärel klõpsake nuppu **edasi**.

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-box-tutorial/IC669826.png "Rakenduse URL-i konfigureerimine")

4.  Klõpsake lehel **Konfigureeri ühekordse sisselogimise väljal** oma metaandmete allalaadimiseks **metaandmete alla laadida**, ja seejärel andmefaili kohapeal arvutisse.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-box-tutorial/IC669824.png "Konfigureerimine Ühekordne sisselogimine")

5.  Edasi dialoogiboksis faili metaandmete tugimeeskonnale. Ühekordse sisselogimise jaoks konfigureerib toe meeskond.

6.  Valige kinnituse ühekordse sisselogimise konfigureerimine, ja klõpsake nuppu **täielik** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Konfigureerimine Ühekordne sisselogimine] (./media/active-directory-saas-box-tutorial/IC769540.png "Konfigureerimine Ühekordne sisselogimine")
##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Selle jaotise eesmärk on liigendamine ettevalmistamine Active Directory kasutajakontode väljale lubamise kohta.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1. Klõpsake lehel **ruut** rakenduste integreerimine Azure klassikaline portaalis **konfigureerimine kasutaja ettevalmistamise** **Konfigureerimine kasutaja ettevalmistamise** dialoogiboksi avamiseks. 

    ![Luba automaatne kasutajate ettevalmistamine] (./media/active-directory-saas-box-tutorial/IC769541.png "Luba automaatne kasutajate ettevalmistamine")

2. Klõpsake **kasutaja ettevalmistamise ruut Luba** dialoogiboksi lehe **lubamine kasutaja ettevalmistamise**. 

    ![Luba automaatne kasutajate ettevalmistamine] (./media/active-directory-saas-box-tutorial/IC769544.png "Luba automaatne kasutajate ettevalmistamine")

3. Klõpsake lehel **sisse logida anda juurdepääsu ruut** vajalik mandaat, ja klõpsake **Autoriseerin**. 

    ![Luba automaatne kasutajate ettevalmistamine] (./media/active-directory-saas-box-tutorial/IC769546.png "Luba automaatne kasutajate ettevalmistamine")


4. Klõpsake **väljal juurdepääsu anda** selle toimingu lubada ja Azure klassikaline portaali. 

    ![Luba automaatne kasutajate ettevalmistamine] (./media/active-directory-saas-box-tutorial/IC769549.png "Luba automaatne kasutajate ettevalmistamine")


5. Lehel **Suvandid ettevalmistamise** **objekti tüübid sätet** märkeruudud lubavad teil valida kas objektide rühmitamine on ette valmistatud väljale Lisaks kasutaja objektid.  Lisateavet leiate allpool "Määramine jaotises kasutajad ja rühmad".


6. Konfiguratsiooni lõpuleviimiseks nuppu valmis. 

    ![Luba automaatne kasutajate ettevalmistamine] (./media/active-directory-saas-box-tutorial/IC769551.png "Luba automaatne kasutajate ettevalmistamine")



##<a name="assigning-a-test-user"></a>Testkasutaja määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Väljale kasutajate määramiseks tehke järgmist.

1. Azure'i klassikaline portaalis testi konto loomine.

2. Klõpsake lehel **ruut **rakenduse integreerimise **määrata kasutajatele**. 

    ![Kasutajate määramine] (./media/active-directory-saas-box-tutorial/IC769552.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks. 

    ![Jah] (./media/active-directory-saas-box-tutorial/IC767830.png "Jah")
  
Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud väljale.

Esimese sammuna kinnitamine, saate ettevalmistamise olek, klõpsates armatuurlaua d Azure klassikaline portaalis lehel ruut rakenduste integreerimine.

![Armatuurlaua] (./media/active-directory-saas-box-tutorial/IC769553.png "Armatuurlaua")

Tsükkel ettevalmistamise edukalt lõpule viidud kasutaja on tähistatud seotud olek:

![Integreerimine olek] (./media/active-directory-saas-box-tutorial/IC769555.png "Integreerimine olek")


Sihtrentnikus ruut sünkroonitud kasutajad on loetletud jaotises **Hallatavad kasutajate** **Halduskonsoolis**.

![Integreerimine olek] (./media/active-directory-saas-box-tutorial/IC769556.png "Integreerimine olek")


##<a name="assigning-users-and-groups"></a>Kasutajate ja rühmade määramine

Funktsiooni **väljale > Kasutajad ja rühmad** Azure klassikaline portaalis menüü võimaldab teil määrata, millised kasutajad ja rühmad peaks olema antud väljale. Kasutaja või rühma ülesande põhjustab tekkida järgmisi asju:

* Azure AD võimaldab autentida väljale määratud kasutajale (kas otse ülesande või rühmakuuluvust). Kui kasutaja pole määratud, siis Azure AD ei luba need väljale sisse logida ja tagastab tõrke Azure AD sisselogimislehel.

* Rakenduspaani väljale lisatakse kasutaja [rakenduse käivitit](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

* Kui automaatne ettevalmistamise on lubatud, siis määratud kasutajate ja/või rühmade lisatakse automaatselt ette valmistatud ettevalmistamise järjekorda.

    * Kui ainult kasutaja objektid on konfigureeritud olema ette valmistatud, seejärel paigutatakse kõik kasutajad otse määratud ettevalmistamise järjekorras ja kõik kasutajad, mis on mis tahes määratud rühma liikmete paigutatakse ettevalmistamise järjekorda. 
    
    * Kui objektide rühmitamine konfigureeritud olema ette valmistatud, siis kõik määratud Rühmita objektid on ette valmistatud ruut, kui ka kõik kasutajad, kes on nende rühmade liikmed. Rühma ja kasutajale liikmestaatuste jäävad alles pärast sellele väljale.
    
Saate kasutada soovitud **Atribuudid > ühekordse sisselogimise** vahekaardil konfigureerida, millised kasutaja atribuudid (või taotluste) on esitatud väljale autentimisel SAML-põhise ja **Atribuudid > Provisioning** konfigureerida, kuidas kasutaja ja rühma atribuute tulenevad Azure AD väljale ajal ettevalmistamise toimingute menüü. Leiate lisateavet teemadest all.


## <a name="additional-resources"></a>Lisaressursid

* [Nõuded välja SAML luba kohandamine](active-directory-saml-claims-customization.md)
* [Ettevalmistamine: Atribuudi vastendused kohandamine](active-directory-saas-customizing-attribute-mappings.md)
* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)
