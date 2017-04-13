<properties
    pageTitle="Pilveteenus integreerimine Azure CDN | Microsoft Azure'i"
    description="Õppeteema, mis õpetab juurutamise pilveteenuses, mis pakub sisu integreeritud Azure CDN lõpp"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Pilveteenus integreerimine Azure CDN-ID

Pilveteenus võib olla integreeritud Azure CDN-ID, mis tahes sisu on pilveteenuses asukohast serveeritakse. Seda moodust annab teile järgmised eelised:

- Lihtne juurutada ja pilte, skripte ja laadilehte oma pilveteenuses projekti kataloogidesse värskendamine
- Hõlpsasti uuendada oma pilveteenuses, nt jQuery või Bootstrap versioonide NuGet-paketid
- Oma veebirakenduse ja teie CDN-kätte sisu kõik sama Visual Studio liides haldamine
- Ühendatud juurutamise töövoo veebirakenduses ja CDN-kätte sisu
- ASP.net-i komplekteerimine ja minification integreerimine Azure CDN-ID

## <a name="what-you-will-learn"></a>Mida on õppida ##

Selles õppetükis saate teada, kuidas:

-   [Oma pilveteenuses integreerimine Azure CDN lõpp ja tähistavad staatiliseks sisuks veebilehtede: Azure'i CDN-ID](#deploy)
-   [Oma pilveteenuses staatiliseks sisuks vahemälu sätete konfigureerimine](#caching)
-   [Selle domeenikontrolleri toimingutest kaudu Azure'i CDN sisu](#controller)
-   [Serve kombineeritud ja minified sisu kaudu Azure'i CDN skripti silumine teenuse Visual Studio säilitades](#bundling)
-   [Konfigureerige Fall skripte ja CSS-i, kui teie Azure'i CDN on ühenduseta](#fallback)

## <a name="what-you-will-build"></a>Mis on koostamine ##

Saate juurutada pilvepõhise teenuse Web rolli Vaikimisi ASP.net-i MVC malli abil, teenida sisu on integreeritud Azure CDN, nt pilt, domeenikontrolleri toimingu tulemuste ja vaikimisi JavaScript ja CSS-i failid: koodi lisamine ja ka kirjutada koodi konfigureerida taandepäringud süsteem teenusepakettide serveeritud juhuks, kui on CDN on ühenduseta.

## <a name="what-you-will-need"></a>Mida on vaja ##

Selles õpetuses on järgmine kohustuslik tarkvara.

-   [Microsoft Azure'i konto](/account/) aktiivne
-   Visual Studio 2015 [Azure'i](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) SDK

> [AZURE.NOTE] Peate selle õpetuse lõpuleviimiseks Azure'i konto.
> + Saate [avada Azure'i konto tasuta](/pricing/free-trial/) – saate krediiti abil saate proovida makstud Azure'i teenuste ja isegi juhul, kui nad kasutada kuni hoiate konto ja kasutage tasuta Azure teenused, nt veebisaidid.
> + Saate [aktiveerida MSDN-i abonendi kasu](/pricing/member-offers/msdn-benefits-details/) – teie MSDN-i tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Pilveteenus juurutamine ##

Selles jaotises kuvatakse vaikimisi ASP.net-i MVC rakenduse malli Visual Studio 2015 juurutama pilvepõhise teenuse Web rolli ja seejärel integreerida uue CDN lõpp-punkti. Järgige alltoodud juhiseid:

1. Visual Studio 2015 loomine pilveteenuses Azure menüü minnes **Fail > uus > projekti > Cloud > Azure'i pilveteenuses**. Pange nimi ja klõpsake nuppu **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Valige **ASP.net-i Web roll** ja klõpsake soovitud **>** nupp. Klõpsake nuppu OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Valige **MVC** ja klõpsake nuppu **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Nüüd avaldada Web Sellel rollil on Azure pilveteenuses. Paremklõpsake pilvepõhise teenuse project ja valige käsk **Avalda**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Kui teil pole veel Microsoft Azure'i logitud, klõpsake ripploendit **... konto lisamine** ja valige menüüst **Lisa konto** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Sisselogimislehel, logige sisse Microsofti kontoga, mida kasutasite Azure'i konto aktiveerimiseks.
7. Kui olete sisse logitud, klõpsake nuppu **edasi**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Eeldades, et te pole loonud pilvepõhise teenuse või salvestusruumi konto, Visual Studio abil saate luua nii. **Pilveteenuses loomine ja konto** dialoogiboksis tippige soovitud teenuse nime ja valige soovitud ala. Klõpsake nuppu **Loo**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Kontrollige konfiguratsiooni avalda sätete lehel ja klõpsake nuppu **Avalda**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Avaldamise protsessi pilveteenustega võtab kaua aega. Funktsiooni lubamiseks Web juurutada kõikide rollide suvandi saate teha oma kiirem pilveteenusesse silumine esitada oma Web rollid kiiresti (vaid ajutised) värskendused. Selle suvandi kohta leiate lisateavet teemast [avaldamise pilveteenus Azure tööriistade abil](http://msdn.microsoft.com/library/ff683672.aspx).

    Kui **Microsoft Azure'i tegevuste Logi** näitab, et oleku avaldamine on **lõpule viidud**, loote CDN näitaja, mis on integreeritud selle pilveteenuses.

    >[AZURE.WARNING] Kui pärast avaldamine, kuvab juurutatud pilveteenuses viga ekraanil, tõenäoliselt seetõttu, et pilveteenuses, juurutamist kasutab [Külastajate OS, mis ei sisalda .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Selle probleemi lahendamiseks saate töötada kasutades [.NET 4.5.2 tööülesandena käivitamise](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>CDN uue profiili loomine

CDN profiil on kogumi CDN lõpp-punktid.  Iga profiil sisaldab ühe või mitme CDN lõpp-punktid.  Soovi korral võite kasutada mitut profiili CDN lõpp-punktide Interneti-domeeni, veebirakenduse või mõnda muude kriteeriumide järgi korraldada.

> [AZURE.TIP] Kui teil on juba CDN profiili, mida soovite kasutada selles õpetuses, jätkake [Loo uus CDN lõpp-punkti](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Uue CDN lõpp-punkti loomine

**On uus CDN lõpp-punkti loomiseks salvestusruumi konto jaoks**

1. [Azure'i haldusportaal](https://portal.azure.com), liikuge CDN profiili.  Teil võib-olla on kinnitatud selle armatuurlaua eelmises etapis.  Kui te pole, leiate selle nuppu **Sirvi**ja seejärel **CDN profiilid**ja profiili plaanite lisada oma lõpp-punkt.

    CDN profiil tera kuvatakse.

    ![CDN profiil][cdn-profile-settings]

2. Klõpsake nuppu **Lisa lõpp-punkti** .

    ![Lõpp-punkti nupp Lisa][cdn-new-endpoint-button]

    **Lisa lõpp** tera kuvatakse.

    ![Lõpp-punkti blade lisamine][cdn-add-endpoint]

3. Sisestage selle CDN lõpp-punkti **nimi** .  Selle nime kasutatakse juurdepääsu oma domeeni vahemällu talletatud ressursside `<EndpointName>.azureedge.net`.

4. Valige rippmenüüst **Origin tüüp** *pilveteenuses*.  

5. Valige rippmenüüst **Origin hostname** oma pilveteenuses.

6. Jätke vaikesätted **Origin tee**, **Origin hosti päise**ja **protokoll/päritolu port**.  Määrake vähemalt üks protocol (HTTP- või HTTPS).

7. Klõpsake nuppu **Lisa** uus lõpp-punkti loomiseks.

8. Kui lõpp-punkti on loodud, kuvatakse see loendis profiili lõpp-punktid. Loendivaate näitab URL-i abil vahemällu talletatud sisu, samuti origin domeeni.

    ![CDN lõpp-punkti][cdn-endpoint-success]

    > [AZURE.NOTE] Lõpp-punkti kohe ei kasutamiseks saadaval.  Võib kuluda kuni 90 minutit registreerimise levitada CDN võrgu kaudu. Kasutajad, kes proovida kasutada CDN domeeninime kohe saada kuni sisu on saadaval on CDN olekukoodi 404.

## <a name="test-the-cdn-endpoint"></a>Testige CDN lõpp-punkti

Kui avaldamise olek on **lõppenud**, avage brauseriaken ja liikuge * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. Minu häälestus see URL on:

    http://camservice.azureedge.net/Content/bootstrap.css

Mis vastab veebisaidil CDN lõpp-punkti järgmine origin URL:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Kui saate liikuda * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, sõltuvalt teie brauser teil palutakse alla laadida või avada bootstrap.css, mis oli avaldatud Web Appist.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Pääsete sarnaselt mis tahes avalikult juurdepääsetava URL-i veebisaidil * *http://*&lt;teenuse nimi >*.cloudapp.net/** otse oma CDN lõpp-punkti. Näiteks:

-   Js faili tee /Script
-   Mis tahes sisu emajaotises faili tee
-   Mis tahes kontrolleril/toiming
-   Kui päringu string on lubatud teie CDN lõpp-punkti, mis tahes URL päringus

Tegelikult ülaltoodud konfiguratsiooni, saate majutada kogu pilveteenusesse: * *http://*&lt;cdnName >*.azureedge.net/**. Kui liigun **http://camservice.azureedge.net/ ** ma saan toimingu tulem Avaleht/register.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

See ei tähenda, et see on alati hea mõte (või üldiselt on hea mõte) kaudu Azure'i CDN on kogu pilveteenusesse. Mõned piirangud on:

-   Seda moodust nõuab terve saidi on avalikud, kuna Azure'i CDN ei saa olla mis tahes tundliku sisuga sel ajal.
-   Kui mingil põhjusel läheb ühenduseta CDN lõpp-punkti, kas ajastatud hooldustööd või kasutaja viga, teie kogu pilveteenuses läheb ühenduseta juhul, kui kliendid saate suunata origin URL-i * *http://*&lt;teenuse nimi >*.cloudapp.net/**.
-   Isegi kui tasute kohandatud olla sätted (vt [konfigureerimine vahemällu suvandite staatilisi faile oma pilveteenuses](#caching)), on CDN lõpp-punkti parandada väga dünaamiline sisu. Kui olete proovinud avalehe laadida oma CDN lõpp-punkti nagu eespool näidatud teade, mis kulus vähemalt 5 sekundit laadida vaikimisi avalehe esimest korda, mis on üsna lihtne leht. Kujutage ette, mis juhtub klientide programmikasutuskogemuse, kui see leht sisaldab dünaamilise sisu, mis tuleb värskendada iga minut. Dünaamiline sisu CDN lõpp serveeritakse nõuab lühike vahemälu aegumise, mis tähendab, et sagedased vahemälu jätab veebisaidil CDN lõpp-punkti. See valus oma pilveteenuses jõudlus ja kaotustest CDN-ID eesmärk.

Teise võimalusena on kindlaks määrata, millist sisu oma pilveteenuses eraldi: Azure'i CDN kätte. Selleks on juba kuidas pääseda juurde üksikuid sisu faile CDN lõpp näinud. Ma näitab teile, kuidas teenida teatud kontrolleril toimingu CDN lõpp-punkti kaudu [sisu kontrolleril toimingud kaudu Azure'i CDN kaudu](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Staatilise failide vahemälu suvandid saate konfigureerida oma pilveteenuses ##

Azure'i CDN integreerimise oma pilveteenuses, saate määrata, kuidas soovite staatilisi sisu kopeerida CDN lõpp-punkti. Selle tegemiseks *Web.config* avamine Web rolli projekti (nt WebRole1) ja lisage soovitud `<staticContent>` elemendi `<system.webServer>`. XML-i allpool konfigureerib vahemälu 3 päeva pärast aegumist.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Kui te ei tee seda, järgib kõik staatilise failid oma pilveteenuses vahemälu CDN sama reegel. Täpsema võimalusi vahemälusätete lisada faili *Web.config* kausta ja lisage oma sätteid seal. Näiteks lisada faili *Web.config* *\Content* kausta ja asendage sisu järgmine XML-i.

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Selle sätte valimisel staatilise kõik failid kausta *\Content* 15 päeva jooksul kopeerida.

Lisateavet selle kohta, kuidas konfigureerida selle `<clientCache>` element, lugege teemat [Kliendi vahemälu &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

[Sisu kontrolleril toimingud kaudu Azure'i CDN kaudu](#controller), ma ka näitan saate kuidas seadistada vahemälusätete kontrolleril toimingu tulemuste CDN vahemälu.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Selle domeenikontrolleri toimingutest kaudu Azure'i CDN sisu ##

Kui pilve teenuse Web rolli integreerimine Azure CDN on üsna lihtne sisu kontrolleril toimingud kaudu Azure'i CDN kaudu. Peale serveeritakse oma pilveteenuses otse Azure'i CDN-ID (näidanud kohal), [Maarten Balliauw](https://twitter.com/maartenballiauw) näitab, kuidas seda teha lõbus MemeGenerator domeenikontrolleri sisse [vähendamine latentsus Azure'i CDN veebis](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Ma lihtsalt paljundada see siin.

Oletame, et teie pilveteenuses soovite luua memes põhjal on young Chuck Norris pilt ( [Hele Alan](http://www.flickr.com/photos/alan-light/218493788/)foto) umbes järgmine:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Teil on lihtne `Index` toiming, mis võimaldab kliente, määrake soovitud ülivõrrete pilt, loob Meem, kui nad postitada toiming. Kuna see on Chuck Norris, ootate saada metsikult populaarne globaalselt sellelt lehelt. See on hea näide serveeritakse koos Azure CDN pooleldi dünaamiline sisu.

Järgige eespool häälestamine kontrolleril toimingut:

1. Klõpsake kaustas *\Controllers* nimega *MemeGeneratorController.cs* .cs uue faili loomine ja asendage sisu järgmine kood. Kindlasti esiletõstetud osa asendamine oma CDN nime.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Paremklõpsake vaikimisi `Index()` toiming ja valige **Lisa vaade**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Nõustuge järgmisi sätteid ja klõpsake nuppu **Lisa**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Avage uus *Views\MemeGenerator\Index.cshtml* ja asendage sisu järgmine lihtne HTML on ülivõrrete esitamise:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Avaldamine pilveteenusesse uuesti ja liikuge * *http://*&lt;teenuse nimi >*.cloudapp.net/MemeGenerator/Index** brauseris.

Kui edastate vormi väärtused asukohta `/MemeGenerator/Index`, `Index_Post` toimingu meetod tagastab link on `Show` vastav Sisestuskeel identifikaatoriga toimingu meetod. Kui klõpsate linki, jõuate järgmine kood:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Kui teie kohaliku Silur on ühendatud, siis saate tavalise silumine kogemus kohaliku suunata. Kui see töötab pilveteenusesse, siis see suunab ümber:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Mis vastab järgmine origin URL veebisaidil oma CDN lõpp-punkt:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Seejärel saate soovitud `OutputCacheAttribute` atribuut klõpsake soovitud `Generate` meetodi abil saate määrata, kuidas toimingu tulem peaks olema vahemälus talletatud, mis Azure'i CDN au. Alljärgnev kood Määrake vahemälu aegumise on 1 tund (3600 sekundit).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Samuti saab olla mis tahes kontrolleril toimingu sisu üles oma pilveteenuses kaudu oma Azure'i CDN, koos vahemällu soovitud suvand.

Järgmise jaotise ma näitab teile, kuidas kogumisse seotud ja minified skripte ja CSS-i kaudu Azure'i CDN.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.net-i komplekteerimine ja minification integreerimine Azure CDN-ID ##

Skripte ja CSS-i laadilehte muuta harva ja peamine, saavad Azure'i CDN vahemälu. Lihtsaim viis komplekteerimine ja minification integreerimine Azure CDN serveeritakse kogu Web rolli kaudu oma Azure'i CDN on. Juhul, kui soovite võib-olla ei selleks, ma näitab teile, kuidas seda teha samal ajal säilitamine soovitud develper kogemusi ja ASP.net-i komplekteerimine minification, näiteks:

-   Hea silumine režiimi kogemus
-   Sujuv juurutamine
-   Kohe värskenduste klientidele skripti/CSS-i versiooni uuendamine
-   Kui teie CDN lõpp-punkti nurjub taandepäringud süsteem
-   Minimeerida koodi muutmine

Avage [integreerida sisuga oma Azure veebisaitide ja serve staatiline: Azure'i CDN veebilehtede lõpp Azure'i CDN](#deploy)loodud Projectis **WebRole1** *App_Start\BundleConfig.cs* ja Heitke pilk selle `bundles.Add()` meetodi kutsed.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Esimene `bundles.Add()` lause lisab skripti komplekt virtuaalse Directory `~/bundles/jquery`. Avage *Views\Shared\_Layout.cshtml* näha, kuidas skripti komplekt silt on muutunud. Peaks saama otsimine Razori kood järgmine rida.

    @Scripts.Render("~/bundles/jquery")

Järgmine kood Razori käitamisel Azure Web roll muudab on `<script>` sildi skript koguhinna umbes järgmine:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Juhul, kui see töötab Visual Studios, tippides `F5`, see muudab iga skriptifail kogumisse eraldi (ülaltoodud juhul ainult üks skriptifail on kogumisse):

    <script src="/Scripts/jquery-1.10.2.js"></script>

See võimaldab teil oma arenduskeskkond JavaScripti koodi silumine ajal vähendada samaaegseid klientrakenduse kaudu (liitmine) ja parandada faili alla laadida jõudluse (minification) valmistamisel. See on suurepärane funktsioon Azure'i CDN integreerimise säilitada. Lisaks sulatatud komplekt sisaldab juba automaatselt loodud versioon on string, soovite paljundada funktsioone seega on iga kord, kui värskendate oma jQuery versiooni kaudu Nugeti, värskendatakse seda kliendipoolse nii kiiresti kui võimalik.

Järgige allpool integreerimine ASP.net-i komplekteerimine ja minification koos oma CDN lõpp-punkti.

1. Olles tagasi kohas *App_Start\BundleConfig.cs*, muuta selle `bundles.Add()` meetodit kasutada eri [komplekt konstruktori](http://msdn.microsoft.com/library/jj646464.aspx), mis määrab CDN meiliaadressi. Selle tegemiseks asendada selle `RegisterBundles` meetod määratlus järgmine kood:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Asendage `<yourCDNName>` oma Azure'i CDN nimi.

    Lihtsate sõnadega, saate määrata `bundles.UseCdn = true` ja hoolikalt CDN URL-i lisatakse iga komplekt. Näiteks esimese ehitaja kood:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    sama, mis on:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Seda konstruktorit ütleb ASP.net-i komplekteerimine ja minification renderdada üksikute skripti faile, kui kohalik silumisel, kuid juurdepääs kõnealuse skripti määratud CDN aadress abil. Pange tähele kahte olulist omadused seda hoolikalt koostatud CDN URL:

    -   Seda URL-i CDN pärineb `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, mis on tegelikult virtuaalne kataloog skripti kogumisse oma pilveteenuses.
    -   Kuna kasutate CDN konstruktori, sisaldab CDN skripti sildi koguhinna enam automaatselt loodud versioon stringi sulatatud URL-is. Iga kord, kui skript kogumisse on muudetud jõustamine veebisaidil oma Azure'i CDN vahemälu vahele, tuleb ainulaadne versiooni stringi käsitsi luua. Samal ajal kordumatu versioon stringile tuleb need ei muutu elu maksimeerimiseks Vahemälutabamusi veebisaidil oma Azure'i CDN pärast kogumisse juurutamist juurutamise kaudu.
    -   Päringu stringi v = < W.X.Y.Z > tõmbab kaudu *Properties\AssemblyInfo.cs* projektis Web roll. Teil võib olla juurutamise töövoo, mis sisaldab suurendades komplekti versioon iga kord, kui avaldate Azure. Või saate muuta ainult *Properties\AssemblyInfo.cs* automaatselt inkrementida stringi versioon iga kord, kui koostate Metamärgi projektis "*". Näiteks:

            [assembly: AssemblyVersion("1.0.0.*")]

        Mis tahes muud strateegia loomisel kordumatu stringi elu juurutuse täiustamiseks töötab siin.

3. Uuesti avaldada pilveteenusesse ja juurdepääs avalehele.

4. Saate vaadata lehe HTML-koodi. Mida peaks olema näeksid renderdamise stringiga kordumatu versioon iga kord, kui avaldate muudatusi teie pilveteenusesse CDN URL-i. Näiteks:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. Visual Studio silumine pilveteenuses Visual Studios, tippides `F5`.,

6. Saate vaadata lehe HTML-koodi. Näete iga skriptifail eraldi muudetakse nii, et olete ühtsete silumine, kogemusi Visual Studio.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Taandepäringud süsteem CDN URL-id ##

Kui teie Azure'i CDN lõpp-punkti ei õnnestu mingil põhjusel, soovite olla piisavalt tark, et juurdepääsu oma origin veebiserver taandepäringud laadimine JavaScripti või alglaaduri suvandi teie veebilehele. See on raske, tõttu CDN andnud, kuid palju raskem kaotada oluline lehe funktsioonid oma skripte ja laadilehte veebisaidil piltide kaotsi minna.

Klassi [komplekt](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) sisaldab [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , mille abil saate konfigureerida taandepäringud süsteem CDN rikkumise atribuudi. Kasutatav atribuut, tehke järgmist.

1. Web rolli projektis Avage *App_Start\BundleConfig.cs*, kuhu lisasite CDN URL-i iga [kogumi ehitaja](http://msdn.microsoft.com/library/jj646464.aspx), ja muuta esiletõstetud vaikimisi kimbud taandepäringud süsteemi lisamiseks:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Kui `CdnFallbackExpression` on pole tühi skript süstitakse HTML-i kontrollida, kas kogumisse on edukalt laaditud ja kui pole, juurdepääs kogumisse otse origin veebiserver. Atribuut peab olema seatud JavaScripti avaldis, mis kontrollib, kas vastav CDN kogumisse laaditakse õigesti. Avaldis, mis on vaja testida iga kogumi kleepimistoimingud sõltuvad sisu. Vaikimisi teenusepakettide ülaltoodud:

    -   `window.jquery`on määratletud JS jquery-{versioon}
    -   `$.validator`on määratletud jquery.validate.js
    -   `window.Modernizr`on määratletud JS modernizer-{versioon}
    -   `$.fn.modal`on määratletud bootstrap.js

    Te olete märganud, et mul pole määratud CdnFallbackExpression jaoks soovitud `~/Cointent/css` komplekt. See on, kuna seal on praegu [System.Web.Optimization viga](https://aspnetoptimization.codeplex.com/workitem/104) , mis serveripoolse on `<script>` sildi taandepäringud CSS asemel eeldatud `<link>` sildi.

    Siiski on hea [Laadi komplekt Fall](https://github.com/EmberConsultingGroup/StyleBundleFallback) pakutud [Hõõguvas nõustamisteenuseid rühma](https://github.com/EmberConsultingGroup).

2. CSS-i lahendus kasutamiseks Web rolli projekti *App_Start* kaust nimega *StyleBundleExtensions.cs*.cs uue faili loomine ja asendada selle sisu [GitHub koodi](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. *App_Start\StyleFundleExtensions.cs*, Nimeta ümber nimeruumi Web rolli nimele (nt **WebRole1**).

3. Minge tagasi `App_Start\BundleConfig.cs` ja muutke Viimane `bundles.Add` lause esiletõstetud järgmine kood:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Uue laiendi meetodi kasutab sama idee sisestab skript HTML kontrollida DOM jaoks soovitud kattuvad ainekursuse nime, reegli nimi ja reegli väärtus määratletud CSS-i komplekt ja langeb tagasi origin veebiserver, kui ta ei leia soovitud vaste.

4. Avaldamine pilveteenusesse uuesti ja juurdepääs avalehele.

5. Saate vaadata lehe HTML-koodi. Tuleks leida paigutatud skriptide umbes järgmine:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Pange tähele, et paigutatud koguhinna CSS-i skripti sisaldab endiselt Hairahtunut jäänuk on `CdnFallbackExpression` atribuudi real:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Kuid kuna esimene osa on || avaldis tagastab alati väärtuse true (Klõpsake joont selle kohal), mitte kunagi kestab document.write() funktsiooni.

## <a name="more-information"></a>Lisateave ##
- [Azure'i Sisuedastusvõrgud (CDN) ülevaade](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Azure'i CDN abil](cdn-create-new-endpoint.md)
- [ASP.net-i komplekteerimine ja Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
