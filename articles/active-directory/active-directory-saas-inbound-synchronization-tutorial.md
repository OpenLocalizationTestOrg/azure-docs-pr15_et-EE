<properties 
    pageTitle="Õpetus: Workday konfigureerimine sissetulevate sünkroonimise | Microsoft Azure'i" 
    description="Siit saate teada, kuidas kasutada Azure Active Directory sünkroonimise sissetulev lubada ühekordse sisselogimise, automatiseeritud ettevalmistamise ja muud!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Õpetus: Sissetuleva sünkroonimise Workday konfigureerimine
>[AZURE.NOTE]Azure Active Directory (AD) Premium on saadaval klientidele, kes Hiinas kasutavad worldwide eksemplari Azure AD.    
Microsoft Azure'i teenus, mida käitab 21Vianet Hiinas Azure AD Premium praegu ei toetata.    

Selle õpetuse eesmärk on kuvamiseks tuleb teha Workday ja Microsoft Azure AD Workday inimesed importimine Microsoft Azure AD juhiseid.    
 Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:  

-   Azure'i kehtiv tellimus  
-   Rentniku jaoks sisse Workday  

Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.  

1.  Rakenduste integreerimise jaoks Workday lubamine  
2.  Rakenduse integreerimise süsteemi kasutaja loomine  
3.  Turberühma loomine  
4.  Turberühma integreerimine süsteemi kasutaja määramine  
5.  Rühma turbesuvandid konfigureerimine  
6.  Turvalisus Poliitikamuudatused aktiveerimine  
7.  Seadistamine kasutajate importimine Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>Rakenduste integreerimise jaoks Workday lubamine

Selle jaotise eesmärk on liigendamine Salesforce'i jaoks rakenduse integreerimise lubamise kohta.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Rakenduste integreerimise jaoks Workday lubamiseks tehke järgmist.

1.  Klõpsake vasakpoolsel navigeerimispaanil Azure'i haldusportaal **Active Directory**.    

    ![Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.    

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .    

    ![Rakenduste] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Rakenduste")  

4.  **Rakenduse Galerii**avamiseks klõpsake nuppu **Lisa An rakendus**ja seejärel klõpsake nuppu **Lisa rakenduse kasutamise oma asutuse jaoks**.    

    ![Mida te soovite teha?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Mida te soovite teha?")  

5.  Tippige **väljale Otsi** **Workday**.    

    ![WORKDAY] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "WORKDAY")  

6.  Tulemuste paanil valige **Workday**, ja klõpsake rakenduse lisamiseks **lõpuleviimine** .    

    ![WORKDAY] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "WORKDAY")  

##<a name="creating-an-integration-system-user"></a>Rakenduse integreerimise süsteemi kasutaja loomine

1.  Sisestage otsinguväljale **luua kasutaja** **Workday töölaua**ja seejärel klõpsake linki, **Luua integreerimine süsteemi kasutaja**.     

    ![kasutaja loomine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "kasutaja loomine")  

