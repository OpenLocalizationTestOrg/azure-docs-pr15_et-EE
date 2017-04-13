<properties 
    pageTitle="Õpetus: Workday konfigureerimine sissetulevate sünkroonimise | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Workday identiteedi andmete allikas Azure Active Directory jaoks." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Õpetus: Sissetuleva sünkroonimise Workday konfigureerimine


Selle õpetuse eesmärk on kuvamiseks tuleb teha Workday ja Azure AD importida inimeste Workday Azure AD juhiseid. 

Stsenaarium, mis on kirjeldatud selles õpetuses eeldab, et teil on juba järgmised üksused:

-   Kehtiv Azure AD Premiumi tellimus
-   Rentniku jaoks sisse Workday
  
Stsenaarium, mis on kirjeldatud selles õpetuses koosneb järgmistest koosteüksused.

1. Rakenduste integreerimise jaoks Workday lubamine 


2. Rakenduse integreerimise süsteemi kasutaja loomine 


3. Turberühma loomine 


4. Turberühma integreerimine süsteemi kasutaja määramine 


5. Rühma turbesuvandid konfigureerimine 


6. Turvalisus Poliitikamuudatused aktiveerimine 


7. Azure AD seadistamine kasutajate importimine 



##<a name="enabling-the-application-integration-for-workday"></a>Rakenduste integreerimise jaoks Workday lubamine
  
Selle jaotise eesmärk on liigendamine Workday jaoks rakenduse integreerimise lubamise kohta.

### <a name="steps"></a>Juhised:

1.  Klassikaline portaalis, klõpsake vasakpoolsel navigeerimispaanil, klõpsake soovitud Azure **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Active Directory")

2.  Valige loendist **Directory** kataloogi, mille jaoks soovite lubada kataloogi integreerimise.

3.  Kuva rakendused directory vaate avamiseks klõpsake ülemises menüüs **rakendused** .

    ![Rakenduste] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Rakenduste")

4.  Klõpsake lehe allosas **Lisa** .

    ![Rakenduse lisamine] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Rakenduse lisamine")

  
5. Tippige otsinguväljale **Workday**.

    ![Rakenduse kaudu gallerry lisamine] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Rakenduse kaudu gallerry lisamine")

6. Tulemuste paanil valige Workday ja nuppu rakenduse lisamiseks.

    ![Rakenduse Galerii] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Rakenduse Galerii")





## <a name="creating-an-integration-system-user"></a>Rakenduse integreerimise süsteemi kasutaja loomine

### <a name="steps"></a>Juhised:


1. Funktsiooni **Workday töölaua**, sisestage otsinguväljale kasutaja loomiseks ja klõpsake siis **Loomine integreerimine süsteemi kasutaja**. 

    ![Kasutaja loomine] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Kasutaja loomine")



2. Ülesande **Loomine integreerimine süsteemi kasutaja** uue integreerimine süsteemi kasutaja kasutajanime ja parooli sisestamise teel.  Jätke nõua uus parool järgmise Logi sisse valik märkimata, kuna see kasutaja on sisselogimist programmiliselt. Jätke vaikeväärtus on 0, mis takistab selle kasutaja seansi ajalõpu ennetähtaegselt seansi ajalõpp minutit. 

    ![Integreerimine süsteemi kasutaja loomine] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Integreerimine süsteemi kasutaja loomine")
 




## <a name="creating-a-security-group"></a>Turberühma loomine

Selles õpetuses liigendatud stsenaariumi peate sundimatu integreerimine süsteemi Turberühma loomine ja kasutajale määrata.

### <a name="steps"></a>Juhised:

1. Sisestage otsinguväljale Turberühma loomine ja klõpsake **Turberühma loomine**. 

    ![CreateSecurity rühm] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity rühm")
 

2. Turberühma loomine tööülesande lõpule viia.  Valige Integration süsteemi turberühma – sundimatu rippmenüüst tüüp rentnikuga turvalisus rühma loomiseks turberühma, kuhu lisatakse selgesõnaliselt liikmed. 

    ![CreateSecurity rühm] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity rühm")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Turberühma integreerimine süsteemi kasutaja määramine

### <a name="steps"></a>Juhised:


1. Sisestage otsinguväljale turberühma redigeerimine ja klõpsake **Turberühma redigeerimine**. 

    ![Turberühma redigeerimine] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Turberühma redigeerimine")
 
 

2. Otsige ja valige uus integreerimine turberühma nime järgi. 

    ![Turberühma redigeerimine] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Turberühma redigeerimine")

 

3. Saate lisada uue turberühma uue integreerimine süsteemi kasutaja. 

    ![Süsteemi turberühm] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Süsteemi turberühm")  




## <a name="configuring-security-group-options"></a>Rühma turbesuvandid konfigureerimine

Selles etapis tuleb teil anda uue rühma õigused tagatud järgmised domeeni turbepoliitikate objektide **hankimine** ja **sellele** toiminguid:

- Välise konto ettevalmistamise

- Töötaja andmed: Avaliku töötaja aruanded

