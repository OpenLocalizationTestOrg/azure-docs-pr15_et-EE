<properties
    pageTitle="Rist-rakenduse SSO Android abil ADAL lubamine | Microsoft Azure'i"
    description="Kuidas kasutada funktsioone ADAL SDK üle rakenduste ühekordse sisselogimise lubamiseks. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Rist-rakenduse SSO Android abil ADAL lubamine


Pakkudes ühekordse sisselogimise (SSO) nii, et kasutajad ainult peate sisestama oma identimisteabe üks kord ja on mandaadi automaatselt töötavad nüüd peaks rakenduste kliendid. Väike ekraan, sageli korda koos täiendavate tegur (2FA), telefonikõne või texted koodi, nt oma kasutajanime ja parooli sisestamise raskusi tulemuste kiirülevaate rahulolematust, kui kasutaja on selleks rohkem kui üks kord teie toode.

Lisaks kui saate kasutada identiteedi platvormi, mis muudes rakendustes võib kasutada näiteks Microsofti Accounts või: Office 365 konto töö, Kliendid eeldavad, et need identimisteabe üle kõik rakendused sõltumata hankija kasutamiseks kättesaadavaks.

Microsofti Identity platvorm, koos meie Microsofti Identity SDK-d, kas kõik selle tööd teile ja pakub võimalus rõõmu oma klientidele koos SSO-ga, kas oma rakenduste komplekti või peab ta võimalus ja Autentija rakendustel kogu seade.

Juhendis ütleb teile, kuidas konfigureerida meie SDK oma rakenduses, et nad saaksid oma klientidele.

Juhendis kehtib:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Azure Active Directory juurdepääsu

Pange tähele, et allpool dokumendi eeldab, saate teadmisi, kuidas [näha rakenduste pärand portaalis Azure Active Directory jaoks](./develop/active-directory-how-to-integrate.md) on samuti on integreeritud [Microsoft identiteedi Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android)rakenduse.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Microsofti Identity platvormi SSO mõisted

### <a name="microsoft-identity-brokers"></a>Microsofti Identity maaklerid

Microsoft pakub rakendused iga mobiilne platvorm, mis võimaldavad identimisteabe ajutiseks kõigis rakendustes erinevad müüjad, samuti võimaldab teisiti täiustatud funktsioone, mis nõuavad ühe kindlas kohas, kus valideerimiseks mandaat. Me kutsume need **maaklerid**. IOS ja Android neid pakutakse tasuta rakenduste kaudu, et kliendid installima ükshaaval või saate lükata seadmele firma, kes haldab mõnda või kõiki seadme saavad töötajate jaoks. Need maaklerid tugiteabe haldamise turvalisus vaid mõned rakendused või kogu seadme põhjal, mida soovivad IT-administraatorid. Windows pakub see funktsioon on operatsioonisüsteem, mida tehnilises Web autentimise ta on sisse ehitatud konto valija.

Mõista nende maaklerid kasutamine ja kuidas oma klientidele, võidakse kuvada need oma sisselogimise liikumise kohta lisateabe saamiseks lugege Microsofti Identity platvormi.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mustrid mobiilsideseadmetes logimine

Juurdepääs identimisteabe seadmetes tehke kahe põhilise mustrid Microsofti Identity platvormi.

* Mitte-ta toetatud logimine
* Keda ta logimine

#### <a name="non-broker-assisted-logins"></a>Mitte-ta toetatud logimine

Mitte-ta toetatud logimine on login kogemusi, mis juhtub Tekstisisese rakenduse ja kasutada seadme rakenduse kohalik salvestusruum. See salvestusruumi võib jagada kõigis rakendustes, kuid identimisteabe tihedalt seotud rakendus või rakendused, et mandaati kasutades komplekti. See on tõenäoliselt olete kogenud palju mobiilirakendustes kogemusi, kuhu sisestada kasutajanimi ja parool rakendusest kiirsõnumeid.

Need sisselogimise on järgmised eelised:

-  Kasutusvõimalused olemas täiesti rakendusest.
-  Mandaadi saab jagada rakendusi, mis on allkirjastatud serti, pakkudes ühekordse sisselogimise kogemusi oma komplektile rakenduste.
-  Juhtelemendi ümber kogemus logimine on esitatud rakendusse enne ja pärast sisselogimist.

Need sisselogimise on järgmised puudused:

- Kasutaja ei saa üle kõik rakendused, mida kasutada Microsofti Identity, ainult üle nende Microsoft Identities rakenduse kuulub ja konfigureerinud kogemus ühekordse sisselogimise kohta.
- Rakenduse täpsemate business funktsioone, nagu juurdepääsu ei saa kasutada või kasutage InTune komplekti tooted.
- Rakenduse ei toeta vastavalt serdi autentimist ärikasutajatele.

Siin on esitus, kuidas Microsofti Identity SDK-d töötamine ühiskasutusega talletamist lubamiseks SSO rakenduste:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Keda ta logimine