2.  Ülesande loomine integreerimine süsteemi kasutaja uue integreerimine süsteemi kasutaja kasutajanime ja parooli sisestamise teel.  Jätke nõua uus parool järgmise Logi sisse valik märkimata, kuna see kasutaja on sisselogimist programmiliselt.    
    Jätke vaikeväärtus on 0, mis takistab selle kasutaja seansi ajalõpu ennetähtaegselt seansi ajalõpp minutit.    

    ![Integreerimine süsteemi kasutaja loomine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Integreerimine süsteemi kasutaja loomine")  

##<a name="creating-a-security-group"></a>Turberühma loomine

Selles õpetuses liigendatud stsenaariumi peate sundimatu integreerimine süsteemi Turberühma loomine ja kasutajale määrata.    

1.  Sisestage otsinguväljale Turberühma loomine, ja seejärel klõpsake linki, Turberühma loomine.     

    ![CreateSecurity rühm] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity rühm")  

2.  Turberühma loomine tööülesande lõpule viia.  Valige Integration süsteemi turberühma – sundimatu rippmenüüst tüüp rentnikuga turvalisus rühma loomiseks turberühma, kuhu lisatakse selgesõnaliselt liikmed.     

    ![CreateSecurity rühm] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity rühm")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>Turberühma integreerimine süsteemi kasutaja määramine

1.  Sisestage otsinguväljale turberühma redigeerimine ja seejärel klõpsake linki, **Turberühma redigeerimine**.     

    ![Turberühma redigeerimine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Turberühma redigeerimine")  

2.  Otsige ja valige uus integreerimine turberühma nime järgi    

    ![Turberühma redigeerimine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Turberühma redigeerimine")  

3.  Saate lisada uue turberühma uue integreerimine süsteemi kasutaja.       

    ![Süsteemi turberühm] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Süsteemi turberühm")  

##<a name="configuring-security-group-options"></a>Rühma turbesuvandid konfigureerimine

Selles etapis tuleb teil anda uue rühma õigused tagatud järgmised domeeni turbepoliitikate objektide hankimine ja pane toiminguid:  

-   Välise konto ettevalmistamise  
-   Töötaja andmed: Avaliku töötaja aruanded  
-   Töötaja andmed: Kõigi positsioonide  
-   Töötaja andmed: Praegune personali teave  
-   Töötaja andmed: Ettevõtte tiitli töötaja profiil  

&nbsp;  

1.  Sisestage väljale Otsing domeeni turbepoliitikate ja seejärel klõpsake linki, domeeni turbepoliitikate otstarbekas ala.     

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Domeeni turbepoliitikate")  

2.  Otsige süsteem ja valige süsteem funktsionaalne ala.  Klõpsake märgistatud, nuppu OK.     

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Domeeni turbepoliitikate")  

3.  Loendis turbepoliitikate süsteemi otstarbekas ala, laiendage turvalisus haldus ja valige domeeni turbepoliitika välise konto ettevalmistamise.     

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Domeeni turbepoliitikate")  

4.  Klõpsake nuppu Redigeeri õigusi ja seejärel kuval õiguste redigeerimine uue turberühma loendisse lisada turberühmade hankimine ja pane integreerimine õigustega.     

    ![Õigusetaseme redigeerimine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Õigusetaseme redigeerimine")  

5.  Korrake samm 1, ülaltoodud valimise otstarbekas alasse, kuvale ja seekord otsimine personal, valige Staffing otstarbekas ala ja klõpsake nuppu OK märgistatud.    

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Domeeni turbepoliitikate")  

6.  Laiendage turbepoliitikate Staffing otstarbekas ala loendis töötaja andmete: Staffing ja korrake juhist 4 ülaltoodud iga allesjäänud turbepoliitikate:    

    -   Töötaja andmed: Avaliku töötaja aruanded  
    -   Töötaja andmed: Kõigi positsioonide  
    -   Töötaja andmed: Praegune personali teave  
    -   Töötaja andmed: Ettevõtte tiitli töötaja profiil    

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Domeeni turbepoliitikate")  

##<a name="activating-security-policy-changes"></a>Turvalisus Poliitikamuudatused aktiveerimine

1.  Sisestage väljale Otsing aktiveerimine ja seejärel klõpsake linki, aktiveerimine ootel turvalisus poliitika muudatused.    

    ![Aktiveerimine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktiveerimine")  

2.   Alustage aktiveerimine ootel turvalisus Poliitikamuudatused tööülesande, sisestades kommentaari Auditeerimispoliitika eesmärgil, ja seejärel klõpsate märgistatud, nuppu OK.      

    ![Ootel turvalisus aktiveerimine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Ootel turvalisus aktiveerimine")  

3.  Klõpsake järgmisel kuval tööülesande lõpule viia, märkides ruudu Kinnita ja märgistatud, nupu OK märgistatud ruut.     

    ![Ootel turvalisus aktiveerimine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Ootel turvalisus aktiveerimine")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Seadistamine kasutajate importimine Microsoft Azure AD

Selle jaotise eesmärk on liigendamine Microsoft Azure AD importida inimeste Workday konfigureerimine.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Kasutajate importimine Microsoft Azure AD konfigureerimiseks tehke järgmist.

1.  Klõpsake lehel **Workday** rakenduse integreerimise **konfigureerimine kasutaja importimine** **Konfigureerimine ettevalmistamise** dialoogiboksi avamiseks.    

2.  Klõpsake lehel **sätted ja administraatori identimisteave** tehke järgmist ja seejärel klõpsake nuppu edasi.    

    ![Sätted ja administraatori identimisteave] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Sätted ja administraatori identimisteave")    

    1.  Tippige tekstiväljale **Workday administraatori kasutajanime** [loomise rakendust integreerimine süsteemi](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) jaotises loodud kasutaja nimi.    
    2.  Tippige tekstiväljale **Workday administraatori parooli** [loomise rakendust integreerimine süsteemi](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) jaotises loodud kasutaja parooli.    
    3.  Tippige tekstiväljale **Workday rentniku URL-i** URL-i või teie Workday rentniku.    

3.  Lehel **testi ühendust** nuppu **testi alustage** kinnitamiseks Ühenduvus ja klõpsake siis nuppu **edasi**.    

    ![Testi ühendust] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Testi ühendust")  

4.  Klõpsake lehel **Provisioning suvandid** nuppu **Järgmine**.    

    ![Ettevalmistamise suvandid] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Ettevalmistamise suvandid")  

5.  Klõpsake dialoogiboksis **Käivita ettevalmistamise** klõpsake nuppu **valmis**.    

    ![Käivitage ettevalmistamine] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Käivitage ettevalmistamine")  

Nüüd saate minna jaotises **Kasutajad** ja kontrollida, kas teie Workday kasutaja on imporditud.    
