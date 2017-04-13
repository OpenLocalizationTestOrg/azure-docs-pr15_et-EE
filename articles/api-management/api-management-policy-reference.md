<properties 
    pageTitle="Azure'i API halduse poliitika viide" 
    description="Lisateavet saadaval API halduse konfigureerimine poliitika." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure'i API halduse poliitika viide

Selles jaotises antakse registri poliitikate [API halduse poliitika viide][]. Lisades ja konfigureerides poliitika kohta leiate teavet teemast [poliitikate API haldus][].

Poliitika avaldiste saate kasutada atribuudiväärtused ega tekstväärtusi mis tahes API teabehalduspoliitikate juhul, kui poliitika määrab teisiti. Mõned poliitika, näiteks [Control flow][] ja [määrata muutuja][] poliitikate põhinevad poliitika avaldised. Lisateabe saamiseks vt [Täpsemalt poliitika][] ja [poliitika avaldised][]

## <a name="policy-reference-index"></a>Poliitika viide index

-   [Juurdepääsu piiramise poliitika][]
    -   [Märkige ruut HTTP päise][] - ja/või HTTP päise väärtus jõustab.
    -   [Piiratud kõne määra tellimuse][] - API takistab kasutus naelu kõne määr, per tellimuse alusel piirata.
    -   [Piiratud kõne määra klahv](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - API takistab kasutus naelu kõne määr, per klahvi alusel piirata.
    -   [Piira helistaja IP][] - filtrid (lubab/keelab) kõnesid teatud IP-aadressid ja/või aadresside vahemikud.
    -   [Määratud kasutuse kvoodi märkimise teel][] – saate Jõusta ühekordse või neid elu jooksul kõne helitugevust ja/või läbilaskevõime kvoodi kohta tellimuse alusel.
    -   [Võtme kvoodi seadmine](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) – võimaldab Jõusta ühekordse või neid elu jooksul kõne helitugevust ja/või läbilaskevõime kvoodi kohta klahvi alusel.
    -   [Kinnitage JWT][] - jõustab olemasolu ja kehtivus JWT, mis on määratud HTTP päis või määratud Päringuparameetri ekstraktimist.
-   [Täiustatud poliitika][]
    -   [Control flow][] - kehtib tingimuslikult poliitika laused tulemuste hindamise kahendmuutujaga [avaldised][].
    -   [Kutse edasi][] - edastab taotluse taustväärtus teenusega.
    -   [Logi sisse sündmuse jaoturi][] – saadab sõnumid määratud vormingus sõnumi suunata määratletud [puuraidur](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) üksus.
    -   [Proovige](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - korduskatsed täitmise väidetest suletud poliitika, kui ja kuni tingimus on täidetud. Täitmise ajavahemiku tagant kordub ja proovige ülespoole määratud arvu.
    -   [Tagasi vastuse](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - peatab müügivõimaluste täitmise ja tagastab määratud vastuse otse helistaja.
    -   [Üks võimalus saatmistaotlus](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) – saadab taotluse määratud URL ilma vastuse ootamine.
    -   [Saatke kutse](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) – saadab taotluse määratud URL.
    -   [Seadmine meetodit](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - võimaldab teil muuta HTTP-päringu jaoks.
    -   [Oleku määramine](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - muutub HTTP olekukoodi määratud väärtus.
    -   [Määrake muutuja][] - jää püsima nimega [kontekstis][] muutuja hiljem juurdepääsu väärtuse.
    -   [Jälita](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - lisab stringi [API inspektor](../api-management/api-management-howto-api-inspector.md) väljundi sisse.
    -   [Oodake](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - ootab suletud saada taotlus, saada väärtust vahemälu või Control flow poliitika, tehke enne jätkamist.
-   [Poliitika autentimine][]
    -   [Autentimise põhiväljaandes][] - autentimise abil elementaarautentimine taustväärtus teenusega.
    -   [Kliendi serdiga autentimise][] - autentimise abil kliendi sertide taustväärtus teenusega.
-   [Vahemällu poliitika][] 
    -   [Vahemälu toomine][] - teha vahemälu otsida ja tagastada kehtiv vahemällu talletatud vastuse, kui need on saadaval.
    -   [Vahemälu poodi][] - vahemälu vastuse vastavalt määratud vahemälu juhtelemendi konfigureerimine.
    -   [Aja mõõtmine vahemälust](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - too vahemällu talletatud üksuse vajutamisega.
    -   [Poe väärtus vahemälu](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - poe üksuse vajutamisega vahemälu.
    -   [Eemaldage väärtus vahemälust](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Eemalda üksuse vajutamisega vahemälu.
-   [Rist poliitika][] 
    -   [Luba domeenide kõned][] – teeb API puuetega inimestele juurdepääsetavate Adobe Flash ja Silverlight Microsofti brauseri kliendid.
    -   [CORS][] – lisab rist-päritolu ressursside jagamise (CORS) toe toimingu või API domeenide kõnede Brauseripõhine kliendi kaudu.
    -   [JSONP][] - lisab JSON toimingu toega täidis (JSONP) või API domeenide kõnede JavaScripti Brauseripõhine kliendi kaudu.
-   [Teisendus poliitika][] 
    -   [XML-i teisendamine JSON][] - teisendab taotluse või vastuse keha kaudu JSON XML-i.
    -   [JSON teisendamine XML][] - teisendab taotluse või vastuse keha JSON-XML-ist.
    -   [Otsing ja asendus stringi kehasse][] - leiab taotluse või vastuse alamstringi ja asendab selle eri alamstringi.
    -   [Sisu maski URL-id][] - vastus linkide uuesti kirjutab (maskid) keha nii, et nad võrdväärse link lüüsi kaudu.
    -   [Määrata taustväärtus teenuse][] - muudab sissetuleva taotluse taustväärtus teenus.
    -   [Määrake keha][] - määrab sõnumi kehasse sissetulevad ja väljaminevad taotlused.
    -   [Määrake HTTP päise][] - määrab väärtuse olemasolevate vastus ja/või taotluse päis või lisab uue vastuse ja/või taotluse päis.
    -   [Määrake päringustringi parameetri][] - lisab asendab väärtus või kustutab taotlemine päringustringi parameetri.
    -   [Ümberkirjutamine URL][] - teisendab taotluse URL-i veebiteenus oodatud vormi oma avaliku kujul.

## <a name="next-steps"></a>Järgmised sammud

Poliitika avaldiste kohta leiate lisateavet teemast järgmisest videost.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Juurdepääsu piiramise poliitika]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Märkige ruut HTTP päis]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Piiratud kõne määr märkimise teel]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Piira helistaja IP-d]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Tellimuse kvoodi Set kasutamine]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Kinnitage JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Täiustatud poliitika]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Muutuja seadmine]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[avaldised]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[kontekst]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Koosolekukutse edasisaatmine]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Jaoturi sündmuste logi]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Poliitika autentimine]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Autentimiseks Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Autentimiseks kliendi sert]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Vahemällu poliitika]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Vahemälu toomine]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Vahemällu talletamine]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Rist poliitika]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Domeenide kõnede lubamine]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Teisendus poliitika]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[XML-i JSON teisendamine]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[XML-i teisendamine JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Otsing ja asendus stringi kehasse]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Sisu maski URL-id]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Määramine taustväärtus teenus]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Kehateksti määramine]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[HTTP-päise seadmine]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Päringustringi parameetri seadmine]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[URL-i ümberkirjutamine]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Poliitikate API haldus]: api-management-howto-policies.md
[API halduse poliitika viide]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Poliitika avaldised]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
