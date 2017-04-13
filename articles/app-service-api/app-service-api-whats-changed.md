<properties
    pageTitle="Rakenduse teenuse API rakendused - mis on muutunud | Microsoft Azure'i"
    description="Siit saate teada, mis on uut teenuses Azure rakenduse API rakendused."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Rakenduse teenuse API rakendused - mis on muutunud

November 2015 üritusel ühendage () täiustused Azure'i rakendust Service olid [teada](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Need parandused kaasata aluseks muudatusi API paremaks joondamiseks Mobile ja veebirakenduste, vähendada mõistet arv ja juurutamise ja käitusaja jõudluse parandamiseks rakendused. Alates 30 November 2015 uue API rakenduste loomist Azure haldusportaali abil või Viimane instrumentaarium kajastuvad need muudatused. Selles artiklis kirjeldatakse need muudatused ning kuidas ümberkorraldamine olemasolevate rakenduste võimaluste ärakasutamiseks.

## <a name="feature-changes"></a>Muudatused funktsioonide
Põhifunktsioonide API rakenduste – autentimine, CORS ja API metaandmete – liikunud otse rakenduse teenus. Selle muudatuse koos funktsioonid on saadaval üle Web, Mobile ja API rakendused. Tegelikult kõigi kolme jagada sama **Microsoft.Web/sites** ressursi tüüp ressursihaldur sisse. API rakendused lüüsi enam ei vaja või ei paku API rakendused. See ka on hõlpsam kasutada Azure'i API haldus, kuna seal on ainult üks API Management gateway.

![API rakenduste ülevaade](./media/app-service-api-whats-changed/api-apps-overview.png)

Olulisemad põhimõtet värskenduse API rakendused on selleks, et saaksite tuua oma API on teie valitud keeles.  Kui teie API on juba juurutanud Web Appi või mobiilirakenduse, pole ümberkorraldamine rakenduse ära uusi funktsioone. Kui kasutate praegu API Apps preview, migreerimise juhised on toodud allpool.

### <a name="authentication"></a>Autentimine
Olemasoleva kasutusvalmis API rakendused, teenuste/mobiilirakenduste ja veebirakenduste autentimise funktsioonid on on ühendatud ja on saadaval ühe Azure'i rakendust Service autentimise tera haldusportaal. Sissejuhatus rakenduse teenuses autentimisteenuste, leiate teemast [laiendamine rakenduse autentimise / luba](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

API stsenaariumi puhul, on oluline uute võimaluste arv.

- **Azure Active Directory otse abil**, ilma vajaduseta AAD sõnet seansi luba Exchange'i kliendi kood: teie klient lihtsalt saate kaasata AAD sõned päises autoriseerimine vastavalt esitaja Turbeloa määratlus. See tähendab, pole teenuse konkreetse rakenduse SDK on vaja kliendi või serveri poolel. 
- **Teenusest või "Internal" juurdepääs**: kui teil on filtrideemoni protsessi või mõnda muud klienti juurdepääsu API-de ilma liidest, taotleda märgiks abil AAD teenuse peamise ja andke seda rakenduse teenusega autentimise oma rakendusega.
- **Edasilükatud autoriseerimine**: paljudes rakendustes on erineva juurdepääsupiirangud erinevate osade rakenduse jaoks. Soovite ehk teatud API-de olema avalikult kättesaadav, aga vajavad Logi sisse. Algse autentimine/autoriseerimine funktsiooni oli konsulaatides, terve saidi nõudva Logi sisse. See suvand on endiselt olemas, kuid võite võimaldab rakenduse koodis renderdamiseks Accessi otsuseid pärast rakenduse teenus on autenditud kasutaja.
 
