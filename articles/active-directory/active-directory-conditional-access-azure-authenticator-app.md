
<properties
    pageTitle="Azure'i Autentija Androidi jaoks | Microsoft Azure'i"
    description="Microsoft Azure'i autentimisrakenduse saab sisse logima juurdepääsu töö ressurssidele. Azure'i autentimisrakenduse annab märku ootel kahekordne kontrollimise taotluse, kuvades teatise mobiilsideseadmesse."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="azure-authenticator-for-android"></a>Azure'i Autentija Androidi jaoks

IT-administraator võib on soovitatav kasutada Microsoft Azure'i Authenticator sisselogimine juurde pääseda oma töö ressursse. Selle rakenduse moodi nende kahe Logi sisse.

* Mitmikautentimise võimaldab teil kaitsta oma töökoha või kooli konto kaheastmelise kontrollimise. Saate sisselogimist kasutades midagi teate (nt parool) ja kaitse konto veelgi midagi teil (Turvalisus võti selle rakenduse). Azure'i autentimisrakenduse annab märku ootel kahekordne kontrollimise taotluse, kuvades teatise mobiilsideseadmesse. Peate lihtsalt vaate rakendus ja puudutage taotluse kinnitamine või tühistamine. Vaheldumisi, võidakse teilt sisestage pääsukood, mis kuvatakse rakenduses.

* Töö konto võimaldab teil Androidi telefonis või tahvelarvutis muutuda usaldusväärne ja ettevõtte rakenduste ühekordse sisselogimise (SSO) pakkumiseks. IT-administraator nõuda saate lisada töö kontole juurdepääsuks ettevõtte ressursse. SSO saate üks kord sisse logida ja üle kõik rakendused teie ettevõttel on kättesaadav teile sisselogimisel automaatselt kasutada.

## <a name="installing-the-azure-authenticator-app"></a>Azure'i autentimisrakenduse installimine

Saate installida Azure autentimisrakenduse Google Play poest.
Juhised Samsung Androidi seadme vs Samsung mõeldud Androidi seadme kaudu töö konto lisamiseks on veidi teistsugused. Juhised nii on loetletud allpool.

<a name="adding-the-work-account-from-samsung-android-device"></a>Töö konto lisamisel Samsung Androidi seadmes
----------------------------------------------------------------------------------------------------------------
###<a name="adding-the-work-account-through-the-app-home-screen"></a>Töö konto kaudu rakenduse avakuval lisamine

Järgmised juhised kehtivad Samsung GS3 ja telefonid või Lisa2 kohal ja tahvelarvutite kohal.

1. Aktsepteerige rakenduse avakuval lõppkasutaja litsentsileping (EULA).
2. Kuval konto aktiveerimiseks klõpsake kontekstimenüüs käsku paremal ja valige **töö konto**.
3. Valige kuval konto lisamine** Töö konto**.
4. Kuval aktiveerida seadme administraator, klõpsake nuppu **Aktiveeri**.
5. Privaatsuspoliitika ekraanil, märkige ruut ja klõpsake nuppu **Kinnita**.
6. Töökoha liitumine Kuva, sisestage kasutajanimi, mis on teie asutuse pakutavat ja klõpsake nuppu **Liitu**.
7. Azure'i autentimisrakenduse sisselogimiseks sisestage oma ettevõtte lisamine *** konto ja parool ning klõpsake nuppu **Logi sisse**.
8. Järgmisele kuvale, kus on kuvatud teave mitmikautentimise (MFA) jaoks suurem turvalisus ja pole kohustuslik. Kui teie töö või kooli jaoks on vaja teise autentimine töö konto loomiseks, siis kuvatakse kuva. See artikkel sisaldab juhiseid täpsemaks oma konto kinnitada.
9. Töökoha liitumine kuva kuvab teate, "**liitumist oma töökoha**". Azure'i autentimisrakenduse liitub oma seadmesse oma töökoha.
10. Peaksite nägema ühendatud töökohal sõnumi järgmisel kuval.

>[AZURE.NOTE]
> Teil on lubatud ühe töö konto teie seadmes.

### <a name="adding-the-work-account-from-the-settings-menu"></a>Töö konto lisamisel menüü Sätted kaudu
Kui olete installinud Azure'i autentimisrakenduse, saate ka luua töö konto Androidi kontohaldur.

1. Klõpsake menüü sätted liikuge **kontod** ja klõpsake käsku **Lisa konto**.
2. Järgige juhiseid 3-10 protseduuri, töö konto kaudu rakenduse avakuval töö konto lisamine.

<a name="adding-the-work-account-from-a-non-samsung-android-device"></a>Töö konto lisamisel Samsung Androidi seadmes
------------------------------------------------------------------------------------------------------------------
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Töö konto kaudu rakenduse avakuval lisamine

