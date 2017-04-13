<properties 
    pageTitle="Azure Active Directory ja Visual Studio ühendatud teenused (MVC projektid) kasutamise alustamine | Microsoft Azure'i" 
    description="Kuidas alustada pärast ühenduse või Azure AD Visual Studio abil luua ühendus teenuste MVC projektide Azure Active Directory abil" 
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

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Alustamine: Azure Active Directory ja Visual Studio ühendatud teenused (MVC projektid)

> [AZURE.SELECTOR]
> - [Alustamine](vs-active-directory-dotnet-getting-started.md)
> - [Mis juhtus](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Nõuab autentimist Accessi vastutavatele 

Kõik domeenikontrollerid projekti olid kunagi **Autoriseerin** atribuuti. See atribuut nõuab enne nende kontrollerid autentida kasutaja. Kontrolleril anonüümselt juurdepääsu lubamiseks eemaldada see atribuut selle domeenikontrolleri. Kui soovite määrata veel Varundustöö taseme õigusi, rakendada iga meetod, mis nõuab autoriseerimine rakendamine kontrolleril tunni asemel atribuuti.
 
##<a name="adding-signin--signout-controls"></a>Lisades sisselogimine / SignOut juhtelemendid 

Lisada mõne vaate SignIn/SignOut juhtelementide abil saate **_LoginPartial.cshtml** osaline vaade funktsioonid lisada ühte oma vaated. Siin on näide standard **_Layout.cshtml** vaatesse lisatud funktsioone. (Pange tähele viimase elemendi div koos klassi navigeerimisriba – Ahenda):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Lisateavet Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
