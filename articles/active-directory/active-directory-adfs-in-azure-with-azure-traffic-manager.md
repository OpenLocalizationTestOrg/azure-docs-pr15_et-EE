<properties
    pageTitle="Kõrge-saadavus-geograafiliste AD FS-i juurutamist Azure Azure'i liikluse haldur | Microsoft Azure'i"
    description="Selles dokumendis saate teada, kuidas kasutada Azure AD FS-i kõrge ligipääsetavusele."
    keywords="Azure'i liikluse haldur, ADFS-i Azure liikluse haldur geograafilised, mitme andmekeskuses, geograafilised andmekeskuste, mitut geograafilist andmekeskuste, AD FS-i juurutada Azure AD FS-i, juurutada azure ADFS-i, azure ADFS-i, azure ad FS-i, juurutada ADFS-i, juurutada ad FS-i ADFS-i Azure, juurutada ADFS-i Azure, juurutada AD FS-i azure, ADFS-i azure AD FS-i, Azure AD FS-i Azure iaas tutvustus , ADFS-i, Azure'i ADFS-i teisaldamine"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Kõrge-saadavus-geograafiliste AD FS-i juurutamist Azure Azure'i liikluse haldur

[Azure AD FS-i juurutamise](active-directory-aadconnect-azure-adfs.md) pakub üksikasjalikke suunise selle kohta, kuidas juurutamist lihtsa AD FS-i taristu Azure oma ettevõtte jaoks. Sellest artiklist leiate Azure'i AD FS-geograafiliste juurutamine loomiseks järgmist [Azure'i liikluse](../traffic-manager/traffic-manager-overview.md)halduris. Azure'i liikluse haldur aitab luua, muutes geograafiliselt ümbruses kättesaadavuse ja suure jõudlusega AD FS-i taristu ettevõtte jaoks kasutage vahemiku marsruutimise meetodite saadaval eri vajadustele infrastruktuuri kaudu.

Väga kättesaadav-geograafiliste AD FS-i taristu võimaldab.

* **Ühe punkti tõrke kõrvaldamiseks:** Tõrkesiirde võimalustega Azure'i liikluse haldur, võite saavutada väga kättesaadav AD FS-i taristu isegi siis, kui üks andmekeskuste osa maakera läheb alla
* **Täiustatud jõudlus:** Selles artiklis soovitatud juurutamise saate esitada suure jõudlusega AD FS-i taristu, mille abil saavad kasutajad autentida kiiremini. 

##<a name="design-principles"></a>Kavandamise põhimõtted

