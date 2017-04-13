<properties 
    pageTitle="MFA serveri Mobile rakenduse veebiteenuse alustamine"
    description="Azure'i mitmekordne autentimine rakendus pakub võimalust täiendavate riba-, autentimine.  See võimaldab MFA serveri kasutada Tõuketeatiste kasutajatele."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>MFA serveri Mobile rakenduse veebiteenuse alustamine

Azure'i mitmekordne autentimine rakendus pakub võimalust täiendavate riba-, autentimine. Asemel automaatne telefonikõne või SMS sisselogimise ajal kasutaja paigutamise, Azure'i Mitmikautentimise sunnib teatise rakendusse Azure'i mitme teguri autentimist kasutaja nutitelefoni või tahvelarvuti. Kasutaja lihtsalt puudutab "Autentimise" (või sisestab PIN-koodi ja puudutab "Autentimise") rakenduses sisse logida.

Azure'i mitmekordne autentimine rakenduse kasutamiseks on vajalik, et rakendus saab edukalt suhelda Mobile rakenduse veebiteenuse:

- Lugege riist- ja tarkvaranõuete nõuded riist- ja tarkvara
- Peate kasutama v6.0 või uuem versioon Server Azure'i mitmekordne autentimine
- Veebiteenuse Mobile'i rakendus peab olema installitud Interneti-ühendusega veebiserver töötab Microsoft® Internet Information Services (IIS) IIS 7.x või hilisem versioon.  IIS-i kohta lisateabe saamiseks vt [IIS.NET](http://www.iis.net/).
- ASP.net-i v4.0.30319 on installitud, registreeritud ja määratud lubatud
- Nõutav rolli teenused sisaldavad ASP.net-i ja IIS 6 metaandmebaasi ühilduvuse parandamiseks
- Rakenduse Web mobiilsideteenuse peab olema avaliku URL-i kaudu
- Rakenduse Web mobiilsideteenuse peavad olema kinnitatud koos SSL-sert.
- Azure'i mitmekordne autentimine Web teenuse SDK peab olema installitud IIS-i 7.x või hilisem versioon serveris mis Server Azure'i mitmekordne autentimine
- Azure'i mitmekordne autentimine Web teenuse SDK peavad olema kinnitatud koos SSL-sert.
- Rakenduse Web mobiilsideteenuse peate saama ühenduse Azure'i mitmekordne autentimine Web teenuse SDK SSL
- Rakenduse Web mobiilsideteenuse peate saama autentida Azure'i mitmekordne autentimine Web teenuse SDK teenuse konto, mis on nimega "PhoneFactor administraatoritel" Turberühma liikme mandaadi abil. Selle teenuse konto ja rühma Active Directorys kui Azure mitmekordne autentimine serveris domeeni ühendatud serveris. Selle teenuse konto ja rühma kohalikult serveris olemas Azure'i mitmekordne autentimine kui see pole ühendatud domeeniga.


Kasutaja portaali installimine peale Azure'i mitmekordne autentimise Server server nõuab kolme järgmist:

1. Veebiteenuse SDK installimine
2. Veebiteenuse mobiilirakenduse installimine
3. Azure'i mitmekordne autentimine serveri mobiilirakenduse sätete konfigureerimine
4. Azure'i mitmekordne autentimine rakenduse aktiveerima kasutajate jaoks

## <a name="install-the-web-service-sdk"></a>Veebiteenuse SDK installimine

Kui Azure mitmekordne autentimine Web teenuse SDK serveris Azure'i mitmekordne autentimine pole juba installitud, millele ja avage Server Azure'i mitme teguri autentimist. Klõpsake ikooni Web teenuse SDK, klõpsake nuppu installida Web teenuse SDK... nupp ja esitatud juhiste järgi. Web teenuse SDK peavad olema kinnitatud koos SSL-sert. Iseallkirjastatud serdi sobib selleks, kuid see on importida "Trusted Root sertimiskeskuste" store kasutaja portaali veebiserver kohaliku arvuti konto, et see on usaldada selle serdi alustamisel SSL-ühenduse.

