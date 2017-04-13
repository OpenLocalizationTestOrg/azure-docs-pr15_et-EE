<properties 
    pageTitle="Kuidas luua ILB ASE, mis Azure'i ressursihaldur mallide kasutamine | Microsoft Azure'i" 
    description="Saate teada, kuidas luua sisemise koormuse koormusetasakaalustusteenuse ASE Azure'i ressursihaldur mallide kasutamine." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Kuidas luua ILB ASE, mis Azure'i ressursihaldur mallide kasutamine

## <a name="overview"></a>Ülevaade ##
Rakenduse teenuse keskkonnas saab luua virtuaalse võrgu sisemine aadress avaliku VIP asemel.  Selle sisemine aadress on esitatud on Azure osa, mida nimetatakse sisemise koormuse koormusetasakaalustusteenuse (ILB).  Azure'i portaalis saab luua ka ILB ASE.  Ta saab luua automatiseerimise teel Azure'i ressursihaldur mallide abil.  Selles artiklis tutvustatakse juhiseid ja süntaks, mis on vaja luua ka ILB ASE Azure'i ressursihaldur mallide abil.

See on kaasatud automatiseerimine loomine mõne ILB ASE kolm toimingut.
1. Esmalt luuakse alus ASE virtuaalse võrgu asemel avaliku VIP sisemise koormuse koormusetasakaalustusteenuse aadressi kasutades.  Selle toimingu käigus juurkausta domeeninimi on määratud ILB ASE.
2. Kui ILB ASE on loodud, SSL-sert on üles laaditud.  
3. Üleslaaditud SSL-sert on selgesõnaliselt määratud ILB ASE nimega "vaikimisi" SSL-i serdi.  Kasutatakse rakendused on adresseeritud määratud ASE (nt https://someapp.mycustomrootcomain.com) levinud juurdomeeni abil SSL-liikluse rakenduste ILB ASE SSL-sert

## <a name="creating-the-base-ilb-ase"></a>Base ILB ASE loomine ##
Mõni näide Azure'i ressursihaldur Mall ja selle seotud parameetreid faili, on saadaval github [siin][quickstartilbasecreate].

Enamik parameetrid *azuredeploy.parameters.json* failis on levinud nii ILB praeguseks kui ka praeguseks seotud avaliku VIP loomiseks.  Loendi all kõnede parameetreid, Märkus või mida on kordumatu ja mõne ILB ASE loomisel.


- *interalLoadBalancingMode*: enamasti Määra see 3, mis tähendab, et HTTP-või HTTPS liiklus pordid 80 ja 443 ja juhtelemendi/andmete kanali pordid kuulata, ASE, FTP-teenus on seotud ka ILB eraldatud virtuaalse võrgu sisemine aadress.  Kui see atribuut on seatud hoopis 2, siis ainult FTP teenusega seotud pordid (kontroll- ja andmete kanalid) on seotud aadressi ILB ajal HTTP-või HTTPS-liikluse jäävad avaliku VIP.
-  *dnsSuffix*: see parameeter määratleb vaikimisi juurdomeeni, mis määratakse ASE.  Azure'i rakendust Service avaliku variatsiooni, on vaikimisi juurdomeeni kõik web apps *azurewebsites.net*.  Siiski on ILB ASE on sisemine kliendi virtuaalse võrku, ei mõttekas kasutada avaliku teenuse vaikimisi juurdomeeni.  Selle asemel on ILB ASE peaks olema vaikimisi juurdomeeni, mis on mõttekas ettevõtte virtuaalse sisevõrgu kasutamiseks.  Näiteks hüpoteetilist Contoso Corporation võib kasutada vaikimisi juurdomeeni *sisemise* contoso.com rakendusi, mis on mõeldud kasutamiseks ainult lahutatavat ja puuetega inimestele juurdepääsetavate Contoso's virtuaalse võrgustikus. 
-  *ipSslAddressCount*: see parameeter tühjendatakse automaatselt väärtus 0 *azuredeploy.json* faili kuna ILB praeguseks on ainult üks ILB aadress.  Konkreetsete IP-SSL-aadressid jaoks soovitud ILB ASE puuduvad ja seega IP-SSL-i aadress rakenduskausta jaoks soovitud ILB ASE peab olema seatud null, muidu ettevalmistamise tõrge ilmneb. 

Kui *azuredeploy.parameters.json* fail on täidetud jaoks soovitud ILB ASE, ILB ASE seejärel loomist järgmist PowerShelli koodilõigu abil.  Saate muuta faili teed vastavaks Azure'i ressursihaldur mallifailid asukohta teie arvutis.  Ärge unustage ka sisestama oma väärtuste Azure'i ressursihaldur juurutamise nime ja ressursside rühma nime.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Pärast Azure ressursihaldur esitatakse malli kulub ILB ASE luua mõne tunni pärast.  Kui loomine lõpule jõudnud, ILB ASE ilmub portaalis Kasutuskogemuse loendist rakendus teenuse keskkonnas juurutamise käivitanud tellimuse.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Üles- ja "Vaikimisi" SSL-serdi konfigureerimine ##

Kui ILB ASE on loodud, SSL-serdi peaks olema seostatud ASE nagu "vaikimisi" SSL-sert, kasutage loomise SSL ühenduste rakendused.  Jätkuva hüpoteetilist Contoso Corporation näiteks, kui selle ASE vaikimisi DNS-i järelliite on *sisemine contoso.com*, siis *https://some-random-app.internal-contoso.com* ühenduse nõuab SSL-sert, mis kehtib **.internal-contoso.com*. 

On mitmel viisil saada lubatud SSL-sert, sh sisemise CAs, osta sert on välised väljaandja ja iseallkirjastatud serdiga.  Sõltumata sellest, SSL-sert, on vaja järgmisi atribuute serdi õigesti konfigureeritud.

- *Teema*: See atribuut peab olema seatud **rakenduvad juur-domeeni here.com*
- *Teema alternatiivne nimi*: See atribuut peab sisaldama nii * *rakenduvad juur-domeeni here.com*, ja * *.scm.your-juur-domeeni-here.com*.  Teisel väljal põhjus on selles, et SSL-i ühendused SCM/Kudu saidile, mis on seotud konkreetse rakenduse tehakse, vormi *your-app-name.scm.your-root-domain-here.com*aadressi kasutades.

Lubatud SSL-serdi poolt, kus on vaja kahte ettevalmistavaid lisatoimingud.  SSL-serdi peab olema teisendatud/salvestatud pfx-fail.  Meeles, et pfx-fail tuleb kaasata kõik vahe ja root certificates ja peab olema kinnitatud parooliga.

Seejärel peab tulemuseks pfx-fail base64 string teisendada, kuna SSL-serdi laaditakse on Azure ressursihaldur malli abil.  Azure'i ressursihaldur Mallid on tekstifailid, vajab base64 stringi ümber nii, et see saab lisada malli parameetrina pfx-fail.

Allpool PowerShelli koodilõigu kujutab näidet genereerimine iseallkirjastatud serdi, eksportimine serdi nimega pfx-faili teisendamiseks pfx-fail on base64 kodeeringuga stringi ja salvestada selle base64 kodeeringuga stringi eraldi faili.  PowerShelli kood base64 kodeeringus on kohandatud [PowerShelli skriptide ajaveebi][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Kui SSL-sert on edukalt loodud ja teisendatakse on base64-kodeeringuga stringi näide Azure'i ressursihaldur malli github [vaikimisi SSL-serdi] konfigureerimine[ configuringDefaultSSLCertificate] saab kasutada.

Failis *azuredeploy.parameters.json* parameetrid on järgmised:

- *appServiceEnvironmentName*: ILB ASE konfigureeritava nime.
- *existingAseLocation*: tekstistring, mis sisaldab Azure piirkond, kus on juurutatud ILB ASE.  Näide: "Lõuna keskse meie".
- *pfxBlobString*: selle based64 kodeeringuga stringi kujutis pfx-fail.  Eespool näidatud koodilõigu kasutamisel saate oleks string "exportedcert.pfx.b64" sisalduvate kopeerida ja kleepida rakenduses *pfxBlobString* atribuudi väärtust.
- *parooli*: parooli kasutada pfx-fail.
- *certificateThumbprint*: serdi sõrmejälje.  Kui selle väärtuse toomine PowerShelli (nt *$certificate. Sõrmejälje* varasemate koodilõigu kaudu), saate kasutada väärtuse, mis-on.  Kuid väärtus kopeerimisel Windowsi serdi dialoogiboks pidage meeles ribad on tühikuid.  *CertificateThumbprint* peaks välja nägema umbes: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: sõbralik stringi identifikaator teie enda valitud Meilikausta kasutatakse serti.  Nime kasutatakse osana Azure'i ressursihaldur ainuidentifikaator *Microsoft.Web/certificates* üksus, mis tähistab SSL-sert.  Nimi **peab** lõppu järgmised järelliide: \_yourASENameHere_InternalLoadBalancingASE.  See kasutab portaali serdi kasutatakse turvamine on ILB lubatud ASE näitaja.


Lühendatud *azuredeploy.parameters.json* näide on allpool näidatud:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Kui *azuredeploy.parameters.json* fail on täidetud, vaikimisi SSL-serdi konfigureeritud, kasutades järgmist PowerShelli koodilõigu.  Saate muuta faili teed vastavaks Azure'i ressursihaldur mallifailid asukohta teie arvutis.  Ärge unustage ka sisestama oma väärtuste Azure'i ressursihaldur juurutamise nime ja ressursside rühma nime.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Pärast Azure ressursihaldur esitatakse malli võtab aega umbes 40 minutit minutit kohta ASE ees muuta.  Näiteks vaikimisi suurusega ASE abil kaks ees-otsa, kus Mall võtab umbes üks tund ja kakskümmend minutit lõpuleviimiseks.  Malli töötamise ajal ei saa ASE mastaabitud.  

Kui mall on lõpule jõudnud, rakenduste ILB ASE pääseb https ja ühendused on turvatud vaikimisi SSL-serdi abil.  Vaikimisi SSL-serdi kasutatavad rakendused ILB ASE on adresseeritud, kasutades kombinatsiooni rakenduse nimi pluss vaikimisi hostname (hostinimi).  Näiteks *https://mycustomapp.internal-contoso.com* kasutaks vaikimisi SSL-serdi jaoks **.internal-contoso.com*.

Siiski nagu rakendusi, siis töötab mitme rentniku avaliku teenuse arendajate saate konfigureerida ka kohandatud hostinimed üksikute rakendused ja seejärel konfigureerige kordumatu SNI SSL-i serdi sidumiste üksikute rakenduste.  


## <a name="getting-started"></a>Alustamine

Alustamine rakenduse teenuse keskkonnas, lugege teemat [rakenduse teenuse keskkonna tutvustus](app-service-app-service-environment-intro.md)

Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
