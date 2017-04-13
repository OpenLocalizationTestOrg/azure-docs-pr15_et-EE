<properties
    pageTitle="Autentimise ja luba API rakendused teenuses Azure rakendus | Microsoft Azure'i"
    description="Azure'i rakendust Service API rakenduste sisaldava autentimise ja luba teenuseid tundma õppida."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Autentimise ja luba API rakendused teenuses Azure rakendus

## <a name="overview"></a>Ülevaade 

> [AZURE.NOTE] Selles teemas migreeritakse soovite konsolideeritud [rakenduse autentimise / autoriseerimine](../app-service/app-service-authentication-overview.md) teema, mis hõlmab Web ja Mobile API rakendused.

Azure'i rakenduse teenus pakub sisseehitatud autentimise ja luba, et [OAuth 2.0](#oauth) ja [OpenID ühendamine](#oauth). Selles artiklis kirjeldatakse teenused ja API rakendused teenuses Azure rakenduse jaoks saadaolevad suvandid.

Järgmine diagramm näitab mõned rakenduse autentimise põhitunnused.

* See preprocesses API sissetulevad taotlused, mis tähendab, et see töötab framework rakenduse teenus ei toeta või keele.
* See pakub mitmeid võimalusi palju autentimist töötada, mida soovite teha oma kood.
* See töötab nii lõppkasutaja ja teenuse konto autentimine. 
* See toetab viis identiteedipakkujad: Azure Active Directory, Facebooki, Google, Twitteri ja Microsofti Account.
* See toimib sama API rakendused, veebirakenduste ja mobiilirakenduste kohta.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Keele agnostik

Rakenduse teenuse autentimise töötlemine toimub enne taotlusi jõuavad API rakendus, mis tähendab, et autentimise funktsioonid töötavad API rakendused või mis tahes keeles kirjutatud.  Oma API põhjal ASP.net-i, Java, Node.js või mis tahes raamistiku, mis toetab rakenduse teenus.

Rakenduse teenuse edastab JSON web valemisilti (JWT) HTTP-päring päises autoriseerimine ja koodi või mis tahes keeles kirjutatud saada teavet märgiks. Lisaks rakenduse teenuse annab teile juurdepääsu kõige sagedamini kasutatav taotluste seadmisega teisiti päised, nt mõni järgmistest:

* X-MS-KLIENDI--TURVASUBJEKTINIMI
* X-MS-KLIENDI-PÕHISUMMA-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
.Net-i API, saate kasutada funktsiooni `Authorize` atribuut ja kohandatud loa saate hõlpsalt kirjutada koodi alusel taotluste, kuna .NET klassid sisustatakse taotluste teave teie jaoks.

## <a name="multiple-protection-options"></a>Mitme kaitse suvandid

Rakenduse teenuse saate vältida anonüümse HTTP päringuid API rakenduse jõudmist, seda saab edasi kõik kutsed ja kinnitage sõned nõuab, et lisada need või saate lasta kaudu kõik kutsed neid toiminguid tegemata:

1. Luba ainult autenditud taotlusi API rakenduse saavutamiseks.

    Kui anonüümse taotluse brauseri kaudu, rakenduse teenuse suunab sisselogimise lehele pakkuja autentimine (Azure AD, Google, Twitteri, jne) teie valitud. 

    See suvand, ei pea te rakenduse üldse mis tahes autentimise koodi kirjutamine ja luba kood on lihtsam, kuna kõige olulisemad nõuded on toodud HTTP päised.

2. Luba kõik kutsed jõuda API rakenduse, kuid valideerida autenditud taotleb ja mööda autentimine on HTTP päistes leiduv teave.

    See suvand annab teile rohkem paindlikkust anonüümse päringute töötlemise, kuid teil on koodi kirjutamine, kui soovite takistada anonüümsete kasutajate abil oma API. Kuna kõige populaarsemate taotluste edastatakse HTTP päringuid päised, kood on üsna lihtne.
    
3. Luba kõik kutsed saavutamiseks oma API, taotluste autentimise teavet pole midagi teha.

    See suvand jätab autentimise ja luba täiesti kuni teie taotlus koodi ülesanded.