Autentimise uute funktsioonide kohta leiate lisateavet teemast [autentimise ja luba API rakendused teenuses Azure rakendus](app-service-api-authentication.md). Lisateavet selle kohta, kuidas saate migreerida teemast olemasoleva API rakenduste eelmise API rakenduste mudeli uue, selle artikli [Migrating olemasoleva API rakendused](#migrating-existing-api-apps) .
 
### <a name="cors"></a>CORS
Komaga eraldatud **MS_CrossDomainOrigins** rakenduse säte, asemel on nüüd tera Azure'i haldusportaal CORS konfigureerida. Teise võimalusena saate konfigureerida, ressursihaldur instrumentaarium, nt Azure PowerShelli, CLI või [Ressursside Exploreri](https://resources.azure.com/)kaudu. Atribuudi **cors** **Microsoft.Web/sites/config** ressursi tippige jaoks oma ** &lt;saidi nimi&gt;/web** ressursi. Näiteks:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API metaandmed
API määratlus tera on nüüd saadaval kogu Web, Mobile ja API rakendused. Haldusportaal, saate määrata, kas suhteline URL-i või osutab lõpp selle hosts ärplema 2.0 kujutis oma API absoluutne url. Teise võimalusena saate konfigureerida, kasutades ressursihaldur instrumentaarium. Atribuudi **apiDefinition** **Microsoft.Web/sites/config** ressursi tippige jaoks oma ** &lt;saidi nimi&gt;/web** ressursi. Näiteks:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Sel ajal, peab olema avalikult juurdepääsetava palju järgneval kliente (nt Visual Studio REST API kliendi genereerimine ja PowerApps "Lisa API" meilivoo) see peab olema autentimata metaandmete lõpp-punkti. See tähendab, kui kasutate rakendust Service autentimist ja soovite seada API määratluse kaudu oma rakenduses, ise, peate suvandi edasilükatud autentimise ülalkirjeldatud nii, et teie ärplema metaandmete tee on avalik.

## <a name="management-portal"></a>Haldusportaal
Valige **uus > Web + Mobile > API rakenduse** portaali loomine API rakendused, millel on kirjeldatud artiklis uute võimaluste kajastamiseks. **Sirvi > API rakenduste** kuvatakse ainult need uue API rakendused. Kui saate sirvida API rakendusse, tera jagab sama paigutus ja funktsioone nagu Web ja mobiilirakenduste kohta. Ainult erinevused on Kiirjuhend sisu ja tellimine sätted.

Olemasoleva API rakendused (või loodud loogika rakenduste turuplats API rakendustele) eelmise eelvaatega võimaluste on jätkuvalt nähtav Designeris loogika rakendused ja kõik ressursside ressursirühma sirvimisel.

## <a name="visual-studio"></a>Visual Studio

Enamik veebirakenduste instrumentaarium töötab kuna need ühiskasutusse andmine on sama aluseks oleva **Microsoft.Web/sites** ressursi tüüpi uue API rakendustega. Azure'i Visual Studio instrumentaarium, tuleks siiski uuele versioonile üle viidud versioonile 2.8.1 või uuem versioon kuna see pakub mitmesuguseid võimalusi teatud API-d. Laadige SDK [Azure allalaadimiste lehe](https://azure.microsoft.com/downloads/)kaudu.

Täiustamise tüüpi teenuse rakendus, mille avaldamine on ka ühendatud jaotises **Avalda > Microsoft Azure'i rakendust Service**:

![API rakenduste avaldamine](./media/app-service-api-whats-changed/api-apps-publish.png)

Lugege lisateavet SDK 2.8.1 teadaanne [ajaveebipostituse](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Teise võimalusena saate käsitsi importida avalda profiili haldusportaali lubamiseks avalda. Siiski Cloud Explorer, koodi loomine ja API rakenduse valiku/loomine nõuab SDK 2.8.1 või uuem versioon.

## <a name="migrating-existing-api-apps"></a>Migreerimine olemasoleva API rakendused
Kui teie kohandatud API on juurutatud eelvaade eelmise versiooni API rakendused, palume migreerimiseks uue mudeli API rakenduste 31 Detsember 2015. Kuna nii vana kui uus mudel põhinevad Web API-de majutatud rakenduse teenus, kood enamik saab uuesti kasutada.

### <a name="hosting-and-redeployment"></a>Majutamist ja positsioonide
Juhised ümbersuunamine on sama, mis tahes olemasolevaid Veebiteenuste juurutamine rakenduse teenusega. Juhised:

1. Saate luua tühja API rakendus. Seda saab teha portaalis uus > API rakenduse Visual Studio avalda või kaudu ressursihaldur instrumentaarium. Kui instrumentaarium ressursihaldur või mallide abil, määrake väärtuseks **liiki** **api** suunatud API stsenaariumid haldusportaal quickstarts ja sätted on tippige **Microsoft.Web/sites** ressursi.
2. Ühenduse loomine ja juurutamine projekti tühja API rakenduse mis tahes juurutamise menetlustele, rakenduse teenus ei toeta. Lugege lisateavet [Azure'i rakendust Service juurutamise dokumentatsiooni](../app-service-web/web-sites-deploy.md) . 
  
### <a name="authentication"></a>Autentimine
Rakenduse teenuse autentimisteenuste tugi sama võimaluste eelmise mudeliga API rakendused on saadaval. Kui kasutate seansi märgid ja nõuavad SDK-d, kasutage selle järgmised kliendi ja serveri SDK-d.

- Kliendi: [Azure'i Mobiilikliendi SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Server: [Microsoft Azure mobiilirakenduse .net-i autentimist laiend](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Kui kasutasite hoopis rakenduse teenuse alfa SDK-d, need on nüüd aegunud:

- Kliendi: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Azure Active Directory, aga pole teenuse konkreetse rakenduse on nõutav kui kasutate AAD luba otse.

### <a name="internal-access"></a>Sisemine juurdepääsu
Eelmise API rakenduste mudel sisaldab sisseehitatud sisemine juurdepääsutase. See on vajalik kasutamine SDK allkirjastamiseks taotlused. Nagu eespool mudeliga uue API rakenduste AAD teenuse põhisumma saab asendaja teenusest autentimine rakenduse teenuse-spetsiifiliste SDK nõudmata. Lugege rohkem teemast [teenuse põhisumma autentimise API rakendused teenuses Azure rakendus](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Tuvastamine
Eelmise API rakenduste mudeli oli API-de otsimise muud API rakendused käitusajal ressursside samasse taha samas lüüsis. See on eriti kasulik arhitektuurides, et microservice mustrite. Kui see ei toeta otseselt, mitmeid määratavaid suvandeid on saadaval.

1. Kasutage Azure'i ressursihaldur API Discovery.
2. Pange Azure'i API halduse rakenduse teenuse majutatud API-de ette. Azure'i API halduse toimib fassaadi ja pakuvad ühed välise suunatud URL-i isegi juhul, kui sisemise topoloogia muutub.
3. Luua oma on discovery API rakenduse ja on muud API rakendused on discovery rakenduse käivitamisel registreerida.
4. Juurutamise ajal asustada lõpp-punktid API muude rakendustega rakenduse sätete kõik API rakendused (ja kliendid). See on otstarbekas malli kasutuselevõttu ja API rakendused on nüüd teile juhtelemendi URL-i.

## <a name="using-api-apps-with-logic-apps"></a>Loogika rakendustega API rakenduste kasutamine

Uus API rakenduste mudel toimib hästi [Loogika rakenduste skeemi versioon 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks lugege [API rakenduste dokumentatsiooni jaotise](https://azure.microsoft.com/documentation/services/app-service/api/)artikleid. Need on värskendatud kajastamiseks uue mudeli API rakendused. Lisaks jõuda Foorumid täiendavad üksikasjad või migreerimise juhised:

- [MSDN-i Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Virnlintdiagrammil ületäitumine](http://stackoverflow.com/questions/tagged/azure-api-apps)
