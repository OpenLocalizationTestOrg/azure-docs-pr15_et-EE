<properties
    pageTitle="Oma esimese web appi funktsioonide lisamine"
    description="Lahedad funktsioonid lisada oma esimese veebirakenduse paar minutit."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Oma esimese web appi funktsioonide lisamine

Klõpsake [Deploy oma esimese veebirakenduse viis minutit Azure](app-service-web-get-started.md), juurutatud valimi web appi [Azure'i rakendust Service](../app-service/app-service-value-prop-what-is.md). Selles artiklis kuvatakse mõned hea funktsioonid lisada kiiresti oma juurutatud veebirakenduse. Paari minutiga, siis:

- Jõusta kasutajate autentimine
- rakenduse automaatselt skaala
- saada teatisi oma rakenduse toimimise kohta

Sõltumata sellest, milline valimi rakendus saate juurutada eelmises artiklis, võite järgida õpetuses.

Selles õpetuses kolme tegevuste on vaid mõned näited palju kasulikke funktsioone, mis kuvatakse, kui lisate oma veebirakenduse rakenduse teenus. Paljud funktsioonid on saadaval **tasuta** esimese taseme (mis on, mida oma esimese veebirakenduse töötab), ja saate proovida funktsioone, mis nõuavad kõrgem hinnad astme oma prooviversiooni krediidi summa liitmisel. Kindel, et oma veebirakenduse jääb **tasuta** astme juhul, kui te konkreetselt muudab seda eri hinnakirjad taseme.

>[AZURE.NOTE] Azure'i CLI loodud veebirakenduse töötab **tasuta** taseme kohta, mis võimaldab ainult ühe ühiskasutusega VM eksemplari serveriressursikvootide. Mida saate koos **tasuta** taseme kohta leiate lisateavet teemast [rakenduse teenuste piirangud](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Oma kasutajate autentimiseks

Nüüd, kui lihtne on lisada autentimine (Välislingid veebisaidil [Rakenduse teenuse autentimise/autoriseerimine](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)) rakenduse vaatame.

1. Portaali tera oma rakenduse, mis just avanud, klõpsake nuppu **sätted** > **autentimine / luba**.  
    ![Autentida - sätted blade](./media/app-service-web-get-started/aad-login-settings.png)

2. Klõpsake **sisse** lülitada autentimist.  

4. Klõpsake **Autentimisteenuse pakkujate** **Azure Active Directory**.  
    ![Autentida – valige Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. Labale **Azure Active Directory sätted** nuppu **Express**ja seejärel klõpsake nuppu **OK**. Vaikesätteid saate luua uue Azure AD Rakenduse kataloogis vaikimisi.  
 ![Autentida – kiire konfigureerimine](./media/app-service-web-get-started/aad-login-express.png)

6. Klõpsake nuppu **Salvesta**.  
    ![Autentida - Salvesta konfigureerimine](./media/app-service-web-get-started/aad-login-save.png)

    Kui muudatus on edukalt, kuvatakse teile teatis Gaussi, roheliseks koos sõbralik sõnumi.

7. Uuesti portaali tera rakenduse, klõpsake **URL-i** link (või **otsige** menüüriba). Link on HTTP-aadress.  
    ![Autentida – sirvige URL-i](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Aga kui see avatakse uuel vahekaardil rakendus, URL väljale ümbersuunamised mitu korda ja lõpetab oma rakenduse HTTPS-aadress. Mida te näete, et olete juba sisse loginud, Azure tellimust ja te olete automaatselt autenditud rakenduses.  
    ![Autentida – sisse logitud](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Nii, et kui te nüüd autentimata seansi avamine mõnes muus brauseris, kuvatakse sisselogimise Kuva kui liigute sama URL-i.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Kui te pole kunagi teinud midagi Azure Active Directory, ei pruugi teie vaikekataloogi Azure AD kasutajad. Sel juhul ilmselt ainult konto seal on Microsofti konto Azure'i tellimus. Miks on rakendus samas brauseris varem olid automaatselt sisse logitud.
   Saate selle sisselogimise lehel sisse sama Microsofti kontoga.

Palju õnne, mida on autentimine kogu liikluse oma veebirakenduse.

Olete ehk märganud selle **autentimine / autoriseerimine** tera, mida saate teha veel palju, näiteks:

- Luba social sisselogimine
- Luba mitu login suvandid
- Kui inimesed saavad liikuda esmalt oma rakenduse vaikekäitumise muutmiseks

Rakenduse teenus pakub võtmed lahenduse mõned levinud autentimine peab seetõttu ei pea te autentimise loogika esitada.
Lisateabe saamiseks vt [Rakenduse teenuse autentimise/autoriseerimine](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Rakenduse põhjal automaatselt nõudmisel skaala

Järgmise, vaatame autoscale rakenduse, et see automaatselt reguleerida seda võimet vastata kasutaja nõudmisel (Välislingid [häälestamine rakenduse Azure skaala](web-sites-scale.md) ja [skaala arvu automaatselt või käsitsi](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Lühidalt, muudate oma veebirakenduse kahel viisil:

- [Skaalal](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): CPU, mälu, kettaruumi ja lisafunktsioone nagu sihtotstarbeline VMs, kohandatud domeene ja sertifikaatide kohta, lavastus slots, autoscaling ja muud. Skaalal, muutes oma rakenduse kuulub rakenduse teenusleping hinnakirjad astme.
- [Välja skaala](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): VM arvu eksemplarid, mis töötavad rakenduse.
Saate muudate koguni 50 eksemplarides, sõltuvalt teie hinnakirjad taseme.

Ilma ado, alustame häälestada autoscaling.

1. Kõigepealt loome skaala kuni autoscaling lubamiseks. Portaali tera rakenduse, klõpsake nuppu **sätted** > **Skaala ülespoole (rakenduse teenuse leping)**.  
    ![Skaalal - sätted blade](./media/app-service-web-get-started/scale-up-settings.png)

2. Leidke ja valige **S1 Standard** taseme, alumise taseme, mis toetab autoscaling (circled kuvatõmmis) ja seejärel klõpsake nuppu **Vali**.  
    ![Skaalal - Vali taseme](./media/app-service-web-get-started/scale-up-select.png)

    Olete valmis skaleerimise üles.

    >[AZURE.IMPORTANT] Selle taseme kulutab oma tasuta prooviversiooni autorid. Kui teil on konto maksma-kasutada, selle andmesidekasutusele lisanduvad kontole.

3. Järgmiseks loome konfigureerimine autoscaling. Portaali tera rakenduse, klõpsake nuppu **sätted** > **Skaala välja (rakenduse teenuse leping)**.  
    ![Välja - sätted blade skaala](./media/app-service-web-get-started/scale-out-settings.png)

4. Saate muuta **CPU protsent** **skaala järgi** . Liugurite all olevat ripploendi vastavalt uuendada. Määratlege **eksemplarid** vahemikus **1** ja **2** ning **sihtväärtust** **40** - **80**. Kas tippides väljadele või, teisaldades liugureid.  
 ![Välja mastaapimiseks – autoscaling konfigureerimine](./media/app-service-web-get-started/scale-out-configure.png)

    Selle konfiguratsiooni põhjal, rakenduse automaatselt skaala välja kui Protsessori kasutuse on suurem kui 80% ja skaala rakenduses alla 40% CPU kasutamise korral.

5. Klõpsake menüüribal nuppu **Salvesta** .

Palju õnne, teie rakendus asub autoscaling.

Olete ehk märganud blade **Skaala sätteid** , mida saate teha veel palju, näiteks:

- Skaala teatud arvul käsitsi
- Tulemuslikkuse näitajad, nt mälu protsent või ketas järjekorda, skaala
- Skaleerimise käitumist jõudluse reegli käivitamisel
- Autoscale ajakava
- Tulevikus autoscaling käitumise seadmine

Ülespoole rakenduse kohta leiate lisateavet teemast [Azure rakenduse skaalal](../app-service-web/web-sites-scale.md). Skaleerimist välja kohta leiate lisateavet teemast [mastaapimiseks arvu automaatselt või käsitsi](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Saada teatisi oma rakenduse

Nüüd, kui teie rakendus on autoscaling, mis juhtub, kui see jõuab soovitud maksimaalse arvu (2) ja CPU on üle soovitud kasutamine (80%)?
Saate häälestada (edasi lugemine [teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) teatise teatada sellest nii, et saate täpsemaks mastaapimiseks üles/välja rakenduse, näiteks. Vaatame kiiresti häälestada teatise seda stsenaariumi.

1. Klõpsake rakenduse portaali labale **Tööriistad** > **teatised**.  
    ![Teatiste - sätted blade](./media/app-service-web-get-started/alert-settings.png)

2. Klõpsake nuppu **Lisa teatis**. Seejärel valige dialoogiboksis **Resource** ressurss, mis lõpeb **(serverfarms)**. Mis on teie rakendus teenusleping.  
    ![Teatiste - teatise jaoks rakenduse teenusleping lisamine](./media/app-service-web-get-started/alert-add.png)

3. Määrake **nimi** `CPU Maxed`, **meetermõõdustik** **CPU protsent**ja **lävi** `90`, seejärel valige **e-posti omanikud, kaasautorid, ja lugejad**ja seejärel klõpsake nuppu **OK**.   
 ![Konfigureerida teatiste - teade](./media/app-service-web-get-started/alert-configure.png)

    Kui Azure lõpetab teatise loomine, näete seda tera **teatised** .  
    ![Teatiste - lõpetanud kuvamine](./media/app-service-web-get-started/alert-done.png)

Palju õnne, nüüd kostub teatised.

See teatis säte kontrollib Protsessori kasutuse iga viie minuti järel. Kui see arv läheb üle 90%, saate meiliteatise koos kõigile, kellel on lubatud. Kui soovite vaadata igaüks, kellel on õigus saada teatisi, minge tagasi oma rakenduse portaali tera ja klõpsake nuppu **Access** .  
![Vaadake, kes saab teatised](./media/app-service-web-get-started/alert-rbac.png)

Peaksite nägema **tellimuse administraatorid** on juba rakenduse **omanik** . Selle rühma kaasaks, kui olete konto administraator Azure tellimuse (nt proovitellimuse). Azure'i Rollipõhine juurdepääsu reguleerimine kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Teatiste reeglid on Azure funktsioon. Lisateavet leiate teemast [teatiste vastuvõtmine](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Järgmised sammud

Teatise konfigureerimiseks teed te olete ehk suurel hulgal erinevaid **Tööriistad** tera tööriistad. Siin saate otsida probleemid, kuvari jõudluse tagamiseks nõrkade kohtade testimine, ressursside haldamine, suhelda VM konsooli ja lisada kasulik laiendid. Kutsume klõpsake igat nende tööriistade avastada sõrmega vihjeid lihtsat, ent võimsat tööriistu.

Vaadake, kuidas teha veel oma juurutatud rakendusega. Siin on ainult osaline loend.

- [Osta ja konfigureerida kohandatud domeeninime](custom-dns-web-site-buydomains-web-app.md) - ostmine on atraktiivseks domeen, mitte veebirakenduse jaoks soovitud *. azurewebsites.net domeeni. Või kasutage domeen, mida teil on juba olemas.
- [Häälestamine lavastus keskkonnas](web-sites-staged-publishing.md) – Juurutage rakendus enne tootmisse lavastus URL. Värskendage oma reaalajas veebirakenduse confidence. Saate häälestada DevOps töötada lahenduse mitme juurutamise teenindusaegade.
- [Pideva häälestamine](app-service-continuous-deployment.md) – rakendusejuurutuse integreerida arvutisüsteemi Juhtelemendi allikas. Juurutada Azure koos iga kinnitamine.
- [Accessi kohapealse ressursid](web-sites-hybrid-connection-get-started.md) - olemasoleva Accessi kohapealse andmebaasi või süsteemi CRM.
- [Rakenduse varundamiseks](web-sites-backup.md) – häälestamine tagasi üles ja veebirakenduse jaoks taastada. Ootamatud tõrked ettevalmistamine ja need taastada.
- [Luba diagnostikalogid](web-sites-enable-diagnostic-log.md) - lugeda IIS-i logid Azure või rakenduse jälgi. Loe neid jooksvas, laadige need alla või port need [Rakenduse ülevaated](../application-insights/app-insights-overview.md) võtmed analüüsi jaoks.
- [Skannige oma rakenduse nõrkade kohtade](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
oma veebirakenduse [Hõbepaber turvalisuse](https://www.tinfoilsecurity.com/)teenuse abil tänapäevane ohtude skannida.
- [Tausta tööde](../azure-functions/functions-overview.md) - Käivita tööd andmete töötlemiseks aruandluseks jne.
- [Siit saate teada, rakendust Service tööpõhimõtted](../app-service/app-service-how-works-readme.md)
