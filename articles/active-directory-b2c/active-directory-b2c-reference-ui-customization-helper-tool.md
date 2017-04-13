<properties
    pageTitle="Azure Active Directory B2C: Lehe UI kohandamise abimees tööriista | Microsoft Azure'i"
    description="Kasutada, et näidata lehe UI kohandamise funktsiooni Azure Active Directory B2C tööriista helper."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Helper tööriista kasutada, et näidata lehe kasutaja kasutajaliidese (UI) kohandamise funktsiooni

See artikkel kehtib on companion [peamise Kasutajaliidese kohandamine artiklis](active-directory-b2c-reference-ui-customization.md) B2C Azure Active Directory (Azure AD). Järgnevalt kirjeldatakse, kuidas kasutada funktsiooni lehe UI kohandamine, kasutades valimi HTML-i ja CSS-i sisu, mis oleme lisanud.

## <a name="get-an-azure-ad-b2c-tenant"></a>Azure'i AD B2C rentnikku hankimine

Enne kui saate kohandada midagi, peate [mõne Azure AD B2C rentniku](active-directory-b2c-get-started.md) saada kui teil pole veel üks.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Registreerumise või sisselogimise poliitika loomine

Oleme lisanud valimi sisu saab kasutada customze kahe lehtede [registreerumise või sisselogimise poliitika](active-directory-b2c-reference-policies.md): [ühendatud lehe](active-directory-b2c-reference-ui-customization.md) ja [omas kinnitanud lehe atribuudid](active-directory-b2c-reference-ui-customization.md). Kui [teie registreerumise või sisselogimise poliitika loomise](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), kohaliku konto (meiliaadress), Facebooki, Google ja Microsoft kuuluva **identiteedipakkujad**. Need on ainult sisepagulaste reintegreerimine, mida aktsepteerib meie valimi HTML-sisu.  Kui soovite, saate lisada ka need riigi sees ümberasustatud isikute alamhulk.

## <a name="register-an-application"></a>Rakenduse registreerimine

[Rakenduse](active-directory-b2c-app-registration.md) registreerimiseks peate oma B2C rentniku, mida saab käivitada teie poliitika. Pärast registreerumist rakenduse, on teil mitu võimalust, mille abil saate käivitada tegelikult teie registreerumise poliitika.

