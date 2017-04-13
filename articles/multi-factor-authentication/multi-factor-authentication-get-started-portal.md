<properties 
    pageTitle="Kasutaja portaali Azure'i mitmekordne autentimine serveri juurutamine"
    description="See on Azure mitmekordne autentimine leht, mis kirjeldab, kuidas alustada Azure'i MFA ja kasutajale portaali."
    services="multi-factor-authentication"
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

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Kasutaja portaali Azure'i mitmekordne autentimine serveri juurutamine

Kasutaja Portaal võimaldab installida ja konfigureerida Azure mitme teguri autentimist kasutaja portaalis administraator. Kasutaja portaal on IIS-i veebisait, mis võimaldab kasutajatel registreeruda Azure Mitmikautentimise ja hallata oma kontoga. Kasutaja võib muuta oma telefoninumber, muutke oma PIN-koodi või mööduda Azure'i Mitmikautentimise ajal oma järgmise Logi sisse.

Kasutajad oma tavaline kasutajanime ja parooli abil kasutaja portaali sisse logida ja on kas Azure'i Mitmikautentimise kõne lõpetamiseks või nende autentimine täitke turvalisuse küsimustele. Kui kasutaja registreerimise on lubatud, kasutaja konfigureerib oma telefoninumbri ja PIN-koodi esimest korda kasutaja portaali sisselogimine.

Kasutaja portaali administraatorid võib häälestamine ja uute kasutajate lisamine ja värskendamine olemasolevatele kasutajatele lubatud.