Ta abil sisselogimise on login kogemusi, mis toimub ta rakenduses ja kasutada salvestusruumi ja turvalisus ta identimisteabe ühiskasutamiseks kõik rakendused seadmes kasutada Microsofti Identity platvormi. See tähendab, et rakenduste sõltuvad ta selleks, et kasutajad sisse logida. IOS ja Android neid pakutakse tasuta rakenduste kaudu, et kliendid installima ükshaaval või saate lükata seadmele mõni ettevõte, kes haldab oma kasutaja seade. Seda tüüpi rakendus näide on Azure Autentija rakenduse kohta. Windows pakub see funktsioon on operatsioonisüsteem, mida tehnilises Web autentimise ta on sisse ehitatud konto valija.
Kogemusi muutub platvormi ja võib mõnikord olla segab kasutajate kui mitte hallatavate õigesti. Olete ilmselt kõige tuttav seda mudelit, kui teil on installitud Facebooki ja Facebook Login funktsioonide kasutamine mõnes muus rakenduses. Microsofti Identity platvormi mõjutab sama muster.

See viib "üleminek" iOS-i animatsiooni, kus saadetakse teie rakendus tausta ajal Azure'i Autentija rakenduste on esiplaanil, millist kontot soovite sisselogimiseks valige kasutaja.  

Androidi ja Windowsi konto valija kuvatakse teie rakendus, mis on väiksem segab kasutaja peal.

#### <a name="how-the-broker-gets-invoked"></a>Kuidas ta saab kasutada

Ühilduvad ta töötab seadme taustal, nagu Azure'i Autentija taotluse, installinud Microsofti Identity SDK-d automaatselt ei töö ta saate kasutada, kui kasutaja näitab, et nad soovivad mis tahes Microsofti Identity platvormi konto abil sisse logida. See võib olla ka Microsofti Account, töö või kooli kontot või konto, et pakkuda ja Azure meie B2C ja B2B tooteid majutada. Äärmiselt turvaline algoritmide ja krüptimise abil tagada me identimisteabe küsida ja toimetatud tagasi rakenduse turvaline viisil. Tehniline täpsete andmete need on avaldatud, kuid on välja töötatud koostöö Apple ja Google.

**Arendaja on valik, kui Microsofti Identity SDK helistab ta või kasutab-ta toetatud voogu.** Aga kui arendaja ei soovi kasutada ta abil voogu ta ilma kasu SSO mandaat, kui kasutaja on juba lisanud seadmes kui ka takistab saavad rakenduse business funktsioone Microsoft pakub oma klientidele, nt Intune haldusvõimalusi tingimusvormingu Access kasutamist ja serdi põhineva autentimise kasutamine.

Need sisselogimise on järgmised eelised:

-  Kasutaja kogemusi SSO üle kõik rakendused sõltumata hankija.
-  Rakenduse saate kasutada täpsemaid business funktsioone, näiteks juurdepääsu või kasutada Intune'i komplekti tooted.
-  Teie rakendus toetab vastavalt serdi autentimist ärikasutajatele.
- Palju turvalist sisselogimine nimega rakendus ja kasutajale on kinnitatud ta taotluse algoritmide kohta täiendavat turvalisus ja krüptimine.

Need sisselogimise on järgmised puudused:

- IOS-is on kasutaja transitioned välja oma rakenduse kogemus ajal identimisteave on valitud.
- Võimalus hallata oma klientidele oma rakenduses login kogemus kadu.



Siin on kujutis sellest, kuidas lubada SSO rakendustega ta töötab Microsofti Identity SDK-d.

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Relvastatud seda peaks oskama paremini mõista ja rakendada oma rakenduses, kasutades Microsofti Identity platvormi ja SDK-d SSO taustateabe.


## <a name="enabling-cross-app-sso-using-adal"></a>Rist-rakenduse SSO ADAL kasutamise lubamine

Siin kasutame ADAL Android SDK abil:

- Lülitage sisse-ta toetatud SSO oma komplekti rakendused
- Ta abil SSO toe sisselülitamine


### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Sisselülitamine SSO-ta jaoks keda SSO-d

Ta toetatud SSO kõigis rakendustes Microsoft identiteedi SDK-d haldamine palju SSO keerukus teie eest. See hõlmab leida õige kasutaja vahemälu ja säilitada saate päringu sisseloginud kasutajate loendit.

Kõigis rakendustes SSO lubamiseks kuulub teile, peate tegema järgmist:

1. Tagada teie rakenduste kasutaja sama kliendi ID või rakenduste ID-d.
* Tagatakse kõikide rakenduste on sama SharedUserID määramine.
* Veenduge, et kõik rakendused, et jagate salvestusruumi jagada sama Allkirjaserdi Google Play poest.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Samm 1: Kasutades sama kliendi ID / rakenduste ID oma rakenduste komplekti kõigi rakenduste jaoks

