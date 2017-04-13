<properties 
    pageTitle="Kuidas volitatud kasutaja registreerimise ja toote tellimuse" 
    description="Saate teada, kuidas volitatud kasutaja registreerimise ja toote tellimuse kolmandale osapoolele Azure'i API haldamine." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Kuidas volitatud kasutaja registreerimise ja toote tellimuse

Delegeerimine võimaldab kasutada oma olemasoleva veebisaidi arendaja Logi-sisse/sign-üles ja toodete asemel sisseehitatud funktsiooni arendaja portaalis tellimus. See võimaldab veebisaidi omanik kasutaja andmeid ning teha järgmist kinnitamise kohandatud viisil.

## <a name="delegate-signin-up"> </a>Sisselogimine ja registreerumise Delegating arendaja

Sisselogimine ja registreerumise, et peate oma saidil, mis toimib algatada API arendaja haldusportaali nõudmise sisendpunkti on teisiti delegeerimine lõpp-punkti loomiseks olemasoleva veebisaidi arendaja volitatud.

Lõplik töövoog on järgmine:

1. Arendaja klõpsuga sisselogimise või registreerumise lingil API haldusportaali arendaja
2. Brauser on ümber delegeerimine lõpp-punkti
3. Delegeerimine lõpp-punkti tagasi suunab või esitab küsib kasutaja sisselogimise või registreerumise UI
4. Edu, kasutaja suunatakse API halduse arendaja portaali lehele võrreldes


Kõigepealt loome esimese ülesseadmine API halduse marsruutimiseks taotleb kaudu oma delegeerimine lõpp-punkti. API Publisheri haldusportaal, klõpsake **Turvalisus** ja seejärel vahekaarti **delegeerimine** . Klõpsake 'Volitatud esindaja sisselogimine ja registreerumise' lubamiseks märkige ruut.

![Lehe delegeerimine][api-management-delegation-signin-up]

* Otsustage, mis teie teisiti delegeerimine lõpp-punkti URL-i jäävad ning sisestage see väljale **delegeerimine lõpp-punkti URL** . 

* Sisestage **delegeerimine autentimise võti** välja arvutada teile kinnitamiseks tagada kutse on tõepoolest pärit Azure API Management signatuuri kasutatud salajase. Võite klõpsata olema API Managemnet CEIP luua võti saate **luua** nupp.

Nüüd tuleb **delegeerimine lõpp-punkti**loomiseks. See on teha mitmesuguseid toiminguid.

1. Vastuvõtmisele järgneva toimingu järgmisel kujul:

    > *{lähtelehe URL} http://www.yourwebsite.com/apimdelegation?operation=signin&returnUrl= ja soola {stringi} = & sig = {stringi}*

    Päringu parameetrite Sisselogimine / registreerumine puhul:
    - **toiming**: tuvastab tüübi delegeerimine seda taotleda on – seda saab ainult **SignIn** sel juhul
    - **returnUrl**: leht, kui kasutaja on klõpsanud sisselogimise või registreerumise lingi URL
    - **soola**: kasutada arvuti turvalisuse räsi teisiti soola string
    - **SIG**: võrdlus oma kasutatakse arvutatud turvalisus räsi arvutada räsi

2. Veenduge, et kutse on pärit Azure API Management (valikuline, kuid väga soovitatav turvalisuse)

    * Arvutage mõne HMAC-i-SHA512 räsi stringi **returnUrl** ja **soola** päringu parameetrite ([allpool toodud näites kood]):
        > HMAC-i (**soola** + '\n' + **returnUrl**)
         
    * Võrrelge **sig** Päringuparameetri väärtus ülaltoodud arvutada räsi. Kui kaks hashes vasta, liikuda edasi järgmise juhise juurde, muidu keelata.

2. On saabunud Logi sisse/Logi-üles taotluse kinnitamine: **toiming** Päringuparameetri väärtuseks "**SignIn**".

3. Praegune kasutaja sisselogimise või registreerumise kasutajaliides

4. Kui kasutaja on sisestamisel tuleb luua vastav konto neid API haldus. [Loo kasutaja] API halduse REST API-ga. Seda tehes tagada Määrake kasutaja ID-d, sama on oma kasutaja salvestada või ID, mida te saate silma peal hoida.

5. Kui kasutaja on edukalt autenditud:

    * [ühekordse sisselogimise (SSO) luba taotleda] API haldamine REST API kaudu

    * olete saanud API kõne kohal SSO URL-i returnUrl Päringuparameetri lisamiseks järgmist.
        > nt https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * Kasutaja suunatakse eespool toodetud URL