<center>![Häälestamine](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Portaali kasutaja autentimise Server Azure'i mitmekordne serveris juurutamine

Järgmised eeltingimused on vaja kasutajad portaali Server Azure'i mitmekordne autentimine serveris.

- IIS-i peab olema installitud, sh ASP.net-i ja 6 IIS-i metaandmete alus ühilduvus (IIS 7 või uuem)
- Sisse logitud kasutaja peab olema administraator õiguste arvuti ja domeeni vajaduse korral.  See on konto vajab õigusi Active Directory turberühmad loomiseks.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Juurutamiseks kasutaja portaali server Azure'i mitmekordne autentimine

1. Jooksul Azure mitmekordne autentimine Server: kasutaja portaali ikooni vasakpoolsest menüüst, installige kasutaja portaali nuppu.
1. Klõpsake nuppu edasi.
1. Klõpsake nuppu edasi.
1. Kui arvuti on ühendatud domeeni ja tagamiseks suhtlemine kasutaja portaali ja Azure Mitmikautentimise teenus Active Directory konfiguratsioon on lõpetamata, kuvatakse Active Directory etappi. Klõpsake selle konfiguratsiooni automaatselt lõpuleviimiseks nuppu edasi.
1. Klõpsake nuppu edasi.
1. Klõpsake nuppu edasi.
1. Klõpsake nuppu Sule.
1. Avage veebibrauser, mis tahes arvutist ja liikuge kuhu on installitud kasutaja portaali URL (nt https://www.publicwebsite.com/MultiFactorAuth). Veenduge, et pole serdi hoiatused ja vigade kuvamise.

<center>![Häälestamine](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Azure'i Mitmikautentimise Server kasutaja portaali eraldi serveri juurutamine

Azure'i mitmekordne autentimine rakenduse kasutamiseks on vajalik, et rakendus saab edukalt suhelda kasutaja portaali:

Lugege riist- ja tarkvaranõuete nõuded riist- ja tarkvara:

- Peate kasutama v6.0 või uuem versioon Server Azure'i mitme teguri autentimist.
- Kasutaja portaali peab olema installitud mõni Interneti-ühendusega web serveris Microsoft® Internet Information Services (IIS) 6.x IIS 7.x või hilisem versioon.
- Kui kasutate IIS-i 6.x, veenduge, et ASP.net-i v2.0.50727 on installitud, registreeritud ja määratud lubatud.
- Nõutav rolli teenuste kasutamisel IIS 7.x või suurem lisada ASP.net-i ja IIS 6 metaandmebaasi ühilduvuse.
- Kasutaja portaali peaks olema turvatud SSL-sert.
- Azure'i mitmekordne autentimine Web teenuse SDK peab olema installitud IIS-i 6.x IIS 7.x või hilisem versioon serveris, kus Server Azure'i mitmekordne autentimine on installitud.
- Azure'i mitmekordne autentimine Web teenuse SDK peavad olema kinnitatud koos SSL-sert.
- Kasutaja portaali peate saama ühendamiseks Azure'i mitmekordne autentimine Web teenuse SDK SSL.
- Kasutaja portaali peate saama autentida Azure'i mitmekordne autentimine Web teenuse SDK konto, on nimega "PhoneFactor administraatorid" Turberühma liikme mandaadi abil. Selle teenuse konto ja rühma Active Directorys kui Azure mitmekordne autentimine serveris domeeni ühendatud serveris. Selle teenuse konto ja rühma kohalikult serveris olemas Azure'i mitmekordne autentimine kui see pole ühendatud domeeniga.

Kasutaja portaali installimine peale Azure'i mitmekordne autentimise Server server nõuab kolme järgmist:

1. Veebiteenuse SDK installimine
2. Kasutaja portaali installimine
3. Azure'i Mitmikautentimise server portaali kasutajasätete konfigureerimine


### <a name="install-the-web-service-sdk"></a>Veebiteenuse SDK installimine

Kui Azure mitmekordne autentimine Web teenuse SDK serveris Azure'i mitmekordne autentimine pole juba installitud, millele ja avage Server Azure'i mitme teguri autentimist. Klõpsake ikooni Web teenuse SDK, klõpsake nuppu installida Web teenuse SDK... nupp ja esitatud juhiste järgi. Web teenuse SDK peavad olema kinnitatud koos SSL-sert. Iseallkirjastatud serdi sobib selleks, kuid see on "Trusted Root sertimiskeskuste" store veebiserver kasutaja portaali konto kohalikku arvutisse importida, et see on usaldada selle serdi alustamisel SSL-ühenduse.

<center>![Häälestamine](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Kasutaja portaali installimine

Enne installimist kasutaja portaali eraldi serveris, arvestage järgmist:

- See on kasulik veebiserverisse Interneti-ühendusega veebibrauseri avamiseks ja liikuge Web teenuse Tarkvaraarenduskomplektist, mis on sisestatud fail URL-i. Kui brauser pääseb veebiteenuse edukalt, palub teil mandaati. Sisestage kasutajanimi ja parool, mis olid sisestatud fail täpselt nii, nagu faili. Veenduge, et pole serdi hoiatused ja vigade kuvamise.
- Kui tulemüüri või puhverserveri tagant asukoha ette kasutaja portaali veebiserver ja läbimiseks SSL-i mahalaadimine, saate redigeerida kasutaja portaali fail ja lisage järgmised võti on <appSettings> jaotise nii, et kasutaja portaali http asemel https kasutada. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>Kasutaja portaali installimiseks

1. Azure'i mitmekordne autentimine Server Server Avage Windows Explorer ja liikuge kausta, kuhu on installitud Server Azure'i mitmekordne autentimine (nt C:\Program Files\Multi-tegur autentimise Server). MultiFactorAuthenticationUserPortalSetup installifail vastavalt vajadusele server, mis installitakse kasutaja portaali 32-bitine või 64-bitise versiooni valimine. Kopeerige installifail serveri Interneti-ühendusega.
2. Interneti-ühendusega veebiserverisse, tuleb faili setup käivitada administraatori õigused. Lihtsaim viis selleks on administraatorina käsuviiba avamiseks ja liikuge asukohta, kuhu installifail kopeeriti.
3. MultiFactorAuthenticationUserPortalSetup64 installi faili käivitada, saidi- ja Virtual Directory nime muutmine kui soovitud.
4. Pärast installimist kasutaja portaali, liikuge sirvides C:\inetpub\wwwroot\MultiFactorAuth (või sobiva kataloogi virtuaalse kausta nime järgi) ja redigeerida fail.
5. Leidke USE_WEB_SERVICE_SDK võti ja muutmine väärtuse false väärtuseks true. Leidke WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ja WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD võtmed ja väärtused määramine kasutajanimi ja parool on PhoneFactor administraatorid turvalisus teenuse konto (vt ülal jaotises nõuetele). Veenduge, et kasutajanimi ja parool, sisestage vahepeal rea lõpus jutumärke (väärtus = "" / >). Soovitatav on kasutada kvalifitseeritud kasutajanimi (nt DOMEEN\kasutajanimi või machine\username)
6. Otsige üles säte pfup_pfwssdk_PfWsSdk ja muutke väärtuse "http://localhost:4898/PfWsSdk.asmx" URL-i Web teenuse Tarkvaraarenduskomplektist, mis töötab Azure'i mitmekordne autentimise Server (nt https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Kuna SSL-i kasutatakse selle ühenduse, peab viide veebis teenuse SDK serveri nimi ja mitte IP-aadress, kuna SSL-sert on välja antud serveri nimi ja URL, mida kasutatakse peab vastama serdi nimi. Kui serveri nimi ei lahenda IP-aadressi Interneti-ühendusega serverist, kirje lisamine hosts-faili, et server Azure'i mitmekordne autentimine serveri nimi vastendamiseks IP-aadress. Salvestage fail pärast tehtud muudatustest.
7. Kui veebisaidi, et kasutaja portaali installiti jaotises (nt Vaikeveebisait) ei ole veel antud binded avalikult allkirjastatud sertifikaadiga, installige serdi server, kui ei juba installitud, avage IIS-i halduri ja siduda serdi veebisaidile.
8. Avage veebibrauser, mis tahes arvutist ja liikuge kuhu on installitud kasutaja portaali URL (nt https://www.publicwebsite.com/MultiFactorAuth). Veenduge, et pole serdi hoiatused ja vigade kuvamise.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Portaali kasutajasätete konfigureerimine server Azure'i mitmekordne autentimine
Nüüd, kui portaalis on installitud, peate konfigureerida Azure mitmekordne autentimine portaali töötamiseks.

Azure'i Mitmikautentimise server pakub mitmeid võimalusi kasutaja portaali.  Järgmises tabelis loetletakse ja need suvandid on seletamist, mida nad kasutavad.

Kasutaja videoportaali sätted|Kirjeldus|
:------------- | :------------- |
Kasutaja portaali URL| Saate sisestada URL, kus portaali majutatakse.
Esmane autentimine| Saate määrata autentimist kasutada portaali sisselogimisel tüüp.  Windows, Radius või LDAP autentimist.
Kasutajad saaksid sisse logida|Võimaldab kasutajatel sisestada kasutajanimi ja parool, klõpsake sisselogimislehel kasutaja portaali.  Kui see on valitud, ruudud tuhm.
Luba kasutaja registreerimine|Võimaldab kasutajal registreeruda mitmikautentimise võttes neid installikuva, mis palub need lisateavet nagu telefoninumber.  Küsi jaoks varukoopia telefoni võimaldab kasutajal määrata teisene telefoninumber.  Kolmanda osapoole Küsi VANDE luba võimaldab kasutajal määrata 3 tootja VANDE luba.
Luba kasutajatel One-Time Meediumierandi| See võimaldab kasutajatel ühekordse meediumierandi.  Kui kasutaja komplekti, mis viivad selle üles see ei mõjuta järgmine kord sisse logib kasutaja.  Küsi meediumierandi sekundi annab kasutajale boksi nii, et nad saavad muuta 300 sekundi vaikimisi.  Muul juhul ühekordse meediumierandi on ainult hea 300 sekundi.
Luba kasutajatel valida meetod| Võimaldab kasutajal määrata oma esmane kontakti meetod.  See võib olla telefonikõne, tekstsõnumi, mobiilirakenduse või VANDE luba.
Valige keel kasutajatele|  Võimaldab kasutajal muuta keelt, mida kasutatakse telefonikõne, tekstsõnumi või mobiilirakenduses VANDE luba.
Kasutajatele mobiilirakenduse aktiveerimine| Võimaldab kasutajatel luua aktiveerimise lõpuleviimiseks mobiilirakenduse aktiveerimisprotsessi, mida kasutatakse serveriga kood.  Samuti saate nad saavad seda aktiveerimine seadmete arv.  Vahemikus 1 kuni 10.
Fall turvaküsimused kasutamiseks|Võimaldab teil kasutada turvaküsimused juhuks, kui mitmikautentimise nurjub.  Saate määrata arv turvaküsimused, mis tuleb edukalt vastata.
Seostamiseks kolmanda osapoole VANDE Luba kasutajatel| Võimaldab kasutajal määrata kolmanda osapoole VANDE luba.
Fall VANDE luba kasutamiseks|Võimaldab kasutada ka VANDE luba juhul, kui selle mitmikautentimise ei õnnestu.  Saate määrata ka seansi ajalõpp minutites.
Logimise lubamine|Lubab kasutaja portaali sisse logida.  Logifailid asuvad: C:\Program Files\Multi-tegur autentimine Server\Logs.

Nende sätete enamik on nähtaval kasutaja, kui need on lubatud ja kasutajale märgid kasutaja portaali sisse.

![Kasutaja videoportaali sätted](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Kasutaja portaali sätete konfigureerimiseks server Azure'i mitmekordne autentimine




1. Azure'i mitmekordne autentimise Server, klõpsake ikooni kasutaja portaali. Klõpsake vahekaardil sätted Sisestage kasutaja portaali kasutaja portaali URL väljale URL. Seda URL-i saab lisada e-kirju, mis saadetakse kasutajad, kui need imporditakse Azure'i mitmekordne autentimise Server, kui e-posti funktsioonid on lubatud.
2. Valige sätted, mida soovite kasutada kasutaja portaalis. Näiteks kui kasutajad tohivad juhtida tema autentimismeetodite, veenduge, et kasutajatel on viis on möllitud koos meetodid, saate valida.
3. Abi mõistmine sätted kuvatakse paremas ülanurgas linki spikker.

<center>![Häälestamine](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Administraatorid menüü
Sellel vahekaardil lihtsalt võimaldab teil lisada kasutajad, kellel on administraatoriõigused.  Kui lisate administraator, saate häälestada õigusi, et nad saavad.  Sellisel viisil, võib olla kindel, et anda ainult vajalikud õigused administraator.  Klõpsake nuppu Lisa ja valige seejärel ja kasutajale ja nende õiguste ja seejärel klõpsake nuppu Lisa.

![Kasutaja videoportaali administraatorid](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Turvaküsimused
Sellel vahekaardil võimaldab teil määrata, mida kasutajad peavad anda vastused, kui valitud on taandepäringud suvandi Kasuta turvaküsimused turvaküsimused.  Azure'i mitmekordne teenindustasemeid Server on vaikimisi küsimusi, mida saate kasutada.  Saate muuta või lisada oma küsimusi.  Kui lisate oma küsimusi, saate seda keelt, mida soovite kuvada ka nende küsimus.

![Kasutaja portaali turvaküsimused](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Edastatud seansid

## <a name="saml"></a>SAML
Saate kasutaja portaali aktsepteerimiseks mõne identiteedipakkuja abil SAML taotluste häälestamise.  Saate määrata seansi ajalõpp, määrake kontrollimise serti ja Logi välja ümbersuunamise URL-i.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Usaldusväärsete IP-d
Sellel vahekaardil võimaldab teil määrata ühe IP-aadresside või IP-aadresside vahemikud, mida saab lisada nii, et kui kasutaja on ühest IP-aadresside sisse logida, siis mitmikautentimise on mööduda.

![Kasutaja portaali usaldusväärsete IP-d](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Kasutaja iseteenindusliku registreerumise
Kui soovite, et kasutajad saaksid sisse logida ja registreeruda peate märkige ruut Luba kasutajatel login ja luba kasutaja registreerimise suvandid. Pidage meeles, et teie mõjutab kasutaja sisselogimine.

Näiteks, kui kasutaja kasutaja portaali sisse ja klõpsab nuppu Logi sisse, need siis võetakse häälestuslehele Azure'i mitme teguri autentimist kasutaja.  Sõltuvalt sellest, kuidas olete konfigureerinud Azure'i Mitmikautentimise, on võimalik, et valida nende autentimise meetodit kasutaja.  

Kui need valida Kõneposti helistamine autentimise meetodit või olla eelnevalt konfigureeritud seda meetodit kasutada, palutakse lehe kasutajal sisestada oma esmane telefoninumber või laiend vajaduse korral.  Need võivad lubatud varukoopia telefoninumbri sisestamine.  

![Kasutaja portaali usaldusväärsed IP-d](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Kui kasutaja on vaja PIN-koodi kasutada, kui nad autentida, lehe küsib ka need PIN-koodi.  Kui olete sisestanud oma telefoni numbritega ja PIN-koodi (vajadusel), kasutaja klõpsab selle kõne mulle nüüd autentimise nupp.  Azure'i Mitmikautentimise täidab telefonikõne autentimist kasutaja esmase telefoninumbrile.  Kasutaja peab telefoni kõnele ja sisestage oma PIN-koodi (vajadusel) ja vajutage # liikumiseks ise registreerimise käigus järgmise juhisega.   

Kui kasutaja valib SMS teksti autentimise meetodit või on eelkonfigureeritud seda meetodit kasutada, palutakse lehe kasutaja jaoks oma mobiiltelefoninumber.  Kui kasutaja on vaja PIN-koodi kasutada, kui nad autentida, lehe küsib ka need PIN-koodi.  Kui olete sisestanud oma telefoninumbri ja PIN-koodi (vajadusel), kasutaja klõpsab soovitud teksti mulle nüüd autentimise nupp.  Azure'i Mitmikautentimise täidab SMS autentimise kasutaja mobiiltelefonile.  Kasutaja peab vastu SMS, mis sisaldab ühe-kellaaja-pääsukood (OTP) ja selle OTP pluss oma PIN-koodi sõnumi vastuse kui on rakendatav) liikumiseks ise registreerimise käigus järgmise juhisega.

![Kasutaja portaali SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Kui kasutaja valib mobiilirakenduse autentimise meetodit või on eelkonfigureeritud seda meetodit kasutada, palutakse lehe kasutaja oma seadmesse installida Azure Mitmikautentimise rakendus ja aktiveerimise koodi loomiseks.  Pärast installimist Azure Mitmikautentimise rakenduse kasutaja klõpsab nuppu Loo aktiveerimine kood.    

>[AZURE.NOTE]Azure'i Mitmikautentimise rakenduse kasutamiseks peab kasutaja lubamine Tõuketeatiste oma seadme.

Seejärel kuvatakse aktiveerimise koodi ja koos vöötkoodi pildi URL.  Kui kasutaja on vaja PIN-koodi kasutada, kui nad autentida, lehe küsib ka need PIN-koodi.  Kasutaja sisestab aktiveerimise koodi ja URL-i Azure Mitmikautentimise rakendusse või kasutab vöötkoodi skanner vöötkoodi pildi skannimiseks ja klõpsab nuppu Aktiveeri.    

Kui aktiveerimine on lõpule jõudnud, kuvatakse kasutaja klõpsab nuppu autentida, nüüd.  Azure'i Mitmikautentimise mõne autentida kasutaja mobiilirakenduse kaudu.  Kasutaja peate sisestama oma PIN-koodi (vajadusel) ja autentimise vajutamine nende mobiilirakenduse liikumiseks ise registreerimise käigus järgmise juhisega.  


Kui administraatorid konfigureerinud Azure'i mitmekordne autentimine serveri turvalisuse küsimuste ja vastuste kogumiseks, võetakse kasutaja turvaküsimused lehele.  Kasutaja peab valige neli turvaküsimused ja sisestage enda valitud küsimustele vastused.    

![Kasutaja portaali turvaküsimused](./media/multi-factor-authentication-get-started-portal/secq.png)  

Kasutaja ise registreerimine on lõppenud ja kasutajale on kasutaja portaali sisse logitud.  Kasutajad saavad uuesti sisse logida kasutaja portaali igal ajal tulevikus muuta nende telefoninumbrite, sõrmed, autentimismeetodeid ja turvaküsimused korral saavad administraatorid.