- Töötaja andmed: Kõigi positsioonide

- Töötaja andmed: Praegune personali teave

- Töötaja andmed: Ettevõtte tiitli töötaja profiil

 
### <a name="steps"></a>Juhised:

1. Sisestage väljale Otsing domeeni turbepoliitikate ja seejärel klõpsake linki, domeeni turbepoliitikate otstarbekas ala.  

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Domeeni turbepoliitikate")  
 

2. Otsige süsteem ja valige **süsteem** funktsionaalne ala.  Klõpsake nuppu **OK**.  

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Domeeni turbepoliitikate")  


3. Loendis turbepoliitikate süsteemi otstarbekas ala, laiendage turvalisus haldus ja valige domeeni turbepoliitika välise konto ettevalmistamise.  

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Domeeni turbepoliitikate")  


4. Klõpsake nuppu **Redigeeri õigusi**ja seejärel lehel **Õiguste redigeerimine**dialoogiboksi lisada uue turberühma turberühmad **hankimine** ja **sellele** integreerimine õigustega loendit. 

    ![Õigusetaseme redigeerimine] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Õigusetaseme redigeerimine")  

 

5. Korrake samm 1, ülaltoodud valimise otstarbekas alasse, kuvale ja seekord otsimine personal, valige Staffing otstarbekas ala ja klõpsake nuppu **OK**.

    ![Domeeni turbepoliitikate] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Domeeni turbepoliitikate")  
 

6. Laiendage turbepoliitikate Staffing otstarbekas ala loendis töötaja andmete: Staffing ja korrake juhist 4 ülaltoodud iga allesjäänud turbepoliitikate:

     - Töötaja andmed: Avaliku töötaja aruanded

     - Töötaja andmed: Kõigi positsioonide

     - Töötaja andmed: Praegune personali teave

     - Töötaja andmed: Ettevõtte tiitli töötaja profiil


    ![Domeeni turbepoliitikate] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Domeeni turbepoliitikate")  







## <a name="activating-security-policy-changes"></a>Turvalisus Poliitikamuudatused aktiveerimine

### <a name="steps"></a>Juhised:

1. Sisestage väljale Otsing aktiveerimine ja seejärel klõpsake linki, aktiveerimine ootel turvalisus poliitika muudatused. 

    ![Aktiveerimine] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktiveerimine") 
 

2. Alustamiseks aktiveerimine ootel turvalisus Poliitikamuudatused tööülesande sisestamine kommentaari Auditeerimispoliitika eesmärgil ning seejärel klõpsake nuppu **OK**. 

    ![Ootel turvalisus aktiveerimine] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Ootel turvalisus aktiveerimine")   
 

3. Järgmisel kuval tööülesande lõpule viia, märkides ruudu Kinnita märgistatud ruut ja seejärel klõpsake nuppu **OK**. 

    ![Ootel turvalisus aktiveerimine] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Ootel turvalisus aktiveerimine")  





## <a name="configuring-user-import-in-azure-ad"></a>Azure AD seadistamine kasutajate importimine

Selle jaotise eesmärk on liigendamine Azure AD importida inimeste Workday konfigureerimine.


### <a name="steps"></a>Juhised:


1. Klõpsake lehel **Workday** rakenduse integreerimise **konfigureerimine kasutaja importimine** **Konfigureerimine ettevalmistamise** dialoogiboksi avamiseks.


2. Klõpsake lehel **sätted ja administraatori identimisteave** tehke järgmist ja klõpsake nuppu **edasi**. 

    ![Sätted ja administraatori identimisteave] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Sätted ja administraatori identimisteave")  
 
    lisamine. Tippige Workday administraator kasutaja nimi rühmitusaluse nimi kasutaja loomist kuvatakse loomine integreerimine süsteemi arvutikasutajatele.

    b. Tippige tekstiväljale Workday administraatori parooli loomist kuvatakse loomine kasutaja parooli integreerimine süsteemi kasutaja jaotis.

    c. Tippige tekstiväljale Workday rentniku URL-i URL-i või teie Workday rentniku.


3. Lehel **testi ühendust** nuppu **testi alustage** kinnitamiseks Ühenduvus ja klõpsake siis nuppu **edasi**. 

    ![Testi ühendust] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Testi ühendust")  
 

4. Klõpsake lehel **Provisioning** suvandid nuppu **Järgmine**. 

    ![Ettevalmistamise suvandid] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Ettevalmistamise suvandid")


5. Klõpsake dialoogiboksis **Käivita ettevalmistamise** klõpsake nuppu **valmis**. 

    ![Käivitage ettevalmistamine] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Käivitage ettevalmistamine")
 

Nüüd saate minna jaotises **Kasutajad** ja kontrollida, kas teie Workday kasutaja on imporditud.



## <a name="additional-resources"></a>Lisaressursid

* [Õpetused kohta, kuidas integreerimine Azure Active Directory SaaS rakenduste loend](active-directory-saas-tutorial-list.md)
* [Mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory?](active-directory-appssoaccess-whatis.md)
