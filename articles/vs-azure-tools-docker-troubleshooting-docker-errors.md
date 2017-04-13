<properties
   pageTitle="Tõrkeotsing keskmise suurusega kliendi Windows Visual Studio abil | Microsoft Azure'i"
   description="Tõrkeotsing, luua ja juurutada veebirakenduste keskmise suurusega Windows Visual Studio abil Visual Studio kasutamisel ilmneda."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Visual Studio keskmise suurusega arengu tõrkeotsing

Töötamine Visual Studio Tools for keskmise suurusega eelvaade, võivad ilmneda probleeme tõttu eelvaade.
Järgnevalt mõned levinud probleemid ja lahendused.


## <a name="unable-to-validate-volume-mapping"></a>Ei saa kontrollida helitugevuse kaardistamine
Helitugevuse vastendamine on vajalik rakendus kausta ümbrises lähtekoodi ja kahendfaile rakenduse ühiskasutusse.  Köite vastendused sisalduvad keskmise suurusega-compose.dev.debug.yml ja keskmise suurusega-compose.dev.release.yml failid. Host teie arvutis on muutunud failid, soovitud ümbriste kajastuvad need muudatused sarnaseid kausta struktuuri.

Helitugevuse vastenduse lubamiseks **... sätete** avamine ikooni Windowsi keskmise suurusega "moby" ja valige vahekaart **Jagatud draivid** .  Veenduge, et nende kontrollimiseks, ning seejärel klõpsata käsku **Rakenda**ühiskasutuses ketas kirja, mis majutab projekti kui ka ketas täht % kausta asukoht.

Kui helitugevuse vastenduse töötab, kui seadmeid on ühiskasutuses testimiseks kas taastada ja F5 kaudu Visual Studio või proovige järgmine käsk: Küsi:

*Windowsi käsuviibal*

*[Märkus: See eeldab, et kasutajad kausta asub "C" ketas ja, et see on ühiskasutusse antud.  Kui olete ühiskasutusse andnud erinevat ketast värskendada vajadusel]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*Linux ümbrises*

```
/ # ls
```

Peaksite nägema loetelu kaustast kasutajate/avaliku kataloogi.
Kui kuvatakse failid ja kausta /c/Users/Public pole tühi, pole helitugevuse vastenduse õigesti konfigureeritud. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Muuta wormhole kataloogi sisu kuvamiseks on `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Märkus:** *Linux VMs töötamisel container failisüsteemi on tõstutundlik.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Koosta: "PrepareForBuild" tööülesande nurjus ootamatult.

Microsoft.DotNet.Docker.CommandLine.ClientException: Ilmnes tõrge ühendust luua:

Veenduge, et vaikimisi keskmise suurusega host töötab. Avage käsuviip ja käivitada.

```
docker info
```

Kui see tagastab tõrke, seejärel proovige **Keskmise suurusega Windowsi** töölaua rakenduse käivitamiseks.  Kui töölauarakenduse töötab seejärel ikooni **moby** salve peaks olema nähtav. Paremklõpsake selle ikooni ja avage **sätted**.  Klõpsake nuppu **Lähtesta** menüü ja seejärel **uuesti keskmise suurusega …**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Käsitsi 0,31 kuni 0. 40 versiooni täiendamine


1. Projekti varundamine
1. Kustutage järgmised failid projekti.

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Sulgege lahendus ja järgmised read eemaldamine .xproj faili.

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Lahendus uuesti
1. Properties\launchSettings.json faili järgmised read eemaldamiseks tehke järgmist.

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Järgmised failid, mis on seotud keskmise suurusega project.json, klõpsake selle publishOptions eemaldamiseks tehke järgmist.

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Eelmist versiooni ja keskmise suurusega tööriistad 0,40 ja **keskmise suurusega tugi-> Lisa** uuesti installida kontekstimenüüst ASP.net-i Core Web või konsooli rakendus. See lisab uusi nõutav keskmise suurusega esemeid tagasi projekti. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Tõrge dialoogiboksi **keskmise suurusega tugi-> Lisa** katsel ilmneb või silumine (F5) ümbris rakenduse ASP.net-i Core

Oleme aeg-ajalt näinud desinstallimine ja laiendid installimist Visual Studio MEF (hallatavate laiendusraamistikus) vahemälu saab rikutud. Kui see juhtub, võib see põhjustada erinevate tõrge dialoogid keskmise suurusega toe lisamisel ja/või proovite käivitada või silumine (F5) ASP.net-i Core rakenduse. Ajutise lahendusena käivitada kustutada ja taastada MEF vahemälu toimige järgmiselt.

1. Sulgege kõik rakenduse Visual Studio
1. Avage %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Kustutage järgmised kaustad
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Avatud Visual Studio
1. Proovi uuesti stsenaarium 
