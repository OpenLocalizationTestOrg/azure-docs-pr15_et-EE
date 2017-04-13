<properties 
    pageTitle="Tükeldatud ühendamine turvalisuse konfiguratsiooni | Microsoft Azure'i" 
    description="Häälestamine x409 krüptimise serdid" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Tükeldatud ühendamine turvalisus konfigureerimine  

Tükeldatud/ühendamine teenuse kasutamiseks tuleb konfigureerida õigesti turvalisus. Teenus on osa elastne skaala funktsioon Microsoft Azure'i SQL-andmebaasi. Lisateabe saamiseks vt [elastne skaala tükeldatud ja teenuse õpetuse ühendada](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Sertide konfigureerimine

Serdid on konfigureeritud kahel viisil. 

1. [SSL-serdi konfigureerimine](#To-Configure-the-SSL#Certificate)
2. [Kliendi sertide konfigureerimine](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Saada

Sertide saab [Windowsi serdi teenuse](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx)või avaliku sertimiskeskuste (CAs). Need on eelistatud meetodid, kuidas serdid.

Kui need suvandid ei ole saadaval, saate luua **iseallkirjastatud serdid**.
 
## <a name="tools-to-generate-certificates"></a>Tööriistad luua serdid

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Tööriistade käivitamiseks

* Kaudu arendaja Käsuviip Visual Studios, lugege teemat [Visual Studio käsureale](http://msdn.microsoft.com/library/ms229859.aspx) 

    Kui installitud, avage.

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Saada WDK kaudu [Windows 8.1: allalaadimine komplektid ja tööriistad](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>SSL-serdi konfigureerimine
SSL-sert on vajalik side ja autentimiseks server. Valige alltoodud kolme stsenaariumi korral ja käivitada kõik oma toimingud.

### <a name="create-a-new-self-signed-certificate"></a>Uue iseallkirjastatud serdi loomine

1.    [Iseallkirjastatud serdi loomine](#Create-a-Self-Signed-Certificate)
2.    [PFX-fail Self-Signed SSL-serdi loomine](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Teenus Cloud SSL-serdi üleslaadimine](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [SSL-serdi konfigureerimine Teenusefail värskendamine](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [SSL-i sertimiskeskus importimine](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Lüüsiarvutis olemasolev sert sert poes kasutamiseks
1. [SSL-serdi eksportimine salv](#Export-SSL-Certificate-From-Certificate-Store)
2. [Teenus Cloud SSL-serdi üleslaadimine](#Upload-SSL-Certificate-to-Cloud-Service)
3. [SSL-serdi konfigureerimine Teenusefail värskendamine](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Lüüsiarvutis olemasolev sert kasutamine PFX-fail

1. [Teenus Cloud SSL-serdi üleslaadimine](#Upload-SSL-Certificate-to-Cloud-Service)
2. [SSL-serdi konfigureerimine Teenusefail värskendamine](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Kliendi sertide konfigureerimine
Kliendi serdid on vaja teha teenusesse autentida. Valige alltoodud kolme stsenaariumi korral ja käivitada kõik oma toimingud.

### <a name="turn-off-client-certificates"></a>Kliendi sertide väljalülitamine
1.    [Kliendi serdi autentimise väljalülitamine](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Probleemi uus iseallkirjastatud kliendi serdid
1.    [Iseallkirjastatud sertimiskeskus loomine](#Create-a-Self-Signed-Certification-Authority)
2.    [Teenus Cloud CA serdi üleslaadimine](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Värskendage CA serdi faili teenuse konfigureerimine](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Probleemi kliendi serdid](#Issue-Client-Certificates)
5.    [Kliendi sertide PFX failide loomine](#Create-PFX-files-for-Client-Certificates)
6.    [Serdi importimine klient](#Import-Client-Certificate)
7.    [Kliendi serdi Thumbprints kopeerimine](#Copy-Client-Certificate-Thumbprints)
8.    [Lubatud kliendid konfiguratsioonifailis teenuse konfigureerimine](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Kasuta olemasolevat kliendi serdid
1.    [Otsige CA avalik võti](#Find-CA-Public Key)
2.    [Teenus Cloud CA serdi üleslaadimine](#Upload-CA-certificate-to-cloud-service)
3.    [Värskendage CA sertifikaadi teenuse konfiguratsioonifail](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Kliendi serdi Thumbprints kopeerimine](#Copy-Client-Certificate-Thumbprints)
5.    [Lubatud kliendid konfiguratsioonifailis teenuse konfigureerimine](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Kliendi serdi tühistamise kontrollimine konfigureerimine](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Lubatud IP-aadressid

Teenuse lõpp-punktid võib olla piiratud teatud IP-aadresside vahemikud.

## <a name="to-configure-encryption-for-the-store"></a>Krüptimise poe konfigureerimine

Mis on talletatud metaandmete salve mandaadi krüptimiseks on vajalik sert. Valige alltoodud kolme stsenaariumi korral ja käivitada kõik oma toimingud.

### <a name="use-a-new-self-signed-certificate"></a>Kasutage uut iseallkirjastatud serti

1.     [Iseallkirjastatud serdi loomine](#Create-a-Self-Signed-Certificate)
2.     [PFX-fail Self-Signed krüptimise serdi loomine](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Teenus Cloud krüptimise serdi üleslaadimine](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Värskendus konfiguratsioonifail teenuse krüptimise sert](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Kasuta olemasolevaid serti serdi poest

1.     [Serdi poe krüptimise serti eksportimine](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Teenus Cloud krüptimise serdi üleslaadimine](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Värskendus konfiguratsioonifail teenuse krüptimise sert](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Lüüsiarvutis olemasolev sert kasutamine PFX-fail

1.     [Teenus Cloud krüptimise serdi üleslaadimine](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Värskendus konfiguratsioonifail teenuse krüptimise sert](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Vaikimisi konfigureerimine

Vaikimisi konfiguratsiooni keelab kõik juurdepääsu HTTP lõpp-punkti. See on soovitatavat sätet, kuna need lõpp-punktid taotluste võib teha tundlikku teavet nagu andmebaasi identimisteabe.
Vaikimisi konfiguratsioon võimaldab juurdepääsu HTTPS-i lõpp-punkti. See säte võib olla piiratud edasi.

### <a name="changing-the-configuration"></a>Konfiguratsiooni muutmine

Juhtelemendi juurdepääsureeglid, mille soovite rakendada ja lõpp-punkti rühma on konfigureeritud selle **<EndpointAcls>** **teenuse konfiguratsioonifail**jaotis.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Reeglite juurdepääsu juhtimine jaotises on konfigureeritud on <AccessControl name=""> osas teenuse konfiguratsioonifail. 

Vorming on selgitatud võrgu juurdepääsu juhtimine loetletud dokumendid.
Näiteks ainult IP-d vahemikus 100.100.0.0 100.100.255.255 HTTPS-i lõpp-punkti juurdepääsu lubamiseks reegleid näeks välja selline:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Eitamine teenuse vältimine

On kaks eri võimalust, mis on toetatud tuvastamiseks ja vältimiseks eitamine teenuse eest:

*    Samaaegseid taotlusi kohta remote host arv piirata (vaikimisi välja lülitatud)
*    Piira määr Accessi remote hosti kohta (klõpsake vaikimisi)

Põhinevad täpsemaks dokumenteerida dünaamiline IP turvalisus IIS-i funktsioone. Kui selle konfiguratsiooni muutmise arvestage järgmisi tegureid:

* Puhverserverid ja remote host teabe üle võrgu aadress tõlge seadmete toimimist
* Iga taotluse mis tahes ressursi web rolli peetakse (nt laadimine skriptide, pildid, jne)

## <a name="restricting-number-of-concurrent-accesses"></a>Samaaegseid arvu piiramine

On sätted, mis konfigureerida seda käitumist.

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Saate muuta DynamicIpRestrictionDenyByConcurrentRequests true lubamiseks kaitse.

## <a name="restricting-rate-of-access"></a>Määra juurdepääsu piiramine

On sätted, mis konfigureerida seda käitumist.

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Keelatud taotluse vastuse konfigureerimine

Järgmise sätte konfigureerib juurdepääsu taotluse vastuse:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Vaadake muude toetatud väärtuste dünaamiline IP turvalisuse IIS-i dokumentatsioonist.

## <a name="operations-for-configuring-service-certificates"></a>Toimingute jaoks konfigureerimise teenuse serdid
See teema on ainult viide. Järgige kirjeldatud konfigureerimise juhised.

* SSL-serdi konfigureerimine
* Kliendi sertide konfigureerimine

## <a name="create-a-self-signed-certificate"></a>Iseallkirjastatud serdi loomine
Täita:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Kohandamiseks tehke järgmist.

*    -n teenuse URL-i. Metamärkide ("CN = * .cloudapp .net") ja alternatiivne nimed (CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") ei toetata.
*    e - serdi aegumiskuupäev keeruka parooli loomine ja määrake see, kui seda küsitakse.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>PFX-fail iseallkirjastatud SSL-serdi loomine

Täita:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Sisestage parool ja seejärel eksportige serdi suvanditest:
* Jah, ekspordi privaatvõti
* Ekspordi kõik laiendatud atribuudid

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-serdi eksportimine salv

* Serdi otsimine
* Klõpsake nuppu toimingud -> kõik tööülesanded -> ekspordi...
* Eksportimine rakendusse serdi lisamine. PFX faili suvanditest:
    * Jah, ekspordi privaatvõti
    * Kaasata kõik serdid sertimiskeskuselt võimalusel * ekspordi kõik laiendatud atribuudid

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL-serdi üleslaadimine pilveteenuses

Laadi ning olemasolevate sertifikaadi või loodud. PFX-fail SSL-i võtme paar:

* Sisestage parool kaitsmine privaatne olulist teavet

## <a name="update-ssl-certificate-in-service-configuration-file"></a>SSL-serdi konfigureerimine teenusefail värskendamine

Värskendada sõrmejälje pilveteenusesse üles laaditud serdi sõrmejälje väärtus teenuse konfiguratsioonifailis järgmisi sätteid:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL-i sertimiskeskus importimine

Kõik konto/kohapeal, et teenuse suhelda, tehke järgmist.

* Topeltklõpsake soovitud. Windows Exploreris CER-faili
* Klõpsake dialoogiboksis serdi serdi installimine...
* Serdi importimine Trusted Root sertimiskeskuste pood

## <a name="turn-off-client-certificate-based-authentication"></a>Kliendi serdi autentimise väljalülitamine

Toetatakse ainult kliendi serdi autentimise ja keelamise see võimaldab juurdepääsu teenuse lõpp-punktid, kui muud on olemas (nt Microsoft Azure'i Virtual Network).

FALSE konfiguratsioonifailis teenuse funktsiooni väljalülitamiseks nende sätete muutmiseks tehke järgmist.

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Seejärel kopeerige sama sõrmejälje SSL-serdi CA serdi säte.

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Iseallkirjastatud sertimiskeskus loomine
Käivita tegutseda sertimiskeskus iseallkirjastatud serdi loomine tehke järgmist:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Seda kohandada

*    e - aegumiskuupäev sertimine


## <a name="find-ca-public-key"></a>Otsige CA avalik võti

Kõik kliendi sertide peab olema välja teenuse jaoks usaldusväärne sertimiskeskus. Otsige avalik võti väljastanud kliendi serdid, mida kavatsete kasutada autentimine, et üleslaadimine pilveteenusesse sertimiskeskus.

Kui faili avalik võti pole saadaval, eksportida serdi poe kaudu:

* Serdi otsimine
    * Kliendi serdi väljaandja sama sertimiskeskus otsimine
* Topeltklõpsake serti.
* Valige vahekaart sertimiskeskuselt dialoogiboks sert.
* Topeltklõpsake kirjet CA tee.
* Serdi atribuutide märkmeid teha.
* Sulgege dialoogiboks **sert** .
* Serdi otsimine
    * Otsige üles märkida CA.
* Klõpsake nuppu toimingud -> kõik tööülesanded -> ekspordi...
* Eksportimine rakendusse serdi lisamine. CER koos suvanditest:
    * **Ei, ekspordi privaatvõti**
    * Kaasata kõik serdid sertimiskeskuselt võimalusel.
    * Ekspordi kõik laiendatud atribuudid.

## <a name="upload-ca-certificate-to-cloud-service"></a>CA serdi üleslaadimine pilveteenuses

Laadi koos olemasoleva serdi või loodud. CER faili CA avalik võti.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Värskenduse CA serdi faili teenuse konfigureerimine

Värskendada sõrmejälje pilveteenusesse üles laaditud serdi sõrmejälje väärtus teenuse konfiguratsioonifailis järgmisi sätteid:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Värskendage järgmised sätte väärtus sama sõrmejälje abil.

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Probleemi kliendi serdid

Iga üksiku teenuse volitatud peaks olema kliendi jaoks, välja arvatud oma kasutamine ja tuleb valida tema enda keeruka parooli, et kaitsta oma privaatvõti. 

Samasse arvutisse, kus loodud CA iseallkirjastatud serti ja talletatud tuleb teostada järgmised toimingud:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Kohandamine:

* n - ID kliendi, mis on autenditud selle serdiga
* e - sertifikaadi aegumiskuupäev
* MyID.pvk ja MyID.cer koos selle kliendi sertifikaadi kordumatu nimi

See käsk küsib parooli luua ja kasutada üks kord. Keeruka parooli kasutada.

## <a name="create-pfx-files-for-client-certificates"></a>PFX failid kliendi sertide loomine

Iga kliendi loodud sert, käivitada:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Kohandamine:

    MyID.pvk and MyID.cer with the filename for the client certificate

Sisestage parool ja seejärel eksportige serdi suvanditest:

* Jah, ekspordi privaatvõti
* Ekspordi kõik laiendatud atribuudid
* Isik, kellele see sert väljastatakse tuleb valida ekspordi parooli

## <a name="import-client-certificate"></a>Serdi importimine klient

Iga üksiku, kellele on välja antud sert kliendi tuleks importida võti paari masinad ta kasutab teenust suhelda.

* Topeltklõpsake soovitud. PFX-fail Windows Exploreris
* Impordi serdi sisse isikliku talletamine ja vähemalt see suvand.
    * Kaasa kõik laiendatud atribuudid, mis on märgitud

## <a name="copy-client-certificate-thumbprints"></a>Kliendi serdi thumbprints kopeerimine
Iga isik, kellele on välja antud sert kliendi peab järgmiste juhiste saamiseks sõrmejälje, oma sert, mis lisatakse teenuse konfiguratsioonifail:
* Käivitage certmgr.exe
* Valige vahekaart isiklik
* Topeltklõpsake kliendi serti kasutatakse autentimine
* Valige avanevas dialoogiboksis serdi vahekaarti üksikasjad
* Veenduge, et kuva kuvab kõik
* Valige väli nimega sõrmejälje loendis
* Kopeerige sõrmejälje **- nähtav Unicode'i märkide ette esimese kustutamine** Kustuta väärtus kõik tühikud

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Lubatud kliendid konfiguratsioonifailis teenuse konfigureerimine

Värskendage järgmised sätte teenuse konfiguratsioonifailis väärtus kliendi serdid lubatud juurdepääs teenusele thumbprints komaga eraldatud loendiga.

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Kliendi serdi tühistamise kontrollimine konfigureerimine

Vaikimisi ei kontrolli asutuse jaoks kliendi serdi tühistamise olek. Sisselülitamiseks kontrolle, kui kliendi sertide väljastanud sertimiskeskus toetab kontrolli järgmist sätet muuta ühe X509RevocationMode loendamine määratletud väärtused:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>PFX faili krüptimise iseallkirjastatud sertide loomine

Krüptimise sert, käivitada:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Kohandamine:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Sisestage parool ja seejärel eksportige serdi suvanditest:
*    Jah, ekspordi privaatvõti
*    Ekspordi kõik laiendatud atribuudid
*    Peate parooli, kui serdi üleslaadimine pilveteenusesse.

## <a name="export-encryption-certificate-from-certificate-store"></a>Serdi poe krüptimise serti eksportimine

*    Serdi otsimine
*    Klõpsake nuppu toimingud -> kõik tööülesanded -> ekspordi...
*    Eksportimine rakendusse serdi lisamine. PFX faili suvanditest: 
  *    Jah, ekspordi privaatvõti
  *    Võimalusel kaasata kõik serdid sertimiskeskuselt 
*    Ekspordi kõik laiendatud atribuudid

## <a name="upload-encryption-certificate-to-cloud-service"></a>Laadige krüptimise serti pilveteenuses

Laadi ning olemasolevate sertifikaadi või loodud. PFX faili krüptimise võti paari:

* Sisestage parool kaitsmine privaatne olulist teavet

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Värskendus konfiguratsioonifail teenuse krüptimise sert

Värskendada sõrmejälje pilveteenusesse üles laaditud serdi sõrmejälje väärtus teenuse konfiguratsioonifailis järgmised sätted:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Serdi tavalised toimingud

* SSL-serdi konfigureerimine
* Kliendi sertide konfigureerimine

## <a name="find-certificate"></a>Serdi otsimine

Tehke järgmist.

1. Käivitage mmc.exe.
2. Faili-lisandmooduli lisamine või eemaldamine >...
3. Valige **serdid**.
4. Klõpsake nuppu **Lisa**.
5. Valige sert poe asukoht.
6. Klõpsake nuppu **valmis**.
7. Klõpsake nuppu **OK**.
8. Laiendage **serdid**.
9. Serdi poe laiendage.
10. Serdi tütarüksuste laiendage.
11. Valige loendis sert.

## <a name="export-certificate"></a>Eksportige sert
**Serdi eksportimise viisard**:

1. Klõpsake nuppu **edasi**.
2. Valige **Jah**, siis **ekspordi privaatvõti**.
3. Klõpsake nuppu **edasi**.
4. Valige soovitud faili vorming.
5. Märkige soovitud suvandid.
6. Märkige ruut **pea parool**.
7. Keerukas parool sisestage ja kinnitage see.
8. Klõpsake nuppu **edasi**.
9. Tippige või Sirvige faili nimi serti salvestuskoha (kasutada lisamine. PFX laiend).
10. Klõpsake nuppu **edasi**.
11. Klõpsake nuppu **valmis**.
12. Klõpsake nuppu **OK**.

## <a name="import-certificate"></a>Serdi importimine

Serdi impordiviisardi:

1. Valige poe asukoht.

    * Valige **Praeguse kasutaja** , kui ainult protsessid tööle, klõpsake jaotises praegune kasutaja pääseb juurde teenuse
    * Valige **Kohalikus arvutis** , kui see arvuti muud protsessid on juurdepääs teenuse
2. Klõpsake nuppu **edasi**.
3. Kui faili importimine, kontrollige faili tee.
4. Kui importimine on. PFX-fail:
    1.     Sisestage parool kaitseb privaatvõti
    2.     Valige impordi suvandid
5.     Valige järgmised poes "Asukoht" serdid
6.     Klõpsake nuppu **Sirvi**.
7.     Valige soovitud pood.
8.     Klõpsake nuppu **valmis**.
       
    * Kui valiti Trusted Root sertimiskeskus pood, klõpsake nuppu **Jah**.
9.     Klõpsake nuppu **OK** kõik dialoogiboksi Windows.

## <a name="upload-certificate"></a>Laadige üles sert

[Azure'i portaal](https://portal.azure.com/)

1. Valige **pilveteenustega**.
2. Valige soovitud pilveteenuses.
3. Klõpsake menüü top **serdid**.
4. Klõpsake allosas oleval ribal, **üles laadida**.
5. Valige sert fail.
6. Kui see on lisamine. PFX faili, sisestage parool privaatvõti.
7. Kui lõpule jõudnud, kopeerige serdi sõrmejälje loendist uus kirje.

## <a name="other-security-considerations"></a>Muud turvakaalutlused
 
SSL-i sätete dokumendis kirjeldatud krüptida suhtlemine teenuse ja oma klientidele HTTPS-i lõpp-punkti kasutamisel. See on oluline, kuna mandaat Accessi andmebaasi ja muu potentsiaalselt tundliku teabe sisalduvad teatises. Pange tähele, et teenus ei lahene sisemise oleku, mandaat, sh selle sisemine tabeli Microsoft Azure SQL andmebaasis on esitatud metaandmete mäluruumi teie tellimus Microsoft Azure'i. Andmebaasi on määratletud osana järgmise sätte teenuse konfigureerimine faili (. CSCFG fail): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Selles andmebaasis talletatud identimisteabe on krüptitud. Siiski hea tava, veenduge, et teie teenuse kasutuselevõttu nii web ka töötajal rollid on ajakohane ja turvaliseks need mõlemad on juurdepääs metaandmete andmebaasi ja dekrüptimine talletatud identimisteabe krüptimise jaoks kasutatavat serti. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