Selleks, et leida, et see on lubatud märkide ühiskasutamiseks rakenduste Microsofti Identity platvormi, iga rakenduste on vaja jagada sama kliendi ID või rakenduste ID-d. See on kordumatu ID, mis andis teile esimese rakenduse registreerimisel portaalis.

Võib tekkida, kuidas te tuvastada erinevate rakenduste Microsofti Identity teenusega kui kasutab sama rakenduse ID. Vastus on **Ümber suunata URI-d**. Iga rakendus võib olla mitu ümber suunata URI-d registreerimis portaalis kasutuselevõtt. Iga app oma komplektis on erinevate redirect URI. Allpool on näide sellest, kuidas see välja näeb.

APP1 Redirect URI:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

APP2 Redirect URI:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 Redirect URI:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

....

Need on ühendatud alla sama kliendi ID / rakenduste ID-d ja vaadanud redirect URI meile konfiguratsioonis SDK tagastada põhjal.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Pange tähele, et need ümber suunata URI-d vorming allpool. Võite kasutada mis tahes ümber suunata URI juhul, kui soovite, et ta, toetavad sel juhul ta peab välja nägema umbes eespool*


#### <a name="step-2-configuring-shared-storage-in-android"></a>Samm 2: Konfigureerimine ühiskasutusega salvestusruumi Android

Säte on `SharedUserID` on selle dokumendi väljapoole, kuid saate õpitut lugedes Google Android dokumentatsiooni [näidata](http://developer.android.com/guide/topics/manifest/manifest-element.html). Oluline on see, te otsustate, mida soovite oma sharedUserID nimetatakse ja kasutada üle kõikide rakenduste.

Kui teil on selle `SharedUserID` kõikides oma rakendustes olete valmis kasutama SSO-d.

> [AZURE.WARNING]
Kui storage ühiskasutamiseks rakenduste mis tahes rakenduses saate kasutajate kustutamine või hullem kustutada kogu rakenduse kõik märgid. See on eriti katastroofiline, kui teil on, et taust töö sõned rakendused. Ühiskasutuse salvestusruumi tähendab, et peate olema väga ettevaatlik, et kõik Eemalda toiminguid Microsofti Identity SDK-d.

See on õige! Microsofti Identity SDK nüüd jagada üle kõikide rakenduste mandaat. Kasutajate loendit jagatakse samuti mitmes eksemplaris rakendus.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Sisselülitamine SSO jaoks ta keda SSO-d

Võimalus kasutada mis tahes ta, mis on seadmesse installitud rakendus on **vaikimisi välja lülitatud**. Rakenduse kasutamine ta peate teha mõned täiendavad konfiguratsioon ja mõned koodi lisamine rakenduse.

On tehke järgmist.

1. Rakenduse koodi kõne MS SDK ta režiimi lubamine
2. Uue suuna URI ja esitage mis nii rakendus ja registreerimise rakendus
3. Kuidas häälestada Androidi manifestis vajalikke õigusi


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Samm 1: Luba oma rakenduse ta režiim
Võimalus rakenduse kasutamiseks ta on sisse lülitatud "settings" või Alghäälestus oma autentimise eksemplari loomisel. Saate seda teha oma ApplicationSettings tüübi koodi:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Samm 2: Luua uue suuna URI koos oma URL-i skeem

Selleks, et me alati naasta mandaati sõned õige rakendus, läheb vaja veendumaks, et me kutsume tagasi rakenduse nii, et Android operatsioonisüsteemi saate kontrollida. Androidi operatsioonisüsteem kasutab serdi räsi Google Play poest. See ei saa võltsitud petturitest rakendus. Seetõttu me kasutada seda koos URI meie ta rakenduse, et märkide on õige rakendus tagasi. Me nõuavad seda kordumatu suunata URI nii luua oma rakenduse ja ümber suunata URI meie arendaja portaalis seadmine.

Teie redirect URI peab olema õige kujul:

`msauth://packagename/Base64UrlencodedSignature`

ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Selle ümber suunata URI peab olema määratud rakenduse registreerimise [Azure klassikaline portaalis](https://manage.windowsazure.com/). Azure AD Rakenduse registreerimise kohta leiate lisateavet teemast [integreerimine Azure Active Directory](./develop/active-directory-how-to-integrate.md).


#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Samm 3: Häälestamine oma rakenduse vajalikke õigusi

Meie ta rakendus Android kasutab funktsiooni kontode halduri Android OS identimisteabe haldamine kõigis rakendustes. Selleks, et kasutada ta Androidi oma rakenduse manifesti peab olema õigused kasutada AccountManager kontod. See käsitletakse üksikasjalikult dokumentides [Google konto halduri siin] (http://developer.android.com/reference/android/accounts/AccountManager.html)

Eelkõige on järgmisi õigusi:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Olete konfigureerinud SSO!

Nüüd Microsofti Identity SDK automaatselt nii identimisteabe ühiskasutamiseks rakenduste ja autonoomsest ta, kui see on olemas oma seadmes.
