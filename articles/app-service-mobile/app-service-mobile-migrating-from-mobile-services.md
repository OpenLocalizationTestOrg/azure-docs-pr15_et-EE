<properties
    pageTitle="Mobiil teenuste migreerida rakenduse teenuse Mobile'i rakendus"
    description="Saate teada, kuidas hõlpsalt migreerida rakenduse teenuse mobiilirakenduse rakenduse Mobile teenused"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Migreerimine olemasoleva Azure mobiilsideteenuse Azure'i rakendust Service

[Azure'i rakenduse teenuse üldiselt kättesaadav], Azure Mobile teenused saitidel hõlpsasti migreeritud kohapealne ära kõik funktsioonid Azure'i rakendust Service.  Selles dokumendis selgitatakse, mis ootab saidi migreerimine versioonist Azure Mobile teenused Azure'i rakendust Service.

## <a name="what-does-migration-do"></a>Mida teeb migreerimise saidile

Migreerimine oma Azure mobiilsideteenuse muutub teie mobiiliteenuse [Azure'i rakendust Service] rakenduse koodi mõjutamata.  Teie teate jaoturi, SQL-i andmeühendust, autentimissätted, ajastatud tööd ja domeeninime ei muudeta.  Oma Azure mobiilsideteenuse kasutamise mobiilikliendid Töövool tavapäraselt edasi.  Migreerimise taaskäivitamist teenust, kui see on üle kantud Azure'i rakendust Service.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Miks peaks migreerimine saidile

Microsoft soovitab migreerimiseks Azure mobiilsideteenuse ära funktsioonid rakenduse Azure'i teenus, sealhulgas:

  *  Uue host funktsioonid, sh [WebJobs] ja [kohandatud domeeni nime].
  *  Ühenduvust teie kohapealne vahendeid [VNet] Lisaks [Hübriid ühenduste]abil.
  *  Jälgimine ja uus reliikvia või [Rakenduse ülevaated]tõrkeotsing.
  *  Sisseehitatud DevOps instrumentaarium, sh [lavastus slots]hinnaalanduse, ja klõpsake tekitamiseks testimine.
  *  [Mastaabi automaatselt], koormusetasakaalustuseks ja [jõudluse jälgimine].

Azure'i rakendust Service eeliste kohta lisateabe saamiseks lugege teemat [Teenuste vs rakenduse mobiilsideteenuse] .

## <a name="before-you-begin"></a>Enne alustamist

Enne alustamist suur töö saidil, peaksite [varundada oma mobiilsideteenuse] skripte ja SQL-andmebaasi.

## <a name="migrating-site"></a>Teie saitide migreerimine

Migreerimisprotsessi migreerib Azure ühe piirkonna kõik saidid.

Saidi migreerimine:

  1.  [Azure'i klassikaline portaali]sisse logida.
  2.  Valige soovitud mobiilsideteenuse ala, mida soovite migreerida.
  3.  **Migreerimine teenusesse rakenduse** nuppu.

    ![Nupp Migrate][0]

  4.  Vt rakenduse teenuse dialoogiboks migreerimine.
  5.  Sisestage väljale teie mobiiliteenuse nimi.  Näiteks kui teie domeeni nimi on contoso.azure-mobile.net, seejärel sisestage _contoso_ vastavale väljale.
  6.  Klõpsake nuppu rist.

Activity monitoris migreerimise oleku jälgimine. Oma saidi loendis *migreerimine* Azure'i klassikaline portaalis.

  ![Migreerimise tegevuse jälgimine][1]

Iga migreerimise võib kuluda 3 15 minutit migreerida mobiilsideteenuse kohta.  Teie sait jääb saadaval üleviimise ajal.
Oma saidi taaskäivitamist migreerimisprotsessi lõpus.  Sait on saadaval käigus uuesti, mis võib kesta paar minutit.

## <a name="finalizing-migration"></a>Migreerimise lõpuleviimine

Lepingu testimiseks migreerimisprotsessi lõppedes mobiiliklienti saidile.  Veenduge, et saab toiminguid kõigi levinud kliendi mobile kliendi muutmata.  

### <a name="update-app-service-tier"></a>Valige sobiv rakenduse teenuse hinnad taseme

Teil on rohkem paindlikkust hinnad pärast seda, kui migreerite Azure'i rakendust Service.

  1.  [Azure'i portaali]sisse logida.
  2.  Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3.  Sätete tera avaneb vaikimisi.
  4.  Klõpsake menüüs sätted **Rakenduse teenuse kavandamine** .
  5.  Klõpsake paani **Hinnad taseme** .
  6.  Klõpsake paani asjakohane teie vajadustele, seejärel klõpsake käsku **Vali**.  Võib-olla peate klõpsake nuppu **Kuva kõik** kuvamiseks saadaval hinnakirjad astme.

Lähtepunktina, soovitame tasemete seas järgmist:

| Mobiilsideteenuse hinnad taseme | Rakenduse teenuse hinnad kiht |
| :-------------------------- | :----------------------- |
| Tasuta                        | Tasuta F1                  |
| Tavaline                       | B1 Basic                 |
| Standard                    | S1 Standard              |

Valides taseme rakenduse hinnad õige on paindlikkus.  Vaadake [Rakenduse teenuse hinnad] üksikasjalik oma uue rakenduse teenuse hinnad.

> [AZURE.TIP]Rakenduse teenuse Standard taseme sisaldab mitmeid funktsioone, mida soovite kasutada, juurdepääsu [lavastus slots]automaatvarunduse, sh ja automaatse skaleerimist.  Kui teil on, vaadake uued võimalused!

### <a name="review-migration-scheduler-jobs"></a>Vaadake üle migreeritud Scheduler tööde haldamine

Töö Scheduler ei ole nähtav, kuni 30 minutit pärast migreerimist.  Ajastatud tööd jätkata taustal.
Pärast seda, kui need on nähtavad uuesti oma ajastatud tööd vaatamine

  1.  [Azure'i portaali]sisse logida.
  2.  Valige **sirvimine >**, sisestage väljale _Filter_ **ajakava** ja seejärel valige **Scheduler saidikogumid**.

On piiratud arv tasuta ajasti töö saadaval migreerimisjärgsed.  Vaadake üle oma kasutus- ja [Azure Scheduler lepingud].

### <a name="configure-cors"></a>Konfigureerimine CORS vajaduse korral

Rist-päritolu ressursside ühiskasutus on tehnika veebisaidil Web API eri domeenis juurdepääsu lubamine.  Kui kasutate Azure Mobile teenused on seotud veebisaidi, siis peate konfigureerima CORS migreerimise osana.  Kui avate Azure Mobile teenused ainult mobiilsideseadmete kaudu, siis CORS pole vaja konfigureerida peale harva.

Teie migreeritud CORS sätted on saadaval **MS_CrossDomainWhitelist** rakenduse säte.  Oma saidi migreeritavad rakenduse teenuse CORS poole

  1.  [Azure'i portaali]sisse logida.
  2.  Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3.  Sätete tera avaneb vaikimisi.
  4.  Klõpsake menüüs API **CORS** .
  5.  Sisestage väljale, vajutades sisestusklahvi pärast igat mis tahes lubatud päritolu.
  6.  Kui teie loend lubatud päritolu on õige, klõpsake nuppu Salvesta.

> [AZURE.TIP]Üks teenuse Azure rakenduse kasutamise eelised on, et käivitada oma veebisaidi ja mobiilsideteenuse samas kohas.  Lisateavet leiate jaotisest [järgmised toimingud](#next-steps) .

### <a name="download-publish-profile"></a>Laadige alla uue profiili avaldamine.

Veebisaidi avaldamise profiili muudetakse kui migreerimine Azure'i rakendust Service.  Kui kavatsete avaldada oma saidi Visual Studios, peate avaldamise uus profiil.  Uue profiili avaldamise allalaadimiseks tehke järgmist.

  1.  [Azure'i portaali]sisse logida.
  2.  Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3.  Klõpsake nuppu **Hangi avaldada profiili**.

PublishSettings fail on teie arvutisse alla laadida.  Seda nimetatakse tavaliselt _sitename_. PublishSettings.  Saate importida oma projekti avalda sätted.

  1.  Visual Studio ja Azure mobiilsideteenuse projekti avamine
  2.  Paremklõpsake **Solution Exploreris** projekti ja valige käsk **Avalda...**
  3.  Klõpsake nuppu **impordi**
  4.  Klõpsake nuppu **Sirvi** ja valige oma allalaaditud avalda sätete fail.  Klõpsake nuppu **OK**
  5.  Klõpsake nuppu Avalda sätted töö tagamiseks **Kinnitage ühendus** .
  6.  Klõpsake nuppu **Avalda** avaldada oma saidile.


## <a name="working-with-your-site"></a>Oma saidi migreerimisjärgsed töötamine

Teie uus rakendus teenuse [Azure portaali] migreerimisjärgsed töötamise alustamine  Järgnevalt on teatud kindlate toimingute kohta, mida kasutasite [Azure klassikaline portaali]koos oma rakenduse teenuse kestvus teha märkmeid.

### <a name="publishing-your-site"></a>Allalaadimist ja migreeritud avaldamissait

Oma saidi kaudu git või ftp ja saate uuesti avaldada, millel on erinevad erinevate, sh WebDeploy, TFS, Mercurial, GitHub ja FTP.  Juurutamise identimisteabe migreeritakse koos ülejäänud saidile.  Kui kas seate mandaadi juurutamise või te ei mäleta neid, saate need lähtestada:

  1. [Azure'i portaali]sisse logida.
  2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3. Sätete tera avaneb vaikimisi.
  4. Klõpsake nuppu AVALDA **juurutamise identimisteabe** menüü.
  5. Sisestage uue juurutamise mandaati ruute ja seejärel klõpsake nuppu Salvesta.

Saate neid mandaate klooni git saidi või GitHub, TFS või Mercurial automatiseeritud juurutuste häälestamine.  Lisateavet leiate [Azure'i rakendust Service juurutamise dokumentatsiooni].

### <a name="appsettings"></a>Rakenduse sätted

Enamik migreeritud mobiilsideteenuse sätted on saadaval rakenduse sätete kaudu.  Saate rakenduse sätete loendi [Azure portaali].
Saate vaadata või rakenduse sätete muutmiseks tehke järgmist.

  1. [Azure'i portaali]sisse logida.
  2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3. Sätete tera avaneb vaikimisi.
  4. Klõpsake menüüs üldine **rakenduse sätted** .
  5. Liikuge kerides jaotisse rakenduse sätete ja otsige oma rakenduse säte.
  6. Klõpsake rakenduse sätte redigeerimiseks väärtus väärtus.  Klõpsake nuppu **Salvesta** väärtus salvestamiseks.

Saate värskendada korraga mitu rakenduse sätted.

> [AZURE.TIP]On kaks rakenduse sätted sama väärtusega.  Näiteks võite näha _ApplicationKey_ ja _MS\_ApplicationKey_.  Värskendada korraga nii rakenduse sätted.

### <a name="authentication"></a>Autentimine

Kõik autentimissätted on saadaval rakenduse sätete migreeritud saidil.  Värskendada oma autentimissätted, peate muutma õige rakenduse sätted.  Järgmine tabel sisaldab õige rakenduse sätete pakkuja autentimist.

| Pakkuja          | Kliendi ID                 | Kliendi salajane                | Muud sätted             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsofti konto | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebooki          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitteri           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Märkus: **MS\_AadTenants** on salvestatud komaga eraldatud loend rentniku domains ("Lubatud rentnikud" välja Mobile Services portaal).

> [AZURE.WARNING] **Ärge kasutage autentimist menetlustele menüüs sätted**
>
> Azure'i rakenduse teenus pakub eraldi "ei nõua koodi" autentimise ja luba süsteemi jaotises selle _autentimine / autoriseerimine_ menüü sätted ja _Mobile autentimine_ (aegunud) suvand jaotises menüü sätted.  Need suvandid ei ühildu migreeritud Azure mobiilsideteenuse.  Saate [uuendada oma saidi](app-service-mobile-net-upgrading-from-mobile-services.md) ära Azure'i rakendust Service autentimist.

### <a name="easytables"></a>Andmete

Menüü _andmed_ Mobile teenused on asendatud _Lihtne tabelite_ Azure portaali.  Lihtne tabelite juurdepääsemiseks tehke järgmist.

  1. [Azure'i portaali]sisse logida.
  2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3. Sätete tera avaneb vaikimisi.
  4. Klõpsake mobiilse menüüs **lihtne tabelid** .

Saate avada olemasoleva tabelite, kui klõpsate tabeli nime või tabeli lisamine, klõpsates nuppu **Lisa** .  On erinevaid toiminguid, mida saate teha selle tera, sh:

* Tabeli õiguste muutmine
* Funktsionaalseid skriptide redigeerimine
* Tabeli skeemi haldamine
* Tabeli kustutamine
* Tabeli sisu tühjendamine
* Kindlate ridade tabeli kustutamine

### <a name="easyapis"></a>API-GA

_Lihtne API -d_ on asendatud Mobile teenused vahekaart _API_ Azure portaali.  Lihtne API-de juurdepääsemiseks tehke järgmist.

  1. [Azure'i portaali]sisse logida.
  2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3. Sätete tera avaneb vaikimisi.
  4. Klõpsake mobiilne menüü **Lihtne API -d** .

Teie migreeritud API-d on juba loetletud tera.  Saate lisada ka selle tera API.  Teatud API haldamiseks valige API.
Uue keelest, saate reguleerida õiguste ja redigeerida skriptide API.

### <a name="on-demand-jobs"></a>Ajasti tööde haldamine

Kõik ajasti tööd on saadaval jaotises saidikogumid ajasti töö kaudu.  Oma töö scheduler juurdepääsemiseks tehke järgmist.

  1. [Azure'i portaali]sisse logida.
  2. Valige **sirvimine >**, sisestage väljale _Filter_ **ajakava** ja seejärel valige **Scheduler saidikogumid**.
  3. Valige töö saidikogumi saidile.  See on nime _sitename_-tööde haldamine.
  4. Klõpsake nuppu **sätted**.
  5. Klõpsake nuppu **Scheduler tööde haldamine** jaotises haldamine.

Ajastatud tööd on loetletud migreerimise enne määratud sagedusega.  Nõudmisel tööde haldamine on keelatud.  Kuvatakse nõudmisel töö käivitamiseks tehke järgmist.

  1. Valige töö, mida soovite käivitada.
  2. Vajaduse korral klõpsake nuppu **Luba** töö lubamiseks.
  3. Klõpsake nuppu **sätted**ja seejärel **ajakava**.
  4. Valige kordumise **üks kord**ja seejärel klõpsake nuppu **Salvesta**

Teie tellitavad tööd asuvad `App_Data/config/scripts/scheduler post-migration`.  Soovitame teisendada kõik tellitavad tööd [WebJobs] või [funktsioonid].  Kirjutage ajasti töökohta [WebJobs] või [funktsioonid].

### <a name="notification-hubs"></a>Teatis jaoturi

Mobile Services kasutab teatis jaoturi tõuketeatisi.  Teate jaoturi linkida teie mobiiliteenuse pärast migreerimist kasutatakse rakenduse järgmised sätted:

| Rakenduse säte                    | Kirjeldus                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Teatis jaoturi Namespace           |
| **MS\_NotificationHubName**             | Teatis jaoturi nimi                |
| **MS\_NotificationHubConnectionString** | Teatis jaoturi ühendusstring   |
| **MS\_NamespaceName**                   | Alias MS_PushEntityNamespace      |

Teie teate jaoturi hallatakse [Azure portaali]kaudu.  Pöörake tähelepanu teate jaoturi nimi (leiate selle rakenduse sätete abil).

  1. [Azure'i portaali]sisse logida.
  2. Valige **Sirvi**>, valige **Teatise jaoturi**
  3. Klõpsake teate jaoturi nime seostatud mobiilsideteenuse ülevaade.

> [AZURE.NOTE]Kui teie teate jaoturi "Segatud" tüüp, ei kuvata.  "Kombineeritud" tippige teate jaoturi kasutada nii teate jaoturi ja pärand teenuse siini funktsioonid.  Enne jätkamist [oma kombineeritud nimeruumid teisendada] .  Kui teisendamine on lõpule jõudnud, kuvatakse teie teate jaoturi [Azure portaali].

Lisateabe saamiseks vaadake [Teatis jaoturi] dokumentatsiooni kohta.

> [AZURE.TIP]Teatis jaoturi haldusfunktsioonid [Azure portaali] on endiselt eelvaade.  [Azure'i klassikaline portaali] jääb teie teate jaoturi haldamise.

### <a name="legacy-push"></a>Pärand tõuketeatised sätted

Kui olete konfigureerinud tõuketeatised sisse oma mobiilsideteenuse enne teatise jaoturi kasutuselevõttu, kasutate _pärand tõuketeatised_.  Kui kasutate tõuketeatised ja teatis jaoturi konfiguratsioonist loendis ei kuvata, siis tõenäoliselt kasutate _pärand tõuketeatised_.  See funktsioon on migreeritud kõiki muid funktsioone.  Soovitame siiski versioonile teatis jaoturi varsti pärast migreerimise lõpuleviimist.

Vahepeal pärand tõuketeatised sätted (välja arvatud märkimisväärne APNs-i sert) on saadaval jaotises rakenduse sätted.  Värskendada, asendades vastavat faili failisüsteemi APNs-i serdi.

### <a name="app-settings"></a>Muud rakenduse sätted

Järgmised täiendavad rakenduse sätted on migreeritud Mobile teenusepakkuja ja klõpsake jaotises *sätted*saadaval > *Rakenduse sätted*:

| Rakenduse säte              | Kirjeldus                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Rakenduse nimi                    |
| **MS\_MobileServiceDomainSuffix** | Domeeni eesliide. st Azure'i-mobile.net |
| **MS\_ApplicationKey**            | Rakenduse tootevõti                    |
| **MS\_peavõtmega**                 | Rakenduse juhtslaidi tootevõti                     |

Kiirmenüü klahvi ja klahvi juhtslaidi on identne oma algse mobiilsideteenuse rakenduse võtmed.  Eelkõige kiirmenüü klahvi saadetakse mobiilikliendid valideerimiseks mobiilsideseadmete API kasutamisel.

### <a name="cliequivalents"></a>Käsurea võrdlus

_Azure'i mobiilsideseadmete_ käsu abil saate enam Azure Mobile teenused saidi haldamiseks.  Selle asemel paljud funktsioonid on asendatud _Azure'i saidi_ käsk.  Levinud käskude ekvivalendid otsimiseks kasutada järgmises tabelis:

| _Azure'i Mobile_ Käsk                     | Võrdväärne _Azure'i saidi_ käsk            |
| :----------------------------------------- | :----------------------------------------- |
| mobiilse asukohad                           | saidi loendis asukoht                         |
| mobiilsideseadmete loendi                                | saidi loendi                                  |
| mobiilse Kuva _nimi_                         | saidi Kuva _nimi_                           |
| mobiilse uuesti _nimi_                      | saidi uuesti _nimi_                        |
| mobiilse ümberkorraldamine _nimi_                     | saidi juurutamise ümberkorraldamine _commitId_ _nimi_ |
| mobiilse klahvi Määrake _nimi_ _Tüüp_ _väärtus_       | saidi appsetting Kustuta _klahvi_ _nimi_ <br/> saidi appsetting lisada _võti_=_väärtuse_ _nimi_ |
| mobiilse config loendi _nimi_                  | saidi appsetting loendi _nimi_                |
| mobiilse config saada _nimi_ _võti_             | saidi appsetting kuvamiseks _klahvi_ _nimi_          |
| mobiilse config seatud _nimi_ _võti_             | saidi appsetting Kustuta _klahvi_ _nimi_ <br/> saidi appsetting lisada _võti_=_väärtuse_ _nimi_ |
| mobiilse domeeni _nimi_                  | saidi domeeni _nimi_                    |
| mobiilse domeeni _nimi_ _domeeni_ lisamine          | saidi domeeni lisada _domeeni_ _nimi_            |
| Kustuta mobiilsideseadmete domeeni _nimi_                | saidi domeeni kustutamine _domeeni_ _nimi_         |
| mobiilse skaala Kuva _nimi_                   | saidi Kuva _nimi_                           |
| mobiilse skaala _nime_ muutmine                 | saidi skaala režiimi _režiimi_ _nimi_ <br /> saidi skaala eksemplarid _eksemplaride_ _nimi_ |
| mobiilse appsetting loendi _nimi_              | saidi appsetting loendi _nimi_                |
| mobiilse appsetting lisada _nimi_ _võti_ _väärtus_ | saidi appsetting lisada _võti_=_väärtuse_ _nimi_   |
| mobiilse appsetting kustutage _nimi_ _võti_      | saidi appsetting Kustuta _klahvi_ _nimi_        |
| mobiilse appsetting Kuva _nimi_ _võti_        | saidi appsetting Kustuta _klahvi_ _nimi_        |

Värskendage autentimist või push Teatiste sätete värskendamine rakenduse sobiv säte.
Failide redigeerimine ja avaldamine saidi ftp või git.

### <a name="diagnostics"></a>Diagnostika- ja logimine

Diagnostikalogimise keelatakse tavaliselt teenuse Azure rakendus.  Diagnostikalogimise lubamiseks tehke järgmist.

  1. [Azure'i portaali]sisse logida.
  2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3. Sätete tera avaneb vaikimisi.
  4. Valige menüüs funktsioonide **Diagnostikalogid** .
  5. **Klõpsake** nuppu Järgmine logide: **Rakenduse logimine (failisüsteemi)**, **üksikasjaliku tõrketeated**ja **nurjunud taotluste jälitamine**
  6. Klõpsake nuppu **Failisüsteemi** veebiserver logimine
  7. Klõpsake nuppu **Salvesta**

Logide vaatamiseks tehke järgmist.

  1. [Azure'i portaali]sisse logida.
  2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu migreeritud mobiilsideteenuse oma nime.
  3. Klõpsake nuppu **Tööriistad**
  4. Valige menüüs järgima **Log voo** .

Nagu need on loodud, kuvatakse aknas logid.  Samuti saate alla laadida logid hilisemaks analüüsiks juurutamise mandaadi abil. Lisateabe saamiseks vaadake [logimise] dokumentatsioonist.

## <a name="known-issues"></a>Teadaolevad probleemid

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Kustutamise viiakse Mobile'i rakendus klooni põhjustab saidi katkestuste

Kui klooni migreeritud mobiiltelefoni teenust Azure PowerShelli kaudu, siis kustutage klooni, eemaldatakse tootmise teenust DNS-i kirje.  Teie saidil on ei saa enam Interneti kaudu juurdepääsetavad.  

Lahendus: Kui soovite oma saidi klooni, teha portaali kaudu.

### <a name="changing-webconfig-does-not-work"></a>Muutmise Web.config ei tööta

Kui teil on mõni ASP.net-i saidi, muutub selle `Web.config` fail pole rakendatud saada.  Azure'i rakendust Service koostab sobiva `Web.config` faili käivitamisel toetamiseks Mobile teenuste runtime.  Teisenduse XML-faili abil saate alistada teatud sätted (nt kohandatud päised).  Faili loomine sisse nimetatakse `applicationHost.xdt` – seda faili peab lõpuks soovitud `D:\home\site` kataloogi Azure'i teenus.  Üleslaadimine on `applicationHost.xdt` abil otse Kudu või arhiivida kohandatud juurutamise skripti kaudu.  Järgnevalt kuvatakse dokument on näide.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Lisateavet leiate github [XDT muuta näidised] dokumentatsioonist.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Migreeritud Mobile teenuste ei saa lisada liikluse haldur

Kui loote liikluse haldur profiili, te ei saa valida otse migreeritud mobiiliteenuse profiilile.  Kasutage "väliste lõpp."  Väliste lõpp-punkti saab lisada ainult PowerShelli kaudu.  Lisateavet leiate teemast [liikluse haldur õpetuse](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui rakenduse migreeritakse rakenduse teenus, on veel rohkem võimalusi, mille abil saate:

  * Juurutamise [lavastus slots] võimaldavad etapp muudatusi saidile ja teha A ja B testimine.
  * [WebJobs] pakuvad nõudmisel ajastatud töö Asenda.
  * Saate [pidevalt juurutada] saidil saidi GitHub, TFS või Mercurial linkimise teel.
  * [Rakenduse ülevaated] abil saate jälgida oma saidile.
  * Väli Serve veebisait ja Mobile API sama koodi.

### <a name="upgrading-your-site"></a>Azure'i Mobile'i rakendused SDK Mobile Services saidi versiooniks

  * Node.js põhise serveri projektide, uue [Mobile'i rakendused Node.js SDK] pakub mitmesuguseid uusi funktsioone. Näiteks saate nüüd teha kohaliku arengu ja silumine, kasutada mis tahes Node.js versiooni kohal 0,10, ja mis tahes Express.js vahevara kohandada.

  * Jaoks. Projektide NET-põhine server uue [Mobile'i rakendused SDK NuGet-paketid](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) on rohkem paindlikkust Nugeti eelnevuste.  Need paketid toetavad uus rakendus teenuse autentimise ja mis tahes ASP.net-i projekti koostamine. Üleminekut kohta lisateabe saamiseks lugege teemat [uuendada oma olemasoleva .NET Mobile teenuse rakenduse teenus](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Rakenduse teenuse hinnad]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Rakenduse ülevaated]: ../application-insights/app-insights-overview.md
[Mastaabi automaatselt]: ../app-service-web/web-sites-scale.md
[Azure'i rakendust Service]: ../app-service/app-service-value-prop-what-is.md
[Azure'i rakendust Service juurutamise dokumentatsioon]: ../app-service-web/web-sites-deploy.md
[Azure'i klassikaline portaal]: https://manage.windowsazure.com
[Azure'i portaal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure'i Scheduler lepingud]: ../scheduler/scheduler-plans-billing.md
[pidevalt juurutamine]: ../app-service-web/app-service-continuous-deployment.md
[Oma kombineeritud nimeruumid teisendamine]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[kohandatud domeeni nimed]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[Azure'i rakenduse üldine kättesaadavus]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hübriidjuurutuse ühendused]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Logimine]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobiilirakenduste Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Teenuste vs rakenduse mobiilsideteenus]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Teatis jaoturi]: ../notification-hubs/notification-hubs-push-notification-overview.md
[jõudluse jälgimine]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Teie mobiiliteenuse varundamine]: ../mobile-services/mobile-services-disaster-recovery.md
[lavastus slots]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT transformatsioon näidised]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funktsioonid]: ../azure-functions/functions-overview.md
