<properties
    pageTitle="MFA Windows Server 2012 R2 AD FS Server | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas alustada Azure'i Mitmikautentimise ja Windows Server 2012 R2 AD FS-i."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Turvaline teie asutusesisese ja pilvepõhise ressursid AD FS Windows Server 2012 R2 Azure'i mitmekordne autentimise serveri kasutamine

Kui kasutate Active Directory Federation Services (AD FS) ja soovite turvaliste pilve või asutusesisese ressursse, saate konfigureerida Azure mitmekordne autentimine Server AD FS-i töötamiseks. Selle konfiguratsiooni käivitab kaheastmelise kontrollimise jaoks kõrge väärtusega lõpp-punktid.

Selles artiklis käsitleme AD FS Windows Server 2012 R2 Azure'i mitme teguriga autentimine serveri kasutamine. Lisateabe saamiseks lugege, kuidas [turvaline asutusesisese ja pilvepõhise](multi-factor-authentication-get-started-adfs-adfs2.md)ressursse Azure'i mitmekordne autentimise Server AD FS 2.0 abil.

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Turvaline Windows Server 2012 R2 AD FS Server Azure'i Mitmikautentimise

Azure'i mitmekordne autentimine serveri installimisel on teil järgmised suvandid.

- Installige Azure'i mitmekordne autentimine serveri kohalikult AD FS-i serveris
- Installige Azure'i Mitmikautentimise adapterit kohalik AD FS server ja seejärel installige mitmekordne autentimine serveri mõnes teises arvutis

Enne alustamist arvestage järgmist teavet:

- Te ei pea installida Azure mitmekordne autentimise Server AD FS-i serverisse. Siiski, peate installima Mitmikautentimise adapterit AD FS-i Windows Server 2012 R2, kus töötab AD FS-i. Saate installida serveri mõnes teises arvutis, kui see on toetatud versiooni ja installimist AD FS-i adapterit eraldi oma AD FS liiduserveri. Vaadake järgmisi toiminguid saate teada, kuidas funktsiooni adapterit eraldi installida.

- Kui loodud MFA Server AD FS-adapterit, on tõenäoline, et AD FS-i võib edastada osalise nimi on adapterit. Tuginedes tootja nime saab siis, kui rakenduse nimi. Kuid see muutus ei olevat puhul. Kui teie asutuses on kasutusel tekst- või mobiilirakenduses kinnitamine meetodid, sisaldavad määratletud ettevõtte sätted stringid kohatäidet, < $$*rakenduse_nimi*>. Selle kohatäite on asendatud automaatselt, kui kasutate AD FS-adapterit. Soovitame eemaldada kohatäite vastav stringid kui te secure AD FS-i.

- Konto, mida kasutate sisselogimiseks peab olema kasutajaõigused teenust Active Directory turberühmad loomiseks.

- Mitmikautentimise AD FS-adapterit, installimise viisardi turberühma nimega PhoneFactor administraatorid teie Active Directory eksemplar. Seejärel lisab federation teenuse AD FS-i teenuse konto sellesse rühma. Soovitame, et veenduda domeenikontrolleri, et PhoneFactor administraatorite rühma on tõepoolest loodud ja AD FS-i teenusekonto on selle rühma liige. Vajaduse korral lisada käsitsi PhoneFactor administraatorite rühma domeenikontrolleri AD FS-i teenuse konto.

