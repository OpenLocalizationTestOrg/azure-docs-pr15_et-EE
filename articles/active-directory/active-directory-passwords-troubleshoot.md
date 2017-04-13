<properties
    pageTitle="Tõrkeotsing: Azure'i AD parooli haldus | Microsoft Azure'i"
    description="Levinud tõrkeotsingu juhiseid Azure AD parooli haldamiseks lähtestamine, muutmine, tagasikirjutusega registreerimine, ja millist teavet lisada, kui otsite abi."
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
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Tõrkeotsing parooli haldus

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

Kui teil on probleeme parooli haldus, me saame aidata. Kõige rohkem võib tekib probleeme lahendada mõne tõrkeotsingu lihtsate, mis saate lugeda all juurutamise tõrkeotsing:

* [**Kui vajate abi kaasata**](#information-to-include-when-you-need-help)
* [**Probleemid Azure'i haldusportaal parooli halduse konfigureerimine**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Parooli haldamine aruannete Azure'i haldusportaal probleemid**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Probleemid parooli lähtestamiseks registreerimise portaal**](#troubleshoot-the-password-reset-registration-portal)
* [**Probleemid parooli lähtestamiseks portaal**](#troubleshoot-the-password-reset-portal)
* [**Parooli tagasikirjutusega probleemid**](#troubleshoot-password-writeback)
  - [Parooli tagasikirjutusega sündmuste logi kuvatavad tõrkekoodid](#password-writeback-event-log-error-codes)
  - [Probleemid parooli tagasikirjutusega Ühenduvus](#troubleshoot-password-writeback-connectivity)

Kui olete proovinud tõrkeotsingu juhiste ja on endiselt probleeme, saate Postitage küsimus [Azure AD Foorumid](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) või pöörduge klienditoe poole ja võtame pilk teie probleemi, niipea, kui võimalik.

## <a name="information-to-include-when-you-need-help"></a>Kui vajate abi kaasata

Kui te ei lahenda teie probleemi alltoodud juhiseid, võite pöörduda meie toe töötajad. Kui teil ühendust võtta, on soovitatav lisada järgmine teave:

 - **Üldise kirjelduse viga** – mis täpne tõrketeade kas kasutaja vt?  Kui tõrketeadet ei kuvata, on kirjeldatud käitu, märkasite üksikasjad.
 - **Lehe** -lehe, mis olid teil kui teile kuvati see tõrge (lisada URL-i)?
 - **Kuupäev / kellaaeg ja ajavöönd** – mis on täpsed kuupäeva ja kellaaja, mis teile kuvati see tõrge (sisaldada soovitud ajavöönd)?
 - **Koodi tugi** – milline oli tugi kood luuakse, kui kasutaja kuvati (leida see, viga, siis ekraani allosas linki toetavad koodi ja saata tugiteeninduse, on tulemuseks GUID) tõrge.
   - Kui olete lehel ilma tugi koodi allosas, vajutage klahvi F12 ja SID ja CID otsimine ja nende kahe tulemuste saatmiseks tugiteeninduse.

    ![][001]

 - **Kasutaja ID -d** – mis oli kuvati see tõrge (nt kasutaja ID-duser@contoso.com)?
 - **Teavet kasutaja** – on ühendatud kasutaja, parooli räsi sünkrooninud, üksnes pilveteenuses?  Kas kasutaja on määratud AAD Premium või AAD lihtsa litsentsi?
 - **Rakenduse sündmuselogi** – kui kasutate parooli tagasikirjutusega ja viga on teie asutusesisese taristu, palun zip koopia rakenduse sündmuselogi serverist Azure'i AD-ühenduse häälestamine ja saatke kutse koos.

Need andmed aitab teie probleemi lahendamiseks nii kiiresti kui võimalik.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Parooli lähtestamine konfiguratsiooni Azure'i haldusportaal tõrkeotsing
Kui teil tekib tõrge konfigureerimine parooli lähtestada, võib olla lahendada tõrkeotsingu järgmist:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Tõrge juhul</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mida viga kasutaja vaadata?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lahendus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ma ei näe <strong>Kasutaja parooli lähtestamine poliitika </strong>jaotist Azure'i haldusportaal vahekaardil <strong>konfigureerimine</strong></p>
            </td>
            <td>
              <p>Jaotise <strong>Kasutaja parooli lähtestamine poliitika </strong>ei ole nähtav, Azure'i haldusportaal menüü <strong>konfigureerimine</strong> .</p>
            </td>
            <td>
              <p>See võib juhtuda siis, kui teil on AAD Premium või AAD lihtsa litsentsi, määratud admin seda toimingut. </p>
              <p>Selle parandamiseks, <strong>litsentside</strong> menüü navigeerides kõnealuse administraatori konto AAD Premium või AAD lihtsa litsentsi määramine ja proovige uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ma ei näe <strong>Kasutaja parooli lähtestamine poliitika</strong> jaotises konfigureerimine suvandeid, mida on kirjeldatud dokumentatsiooni.</p>
            </td>
            <td>
              <p>Jaotise <strong>Kasutaja parooli lähtestamine poliitika </strong>on nähtav, kuid ainult lipu, mis kuvatakse selle all <strong>Kasutajatele lubada parooli lähtestamise</strong> lipp.</p>
            </td>
            <td>
              <p>Ülejäänud UI kuvatakse siis, kui vahetate <strong>Kasutajatele lubada parooli lähtestamise</strong> lipp <strong>Jah.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ma ei näe kindla konfigureerimist.</p>
            </td>
            <td>
              <p>Ma ei näe näiteks suvandi <strong>enne, kui kasutaja peab kinnitage oma kontaktandmed päevade arv,</strong> kui ma sirvige jaotises <strong>Kasutaja parooli lähtestamine poliitika</strong> (või teisi näiteid sama probleemi).</p>
            </td>
            <td>
              <p>Mitme Kasutajaliidese elemendid on peidetud, kuni ta on vaja. Proovige lubada kõigi suvandite lehel, kui soovite näha.</p>
              <p>Kõiki kontrolle, mis on saadaval kohta lisateabe saamiseks vt <a href="active-directory-passwords-customize.md#password-management-behavior">parooli juhtimise käitumist</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ma ei näe <strong>Kirjutamine tagasi paroolide kohapealse</strong> konfiguratsiooni valik</p>
            </td>
            <td>
              <p><strong>Kirjutage tagasi paroolide kohapealse</strong> suvand ei ole nähtav, Azure'i haldusportaal vahekaardil <strong>konfigureerimine</strong></p>
            </td>
            <td>
              <p>See suvand on nähtav ainult siis, kui teil on alla laaditud Azure'i AD-ühenduse ja parooli tagasikirjutusega konfigureeritud. Kui see on tehtud, seda suvandit kuvatakse ja võimaldab teil lubada või keelata tagasikirjutusega pilveteenuses.</p>
              <p>Lisateavet selle kohta teemast <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Parooli tagasikirjutusega lubamiseks klõpsake Azure'i AD-ühenduse</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Parooli haldamine aruannete Azure'i haldusportaal tõrkeotsing
Kui teil tekib tõrge parooli halduse aruannete kasutamine, võib olla lahendada tõrkeotsingu järgmist:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Tõrge juhul</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mida viga kasutaja vaadata?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lahendus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ma ei näe aruannetega parooli haldus</p>
            </td>
            <td>
              <p><strong>Parooli lähtestamise tegevuste</strong> ja <strong>parooli lähtestamise registreerimise tegevuse</strong> aruandeid ei ole nähtav, jaotises <strong>Tegevuse Log</strong> aruannete vahekaart <strong>aruanded</strong> .</p>
            </td>
            <td>
              <p>See võib juhtuda siis, kui teil on AAD Premium või AAD lihtsa litsentsi, määratud admin seda toimingut. </p>
              <p>Selle parandamiseks, <strong>litsentside</strong> menüü navigeerides kõnealuse administraatori konto AAD Premium või AAD lihtsa litsentsi määramine ja proovige uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kasutaja registreerimine kuvada mitu korda</p>
            </td>
            <td>
              <p>Kui kasutaja registreerib alternatiivne meiliaadress, mobiiltelefoni ja turvaküsimused, iga kuvatakse tema nime asemel ühe rea asemel eraldi ridadele.</p>
            </td>
            <td>
              <p>Praegu, kui kasutaja registreerib, me ei saa endale, et need kõik Esita registreerimise lehel registreerida. Selle tulemusena me praegu logige iga üksiku tekstilõigu andmeid, mis on registreeritud eraldi sündmus.</p>
              <p>Kui soovite selle andmeid, saate alla laadida aruanne ja avage andmed PivotTable-liigendtabeli rakenduses excel on rohkem paindlikkust.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Parooli lähtestamine registreerimise portaali tõrkeotsing
Kui teil ilmneb tõrge, kui kasutaja parooli lähtestamiseks registreerimine, võib olla lahendada tõrkeotsingu järgmist:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Tõrge juhul</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mida viga kasutaja vaadata?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lahendus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kataloogi pole lubatud parooli lähtestamine</p>
            </td>
            <td>
              <p>Teie administraator on selle funktsiooni kasutamiseks peab teil pole lubatud.</p>
            </td>
            <td>
              <p>Aktiveerige <strong>Kasutajatele lubada parooli lähtestamise</strong> lipp <strong>Jah</strong> ja tabas Azure'i haldusportaal directory konfiguratsiooni vahekaardil <strong>salvestamine</strong> . Teil peab on Azure AD Premium või tavaline seda toimingut admin määratud litsentsi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kasutaja ei saa määratud AAD Premium või AAD lihtsa litsentsi</p>
            </td>
            <td>
              <p>Teie administraator on selle funktsiooni kasutamiseks peab teil pole lubatud.</p>
            </td>
            <td>
              <p>Azure'i AD Premium või Azure AD lihtsa litsentsi määramine kasutajale <strong>litsentsi</strong> menüü Azure'i haldusportaal. Teil peab on Azure AD Premium või tavaline seda toimingut admin määratud litsentsi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Tõrge töötlemise taotlus</p>
            </td>
            <td>
              <p>Kasutaja näeb viga, mis kinnitab:</p>
              <p>

              </p>
              <p>Tõrge töötlemise taotlus </p>
              <p>Kui proovite parooli lähtestamine.</p>
            </td>
            <td>
              <p>Selle põhjuseks võib olla palju probleeme, kuid üldiselt selle tõrke põhjuseks mõlemal on teenus katkestuste või konfiguratsiooni probleemi ei saa lahendada. </p>
              <p>Kui näete selle tõrke ja see on teie ettevõtte mõjutavad, võtke ühendust toega ja aitame teil ASAP. Täpsemat <a href="#information-to-include-when-you-need-help">teavet lisada, kui vajate abi</a> kuvamiseks, mis peaks andma tugiteeninduse kiiresti abi.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Parooli lähtestamine portaali tõrkeotsing
Kui teil ilmneb tõrge, kui kasutaja parooli lähtestamine, võib olla lahendada tõrkeotsingu järgmist:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Tõrge juhul</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mida viga kasutaja vaadata?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lahendus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kataloogi pole lubatud parooli lähtestamine</p>
            </td>
            <td>
              <p>Teie konto pole lubatud parooli lähtestamine</p>
              <p>Palume vabandust, kuid teie administraator pole häälestanud kasutamiseks koos selle teenuse konto. </p>
              <p>

              </p>
              <p>Kui soovite, me võite pöörduda oma ettevõtte administraator teie parool lähtestada.</p>
            </td>
            <td>
              <p>Aktiveerige <strong>Kasutajatele lubada parooli lähtestamise</strong> lipp <strong>Jah</strong> ja tabas Azure'i haldusportaal directory konfiguratsiooni vahekaardil <strong>salvestamine</strong> . Teil peab on Azure AD Premium või tavaline seda toimingut admin määratud litsentsi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kasutaja ei saa määratud AAD Premium või AAD lihtsa litsentsi</p>
            </td>
            <td>
              <p>Kuigi me ei saa automaatselt-administraatori konto paroole lähtestada, saame võtke ühendust teie ettevõtte administraator seda teha.</p>
            </td>
            <td>
              <p>Azure'i AD Premium või Azure AD lihtsa litsentsi määramine kasutajale <strong>litsentsi</strong> menüü Azure'i haldusportaal. Teil peab on Azure AD Premium või tavaline seda toimingut admin määratud litsentsi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kataloogi on lubatud parooli lähtestamiseks, kuid kasutajal on puudu või mal loodud autentimise teavet</p>
            </td>
            <td>
              <p>Teie konto pole lubatud parooli lähtestamine</p>
              <p>Palume vabandust, kuid teie administraator pole häälestanud kasutamiseks koos selle teenuse konto. </p>
              <p>

              </p>
              <p>Kui soovite, me võtke ühendust oma asutuse administraator teie parool lähtestada.</p>
            </td>
            <td>
              <p>Veenduge, et see kasutaja on õige kujuga kontaktandmed faili kataloogi enne jätkamist. Konfigureerimise autentimise teavet kataloogis nii, et kasutajad ei näe selle tõrke kohta lisateabe saamiseks vt <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">andmete kasutab parooli lähtestamine</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kataloogi on lubatud parooli lähtestamiseks, kuid kasutaja on ainult üks osa kontaktandmed faili, kui poliitika on seatud kaks kontrollimise juhiseid nõudma</p>
            </td>
            <td>
              <p>Teie konto pole lubatud parooli lähtestamine</p>
              <p>Palume vabandust, kuid teie administraator pole häälestanud kasutamiseks koos selle teenuse konto. </p>
              <p>

              </p>
              <p>Kui soovite, me võtke ühendust oma asutuse administraator teie parool lähtestada.</p>
            </td>
            <td>
              <p>Veenduge, et kasutajal on vähemalt kaks õigesti konfigureeritud kontaktandmete (nt mobiiltelefon ja Töötelefon) enne jätkamist. Konfigureerimise autentimise teavet kataloogis nii, et kasutajad ei näe selle tõrke kohta lisateabe saamiseks vt <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">andmete kasutab parooli lähtestamine</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kataloogi on lubatud parooli lähtestamiseks, ja kasutajale on õigesti konfigureeritud, kuid kasutaja ei saa ühendust võtta </p>
            </td>
            <td>
              <p>Registreerumine  Ilmnes ootamatu tõrge ilmnes teiega.</p>
            </td>
            <td>
              <p>See võib olla ajutine teenusetõrge või valesti konfigureeritud kontaktandmed, mida me ei tuvastanud õigesti. Kui kasutaja ootab 10 sekundi, proovige uuesti ja kuvatakse link "võtke ühendust oma administraatoriga". Klõpsake nuppu Proovi uuesti uuesti saata kõne, klõpsates "võtke ühendust oma administraatoriga" kas saatma vormi e-posti kasutaja, parool või üldadministraatorid (selles tähtsuse järjekorras) paluda parooli lähtestamiseks loodud kasutajakonto teha.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kunagi saab kasutaja parooli lähtestamine SMS või telefonikõne ajal</p>
            </td>
            <td>
              <p>Kasutaja klõpsab käsku "tekst mina" või "Helista mulle" ja siis kunagi saab midagi.</p>
            </td>
            <td>
              <p>See võib olla kataloogis mal loodud telefoninumber. Veenduge, et telefoninumber on vormingus "+ ccc xxxyyyzzzzXeeee". Lisateavet vormingu telefoni arvude jaoks parooli lähtestamine vaadata, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">milliseid andmeid kasutatakse parooli lähtestamine</a>.</p>
              <p>Kui vajate laiendit marsruutida kõnealuse kasutaja, Pange tähele selle parooli lähtestamist ei toeta laiendid, isegi siis, kui määrate ühe kataloogis (nad on tühjendanud enne kõne saadetakse). Proovige kasutada ilma laiendita, arvu või laiendamine integreerimine väljale oma PBX.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kasutaja saab kunagi e-posti parooli lähtestamine</p>
            </td>
            <td>
              <p>Kasutaja klõpsab "e-posti mind" ja seejärel kunagi saab midagi.</p>
            </td>
            <td>
              <p>Probleemi lahendamiseks kõige levinum põhjus on, et sõnum on tagasi lükatud rämpspostifiltrite abil. Kontrollige oma rämpsposti, e-posti kausta Rämpspost või kustutatud üksused.</p>
              <p>Lisaks veenduge, et olete end õige e-posti sõnumi... paljudele inimestele on väga sarnased e-posti aadressid ja lõpuks kontrollimine sõnumi valesti sisendkausta. Kui need suvandid ei aita, on võimalik, et kataloogi meiliaadress on vigane, veenduge, et meiliaadress oleks õige ja proovige uuesti kontrollida. Lisateavet vormingu e-posti meiliaadresside jaoks parooli lähtestamine näha, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">millised andmed on kasutatavad parooli lähtestamine</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Olen parooli lähtestamine poliitika seadnud, kuid kuna administraatorikonto parooli lähtestamine, pole rakendatud poliitika</p>
            </td>
            <td>
              <p>Oma parooli lähtestamise administraatorikontodele teemast parooli lähtestamine, e-posti ja mobiiltelefoni, olenemata sellest, millist poliitika on seatud <strong>konfigureerimine</strong> menüü jaotises <strong>Kasutaja parooli lähtestamine poliitika</strong> lubatud samad suvandid.</p>
            </td>
            <td>
              <p>Konfigureeritud <strong>Kasutaja parooli lähtestamine poliitika</strong> jaotises <strong>konfigureerimine</strong> menüü Suvandid kehtivad ainult teie ettevõtte lõppkasutajatele.</p>
              <p>Haldab Microsoft ja juhtelemendid administraatori parooli lähtestamine poliitika turvalisuse kõrgeima taseme tagamiseks</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kasutaja takistada proovite parooli lähtestamise liiga palju kordi sisse päeva</p>
            </td>
            <td>
              <p>Kasutaja näeb veateate selle kohta:</p>
              <p>

              </p>
              <p>Kasutage mõni muu suvand.</p>
              <p>Olete proovinud liiga palju kordi viimase 1 tund (e) sisse oma konto kinnitada. Turvalisuse põhjustel peate oodake 24 tund (e) enne proovige uuesti. </p>
              <p>Kui soovite, me võtke ühendust oma asutuse administraator teie parool lähtestada.</p>
            </td>
            <td>
              <p>Me rakendada mõne automaatse ahendamise süsteem Blokeeri kasutajatele katsel paroolide lähtestamiseks liiga palju kordi sisse lühikese aja jooksul. See juhtub, kui:</p>
              <ol class="ordered">
                <li>
Kasutaja proovib valideerimiseks telefoninumbri 5 korda üks tund.<br\><br\></li>
                <li>
Kasutaja proovib kasutada turvalisuse küsimused värav 5 korda üks tund.<br\><br\></li>
                <li>
Kasutaja proovib sama kasutajakonto parooli lähtestamine 5 korda sisse üks tund.<br\><br\></li>
              </ol>
              <p>Selle probleemi lahendamiseks paluge kasutajal oodake 24 tundi pärast viimast proovi ja kasutajale siis saaksid oma parooli lähtestada.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kasutaja näeb tõrke valideerimisel tema telefoninumber</p>
            </td>
            <td>
              <p>Kui proovite kinnitamine autentimise meetod kasutada telefoni, näeb kasutaja veateate selle kohta:</p>
              <p>

              </p>
              <p>Määratud vale telefoninumber.</p>
            </td>
            <td>
              <p>See tõrge ilmneb juhul, kui sisestatud telefoninumber ei sobi faili telefoninumbrit.</p>
              <p>Veenduge, et kasutajal on sisestamise täielik telefoninumber, sh ja riigi kood, kui proovite parooli lähtestamiseks telefoni teel meetodi kasutamise kohta.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Tõrge töötlemise taotlus</p>
            </td>
            <td>
              <p>Kasutaja näeb viga, mis kinnitab:</p>
              <p>

              </p>
              <p>Tõrge töötlemise taotlus </p>
              <p>Kui proovite parooli lähtestamine.</p>
            </td>
            <td>
              <p>Selle põhjuseks võib olla palju probleeme, kuid üldiselt selle tõrke põhjuseks mõlemal on teenus katkestuste või konfiguratsiooni probleemi ei saa lahendada. </p>
              <p>Kui näete selle tõrke ja see on teie ettevõtte mõjutavad, võtke ühendust toega ja aitame teil ASAP. Täpsemat <a href="#information-to-include-when-you-need-help">teavet lisada, kui vajate abi</a> kuvamiseks, mis peaks andma tugiteeninduse kiiresti abi.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Parooli tagasikirjutusega tõrkeotsing
Kui teil ilmneb tõrge, kui lubamine, keelamine või tagasikirjutusega parooli abil, võib olla lahendada tõrkeotsingu järgmist:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Tõrge juhul</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mida viga kasutaja vaadata?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lahendus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Üldine kasutuselevõtt ja käivitus tõrked</p>
            </td>
            <td>
              <p>Parooli lähtestamine teenus ei käivitu kohapealne tõrkega 6800 Azure'i AD-ühenduse arvuti rakenduse sündmuste logi sisse.</p>
              <p>

              </p>
              <p>Pärast kasutuselevõtt, ühendatud või parooli räsi ei saa sünkroonitud kasutajate paroolide lähtestamiseks.</p>
            </td>
            <td>
              <p>Kui parooli tagasikirjutusega on lubatud, helistage sync engine tagasikirjutusega teegi konfigureerimise (kasutuselevõtt) pilve kasutuselevõtu teel. Ilmnes ajal kasutuselevõtt või käivitamisel WCF-i lõpp-punkti jaoks parooli tagasikirjutusega vigu tulemuseks tõrgete sündmuste logi Azure'i AD-ühenduse seadme sündmuste logi sisse.</p>
              <p>Kui tagasikirjutusega on konfigureeritud, ajal uuesti ADSync teenuse, alustatakse WCF-i lõpp-punkti. Juhul, kui lõpp-punkti käivitamine nurjus, me lihtsalt Logi sündmus 6800 ja lasta sünkroonimise teenuse käivitamine. Sündmuse olemasolu tähendab, et parooli tagasikirjutusega, lõpp-punkti ei alustatud üles. Sündmuste logi üksikasjad selle sündmuse (6800) koos sündmuselogi kirjeid luua PasswordResetService osa näitab, miks lõpp-punkti ei saa käivitada. Vaadake üle nende vigade sündmuste logi ja proovige uuesti alustamiseks Azure AD ühenduse kui parooli tagasikirjutusega ikka ei tööta. Kui probleem ei lahene, proovige välja lülitada ja parooli tagasikirjutusega uuesti sisse.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Kui kasutaja üritab parooli lähtestamine või avada konto parooli tagasikirjutusega PivotTable-liigendtabelid lubatud, toiming nurjub. Lisaks kuvatakse sündmuse Azure'i AD-ühenduse sündmuste logi sisaldavad: "sünkroonimise tagastatakse veaväärtus hr = 800700CE, sõnumi = faili nimi või laiend on liiga pikk" Ava toimingu ilmnemisel.
                            </p>
            </td>
            <td>
              <p>See võib juhtuda siis, kui läksite oli vanemate versioonide Azure'i AD-ühenduse või DirSync. Täiendamine versiooniks varasemate versioonide Azure'i AD-ühenduse parooli 254 Märgi konto Azure AD haldamise Agent (uuemad versioonid määratud 127 märgi pikkuse parool). Sellise pikk paroolide töötavad AD Connector impordi ja ekspordi toimingute jaoks, kuid nad ei toeta avamiseks selle toimingu.
                            </p>
            </td>
            <td>
              <p>[Otsige Active Directory kontot](active-directory-aadconnect-accounts-permissions.md#active-directory-account) Azure'i AD-ühenduse ja Lähtesta parool sisaldab rohkem kui 127 märki. Avage menüü Start kaudu **Sünkroonimise teenuse** . Liikuge **konnektorid** ja otsige **Active Directory konnektor**. Valige see ja klõpsake käsku **Atribuudid**. Liikuge lehele **identimisteabe** ja sisestage uus parool. Klõpsake nuppu **OK** lehe sulgemiseks.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Konfigureerimise tagasikirjutusega Azure'i AD-ühenduse installimisel ilmnes tõrge.</p>
            </td>
            <td>
              <p>Veebisaidil viimases etapis installitoimingut Azure'i AD-ühenduse, kuvatakse tõrge, mis näitab, et tagasikirjutusega parooli ei saanud konfigureerida.</p>
              <p>

              </p>
              <p>Azure'i AD-ühenduse rakenduse sündmuselogi sisaldab viga 32009 tekstiga "Tõrge saada auth Turbeloa".</p>
            </td>
            <td>
              <p>See tõrge ilmneb kaks järgmistel juhtudel:</p>
              <ul>
                <li class="unordered">
Olete määranud vale installitoimingut Azure'i AD-ühenduse alguses määratud üldadministraator konto parooli.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Proovisite kasutada välise kasutaja määratud installitoimingut Azure'i AD-ühenduse alguses üldadministraator konto.<br\><br\></li>
              </ul>
              <p>Selle vea parandamiseks veenduge, et te ei kasuta ühendatud konto jaoks määratud installitoimingut Azure'i AD-ühenduse alguses globaalne administraator ja et määratud parool on õiged.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Konfigureerimise tagasikirjutusega Azure'i AD-ühenduse installimisel ilmnes tõrge.</p>
            </td>
            <td>
              <p>Azure'i AD-ühenduse arvuti sündmuste logi sisaldab viga 32002 visanud selle PasswordResetService.</p>
              <p>

              </p>
              <p>Kuvatakse tõrketeade: "tõrge ühenduse ServiceBus, Turbeloa pakkuja ei saa anda Turbeloa..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>Selle tõrke põhjuseks on, et parooli lähtestamine teenus, mis töötab teie asutusesiseses keskkonnas ei saa ühenduse teenuse siini lõpp-punkti pilveteenuses. Selle tõrke põhjuseks tavaliselt tavaliselt tulemüüri reegli blokeerivad jaoks väljamineva ühenduse kindla pordi või veebiaadress.</p>
              <p>

              </p>
              <p>Veenduge, et teie tulemüür lubab Väljaminevad ühendused järgmist:</p>
              <ul>
                <li class="unordered">
Kogu liikluse üle TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Väljaminevad ühendused <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Pärast värskendamist järgmisi reegleid, Azure'i AD-ühenduse arvuti taaskäivitama ja parooli tagasikirjutusega tuleks alustada tööd uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Parooli tagasikirjutusega lõpp-punkti kohapeal pole kättesaadav</p>
            </td>
            <td>
              <p>Pärast teatud aja, ühendatud või parooli räsi tööpäeva ei saa sünkroonitud kasutajate paroolide lähtestamiseks.</p>
            </td>
            <td>
              <p>Mõned harva, ei pruugi parooli tagasikirjutusega teenuse uuesti käivitada, kui Azure'i AD-ühenduse uuesti käivitas. Sellisel juhul esmalt kontrollida, kas parooli tagasikirjutusega näib lubatud prem. Seda saab teha Azure'i AD-ühenduse viisardi või PowerShelli (vt HowTos ülaltoodud) abil. Kui see funktsioon lubatud, proovige lubamine või keelamine funktsiooni uuesti Kasutajaliidese või PowerShelli kaudu. Vt "samm 2: luba parooli tagasikirjutusega kataloogi sünkroonimine arvuti &amp; Konfigureerige tulemüür reeglid" Lisateavet selle kohta, <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">Kuidas lubada või keelata parooli tagasikirjutusega</a> .</p>
              <p>

              </p>
              <p>Kui see ei toimi, proovige desinstallida ja uuesti installida Azure AD-ühenduse.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Õiguste tõrked</p>
            </td>
            <td>
              <p>Ühendatud või parooli räsi sünkroonimise 'd lähtestada oma parooli vt tõrke olete parooli, mis näitab ilmnes probleem teenuse kasutajatele.</p>
              <p>

              </p>
              <p>Lisaks sellele parooli lähtestamine toimingute käigus, võidakse kuvada tõrke kohta haldamise agent puudub juurdepääs sees tööruumide sündmuselogide juurdepääsu.</p>
            </td>
            <td>
              <p>Kui näete oma sündmuste logi nende vigade, veenduge, et AD MA konto (mis on määratud viisardi konfiguratsiooni ajal) on vajalikud õigused jaoks parooli tagasikirjutusega.</p>
              <p>

              </p>
              <p>Pange tähele, et kui see õigus antakse võib kuluda kuni 1 tund selleks, et käputäis alla kaudu näiteks Põhiliselt sdprop tausta tööülesande õigused. </p>
              <p>Parooli lähtestamine töötada, peab luba kasutaja objekti, mille parooli lähtestatakse turvakirjelduse tempel. Seni, kuni see õigus kuvatakse kasutajaobjektis, paroolide lähtestamine jätkab nurjuda juurdepääs keelatud.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Tõrge parooli tagasikirjutusega konfigureerimisel konfiguratsiooniviisardi Azure'i AD-ühenduse kaudu </p>
            </td>
            <td>
              <p>"Otsimine MA ei saa" viga viisardis parooli tagasikirjutusega lubamine ja keelamine</p>
            </td>
            <td>
              <p>Azure'i AD-ühenduse mis manifestid järgmises olukorras väljastatud versioon on teadaolev viga:</p>
              <ol class="ordered">
                <li>
Saate konfigureerida Azure'i AD-ühenduse rentniku abc.com (kinnitatud Domeen) abil creds jaoks. Selle tulemuseks AAD konnektor, mille nimi "abc.com – AAD" luuakse.<br\><br\></li>
                <li>
Klõpsake seejärel muudate AAD creds konnektor (syncis vana UI) (Pange tähele, on sama rentniku, kuid muu domeeni nimi). <br\><br\></li>
                <li>
Nüüd, kui proovite lubada või keelata parooli tagasikirjutusega. Viisard on ehitada creds, kasutades "abc.onmicrosoft.com – AAD" konnektor nime ja edastama parooli tagasikirjutusega cmdlet-käsk. Nurjub, kuna pole loodud selle nimega konnektor.<br\><br\></li>
              </ol>
              <p>See on fikseeritud meie uusima järgud. Kui teil on mõne vanema koostamine, üks lahendus on funktsiooni lubada või keelata PowerShelli cmdleti abil. Vt "samm 2: luba parooli tagasikirjutusega kataloogi sünkroonimine arvuti &amp; Konfigureerige tulemüür reeglid" Lisateavet selle kohta, <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">Kuidas lubada või keelata parooli tagasikirjutusega</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ei saa rühmadele, nagu näiteks Domain Admins kasutajate parooli lähtestamine ja ettevõtte administraatorid jne.</p>
            </td>
            <td>
              <p>Ühendatud või parooli räsi sünkroonimise 'd kasutajad, kes kuuluvad kaitstud rühmad ja lähtestamiseks oma paroolide vt olete parooli, mis näitab ilmnes probleem teenuse tõrge.</p>
            </td>
            <td>
              <p>Active Directory õigustega kasutajad on kaitstud AdminSDHolder abil. Vaadake lisateavet <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> . </p>
              <p>

              </p>
              <p>See tähendab, et nende objektide turvalisus kirjelduste ühe määratletud AdminSDHolder vastavaks regulaarselt kontrollida ja on lähtestatud, kui need on erinevad. Täiendavad õigused, mis on vajalikud parooli tagasikirjutusega seetõttu käputäis sellistele kasutajatele. See võib põhjustada parooli tagasikirjutusega ei tööta selliste kasutajate jaoks. Selle tulemusena me ei toeta haldamise paroolide kasutajate neid rühmades kuna seda mudelit AD piirid.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Lähtesta toimingute nurjub objekti ei leitud</p>
            </td>
            <td>
              <p>Ühendatud või parooli räsi sünkroonimise 'd lähtestada oma parooli vt tõrke olete parooli, mis näitab ilmnes probleem teenuse kasutajatele.</p>
              <p>

              </p>
              <p>Lisaks sellele parooli lähtestamine toimingute käigus, võite näha tõrke oma sündmuselogide Azure'i AD-ühenduse teenuse loetletud tõrkega "Objekti ei leitud".</p>
            </td>
            <td>
              <p>Selle tõrke tavaliselt on märgitud sync engine ei leia AAD konnektori ruumi või MV lingitud objekt või AD konnektori ruumi objekti. </p>
              <p>

              </p>
              <p>Probleemi tõrkeotsingu sooritamiseks, veenduge, et kasutajal on tõepoolest sünkroonitud kohapeal AAD praegust eksemplari Azure'i AD-ühenduse kaudu ja objektide konnektor tühikuid ja MV oleku kontrollimine. Veenduge, et AD CS objekt on konnektor MV objektile reegli "Microsoft.InfromADUserAccountEnabled.xxx" kaudu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Lähtesta toimingute nurjub mitu vastet leitud eror</p>
            </td>
            <td>
              <p>Ühendatud või parooli räsi sünkroonimise 'd lähtestada oma parooli vt tõrke olete parooli, mis näitab ilmnes probleem teenuse kasutajatele.</p>
              <p>

              </p>
              <p>Lisaks sellele parooli lähtestamine toimingute käigus, võite näha tõrke oma sündmuselogide Azure'i AD-ühenduse teenuse, mis näitab, kuvatakse tõrketeade "Mitme maches leitud".</p>
            </td>
            <td>
              <p>See näitab, et sync engine tuvastanud, et MV objekti on ühendatud mitu AD CS objektid "Microsoft.InfromADUserAccountEnabled.xxx" kaudu. See tähendab, et kasutajal on lubatud konto rohkem kui üks mets. </p>
              <p>

              </p>
              <p>Selle stsenaariumi puhul parooli tagasikirjutusega praegu ei toetata.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Parooli toimingud ei konfiguratsiooni tõrke.</p>
            </td>
            <td>
              <p>Parooli toimingud ei konfiguratsiooni tõrke. Rakenduse sündmuselogi sisaldab Azure'i AD-ühenduse tõrge 6329 tekstiga: 0x8023061f (toiming nurjus, kuna parooli sünkroonimine on sisse lülitatud selle haldamise Agent.)</p>
            </td>
            <td>
              <p>See juhtub, kui Azure'i AD-ühenduse konfiguratsiooni on muudetud lisada&nbsp;uue AD mets (või kui soovite eemaldada ega uuesti lisada mõne olemasoleva mets) <strong>pärast</strong> parooli tagasikirjutusega funktsioon on juba lubatud. Äsja lisatud parool toimingute kasutajate näiteks metsade nurjub. Probleemi lahendamiseks keelata ja lubage parooli tagasikirjutusega funktsiooni pärast mets konfiguratsioonimuudatuste on lõpule viidud.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kirjalikult tagasi paroole, mis on lähtestatud, kasutajate töötab õigesti, kuid kirjutamine tagasi paroole muuta kasutaja või lähtestage kasutaja administraator nurjub.</p>
            </td>
            <td>
              <p>Kui proovite nimel Azure'i haldusportaal kasutaja parooli lähtestamine, näete teadet, mis kinnitab: "parooli lähtestamine teenus, mis töötab teie asutusesiseses keskkonnas ei toeta kasutaja parooli lähtestamise administraatorid. Palun võtke kasutusele uusima versiooni Azure'i AD-ühenduse probleemi lahendamiseks."</p>
            </td>
            <td>
              <p>See juhtub siis, kui sünkroonimise mootori versioon ei toeta kindla parooli tagasikirjutusega toiming, mida kasutati. Versioonide Azure'i AD-ühenduse hiljem kui 1.0.0419.0911 toetada kõik parool haldamise toimingud, sh parooli lähtestamine tagasikirjutusega, parooli muutmine tagasikirjutusega ja algatatud administraatori parooli lähtestamine tagasikirjutusega: Azure'i haldusportaal.&nbsp; DirSync versioonid hiljem kui 1.0.6862 toetavad parooli lähtestamine tagasikirjutusega ainult. Selle probleemi lahendamiseks Soovitame installida Azure AD-ühenduse või Azure Active Directory ühenduse uusim versioon (Lisateavet leiate teemast <a href="active-directory-aadconnect">Kataloogiintegreerimise tööriistad</a>) selle probleemi lahendamiseks ja parimal viisil parooli tagasikirjutusega teie asutuses.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Parooli tagasikirjutusega sündmuste logi kuvatavad tõrkekoodid
Kui kontrolli selle rakenduse sündmuste logi arvutis Azure'i AD-ühenduse tõrkeotsingu parooli tagasikirjutusega PivotTable-liigendtabelid on hea tava. See sündmuste logi sisaldab kaks allikatest huvi sündmuste jaoks parooli tagasikirjutusega. PasswordResetService allika kirjeldatakse toiminguid ning parooli tagasikirjutusega toimingu seotud probleemid. ADSync allika kirjeldatakse toiminguid ja paroolide määramise AD keskkonna seotud probleemid.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kood</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Nime / sõnum</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Allikas</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Kirjeldus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>KAUTSJONI: MMS(4924) 0x80230619 – "piirang takistab parool on muutunud määratud olemasoleva."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>See sündmus juhul, kui parool tagasikirjutusega teenuse proovib teie kohalikku kausta, mis ei vasta parooli vanus, ajalugu, keerukuse või filtreerimise nõuded domeeni parooli seada.</p>
              <ul>
                <li class="unordered">
Kui teil on parooli vanus, ja selle aja jooksul parool on viimati muudetud, ei saa muuta parooli uuesti, kuni see jõuab märgitud teie domeeni. Katsetamiseks, peaks vanuse väärtuseks 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kui teil on lubatud parooli ajaloo nõuded ja seejärel valige parool, mida pole kasutatud viimase N korda, kui N on sätte parooli ajalugu. Kui valite selline parool, mida on kasutatud viimase N korda, siis kuvatakse tõrge sel juhul. Katsetamiseks, tuleks ajalugu väärtuseks 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kui teil on parooli keerukuse nõuetele, jõustatakse kõik neist, kui kasutaja proovib muutmine või lähtestamine parooli.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kui teil on lubatud parooli filtrid ja kasutaja valib parool, mis ei vasta kriteeriumidele filtreerimise, siis lähtestamine või muutmine toiming nurjub.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Sünkroonimise tagastatakse veaväärtus hr = 80230402, sõnumi = katse nurjus, kuna seal on sama ankur koos duplikaatkirjete objekti saada</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>See sündmus juhul, kui sama kasutaja ID-d on lubatud mitu domeenides.  Näiteks, kui teil on sünkroonimise konto/ressursi metsade ja on sama esitamine ja lubatud iga kasutaja id, see tõrge võib ilmneda.  </p>
              <p>See tõrge võib ilmneda ka juhul, kui kasutate-kordumatu ankur atribuut (nt pseudonüümi või UPN-i) ja kaks kasutajat ühiskasutusse sama ankur atribuudi.</p>
              <p>Selle probleemi lahendamiseks veenduge, et teil on kõik dubleeritud Kasutajad domeeni sees ja et te kasutate iga kasutaja jaoks kordumatu ankur atribuut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et asutusesisese teenuse tuvastatud parooli lähtestamine kutse on ühendatud või parooli räsi sünkroonimise 'd Pilveteenusest pärinevad kasutaja. Sel juhul on esimene sündmus iga parool lähtestada tagasikirjutusega toiming.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et kasutaja valitud uus parool parooli lähtestamine töötamise ajal me kindlaks, et see parool vastab ettevõtte parool ja parooli on edukalt kirjutatud tagasi kohaliku AD keskkonnas.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et kasutaja valitud parool ja parooli jõudnud edukalt asutusesiseses keskkonnas, et kui me proovitakse seada parooli kohaliku AD keskkonnas, ilmnes tõrge. Sellel võib olla mitu põhjust:</p>
              <ul>
                <li class="unordered">
Kasutaja parooli ei vasta vanus, ajalugu, keerukuse või nõuded domeeni filtreerida. Proovige probleemi lahendamiseks täiesti uus parool.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Teenusekonto MA ei ole kasutaja nimel uue parooli määramiseks vajalikud õigused.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kasutaja konto on kaitstud rühma, nt domeeni või ettevõtte administraatorid, teie parooli määramine toimingud.<br\><br\></li>
              </ul>
              <p>Lugege teemat <a href="#troubleshoot-password-writeback">Tõrkeotsing parooli tagasikirjutusega</a> leiate veel kohta, mis muud situtions võivad põhjustada tõrke.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul ilmneb juhul, kui lubate parooli tagasikirjutusega koos Azure'i AD-ühenduse ja näitab, et saaksime alustamine kasutuselevõtt ettevõttes parooli tagasikirjutusega veebiteenusele.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab kasutuselevõtt viidud ja et parooli tagasikirjutusega võimalus on kasutamiseks valmis.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et asutusesisese teenuse tuvastatud parooli muutmine taotluse on ühendatud või parooli räsi sünkroonimise 'd kasutaja pärinevate pilve. Sel juhul on esimene sündmus iga parooli muutmine tagasikirjutusega toiminguga.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et kasutaja valitud uus parool parool Muuda töötamise ajal me kindlaks, et see parool vastab ettevõtte parool ja parooli on edukalt kirjutatud tagasi kohaliku AD keskkonnas.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et kasutaja valitud parool ja parooli jõudnud edukalt asutusesiseses keskkonnas, et kui me proovitakse seada parooli kohaliku AD keskkonnas, ilmnes tõrge. Sellel võib olla mitu põhjust:</p>
              <ul>
                <li class="unordered">
Kasutaja parooli ei vasta vanus, ajalugu, keerukuse või nõuded domeeni filtreerida. Proovige probleemi lahendamiseks täiesti uus parool.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Teenusekonto MA ei ole kasutaja nimel uue parooli määramiseks vajalikud õigused.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kasutaja konto on kaitstud rühma, nt domeeni või ettevõtte administraatorid, teie parooli määramine toimingud.<br\><br\></li>
              </ul>
              <p>Lugege teemat <a href="#troubleshoot-password-writeback">Tõrkeotsing parooli tagasikirjutusega</a> leiate veel mis muude olukordade kohta, võivad põhjustada tõrke.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Kohapealse teenuse tuvastati parooli lähtestamine kutse on ühendatud või parooli räsi sünkroonimise 'd kasutaja administraator on pärit kasutaja nimel. Sel juhul on esimene sündmus iga algatatud administraatori parooli lähtestamine tagasikirjutusega toiminguga.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Admin valitud algatatud administraatori parooli lähtestamine toimingu käigus uus parool, me kindlaks, et see parool vastab ettevõtte parool ja parooli on edukalt kirjutatud tagasi kohaliku AD keskkonnas.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Admin valitud parool on kasutaja nimel ja kohapealse keskkonna jõudnud edukalt parooli, kuid kui me proovitakse seada parooli kohaliku AD keskkonnas, ilmnes tõrge. Sellel võib olla mitu põhjust:</p>
              <ul>
                <li class="unordered">
Kasutaja parooli ei vasta vanus, ajalugu, keerukuse või nõuded domeeni filtreerida. Proovige probleemi lahendamiseks täiesti uus parool.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Teenusekonto MA ei ole kasutaja nimel uue parooli määramiseks vajalikud õigused.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kasutaja konto on kaitstud rühma, nt domeeni või ettevõtte administraatorid, teie parooli määramine toimingud.<br\><br\></li>
              </ul>
              <p>Lugege teemat <a href="#troubleshoot-password-writeback">Tõrkeotsing parooli tagasikirjutusega</a> leiate veel kohta, mis muud situtions võivad põhjustada tõrke.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul ilmneb juhul, kui keelate parooli tagasikirjutusega koos Azure'i AD-ühenduse ja näitab, et saaksime alustamine offboarding oma asutuse parooli tagasikirjutusega veebiteenus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab offboarding viidud ja parooli tagasikirjutusega võimalus edukalt keelatud.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab offboarding protsessi ei õnnestunud. See võib olla pilve või kohapealse administraatori konto konfigureerimise käigus määratud õiguste tõrke tõttu või sellepärast, mida proovite kasutada ühendatud pilve üldadministraator, kui parool tagasikirjutusega keelamine. Vea parandamiseks märkige oma administraatoriõigusi ja mida te ei kasuta ühendatud konto konfigureerimisel parooli tagasikirjutusega võimalus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et parooli tagasikirjutusega teenuse käivitatud ja on valmis aktsepteerige parooli haldus pilve.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et parooli tagasikirjutusega teenus on peatatud ja et taotlused parooli haldus pilve ei pruugi õnnestuda.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et me edukalt tuua mõne autoriseerimine luba globaalne administraator, täpsustada Azure'i AD-ühenduse häälestamise ajal, et alustada offboarding või kasutuselevõtt.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et saaksime loodud parooli krüptovõtme, mida kasutatakse krüptimiseks paroole, mis saadetakse teie kohapealse keskkonna pilvest.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab tundmatu tõrge parooli halduse töötamise ajal. Vaadake erandi teksti korral üksikasjalikumat teavet. Kui teil on probleeme, proovige uuesti lubamine ja keelamine parooli tagasikirjutusega. Kui see ei aita, lisada koopia oma sündmuste logi koos jälgimise id määratud insider, et teie tugiteeninduse.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab esines tõrke ühendamiseks pilveteenuse parooli lähtestamiseks teenuse ja üldiselt juhul, kui kohapealse teenus ei saanud ühenduse veebiteenuse parooli lähtestamine. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab ilmnes tõrge ühenduse oma rentniku siini eksemplari. See võib juhtuda, kuna te blokeerivad teie asutusesiseses keskkonnas Väljaminevad ühendused. Kontrollige oma tulemüüri tagada teie luba ühendused TCP 443 ja <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>ja proovige uuesti. Kui teil on endiselt probleeme, proovige uuesti lubamine ja keelamine parooli tagasikirjutusega.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab, et edasi meie teenuste Veebiteenuste sisend oli sobimatu. Proovige uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et ilmnes tõrge dekrüptimine parool, mis saabusid pilvest. See võib olla dekrüptimine võtme erinevuse pilveteenusesse ja kohapealse keskkonna tõttu. Selleks, et probleemi lahendamiseks keelata ja lubage parooli tagasikirjutusega teie asutusesiseses keskkonnas.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ajal kasutuselevõtt, saame salvestada rentniku teavet oma kohapealse keskkonna konfiguratsioon faili. See sündmus näitab ilmnes tõrge salvestamine või selle faili, mille käivitamisel teenus on olemas, ilmneb tõrge faili lugemisel. Selle probleemi lahendamiseks proovige uuesti lubamine ja keelamine parooli tagasikirjutusega jõustamine konfiguratsioonifail ümber kirjutada. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AEGUNUD – sel juhul ei esine Azure'i AD-ühenduse, ainult juba varakult koostab, mis toetatud tagasikirjutusega DirSync.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Kasutuselevõtt, saadame, andmete pilvest asutusesisese parooli lähtestamiseks teenus. Enne saatmist sünkroonimine teenusega andmed salvestama turvaliselt kettal andmeid-mälu faili seejärel kirjutada. Sel juhul näitab probleem kirjutamiseks või mälu andmete värskendamine. Selle probleemi lahendamiseks proovige uuesti lubamine ja keelamine parooli tagasikirjutusega jõustamine selle konfiguratsiooni uuesti kirjutada.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab sobimatu vastus saadud veebiteenuse parooli lähtestamine. Selle probleemi lahendamiseks proovige uuesti lubamine ja keelamine parooli tagasikirjutusega.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et me ei saanud luba Turbeloa üldadministraator konto määratud Azure'i AD-ühenduse häälestamise ajal. See tõrge võib olla põhjustatud halb kasutajanimi või parool üldadministraator konto jaoks määratud või kuna määratud üldadministraatori kontoga on ühendatud. Selle probleemi lahendamiseks käivitage uuesti konfigureerimine õige kasutajanime ja parooliga ja veenduge, et administraator on mõne hallatava (vaid või parooli sünkroonimise oleks) konto.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab parooli loomisel ilmnes tõrge krüptovõtme või dekrüptimine parooli, et saabub pilveteenusesse. See tõrge võib näitab keskkonna probleeme. Vaadake lisateavet ja probleemi lahendamiseks oma sündmuselogi üksikasjad. Võite proovida ka keelamise ja uuesti lubamine parooli tagasikirjutusega teenuse probleemi lahendamiseks.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab ei saanud asutusesisese teenuse kasutuselevõtt protsessi käivitamiseks veebiteenuse parooli lähtestamine õigesti suhelda. Põhjuseks võib olla tulemüüri reegel või probleem on auth luba saada oma rentniku jaoks. Vea parandamiseks veenduge, et te ei blokeeri Väljaminevad ühendused üle TCP 443 TCP 9350-9354 või <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>ja kasutate pardal AAD administraatori konto on ühendatud. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AEGUNUD – sel juhul ei esine Azure'i AD-ühenduse, ainult juba varakult koostab, mis toetatud tagasikirjutusega DirSync.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab ei saanud asutusesisese teenuse offboarding protsessi käivitamiseks veebiteenuse parooli lähtestamine õigesti suhelda. Põhjuseks võib olla tulemüüri reegel või probleem on autoriseerimine luba saada oma rentniku jaoks. Vea parandamiseks veenduge, et te ei blokeeri Väljaminevad ühendused üle 443 või <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>ja et halduskeskuse AAD konto abil offboard on ühendatud. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et meil oli uuesti ühenduse loomiseks oma rentniku siini eksemplari. Tavaline tingimustel see peaks ei puuduta, kuid kui näete selle sündmuse mitu korda, kaaluge kontrollimine võrguühendus teenuse siini, eriti siis, kui see on kitsaribaline ühendus või pika latentsusajaga.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Oma parooli tagasikirjutusega teenuse seisundi jälgimiseks saadame südamelöögi ajal andmeid meie parooli lähtestamiseks veebiteenuse iga 5 minuti järel. See sündmus näitab, et ilmnes tõrge selle seisundi teabe tagasi saatmisega web pilveteenuses. Selle seisundi andmed ei sisalda mõnda OII või PII andmeid ja on puhtalt südamelöögi ajal ja põhiteenused statistika nii, et teenuse olek teavet pilveteenuses pakume.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab esines tagastatud AD tundmatu tõrge, sündmuselogist Azure'i AD-ühenduse server selle tõrke kohta lisateavet lähtekoha ADSync sündmuste jaoks.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et kasutaja, kes proovib lähtestamine või muutmine parooli ei leitud kohapealse kataloogi. See võib tekkida siis, kui kasutaja on kohapeal kustutatud, kuid mitte pilves, või kui on probleeme sünkroonimisega. Märkige oma sünkroonimine logid, samuti Viimane käivitada üksikasjad leiate lisateavet mõne sünkroonimine.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Kui parooli lähtestamine või muutmine taotluse pärineb pilves, kasutame pilve ankur määratud Azure'i AD-ühenduse häälestamise käigus määrata, kuidas lingi taotluse kohapealse keskkonna kasutaja. See sündmus näitab leidsime kaks kasutajat kohapealse kataloogi atribuudiga sama cloud ankur. Märkige oma sünkroonimine logid, samuti Viimane käivitada üksikasjad leiate lisateavet mõne sünkroonimine.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sel juhul näitab haldamise Agent teenusekonto on vajalikud õigused kõnealuse uue parooliga kontol. Veenduge, et kasutaja mets MA konto paigaldatud parooli lähtestamine ja muutke õigused kõigi objektide mets.  Lisateavet selle kohta, kuidas teha seda, lugege teemat <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">Samm 4: häälestamine Active Directory vajalikud õigused</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab, et saaksime proovisite lähtestamine või keelamist kohapealse konto parooli muutmine. Luba konto ja proovige uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sündmuse näitab, et saaksime proovitakse lähtestamine või kontot, mis on lukustatud kohapealne parooli muuta. Blokeerimise võib tekkida siis, kui kasutaja on proovinud muudatuse või lähtestage parool toiming liiga palju kordi lühikest. Ava konto ja proovige uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Sündmuse näitab, et kasutaja määratud vale praegune parool, kui toiming läbimiseks parooli muutmine. Määrake õige praegune parool ja proovige uuesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus juhul, kui parool tagasikirjutusega teenuse proovib teie kohalikku kausta, mis ei vasta parooli vanus, ajalugu, keerukuse või filtreerimise nõuded domeeni parooli seada.</p>
              <ul>
                <li class="unordered">
Kui teil on parooli vanus, ja selle aja jooksul parool on viimati muudetud, ei saa muuta parooli uuesti, kuni see jõuab märgitud teie domeeni. Katsetamiseks, peaks vanuse väärtuseks 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kui teil on lubatud parooli ajaloo nõuded ja seejärel valige parool, mida pole kasutatud viimase N korda, kui N on sätte parooli ajalugu. Kui valite selline parool, mida on kasutatud viimase N korda, siis kuvatakse tõrge sel juhul. Katsetamiseks, tuleks ajalugu väärtuseks 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kui teil on parooli keerukuse nõuetele, jõustatakse kõik neist, kui kasutaja proovib muutmine või lähtestamine parooli.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Kui teil on lubatud parooli filtrid ja kasutaja valib parool, mis ei vasta kriteeriumidele filtreerimise, siis lähtestamine või muutmine toiming nurjub.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>See sündmus näitab ilmnes probleem kirjutamine parool uuesti oma kohapealse kataloogi Active Directory tõttu mõni konfiguratsiooniprobleem. Märkige ruut Azure'i AD-ühenduse arvuti rakenduse sündmuselogi sõnumid, mis tõrke kohta lisateabe saamiseks teenuse ADSync. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AEGUNUD – sel juhul ei esine Azure'i AD-ühenduse, ainult juba varakult koostab, mis toetatud tagasikirjutusega DirSync.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AEGUNUD – sel juhul ei esine Azure'i AD-ühenduse, ainult juba varakult koostab, mis toetatud tagasikirjutusega DirSync.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>AEGUNUD – sel juhul ei esine Azure'i AD-ühenduse, ainult juba varakult koostab, mis toetatud tagasikirjutusega DirSync.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Parooli tagasikirjutusega ühenduvuse tõrkeotsing

Kui teil on probleeme teenuse katkemise parooli tagasikirjutusega komponent, Azure'i AD-ühenduse, siin on mõned kiirtoimingud, saate probleemi lahendamiseks.

 - [Uuesti Azure AD ühenduse sünkroonimise teenuse](#restart-the-azure-AD-Connect-sync-service)
 - [Keelake ja parooli tagasikirjutusega funktsiooni uuesti lubamine](#disable-and-re-enable-the-password-writeback-feature)
 - [Installige Azure'i AD-ühenduse uusim versioon](#install-the-latest-azure-ad-connect-release)
 - [Parooli tagasikirjutusega tõrkeotsing](#troubleshoot-password-writeback)

Soovitame üldiselt täidate neid juhiseid selleks, et taastada teenust kiirete viisil eeltoodud järjekorras.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Uuesti Azure AD ühenduse sünkroonimise teenuse
Azure'i AD-ühenduse sünkroonimise teenuse taaskäivitada aitab ühenduvusprobleemid või muude teenuse siirdamiseks probleemide lahendamiseks.

 1. Administraatorina, klõpsake **alustada** **Azure'i AD-ühenduse**serveriga.
 2. Tippige otsinguväljale **"tekst services.msc"** ja vajutage sisestusklahvi **Enter**.
 3. Otsige **Microsoft Azure'i AD-ühenduse** kirje.
 4. Paremklõpsake teenuse kirjet, klõpsake **uuesti**ja oodake, kuni toiming lõpule jõuab.

    ![][002]

Need juhised on pilvepõhise teenusega ühendust taastada ja lahendada katkestuste võib ilmneda.  Sünkroonimise teenuse taaskäivitamine ei lahendanud teie probleemi, soovitame püüate välja lülitada ja uuesti sisse järgmise sammuna parooli tagasikirjutusega funktsiooni.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Keelake ja parooli tagasikirjutusega funktsiooni uuesti lubamine
Keelamise ja uuesti lubamine funktsiooni parooli tagasikirjutusega aitab ühenduvuse probleemide lahendamiseks.

 1. **Azure'i AD-ühenduse konfiguratsiooniviisardi**avamine administraatorina.
 2. Sisestage dialoogiboksis **ühenduse Azure AD** **Azure AD üldadministraator identimisteave**
 3. Sisestage dialoogiboksis **ühenduse AD DS** **AD domeeniteenused administraatori identimisteave**.
 4. Klõpsake dialoogiboksis **kordumatult tuvastada kasutajate** klõpsake nuppu **edasi** .
 5. Klõpsake dialoogiboksis **valikulised funktsioonid** tühjendage märkeruut **parooli allahindluse** .

    ![][003]

 6. Klõpsake nuppu **Järgmine** ülejäänud dialoogiboksi lehtedelt muutmata midagi, kuni saate **konfigureerida** lehele.
 7. Veenduge, et lehe konfigureerimine kuvatakse **parooli allahindluse suvandi nimega keelatud** ja klõpsake rohelise **konfigureerimine** nuppu Kinnita muudatused.
 8. Klõpsake dialoogiboksis **lõpetatud** tühjendage ruut **Sünkrooni kohe** suvand ja klõpsake viisardi sulgemiseks **valmis** .
 9. Avage uuesti **Azure'i AD-ühenduse konfiguratsiooniviisardi**.
 10.    **Korrake juhiseid 2 – 8**, välja arvatud tagada teie **parool allahindluse ruudu** **valikulised funktsioonid** ekraani teenuse uuesti lubada.

    ![][004]

Need juhised on uuesti luua oma seoses meie pilveteenuses ja lahendada katkestuste võib ilmneda.

Kui keelamise ja uuesti lubamine parooli tagasikirjutusega funktsioon ei lahendanud teie probleemi, soovitame, proovige uuesti installida Azure AD-ühenduse järgmise sammuna.

### <a name="install-the-latest-azure-ad-connect-release"></a>Installige Azure'i AD-ühenduse uusim versioon
Uuesti installida Azure AD-ühenduse paketi lahendada konfiguratsiooni probleemid, mis võivad mõjutada teie võimalus ühte ühendada pilveteenustega meie või teie kohaliku AD keskkonnas paroolide haldamine.
Soovitame teil seda juhist alles pärast seda, kui proovite kaks esimest eespool kirjeldatud juhiseid.

 1. Alla laadida Office'i uusima versiooni Azure'i AD-ühenduse [siin](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Kuna on juba installitud Azure'i AD-ühenduse, peate ainult kohapealse versiooni täiendamiseks värskendamiseks installi Azure'i AD-ühenduse uusim versioon.
 3. Allalaaditud pakett käivitada ja järgige selle ekraanil kuvatavaid juhiseid, et Azure'i AD-ühenduse arvuti värskendamine.  Pole täiendavad juhised, mis on vajalikud juhul, kui olete kohandanud välja ruut Sünkrooni reegleid, millisel juhul tuleks **tagasi need enne jätkamist täiendamine ja käsitsi uuesti juurutada need pärast seda, kui olete lõpetanud**.

Need juhised on uuesti luua oma seoses meie pilveteenuses ja lahendada katkestuste võib ilmneda.

Kui server Azure'i AD-ühenduse uusima versiooni installimine ei lahendanud teie probleemi, soovitame, proovige pärast installimist uusimate sünkroonimine QFE viimase sammuna parool uuesti lubamine ja keelamine tagasikirjutusega.

Kui see ei lahenda teie probleemi, siis soovitame, et te Heitke pilk [Tõrkeotsing parooli tagasikirjutusega](#troubleshoot-password-writeback) ja [Azure AD parooli halduse KKK](active-directory-passwords-faq.md) kuvamiseks, kui teie probleem võib arutada seal.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Linkide parooli lähtestamiseks dokumentatsioon
Allpool on lingid kõik lehed Azure AD parooli lähtestamist dokumentatsiooni:

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
* [**Kuidas see toimib**](active-directory-passwords-how-it-works.md) - teave kuus erinevate osade teenuste, mida iga ei
* [**Alustamine**](active-directory-passwords-getting-started.md) – saate teada, kuidas võimaldavad kasutajatel lähtestamine ja pilveteenuse või kohapealse parooli muutmine
* [**Kohanda**](active-directory-passwords-customize.md) – siit saate teada, kuidas kohandada ilme ja olemus ja käitumine teenuse teie asutuse vajadustest
* [**Head tavad**](active-directory-passwords-best-practices.md) – saate teada, kuidas kiiresti juurutada ja paroolide ettevõtte tõhus haldamine
* [**Saada ülevaadet**](active-directory-passwords-get-insights.md) - Lisateavet meie integreeritud aruandlusvõimalused
* [**FAQ**](active-directory-passwords-faq.md) – siin on vastused korduma kippuvatele küsimustele
* [**Lisateavet leiate teemast**](active-directory-passwords-learn-more.md) -valige suure tehnilise üksikasjadesse teenuse tööpõhimõtted



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
