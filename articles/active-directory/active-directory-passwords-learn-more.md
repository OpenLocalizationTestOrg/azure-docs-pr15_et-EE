<properties
    pageTitle="Lisateave: Azure'i AD parooli haldus | Microsoft Azure'i"
    description="Täpsemate teemade loendit veebisaidil Azure AD parooli haldus, sh parooli tagasikirjutusega toimimise parooli tagasikirjutusega turvalisus, kuidas parooli lähtestamise portaali õige töö ja milliseid andmeid kasutab parooli lähtestamine."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Lisateavet parooli haldus

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

Kui teil juba juurutanud parooli haldus või lihtsalt vaadata asja tuum sellest, kuidas see toimib enne juurutamist, see jaotis teile hea ülevaade tehnilise mõisteid taga teenuse tehniliste kohta. Käsitleme järgmist:

* [**Parooli tagasikirjutusega ülevaade**](#password-writeback-overview)
  - [Parool tagasikirjutusega tööpõhimõtted](#how-password-writeback-works)
  - [Toetatud parooli tagasikirjutusega stsenaariumid](#scenarios-supported-for-password-writeback)
  - [Parooli tagasikirjutusega turvalisus mudel](#password-writeback-security-model)
* [**Kuidas parooli lähtestamise töö portaali?**](#how-does-the-password-reset-portal-work)
  - [Milliseid andmeid kasutatakse parooli lähtestamine?](#what-data-is-used-by-password-reset)
  - [Kuidas pääseda juurde parooli lähtestamine kasutajate andmed](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Parooli tagasikirjutusega ülevaade
Parooli tagasikirjutusega on [Azure Active Directory ühenduse](active-directory-aadconnect.md) komponent saab lubada ja kasutada praeguse tellijad, Azure Active Directory Premium. Lisateavet leiate teemast [Azure Active Directory väljaanded](active-directory-editions.md).

Parooli tagasikirjutusega võimaldab kirjutamiseks paroolide teile kohapealse Active Directory cloud rentniku konfigureerimine.  See välistab saate häälestada ja hallata keerukaid kohapealse Iseteeninduslik parooli lähtestamine lahenduse võttes ja pakub pilvepõhist mugavam kasutajate oma kohapealse paroole lähtestada asukohast olenemata.  Mõned peamised omadused parooli tagasikirjutusega lugege edasi.

- **Null viivitus tagasiside.**  Parooli tagasikirjutusega on sünkroonse toiming.  Kasutajaid teavitama kohe, kui parool ei vasta poliitika või ei saa lähtestamine või muuta mis tahes põhjusel.
- **Toetab AD FS-i või muid tehnoloogiaid federation abil kasutajate paroole lähtestada.**  Parooli tagasikirjutusega PivotTable-liigendtabelid, kui ühendatud Kasutajakontod sünkroonitakse teie Azure AD rentniku, nad suudavad hallata oma kohapealse AD paroolide pilveteenuses.
- **Toetab parooli räsi syncis kasutajate paroole lähtestada.** Kui parooli lähtestamine teenuse tuvastab, et sünkroonitud kasutajakonto parooli räsi sünkroonimise jaoks on lubatud, lähtestatakse nii selle konto kohapealse ja pilveteenuse parool korraga.
- **Muuta parooli kasutada paneeli ja Office 365 toetab.**  Kui ühendatud või parooli sünkroonimise oleks kasutajad on aegunud või aegunud parooli vahetada, me kirjutada need paroolid tagasi oma kohalik AD keskkonnas.
- **Toetab kirjutamine tagasi parooli lähtestamisel administraator neid on** [**Azure'i haldusportaal**](https://manage.windowsazure.com).  Iga kord, kui administraator lähtestatakse kasutaja parooliga [Azure'i haldusportaal](https://manage.windowsazure.com), kui kasutaja on ühendatud või parooli sünkroonimise, me seada parooli admin valib teie kohaliku AD kohta.  See on praegu ei toeta Office haldusportaali.
- **Teie kohapealse jõustab AD parool poliitika.**  Kui kasutaja oma parooli lähtestab, me veenduge, et see, et see vastaks teie asutusesisese AD poliitika enne rakendamist selle kausta.  See hõlmab ajalugu, keerukuse, vanus, parooli filtrid ja muud parooli piirangud on määratletud teie kohaliku AD.
- **Ei nõua sissetuleva tulemüüri reeglid.**  Parooli tagasikirjutusega kasutab mõnda Azure'i teenus siini relay aluseks side kanal, mis tähendab, et teil on avamiseks mis tahes sissetuleva pordid selle funktsiooni töötamiseks.
- **Kasutajakonto, mis on olemas kaitstud rühmades oma kohapealse Active Directory ei toetata.** Kaitstud rühmade kohta leiate lisateavet teemast [kaitstud kontode ja rühma Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Parooli tagasikirjutusega tööpõhimõtted
Parooli tagasikirjutusega on kolmest põhikomponendist.

- Parooli lähtestamine pilveteenuses (see on ka integreeritud Azure AD parooli muutmine lehed)
- Rentniku kohased Azure'i teenus siini edastamine
- Kohapeal parooli lähtestamise lõpp-punkti

Need sobivad kirjeldatud on allpool joonisel:

  ![][001]

Kui ühendatud või parooli räsi sünkroonimine sooviksite kasutaja on lähtestamine või muuta oma parooli pilves, toimub järgmine:

1.  Me kontrollige, millist tüüpi parooli kasutajal on.  Kui näeme, et kohapealne hallatakse parooli, siis me tagada tagasikirjutusega teenus on tööks.  Kui see on, anname selle kasutaja jätkata, kui ei ole me öelda kasutaja, mida ei saa oma parooli lähtestada siin.
2.  Järgmiseks kasutaja edastab sobiv autentimise gates ja jõuab ekraani Lähtesta parool.
3.  Kasutaja valib uue parooli ja kinnitab seda.
4.  Korral klõpsake nuppu Edasta, me krüptimine parooli Lihttekst sümmeetriline võti, mis on loodud tagasikirjutusega häälestamise käigus.
5.  Pärast krüptimine parooli, meil on see last, mis saadetakse üle mõne HTTPS kanali oma rentniku kindla teenuse siini relay (mis me ka teie häälestada tagasikirjutusega häälestamise käigus).  See edastamine on kaitstud juhuslik parool, mida teab ainult ettevõttesiseselt installitud.
6.  Kui sõnum jõuab teenuse siini, parooli lähtestamine lõpp-punkti automaatselt ärkab ja näeb, et see on ootel taotlust lähtestamine.
7.  Teenuse siis otsitakse kasutaja kõnealuse cloud ankur atribuudi abil.  Õnnestub, selle otsingu jaoks peab kasutajaobjektis olemas AD konnektori ruumi, peab olema seotud vastavate MV objekti ja peab olema seotud vastava AAD konnektor objekti. Lõpuks peab olema AD konnektor objekti lingi MV selleks Sünkrooni see kasutajakonto leidmiseks, sünkroonimise reegli `Microsoft.InfromADUserAccountEnabled.xxx` linki.  See on vajalik, kuna kui kõne tuleb pilvest, sync engine kasutab cloudAnchor atribuuti otsimiseks AAD konnektori ruumi objekti, seejärel järgmine link naasmiseks MV objekti ja seejärel järgmine link naasmiseks AD objekti. Kuna võib olla mitu AD objekti (mitme mets) sama kasutaja jaoks, sync engine sõltub selle `Microsoft.InfromADUserAccountEnabled.xxx` valida õige link.
8.  Kui kasutajakonto on leitud, proovige me otse vastav AD mets parooli lähtestamine.
9.  Kui parool toiming on edukalt, me öelda kasutaja parooli on muudetud ja mida nad oma kuidas minna.
10. Kui parool toiming nurjub, tagastatakse tõrge kasutajale ja temaga ja proovige uuesti.  Selle toimingu võib nurjuda, kuna teenus on allapoole, kuna need valitud parool ei vasta asutuse poliitikate, kuna me ei leia kasutaja kohaliku AD, või mis tahes mitmel põhjusel.  Oleme kindla sõnumi paljudele sellisel juhul ja kasutajale teada saada, mida nad saavad teha selle probleemi lahendamiseks.

### <a name="scenarios-supported-for-password-writeback"></a>Toetatud parooli tagasikirjutusega stsenaariumid
Järgmises tabelis kirjeldatakse, millised versioonid meie sünkroonimine võimaluste stsenaariumid, mis on toetatud.  Üldiselt on soovitatav, kui soovite kasutada parooli tagasikirjutusega installida [Azure AD-ühenduse](active-directory-aadconnect.md#install-azure-ad-connect) uusim versioon.

  ![][002]

### <a name="password-writeback-security-model"></a>Parooli tagasikirjutusega turvalisus mudel
Parooli tagasikirjutusega on väga turvaline ja robustne teenus.  Selleks, et tagada, et teie andmed on kaitstud, võimaldada 4-astmelise turvalisus mudelit, mida on kirjeldatud allpool.

- **Rentniku kindla teenuse-siini edastamine** – teenus häälestamisel me loodud rentniku kohased teenuse siini relay, mis on kaitstud juhuslik keeruka parooli Microsoft kunagi on juurdepääs.
- **Lukus allapoole, krüptimise osas tugev, parooli krüptovõtme** – teenus siini relay loomise järel, saame luua tugev sümmeetriline võti, mida me kasutame krüptimine parooli, kui tegemist on üle kaabel.  See võti elu ainult teie ettevõtte salajane poe pilves, mis on tugevalt lukus ja auditeerida nii nagu mis tahes parooli kataloogis.
- **Valdkonna standard TLS** – kui parooli lähtestamine või muutmine toiming ilmneb pilves, võtame Lihttekst parooli ja krüptimiseks oma avalik võti.  Me siis sulpsti mis HTTPS sõnumisse, mis saadetakse üle krüptitud kanalit kasutades Microsofti SSL-i certs, et teie teenuse siini relay.  Pärast see sõnum saabub teenuse siini sisse oma kohapeal agendi ärkab autendib teenuse siini olid varem loonud keeruka parooli abil, jätkab krüptitud sõnumi, dekrüptitakse see abil me loodud privaatvõti, ja seejärel katsete seada parooli AD DS SetPassword API kaudu.  See etapp on, mis võimaldab Jõusta oma AD kohapeal paroolipoliitika (keerukuse, vanus, ajalugu, filtrid jne) pilveteenuses.
- **Sõnumi aegumise poliitika kohta** – lõpuks, kui mingil põhjusel sõnumi asub teenuse siini kuna teie kohapeal ei tööta, see aegus ja eemaldada mõne minuti pärast suurendamiseks veelgi turvalisus.

## <a name="how-does-the-password-reset-portal-work"></a>Kuidas parooli lähtestamise töö portaali?
Kui kasutaja viib parooli lähtestamise portaali, töövoog käivitati on kas kasutaja konto on lubatud, mis organisatsiooni kasutajad kuulub, hallatakse teise kasutaja parooli ja kas kasutajale litsentsitud funktsiooni kasutamise kohta.  Lugeda Lisateavet parooli lähtestamise lehele loogika alltoodud juhiseid.

1.  Kasutaja klõpsab ning te ei pääse juurde teie konto lingile või läheb otse [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Kasutaja sisestab kasutaja id ja edastab captcha.
3.  Azure AD kinnitab, kui kasutaja on võimalik, et seda funktsiooni kasutada, tehes järgmist:
    - Kontrollib, kas kasutaja on see funktsioon on sisse lülitatud ja Azure AD litsents määratud.
        - Kui kasutajal pole see funktsioon on sisse lülitatud või määratud litsentsi, palutakse kasutajal oma parooli lähtestamiseks oma administraatori poole.
    - Kontrollib, et kasutajal on õige väljakutse määratletud administraator poliitika vastavalt oma konto andmeid.
        - Kui poliitika nõuab ainult üks probleem, siis on tagada, et kasutajal on vähemalt ühe administraatori poliitika lubatud probleemide korral andmete.
          - Kui kasutaja on konfigureeritud, siis kasutaja soovitatav oma parooli lähtestamiseks oma administraatori poole.
        - Kui poliitika nõuab kahte väljakutsed, siis on tagada, et kasutajal on vähemalt kaks administraator poliitika lubatud probleemide jaoks määratletud vastav andmed.
          - Kui kasutajal pole konfigureeritud, siis me kasutaja on soovitatav oma parooli lähtestamiseks oma administraatori poole.
    - Kontrollib, kas kasutaja parooli hallatakse kohapealne (ühendatud või parooli räsi sünkroonimise oli).
       - Kui tagasikirjutusega juurutatakse ja kohapealne hallatakse kasutaja parooli, siis kasutaja lubatud autentimiseks ja oma parooli lähtestada.
       - Kui tagasikirjutusega on juurutatud ja kohapealne hallatakse kasutaja parooli, siis kasutaja palutakse oma parooli lähtestamiseks oma administraatori poole.
4.  Kui tehakse kindlaks, et kasutaja on edukalt lähtestada oma parooli, siis kasutaja on juhendamisega reset protsessi kaudu.

Lisateavet parooli tagasikirjutusega veebisaidil juurutamise [Alustamine: Azure'i AD parooli halduse](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Milliseid andmeid kasutatakse parooli lähtestamine?
Järgmine tabel liigendusi kus ja kuidas neid andmeid kasutatakse ajal parooli lähtestamine ja aitab teil otsustada, kas teie ettevõtte jaoks sobivad suvandid, mida autentimist. Selles tabelis mis tahes vormindamise nõuded juhul, kui esitate andmete Sisestuskeel teed, kus Valideeri andmed kasutajate nimel.

> [AZURE.NOTE] Töötelefoninumber ei kuvata registreerimise portaalis kuna kasutajad pole praegu redigeerida selle atribuudi kataloogis.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Meetodi nimi</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory andmeväli</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Kasutatud / kõigest kus?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Vormingu nõuded</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Töötelefoninumber</p>
            </td>
            <td>
              <p>Telefoninumber</p>
              <p>nt Set-MsolUser - UserPrincipalName JWarner@contoso.com - telefoni number "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Kasutada:</p>
              <p>Parooli lähtestamine portaal</p>
              <p>Kõigest kaudu:</p>
              <p>Telefoni number on kõigest PowerShelli, DirSync, Azure'i haldusportaal ja Office'i halduskeskuse kaudu</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (nt 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Peab võimaldama riigi kood<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Peab võimaldama suunakoodi (vajadusel)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Andma on a + ette riigi kood<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Peab olema riigi kood ja ülejäänud arvu vahel tühik<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Laiendid pole toetatud, kui teil on mis tahes määratud laiendid, me riba see on enne kõne edasisaatmine.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobiiltelefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>VÕI</p>
              <p>Mobiiltelefon</p>
              <p>(Telefoni kasutatakse siis, kui andmed on autentimine, muidu see langeb tagasi väli mobiiltelefon).</p>
              <p>nt Set-MsolUser - UserPrincipalName JWarner@contoso.com - mobiiltelefon "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Kasutada:</p>
              <p>Parooli lähtestamine portaal</p>
              <p>Registreerimise portaal</p>
              <p>Kõigest kaudu: </p>
              <p>AuthenticationPhone on kõigest parooli lähtestamine registreerimise portaali või MFA registreerimise portaalis.</p>
              <p>Mobiiltelefon on kõigest PowerShelli, DirSync, Azure'i haldusportaal ja Office'i halduskeskuse kaudu</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (nt 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Peab võimaldama riigi kood.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Peate sisestama suunakoodi (vajaduse korral).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Peab olema võimaldama on + ette riigi kood.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Peab olema riigi kood ja ülejäänud arvu vahel tühik.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Laiendid pole toetatud, kui teil on mis tahes määratud laiendid, saame eirake seda, kui telefonikõne edasisaatmine.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternatiivne meiliaadress</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>VÕI</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Kasutatakse e-posti, kui andmed on autentimine, muidu see kuulub uuesti väljale alternatiivne meiliaadress).</p>
              <p>Märkus: väljale alternatiivne meiliaadress on määratud kataloogi stringid massiivi.  Kasutame esimese kirje selles massiivis.</p>
              <p>nt Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Kasutada:</p>
              <p>Parooli lähtestamine portaal</p>
              <p>Registreerimise portaal</p>
              <p>Kõigest kaudu: </p>
              <p>AuthenticationEmail on kõigest parooli lähtestamine registreerimise portaali või MFA registreerimise portaalis.</p>
              <p>AlternateEmail on kõigest PowerShelli, Azure'i haldusportaal ja Office'i halduskeskuse kaudu</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>või甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-kirju peaks järgige standardne vorming nimega kohta.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Unicode'i e-kirju on toetatud.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Turvalisuse küsimused ja vastused</p>
            </td>
            <td>
              <p>Muuta otse kataloogis pole saadaval.</p>
            </td>
            <td>
              <p>Kasutada:</p>
              <p>Parooli lähtestamine portaal</p>
              <p>Registreerimise portaal </p>
              <p>Kõigest kaudu: </p>
              <p>Ainus võimalus määrata turvaküsimused on kaudu Azure'i haldusportaal.</p>
              <p>Ainus võimalus määrata konkreetse kasutaja turvalisuse küsimustele vastused on registreerimise portaali kaudu.</p>
            </td>
            <td>
              <p>Turvaküsimused on kuni 200 tähemärki ja min 3 märke</p>
              <p>Vastused on max 40 märgi ja min 3 märke</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Kuidas pääseda juurde parooli lähtestamine kasutajate andmed
####<a name="data-settable-via-synchronization"></a>Andmete kõigest kaudu sünkroonimine
Kohapealsete saab sünkroonida järgmised väljad:

* Mobiiltelefon
* Töötelefoninumber

####<a name="data-settable-with-azure-ad-powershell"></a>Andmete kõigest Azure AD PowerShelli abil
Azure'i AD PowerShelli & Graph API juurdepääsetavat järgmised väljad:

* Alternatiivne meiliaadress
* Mobiiltelefon
* Töötelefoninumber
* Telefoni autentimine
* E-posti autentimine

####<a name="data-settable-with-registration-ui-only"></a>Andmete registreerimise UI ainult kõigest
Järgmised väljad on ainult SSPR registreerimise Kasutajaliidese (https://aka.ms/ssprsetup) kaudu.

* Turvalisuse küsimused ja vastused

####<a name="what-happens-when-a-user-registers"></a>Mis juhtub, kui kasutaja registreerib?
Kui kasutaja registreerib, registreerimise leht kuvatakse **alati** Sea järgmised väljad:

* Telefoni autentimine
* E-posti autentimine
* Turvalisuse küsimused ja vastused

Kui olete andnud väärtuse **mobiiltelefon** või **Alternatiivne meiliaadress**, kasutajad saavad kohe kasutada need oma parooli lähtestamiseks isegi juhul, kui nad pole registreeritud teenuse.  Lisaks kasutajate registreerimisel esimest korda, lugege teemat neid väärtusi, ja neid muuta, kui nad soovivad.  Siiski pärast seda, kui nad on edukalt registreerida, need väärtused kuvatakse sunnita väljadel **Autentimise telefoni** ja **E-posti autentimise** vastavalt.

See võib olla kasulik viis suure hulga kasutajate kasutada SSPR lubades siiski kasutajad seda teavet registreerimise käigus kinnitamiseks blokeeringu tühistada.

####<a name="setting-password-reset-data-with-powershell"></a>Sätte parooli lähtestamise andmete PowerShelli abil
Saate määrata järgmised väljad Azure AD PowerShelliga väärtused.

* Alternatiivne meiliaadress
* Mobiiltelefon
* Töötelefoninumber

Alustamiseks esmalt peate [alla laadima](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)ja installima Azure AD PowerShelli mooduli.  Kui teil on installitud, saate järgite konfigureerida iga välja alltoodud juhiseid.

#####<a name="alternate-email"></a>Alternatiivne meiliaadress
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobiiltelefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Töötelefoninumber
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Lugemispaanil parooli lähtestamise andmete PowerShelli abil
Leiate Azure'i AD-PowerShelli järgmiste väljade väärtused.

* Alternatiivne meiliaadress
* Mobiiltelefon
* Töötelefoninumber
* Telefoni autentimine
* E-posti autentimine

Alustamiseks esmalt peate [alla laadima](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)ja installima Azure AD PowerShelli mooduli.  Kui teil on installitud, saate järgite konfigureerida iga välja alltoodud juhiseid.

#####<a name="alternate-email"></a>Alternatiivne meiliaadress
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobiiltelefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Töötelefoninumber
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Telefoni autentimine
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>E-posti autentimine
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Linkide parooli lähtestamiseks dokumentatsioon
Allpool on lingid kõik lehed Azure AD parooli lähtestamist dokumentatsiooni:

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
* [**Kuidas see toimib**](active-directory-passwords-how-it-works.md) - teave kuus erinevate osade teenuste, mida iga ei
* [**Alustamine**](active-directory-passwords-getting-started.md) – saate teada, kuidas võimaldavad kasutajatel lähtestamine ja pilveteenuse või kohapealse parooli muutmine
* [**Kohanda**](active-directory-passwords-customize.md) – saate teada, kuidas kohandada ilme & olemust ning teie asutuse vajadustest teenuse käitumine
* [**Head tavad**](active-directory-passwords-best-practices.md) – saate teada, kuidas kiiresti juurutada ja paroolide ettevõtte tõhus haldamine
* [**Saada ülevaadet**](active-directory-passwords-get-insights.md) - Lisateavet meie integreeritud aruandlusvõimalused
* [**FAQ**](active-directory-passwords-faq.md) – siin on vastused korduma kippuvatele küsimustele
* [**Tõrkeotsing**](active-directory-passwords-troubleshoot.md) – saate teada, kuidas kiiresti teenuse probleemide tõrkeotsing



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
