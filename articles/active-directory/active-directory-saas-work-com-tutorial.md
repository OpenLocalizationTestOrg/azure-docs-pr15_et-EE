<properties 
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Work.com | Microsoft Azure'i" 
    description="Saate teada, kuidas lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud Azure Active Directory Work.com abil!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Õpetus: Azure'i Active Directory integreerimine Work.com
  
Selle õpetuse eesmärk on integreerimine Azure ja Work.com kuvamiseks.  
Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Azure'i kehtiv tellimus
-   Work.com ühekordse sisselogimise lubatud tellimus
  
Pärast selle õpetuse, AAD kasutajad, kellele olete määrata Work.com juurdepääs on võimalik ühekordse sisselogimise rakendusse Work.com ettevõtte saidi (teenuse pakkuja algatatud Logi sisse) või [Sissejuhatus Accessi juhtpaneeli](active-directory-saas-access-panel-introduction.md)kaudu.
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1.  Rakenduste integreerimise jaoks Work.com lubamine
2.  Ühekordse sisselogimise konfigureerimine
3.  Kasutaja ettevalmistamise konfigureerimine
4.  Kasutajate määramine

![Stsenaarium] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Stsenaarium")

##<a name="enabling-the-application-integration-for-workcom"></a>Rakenduste integreerimise jaoks Work.com lubamine
  
Selle jaotise eesmärk on liigendamine Work.com jaoks rakenduse integreerimise lubamise kohta.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Work.com lubamiseks tehke järgmist.

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Rakenduse lisamine")

5.  Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Rakenduse kaudu gallerry lisamine")

6.  Tippige **väljale Otsi** **Work.com**.

    ![Rakenduse Galerii] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Rakenduse Galerii")

7.  Tulemuste paanil valige **Work.com**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Ühekordse sisselogimise konfigureerimine
  
Selle jaotise eesmärk on liigendamine kasutajate autentimiseks Work.com oma konto abil SAML protokolli federation Azure AD lubamise kohta.  
Selle toimingu käigus on vaja serdi üleslaadimine Work.com.com.

>[AZURE.NOTE] Ühekordse sisselogimise konfigureerimiseks peate setup veel Work.com kohandatud domeeni nime. Peate olema vähemalt domeeni nime määratlemine, testige teie domeeni nimi ja selle juurutama kogu ettevõtte.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Ühekordse sisselogimise konfigureerimiseks tehke järgmist.

1.  Logige sisse oma Work.com rentniku administraatorina.

2.  Minge **häälestamise**.

    ![Häälestamine] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Häälestamine")

3.  Klõpsake vasakpoolsel navigeerimispaanil jaotises **Administreerimine** käsku **Domeenihaldus** seotud jaotise laiendamiseks ja klõpsake **Minu domeeni** **Minu domeeni** lehe avamiseks. 

    ![Oma domeeni] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Oma domeeni")

4.  Veenduge, et teie domeen on õigesti häälestatud, veenduge, et see on "**Samm 4 juurutatud kasutajate**" ja vaadake üle oma "**Minu domeeni sätted**".

    ![Kasutaja juurutatud Doman] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Kasutaja juurutatud Doman")

5.  Erinevate web brauseriaknas, logige sisse oma Azure klassikaline portaali.

6.  Klõpsake lehel **Work.com **rakenduse integreerimise **konfigureerimine ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Ühekordse sisselogimise konfigureerimine")

7.  Lehel **Kuidas soovite kasutajate Work.com logida** , valige **Microsoft Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Ühekordse sisselogimise konfigureerimine")

8.  Lehel **Rakenduse URL-i konfigureerimine** **Work.com Logi sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad logida Work.com rakenduse (nt: " *http://company.my.salesforce.com*"), ja seejärel klõpsake nuppu **Järgmine**: 

    ![Rakenduse URL-i konfigureerimine] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Rakenduse URL-i konfigureerimine")

9.  Lehel **Konfigureeri ühekordse sisselogimise Work.com veebisaidil** oma sertifikaadi allalaadimiseks nuppu **Laadi alla serdi**ja seejärel salvestage serdi fail teie arvutile.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Ühekordse sisselogimise konfigureerimine")

10. Logige sisse oma Work.com rentniku.

11. Minge **häälestamise**.

    ![Häälestamine] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Häälestamine")

12. Laiendage **Turbemeetmed** menüü ja valige **Ühekordse sisselogimise sätted**.

    ![Ühekordse sisselogimise sätted] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Ühekordse sisselogimise sätted")

13. **Ühekordse sisselogimise sätete** dialoogiboks lehel tehke järgmist.

    ![SAML lubatud] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML lubatud")

    1.  Valige **SAML lubatud**.
    2.  Klõpsake nuppu **Uus**.

