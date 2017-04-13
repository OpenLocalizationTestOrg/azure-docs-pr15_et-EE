<properties
    pageTitle="Õpetus: Azure'i Active Directory integreerimine Qlik mõttes Enterprise | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida ühekordse sisselogimise Azure Active Directory ja Qlik mõttes ettevõtte vahel."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Õpetus: Azure'i Active Directory integreerimine Qlik mõttes Enterprise

Selles õppetükis saate teada, kuidas Qlik mõttes Enterprise integreerimine Azure Active Directory (Azure AD).

Qlik mõttes Enterprise integreerimine Azure AD annab teile järgmised eelised:

- Saate määrata Azure AD, kellel on juurdepääs Qlik mõttes Enterprise
- Saate lubada Azure AD kontodega tema kasutajatele automaatselt saada allkirjastatud-on Qlik mõttes Enterprise (ühekordse sisselogimise)
- Saate hallata ühes keskses kohas – klassikaline Azure portaali konto

Kui soovite leida SaaS rakenduse integreerimine Azure AD kohta rohkem üksikasju, vaadake, [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Eeltingimused

Azure AD integreerimise konfigureerimiseks Qlik mõttes ettevõttega vajate järgmist:

- Tellimuse Azure AD
- Mõne Qlik mõttes Enterprise'i ühekordse sisselogimise lubatud tellimuse


> [AZURE.NOTE] Selle õpetuse samme testimiseks me ei soovita tootmiskeskkonna abil.


Selle õpetuse samme testimiseks tuleks nende soovituste täitmisel:

- Ärge kasutage tootmiskeskkonnast, kui see on vajalik.
- Kui teil pole keskkonnas Azure AD prooviversiooni, saate on ühe kuu prooviversiooni [siin](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Stsenaarium kirjeldus
Selles õppetükis saate testida Azure AD ühekordse sisselogimise testimiskeskkonnas.

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb kaks peamist:

1. Qlik mõttes Enterprise lisamine galeriist
2. Konfigureerimine ja testimine Azure AD ühekordse sisselogimise


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik mõttes Enterprise lisamine galeriist
Qlik mõttes Enterprise integreerimine Azure AD konfigureerimiseks tuleb kõigepealt lisada Qlik mõttes Enterprise galeriist hallatavate SaaS rakenduste loendisse.

**Qlik mõttes Enterprise galeriist lisamiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Active Directory][1]
2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste][2]

4. Klõpsake lehe allosas **Lisa** .

    ![Rakenduste][3]

5. Klõpsake dialoogiboksis **soovitud teha,** klõpsake nuppu **Lisa rakendus galeriist**.

    ![Rakenduste][4]

6. Tippige otsinguväljale **Qlik mõttes Enterprise**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Tulemuste paanil valige **Qlik mõttes Enterprise**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigureerimine ja testimine Azure AD ühekordse sisselogimise
Selles jaotises konfigureerimine ja testige Azure AD ühekordse sisselogimise Qlik mõttes Enterprise nimega "Britta Simon" testkasutaja põhjal.

Ühekordse sisselogimise töötada, peab Azure AD teada, mis Qlik mõttes Enterprise'i töölauafunktsioonid kasutaja on kasutaja Azure AD. Teisisõnu, peab toimuma Azure AD kasutaja ja seotud kasutaja Qlik mõttes Enterprise'i link seos.

Selle lingi seos on loodud määramine Azure AD **kasutajanimi** Qlik mõttes Enterprise väärtus **kasutaja nimi** väärtus.

Konfigureerimine ja testimine Azure AD ühekordse sisselogimise Qlik mõttes ettevõttega, peate täitma järgmised koosteüksuste:

