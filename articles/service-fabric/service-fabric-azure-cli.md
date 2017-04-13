<properties
   pageTitle="Teenuse struktuuri kogumite abil CLI suheldes | Microsoft Azure'i"
   description="Kuidas kasutada Azure CLI suhelda teenuse struktuuri kobar"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Azure'i CLI abil saate suhelda teenuse struktuuri kobar

Teenuse struktuuri kobar Linux masinad Linux Azure'i CLI abil saate suhelda.

Kõigepealt on too CLI git Vabariik: uusima versiooni ja häälestada oma teed kasutades järgmised käsud:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Iga käsu seda toetab, saate tippida käsk saada selle käsu nimi. Automaatne lõpetamine on toetatud käske. Näiteks järgmine käsk annab teile abi rakenduse kõik käsud. 

```sh
 azure servicefabric application 
```

Mõne kindla käsu abil saate filtreerida täpsemaks nagu järgmises näites:

```sh
 azure servicefabric application  create
```

Luba automaatne lõpetamine CLI, käivitage järgmine käsk:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Järgmised käsud ühenduse klaster ja näete sõlmed klaster.

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Nimega parameetreid kasutada, ja mida nad, saate tippida--aidata pärast käsu. Näiteks:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Kui ühendate mitme masina kobar arvutisse, **mis pole osa klaster**, kasutage järgmist käsku:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Asendage PublicIPorFQDN sildi real IP- või FQDN vastavalt vajadusele. Kui ühendate mitme masina kobar arvutisse, **mis on osa klaster**, kasutage järgmist käsku:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Saate kasutada PowerShelli või CLI suhelda klaster Linux teenuse struktuuri loodud Azure portaali kaudu. 

**Ettevaatlik:** Nimetatud pole turvaline, seega saate võib avamine boksi ühe kobar manifestis avaliku IP-aadressi lisamisega.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Ühenduse loomine teenuse struktuuri kobar Azure'i CLI abil

Azure'i CLI järgmised käsud on kirjeldatud, kuidas ühendada turvaline kobar. Serdi üksikasjade peab vastama kobar sõlmed sert.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Kui teie sert on sertimiskeskuste (CAs), peate lisama--ca-cert-tee parameeter, nagu järgmises näites: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Kui teil on mitu CAs, kasutage eraldajana koma.
 
Kui teie levinud nimi serti ei sobi ühenduse lõpp-punkti, võite kasutada parameetrit `--strict-ssl` mööda hiilida kinnitamine, nagu on näidatud järgmine käsk: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Kui soovite vahele jätta CA kontrollimine, saate lisada--Hülga volitused parameeter, nagu on näidatud järgmine käsk: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Pärast ühenduse loomist peaks oskama muude CLI käsud klaster suhelda. 

## <a name="deploying-your-service-fabric-application"></a>Teenuse struktuuri rakenduse juurutamine

Käivita kopeerida, registreerida ja struktuuri teenuserakenduse käivitage järgmised käsud:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Rakenduse täiendamine

Protsess on sarnane [Windowsi](service-fabric-application-upgrade-tutorial-powershell.md)).

Koostada, kopeerida, register ja luua rakenduse project juurkaust. Kui teie rakenduse eksemplari nimi on struktuuri: / MySFApp ja tüüp on MySFApp, on järgmised käsud:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Tehke soovitud muudatused rakenduse ja muudetud teenuse taastada.  Värskendage muudetud teenuse manifestifaili (ServiceManifest.xml) värskendatud versiooni teenuse (ja koodi või Config või asjakohased andmed). Ka muuta selle rakenduse manifesti (ApplicationManifest.xml), ja modifitseeritud teenuse numbriga värskendatud versiooni.  Nüüd, kopeerida ja registreerida värskendatud rakenduse abil järgmised käsud:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Nüüd saate alustada rakenduse täiendamine järgmine käsk:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Nüüd saate jälgida rakenduse täiendamine, kasutades SFX. Mõne minuti jooksul rakendus oleks on värskendatud.  Saate värskendatud rakendus veaga proovimine ja märkige ruut automaatne tagasipööramine funktsiooni teenuse struktuuri.

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="copying-of-the-application-package-does-not-succeed"></a>Rakendusepaketi kopeerimine ei õnnestu

Märkige ruut, kui `openssh` on installitud. Vaikimisi Ubuntu töölaua pole installitud. Installida, kasutades järgmine käsk:

```
 sudo apt-get install openssh-server openssh-client**
```

Kui probleem ei lahene, proovige keelamine PAM jaoks ssh, muutes **sshd_config** faili, kasutades järgmisi käske.

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Kui probleem ei kao, proovige arvu suurendamine ssh seansid täites järgmised käsud:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Klahvide abil ssh autentimine (erinevalt paroolid) pole veel toetatud (Kuna platvormi kasutab ssh kopeerimiseks paketid), seega kasutage selle asemel Paroolautentimine.


## <a name="next-steps"></a>Järgmised sammud

Funktsiooni arenduskeskkond luua ja juurutada teenuse struktuuri rakenduse Linuxi arvutikobaras.