1. Aktsepteerige rakenduse avakuval lõppkasutaja litsentsileping (EULA).
2. Kuval konto aktiveerimiseks klõpsake kontekstimenüüs käsku paremal ja valige **töö konto**.
3. Valige kuval kontod nuppu **Lisa konto**.
4. Kui näete kuval kontod, klõpsake nuppu **Lisa konto**. Kui töö konto on juba varem loodud, kuvatakse sünkroonimise ekraanil nähtaval olemasoleva töökonto. Saate säilitada töö kontoga lihtsalt koputage tagasinoolt avakuvale. Vaheldumisi, saate valida konto eemaldamine ja uuesti luua uue töö konto On the töökoha liitumine Kuva, sisestage kasutajanimi, mis on teie asutuse pakutavat ja klõpsake nuppu Liitu.
5. Azure'i autentimisrakenduse sisselogimiseks sisestage oma ettevõtte konto ja parool ja klõpsake nuppu **Logi sisse**.
7. Järgmisel kuval, mis kuvatakse mitme teguriga autentimine (MFA) teave on mõeldud suurem turvalisus ja pole kohustuslik. Kui teie töö või kooli jaoks on vaja teise autentimine töö konto loomiseks, siis kuvatakse kuva. See artikkel sisaldab juhiseid täpsemaks oma konto kinnitada.
8. Klõpsake järgmisel kuval nuppu **OK** . Serdi nimi ei muutu.
Klõpsake sõnumis "Ühendab oma töökoha". Azure'i autentimisrakenduse liitub oma seadmesse oma töökoha.
Peaksite nägema ühendatud töökohal sõnumi järgmisel kuval.

>[AZURE.NOTE]
> Teil on lubatud ühe töö konto teie seadmes.

Kui olete installinud Azure'i autentimisrakenduse, saate ka luua töö konto Androidi kontohaldur.

1. Klõpsake menüü **sätted** liikuge kontod ja klõpsake käsku **Lisa konto**.
2. Järgige juhiseid 2 – 7 toimingut, kuni soovitud rakenduse Avaleht Kuva **, töö konto lisamisel töö konto lisamiseks.

### <a name="how-to-find-out-which-version-is-installed"></a>Kuidas teada saada, milline versioon on installitud

1. Saate teada, millist versiooni Azure'i autentimisrakenduse ja oma seadmesse on installitud versiooniga seotud teenus.
2. Hüpikmenüüst, klõpsake nuppu **teave**.
3. Teave ekraanil kuvatakse rakenduse ja teie seadmesse installitud versioonid.
 
### <a name="sending-log-files-to-report-issues"></a>Logifailide saata teatada probleemidest

1. Järgige juhiseid Microsoft Support aruande koos Azure autentimisrakenduse juhtum, saada juhtumi numbri ja saata määratud arvu langeva logifailid järgmiselt:
2. Hüpikmenüüst, klõpsake nuppu **logimine**.
3. Kui teil on Microsoft Support avatud juhtumi, märkige üles langeva arvu (peate seda hiljem). Kui te ei ole juba loonud tugiteenuse juhtumi ja soovite abi, järgige juhiseid [Microsofti tugiteenuste](https://support.microsoft.com/en-us/contactus) avada uue juhtumi.
4. Logimise ekraanil, klõpsake nuppu **Saada kohe**.
5. Valige meiliteenuse pakkuja, mida soovite kasutada.
7. Kui teil on juba avatud juhtum Microsoft Support, võtke ühendust teie probleemi, et teada saada, kuidas saata Logi andmete määratud tugiteeninduse ja on seostatud teie juhtum. Tugiteeninduse annab teile e-posti aadress ja teema rea kohta teavet. Kui teil on juba tugiteenuse juhtumi, järgige juhiseid veebisaidil Microsoft Support avada uue juhtumi.
9. Redigeerige reale **Adressaat** ja **teemareal Microsoft support saite teabega** .
10. Azure'i autentimisrakenduse manustab logifaili saadate meilisõnumi. Teil on probleeme Kirjelda, värskendage adressaatide loend (valikuline) ja saatke meilisõnum.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>Töö konto kustutamine ja jättes oma töökoha

Saate eemaldada töö konto loodud igal ajal järgmiselt:

**Menüü Sätted kaudu töö konto kustutamine**

1. Valige kontod haldur **töö konto**.
2. Kuval töö konto **Üldsätted**, valige **konto sätted – jätke töökoha võrgus**.
3. Valige **jätta** ekraanile **Töökoha liituda** .
4. Klõpsake nuppu **OK** , kui kuvatakse teade"soovite kindlasti töökoha" kuvatakse.
5. See tagab, et olete kustutanud oma töö konto oma töökoha.

>[AZURE.NOTE]
>Soovitatav on, et te ei kasuta kustutada töö konto, kui seda suvandit ei pruugi töötada mõnes varasemas versioonis Androidi suvand Eemalda konto.

##<a name="uninstalling-the-app"></a>Rakenduse desinstallimine

Samsung Androidi seadmes seadme administraatoriõigused eemaldada järgmiselt desinstallimine enne selle 
1. Valige menüü **sätted** **süsteem** **Turvalisus**.
2. Sees D**andmed haldus**, klõpsake nuppu **seadme administraatoritele**. Veenduge, et **Azure'i Autentija** kõrval olev ruut oleks tühi.

##<a name="troubleshooting"></a>Tõrkeotsing

Kui kuvatakse **Tõrketeade võtmehoidjas**, võimalik, et teil pole lukustus ekraani Sea PIN-koodi häälestamine. Selle probleemi lahendamiseks desinstallige Azure'i autentimisrakenduse, konfigureerimine lukustuskuval PIN-koodi ja rakenduse uuesti.
