<properties 
    pageTitle="Mitme-DRM ja juurdepääsu reguleerimine CENC: viide kujundus ja rakendada Azure ja Azure Media Servicesi | Microsoft Azure'i" 
    description="Siit saate teada, kuidas Microsoft® sujuva voogesituse kliendi teisaldamise Kit litsentsimiskeskuse." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>Mitme-DRM ja juurdepääsu reguleerimine CENC: viide kujundus ja rakendada Azure ja Azure Media Servicesi

##<a name="key-words"></a>Kirjaga
 
Azure Active Directory Azure Media Servicesi, Azure Media Playeris, dünaamiline krüptimise, litsentsi kohaletoimetamise PlayReady Widevine, FairPlay, levinud Encryption(CENC), mitme-DRM, Axinom, MÕTTEKRIIPSU, EME, MSE, JSON Web Turbeloa (JWT), taotluste, tänapäevane brauserites, võti edastamiseks, sümmeetriline võti, asümmeetriline võti, OpenID Connect X509 sert. 

##<a name="in-this-article"></a>Selle artikli teemad

Selles artiklis käsitletakse järgmisi teemasid:

- [Sissejuhatus](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Selles artiklis ülevaade](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Viide kujundus](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Vastendamine tehnoloogiaga rakendamiseks kujundus](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Rakendamine](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Viisid](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Mõned komistuskive saavutamisel rakendamiseks](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Täiendavad teemade rakendamiseks](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP- või HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory allkirjastamise võtme edastamiseks](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Kus asub Accessi Turbeloa?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Kuidas on lood Live Streaming?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Kuidas on lood litsentsi serverid väljaspool Azure Media Servicesi?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Mida teha, kui soovin kasutada kohandatud STS?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [Lõpetatud süsteem ja test](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Kasutaja sisselogimine](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Krüptitud meediumi laiendid kasutamise PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [EME kasutamise Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Kasutajad ei õigus](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Kohandatud Secure Turbeloa teenus](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Kokkuvõte](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Sissejuhatus

See on tuntud on keerukas toiming kujundada ja koostada DRM alamsüsteem jaoks soovitud OTT või online streaming lahendus. Ja see on levinud tehtemärkide/online video pakkujate tellida see osa eriotstarbelisi DRM teenusepakkujatele. Selle dokumendi eesmärk on esitada viide kujundus ja -lõpuni DRM alamsüsteem OTT või online streaming lahendus rakendamine.

Selle dokumendi suunatud lugejad on DRM alamsüsteem OTT või online streaming/mitme-screen lahenduste insenerideni või mis tahes lugejad huvitatud DRM alamsüsteem. Eeldatakse, et lugejad on vähemalt üks DRM tehnoloogiaid turu, nt PlayReady, Widevine, FairPlay või Adobe Accessi tuttav.

DRM, meil ka CENC (levinud krüptimine) koos mitme-DRM. Suur trend online streaming ja OTT valdkonna on kasutada CENC mitme-native-DRM mitme kliendi platvormi, mis on ühe DRM ja oma kliendi SDK kasutamise erinevad kliendi platvormid eelmise trend üleminekut. CENC kasutamisel koos native-DRM PlayReady nii Widevine krüptitud määratlus [Levinud krüptimise (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) kohta.

Mitme-DRM CENC eelised on järgmised:

1. Vähendab krüptimise maksab, kuna ühe krüptimise töötlemine kasutatakse suunamise eri platvormide oma kohalikke digitaalõiguste;
1. Vähendab krüptitud varade haldamine, kuna ainult ühe eksemplari krüptitud varad pole vaja;
1. Kõrvaldab DRM kliendi litsentsimise maksumus DRM omakliendi on tavaliselt tasuta oma kohalikke platvormi.

Microsoft on aktiivne korraldaja MÕTTEKRIIPSU ja CENC koos mõned peamised valdkonna mängijad. Microsoft Azure Media Servicesi pakkunud MÕTTEKRIIPSU ja CENC. Viimased teated, leiate Mingfei's ajaveebide: [väljakuulutamist Google Widevine litsentsi kohaletoimetamise teenuses Azure Media Servicesi](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)ja [Azure Media Servicesi lisab Google Widevine pakendit mitme-DRM voo pakkuda](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Selles artiklis ülevaade

Eesmärk on see artikkel sisaldab järgmist:

1. Pakub viide kujundus DRM alasüsteemi CENC abil mitme-DRM;
1. Viide rakendamist pakub Microsoft Azure/Azure Media Servicesi platvormi;
1. Käsitletakse mõned teemad ja rakendamist.

Selle artikli "mitme-DRM" hõlmab järgmist:

1. Microsoft PlayReady
1. Google Widevine
1. Apple'i FairPlay (pole veel toetas Azure Media Servicesi)

Järgmises tabelis on kokkuvõte kohalikke platvormi/native rakendus ja iga DRM toetatud brauserid.

**Kliendi platvormi**|**Kohalikke DRM tugi**|**Brauser/rakenduse**|**Streaming vormingud**
----|------|----|----
**Nutikas televiisor tehtemärk STBs, OTT STBs**|PlayReady peamiselt Widevine, või muu|Linux, Opera, WebKit, muud|Erinevate vormingud
**Windows 10 seadmed (Windows Arvutis, Windowsi tahvelarvutite, Windows Phone, Xbox)**|PlayReady|MS EME/serva/IE11<br/><br/><br/>UWP|MÕTTEKRIIPSU (HLS jaoks, PlayReady ei toetata)<br/><br/>MÕTTEKRIIPSU, sujuva voogesituse (jaoks HLS, PlayReady ei toetata) 
**Androidi seadmed (telefoni, tahvelarvuti seeria)**|Widevine|Chrome'i/EME|MÕTTEKRIIPSU
**iOS (iPhone, iPad), OS X kliendid ja Apple TV**|FairPlay|Safari 8 +/ EME|HLS
**Lisandmoodul: Adobe Primetime**|Primetime juurdepääs|Brauseri lisandmoodul|HDS HLS

Iga DRM juurutamine hetkeseisu, arvestades teenuse tavaliselt soovite rakendada 2 või 3 digitaalõiguste veendumaks, et teie aadress lõpp-punktid tüüpi parimal viisil.

On soovitud Miinuseks teenuse loogika keerukus ja keerukuse kliendi poolel teatud tasemel kasutusvõimalused erinevate klientide vahel.

Valiku tegemiseks võtke arvesse järgmisi asjaolusid:

- PlayReady algupäraselt rakendatakse iga Windowsi seadmes mõnes Androidi seadmes ja tarkvara SDK-d peaaegu iga platvormi kaudu
- Widevine rakendatakse algupäraselt Android-seadmes, Chrome ja mõnda muud seadmed
- FairPlay on saadaval ainult iOS-i ja Mac OS-is kliendid või iTunes kaudu.

Nii, et oleks tüüpiline mitme DRM:

- Variant 1: PlayReady ja Widevine
- Variant 2: PlayReady, Widevine ja FairPlay


## <a name="a-reference-design"></a>Viide kujundus

Selles jaotises tutvustame viite kujundus, mis on agnostik kasutatavaid seda rakendada.

DRM alamsüsteem võib sisaldada järgmisi komponente:

1. Võtme haldus
1. DRM pakendit
1. DRM litsentsi kohaletoimetamise
1. Õiguste kontrollimine
1. Luba/autentimine
1. Mängija
1. Origin/CDN-ID

Järgmine diagramm näitab kõrge taseme suhtluse komponentide DRM alamsüsteemis vahel.

![DRM alasüsteemi CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Kujunduse on kolme lihtsa "kihid".

1. Tagasi Office'i kiht (must), mis on esitatud väliselt;
1. "DMZ" kiht (sinine), mis sisaldab kõiki lõpp-punktid vastastikuste avaliku;
1. Avaliku Interneti kiht (helesinine) sisaldava CDN-ID ja mängijad liikluse üle avaliku Interneti.
 
On sisuhaldus tööriist, et kontrollida DRM kaitse, sõltumata sellest, milline see on staatiline või dünaamiline krüptimise. Sisendina DRM krüptimiseks peaks sisaldama järgmist:

1. MBR video sisu;
1. Sisu klahvi;
1. URL-ide litsentsi tuua.

Esituse ajal kõrge vool on:

1. Autenditud kasutaja;
1. Luba luba luuakse kasutaja;
1. Kaitsega sisu (manifest) laaditakse mängija;
1. Playeri esitab litsentsi omandamise taotluse litsentsi serverid koos võtme ID ja luba Turbeloa.

Enne järgmise teema, paar sõna võtme haldamise kujundus.

**ContentKey – vara**|**Stsenaarium**
------|---------------------------
1 – 1|Lihtsaim puhul. Pakub parimaid juhtelementi. Kuid üldiselt tulemuseks suurima litsentsi kohaletoimetamise kulu. Vähemalt ühe litsentsi taotlus on nõutav iga kaitstud vara.
1 – mitmele|Võite sama sisu klahvi mitme varad. Näiteks kõigi varade loogiline rühm, nt Žanr või alamhulga Žanr (või filmi geeni), võite kasutada ühte sisu klahvi.
Mitu – 1|Iga üksuse jaoks on vaja mitme sisu võtmed. <br/><br/>Näiteks kui soovite rakendada mitme-DRM MPEG-kriips dünaamiline CENC kaitse ja dünaamiline AES 128 krüptimine HLS, peate kahte eraldi sisu klahvid, igal versioonil eraldi ContentKeyType. (Kasutatakse dünaamilise CENC kaitse sisu võti, peaks olema ContentKeyType.CommonEncryption kasutada, samal ajal sisu dünaamilise AES 128 krüptimise kasutatud võti, tuleks kasutada ContentKeyType.EnvelopeEncryption.)<br/><br/>Teine näide CENC kaitse KRIIPSJOONE sisu, põhimõtteliselt ühte sisu võtme saab kaitsta videovoo ja muu sisu võti heli voo kaitsta. 
Mitme – mitmele-|Kombinatsiooni ülaltoodud kahte järgmist stsenaariumi: üks kogum sisu klahvid kasutatakse iga mitme vara sama vara "group".


Mõne muu oluline silmas pidada on püsiv ja püsiv litsentse kasutada.

Miks on need kaalutlused olulised? 

Neil on otsene mõju litsentsi kohaletoimetamise maksumus kui kasutate avaliku pilve litsentsi kohaletoimetamiseks. Vaatame järgmised kaks eri kujundus juhtudel illustreerimiseks:



1. Kuu tellimus: püsivate litsents ja 1-mitmele sisu võti-varade vastenduse kasutamine. Nt kasutame funktsiooni lastele Filmid ühe sisu võtme krüptimiseks. Selles näites: 

    Kokku kõik lastele Filmid/seadme jaoks nõutav # litsentside = 1

1. Kuu tellimus: püsiv litsents ja 1-1 vastenduse sisu võti ja varade vahel. Selles näites:

    Kokku kõik lastele Filmid/seadme jaoks nõutav # litsentside [# filmi vaadatud] = [# seansid] x

Nagu näete, tulemuseks kaks erinevaid kujundusi oluliselt erinev litsentsi taotluse mustrite seega litsentsi kohaletoimetamise maksumus kui litsentsi teenus on avalik pilv, nt Azure Media Servicesi.

## <a name="mapping-design-to-technology-for-implementation"></a>Vastendamine tehnoloogiaga rakendamiseks kujundus

Järgmiseks me Vastendage meie üldise kujundus tehnoloogiad Microsoft Azure/Azure Media Servicesi platvorm, määrates tehnoloogia kasutada iga koosteüksuse.

Järgmine tabel sisaldab vastendust.

**Koosteüksuse**|**Tehnoloogia**
------|-------
**Mängija**|[Azure Media Playeri](https://azure.microsoft.com/services/media-services/media-player/)
**Identiteedipakkuja (IDP)**|Azure Active Directory
**Turvaline Turbeloa teenus (STS)**|Azure Active Directory
**Töövoo DRM kaitse**|Azure Media Services dünaamilise kaitse
**DRM litsentsi kohaletoimetamise**|1. azure Media Services litsentsi kohaletoimetamise (PlayReady Widevine, FairPlay), või <br/>2 Axinom litsentsi Server, <br/>3. kohandatud PlayReady litsentsi Server
**Origin**|Azure Media Services Streaming lõpp-punkti
**Key Management**|Pole vaja viide rakendamiseks
**Veebisisu haldus**|C# konsooli rakendus

Teisisõnu on Azure AD nii identiteedi pakkuja (IDP) ja Secure Turbeloa teenus (STS). Mängija, kasutame [Azure Media Playeri API -ga](http://amp.azure.net/libs/amp/latest/docs/). Nii Azure Media Servicesi ja Azure Media Playeri tugi MÕTTEKRIIPSU ja CENC mitme-DRM.

Järgmisel joonisel on esitatud üldist struktuuri ja meilivoo koos ülaltoodud tehnoloogia kaardistamine.

![CENC AMS kohta](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Dünaamiline CENC krüptimise häälestamine, et sisuhaldus tööriist kasutab järgmist sisendina:

1. Avatud sisu;
1. Sisu võti Key loomine ja haldamine;
1. Litsentsi omandamise URL-id;
1. Teave Azure AD loend.

Sisuhaldus tööriista väljund on järgmised:

1. ContentKeyAuthorizationPolicy sisaldav määratlus kohta, kuidas litsentsi kohaletoimetamise kontrollib JWT märgiks ja DRM litsentsi tehnilised andmed;
1. AssetDeliveryPolicy, mis sisaldab kirjeldusi streaming vorming, DRM kaitse ja litsentsi omandamise URL-ide kohta.

Ajal runtime, on nagu allpool:

1. Kasutaja autentimise korral luuakse JWT luba;
1. Üks JWT luba sisalduvad nõuded on "rühmad" taotluste, mis sisaldab rühma objekti ID-d "EntitledUserGroup". Selle nõude kasutatakse läbimise "õiguse kontroll".
1. Playeri allalaadimine kliendi kaubamanifesti on CENC kaitstud sisu ja "näeb" järgmist:
    1. võti ID 
    1. sisu on kaitstud, CENC
    1. URL-ide litsentsi tuua.

1. Mängija taotleb litsentsi omandamise Toetatud brauserit/DRM põhjal. Litsentsi omandamise taotluse, sisestage ID ja esitatakse ka JWT luba. Litsentsi teenus kontrollib JWT luba ja nõuete enne väljastamist vajalik litsents.

##<a name="implementation"></a>Rakendamine

###<a name="implementation-procedures"></a>Viisid

Rakendamist hõlmab järgmist:

1. Ettevalmistused testi vara(de): mitme bitikiirusega testi videona kodeerida või paketi killustatud MP4 Azure Media Servicesi sisse. Kaitsega ei ole selles vara. Dünaamilise kaitse hiljem tehakse DRM kaitse.
1. Võtme ID ja sisu loomine klahv (soovi korral võtme seemnest). Meie eesmärgil võtme süsteemi ei ole vaja, kuna tegemist on ainult üks komplekt ID ja sisu klahvi paar testi varad;
1. AMS API abil saate konfigureerida mitme-DRM litsentsi kohaletoimetamise teenused varade test. Kui kasutate oma ettevõtte või oma ettevõtte tarnijatele litsentsi teenuses Azure Media Servicesi asemel kohandatud litsentsi, saate selle sammu vahele jätta ja määrata litsentsi omandamise URL-ide konfigureerimine litsentsi kohaletoimetamise etapis. AMS API on vaja määrata teatud üksikasjalikud konfiguratsioone, nt autoriseerimine poliitika piirang, litsentsi vastuse Mallid erinevad DRM litsentsi teenused jne. Sel ajal, Azure portaali ei võimalda vajadusele UI selle konfiguratsiooni puhul. Saate otsida API taseme teave ja proovi kood Julia Kornich dokumendis: [PlayReady kasutamise ja/või Widevine dünaamiline levinud krüptimine](media-services-protect-with-drm.md). 
1. AMS API abil saate konfigureerida varade kohaletoimetamise poliitika testi vara. Saate otsida API taseme teave ja proovi kood Julia Kornich dokumendis: [PlayReady kasutamise ja/või Widevine dünaamiline levinud krüptimine](media-services-protect-with-drm.md).
1. Loomine ja konfigureerimine on Azure Active Directory rentnik Azure;
1. Mõned Kasutajakontod ja rühmade loomine oma Azure Active Directory rentnik: loomist peaks olema vähemalt "EntitledUser" rühmitada ja selle rühma kasutaja lisamine. Selle rühma kasutajad läheb litsentsi omandamise õiguse sisse ja kasutajate selles rühmas edasi autentimise kontrollimine nurjub ja ei saa hankida mis tahes litsentsi. Nõutavad "rühmad" nõude JWT luba väljastatud Azure AD on see "EntitledUser" rühma liige. Selle nõude nõue määrata konfigureerimine mitme-DRM litsentsi kohaletoimetamise teenuste toimingut.
1. ASP.net-i MVC rakenduse, mis on hosting oma videopleieri loomine. ASP.net-i rakenduse kaitstud kasutaja autentimise Azure Active Directory rentniku vastu. Proper taotluste kaasatakse Accessi märgid, mis on saadud kasutaja autentimise. OpenID ühenduse API on soovitatav seda toimingut. Peate installima järgmised Nugeti paketid:
    - Installi pakett Microsoft.Azure.ActiveDirectory.GraphClient
    - Installi pakett Microsoft.Owin.Security.OpenIdConnect
    - Installi pakett Microsoft.Owin.Security.Cookies
    - Installi pakett Microsoft.Owin.Host.SystemWeb
    - Installi pakett Microsoft.IdentityModel.Clients.ActiveDirectory
1. Looge mängija [Azure Media Playeri API](http://amp.azure.net/libs/amp/latest/docs/)abil. [Azure Media Playeri ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) võimaldab teil määrata, millised DRM tehnoloogia kasutamine eri DRM platvormi.
1. Maatriks test:

**DRM**|**Brauseris**|**Tulem õigus kasutaja jaoks**|**Tulem un õigus kasutaja jaoks**
---|---|-----|---------
**PlayReady**|MS äärele või IE11 opsüsteemis Windows 10|Õnnestub|Nurjuda
**Widevine**|Chrome'i opsüsteemis Windows 10|Õnnestub|Nurjuda
**FairPlay** |MÄÄRATLETAKSE HILJEM||

George Trifonov Azure Media Services meeskond on kirjutatud esitada üksikasjalikud juhised loomisel Azure Active Directory ASP.net-i MVC mängija rakenduse ajaveebi: [integreerida Azure Media Services OWIN MVC vastavalt rakenduse Azure Active Directory ja sisu võtme kohaletoimetamise JWT taotluste alusel piirata](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Georg on kirjutanud ajaveebi ka [Azure Media Servicesi ja dünaamiline krüptimise JWT Turbeloa autentimist](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). Ja siin on tema [valimi Azure AD integreerimine Azure Media Servicesi võtme kohaletoimetamise kohta](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Lisateavet Azure Active Directory:

- [Azure Active Directory arendaja](../active-directory/active-directory-developers-guide.md)juhendist leiate teavet.
- Teave administraatorile leiate [Haldamine Azure AD Directory](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Mõned komistuskive saavutamisel rakendamiseks

Rakendamisel on mõned "komistuskive saavutamisel". Loodetavasti "komistuskive saavutamisel" järgmises loendis aitavad teil tõrkeotsing juhul, kui teil tekib probleeme.

1. **Väljaandja** URL-i peaks lõpus **"/"**.  

    **Sihtrühma** tuleks mängija rakenduse kliendi ID ja väljaandja URL-i lõpus tuleks lisada ka **"/"** .

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    [JWT kodeerimise](http://jwt.calebb.net/), peaksite nägema **aud** ja **iss** nimega all JWT luba:
    
    ![1st Sainpas su](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Õiguste lisamine rakenduses AAD (vahekaardil konfigureerimine rakenduse). See on nõutav iga rakenduse (kohaliku ja juurutatud versioonid).

    ![2 Sainpas su](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Õige väljaandja dünaamilise CENC kaitse loomisel kasutada.

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    Järgmine ei tööta:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    GUID on AAD rentniku ID-ga. GUID leiate Azure'i portaalis popup lõpp-punktid.

4. Andke rühmakuuluvus nõuded õigusi. Veenduge, et AAD rakenduse manifestifaili, oleme järgmist

    "groupMembershipClaims": "Kõik" (vaikeväärtus on tühi)

5. Sätte proper TokenType piirangu nõudeid loomisel.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Kuna lisamise tugi JWT (AAD) Lisaks SWT (ACS), on vaikimisi TokenType TokenType.JWT. Kui kasutate SWT/ACS, peate määrama TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Täiendavad teemade rakendamiseks
Järgmine me ei ketas uss mõned täiendavad teemade ja meie rakendamist.

###<a name="http-or-https"></a>HTTP- või HTTPS?

ASP.net-i MVC pleieri rakendus meil ehitatud peab toetama järgmist:

1. Kasutaja autentimise kaudu Azure AD mis tuleb jaotises HTTPS;
1. JWT Turbeloa Exchange'i kliendi ja Azure AD, mis peab olema jaotises HTTPS;
1. DRM litsentsi omandamise klient, mis peab olema jaotises HTTPS kui litsentsi kohaletoimetamise on Azure Media Servicesi. Muidugi mandaat PlayReady toote komplekti HTTPS kohaletoimetamiseks litsentsi. Kui teie PlayReady litsentsi server on väljaspool Azure Media Servicesi, võib kasutada HTTP- või HTTPS.

Seetõttu ASP.net-i mängija rakendusele HTTPS hea tava. See tähendab, Azure Media Playeri lehel jaotises HTTPS. Siiski voogesitamise me eelistate HTTP, seega tuleb arvestada kombineeritud sisu probleemi.

1. Brauser ei luba segasisu. Kuid lisandmoodulid nagu Silverlighti ja OSMF lisandmooduli sujuvaks ja MÕTTEKRIIPSU. Segasisu on probleemiks turbe - selle põhjuseks võimalus sisestab pahatahtlik JS, mis võivad põhjustada kliendiandmete riskid ohtu.  Brauserid blokeerida see vaikimisi ja siiani ainus viis selle lahendamiseks on serveripoolne (päritolu), kõik domeenid (sõltumata https või http) lubamiseks. See on ilmselt ei ole mõistlik teha.
1. Meil peaks vältimiseks segasisu: nii kasutada HTTP või mõlemad kasutama HTTPS-i. Segasisu esitamisel silverlightSS tehnoloogia nõuab tühjendada kombineeritud hoiatus. flashSS tehnoloogia tegeleb segasisu ilma kombineeritud hoiatus.
1. Kui teie streaming lõpp-punkti loodi enne August 2014, ei toeta HTTPS. Sel juhul lugege loomine ja kasutamine uue streaming lõpp-punkti HTTPS.

Viide rakendamiseks kaitsega sisu, rakenduse ja streaming ei ole HTTTPS. Avatud sisu, mängija ei pea autentimist või litsents, et saaksite vabalt kasutada HTTP- või HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory allkirjastamise võtme edastamiseks

See on oluline punkt oma rakendamise arvesse võtta. Kui te ei pea seda oma rakendamiseks, katkestab lõpuleviidud süsteemi töötamise lõpuks täielikult kõige 6 nädala jooksul.

Azure AD kasutab tööstusharu standardile usalda ise ja Azure AD rakenduste vahel. Täpsemalt Azure AD kasutab allkirjastamiseks võti, mis koosneb avalike ja privaatvõ võtme paari. Kui Azure AD loob Turbeloa, mis sisaldab teavet kasutaja, on see luba allkirjastatud abil oma privaatvõti enne saatmist tagasi rakendusele Azure AD. Veenduge, et luba on lubatud ja tegelikult pärineb: Azure'i AD, rakenduse peate kontrollima selle luba allkirja abil esitatud Azure AD rentniku federation metaandmete dokumendi sisalduv avalik võti. See avalik võti- ja allkirjastamiseks võti, millest ta saab – on sama, mida kasutatakse kõikide rentnike jaoks Azure AD.

Dokumendi leiate Azure'i AD-klahv üle liikumisel üksikasjalik teave: [Oluline teave sisselogimise klahvi edastamiseks Azure AD](../active-directory/active-directory-signing-key-rollover.md).

[Era-avalik võti paari](https://login.windows.net/common/discovery/keys/)vahele 

- Privaatvõti on kasutab Azure Active Directory JWT märgiks loomiseks
- Avalik võti kasutab rakendus nagu DRM litsentsi kohaletoimetamise teenused AMS kinnitamiseks JWT luba;
 
Turvalisuse eesmärgil Azure Active Directory pööratakse selle serdi perioodiliselt (iga 6 nädala järel). Võtme edastamiseks kulgevate korral turvalisus seotud rikkumiste eest, igal ajal. Seetõttu litsentsi kohaletoimetamise teenuseid AMS värskendama avalik võti kasutada Azure AD pööratakse võti paari, muidu ei õnnestu Turbeloa autentimine AMS ja antakse litsentsi. 

See on saavutada, määrates TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument konfigureerimisel DRM litsentsi kohaletoimetamise teenuseid.

JWT Turbeloa vool on nagu allpool:

1.  Azure AD välja JWT luba praeguse privaatvõti autenditud kasutaja;
2.  Kui mängija näeb on CENC mitme-kaitsega sisu, selle välja Azure AD JWT luba esmalt otsida.
3.  Mängija saadab litsentsi omandamise korral JWT luba litsentsi kohaletoimetamise teenused AMS;
4.  Litsentsi kohaletoimetamise teenuseid AMS kasutab praeguse/kehtiv avalik võti: Azure'i AD JWT luba, veenduge enne litsentside väljastamist.

DRM litsentsi kohaletoimetamise teenused on alati kontroll praeguse/kehtiv avalik võti: Azure'i AD. Avalik võti, mis on esitatud Azure AD on antud Azure AD JWT märgiks kontrollimiseks kasutatava võti.

Mida teha, kui võtme edastamiseks juhtub pärast AAD loob JWT luba, kuid enne selle JWT saadetakse mängijad luba DRM kinnitamine AMS litsentsi kohaletoimetamise teenused? 

Kuna klahvi võib igal ajal pöörata, on alati rohkem kui üks kehtiv avalik võti federation metaandmete dokumendis saadaval. Azure Media Servicesi litsentsi kohaletoimetamise saate kasutada mis tahes võtmed määratud dokumenti, kuna üks klahv võib kiiresti võetud, teise võib olla selle asendamine ja nii edasi.

### <a name="where-is-the-access-token"></a>Kus asub Accessi Turbeloa?

Kui vaatate kuidas web Appis nõuab API rakenduse jaotises [Rakenduse identiteedi OAuthi 2.0 kliendi identimisteabe anda](active-directory-authentication-scenarios.md#web-application-to-web-api), autentimine on nagu allpool:

1.  Kasutaja on sisse logitud Azure AD veebirakenduse (vt teemat [Veebirakenduse veebibrauser](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Azure AD autoriseerimine lõpp-punkti suunab kasutaja agent tagasi koos autoriseerimine tähisega klientrakendust. Kasutaja agent tagastab kliendi rakendus redirect URI kood.
3.  Veebirakenduse peab omandada juurdepääsu sümboolse, nii et see saab autentida veebi-API ja tuua soovitud ressurss. See muudab taotluse Azure AD Turbeloa lõpp-punkti, pakkudes mandaat, kliendi ID ja API veebirakenduse ID URI. Selles esitatakse luba kood tõestamaks, et kasutajal on nõustunud.
4.  Azure AD autendib rakendus ja tagastab JWT juurdepääsu luba kasutatava veebi-API kõne.
5.  Https, kasutab veebirakenduse tagastatud JWT juurdepääsu luba, et lisada JWT string "Esitaja" nimetuse taotluse päises autoriseerimine veebi-API. Veebi-API seejärel kinnitatakse JWT luba, ja kui kinnitamine õnnestus, tagastab soovitud ressurss.

See "rakenduse identiteet" vool, loodab veebi-API, et veebirakenduse autenditud kasutaja. Seetõttu nimetatakse seda mudelit usaldusväärse alamsüsteemi. [Diagrammi sellel lehel](http://msdn.microsoft.com/library/azure/dn645542.aspx/) kirjeldatakse, kuidas anda loa kood meilivoo töötab.

Litsentsi omandamise Turbeloa piiranguga, me jälgivad sama usaldusväärse alamsüsteemi muster. Ja Azure Media Servicesi teenuse litsentsi kohaletoimetamise web API ressurss, "kirjutamata ressursi" veebirakenduse vajab. Kus on nii juurdepääsu luba?

Tõepoolest me on saamine juurdepääsu luba Azure AD. Pärast eduka kasutaja autentimise, tagastatakse kood. Luba kood seejärel kasutatakse koos kliendi ID ja rakenduse võtme juurdepääsu luba vahetada. Ja juurdepääsu luba juurdepääs "kursor" rakenduse osutamine või tähistav Azure Media Servicesi litsentsi teenus.

Läheb vaja registreerida ja Azure AD "kursori" rakenduse konfigureerimiseks, järgides allolevaid juhiseid:

1.  Azure AD rentniku

    - sisselogimise URL-i taotluse (ressurss) lisamiseks tehke järgmist. 

    https://[resource_name].azurewebsites.net/ ja 

    - rakenduse ID URL: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Lisage uus võti ressursi rakenduse;
3.  Rakenduse Avaldamisfail värskendada, et groupMembershipClaims on järgmine väärtus: "groupMembershipClaims": "Kõik"  
4.  Osutab mängija web appi Azure AD Rakenduse jaotises "õigused muudes rakendustes", lisada ressursi rakendus, mis lisati juhises 1 ülaltoodud. Märkige jaotises "delegeeritud õiguste" "Juurdepääs [resource_name]" märge. See annab web appi õigus juurdepääsuks ressursi rakenduse access tähiste loomiseks. Tehke seda kohalikke ja juurutatud web appi versioon kui arendate Visual Studio ja Azure web Appiga.
    
Seetõttu on tõepoolest välja Azure AD JWT luba juurdepääsu luba juurdepääsuks ressurss "kursori".

### <a name="what-about-live-streaming"></a>Kuidas on lood Live Streaming?

Klõpsake eeltoodud meie arutelu on suunatud nõudmisel varad. Kuidas on lood live streaming?

Hea Uudised on, et saate kasutada täpselt sama ja rakendamist reaalajas streaming Azure Media Servicesi, käsitledes seostatud programmi nimega "VOD vara" vara kaitsmiseks.

Täpsemalt on teada, et teha reaalajas streaming Azure Media Servicesi, peate looma kanali ja seejärel klõpsake jaotises kanali programmi. Programmi loomiseks peate looma väärtus, mis sisaldab reaalajas arhiivi programmi jaoks. Mitme-DRM kaitsega sisu reaalajas CENC pakkumiseks kõik, mida tuleb teha, on rakendada sama setup/töötlemise vara nagu siis, kui see on "VOD vara" enne alustamist programmi.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Kuidas on lood litsentsi serverid väljaspool Azure Media Servicesi?

Sageli, võib kliendid on investeerida litsentsi serveripargi kas oma andmete keskele või majutatud DRM teenuse pakkujad. Õnneks Azure Media Services sisu kaitse võimaldab teil tegutseda hübriidrežiimis: sisu majutatud ja dünaamiliselt kaitstud Azure Media Servicesi, samal ajal DRM litsentsid on esitatud serverid Azure Media Servicesi väljaspool. Sel juhul on muudatused järgmisega.

1. Turvaline Turbeloa teenus nõuab märgid, mis on lubatud ja saavad serveripargi litsentsi kontrollida. Näiteks Widevine litsentsi serverid esitatud Axinom nõuab teatud JWT luba, mis sisaldab "õiguse sõnum". Seega peate mõne STS väljastamist sellise JWT luba. Autorid on lõpetatud sellise rakendamist ja üksikasjad leiate [Azure'i dokumentatsiooni](https://azure.microsoft.com/documentation/)keskuses järgmises dokumendis: [Abil Axinom esitamisel Azure Media Servicesi Widevine litsentse](media-services-axinom-integration.md). 
1. Te enam ei vaja konfigureerida Azure Media Servicesi litsentsi teenus (ContentKeyAuthorizationPolicy). Mida peate tegema on pakkuda litsentsi omandamise URL-id (PlayReady ning Widevine FairPlay) Kui konfigureerite AssetDeliveryPolicy häälestamise CENC mitme-DRM.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Mida teha, kui soovin kasutada kohandatud STS?

Põhjusi võib olla mõni kliendi võib kasutada kohandatud STS (Secure Turbeloa teenus) osutamiseks JWT sõned valida. Mõned neist on:

1.  Identiteedi pakkuja (IDP) kasutavad kliendi STS ei toeta. Sel juhul võib kohandatud STS soovitud suvand.
2.  Klient võib-olla rohkem paindlikumaks ja rakendusekäivitiga juhtimise STS integreerimise kliendi abonendi arveldus süsteem. Näiteks tehtemärgi MVPD võivad pakkuda mitme OTT abonendi pakette, nagu näiteks premium, tavaline, sport jne. Tehtemärk võiksite vastavad nõuded abonendi paketi luba, et ainult õige pakett sisu on kättesaadav. Sel juhul pakub kohandatud STS vajadusele paindlikkust ja juhtimine.

Kahe muudatused on vaja teha, kui kasutate kohandatud STS:

1.  Vara litsentsi teenus konfigureerimisel peate määrama kohandatud STS (täpsemalt allpool) asemel Azure Active Directory praeguse võtme kinnitamiseks kasutatavad turvakood.
2.  JTW luba loomisel asemel praeguse X509 privaatvõti on määratud turvalisus klahvi Azure Active Directory sert.

On kahte tüüpi turvalisus võtmed.

1.  Sümmeetriline võti: sama klahvi on kasutada nii genereerimine ja kontrollimiseks JWT luba;
2.  Asümmeetriline võti: era-avalik võti paari mõne X509 serdi kasutatakse koos privaatvõti krüptimine/loomisel JWT märgiks ja avalik võti luba kontrollimiseks klõpsake.

####<a name="tech-note"></a>Tehnilise Märkus

Kui kasutate .NET Framework / C# arengu platvorm, on X509 sert, mida kasutatakse asümmeetriline turvalisuse võti peab olema võtme pikkus vähemalt 2048. See on nõutav .NET Framework System.IdentityModel.Tokens.X509AsymmetricSecurityKey klassi. Muul juhul visatakse järgmine erand:

IDX10630: System.IdentityModel.Tokens.X509AsymmetricSecurityKey allkirjastamiseks ei saa olla väiksem kui "2048" bittide. 

## <a name="the-completed-system-and-test"></a>Lõpetatud süsteem ja test

Me juhendab mõned stsenaariumid lõplikus lõpuni süsteemis nii, et lugejad on põhilised "pilt" käitumine enne sisselogimist konto.

Veebirakenduse mängija ja selle login leiate [siit](https://openidconnectweb.azurewebsites.net/).

Kui vajate on "integreeritud" stsenaarium: video varad majutatud Azure Media Servicesi, mis on kas kaitsmata või DRM kaitstud, kuid ilma Turbeloa autentimine (välja litsentsi kes taotlev), seda testida ilma login (, saate aktiveerida HTTP, kui teie video voogesitus on http kaudu).

Kui vajate on lõpuni integreeritud stsenaarium: video varad on dünaamiline DRM kaitse Azure Media Servicesi, Turbeloa autentimist ja Azure AD loodud JWT luba alla, peate sisse logima.

### <a name="user-login"></a>Kasutaja sisselogimine

Selleks, et testida lõpuni integreeritud DRM süsteemi, peate "konto" loodud või lisatud. 

Millist kontot?

Kuigi Azure'i algselt lubatud juurdepääs ainult Microsofti konto kasutajate poolt, võimaldab nüüd nii kasutajate juurdepääsu. Seda tehti kõik Azure atribuudid usalda Azure AD autentimise Azure AD autentida organisatsiooni kasutajad, kellel on ja luua seose federation kus Azure AD loodab Microsofti konto tarbija identiteedi süsteem tarbija kasutajate autentimiseks. Selle tulemusena Azure AD on võimalik autentida "külaline" Microsofti kontot kui ka "kohalikke" Azure AD kontod.

Kuna Azure AD loodab Microsofti konto (MSA) domeen, saate lisada mis tahes järgmised domeenid kontodest kohandatud Azure AD rentniku ja logida selle kontoga:

**Domeeninimi**|**Domeeni**
-----------|----------
**Kohandatud Azure AD rentniku domeeni**|somename.onmicrosoft.com
**Ettevõtte Domeen**|saidi Microsoft.com
**Microsofti konto (MSA) Domeen**|Outlook.com, live.com, hotmail.com

Võib-olla mis tahes autorid on loodud või lisatud, saate konto võtke ühendust. 

Allpool on kuvatõmmised erinevate login lehed kasutavad domeenil kontod.

**Kohandatud Azure AD rentniku domeenikonto**: sel juhul näete, kohandatud kohandatud sisselogimislehe Azure AD rentniku domeeni.

![Kohandatud Azure AD rentniku domeeni kontoga](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Kiipkaardi domeeni Microsofti kontoga**: sel juhul näete kahekordne autentimine see Microsoft ettevõtte kohandatud sisselogimise lehele.

![Kohandatud Azure AD rentniku domeeni kontoga](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsofti konto (MSA)**: sel juhul näete sisselogimislehe Microsofti Account tarbijate jaoks.

![Kohandatud Azure AD rentniku domeeni kontoga](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Krüptitud meediumi laiendid kasutamise PlayReady

Kaasaegne brauser koos krüptitud Media laiendid (EME) PlayReady tuge, nt IE 11 opsüsteemis Windows 8.1 ja üles ja Microsoft Edge'i brauseri opsüsteemis Windows 10, saab PlayReady aluseks oleva DRM EME jaoks.

![EME kasutamise PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Tume Playeri ala on, et PlayReady kaitse takistab ühte teha ekraanipildi kaitstud video. 

Järgneval pildil on kuvatud Playeri lisandmoodulid ja MSE/EME tugi.

![EME kasutamise PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Microsoft Edge ja IE 11 opsüsteemis Windows 10 EME võimaldab kasutada, [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) Windows 10 seadmes, mis toetavad seda. PlayReady SL3000 avab täiustatud premium sisu voo (4K, HDR, jne) ja uus sisu kohaletoimetamise mudelite (täiustatud sisu ennetähtaegse aken).

Keskenduge Windowsi jaoks: PlayReady on ainult DRM saadaval Windowsi seadmetes (PlayReady SL3000) HW sisse. Streaming teenuse saate kasutada PlayReady EME või UWP rakenduse kaudu ja pakkuda videokvaliteeti PlayReady SL3000 kui teise DRM abil. Tavaliselt on 2K sisu flow Chrome'i või Firefoxi ja 4K sisu kaudu Microsoft Edge/IE11 või UWP rakenduse samasse seadmesse (sõltuvalt Teenusesätted ja juurutamine) kaudu.


#### <a name="using-eme-for-widevine"></a>EME kasutamise Widevine

Tänapäevane brauseri EME/Widevine tugi, nt Chrome'i 41 + Windows 10, Windows 8.1, Mac OSX Yosemite ja Chrome'i kohta Android 4.4.4, Google Widevine asub DRM EME taha.

![EME kasutamise Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Pange tähele, et Widevine ei takista ühte teha ekraanipildi kaitstud video.

![EME kasutamise Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Kasutajad ei õigus

Kui kasutaja pole "Kasutajad, kellel on õigus" rühma liige, kasutaja ei saa edasi "õiguse kontroll" ja mitme-DRM litsentsi teenuse keeldub probleemi nõutud litsents, nagu allpool näidatud. Üksikasjalik kirjeldus on "litsentsi hankimine nurjus", mis on ootuspäraselt.

![Un õigus kasutajad](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Kohandatud Secure Turbeloa teenus

Kohandatud Secure Turbeloa teenus (STS) käitamise stsenaariumi JWT luba väljastatava kohandatud STS kasutada sümmeetriline- või asümmeetriline. 

Kasutades sümmeetriline võti (Chrome'i kasutades) puhul:

![Kohandatud STS töötab](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Puhul abil asümmeetriline võti kaudu mõne X509 certificate (Microsoft tänapäevane brauseri abil).

![Kohandatud STS töötab](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Mõlema eeltoodud juhtudel kasutaja autentimise jääb samaks – Azure AD kaudu. Ainus erinevus on JWT sõned väljastab kohandatud STS Azure AD asemel. Muidugi dünaamilise CENC kaitse konfigureerimisel piirata litsentsi teenus määrab tüüpi JWT luba, kas sümmeetriline või asümmeetriline.

## <a name="summary"></a>Kokkuvõte

Selles dokumendis me arutada CENC native-DRM ja juurdepääsu juhtimiseks Turbeloa autentimise kaudu: selle kujundus ja rakendamise Azure, Azure Media Servicesi ja Azure Media Playeri abil.

- Viide kujundus on esitatud, mis sisaldab kõiki vajalikke komponente DRM/CENC alamsüsteemis;
- Viide rakendamise Azure, Azure Media Servicesi ja Azure Media Player.
- Mõned teemad ja rakendamist otseselt seotud, käsitletakse ka.


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Litsents 

William Zhang Mingfei Yan, Roland Le frangi, Kilroy Hughes, Julia Kornich