<center>![Häälestamine](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>Veebiteenuse mobiilirakenduse installimine
Enne installimist mobiilirakenduse veebiteenuse, arvestage järgmist:

- Kui Azure mitme teguri autentimist kasutaja portaali on juba installitud Interneti-ühendusega server, kasutajanimi, parool ja URL-i Web teenuse SDK saate kopeerida kasutaja portaali fail.
- See on kasulik veebiserverisse Interneti-ühendusega veebibrauseri avamiseks ja liikuge Web teenuse Tarkvaraarenduskomplektist, mis on sisestatud fail URL-i. Kui brauser pääseb veebiteenuse edukalt, palub teil mandaati. Sisestage kasutajanimi ja parool, mis olid sisestatud fail täpselt nii, nagu faili. Veenduge, et pole serdi hoiatused ja vigade kuvamise.
- Kui tulemüüri või puhverserveri tagant asukoha ette Mobile rakenduse veebiteenuse veebiserver ja läbimiseks SSL-i mahalaadimine, saate redigeerida Mobile rakenduse veebiteenuse fail ja lisage järgmised võti on <appSettings> jaotise nii, et Mobile rakenduse veebiteenuse abil saate http asemel HTTPS-i. Aga on vaja rakenduses Mobile tulemüüri/tagasi puhverserveri SSL. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>Veebiteenuse mobiilirakenduse installimine

<ol>
<li>Avage Windows Explorer serveris Azure'i mitme teguri autentimist ja liikuge kausta, kuhu on installitud Server Azure'i mitmekordne autentimine (nt C:\Program Files\Azure Mitmikautentimise). Azure'i mitme teguri AuthenticationPhoneAppWebServiceSetup installifail vastavalt vajadusele server, mis installitakse Mobile rakenduse veebiteenuse 32-bitine või 64-bitise versiooni valimine. Kopeerige installifail serveri Interneti-ühendusega.</li>

<li>Interneti-ühendusega veebiserverisse, tuleb faili setup käivitada administraatori õigused. Lihtsaim viis selleks on administraatorina käsuviiba avamiseks ja liikuge asukohta, kuhu installifail kopeeriti.</li>  

<li>Mitme teguri AuthenticationMobileAppWebServiceSetup installi faili käivitada, muuta saidi soovi ja muuta Virtual kataloogi nagu "PA" lühike nimi. Lühike virtuaalse kausta nime pole soovitatav, kuna kasutajad peavad sisestama Mobile'i rakendus veebiteenuste URL üheks mobiilsideseadme aktiveerimise ajal.</li>

<li>Pärast Azure mitme teguri AuthenticationMobileAppWebServiceSetup installi, liikuge sirvides C:\inetpub\wwwroot\PA (või sobiva kataloogi virtuaalse kausta nime järgi) ja redigeerida fail.</li>  

<li>Leidke WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ja WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD võtmed ja väärtused määramine kasutajanimi ja parool on PhoneFactor administraatorid turvalisus teenuse konto (vt ülal jaotises nõuetele). See võib olla sama kontoga, mida kasutatakse Azure mitme teguri autentimist kasutaja portaali ID, kui see on varem installitud. Veenduge, et kasutajanimi ja parool, sisestage vahepeal rea lõpus jutumärke (väärtus = "" / >). Soovitatav on kasutada kvalifitseeritud kasutajanimi (nt DOMEEN\kasutajanimi või machine\username).</li>  

<li>Otsige üles säte pfMobile rakenduse Web Service_pfwssdk_PfWsSdk ja muutke väärtuse "http://localhost:4898/PfWsSdk.asmx" URL-i Web teenuse Tarkvaraarenduskomplektist, mis töötab Azure'i mitmekordne autentimise Server (nt https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Kuna SSL-i kasutatakse selle ühenduse, peab viide veebis teenuse SDK serveri nimi ja mitte IP-aadress, kuna SSL-sert on välja antud serveri nimi ja URL, mida kasutatakse peab vastama serdi nimi. Kui serveri nimi ei lahendanud IP-aadressi Interneti-ühendusega serverist, kirje lisamine hosts-faili, et server Azure'i mitmekordne autentimine serveri nimi vastendamiseks IP-aadress. Salvestage fail pärast tehtud muudatustest.</li>  

<li>Kui veebisaidi veebiteenuse Mobile'i rakendus on installitud jaotises (nt Vaikeveebisait) ei ole veel antud binded avalikult allkirjastatud sertifikaadiga, installige serdi server, kui ei juba installitud, avage IIS-i halduri ja siduda serdi veebisaidile.</li>  

<li>Veebibrauseri avamiseks mis tahes arvutist ja liikuge kuhu on installitud Mobile rakenduse veebiteenuse URL (nt https://www.publicwebsite.com/PA). Veenduge, et pole serdi hoiatused ja vigade kuvamise.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure'i mitmekordne autentimine serveri mobiilirakenduse sätete konfigureerimine
Nüüd, kui veebiteenuse Mobile'i rakendus on installitud, peate konfigureerida Azure mitmekordne autentimine portaali töötamiseks.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Mobiilirakenduse sätete konfigureerimiseks server Azure'i mitmekordne autentimine

1. Azure'i mitmekordne autentimise Server, klõpsake ikooni kasutaja portaali. Kui kasutajad tohivad juhtida tema autentimismeetodite vahekaardi sätted jaotises Luba kasutajatel valida meetod, kontrollige Mobile'i rakendus. See funktsioon on sisse lülitatud, ilma lõppkasutajad peavad pöörduda oma kasutajatoe Mobile rakenduse aktiveerimiseks.
2. Märkige ruut Luba kasutajatel mobiilirakenduse väljale aktiveerimiseks.
3. Märkige ruut Luba kasutaja registreerimise.
4. Klõpsake ikooni Mobile'i rakendus.
5. Sisestage URL-i kasutatakse koos virtuaalse kataloogi, mis on loodud Azure'i mitme teguri AuthenticationMobileAppWebServiceSetup installimisel. Konto nimi võib kanda ruumi. Mobiilirakenduse kuvatakse selle ettevõtte nime. Kui tühjaks, kuvatakse teie loodud Azure'i haldusportaal mitmekordne Auth pakkuja nimi.



<center>![Häälestamine](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
