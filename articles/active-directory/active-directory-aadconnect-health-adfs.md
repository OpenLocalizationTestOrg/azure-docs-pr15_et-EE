
<properties
    pageTitle="Kasutades Azure AD seisund kasutajaga AD FS-i | Microsoft Azure'i"
    description="See on Azure'i AD-ühenduse seisundi lehel, kuidas jälgida oma kohapealse AD FS-i taristu."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Kasutades Azure AD AD FS-i seisundit kasutajaga
Järgmised dokumendid on seotud teie AD FS-i IT-taristu Azure'i AD-ühenduse seisundi jälgimine. Azure'i AD-ühenduse (Sünkrooni) jälgi Azure'i AD-ühenduse seisundi kohta leiate teavet teemast [abil Azure'i AD-ühenduse seisund sünkroonimise](active-directory-aadconnect-health-sync.md). Lisaks Active Directory domeeniteenused Azure'i AD-ühenduse seisundi jälgimine kohta lisateabe saamiseks vt [AD DS tervist abil Azure'i AD-ühenduse](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Teatiste AD FS-i
Azure'i AD-ühenduse seisund teatiste jaotises pakub teile aktiivse teatiste loendit. Iga teade sisaldab asjakohast teavet, eraldusvõime juhiseid ja lingid seotud dokumendid.

Topeltklõpsake aktiivne või lahendatud teatise, avada uus laba lisateabe saamiseks lahendamiseks teatise ja lingid asjakohased dokumendid tehtavad toimingud. Andmeid saate vaadata ka varem lahendatud teatised.

![Azure'i AD-ühenduse seisundi portaal](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Kasutusanalüüsi AD FS-i
Azure'i AD-ühendus seisund Kasutusanalüüsi analüüsib autentimise liikluse teie federation serverid. Topeltklõpsake kasutus analytics ruut kasutus analytics teravik, mis kuvatakse mitme mõõdikute ja rühmitustega avamiseks.

>[AZURE.NOTE] Kasutusanalüüsi AD FS-i kasutamiseks peab kontrollige, kas AD FS-i auditeerimine on lubatud. Lisateavet leiate teemast [Luba auditi AD FS-i](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Azure'i AD-ühenduse seisundi portaal](./media/active-directory-aadconnect-health/report1.png)

Valige täiendavad mõõdikute, määrata ajavahemiku või rühmitamist muuta, paremklõpsake kasutus analytics diagrammi ja valige Redigeeri diagrammi. Seejärel saate määrata ajavahemiku, valige muu mõõdiku ja rühmitamist muuta. Saate vaadata erinevate "mõõdikute" põhjal autentimine liiklus jaotuse ja rühma iga mõõdiku järgmises tabelis kirjeldatud parameetritega oluline "rühm,":

| Meetermõõdustik | Rühmitusalus | Mis on rühmitamise tähendab ja miks on kasulik? |
| ------ | -------- | -------------------------------------------- |
| Kogusumma taotleb: Koguarvu taotlusi töödelda federation teenus | Kõik | Kuvatakse ilma selleta tellimuste koguarvu arv. |
|  | Rakenduse | Rühmade põhinevad sihtarvutites osalise kokku taotlused. See rühmitus on kasulik mõista, mis võtab vastu palju kokku liikluse protsent. |
|  | Server | Rühmade kokku taotlused server, millega töötlemise taotluse põhjal. See rühmitus on kasulik mõista jaotus kokku liiklust. |
|  | Töökoha ühendamine | Rühmade kokku taotlusi vastavalt kas need on pärit seadmetes, mis on liitunud töökoha (ehk). See rühmitus on kasulik mõista, kui teie ressursid on juurdepääs tundmatud identiteedi taristu seadmete abil. |
|  | Autentimismeetodi määramine | Rühmade kokku taotluste autentimise meetodit kasutada autentimiseks. See rühmitus on kasulik mõista levinud autentimise meetodit, mida saab kasutada autentimist. Järgnevalt on võimalik autentimismeetodite <ol> <li>Windowsi integreeritud autentimine (Windows)</li> <li>Vormid vastavalt autentimine (vorm)</li> <li>SSO (ühekordse sisselogimise kohta)</li> <li>X509 serdi autentimine (sert)</li> <br>Kui liiduserveris koos mõne SSO küpsise kutse vastu võtta, loendatakse taotluse SSO (ühekordse sisselogimise). Sellisel juhul kui küpsis on lubatud, kasutajal palutakse sisestada mandaat ja saab sujuvalt juurdepääsu rakenduse. Selline käitumine on levinud, kui teil on mitu osalistele liiduserveris kaitstud. |
|  | Võrgukoht | Rühmade kokku taotlusi, mis põhineb kasutaja võrgukoht. See võib olla kas sise- või Suhtevõrgu. See rühmitus on kasulik teada liiklus protsendimäär on pärit sisevõrk või Suhtevõrgu. |
| Kokku nurjunud taotlused: Koguarv nurjus töödelda federation teenuse taotlused. <br> (Selle mõõdiku on saadaval ainult Windows Server 2012 R2 AD FS)| Tõrke tüüp | Kuvab arvu põhjal eelmääratletud tõrge tüüpi vigade. See rühmitus on kasulik mõista levinumat tüüpi vead. <ul><li>Vale kasutajanimi või parool: tõrgete tõttu vale kasutajanimi ja parool.</li> <li>"Suhtevõrgu blokeeringu": tõrgete tõttu taotlusi, mis on saadud kasutaja, mis on lukustatud: Suhtevõrgu </li><li> "Aegunud parooli": tõrgete tõttu kasutajad aegunud parooli sisse logimise.</li><li>"Keelatud konto": tõrgete tõttu keelatud kontoga kasutajatele.</li><li>"Seadme autentimise": tõrgete tõttu kasutajad ei autentida seadme autentimist kasutades.</li><li>"Kasutaja serdi autentimist": tõrgete tõttu kasutajad ei tõttu kehtetu sertifikaat autentida.</li><li>"MFA": tõrgete tõttu kasutaja ei Mitmikautentimise abil.</li><li>"Muude mandaatide": "Emissiooni autoriseerimine": tõrgete tõttu autoriseerimine ebaõnnestumist.</li><li>"Emissiooni delegeerimine": tõrgete tõttu emissiooni delegeerimine tõrkeid.</li><li>"Sümboolne nõusoleku": tõrgete tõttu ADFS-i lükake need tagasi luba kaudu kolmanda osapoole identiteedipakkuja juures.</li><li>"Protokoll": tõrke Protocol (protokoll) tõrgete tõttu.</li><li>"Tundmatu": püüdma kogu. Mis tahes tõrkeid, mis ei sobi määratletud kategooriad.</li> |
|  | Server | Rühmade põhjal Serveri tõrked. See rühmitus on kasulik mõista vigade jaotust serverites. Ebaühtlane jaotumine võib olla näitaja serveri vigase olekus. |
|  | Võrgukoht | Rühmade tõrgete alusel taotlusi (sisevõrk vs Suhtevõrgu) võrgukoht. See rühmitus on kasulik mõista taotlusi, mis ei suuda tüüp. |
|  | Rakenduse | Rühmade tõrkeid, mis põhinevad sihtarvutites rakendus (tuginedes pool). See rühmitus on kasulik, et aru saada, milline suunatud rakendus on näha kõige vigade arv. |
| Kasutajate arv: Keskmine arv kordumatu kasutajate süsteemi aktiivne | Kõik | Selle mõõdiku pakub keskmine arv valitud aja sektorit federation teenuse kasutamine kasutajate arv. Kasutajad on rühmitatud. <br>Keskmise sõltub valitud aja sektorit. |
|  | Rakenduse | Rühmade keskmine arv kasutajatele põhinevad sihtarvutites rakendus (tuginedes pool). Jaotus on kasulik vaadata, mitu kasutajat, on millised rakenduse abil. |


## <a name="performance-monitoring-for-ad-fs"></a>Jõudluse AD FS-i jälgimine
Azure'i AD-ühendus seisund jõudluse jälgimine annab mõõdikute jälgimisega seotud teavet. Ruudu jälgimis, avatakse uus laba mõõdikud üksikasjalik teave.


![Azure'i AD-ühenduse seisundi portaal](./media/active-directory-aadconnect-health/perf1.png)


Valides tera ülaservas suvand Filter, saate filtreerida serveri üksikute serveri mõõdikute kuvamiseks. Mõõdikute muutmiseks paremklõpsake jaotises jälgimisega seotud tera jälgimisega seotud diagrammi ja valige Redigeeri. Siis keelest uue avanenud, saate valida rippmenüüst täiendavad mõõdikute ja määrata ajavahemiku, tulemustega seotud andmete vaatamine.

## <a name="reports-for-ad-fs"></a>AD FS-i aruanded
Azure'i AD-ühendus seisund pakub aruandeid tegevuste ja AD FS-i jõudlus. Aruannete abil saavad administraatorid, ülevaate tegevuste oma AD FS-i serverites.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Nurjunud kasutajanime ja parooliga sisselogimise Top 50 kasutajad

Üks levinumad põhjused Nurjunud autentimine taotluse AD FS server on taotluse lubamatuid mandaadiga, mis on vale kasutajanimi või parool. Tavaliselt juhtub keerukate paroolide, unustatud parooli või Näpu tõttu.

Kuid on ka muid põhjusi, mis võivad põhjustada ootamatu arvu taotlusi, mida kasutavad oma AD FS server, näiteks: rakendus, mis vahemälu kasutajatunnust ja mandaadi aegumise või pahatahtlik kasutaja katsel logige sisse kontole tuntud paroolide seeriana. Need kaks näidet on kehtiv põhjust, mis võivad põhjustada taotlusi tõusu.

Azure'i AD-ühendus seisundit ADFS-i jaoks pakub aruande kohta ülemise 50 kasutajat nurjunud sisselogimise katsed tõttu sobimatu kasutajanimi ja parool. Aruande on saavutada töötlemine loodud AD FS-i serverid on parkides auditilogi sündmused

![Azure'i AD-ühenduse seisundi portaal](./media/active-directory-aadconnect-health-adfs/report1a.png)

Selles aruandes on teil järgmised osad teavet hõlpsalt juurde.

- Kokku # nurjunud taotluste viimase 30 päeva jooksul vale kasutajanime ja parooli abil
- Keskmine # kasutajate päevas halb kasutajanime ja parooliga sisselogimine nurjus.


See osa klõpsamine viib teid peamised aruande tera, mis pakub täiendavaid üksikasju. See blade sisaldab graafik trendid teavet, et aidata luua võrdlusalus taotluste vale kasutajanimi või parool. Lisaks pakub top 50 kasutajate loendit nurjunud katsete arv.

Graafik sisaldab järgmist teavet:

- Kokku # on nurjunud sisselogimise tõttu halb kasutajanime ja parooli päevas alusel.
- Kokku # kordumatu kasutajate sisselogimise päevas alusel nurjus.
- Kliendi IP-aadress viimase taotlus

![Azure'i AD-ühenduse seisundi portaal](./media/active-directory-aadconnect-health-adfs/report3a.png)

Aruanne sisaldab järgmist teavet:

| Aruande üksus | Kirjeldus
| ------ | -------- |
|Kasutaja-ID| Kuvatakse kasutaja ID, mida kasutati. See väärtus, mida kasutaja tipitud, mis mõnel juhul on vale kasutaja ID kasutusel.|
|Nurjunud katsete| Näitab kokku # nurjunud üritab selle kindla kasutaja ID-ga. Tabel on sorditud kõige laskuvas järjestuses nurjunud katsete arv.|
|Viimase tõrge| Näitab ajatempel, kui viimase tõrge ilmnes.
|Viimati tõrge IP | Kuvatakse Viimane vigane päring kliendi IP-aadressi.|



>[AZURE.NOTE] Seda aruannet värskendatakse automaatselt pärast selle aja jooksul kogutud teavet iga kahe tunni. Seetõttu võib sisselogimise katsete viimase kahe tunni jooksul kaasatud aruanne.



## <a name="related-links"></a>Seotud lingid

* [Azure'i AD-ühenduse seisund](active-directory-aadconnect-health.md)
* [Azure'i AD-ühenduse seisund agenti installi](active-directory-aadconnect-health-agent-install.md)
* [Azure'i AD-ühenduse seisund toimingud](active-directory-aadconnect-health-operations.md)
* [Azure'i AD-ühenduse seisundi kasutamise sünkroonimine](active-directory-aadconnect-health-sync.md)
* [Kasutades Azure AD kasutajaga AD DS seisund](active-directory-aadconnect-health-adds.md)
* [Azure'i AD-ühenduse seisund KKK](active-directory-aadconnect-health-faq.md)
* [Azure'i AD-ühenduse seisund versiooniajalugu](active-directory-aadconnect-health-version-history.md)
