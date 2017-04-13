<properties
   pageTitle="Rakenduste kohaliku keskmise suurusega ümbrises silumine | Microsoft Azure'i"
   description="Saate teada, kuidas rakenduse, kus töötab kohaliku keskmise suurusega ümbrises muutmine, värskendamine container kaudu redigeerimine ja värskendamine ja silumine katkestuspunktide määramine"
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
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Rakenduste kohaliku keskmise suurusega ümbrises silumine

## <a name="overview"></a>Ülevaade
Visual Studio Tools for keskmise suurusega pakub ühtne viis arendada lisamine ja kinnitage rakenduse kohalik Linux keskmise suurusega ümbrises.
Te ei pea iga kord, kui teete koodi muuta ümbrisest taaskäivitama.
Selles artiklis on selgitavad, kuidas kasutada funktsiooni "Redigeerimine ja värskendamine" ASP.net-i Core Web rakenduse käivitamine kohaliku keskmise suurusega ümbrises, tehke vajalikud muudatused ja seejärel värskendate brauseri nende muudatuste nägemiseks.
See näitab ka teie suhtes silumine seadmise kohta.

> [AZURE.NOTE] Windowsi Container tugi tulevad tulevikus

## <a name="prerequisites"></a>Eeltingimused
Järgmised tööriistad tuleb installida.

- [Visual Studio 2015 värskenduse 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129) installimine
- [Microsoft ASP.net-i Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Keskmise suurusega ümbriste käivitamiseks, peate kohaliku keskmise suurusega kliendi.
Saate kasutada välja [Keskmise suurusega tööriistakasti](https://www.docker.com/products/overview#/docker_toolbox) mis nõuab Hyper-V keelatava või [Keskmise suurusega Windows beeta](https://beta.docker.com) , mis kasutab Hyper-V ja nõuab Windows 10 saate kasutada.

Kui keskmise suurusega tööriistakasti, peate [keskmise suurusega kliendi konfigureerimine](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. luua veebirakenduse

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2 keskmise suurusega toe lisamine

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. muuta oma kood ja värskendamine

Kiiresti kordamiseks muudatusi, saate oma rakenduse ümbris sees ja jätkata, teha muudatusi, nende vaatamist, nagu IIS-i kiire.

1. Lahendus konfiguratsioonis määratud `Debug` ja vajutage ** &lt;klahvikombinatsiooni CTRL + F5 >** luua oma keskmise suurusega pildi ja käivitage see kohalikult.

    Pärast pildi container on ehitatud ja keskmise suurusega ümbrises töötab, käivitab Visual Studio Web app brauseris vaikimisi.
    Kui teil on Microsoft Edge'i brauseri kaudu või muul viisil on tõrkeid, vaadake [tõrkeotsingu](vs-azure-tools-docker-troubleshooting-docker-errors.md) osa.

1. Liikuge lehel teave, mis on kui me meie muudatusi teha.

1. Naaske rakendusse Visual Studio ja avage `Views\Home\About.cshtml`.

1. Järgmised HTML-i sisu lisada faili lõppu ja salvestage soovitud muudatused.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Väljundi aknas, kui .NET koostamine on lõpetatud ja nende ridade kuvamiseks vaatamiseks aktiveerige oma brauserit ja värskendage lehel teave.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Muudatuste rakendamist!

## <a name="4-debug-with-breakpoints"></a>4. silumine katkestuspunktid abil

Sageli muudatused on seda vaja täiendavalt kontrolli, Visual Studio silumine funktsioonide kasutamine.

1.  Naaske Visual Studio ja avamine`Controllers\HomeController.cs`

1.  Asendage sisu About() meetodi järgmist:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Määrate katkestuspunkti vasakul olevat `string message`... joon.

1.  Tulemus ** &lt;F5 >** alustamiseks silumine.

1.  Liikuge oma katkestuspunkt tabas lehel teave.

1.  Visual Studio katkestuspunkt kuvamiseks aktiveerige ja kontrollida sõnumi väärtust.

    ![][2]

##<a name="summary"></a>Kokkuvõte

[Visual Studio 2015 tööriistad keskmise suurusega](https://aka.ms/DockerToolsForVS), saate tootlikkuse tootmise realism arendamise keskmise suurusega container kohalikult, töötamine.

## <a name="troubleshooting"></a>Tõrkeotsing

[Visual Studio keskmise suurusega arengu tõrkeotsing](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Lisateavet leiate teemast kus Visual Studio ja Windows Azure'i keskmise suurusega

- [Keskmise suurusega Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - arendada oma .NET Core kood ümbris
- [Keskmise suurusega Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - luua ja juurutada keskmise suurusega ümbriste
- [Keskmise suurusega Tools for Visual Studio kood](http://aka.ms/dockertoolsforvscode) - keele teenuste abil veel e2e stsenaariumid varsti keskmise suurusega faile, redigeerimiseks
- [Teavet Windows Container](http://aka.ms/containers)Windows Server ja Nano serveri teave
- [Azure'i teenus](https://azure.microsoft.com/services/container-service/) - [Azure Container teenuse sisu](http://aka.ms/AzureContainerService)
-    Veel näiteid keskmise suurusega töötamine, vt [töötamine keskmise suurusega](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 ühendamine [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Lisateavet quickstarts HealthClinic.biz demo, leiate [Azure'i Arendaja tööriistad Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Erinevate keskmise suurusega tööriistad

[Mõned hea keskmise suurusega tööriistad (Steve Lasker ajaveebi)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Hea artikleid

[Sissejuhatus: NGINX Microservices](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Esitluste

- [Steve Lasker: VS reaalajas Las Vegas 2016 - keskmise suurusega e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [ASP.net-i Core tutvustus @ koostada 2016 - kuhu saate veebisaidil Demo](https://channel9.msdn.com/Events/Build/2016/B810)
- [Ümbriste kanali 9 .NET rakenduste arendamise](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
