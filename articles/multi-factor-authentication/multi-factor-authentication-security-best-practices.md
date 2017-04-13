<properties 
    pageTitle="Turvalisus kasutamise head tavad Azure'i MFA"
    description="Selles dokumendis on toodud heade tavade ümber Azure'i MFA Azure kontode kasutamine"
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
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Turvalisus kasutamise head tavad Azure'i Mitmikautentimise Azure AD kontode

Kui tegemist on teie autentimise protsessi täiustamine mitmikautentimise on enamiku organisatsioonide eelistatud valik. Azure'i mitme teguriga autentimine (MFA) võimaldab ettevõtetel nende Turve ja nõuetele vastavus nõuetele vastavuse tagades lihtsa sisselogimine oma kasutajate jaoks. Käesolevas artiklis hõlmab parimaid tavasid, mida peaks kaaluma Azure'i MFA vastuvõtmise kavandamisel.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Azure'i Mitmikautentimise pilveteenuses head tavad
Selleks, et kõik kasutajad pakkuda mitmikautentimise ja laiendatud funktsioonid, mis pakub Azure'i Mitmikautentimise soodustusi, peate lubamiseks Azure Mitmikautentimise kõik kasutajad.  Seda saab teha, kasutades ühte järgmistest:

- Azure'i AD Premium või ettevõtte mobiilsus
- Mitme teguri Auth pakkuja

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure'i AD Premium ettevõtte mobiilsus

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Litsentside määramine kasutajatele on soovitatav kõigepealt vastu Azure'i MFA Azure AD Premium või ettevõtte mobiilsus abil pilves.  Azure'i Mitmikautentimise on osa järgmisi komplektide ja sellisena teie asutus ei pea midagi täiendavad laiendamine mitmikautentimise võimalus kõigile kasutajatele.

Kui häälestamise Mitmikautentimise võtke arvesse järgmist:

- Pole vaja luua mitme teguri Auth pakkuja.  Azure'i AD Premium ja ettevõtte mobiilsus on Azure Mitmikautentimise.  Kui loote Auth pakkuja, mis võivad teile kahekordsete arve.
- Azure'i AD-ühendus on ainult nõue kui sünkroonite kohapealse keskkonna Active Directory on Azure AD Directory. Kui kasutate ainult Azure AD kataloogi, mis on sünkroonitud kohapealse Active Directory eksemplar, mida pole vaja Azure'i AD-ühenduse.


### <a name="multi-factor-auth-provider"></a>Mitme teguri Auth pakkuja

