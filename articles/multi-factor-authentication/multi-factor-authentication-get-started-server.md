<properties 
    pageTitle="Azure'i mitmekordne autentimine serveriga töötamise alustamine"
    description="See on Azure mitmekordne autentimise leht, mis kirjeldab, kuidas alustada Azure'i MFA Server."
    services="multi-factor-authentication"
    keywords="autentimise server Azure'i mitme tegur autentimise rakenduse aktiveerimise lehele, autentimine serveri allalaadimine"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Azure'i mitmekordne autentimine serveriga töötamise alustamine




<center>![Pilveteenuse](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nüüd kus oleme kindel, kas kasutada kohapealse mitmikautentimise, vaatame minema. Selle lehe hõlmab uue installi server ja saada see setup kohapealse Active Directory. Kui teil on juba installitud PhoneFactor server ja vaatavad täiendamine leiate [Azure'i mitmekordne serveri versioonile](multi-factor-authentication-get-started-server-upgrade.md) , või kui otsite installimise kohta lisateabe lihtsalt veebiteenuse leiate [Azure'i mitmekordne autentimine serveri Mobile rakenduse veebiteenuse juurutamine](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Azure'i Mitmikautentimise Server allalaadimine



On kaks võimalust, saate alla laadida autentimise Server Azure'i mitut tegurit. Mõlemad on tehtud Azure portaali kaudu. Esimene on hallata mitut tegurit Auth pakkuja otse. Teine on teenusesätete lehe kaudu. Teine võimalus jaoks on vaja mitut tegurit Auth pakkuja või on Azure MFA, Azure AD Premium või ettevõtte mobiilsus litsents.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Alla laadida Azure'i Mitmikautentimise server Azure'i portaalis
--------------------------------------------------------------------------------

1. Azure'i portaali administraatorina sisse logima.
2. Valige vasakul Active Directory.
3. Active Directory lehe ülaservas nuppu **Mitmekordne Auth pakkujad**
4. Kuva allosas nuppu **haldamine**
5. See avab uue lehe.  Klõpsake **allalaadimist.** 
 ![Allalaadimine](./media/multi-factor-authentication-sdk/download.png)
6. **Aktiveerimise Generate identimisteave**kohal nuppu **alla laadida.** 
 ![Allalaadimine](./media/multi-factor-authentication-get-started-server/download4.png)
7. Salvestage allalaadimine.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Allalaadimiseks Azure'i mitmekordne autentimise Server teenusesätete lehe kaudu


1. Azure'i portaali administraatorina sisse logima.
2. Valige vasakul Active Directory.
3. Topeltklõpsake oma Azure AD eksemplari.
4. Ülaosas nuppu **Konfigureeri**
![allalaadimine](./media/multi-factor-authentication-sdk/download2.png)
5. Valige jaotises mitmikautentimise **teenuse sätete haldamine**
6. Teenuste sätete lehel ekraani allservas nuppu, **minge portaali**.
![Laadi alla](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. See avab uue lehe. Klõpsake **allalaadimist.**
8. **Aktiveerimise Generate identimisteave**kohal nuppu **alla laadida.**
9. Salvestage allalaadimine.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Installima ja konfigureerima Server Azure'i Mitmikautentimise
Nüüd, kui laadisite server saate installida ja konfigureerida.  Olla kindel, et saate installida selle serveri vastab järgmistele nõuetele.



Azure'i Mitmikautentimise serveri nõuded|Kirjeldus|
:------------- | :------------- |
Riistvara|<li>200 MB kõvakettaruumi</li><li>x32 või x64 toega protsessor</li><li>1 GB või rohkem RAM-i</li>
Tarkvara|<li>Windows Server 2008 või suurem kui host on OS serveris</li><li>Windows 7 või suurem kui selle hosti on OS klient</li><li>Microsoft .NET 4.0 raames</li><li>IIS 7.0 või uuem versioon, kui installite kasutaja portaali või web SDK</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure'i mitmekordne autentimise Server tulemüüri nõuded
--------------------------------------------------------------------------------
Iga MFA server tuleb suhelda pordi 443 Väljamineva meili järgmine:

- https://pfd.phonefactor.net
- https://pfd2.phonefactor.net
- https://CSS.phonefactor.net

Kui väljaminev tulemüürid on piiratud pordi 443, tuleb avada järgmisi IP-aadresside vahemikud:

IP alamvõrgu|Võrgumask|IP-vahemiku
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Kui te ei kasuta Azure'i mitme teguriga autentimine sündmuse kinnituse funktsioonid ja kui kasutajad ei ole autentimist mitmekordne Auth mobiilirakendused seadmetest ettevõtte võrgus IP vahemikke saab vähendada järgmine:


IP alamvõrgu|Võrgumask|IP-vahemiku
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Installida ja konfigureerida Azure Mitmikautentimise
--------------------------------------------------------------------------------


1. Topeltklõpsake käivitatava. See on installimise alustamiseks.
2. Valige installikaust ekraanil, veenduge, et kaust oleks õige ja klõpsake nuppu edasi.
3. Kui installimine on lõpule viia, klõpsake nuppu valmis.  See käivitab konfiguratsiooniviisardi.
4. Konfiguratsiooniviisardi tervituskuva, märkige **Jäta autentimise konfigureerimine viisardi abil** ja klõpsake nuppu **edasi**.  See sulgege viisard ja käivitage server.
    ![Pilveteenuse](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Tagasi lehel, kus meil alla laaditud server, klõpsake nuppu **Aktiveerimise Generate identimisteave** .  Kopeerige see teave väljadele esitatud Server Azure'i MFA ja klõpsake nuppu **Aktiveeri**.


Eespool kirjeldatud toimingud kuvada ka Kiirhäälestus konfiguratsiooniviisardi abil.  Autentimise viisardi saate uuesti käivitada, valides selle serveri menüü Tööriistad.



##<a name="import-users-from-active-directory"></a>Kasutajate importimine Active Directory

Nüüd, kui server on installinud ja konfigureerinud saate kiiresti kasutajate Server Azure'i MFA importida.

### <a name="to-import-users-from-active-directory"></a>Kasutajate importimine Active Directory
--------------------------------------------------------------------------------


1. Azure'i MFA serveri vasakul, valige **Kasutajad**.
2. Allosas, valige **impordi Active Directoryst**.
3. Nüüd saate otsida üksikkasutajate või organisatsiooniüksused AD kataloogi otsida neid kasutajatega.  Sel juhul me ei määrata kindlad kasutajad OU.
4. Kõigi kasutajate paremal esile tõsta ja klõpsake nuppu **impordi**.  Peaksite nägema ütleb, et te ei õnnestunud hüpikakna.  Sulgege aken impordi.

![Pilveteenuse](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Kasutajate e-posti saatmine
Nüüd, kui kasutajad on importida Azure'i Mitmikautentimise server, on soovitatav oma kasutajatele saata meilisõnumi, mis teavitab neid, et nad on antud mitmikautentimise registreerunud.

Azure'i mitmekordne autentimis serveriga on mitmel viisil konfigureerida kasutajate mitmikautentimise kasutamise kohta.  Näiteks, kui te teate kasutajate telefoninumbrite või importida oma ettevõtte kataloogist Server Azure'i mitmekordne autentimine telefoninumbrid olid, e-posti lubage kasutajatele teada, et need on konfigureeritud Azure Mitmikautentimise, sisestada mõned juhised Azure'i Mitmikautentimise kasutamise kohta ja nad saavad oma isikutuvastusi telefoninumbri kasutaja teavitamiseks.  

Meilisõnumi sisu sõltuvalt määratud kasutajale (nt telefonikõne, SMS, mobiilirakenduse) autentimise meetodit.  Näiteks kui kasutajal on vaja PIN-koodi kasutada, kui nad autentida, e-posti ütleb neid mis nende algne PIN-kood on seatud.  Kasutajad peavad tavaliselt oma esimese autentimisel oma PIN-koodi muutmiseks.

Kui kasutajate telefoninumbrite pole konfigureeritud või imporditud Server Azure'i mitme teguri autentimist või kasutajad on eelkonfigureeritud kasutama mobiilirakenduse autentimine, saate saata neile e-posti, mis võimaldab talle teada, et need on konfigureeritud kasutama Azure'i Mitmikautentimise ja see suunab neid oma konto registreerimine Azure'i mitme teguri autentimist kasutaja portaali kaudu lõpuleviimiseks.  Hüperlingi kaasatakse, et kasutaja klõpsab kasutaja portaali. Kui kasutaja klõpsab hüperlinki, oma veebibrauseris avada ja oma ettevõtte Azure mitme teguri autentimist kasutaja portaali viimine.   


### <a name="configuring-email-and-email-templates"></a>E-posti ja meilisõnumimalle konfigureerimine

E-posti ikooni vasakul klõpsates saate häälestada sätteid nende meilide saatmisel.  See on, kuhu saate sisestada SMTP teave teie meiliserveri ja see võimaldab teil saada üldine lai meili saatmine märge lisamisega kirju kasutajad ruut.

![E-posti sätted](./media/multi-factor-authentication-get-started-server/email1.png)

Vahekaardi e-posti sisu kuvatakse kõik erinevad e-posti mallidest, mis on saadaval valida.  Sõltuvalt sellest, kuidas olete konfigureerinud mitmikautentimise kasutajatele, saate valida malli selle kõige paremini sobib teile.

![E-posti Mallid](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Kuidas Server Azure'i mitmekordne autentimine käsitleb kasutaja andmed

Mitme teguriga autentimine (MFA) serveri asutusesisese kasutamisel talletatakse kohapealsed serverid kasutaja andmeid. Püsivate kasutaja andmeid talletatakse pilves. Kui kasutaja sooritab kahekordne autentimine, saadetakse MFA Server Azure'i MFA pilveteenuses autentida soovitud andmed. Nende taotluste autentimise saatmise funktsiooni pilveteenusesse, saadetakse järgmised väljad taotlus ja logid nii, et need on saadaval autentimise kliendi kasutusaruannete. Mõned väljad on valikulised nii, et nad saavad lubada või keelata jooksul mitme teguriga autentimine Server. MFA pilveteenuses teatis MFA Server kasutab SSL/TLS üle pordi 443 Väljamineva meili. Need väljad on:

- Kordumatu ID - kasutajanimi või sisemine MFA serveri ID
- Esimene ja perekonnanimi - valikuline
- E-posti aadress - valikuline
- Telefoninumbri – kui teed telefonikõne või SMS-autentimine
- Seadme luba - tehes mobiilirakenduse autentimine
- Autentimisrežiim
- Autentimise tulem
- MFA serveri nimi
- MFA serveri IP
- Kliendi IP-kui see on saadaval



Lisaks ülaltoodud väljad, autentimise tulem (edu/eitamine) ja mis tahes keeldumiste põhjus on ka talletatud andmetega autentimise ja selle autentimis-ja kasutusaruannete kaudu.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Täpsemad Azure'i Mitmikautentimise serveri konfiguratsiooni
Lisateabe saamiseks klõpsake Täpsem häälestus ja konfiguratsiooniteavet kasutage allolevat tabelit.

Meetod|Kirjeldus
:------------- | :------------- |
[Kasutaja portaal](multi-factor-authentication-get-started-portal.md)|  Teave häälestamise ja konfigureerimise kasutaja portaali, sh juurutus- ja kasutajale iseteenindusliku.
[Teenuse Active Directory Federation](multi-factor-authentication-get-started-adfs.md)|Azure'i Mitmikautentimise AD FS häälestamise teave.
[RADIUS autentimine](multi-factor-authentication-get-started-server-radius.md)|  Teave häälestamise ja konfigureerimise Server Azure'i MFA raadiusega.
[IIS-i autentimine](multi-factor-authentication-get-started-server-iis.md)|Teave häälestamise ja konfigureerimise abil IIS-i Server Azure'i MFA.
[Windowsi autentimine](multi-factor-authentication-get-started-server-windows.md)|  Teave häälestamise ja konfigureerib Azure'i MFA serveri Windowsi autentimist.
[LDAP autentimine](multi-factor-authentication-get-started-server-ldap.md)|Teave häälestamise ja konfigureerimise Server Azure'i MFA LDAP autentimise.
[Remote lüüsi ja Azure mitmekordne autentimine serveri RADIUS abil](multi-factor-authentication-get-started-server-rdg.md)|  Teave häälestamise ja konfigureerib Azure'i MFA serveri Remote lüüsi RADIUS abil.
[Windows Serveri Active Directory sünkroonimine](multi-factor-authentication-get-started-server-dirint.md)|Teave häälestamise ja konfigureerimise Active Directory ja Azure MFA serveriga sünkroonimiseks.
[Azure'i Mitmikautentimise Server Mobile'i rakendus veebiteenuse juurutamine](multi-factor-authentication-get-started-server-webservice.md)|Häälestamise ja konfigureerimise veebiteenuse Azure'i MFA serveri teave.
