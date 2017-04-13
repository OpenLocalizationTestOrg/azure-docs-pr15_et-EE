<properties 
    pageTitle="Tõrge autentimise automaattuvastus" 
    description="Active directory ühenduse viisard tuvastas mitteühilduvad autentimistüüp" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Tõrge autentimise automaattuvastus

Ajal eelmisele autentimise koodi tuvastamine, viisard tuvastas mitteühilduvad autentimistüüp.   

##<a name="what-is-being-checked"></a>Mis on kontrollitud?

**Märkus:** Selleks, et õigesti tuvastanud eelmise autentimise koodi projektis, peavad olema ehitatud projekt.  Kui see tõrge ilmnes ja teil pole eelmise autentimise koodi projektis, taastada ja proovige uuesti.

###<a name="project-types"></a>Projekti tüübid

Millist tüüpi projekti arendate nii, et see saab annavad õige autentimise loogika projekti kontrollib viisard.  Kui seal on mis tahes lehel kohta, mis tuleneb `ApiController` projekt, projekti käsitletakse WebAPI projekti.  Kui on ainult domeenikontrollerid tulenevad `MVC.Controller` projekt, projekti käsitletakse mõnda MVC projekti.  Midagi muud viisard ei toeta.  WebForms projektide pole praegu toetatud.

###<a name="compatible-authentication-code"></a>Ühilduvaid Taotluspõhiseid kood

Viisard otsib ka viisardi eelnevalt konfigureeritud või ühilduvad viisardi autentimissätted.  Kui kõik sätted on olemas, leitakse re-entrant juhtumi ja viisardi avaneb ja sätete kuvamine.  Kui ainult teatud sätted on olemas, leitakse mõne veaväärtuse juhul.

Projectis MVC viisard kontrollib järgmised sätted, mis tuleneb viisardi kasutamise:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Lisaks viisard otsib mõni järgmised sätted Veebiteenuste Projectis, mis tuleneb viisardi kasutamise:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Mitteühilduvad autentimise kood

Lõpuks viisardi proovib tuvastada autentimise koodi versioonides, mis on konfigureeritud Visual Studio varasemate versioonidega. Kui olete saanud selle vea, tähendab see, projekti sisaldab mitteühilduvad autentimistüüp. Viisardi tuvastab autentimise varasemate versioonide Visual Studio järgmist tüüpi:

* Windowsi autentimine 
* Kasutajakontode 
* Ettevõtte kontod 
 

Windowsi autentimine tuvastamiseks MVC projekti viisard otsib soovitud `authentication` elemendi **web.config** failist.

<pre>
    &lt;konfiguratsiooni&gt;
        &lt;System.Web mitu&gt;
            <span style="background-color: yellow">&lt;autentimisrežiim = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Windowsi autentimine tuvastamiseks Veebiteenuste projektis viisard otsib soovitud `IISExpressWindowsAuthentication` elemendi projekti **.csproj** failist:

<pre>
    &lt;Projekti&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;lubatud&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/projekti        &gt;
</pre>

Üksikute kasutajakontode autentimise tuvastamiseks viisard otsib paketi elemendi **Packages.config** failist.

<pre>
    &lt;pakettide&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework paketti" versioon = "2.1.0" targetFramework = "net45" /&gt;</span>
    &lt;/paketid&gt;
</pre>

Ettevõtte konto autentimine vanaks vormiks tuvastamiseks viisardi näeb välja järgmise elemendi **web.config**kaudu:

<pre>
    &lt;konfiguratsiooni&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;lisada võti = "ida: Domeen" väärtus = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Autentimise tüüp muutmiseks eemaldage mitteühilduvad autentimistüüp ja käivitage viisard uuesti.

Lisateabe saamiseks vt [Autentimise stsenaariumid Azure AD](active-directory-authentication-scenarios.md).