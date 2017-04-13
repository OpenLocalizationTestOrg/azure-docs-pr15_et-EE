###<a name="server-auth"></a>Kuidas: autentida pakkujaga (Server vool)

On Mobile'i rakenduste haldamine autentimise protsessi rakenduse, peate registreerima rakenduse oma identiteedipakkuja juures. Seejärel Azure'i rakendust Service, peate rakenduse ID ja salajane pakkuja konfigureerimine.
Lisateabe saamiseks vt õpetuse [rakenduse lisamine autentimise].

Kui olete oma identiteedipakkuja registreeritud, lihtsalt helistada .login() meetodit pakkuja nimi. Näiteks järgmine kood Facebooki kasutamine sisse logida.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Identiteedi pakkuja peale Facebooki kasutamisel edasi ühte järgmistest eeltoodud meetodi login väärtust muuta: `microsoftaccount`, `facebook`, `twitter`, `google`, või `aad`.

Sel juhul Azure'i rakendust Service haldab OAuth 2.0 autentimist voo kuvamine sisselogimislehe valitud pakkuja ja luues rakenduse teenuse autentimise luba pärast edukat sisselogimist identiteedipakkuja juures. Logi sisse, kui olete lõpetanud, tagastab funktsioon JSON objekt (kasutaja), mis seab kasutaja ID ja rakenduse teenuse autentimise luba väljadele Kasutajanimi ja authenticationToken vastavalt. Selle märgiks saab vahemälus talletatud ja uuesti kasutada enne selle lõppemist.

###<a name="client-auth"></a>Kuidas: autentida pakkujaga (klient vool)

Rakenduse saate ka sõltumatult pakkujalt identiteedi ja seejärel andke tagastatud luba rakenduse teenust autentimiseks. See kliendi vool võimaldab teil ühe sisselogimine kasutaja või kasutajate täiendavate andmete toomiseks identiteedipakkuja juures.

#### <a name="social-authentication-basic-example"></a>Social autentimise tavalise näide

Selles näites kasutatakse Facebooki kliendi SDK autentimise:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
Selles näites eeldab, et vastava pakkuja SDK luba on talletatud Turbeloa muutujana.

#### <a name="microsoft-account-example"></a>Microsofti Account näide

Järgmises näites kasutatakse Live Tarkvaraarenduskomplektist, mis toetab ühekordse sisselogimise Windowsi poe rakenduste Microsoft Account abil:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

Selles näites saab märgiks Live ühenduse esitatud rakenduse teenust login funktsiooni.

###<a name="auth-getinfo"></a>Kuidas: autenditud kasutaja kohta teabe saamiseks

Praeguse kasutaja autentimise teavet võib laadida soovitud `/.auth/me` lõpp-punkti AJAXI mis tahes viisil.  Veenduge, et saate määrata selle `X-ZUMO-AUTH` oma autentimise luba päis.  Luba autentimine on talletatud `client.currentUser.mobileServiceAuthenticationToken`.  Näiteks, et kasutada toomise API:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Toomise on saadaval npm dokumendina või brauseri alla laadida CDNJS. Teabe toomiseks võib kasutada ka jQuery või muu AJAXI API.  Andmete vastu JSON objektina.
