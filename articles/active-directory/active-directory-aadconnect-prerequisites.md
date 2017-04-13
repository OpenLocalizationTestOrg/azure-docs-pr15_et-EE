<properties
   pageTitle="Azure'i AD-ühendus: Eeltingimused ja riistvara | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse selle eeltingimused ja Azure'i AD-ühenduse riistvaranõuded"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Eeltingimuste Azure AD ühenduse loomine
Selles teemas kirjeldatakse selle eeltingimused ja riistvaranõuded Azure'i AD-ühenduse.

## <a name="before-you-install-azure-ad-connect"></a>Enne installimist Azure'i AD-ühenduse
Enne installimist Azure'i AD-ühenduse, on mõned asjad, mida teil on vaja.

### <a name="azure-ad"></a>Azure AD
- Azure'i tellimuse või on [Azure prooviversiooni tellimuse](https://azure.microsoft.com/pricing/free-trial/). See kehtib ainult nõutav juurdepääs Azure'i portaalis, mitte Azure'i AD-ühenduse kaudu. Kui kasutate PowerShelli või Office 365 tellimuse Azure kasutada Azure'i AD-ühenduse pole vaja. Kui teil on Office 365 portaalis saate kasutada ka Office 365 litsentsi. Tasulist Office 365 litsentsi abil saate avada ka Azure portaali sisse Office 365 portaali kaudu.
    - Samuti saate Azure AD eelvaate funktsioon [Azure portaali](https://portal.azure.com). See portaal ei nõua on Azure litsents.
- [Lisamine ja kinnitada domeeni](active-directory-add-domain.md) kavatsete kasutada Azure AD. Näiteks kui kavatsete kasutada contoso.com kasutajate jaoks, siis veenduge, et see domeen on kinnitatud ja te ei kasuta ainult contoso.onmicrosoft.com vaikedomeeni.
- Mõni Azure AD rentniku võimaldab vaikeväärtus 50k objektid. Kui domeeni kinnitamine, on limiit suurendada 300 k objektid. Kui teil on vaja rohkem objektide Azure AD peate olema suurem veelgi limiit esitamist avama. Kui vajate rohkem kui 500 k objektid, siis peate litsentsi, nt Office 365, Azure AD Basic, Azure AD Premium või ettevõtte mobiilsus.

### <a name="prepare-your-on-premises-data"></a>Teie kohapealse andmete ettevalmistamine
- [Valikuline sünkroonimine funktsioone, saate lubada Azure AD](active-directory-aadconnectsyncservice-features.md) ja hindab lubage funktsioonid.

### <a name="on-premises-servers-and-environment"></a>Kohapealsed serverid ja keskkonnas
- AD skeemi versiooni ja mets otstarbekas peab olema Windows Server 2003 või uuemat versiooni. Domeenikontrollerid saate kasutada mis tahes versiooni, kui skeemi ja mets taseme nõuded on täidetud.
- Kui kavatsete kasutada funktsiooni **parooli tagasikirjutusega** domeenikontrollerid peab olema Windows Server 2008 (koos uusima SP) või uuem versioon. Kui teie DCs on 2008 (eel-R2), siis peate ka [värskenduse KB2386717](http://support.microsoft.com/kb/2386717)rakendamist.
- Selle domeenikontrolleri kasutatavaid Azure AD tuleb kirjutatav. Ei toetata kasutada RODC (kirjutuskaitstud domeenikontrolleri) ja Azure'i AD-ühenduse järgige mis tahes kirjutamine ümbersuunamisi.
- Azure'i AD-ühendus, ei saa installida Small Business Server või Windows Server Essentials. Server kasutama Windows Server standard- või parem.
- Azure'i AD-ühenduse server peab olema installitud täielik GUI. Ei toetata server core installida.
- Azure'i AD-ühendus peab olema installitud Windows Server 2008 või uuema versiooniga. See server võib olla domeenikontrolleri või liige server, kui kiire sätete abil. Kui kasutate kohandatud sätteid, server võib olla ka eraldiseisev ja ei pea olema ühendatud domeeniga.
- Kui installite Windows Server 2008 Azure'i AD-ühenduse, veenduge, et rakendada Viimane käigultparandused Windows Update. Installi ei saa alustada unpatched serveriga.
- Kui kavatsete kasutada funktsiooni **parooli sünkroonimise**, tuleb Azure'i AD-ühenduse serveriga Windows Server 2008 R2 SP1 või uuem versioon.
- Azure'i AD-ühenduse server peab olema [.NET Frameworki 4.5.1](#component-prerequisites) või uuem versioon ja [Microsoft PowerShelli 3.0](#component-prerequisites) või uuem versioon on installitud.
- Kui Active Directory Federation Services juurutatakse, kuhu on installitud AD FS-i või Web rakenduse puhverserveri serverid peab olema Windows Server 2012 R2 või uuem versioon. [Windowsi kaughalduse](#windows-remote-management) peab olema lubatud remote installi nendes serverites.
- Kui Active Directory Federation Services juurutatakse, peate [SSL-sertide](#ssl-certificate-requirements).
- Kui Active Directory Federation Services juurutatakse, siis peate [nimelahendus](#name-resolution-for-federation-servers)konfigureerimiseks.
- Azure'i AD-ühendus nõuab SQL serveri andmebaasi identiteedi andmete talletamiseks. Vaikimisi on SQL Server 2012 kiire LocalDB (liht versiooni SQL Server Express) on installitud ja teenusekonto teenuse luuakse kohalikus arvutis. SQL Server Express on 10GB mahupiirang, mis võimaldab teil hallata ligikaudu 100 000 objektid. Kui teil on vaja haldamine suurem maht directory objekti, peate mõne muu installi SQL Serveri installimise viisard osutamiseks.
- Kui kasutate eraldi SQL Server, siis kehtivad nendele nõuetele.
    - Azure'i AD-ühendus toetab kõik maitsed Microsoft SQL serveri SQL Server 2008 (koos SP4) SQL serveri 2014. Microsoft Azure'i SQL-andmebaasi **ei** toetata andmebaas.
    - Kasutage väiketähed SQL-sortimine. Need on tähistatud on \_CI_ tema nime. See on tõstutundlik võrdlemine, mis on tähistatud kasutama **ei toetata** \_CS_ tema nime.
    - Teil on ainult üks sünkroonimine mootori eksemplari. See on andmebaasi eksemplari ühiskasutusse FIM/MIM sünkroonimine, DirSync või Azure AD sünkroonimine **ei toetata** .

### <a name="accounts"></a>Kontod
- Konto Azure AD üldadministraator Azure AD kataloogi, mida soovite integreerida. See peab olema **kooli või ettevõtte konto** ja veel **Microsofti kontot**ei saa.
- Kui kasutate kiirsätteid või DirSync uuendada, siis peab olema ettevõtte administraatori konto teie kohaliku Active Directory jaoks.
- Kui kasutate kohandatud sätteid Installitee [Active Directory kontod](./connect/active-directory-aadconnect-accounts-permissions.md) .

### <a name="azure-ad-connect-server-configuration"></a>Azure'i AD-ühendus serveri konfigureerimine
- Kui teie globaalse administraatorid on MFA lubatud, siis URL-i **https://secure.aadcdn.microsoftonline-p.com** peab olema usaldusväärsete saitide loendist. Teil palutakse see usaldusväärsete saitide loendisse lisada, kui see on lisatud, enne kui teilt küsitakse on MFA ülesanne. Internet Exploreri abil saate lisada oma usaldusväärsete saitide loendisse.

### <a name="connectivity"></a>Ühenduvus
- Azure'i AD-ühenduse server peab DNS-i resolvimise sise-ja internet. DNS-i server tuleb lahendada nimed nii oma kohapealse Active Directory kui ka Azure AD lõpp-punktid.
- Kui teil on vaja tulemüürides sisevõrgu ja peate avatud pordid Azure'i AD-ühenduse serverid ja oma domeenikontrollerid vahel, siis leiate [Azure'i AD-ühenduse pordid](active-directory-aadconnect-ports.md) lisateavet.
- Kui teie puhverserveri või tulemüüri limiit millele pääseb URL-id, siis URL-id, [Office 365 URL-id ja IP-aadresside vahemikud](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) dokumenteerida tuleb avada.
    - Kui kasutate Microsoft Cloud Saksamaa või Microsoft Azure Governmenti pilves, siis vt [Azure'i AD-ühenduse sünkroonimise teenuse eksemplari kaalutlused](active-directory-aadconnect-instances.md) URL-id.
- Azure'i AD-ühendus on vaikimisi TLS 1.0 abil saate suhelda Azure AD. Saate muuta selle TLS 1.2 [lubamine TLS 1.2 Azure'i AD-ühenduse](#enable-tls-12-for-azure-ad-connect)juhiseid järgides.
- Kui kasutate väljamineva liikluse puhverserveri Interneti-ühenduse, tuleb lisada järgmise sätte **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** faili installimise viisard ja Azure'i AD-ühenduse sünkroonimine saama ühenduse Interneti- ja Azure AD. See tekst tuleb sisestada faili allosas. Selle koodi &lt;PROXYADRESS&gt; tähistab tegelik puhverserveri IP aadress või hosti nimi.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Kui teie puhverserveri server nõuab autentimist, [teenusekonto](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) peab asuma domeeni ja peate kasutama kohandatud sätted Installitee Määrake [kohandatud teenusekonto](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components). Samuti vajate eri muudatuse fail "Machine.config". See muutus machine.config installimise viisard ja sünkroonimine mootor vastata taotluste autentimise puhverserveri. Kõik installimise viisard kasutatakse lehtede, välja arvatud lehe **konfigureerimine** allkirjastatud kasutaja mandaat. Installimise viisard lõpus lehe **konfigureerimine** lülitatakse [teenusekonto](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) teie poolt loodud kontekstis. Jaotises fail "Machine.config" peaks välja nägema järgmine.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

[Vaikimisi puhverserveri elemendi](https://msdn.microsoft.com/library/kd3cf2ex.aspx)kohta lisateavet teemast MSDN-i.

Kui teil on probleeme ühenduvust, vaadake [ühendusprobleemide tõrkeotsing](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Muud
- Valikuline: Testi kasutajakonto sünkroonimise kinnitamiseks.

## <a name="component-prerequisites"></a>Komponendi eeltingimused

### <a name="powershell-and-net-framework"></a>PowerShell ja .net Framework
Azure'i AD-ühendus sõltub Microsoft PowerShelli .NET Frameworki 4.5.1. Peate selle versiooni või uuem versioon serverisse installitud. Sõltuvalt teie Windowsi serveri versiooni, tehke järgmist.

- Windows Serveri 2012R2
  - Vaikimisi installitakse Microsoft PowerShelli, enam pole vaja midagi.
  - .NET Frameworki 4.5.1 ja uuemad versioonid väljalasete on saadaval Windows Update'i kaudu. Veenduge, et uusimad värskendused on installitud Windows Server juhtpaneeli.
- Windows Serveri 2008R2 ja Windows Server 2012
  - Microsoft PowerShelli uusim versioon on saadaval **Windows Management Framework 4.0**saadaval [Microsofti allalaadimiskeskusest](http://www.microsoft.com/downloads).
  - .NET Frameworki 4.5.1 ja uuemad versioonid väljalasete on saadaval [Microsofti allalaadimiskeskusest](http://www.microsoft.com/downloads).
- Windows Server 2008
  - PowerShelli Viimane toetatud versioon on saadaval **Windows Management Framework 3.0**, saadaval [Microsofti allalaadimiskeskusest](http://www.microsoft.com/downloads).
 - .NET Frameworki 4.5.1 ja uuemad versioonid väljalasete on saadaval [Microsofti allalaadimiskeskusest](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Luba TLS 1.2 jaoks Azure'i AD-ühenduse
Azure'i AD-ühendus kasutab vaikimisi TLS 1.0 krüptimiseks suhtlemine sünkroonimine engine server ja Azure AD. .Net-i rakendusi kasutada TLS 1.2 serveris vaikimisi konfigureerimise abil saate seda muuta. Lisateavet TLS 1.2 leiate [Microsofti turvalisus nõu 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. Windows Server 2008 ei saa lubada TLS 1.2. Teil on vaja Windows Server 2008R2 või uuem versioon. Veenduge, et on installitud opsüsteem .net 4.5.1 kiirparandus kohta leiate teavet artiklist [Microsoft turvalisus nõu 2960358](https://technet.microsoft.com/security/advisory/2960358). Teil võib see või uuem väljaanne, mis on juba serverisse installitud.
2. Kui kasutate Windows Server 2008R2, siis veenduge, et TLS 1.2 on lubatud. Windows Server 2012 server ja uuemad versioonid, juba olema lubatud TLS 1.2.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Kõigis opsüsteemides, määrake see registrivõti ja uuesti serverisse.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Kui ka soovite lubada TLS 1.2 sünkroonimine engine serveri ja serveri SQL Server, siis veenduge, et teil on installitud [Microsoft SQL serveri](https://support.microsoft.com/kb/3135244)tugi TLS 1.2 nõutav versioonid.

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Eeltingimuste federation installimine ja konfigureerimine

### <a name="windows-remote-management"></a>Windowsi kaughalduse
Azure'i AD-ühenduse abil saate juurutada Active Directory Federation Services või teenuserakenduse puhverserverit, märkige ruut nõuded allpool tagada ühendus ja konfigureerimine õnnestub.

- Kui sihtkoht server on ühendatud domeeni, kontrollige, kas Windows Serveri hallatavate on lubatud
    - Laiendatud PSH käsuaknas käsuga`Enable-PSRemoting –force`
- Kui sihtkoht server on-domeeni ühendatud WAP arvuti, on paar lisanõuded
    - Sihtarvutis (WAP kohapeal):
         - Veenduge, et winrm (Windowsi kaughalduse / oli-halduse) teenus töötab kaudu teenuste lisandmoodul
         - Laiendatud PSH käsuaknas käsuga`Enable-PSRemoting –force`
    - Arvutis, kus viisardi töötab (kui sihtarvutis on-domeeni ühendatud või ebausaldusväärsete Domeen):
        - Laiendatud PSH käsu aknas, kasutage käsku`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - Server Manager:
             - DMZ WAP hosti arvuti ühendada lisamine (server manager -> Halda -> Lisa serverid... vahekaardil DNS-i)
             - Server Manager kõik serverid menüü: Paremklõpsake WAP-serveri ja valige Halda AS..., sisestage kohalik (mitte Domeen) creds WAP arvutisse
             - Valideerimiseks PSH remote connectivity Server Manager kõik serverid vahekaardil: Paremklõpsake WAP-serveri ja valige käsk Windows PowerShelli.  PSH kaugseansi peaksite avama tagada PowerShelli kaugseansi saab kindlaks teha.

### <a name="ssl-certificate-requirements"></a>SSL-i serdi nõuded
**Tähtis:** tungivalt soovitatav on kasutada sama SSL-serdi üle kõik sõlmed serveripargi AD FS-i kui ka kõik veebirakenduse puhverserverite.

- Serdi peab olema ka X509 sert.
- Saate kasutada iseallkirjastatud serdi liiduserverite lab testimiskeskkonnas. Siiski tootmiskeskkonna, soovitame serdi hankima avaliku CA.
    - Kui serdiga, mis pole avalikult usaldusväärne, veenduge, et iga Web rakenduse puhverserveri serverisse installitud serdi on usaldusväärne nii kohalikus serveris ja kõik liiduserverite
- Identiteedi serdi peab vastama federation teenuse nimi (nt sts.contoso.com).
    - Andmed on kas teema alternatiivne nimi (SAN) pikendamine tüüp dnsnameWindows või kui kirjeid pole SAN, teema nimes määratletud nime.  
    - Mitme SAN kirjed saavad olla sert, antud üks neist vastab federation teenuse nimi.
    - Kui kavatsete kasutada töökoha liituda, mis täiendavad SAN on vaja väärtusega **enterpriseregistration.** järgneb teie ettevõtte kasutaja põhisumma nimi (UPN) järelliite näiteks **enterpriseregistration.contoso.com**.
- CryptoAPI järgmise genereerimine (CNG) võtmed ja võtme salvestusruumi pakkujate serdid ei toetata. See tähendab, et peate kasutama sert on autori nimega (cryptographic teenusepakkuja) ja mitte Muudeti (võtme salvestusruumi pakkuja) alusel.
- Looduses kaardi serdid on toetatud.

### <a name="name-resolution-for-federation-servers"></a>Nimelahendus liiduserverite jaoks
- Saate seadistada DNS-i kirjete AD FS liiduserveri teenuse nimi (nt sts.contoso.com) sisevõrgu (teie sisemise DNS-i server)-ja suhtevõrgus (avaliku DNS-i kaudu oma domeeniregistraatori). Sisevõrk DNS-i kirje tagama saate A kasutada kirjed ja mitte CNAME-kirje. See on vaja töötada õigesti ühendatud arvuti domeeni Windowsi autentimist.
- Kui juurutate rohkem kui üks AD FS server või Web rakenduse puhverserverit, siis veenduge, et olete konfigureerinud oma laadi koormusetasakaalustusteenuse ja, et DNS-kirjeid AD FS liiduserveri teenuse nimi (nt sts.contoso.com) valige käsk Laadi koormusetasakaalustusteenuse.
- Windowsi integreeritud autentimine Internet Exploreris teie sisevõrgu brauseri rakenduste tööle, veenduge, et AD FS liiduserveri teenuse nimi (nt sts.contoso.com) lisatakse sisevõrgu tsoon IE. See saab kontrollida rühmapoliitika kaudu ja teie domeeni ühendatud arvutites juurutatud.

## <a name="azure-ad-connect-supporting-components"></a>Azure'i AD-ühendus toetavad komponendid
Järgmises loendi komponendid, mis installitakse Azure'i AD-ühenduse server kuhu on installitud Azure'i AD-ühenduse. See loend on lihtne Express installi.  Kui otsustate kasutada eri SQL serveri installi sünkroonimise teenuste lehel, siis SQL-i kiire LocalDB pole installitud kohalikult.

- Azure'i AD-ühenduse seisund
- Microsoft Online Services sisselogimise Assistant IT-spetsialistidele (installitud, kuid pole sõltuvus)
- Microsoft SQL Server 2012 käsurea Utiliidid
- Microsoft SQL Server 2012 kiire LocalDB
- Microsoft SQL Server 2012 Omakliendi
- Microsoft Visual C++ 2013 korduvalt pakett

## <a name="hardware-requirements-for-azure-ad-connect"></a>Azure'i AD-ühenduse riistvaranõuded
Järgmises tabelis antakse ülevaade miinimumnõuded Azure'i AD-ühenduse sünkroonimine arvuti.

| Objektide arv igas Active Directory | CPU | Mälu | Kõvaketta suurus |
| ------------------------------------- | --- | ------ | --------------- |
| Vähem kui 10 000 | 1,6 GHz | 4 GB | 70 GB |
| 10 000 – 50 000 | 1,6 GHz | 4 GB | 70 GB |
| 50,000 – 100 000 | 1,6 GHz | 16 GB | 100 GB |
| 100000 või rohkem objektide SQL serveri täielik versioon on nõutav|  |  |  |
| 100 000 – 300 000 | 1,6 GHz | 32 GB | 300 GB |
| 300 000 – 600 000 | 1,6 GHz | 32 GB | 450 GB |
| Rohkem kui 600 000 | 1,6 GHz | 32 GB | 500 GB |

AD FS-i või Web rakenduste serverid arvutid miinimumnõuded on järgmine:

- Protsessor: Kahetuumaline 1,6 GHz või uuem versioon
- MÄLU: 2GB või uuem versioon
- Azure'i VM: A2 konfiguratsiooni või uuem versioon

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).
