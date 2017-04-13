<properties 
    pageTitle="Cloud Services ja halduse serdid | Microsoft Azure'i" 
    description="Siit saate teada, kuidas luua ja kasutada Microsoft Azure'i serdid" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Azure'i pilveteenustega serdid ülevaade
Serdid on kasutusel Azure pilveteenustega ([teenuse serdid](#what-are-service-certificates)) ja autentimiseks halduse API ([halduse serdid](#what-are-management-certificates) Azure klassikaline portaali ja ei ARM kasutamisel). See teema annab ülevaate nii serdi tüübid, kuidas [luua](#create) ja [juurutada](#deploy) Azure neile.

Azure'i kasutatud serdid on x.509 v3 serdid ja allkirjastada teise usaldusväärne sert või need võivad olla iseallkirjastatud. Iseallkirjastatud sert on alla oma looja ja seetõttu, usaldatakse vaikimisi. Enamiku brauserite saate ignoreerib seda. Ise väljatöötamisel ja testige oma pilveteenustega tuleks kasutada ainult iseallkirjastatud serdid. 

Azure'i kasutatavaid serdid võivad sisaldada privaatse või avalik võti. Serdid on sõrmejälje, mis pakub võimalust neid üheselt viis kindlaks teha. See sõrmejälje kasutatakse Azure [konfiguratsioonifail](cloud-services-configure-ssl-certificate.md) tuvastamiseks pilveteenus tuleks kasutada sert. 

## <a name="what-are-service-certificates"></a>Mis on teenuse serdid?
Teenuse serdid on lisatud cloud services ja turvaline side ja sealt teenuse lubamine. Näiteks kui olete juurutanud web rolli, tuleks soovite esitama sert, mida saate autentida exposed HTTPS-i lõpp-punkti. Teenuse serdid, määratletud definitsiooni teenus on automaatselt juurutatud virtual arvutisse, kus töötab teie roll eksemplari. 

Azure'i klassikaline portaali kasutades Azure klassikaline portaalis või teenuse juhtimise API abil saate laadida teenuse serdid. Teenuse serdid on seostatud teatud pilveteenusesse ning määratud teenuse definitsioonifail juurutamine.

Teenuse serdid saate hallata eraldi oma teenuste ja haldab eri isikud. Näiteks võib arendaja Laadige teenuse paketi, mis viitab IT-juht eelnevalt on üles laaditud Azure'i sert. IT-juht saab hallata ja pikendada selle serdi teenuse konfiguratsiooni muutmise ilma uue teenuse paketi üleslaadimine. See on võimalik, kuna loogiline nimi serti ja poe nimi ja asukoht on määratud määratlus teenusefail ajal serdi sõrmejälje on määratud konfiguratsioonifailis teenus. Serdi värskendamiseks on vaja ainult üles laadida uut serti ja teenuse konfiguratsioonifail sõrmejälje väärtust muuta.

## <a name="what-are-management-certificates"></a>Mis on halduse serdid?
Halduse serdid võimaldavad Azure'i klassikaline esitatud teenuse juhtimise API autentimiseks. Paljud programmid ja tööriistad (nt Visual Studio või Azure SDK) kasutatakse need serdid konfigureerimine ja erinevate Azure'i teenuste automatiseerimiseks. Neid ei ole eriti seotud pilveteenused. 

>[AZURE.WARNING] Ole ettevaatlik! Järgmist tüüpi tunnistuste luba kõigile, kellel autendib neid need on seostatud tellimuse haldamiseks. 

### <a name="limitations"></a>Piirangud
On limiit 100 halduse sertide tellimuse kohta. Olemas on ka limiit 100 korraldamise tunnistusi kõigi tellimuste jaotises kindla teenuse administraator kasutaja ID-ga. Kui kasutaja ID-d administraatori konto on juba kasutatud lisada 100 halduse serdid ja on vaja veel sertide, saate lisada koostöö administraator lisada täiendavad serdid. 

Enne lisada rohkem kui 100 serdid, vaadake, kui saate uuesti kasutada lüüsiarvutis olemasolev sert. Potentsiaalselt Ebavajalike keerukuse abil kaasadministraatorite lisatakse teie serdi haldus protsess.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Uue iseallkirjastatud serdi loomine
Saate kasutada mis tahes tööriista iseallkirjastatud serdi loomine, kui nad järgivad neid sätteid saadaval.

* Mõne x.509 vastav sert.
* Sisaldab privaatvõti.
* Loodud võtme exchange (pfx-fail).
* Subjekti nimi peab vastama domeeni kasutada pilveteenusesse. 
    > Te ei saa omandada SSL-i serdi on cloudapp.net (või mis tahes Azure'i seotud) Domeen; selle serdi subjekti nimi peab vastama kohandatud domeeni nimi, mida kasutatakse teie taotlus. Näiteks **contoso.net**, **contoso.cloudapp.net**.
* Vähemalt 2048-bitine krüptimine.
* **Teenuse serdi ainult**: kliendipoolne serdi peab asuma *isikliku* serdi pood.

On kaks lihtsat võimalust luua Windows sert on `makecert.exe` kasuliku või IIS-i.

### <a name="makecertexe"></a>MakeCert.exe

Selle kasuliku on taunitud, ja enam siin dokumenteerida. Lugege lisateavet [MSDN-i artiklit](https://msdn.microsoft.com/library/windows/desktop/aa386968) .

### <a name="powershell"></a>PowerShelli

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Kui soovite serti IP-aadress, domeeni asemel kasutada, kasutage - dnsnameWindows parameetri IP-aadress.


Kui soovite kasutada selle [serdi haldus portaalis](../azure-api-management-certs.md), eksportimine **CER-** fail:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Teenusekomplekti Internet Information Services (IIS)

On palju lehekülgi Internetis, mis hõlmavad kuidas seda teha IIS-i. [Siin](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) on suur üks sain on vastavalt mulle selgitatakse seda ka. 

### <a name="java"></a>Java
Java abil saate [serdi loomine](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[Selles](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) artiklis kirjeldatakse, kuidas luua SSH serdid.

## <a name="next-steps"></a>Järgmised sammud

[Laadige oma Azure klassikaline portaali tunnistus](cloud-services-configure-ssl-certificate.md) (või [Azure'i portaal](cloud-services-configure-ssl-certificate-portal.md)).

Azure'i klassikaline portaali [halduse API serdi](../azure-api-management-certs.md) üleslaadimine.

>[AZURE.NOTE] Azure portaali abil halduse serdid API juurdepääsu, kuid selle asemel kasutab Kasutajakontod.