[Azure'i portaalis](https://portal.azure.com/), valite suvandi, mida soovite selle **autentimine / autoriseerimine** tera.

![](./media/app-service-api-authentication/authblade.png)

Suvandite 1 ja 2, sisselülitamine **Rakenduse autentimise**ja valige ripploendist **toiming taotluse pole autenditud** **Logi sisse** - või **luba taotluse (midagi)**.  Kui otsustate, et **sisse logida**, tuleb valida autentimisteenuse pakkuja ja selle pakkuja konfigureerimine.

![](./media/app-service-api-authentication/actiontotake.png)

Üksikasjalikku teavet selle kohta, kuidas konfigureerida autentimine, vaadake, [Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Azure Active Directory Logi sisse](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). See artikkel kehtib API apps kui ka mobiilirakenduste ja selle lingid teiste artiklite autentimisteenuse pakkujate.
 
## <a id="internal"></a>Teenuse konto autentimine

Rakenduse autentimise toimib nagu sisemine võtted helistaja API ühest rakendusest teise API rakendusse. Sel juhul saate märgiks teenuse konto asemel lõppkasutaja mandaadi mandaadi abil. Teenusele konto on ka *teenuse põhisumma* Azure Active Directory ja sellise konto kasutamine autentimine on ka teenusest stsenaarium. 

Teenusest stsenaariumid, nimega API rakenduse Azure Active Directory abil kaitsta ja sisestage AAD teenuse põhisumma autoriseerimine luba API rakenduse helistamisel. Saate esitada kliendi ID ja kliendi salajane rakendusest AAD märgiks. Pole teisiti Azure ainult kood on nõutav, peab olema täidetud käsitlemise Mobile teenuste Zumo luba kasutada. Selle stsenaariumi ASP.net-i API rakenduste kasutamise näide on hõlmatud õpetuse [põhisumma autentimise API rakendused](app-service-api-dotnet-service-principal-auth.md).

Kui soovite käsitleda-teenusest stsenaarium rakenduse autentimise kasutamata, saate kliendi sertide või lihttekstautentimine. Kliendi sertide Azure kohta leiate teavet teemast [Kuidas soovite konfigureerida TLS vastastikune autentimine Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Elementaarautentimine ASP.net-i kohta leiate teavet teemast [Autentimise filtrid ASP.net-i veebi-API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Teenuse konto autentimine rakendus App teenuse loogika API rakendus on eriline juhtum, mis on selgitatud teemas [oma kohandatud API majutatud rakenduse teenuse loogika rakenduste kaudu](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobiilikliendi autentimine

Lisateavet selle kohta, kuidas hallata autentimine mobiilikliendid, leiate [dokumendid autentimise mobiilirakenduste kohta](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Rakenduse autentimise töötab samal viisil mobiilirakenduste ja API rakendused.
  
## <a name="more-information"></a>Lisateave

Vaadake autentimise ja luba teenuses Azure rakenduse kohta leiate lisateavet järgmistest teemadest:

* [Laiendatud rakenduse autentimise ja luba](/blog/announcing-app-service-authentication-authorization/)
* [Kuidas konfigureerida rakenduse teenuserakenduse kasutamiseks Azure Active Directory sisselogimine](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Sisaldab linke muude autentimisteenuse pakkujate lehe ülaosas.) 

OAuth 2.0 kohta lisateabe saamiseks OpenID ühendamine ja JSON Web sõned (JWT), leiate järgmistest teemadest.

* [OAuth 2.0 töötamise alustamine] (http://shop.oreilly.com/product/0636920021810.do "OAuthi 2.0 töötamise alustamine") 
* [Sissejuhatus OAuth2, OpenID ühendamine ja JSON Web märkide (JWT) – PluralSight kursuse](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Koostamise ja turvaliseks rahulik API ASP.net-i - PluralSight kursuse mitme kliendi jaoks](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Teemast Azure Active Directory kohta leiate lisateavet järgmistest teemadest.

* [Azure'i AD stsenaariumid](http://aka.ms/aadscenarios)
* [Azure'i AD arendajate juhend](http://aka.ms/aaddev)
* [Azure'i AD näidised](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis on selgitatud autentimise ja luba funktsioonid rakenduse teenuse, mida saate kasutada API rakendused. Järgmise õppeteema saamisega alustamine sarja näitab, kuidas rakendada [Kasutaja autentimise rakenduse teenuse API rakendustes](app-service-api-dotnet-user-principal-auth.md).