- Kasutaja portaalis Web teenuse SDK installimise kohta teabe saamiseks lugege [kasutaja portaali Azure'i mitme teguriga autentimine serveri juurutamine.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Installige Azure'i mitmekordne autentimise Server kohalikult AD FS server

1. Laadige alla ja installige Azure'i mitmekordne autentimise Server oma AD FS server. Lisateavet lugege [Azure'i mitmekordne autentimise serveriga töötamise alustamine](multi-factor-authentication-get-started-server.md).
2. Azure'i mitmekordne autentimine serveri halduskonsoolis **AD FS-i** ikooni ja valige soovitud suvandid **Luba kasutaja registreerimise** ja **Luba kasutajatel valida meetod**.
3. Valige veel suvandeid soovite määrata oma ettevõtte jaoks.
4. Klõpsake nuppu **Installi AD FS-i adapterit**.
<center>![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Kui kuvatakse akna Active Directory, mis tähendab, et kaks asja. Teie arvuti on ühendatud domeeniga ja tagamiseks suhtlemine AD FS-i adapterit ja Mitmikautentimise teenus Active Directory konfiguratsioon on lõpetamata. Klõpsake nuppu **Järgmine** automaatselt selle konfiguratsiooni lõpetamiseks või valige soovitud **Active Directory automaatne konfigureerimine vahele jätta ja sätete konfigureerimine käsitsi** ruut ja klõpsake siis nuppu **edasi**.
6. Kui kohalik rühma windows ei kuvata, mis tähendab, et kahest asjaolust. Teie arvuti pole ühendatud domeeniga ja kohalike rühma konfiguratsioon kindlustada side adapterit AD FS-i ja Mitmikautentimise teenus on lõpetamata. **Järgmise** automaatselt selle konfiguratsiooni, või valimiseks klõpsake soovitud **kohalik jaotises Automaatne konfigureerimine vahele jätta ja sätete konfigureerimine käsitsi** ruut ja klõpsake siis nuppu **edasi**.
7. Installiviisardis, klõpsake nuppu **edasi**. Azure'i mitmekordne autentimine serveri loob PhoneFactor administraatorite rühma ja lisab AD FS-i teenuse konto PhoneFactor administraatorite rühma.
<center>![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. **Käivitage Installer** lehel klõpsake nuppu **edasi**.
9. Mitmikautentimise AD FS-adapterit, installer, klõpsake nuppu **edasi**.
10. Kui installimine on lõpule jõudnud, klõpsake **Sule** .
11. Kui soovitud adapterit on installitud, peate selle registreerima AD FS. Avage Windows PowerShell ja käivitage järgmine käsk:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Kasuta esimest korda registreeritud adapterit, redigeerida globaalne autentimise poliitika AD FS-i. Minge halduskonsoolis AD FS-i **Autentimist poliitika** sõlme. **Mitmikautentimise** jaotises linki **Redigeeri** **Globaalsätete** jaotise kõrval. Aknas **Globaalne autentimise poliitika redigeerimine** valige **Mitmikautentimise** täiendavad autentimise meetod ja seejärel klõpsake nuppu **OK**. Funktsiooni adapterit on registreeritud WindowsAzureMultiFactorAuthentication. AD FS-i registreerimiseks muudatuste jõustumiseks taaskäivitage.

<center>![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Selles etapis mitmekordne autentimise Server on seadistatud olla täiendavad autentimisteenuse pakkuja AD FS-i abil.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Installige eraldi eksemplari AD FS-i adapterit Web teenuse SDK abil
1. Installige Web teenuse SDK serveris, kus töötab mitme teguri autentimise Server.
2. Kopeerige järgmised failid on \Program Files\Multi-tegur autentimise Server directory AD FS-i adapterit installimiseks plaanite server:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Käivitage MultiFactorAuthenticationAdfsAdapterSetup64.msi installifail.
4. Mitmikautentimise AD FS-i installiprogrammi adapterit, klõpsake **järgmise** installimise alustamiseks.
5. Kui installimine on lõpule jõudnud, klõpsake **Sule** .

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>MultiFactorAuthenticationAdfsAdapter.config faili redigeerimine

MultiFactorAuthenticationAdfsAdapter.config faili redigeerimiseks tehke järgmist

1. **UseWebServiceSdk** sõlm väärtuseks **true**.  
2. Seadke **WebServiceSdkUrl** mitmekordne autentimine Web teenuse SDK URL-i jaoks. Näide: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, kus certificatename on teie serdi nimi.  
3. Redigeeri Register-MultiFactorAuthenticationAdfsAdapter.ps1 skripti lisamisega *- ConfigurationFilePath &lt;tee&gt; * lõppu on `Register-AdfsAuthenticationProvider` käsk, kus * &lt;tee&gt; * on MultiFactorAuthenticationAdfsAdapter.config faili täielik tee.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Kasutajanime ja parooliga Web teenuse SDK konfigureerimine

On kaks võimalust konfigureerida Web teenuse SDK. Esimene on kasutajanimi ja parool, teine kliendi sertifikaadiga. Tehke esimene variant või vahele jätta ees teise.  

1. Määramiseks väärtuse **WebServiceSdkUsername** kontoga, mis on PhoneFactor administraatorite turberühma liige. Kasutage funktsiooni &lt;domeeni&gt;& #92; &lt;kasutajanimi&gt; vorming.  
2. Määrake väärtuseks **WebServiceSdkPassword** jaoks sobiv konto parooli.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Kliendi serti ja SDK Web teenuse konfigureerimine

Kui te ei soovi kasutada kasutajanime ja parooli, tehke konfigureerida Web teenuse SDK kliendi serti.

1. Kliendi serdi hankima sertimisorgan server, kus töötab teenus Web SDK. Siit saate teada, kuidas [saada kliendi](https://technet.microsoft.com/library/cc770328.aspx).  
2. Kliendi sert importida serveris, kus töötab teenus Web SDK isikliku serdi kohalikku arvutisse salvestada. Veenduge, et sertimiskeskuse avaliku sert on usaldusväärsed juursertide serdi poest.  
3. Kliendi sert avalike ja privaatvõ klahvid eksportimine pfx-fail.  
4. Avalik võti Base64 vormingus eksportimine CER-fail.  
5. Server Manager, veenduge, et Web Server (IIS) \Web Server\Security\IIS kliendi serdi vastendamise autentimise funktsioon on installitud. Kui see on installitud, valige **rollide ja funktsioonide lisamine** , et lisada see funktsioon.  
6. IIS-i topeltklõpsake **Konfigureerimine redaktoris** veebisaidi, mis sisaldab Web teenuse SDK virtuaalse kausta. See on oluline selleks veebisaidi tasemel ja pole virtuaalne kaust tasemel.  
7. Liikuge jaotisse **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Määrata lubatud väärtuseks **true**.  
9. OneToOneCertificateMappingsEnabled väärtuseks **true**.  
10. OneToOneMappings kõrval nuppu **…** , ja seejärel klõpsake nuppu **Lisa** link.  
11. Avage Base64 CER-fail, mille te eksportisite varem. Eemaldage *---alustada serdi---*, *---lõpetamine serdi---*ja mis tahes reapiire. Kopeerige tulemuseks string.  
12. Eelmises etapis kopeeritud määramine serdi string.  
13. Määrata lubatud väärtuseks **true**.  
14. Määrake kasutajanimi, kontoga, mis on turvalisus PhoneFactor administraatorite rühma liige. Kasutage funktsiooni &lt;domeeni&gt;& #92; &lt;kasutajanimi&gt; vorming.  
15. Määra parool vastav konto parool ja seejärel sulgege konfiguratsiooni redaktor.  
16. Klõpsake nuppu **Rakenda** link.  
17. Topeltklõpsake kataloogis Web teenuse SDK virtuaalse **autentimist**.  
18. Veenduge, et ASP.net-i isikustamine ja Elementaarautentimine on seatud **lubatud**ja et kõigi muude üksuste on seatud **keelatud**.  
19. Topeltklõpsake kataloogis Web teenuse SDK virtuaalse **SSL-i sätted**.  
20. Määratud kliendi sertide **Aktsepteeri**ja seejärel klõpsake nuppu **Rakenda**.  
21. Kopeerige pfx-fail, mille te eksportisite varem serveris, kus töötab AD FS-i adapterit.  
22. Pfx-faili importida isikliku serdi kohalikku arvutisse salvestada.  
23. Paremklõpsake ja valige **Halda privaatvõtmete**ja anda seejärel selle kontoga, mida kasutasite AD FS-i teenuse sisse logida.  
24. Avage kliendi serti ja kopeerige soovitud sõrmejälje vahekaarti **üksikasjad** .  
25. Failis MultiFactorAuthenticationAdfsAdapter.config eelmises etapis kopeeritud määramine **WebServiceSdkCertificateThumbprint** string.  


Lõpuks registreerida soovitud adapterit, käivitage selle \Program Files\Multi-tegur autentimine Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShelli skripti. Funktsiooni adapterit on registreeritud WindowsAzureMultiFactorAuthentication. AD FS-i registreerimiseks muudatuste jõustumiseks taaskäivitage.

## <a name="related-topics"></a>Seotud teemad

Tõrkeotsingu kohta abi, lugege teemat [Azure mitmekordne autentimine KKK](multi-factor-authentication-faq.md)