14. **SAML ühekordse sisselogimise sätted** jaotises tehke järgmist.

    ![SAML ühekordse sisselogimise säte] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML ühekordse sisselogimise säte")

    1.  Tippige **nimi** rühmitusaluse nimi oma konfiguratsioon.  

        >[AZURE.NOTE] **Nimi** väärtus esitada automaatselt asustada **API nimi** rühmitusaluse.

    2.  Azure klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Work.com** dialoogiboksi **Väljaandja URL-i** väärtus kopeerida ja kleepida selle **väljaandja** tekstiväli.
    3.  Laadige alla serdi, klõpsake nuppu **Sirvi**.
    4.  Tippige tekstiväljale **Üksuse Id** **https://salesforce-work.com**.
    5.  **SAML identiteedi tüüp**, valige **argument sisaldab kasutaja objekti Federation ID -d**.
    6.  **SAML identiteedi asukoht**, valige **teema lause NameIdentfier element on**.
    7.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Work.com** dialoogiboksi kopeerige **Remote sisselogimise URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja sisselogimise URL-i** tekstiväli.
    8.  Azure'i klassikaline portaalis lehel **Konfigureeri ühekordse sisselogimise veebisaidil Work.com** dialoogiboksi kopeerige **Remote välju URL-i** väärtus ja seejärel kleepige **Identiteedi pakkuja välju URL-i** tekstiväli.
    9.  Valige **Teenuse pakkuja algatatud taotlemine Köitmine**, **HTTP Post**.
    10. Klõpsake nuppu **Salvesta**.

15. Oma Work.com klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil käsku **Domeenihaldus** seotud jaotise laiendamiseks ja klõpsake **Minu domeeni** **Minu domeeni** lehe avamiseks. 

    ![Oma domeeni] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Oma domeeni")

16. Klõpsake lehel **Minu domeeni** **Login sümboolika leht** jaotises **redigeerimine**.

    ![Sisselogimislehe sümboolika] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Sisselogimislehe sümboolika")

17. Lehel **Sisselogimine lehe sümboolika** **Autentimisteenus** jaotises kuvatakse nime **SAML SSO sätted** . Valige see ja klõpsake siis nuppu **Salvesta**.

    ![Sisselogimislehe sümboolika] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Sisselogimislehe sümboolika")

18. Azure'i klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake **lõpuleviimine** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi sulgemiseks.

    ![Ühekordse sisselogimise konfigureerimine] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Ühekordse sisselogimise konfigureerimine")

##<a name="configuring-user-provisioning"></a>Kasutaja ettevalmistamise konfigureerimine
  
Azure Active Directory kasutajad saaksid sisse logida, peab ta ettevalmistatud Work.com abil.  
Puhul Work.com, ettevalmistamise on käsitsi ülesanne.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kasutaja ettevalmistamise konfigureerimiseks tehke järgmist.

1.  Logige saidile Work.com ettevõtte administraatorina.

2.  Minge **häälestamise**.

    ![Häälestamine] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Häälestamine")

3.  Minge **kasutajate haldamine \> kasutajate**.

    ![Kasutajate haldamine] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Kasutajate haldamine")

4.  Klõpsake nuppu **Uus kasutaja**.

    ![Kõik kasutajad] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Kõik kasutajad")

5.  Kasutaja redigeerimine jaotises tehke järgmist.

    ![Kasutaja redigeerimine] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Kasutaja redigeerimine")

    1.  Tippige **Perekonnanimi**, **pseudonüümi**, **e-posti**, **kasutajanimi** ja **hüüdnimi** atribuutide sobivat Azure Active Directory kontot, mida soovite seotud tekstiväljad säte.
    2.  Valige **roll**, **kasutajalitsentsi** ja **profiili**.
    3.  Klõpsake nuppu **Salvesta**.  

        >[AZURE.NOTE] Azure Active Directory konto omanik saab e-posti, sh link konto kinnitada, kuni see muutub aktiivne.

>[AZURE.NOTE] Saate kasutada mis tahes muud Work.com kasutaja konto loomise tööriistade või API-de esitatud Work.com AAD kasutajakontode.

##<a name="assigning-users"></a>Kasutajate määramine
  
Teie konfiguratsiooni testimiseks peate Azure AD kasutajatele anda soovite lubada abil oma rakenduse juurdepääsu, määrates neile.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Work.com kasutajate määramiseks tehke järgmist.

1.  Azure'i klassikaline portaalis testi konto loomine.

2.  Klõpsake lehel Work.com rakenduse integreerimise **määrata kasutajatele**.

    ![Kasutajate määramine] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Kasutajate määramine")

3.  Valige oma testkasutaja, klõpsake nuppu **Määra**, ja seejärel nuppu **Jah** ülesande kinnitamiseks.

    ![Jah] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Jah")
  
Nüüd peaks oodake, kuni 10 minuti ja veenduge, et konto on sünkroonitud Work.com.com.
  
Kui soovite ühekordse sisselogimise sätete testimiseks, avage Accessi paani. Accessi paani kohta leiate lisateavet teemast [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md).