![Mitme teguri Auth pakkuja](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Kui teil on Azure AD Premium või ettevõtte mobiilsus siis soovitatav kõigepealt vastu Azure'i MFA pilveteenuses on MFA Auth osutaja loomiseks. Kuigi MFA on vaikimisi globaalne administraatoritele, kes on Azure Active Directory, kui juurutate MFA peate mitmikautentimise võimalus kõigile kasutajatele ja teha laiendada, et teil on vaja mitut tegurit Auth pakkuja ettevõtte jaoks saadaval.
Valides Auth pakkuja, peate valige kaust ja arvestage järgmist:

- Peate mõne Azure AD directory mitme teguri Auth pakkuja loomiseks.
- Peate on Azure AD directory mitme teguri Auth pakkuja seostada, kui soovite hõlmavad mitmikautentimise kõik kasutajad ja/või soovite oma globaalse administraatorid saama ära funktsioone nagu haldusportaali, kohandatud Tervitused ja aruandeid.
- DirSync või AAD Sync on ainult nõue, kui mõni Azure AD Directory kohapealse keskkonna Active Directory sünkroonimine. Kui kasutate ainult Azure AD kataloogi, mis on sünkroonitud kohapealse Active Directory eksemplar, mida pole vaja DirSync või AAD Sync.
- Kui teil on Azure AD Premium või ettevõtte mobiilsus, peate looma mitmekordne Auth pakkuja. Teil on vaja ainult kasutajale litsentsi määramine ja siis saate alustada sisselülitamine MFA kasutajate jaoks.
- Veenduge, et valida õige kasutus mudeli ettevõtetele (auth või lubatud kasutaja kohta), kui valite kasutus ei saaks seda muuta.

### <a name="user-account"></a>Kasutajakonto
Kui lubamine MFA kasutajate jaoks, kasutajakontode võib olla üks kolmest core: keelatud, lubatud "või" jõustatud.
Veenduge, et kasutate juurutamise jaoks sobivaim võimalus kasutada allpool toodud juhised.

- Kui kasutaja olek on seatud väärtusele keelatud, et kasutaja ei kasuta mitmikautentimise. See on vaikimisi olekus.
- Kui kasutaja olek on seatud väärtusele lubatud, see tähendab, et kasutajal on lubatud, kuid on lõppenud registreerimise käigus. Pakutakse lõpule viia veebisaidil järgmise Logi sisse. See säte ei mõjuta mitte brauseri rakendused. Kõik rakendused endiselt kasutada kuni registreerimise protsess on lõpule viidud.
- Kui kasutaja olek on seatud väärtusele jõustatud, tähendab see, et kasutaja võib või võib olla lõpetatud registreerimise. Kui need on täidetud registreerimise käigus siis nad kasutavad mitmikautentimise. Muul juhul palutakse kasutaja registreerimise lõpule veebisaidil järgmise Logi sisse. See säte mõjutab mitte brauseri rakendused. Need rakendused ei tööta, kuni rakenduse paroolid on loodud ja kasutada.
- Malli kasutaja teatise saadaval artiklist [Alustamine Azure'i Mitmikautentimise pilveteenuses](multi-factor-authentication-get-started-cloud.md) meilisõnumit saata kasutajatele MFA kasutuselevõtu kohta.

### <a name="supportability"></a>Klienditoe

Kuna enamik kasutajad on harjunud autentida ainult paroolide kasutamise, on oluline, et teie ettevõtte tuua teadlikkuse kõigile kasutajatele selle protsessi kohta. Selle teadlikkuse saate vähendada tõenäosus, et kasutajad helistab klienditoega seotud MFA väiksemaid probleeme.
Siiski on mõnel juhul, kui ajutiselt keelamine MFA on vajalik. Kasutage allpool toodud juhised mõistmaks, kuidas need stsenaariumid:

- Veenduge, et teie tehnilise toe töötajatele õpetatakse pidet stsenaariumid, kus mobiilirakenduse või telefoni ei saanud teatist või telefonikõne ajal ja seetõttu kasutaja ei saa sisse logida. Need saate lubada kasutajal autentida ühel ajal "mööda" mitmikautentimise ühekordse meediumierandi võimalus. Funktsiooni meediumierandi on ajutine ja lõpeb pärast määratud sekundite arv.
- Vajaduse korral saate kasutada usaldusväärsed IP-d võimalus Azure'i MFA sisse. See funktsioon võimaldab hallatavate või ühendatud rentniku administraatorite võimalus mööduda mitmikautentimise kasutajatele, kellel on ettevõtte kohaliku sisevõrgu sisse logida. Funktsioonid on saadaval Azure AD rentnikud, mis on Azure AD Premium, ettevõtte mobiilsus või Azure'i Mitmikautentimise litsentse.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Azure'i Mitmikautentimise kohapealse head tavad
Kui teie ettevõte otsustanud kasutada oma taristu lubamiseks MFA, tuleb juurutada Azure mitmekordne autentimine serveri kohapealses. MFA serveri komponendid on näidatud alloleval joonisel:

![Mitme teguri Auth pakkuja](./media/multi-factor-authentication-security-best-practices/server.png)
*ei installita vaikimisi ** installitud, kuid vaikimisi pole lubatud


Azure'i mitmekordne autentimine Server saab kasutada secure cloud materjale ja kohapealse ressursse, mis on Azure AD kontod.  Kuid see võib ainult veebiteenuste federation abil.  Seega peate on AD FS-i ja on see teie Azure AD rentniku domeeniliidu.
Kui seadistamine mitme teguriga autentimine Server võtke arvesse järgmist:

- Kui olete turvaliseks Azure AD ressursid Active Directory Federation Services abil 1st teguri autentimine on läbi ja seejärel kohapealsete AD FS-i abil ja 2 dispersioonanalüüs on tehtud asutusesisese austus nõude.
- See ei ole installitud Server Azure'i mitmekordne autentimine oma AD FS liiduserveri, aga mitmekordne autentimine adapterit AD FS-i peab olema installitud Windows Server 2012 R2 AD FS-i töötab nõue. Saate installida server mõnes teises arvutis, kui see on toetatud versiooni ja AD FS-i adapterit oma AD FS liiduserveri eraldi installida. Vaadake allpool juhised selle adapterit eraldi installimist.
- Mitmikautentimise AD FS-i adapterit installimise viisard loob tuntud PhoneFactor administraatorid teie Active Directory turberühma ja lisab seejärel tulemile arvu federation teenuse AD FS-i teenuse konto sellesse rühma. On soovitatav, et veenduda domeenikontrolleri, et PhoneFactor administraatorite rühma on tõepoolest loodud ja AD FS-i teenusekonto on selle rühma liige. Vajaduse korral lisada AD FS-i teenuse konto PhoneFactor administraatorite rühma domeenikontrolleri käsitsi.

### <a name="user-portal"></a>Kasutaja portaal
See portaal töötab Internet Information Server (IIS) veebisait, mis võimaldab iseteenindusliku võimalusi pakub täielikku komplekti kasutaja halduse võimaluste. Allpool toodud juhised abil saate konfigureerida selle osa:

- IIS 6 või suurem on nõutav
- ASP.net-i v2.0.507207 peavad olema installitud ja registreeritud
- See server saab kasutada perimeetri võrgus.



### <a name="app-passwords"></a>Rakenduse paroolid
Kui teie asutuses on ühendatud, kasutades Azure AD SSO ja te ei kavatse kasutada Azure MFA, siis pidage silmas järgmist rakenduse paroolid kasutamisel (Pidage meeles, et see kehtib ainult ühendatud (SSO) kasutatakse):

- Rakenduse parool on kinnitatud Azure AD ja seetõttu mille liiklus möödub federation. Federation kasutatakse ainult rakenduse parooli häälestamisel.
- Ühendatud (SSO) salvestatakse kasutajate paroole ettevõtte ID-d. Kui kasutaja lahkub ettevõtte, on seda teavet organisatsiooni id abil DirSync reaalajas jätkata. Konto keelata/kustutamine võib kuluda kuni 3 tundi sünkroonimiseks edasi lükata keelata/kustutamise rakenduse parooli ja Azure AD.
- Kohapealse kliendi juurdepääsu reguleerimine sätted on au rakenduse parooli abil
- Kohapealse Autentimiseta logimine ja videovõimaluste kontrollimine on saadaval rakenduse parooli
- Veel lõppkasutaja õppurite jaoks on vaja Microsoft Lync 2013 kliendi.
- Teatud täpsemate arhitektuuri kujunduse võib olla nõutav, kasutades kombinatsiooni ettevõtte kasutajanime ja parooli ja rakenduse paroolid klientidega, olenevalt sellest, kus need autentida mitmikautentimise kasutamisel. Kohapealse infrastruktuur autentimiseks klientide puhul kasutaks mõnda ettevõtte kasutajanime ja parooliga. Azure AD autentimiseks klientide puhul kasutage rakenduse parool.
- Vaikimisi kasutajad ei saa luua rakenduse parooli, kui teie ettevõte eeldab, et või kui soovite lubada kasutajatel rakenduse parooli loomine mõnel juhul veenduge, et valitud on suvand Luba kasutajatel luua rakenduse paroolid sisselogimise-brauseri rakendustesse.

## <a name="additional-considerations"></a>Täiendavad kaalutlused
Järgmine loend abil saate ülevaate mõned täiendavad kaalutlused ja head tavad iga komponendi, mis on juurutatud asutusesisese:

Meetod|Kirjeldus
:------------- | :------------- |
[Teenuse Active Directory Federation](multi-factor-authentication-get-started-adfs.md)|Azure'i Mitmikautentimise AD FS häälestamise teave.
[RADIUS teenindustasemeid](multi-factor-authentication-get-started-server-radius.md)|  Teave häälestamise ja konfigureerimise Server Azure'i MFA raadiusega.
[IIS-i autentimine](multi-factor-authentication-get-started-server-iis.md)|Teave häälestamise ja konfigureerimise abil IIS-i Server Azure'i MFA.
[Windowsi teenindustasemeid](multi-factor-authentication-get-started-server-windows.md)|  Teave häälestamise ja konfigureerib Azure'i MFA serveri Windowsi autentimist.
[LDAP autentimine](multi-factor-authentication-get-started-server-ldap.md)|Teave häälestamise ja konfigureerimise Server Azure'i MFA LDAP autentimise.
[Remote lüüsi ja Azure mitme teguriga autentimine serveri RADIUS abil](multi-factor-authentication-get-started-server-rdg.md)|  Teave häälestamise ja konfigureerib Azure'i MFA serveri Remote lüüsi RADIUS abil.
[Windows Serveri Active Directory sünkroonimine](multi-factor-authentication-get-started-server-dirint.md)|Teave häälestamise ja konfigureerimise Active Directory ja Azure MFA serveriga sünkroonimiseks.
[Azure'i Mitmikautentimise Server Mobile'i rakendus veebiteenuse juurutamine](multi-factor-authentication-get-started-server-webservice.md)|Häälestamise ja konfigureerimise veebiteenuse Azure'i MFA serveri teave.
[Azure'i Mitmikautentimise täpsemalt VPN-i konfigureerimine](multi-factor-authentication-advanced-vpn-configurations.md)|Teave Cisco ASA, Citrix Netscaler ja kadakas/Pulse'i Secure VPN seadmete LDAP või RADIUS konfigureerimise kohta.


## <a name="additional-resources"></a>Lisaressursid
Selles artiklis tõstab esile parimaid tavasid Azure'i MFA jaoks, on muud ressursid, mida saate kasutada ka oma MFA juurutuse kavandamisel. Järgmises loendis on mõned peamised artiklitele, mis aitavad teil selle käigus.

- [Azure'i Mitmikautentimise aruanded](multi-factor-authentication-manage-reports.md)
- [Azure'i Mitmikautentimise häälestamine kogemus](multi-factor-authentication-end-user-first-time.md)
- [Azure'i Mitmikautentimise KKK](multi-factor-authentication-faq.md)