- Koostada ühe kiire alustada rakenduste loendis jaotises "Get started" Azure AD B2C [logige üles ja logige sisse oma rakendused tarbijad](active-directory-b2c-overview.md#getting-started).
- Valmismallide [Azure AD B2C mänguväljak](https://aadb2cplayground.azurewebsites.net) rakendust kasutada. Kui otsustate kasutada mänguväljakut, peate registreerima rakenduse teie B2C rentniku abil **ümber suunata URI** `https://aadb2cplayground.azurewebsites.net/`.
- Kasutada nuppu **Rakenda kohe** teie poliitika [Azure portaali](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Teie poliitika kohandamine

Ilme ja teie poliitika kohandamiseks loomiseks vajaliku esmalt teatud põhimõtted Azure AD B2C HTML-i ja CSS-i failidega. Seejärel saate laadida oma staatiliseks sisuks avalikult kättesaadavaks asukohta, nii, et Azure AD B2C sellele juurde pääsevad. See võib olla oma spetsiaalne veebiserverisse, Azure'i bloobimälu, Azure'i sisu kohaletoimetamise võrgu või mis tahes muud staatiline ressursi majutusteenuse pakkuja. Ainult nõuded on, et sisu on saadaval https ja CORS abil avada. Kui olete avatud oma staatiliseks sisuks veebis, saate muuta oma poliitika, osutage käsule selle asukoha ja selle sisu esitada oma klientidele. [Peamise Kasutajaliidese kohandamine artiklis](active-directory-b2c-reference-ui-customization.md) on üksikasjalikult kirjeldatud, kuidas Azure AD B2C kohandamine funktsioon töötab.

Selleks et selles õpetuses, oleme juba loodud teatud valimi sisu ja majutatud Azure'i bloobimälu. Valimi sisu on väga lihtsa kohandamine kujunduse väljamõeldud firma "Wingtip mänguasjad". Proovige järele oma poliitikat, toimige järgmiselt.

1. Logige sisse oma rentniku [Azure portaali](https://portal.azure.com/) ja liikuge B2C funktsioonid tera.
2. Klõpsake **registreerumise või sisselogimise poliitika** ja seejärel nuppu teie poliitika (nt "b2c\_1\_Logi\_üles\_Logi\_sisse").
3. Klõpsake **lehe UI kohandamine** ja seejärel **lehe ühendatud registreerumise või Logi sisse**.
4. Tavalise **kasutada kohandatud lehe** aktiveerimine **Jah**. Sisestage väljale **kohandatud lehe URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klõpsake nuppu **OK**.
5. Klõpsake nuppu **kohalik konto loomise lehele**. Tavalise **kasutada kohandatud malli** aktiveerimine **Jah**. Sisestage väljale **kohandatud lehe URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Korrake sama **suhtlusvõrgu konto loomise lehele**.
 Sulgemiseks klõpsake nuppu **OK** kaks korda UI kohandamine labad.
6. Klõpsake nuppu **Salvesta**.

Nüüd saate proovida oma kohandatud poliitika. Saate oma rakenduse või Azure AD B2C mänguväljak, kui soovite, kuid võite klõpsata ka lihtsalt poliitika tera käsk **Rakenda kohe** . Valige rakenduse drop-down väljal ja valige sobiv redirect URI. Klõpsake nuppu **Käivita kohe** . Avatakse brauseris uus vahekaart ja käivitada rakenduse uue sisu kohas sisselogimisega kasutuskogemuse kaudu!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Azure'i bloobimälu valimi sisu üleslaadimine

Kui soovite kasutada Azure'i bloobimälu majutada oma lehe sisu, saate luua oma konto salvestusruumi ja laadige failid üles meie B2C helper tööriista abil.

### <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Valige **+ Uus** > **andmete + salvestusruumi** > **salvestusruumi konto**. Peate Azure tellimuse Azure'i bloobimälu konto loomiseks. Saate registreeruda tasuta prooviversiooni [Azure veebisaidi](https://azure.microsoft.com/pricing/free-trial/).
3. Sisestage **nimi** salvestusruumi konto (nt "contoso") ja valige vastavad Valikud **hinnakirjad taseme**, **ressursirühm** ja **tellimus**. Veenduge, et teil on märgitud suvand **Kinnita Startboard** . Klõpsake nuppu **Loo**.
4. Minge tagasi soovitud Startboard ja klõpsake äsja loodud salvestusruumi kontot.
5. Jaotises **Kokkuvõte** klõpsake **ümbriste**ja seejärel klõpsake nuppu **+ Lisa**.
6. Valige **bloobimälu** **juurdepääsutüüp** **nime** ette ümbrises (nt "b2c"). Klõpsake nuppu **OK**.
7. Ümbris, mille lõite kuvatakse loendis **plekid** enne. Kirjutage URL-i ümbris; näiteks peaks sarnanema järgmisega `https://contoso.blob.core.windows.net/b2c`. Sulgege **plekid** tera.
8. Salvestusruumi enne konto, klõpsake **võtmed** ja kirjutage **Salvestusruumikonto nimi** ja **Accessi primaarvõtme** väljade väärtused.

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Valige **+ Uus** > **andmete + salvestusruumi** > **salvestusruumi konto**. Peate Azure tellimuse Azure'i bloobimälu konto loomiseks. Saate registreeruda tasuta prooviversiooni [Azure veebisaidi](https://azure.microsoft.com/pricing/free-trial/).
3. Valige jaotises **Konto tüüpi** **Bloobimälu** ja jätke muude väärtuste vaikeväärtuseks.  Soovi korral saate redigeerida ressursirühm ja asukoht.  Klõpsake nuppu **Loo**.
4. Minge tagasi soovitud Startboard ja klõpsake äsja loodud salvestusruumi kontot.
5. Klõpsake jaotises **Kokkuvõte** **+ ümbrises**.
6. Valige **bloobimälu** **juurdepääsutüüp** **nime** ette ümbris (nt "b2c"). Klõpsake nuppu **OK**.
7. Avage container **Atribuudid**ja kirjutage URL-i ümbris; näiteks peaks sarnanema järgmisega `https://contoso.blob.core.windows.net/b2c`. Sulgege container tera.
8. Salvestusruumi enne konto, klõpsake **Klahvi ikoon** ja kirjutage **Salvestusruumikonto nimi** ja **Accessi primaarvõtme** väljad.

> [AZURE.NOTE]
    **Accessi primaarvõtme** on mõni oluline turvalisuse mandaati.

### <a name="download-the-helper-tool-and-sample-files"></a>Helper tööriista ja valimi failide allalaadimine

[Azure'i bloobimälu helper tööriista ja valimi failid ZIP-faili](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) alla laadida või klooni seda GitHub kaudu saate.

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

See hoidla sisaldab mõnda `sample_templates\wingtip` kaust, mis sisaldab näide HTML-i, CSS-i ja pilte. Järgmised Mallid viitamiseks Azure'i bloobimälu oma konto, peate HTML-i failide redigeerimiseks. Avatud `unified.html` ja `selfasserted.html` ja asendada esinemisvõimaluse `https://localhost` oma ümbrises, et eelmiste juhiste järgi üleskirjutatud URL. Te peate kasutama absoluutne tee HTML-faile, sest sel juhul HTML-i pakutakse Azure AD, klõpsake jaotises domeeni `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Valimi failide üleslaadimine

Sama hoidlas, pakkige lahti `B2CAzureStorageClient.zip` ja käivitage selle `B2CAzureStorageClient.exe` jooksul. See programm lihtsalt üles kõik teie salvestusruumi kontole määratud kataloogis failid ja need failid CORS juurdepääsu lubamiseks. Kui järgisite ülal esitatud juhiseid, HTML-i ja CSS-i failid kohe osutamine salvestusruumi kontole. Pange tähele, et teie salvestusruumi konto nimi eelneb osade `blob.core.windows.net`; näiteks `contoso`. Saate kontrollida, et sisu on üles laaditud õigesti püüdes juurdepääsu `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` klõpsake brauseris. Veenduge, et sisu on nüüd lubatud CORS abil [http://test-cors.org/](http://test-cors.org/) . (Otsige "XHR olek: 200" tulemis.)

### <a name="customize-your-policy-again"></a>Teie poliitika uuesti kohandamine

Nüüd, kui üleslaadimist valimi sisu oma salvestusruumi konto, tuleb viitamiseks teie registreerumise poliitika redigeerida. Korrake toiminguid eeltoodud jaotises ["Kohanda teie poliitika"](#customize-your-policy) seekord oma salvestusruumi konto URL-ID abil. Näiteks asukoha oma `unified.html` faili oleks `<url-of-your-container>/wingtip/unified.html`.

Nüüd saate nuppu **Käivita nüüd** või oma rakenduste teie poliitika uuesti käivitada. Tulemus peaks välja nägema peaaegu täpselt sama – saate kasutada sama Kuulake HTML-i ja CSS-i mõlemal juhul. Siiski oma poliitika on nüüd viitamine Azure'i bloobimälu oma eksemplarist ja teil on õigus redigeerida ja laadige failid uuesti, nagu te palun. Kohandamise HTML-i ja CSS-i kohta lisateabe saamiseks vaadake [peamise Kasutajaliidese kohandamine artiklis](active-directory-b2c-reference-ui-customization.md).