1. **[Seadistamine Azure AD ühekordse sisselogimise](#configuring-azure-ad-single-sign-on)** -, et kasutajad saaksid seda funktsiooni kasutada.
2. **[Azure AD loomise katse kasutaja](#creating-an-azure-ad-test-user)** - Azure AD ühekordse sisselogimise Britta Siimoni testida.
3. **[Kasutaja loomise Qlik mõttes Enterprise testimiseks](#creating-a-qliksense-enterprise-test-user)** – olema töölauafunktsioonid Britta Simon Qlik mõttes Enterprise Azure AD kujutis tema lingitud.
4. **[Määramine Azure AD testida kasutaja](#assigning-the-azure-ad-test-user)** - Britta Simon kasutada Azure AD ühekordse sisselogimise lubamiseks.
5. **[Testimine ühekordse sisselogimise](#testing-single-sign-on)** - kontrollimaks, kas konfiguratsiooni töötab.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD ühekordse sisselogimise konfigureerimine

Selles jaotises saate Azure AD ühekordse sisselogimise klassikaline portaalis lubamine ja konfigureerimine ühekordse sisselogimise Qlik mõttes ettevõtterakenduse.


**Konfigureerida Azure AD ühekordse sisselogimise Qlik mõttes ettevõttega, tehke järgmist.**

1. Klassikaline portaalis lehel **Qlik mõttes ettevõtte** rakenduse integreerimise nuppu **Konfigureeri ühekordse sisselogimise** **Konfigureerimine ühekordse sisselogimise** dialoogiboksi avamiseks.
     
    ![Ühekordse sisselogimise konfigureerimine][6] 

2. Lehel **Kuidas soovite kasutajad logida Qlik mõttes Enterprise** valige **Azure AD ühekordse sisselogimise**, ja seejärel klõpsake nuppu **edasi**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. **Rakenduse sätete konfigureerimine** dialoogiboksi lehel tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    lisamine. **Logige sisse URL** väljale tippige URL, mida kasutatakse teie kasutajad sisselogimise Qlik mõttes Enterprise rakenduse abil järgmist mustrit: **https://\<Qlik mõttes täielikult Qualifed Hostname\>: 443 / < virtuaalse puhverserveri eesliide\>/samlauthn/**.
    
    > [AZURE.NOTE] Pange tähele selle URI lõpus lõpunullid kärpida.  See on nõutav.

    b. Klõpsake nuppu **Järgmine**
 
4. Lehel **Konfigureeri ühekordse sisselogimise Qlik mõttes ettevõtte** tehke järgmist.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    lisamine. Klõpsake **metaandmete alla laadida**ja seejärel salvestage fail oma arvutis.  Valmis enne Qlik mõttes serverisse üles selle metaandmete faili redigeerida.

    b. Klõpsake nuppu **edasi**.

5. Ettevalmistamine Federation metaandmete XML-fail, nii et saate mis Qlik mõttes serverisse üles laadida.

    > [AZURE.NOTE] Enne Qlik mõttes serverisse üles IdP metaandmeid, tuleb faili teabe vahel Azure AD õige töö tagamiseks eemaldamiseks redigeerida ja Qlik mõttes serveris.

    ![QlikSense][qs24]

    lisamine. Avage tekstiredaktoris Azure'i alla laaditud FederationMetaData.xml fail.

    b. Otsida väärtuseks **RoleDescriptor**.  Seal on neli kirjet (kaks paari avamise ja sulgemise elemendi sildid).

    c. RoleDescriptor sildid ja kogu teabe vahel failist kustutada.

    d. Salvestage fail ja jätke see läheduses kasutamiseks hiljem selles dokumendis.

6. Avage kasutaja, kes saab luua virtuaalse puhverserveri konfiguratsiooni abil soovitud Qlik mõttes Qlik Management Console (QMC).

7. Klõpsake QMC, virtuaalse puhverserveri menüükäsu.

    ![QlikSense][qs6] 

8. Ekraani allservas nuppu Loo uus.

    ![QlikSense][qs7]

9. Virtual puhverserveri Redigeeri ekraanil kuvatakse.  Klõpsake ekraani paremas servas on menüü muutes konfiguratsiooni suvandid nähtavaks.

    ![QlikSense][qs9]

10. Sisestage kood menüü suvand märgitud, Azure virtuaalse puhverserveri konfigureerimise identimisteabe.

    ![QlikSense][qs8]  
    
    lisamine. Väljale Kirjeldus on virtuaalse puhverserveri konfigureerimine sõbralik nimi.  Sisestage väärtus kirjelduse.
    
    b. Eesliite välja virtuaalse puhverserveri lõpp-punkti ühenduse Qlik mõttes koos Azure AD ühekordse sisselogimise ID.  Sisestage selle virtuaalse puhverserveri eesliite kordumatu nimi.

    c. Seansi möllitud ajalõpp (minutites) on aeg, mille ühendused selles virtuaalse puhverserveri kaudu.

    d. Seansi küpsise tabelipäise nimi on Küpsise nimi talletamise seansi identifikaator Qlik mõttes seansi kasutaja saab pärast eduka autentimise kohta.  See nimi peab olema kordumatu.

11. Klõpsake menüü autentimise suvandit nähtavaks muuta.  Kuvatakse autentimist.

    ![QlikSense][qs10]

    lisamine. **Anonüümse juurdepääsu režiimi** rippmenüü määratleb, kui anonüümsed kasutajad pääsevad Qlik mõttes virtuaalse puhverserveri kaudu.  Vaikimisi on pole Anonüümne kasutaja.

    b. Rippmenüü **autentimise meetodit** määrab, virtuaalse puhverserveri autentimine kava kasutab.  Valige rippmenüüst SAML.  Selle tulemusena kuvatakse rohkem suvandeid.

    c. **SAML host URI väljal**sisendi hostname kasutaja sisestab juurdepääsu Qlik mõttes selle SAML virtuaalse puhverserveri kaudu.  Hostname on Qlik mõttes serveri uri.

    d. **SAML üksuse ID**, sisestage sama SAML host URI väljale sisestatud väärtus.

    e. **SAML IdP metaandmete** on faili redigeerida **Azure AD konfiguratsiooni Federation metaandmete redigeerimine** jaotises varasemas versioonis.  **Enne üleslaadimist IdP metaandmeid, tuleb faili redigeerida** teabe vahel Azure AD õige töö tagamiseks eemaldamiseks ja Qlik mõttes serveris.  **Vaadake juhiseid kohal kui fail on veel redigeerida.**  Kui fail on redigeeritud klõpsake nuppu Sirvi ja valige redigeeritud metaandmete faili üleslaadimiseks virtuaalse puhverserveri konfigureerimine.

    f. Sisestage atribuudi nimi või skeemi viide SAML atribuudi tähistav **kasutajanimi** Azure AD saadab Qlik mõttes serverisse.  Skeemi teavet on saadaval Azure rakenduse ekraanid postituse konfigureerimine.  Atribuut name, **Sisestage http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**kasutada.

    g. Sisestage väärtus **kasutaja directory** manustatakse kasutajatele, kui nad autentida Qlik mõttes serveri kaudu Azure AD.  Kõva väärtused peavad olema ümbritsetud **nurksulgudega []**.  Atribuut saadetud Azure AD SAML kinnituse kasutamiseks sisestage see tekst väljale **ilma** nurksulgude atribuudi nimi.

    h. **SAML allkirjastamiseks algoritmi** määrab teenuse pakkuja (sel juhul Qlik mõttes server) serdi allkirjastamise virtuaalse puhverserveri konfigureerimise.  Kui Qlik mõttes server kasutab usaldusväärsete Microsoft RSA täiustatud ja AES Cryptographic Provider abil loodud sert, muuta SAML allkirjastamiseks algoritmi **SHA**256.

    Ma. Jaotise SAML atribuudi vastendamine võimaldab täiendavate atribuutide nagu rühmade saadetakse Qlik mõttes turvalisus reegleid kasutada.

12. Klõpsake Laadi tasakaalustamiseks menüüsuvandi nähtavaks muuta.  Koormusetasakaalustuseks ekraanil kuvatakse.

    ![QlikSense][qs11]

13. Klõpsake nupul Lisa uus server sõlm, valige engine sõlm või sõlmed Qlik mõttes seansid, et koormuse tasakaalustamiseks saata, ja klõpsake nuppu Lisa.

    ![QlikSense][qs12]

14. Klõpsake menüüd Täpsemalt suvandit nähtavaks muuta. Täiustatud ekraanil kuvatakse.

    ![QlikSense][qs13]

    lisamine. Host valge loendi tuvastab hostinimed aktsepteeritud Qlik mõttes serveriga ühenduse loomise korral.  **Sisestage hostname kasutajad saavad määrata Qlik mõttes serveriga ühenduse loomise korral.** Hostname on sama väärtuse, mis SAML host uri ilma https://.

15. Klõpsake nuppu Rakenda.

    ![QlikSense][qs14]

16. Klõpsake nuppu OK nõustuda hoiatusteade, mis kinnitab puhverserverite lingitud virtuaalse puhverserveri taaskäivitatakse.

    ![QlikSense][qs15]

17. Kuva paremas servas kuvatakse menüü assotsieerunud üksused.  Klõpsake suvandit puhverserverite menüü.

    ![QlikSense][qs16]

18. Puhverserveri ekraanil kuvatakse.  Puhverserveri linkimiseks virtuaalse puhverserveri allosas nuppu Link.

    ![QlikSense][qs17]

19. Valige puhverserveri sõlm, mis toetavad see virtuaalse puhverserveri ühendus ja klõpsake nuppu lingi.  Pärast linkimist puhverserverist jaotises seotud puhvrid.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. 5 – 10 minutit pärast sõnumi värskendamine QMC kuvatakse.  Klõpsake nuppu Värskenda QMC.

    ![QlikSense][qs20]

21. Kui soovitud QMC värskendab, klõpsake Virtual puhverserverite menüükäsu. Kuval tabelis on loetletud SAML virtuaalse puhverserveri uus kirje.  Ühe klõpsuga virtuaalse puhverserveri kirje.

    ![QlikSense][qs51]

22. Ekraani allservas nuppu alla laadida SP metaandmete aktiveerib.  Metaandmete faili salvestamiseks nuppu metaandmete SP alla laadida.

    ![QlikSense][qs52]

23. Sp metaandmete faili avada.  Jälgige **entityID** kirje ja **AssertionConsumerService** kirje.  Need väärtused on võrdne **identifikaator** ja selle **URL-i sisse logida** sisse Azure AD rakenduse konfigureerimine. Need pole vastavuses olevatele siis te peaksite Asendage need Azure AD Rakenduse konfiguratsiooniviisardi.

    ![QlikSense][qs53]

24. Klassikaline portaalis valige kinnituse ühekordse sisselogimise konfigureerimine ja klõpsake siis nuppu **edasi**.
    
    ![Azure'i AD ühekordse sisselogimise][10]

25. **Ühekordse sisselogimise kinnitamise** lehel nuppu **valmis**.  
 
    ![Azure'i AD ühekordse sisselogimise][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure'i AD testi kasutaja loomine
Selles jaotises testi kasutaja nimega Britta Simon klassikaline portaali loomine.


![Azure'i AD kasutaja loomine][20]

**Azure AD testi kasutaja loomiseks tehke järgmist.**

1. **Azure'i klassikaline portaalis**, klõpsake vasakpoolsel navigeerimispaanil nuppu **Active Directory**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3. Menüü peal kasutajate loendi kuvamiseks klõpsake linki **Kasutajad**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. **Lisa kasutaja** dialoogiboksi avamiseks klõpsake tööriistariba allosas nuppu **Lisa kasutaja**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Lehel **meile selle kasutaja kohta** dialoogiboksi, tehke järgmist:  ![Azure AD test kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    lisamine. Kasutaja tüüp, valige ettevõttes uue kasutaja.

    b. Tippige väljale Kasutajanimi **tekstivälja** **BrittaSimon**.

    c. Klõpsake nuppu **edasi**.

6.  **Kasutajaprofiili** dialoogiboksi lehel tehke järgmist: ![Azure AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    lisamine. Tippige tekstiväljale **eesnimi** **Britta**.  

    b. Tekstiväljale **Perekonnanimi** tüüp, **Simon**.

    c. Tippige väljale **Kuvatav nimi** **Britta Simon**.

    d. Valige loendis **rolliga** **kasutaja**.

    e. Klõpsake nuppu **edasi**.

7. Klõpsake **saada ajutine parool** dialoogiboksis lehe **loomine**.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. **Saada ajutine parool** dialoogiboksis lehel tehke järgmist.

    ![Azure'i AD testi kasutaja loomine](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    lisamine. Kirjutage üles väärtus **Uus parool**.

    b. Klõpsake nuppu **valmis**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Loomise rakendust Qlik mõttes Enterprise test

Selles jaotises nimega Britta Simon Qlik mõttes Enterprise'i kasutaja loomine. Tehke koostööd Qlik mõttes ettevõtte tugimeeskonna Qlik mõttes Enterprise platvormi kasutajate lisamiseks.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD testkasutaja määramine

Selles jaotises saate lubada Britta Simon kasutada Azure ühekordse sisselogimise tema juurdepääsuõiguse andmise Qlik mõttes Enterprise.

![Kasutaja määramine][200] 

**Qlik mõttes Enterprise Britta Simon määramiseks tehke järgmist.**

1. Klassikaline portaalis avamiseks Kuva rakendused kataloogi vaates, klõpsake **rakenduste** ülemises menüüs.

    ![Kasutaja määramine][201] 

2. Valige rakenduste loendis **Qlik mõttes Enterprise**.

    ![Ühekordse sisselogimise konfigureerimine](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. Klõpsake menüü peal, **Kasutajad**.

    ![Kasutaja määramine][203]

4. Valige loendis kasutajate **Britta Simon**.

5. Klõpsake tööriistaribal allosas **määramine**.

    ![Kasutaja määramine][205]


## <a name="testing-single-sign-on"></a>Ühekordse sisselogimise testimine

Selles jaotises saate oma Azure AD ühekordse sisselogimise konfiguratsiooni testimiseks Accessi paneeli abil.

Kui klõpsate paanil Accessi paani Qlik mõttes Enterprise, saate tuleks saada automaatselt allkirjastatud-on rakenduse Qlik mõttes Enterprise.


## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png