Lisaks **SignIn** toimingu, saate teha ka kontohaldus eelmisi juhiseid järgides ja kasutades ühte järgmistest toimingutest.

-   **Parooli muutmine**
-   **ChangeProfile**
-   **CloseAccount**

Päringu järgmisi konto toimingute jaoks peate läbima.

-   **toiming**: tuvastab, mis tüüpi delegeerimine taotlus on (parooli muutmine, ChangeProfile või CloseAccount)
-   **kasutajanimi**: kasutaja id konto haldamine
-   **soola**: kasutada arvuti turvalisuse räsi teisiti soola string
-   **SIG**: võrdlus oma kasutatakse arvutatud turvalisus räsi arvutada räsi

## <a name="delegate-product-subscription"> </a>Delegeerivad toote tellimuse

Toote tellimuse delegeerivad töötab sarnaselt delegeerivad kasutaja sisselogimine/üles. Lõplik töövoog oleks järgmiselt:

1. Arendaja valib toote API haldusportaal arendaja ja klõpsake nuppu Telli
2. Brauser on ümber delegeerimine lõpp-punkti
3. Delegeerimine lõpp-punkti sooritab nõutud toote tellimuse juhiseid – see on teie ja võib kaasa tuua ümbersuunamine taotleda arveldusteabe, täiendavaid küsimusi, või lihtsalt teabe salvestamine ja ei nõua mis tahes Kasutajatoiming muule lehele


Selle funktsiooni lubamiseks klõpsake lehel **delegeerimine** **volitatud esindaja toote tellimuse**.

Veenduge, et delegeerimine lõpp-punkti teha järgmisi toiminguid:


1. Vastuvõtmisele järgneva toimingu järgmisel kujul:

    > *{toiming} http://www.yourwebsite.com/apimdelegation?operation= & productId = {toote tellida} & kasutajanimi = {kasutaja taotluse} & soola = {stringi} & sig = {stringi}*

    Päringu parameetrite toote tellimuse puhul:
    - **toiming**: tuvastab, millist tüüpi delegeerimine taotlus on. Toote tellimuse taotlused kehtivad suvandid on:
        - "Telli": esitatud taotluse tellimise antud toote kasutaja ID-d (vt allpool)
        - "Tellimuse tühistamine": kasutaja toote tellimuse tühistamise taotlus
        - "Pikendada": on taotluse korral andma (nt mis aeguv) tellimuse pikendamiseks
    - **toote ID**: kasutajal palutakse tellimine toote ID
    - **kasutajanimi**: kasutaja, kelle taotluse ID-d
    - **soola**: kasutada arvuti turvalisuse räsi teisiti soola string
    - **SIG**: võrdlus oma kasutatakse arvutatud turvalisus räsi arvutada räsi


2. Veenduge, et kutse on pärit Azure API Management (valikuline, kuid väga soovitatav turvalisuse)

    * Arvutage HMAC-i SHA512 välja **toote ID**, **kasutajanimi** ja **soola** päringu parameetrite stringi:
        > HMAC-i (**soola** '\n' + **productId** + '\n' + **kasutajanimi**)
         
    * Võrrelge **sig** Päringuparameetri väärtus ülaltoodud arvutada räsi. Kui kaks hashes vasta, liikuda edasi järgmise juhise juurde, muidu keelata.
    
3. Toote tellimuse töötlemine põhjal tüüpi nõutud **toiming** – nt arveldus täiendavate küsimuste jne toiming teha.

4. Tellige edukalt tellimise tootega teie kasutaja, klõpsake helistades [REST API toote tellimuse]kasutajal toote API haldamine.

## <a name="delegate-example-code"></a> Näide kood ##

Kuidas võtta *delegeerimine valideerimine klahvi*, mida on seatud delegeerimine kuval Publisheri portaali, luua mõne HMAC-i siis soovitud signatuur kinnitamiseks kasutatava tõendada edastatud returnUrl kehtivust nende koodinäiteid kuvamine Sama koodi toimib productId ja koos vähesel kasutajanimi.

**C# koodi loomiseks räsi returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**Luua returnUrl räsi NodeJS kood**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Järgmised sammud

Delegeerimine kohta leiate lisateavet teemast järgmisest videost.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[ühekordse sisselogimise (SSO) luba taotlemine]: http://go.microsoft.com/fwlink/?LinkId=507409
[kasutaja loomine]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[helistamiseks REST API toote tellimuse jaoks]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[alltoodud näites kood]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 