![Üldine kujundus](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Kujundamise põhimõtted on sama, nagu on loetletud kavandamise põhimõtted artikli Azure AD FS-i juurutamist. Ülaltoodud joonisel on lihtne pikendamine lihtsa juurutamise teise geograafiliste alale. Allpool on mõned punktid kaaluda laiendamisel juurutamise uue geograafiliste alale

* **Virtuaalne võrgu:** Soovite juurutada täiendav AD FS-i taristu geograafilise piirkonna peaksite looma uue virtuaalse võrgu. Ülaltoodud joonisel näha saate Geo1 VNET ja Geo2 VNET kahe virtuaalse võrgu geograafiliste piirkondade.

* **Domeenikontrollerid ja uue geograafiliste VNET AD FS-i serverid:** Soovitatav on juurutada domeeni domeenikontrollerid uue geograafilise asukoha nii, et uues AD FS-i serverid ei ole pöörduda domeenikontrolleri teise kaugele asetada võrgu autentimise ja parandades jõudlus.

* **Salvestusruumi kontod:** Salvestusruumi kontod on seotud piirkonnas. Kuna te võtavad masinad uue geograafilised piirkonnas, on teil luua uusi salvestusruumi piirkonna kasutada.  

* **Network turberühmad:** Salvestusruumi kontod ning võrgu turberühmad loodud piirkonnas ei saa kasutada mõne muu geograafilise piirkonna. Seega peate luua uue võrgu turberühmad sarnanevad esimese geograafilise asukoha jaoks INT ja DMZ alamvõrgu uue geograafilise asukoha.

* **DNS-i sildid avaliku IP-aadressid:** Azure'i liikluse haldur võib viidata lõpp-punktid ainult DNS-i sildid kaudu. Seetõttu on vaja luua välise koormus soolise avaliku IP-aadressid DNS-i sildid.

* **Azure liikluse haldur:** Microsoft Azure'i liikluse haldur võimaldab teil määrata kasutaja-liikluse oma teenuse lõpp-punktid töötab üle maailma eri andmekeskuste jaotuse. Azure'i liikluse haldur töötab DNS-i tasemel. DNS-i vastuste kasutab globaalselt jaotatud lõpp-punktid lõppkasutaja liikluse suunamiseks. Klientide ühenduse siis otse nende lõpp-punktid. Marsruutimise erinevaid võimalusi jõudluse, kaalutud ja Priority (prioriteet), saate hõlpsasti marsruutimist, mis sobib teie asutuse vajadustele kõige paremini. 

* **V-net V-neto ühenduvuse kahe piirkondade vahel:** Teil pole vaja on virtuaalse võrgu Ühenduvus ise. Kuna iga virtuaalse võrgu on juurdepääs domeenikontrollerid ja AD FS-i ja WAP server on juba ise, saate need töötavad ilma mis tahes ühendus virtuaalse võrgu eri piirkondade vahel. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Azure'i liikluse haldur sammud

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>AD FS-i uue geograafilise asukoha juurutamine
Järgige juhiseid ja [Azure AD FS-i juurutamise](active-directory-aadconnect-azure-adfs.md) juurutamiseks sama topoloogia uue geograafilise piirkonna juhised.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>DNS-i sildid Internet vastastikuste (avaliku) koormus soolise avaliku IP-aadressid
Eespool nimetatud Azure'i liikluse haldur saab ainult viidata DNS-i sildid nimega lõpp-punktid ja seetõttu on oluline DNS-i väliste koormus soolise avaliku IP-aadresside jaoks siltide loomine. Allpool pildil näitab, kuidas saate konfigureerida oma DNS-i sildi avaliku IP-aadressi. 

![DNS-i silt](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Azure'i liikluse haldur juurutamine

Järgige juhiseid, et luua liikluse haldur profiili. Lisateabe saamiseks ka leiate [Azure'i liikluse haldur profiil on haldamine](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Liikluse haldur profiili loomine:** Andke profiili liikluse haldur kordumatu nimi. Seda profiili nimi on osa DNS-i nimi ja eesliide sildi liikluse haldur domeeni nimi. Nime / lisatakse eesliide. trafficmanager.net luua oma liikluse sildi DNS-i haldur. Pildil kujutatud liikluse ülemus DNS-i eesliide loodavasse mysts ja tulemuseks DNS-i silt on mysts.trafficmanager.net. 

    ![Liikluse haldur profiili loomine](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Liikluse marsruutimise meetod:** See on saadaval liikluse haldur marsruutimise kolm võimalust:

    * Priority (prioriteet) 
    * Jõudluse
    * Kaalutud
    
    **Jõudluse** on soovitatav suvand väga tundlik AD FS-i taristu saavutamiseks. Siiski saate valida mõne marsruutimise meetodi juurutamise teie vajadustele kõige paremini. AD FS-i funktsioonid ei mõjutab marsruutimise valitud suvand. Lisateabe saamiseks vaadake [liikluse haldur liikluse marsruutimise meetodid](../traffic-manager/traffic-manager-routing-methods.md) . Valimis pildil, mida saate vaadata valitud meetodit **jõudlust** .
   
3.  **Konfigureerimine lõpp-punktid:** Liikluse haldur lehel klõpsake lõpp-punktid ja valige Lisa. See avab lisa lõpp-punkti lehe sarnane pildil
 
    ![Lõpp-punktid konfigureerimine](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Erinevad sisendeid, järgige alltoodud juhiste:

    **Tüüp:** Valige Azure lõpp-punkti, nagu me ei osutab Azure avaliku IP-aadressi.

    **Nimi:** Nimi, mille soovite seostada lõpp-punkti loomine See pole DNS-i nimi ja ei mõjuta DNS-i kirjed.

    **Suunata ressursi tüüp:** Valige selle atribuudi väärtuseks avaliku IP-aadress. 

    **Suunata ressurss:** See annab teile erinevate DNS-i siltide kaudu valida, kas teil on saadaval jaotises tellimuse. Valige DNS-i sildi jaoks.

    Lõpp-punkti iga geograafilise piirkonna Azure'i liikluse Manager-liikluse marsruutimiseks soovite lisada.
    Lisateavet ja üksikasjalikud juhised selle kohta, kuidas lisada / konfigureerimine lõpp-punktid liikluse haldur, vaadake lisamiseks [, lubamine ja keelamine kustutamine lõpp-punktid](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Konfigureerimine juures:** Liikluse domeenihalduri lehel klõpsake konfigureerimine. Konfiguratsiooni lehel peate sond HTTP port 80 ja suhteline tee /adfs/probe kuvari sätete muutmine

    ![Juures konfigureerimine](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Veenduge, et lõpp-punktid olek on ONLINE kui konfiguratsioon on lõpule viidud**. Kui kõik lõpp-punktid on 'halvenenud' olekus, ei Azure'i liikluse haldur parim katse marsruutida liikluse eeldades, et diagnostika on vale ja kõik lõpp-punktid on kättesaadav.

5. **Muutmine DNS-i kirjete Azure'i liikluse haldur:** Federation teenust peaks olema ka CNAME Azure'i liikluse haldur DNS-i nimi. Luua ka CNAME avaliku DNS-i kirjeid nii, et kes üritab jõuda federation teenuse jõuab Azure'i liikluse ülemus.

    Näiteks osutama federation teenuse fs.fabidentity.com liikluse haldur, peate värskendama oma DNS-i ressursikirje on järgmised:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Testige marsruudi ja AD FS-i sisselogimine   

###<a name="routing-test"></a>Marsruutimise test

Väga lihtne test marsruutimise oleks proovige ping seadmes geograafiliste piirkondade federation teenuse DNS-i nimi. Sõltuvalt marsruutimise valitud, kajastub see tegelikult ping lõpp-punkti ping kuvamine. Näiteks kui valisite, et täitmise marsruutimine, seejärel lõpp-punkti lähima regiooni kliendi jõutakse. Allpool on kaks ping kaks teises regioonis klientarvutite hetktõmmist, üks vihaobjekt piirkonnas ja Lääne US. 

![Marsruutimise test](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS-i sisselogimise test

Lihtsaim viis testimiseks AD FS-i on lehe IdpInitiatedSignon.aspx abil. Selleks, et teha, et see on nõutav IdpInitiatedSignOn AD FS-i atribuutide kohta. Järgige allpool AD FS-i häälestuse kontrollimine
 
1. Käivitage selle all cmdlet AD FS server, määrake selleks lubatud PowerShelli abil. Set-AdfsProperties - EnableIdPInitiatedSignonPage $true
2. Mis tahes seadme välise juurdepääsu https:// kaudu<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Peaksite nägema AD FS-i lehe nagu allpool:

    ![Test ADFS - autentimine ülesanne](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    ja eduka sisselogimine, klõpsake seda annab teile edu sõnum nagu allpool näidatud:

    ![Test ADFS - autentimine edu](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Seotud lingid
* [Tavaline AD FS-i juurutamist Azure](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure'i liikluse haldur](../traffic-manager/traffic-manager-overview.md)
* [Liikluse haldur liikluse marsruutimise meetodid](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Järgmised sammud
* [Azure'i liikluse haldur profiil on haldamine](../traffic-manager/traffic-manager-manage-profiles.md)
* [Lisamine, keelata, lubada või kustutada lõpp-punktid](../traffic-manager/traffic-manager-endpoints.md) 

