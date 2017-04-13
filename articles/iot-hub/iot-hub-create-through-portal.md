<properties
     pageTitle="Soovitud asjade keskuse loomiseks kasutada Azure portaali | Microsoft Azure'i"
     description="Ülevaade sellest, kuidas luua ja hallata Azure asjade jaoturi Azure portaali kaudu"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Soovitud asjade jaoturi Azure portaali loomine

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Sissejuhatus

Selles artiklis kirjeldatakse, kuidas Azure'i portaalis teenuse jaoturi asjade leidmiseks ja kuidas luua ja hallata asjade jaoturi.

## <a name="where-to-find-iot-hubs"></a>Kust leida asjade jaoturi

On erinevad kohad, kust leiate asjade jaoturi.

1. **+ Uus**: **Azure'i asjade jaoturi** on teenuse asjade ja leiate kategooria **Asjade Internet**, klõpsake jaotises **+ Uus**, sarnaselt muude teenustega.

2. Asjade jaoturi pääseb juurde ka kaudu kuvatakse Marketplace'ist **Asjade Interneti**teenuse pilt nimega.

## <a name="create-an-iot-hub"></a>Soovitud asjade keskuse loomine

Saate luua ka asjade jaoturi järgmistel viisidel.

- Soovitud asjade keskuse kaudu suvand **+ Uus** loomise viib järgmise ekraanipilt tera. Juhised selle meetodi abil ja kaudu kuvatakse Marketplace'ist asjade keskuse loomiseks on täpselt ühesugused.

- Soovitud asjade keskuse kaudu kuvatakse Marketplace'ist loomine: **Loo** klõpsamisel avatakse tera, mis on identne eelmise tera **+ Uus** kogemuse. Järgmiste jaotiste loendis mitme etappe soovitud asjade keskuse loomine.

### <a name="choose-the-name-of-the-iot-hub"></a>Valige asjade jaoturi nimi

Soovitud asjade keskuse loomiseks peate nime jaoturi. See nimi peab olema kordumatu jaoturiga. Jaoturi dubleeriks on lubatud tagasi lõpuks, seega on soovitatav see jaoturi võimalikult kordumatult nimega.

### <a name="choose-the-pricing-tier"></a>Valige hinnakirjad taseme

Saate valida neli taset: **tasuta**, **Standard 1** ja **Standard 2**ja **Standard S3**. Tasuta taseme võimaldab asjade jaoturi ja kuni 8 000 sõnumit päevas ühendatakse ainult 500 seadmed.

**Standard S1**: asjade jaoturi S1 edition on mõeldud asjade lahendusi, mis on suure hulga seadmete genereerimine suhteliselt andmehulkade seadme kohta. Iga üksuse S1 väljaanne võimaldab kuni 400 000 sõnumit päevas ühendatud kõikides seadmetes.

**Standardse S2**: asjade jaoturi S2 edition on mõeldud asjade lahendusi, kus seadmed luua suurte andmehulkade. Iga üksuse S2 väljaanne võimaldab kuni 6 miljonit sõnumite kõigi ühendatud seadmete päevas.

**Standardse S3**: asjade jaoturi S3 edition on mõeldud suurte andmehulkade andvate asjade lahendusi. Iga üksuse S3 väljaanne võimaldab kuni 300 miljonit sõnumit päevas kõigi ühendatud seadmete vahel.

![][4]

> [AZURE.NOTE] Asjade jaoturi abil saavad ainult ühe tasuta jaoturi Azure tellimuse kohta.

### <a name="iot-hub-units"></a>Asjade jaoturi ühikud

Üksuse asjade jaoturi sisaldab päevas sõnumite arv. Selle jaoturi jaoks toetatud sõnumite arv on ühikute korrutatuna päevas teise astme teadete arv. Näiteks, kui soovite asjade jaoturi toetamiseks sissepääsu 700.000 sõnumeid, valige kahe S1 esimese taseme üksuste.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Seadme cloud sektsioonid ja ressursirühm

Saate muuta sektsioonid jaoks soovitud asjade jaoturi arv. Vaikimisi sektsioonid on seatud 4; Siiski saate valida erineva arvu sektsioonid rippmenüü loendi.

Ressursi rühmade jaoks pole vaja ka tühjad ressursikeskuse rühma loomine. Ressursi loomisel saate luua uue ressursirühma või saate kasutada olemasolevat rühma ressursi.

![][5]

### <a name="choose-subscriptions"></a>Valige tellimuste

Azure'i asjade jaoturi kuvatakse automaatselt Azure kasutaja kontoga seotud tellimuste loend. Saate valida ühe suvandid Azure tellimuse asjade keskuses seostada.

### <a name="choose-the-location"></a>Valige asukoht

Asukoha valik loetletakse regioonid, kus asjade jaoturi on saadaval. Asjade jaoturi on saadaval, mis on järgmisest kohast: Austraalia Ida, Austraalia kodutee, Aasia Ida, Aasia, Kagu, Põhja-Euroopa, Euroopa Lääne, Jaapani Ida, Jaapan Lääne, meile Ida meile Lääne.

### <a name="create-the-iot-hub"></a>Asjade keskuse loomine

Kui kõik eelnevate juhiste järgimisel on lõpetatud, asjade jaoturi on loomiseks valmis. Klõpsake nuppu **Loo** loomise selle asjade jaoturi teatud suvanditega tagaandmebaas protsessi käivitamiseks ja juurutamiseks määratud asukohta.

Võib kuluda paar minutit asjade jaoturi loodud võtab aega esineda sobivasse serverid juurutamiseks tagaandmebaas.

## <a name="change-the-settings-of-the-iot-hub"></a>Asjade jaoturi sätete muutmine

Pärast selle loomist asjade jaoturi keelest mõne olemasoleva asjade jaoturi sätete muutmiseks.

![][8]

**Ühiskasutusse antud Juurdepääsupoliitikaid**: Need poliitikad määratleda ühenduse asjade jaoturi seadmed ja teenuste jaoks õigusi. Need poliitikad pääsete, klõpsates nuppu **Ühiskasutuses Juurdepääsupoliitikaid** jaotises **Üldine**. Klõpsake selle tera saate muuta olemasolevaid poliitika või lisada uue poliitika.

### <a name="create-a-policy"></a>Poliitika loomine

- Klõpsake nuppu **Lisa** tera avamiseks. Sisestage uue poliitika nimi ja õigused, mille soovite seostada selle poliitika, nagu on näidatud järgmisel joonisel.

    On mitu õigused, mis võib olla seotud ühiskasutusega poliitika. Kaks esimest poliitika, **registri lugeda** ja **kirjutada registri**, anda lugeda või seadme identiteedi poe või identiteedi registri kirjutamisõigused õigused. Valiku kirjutamine automaatselt valib loetuks võimalus samuti.

    **Teenuse ühenduse** poliitika annab juurdepääs näiteks tarbija rühma teenuste ühenduse asjade jaoturi cloud-side lõpp-punktid. **Ühendage seade** poliitika annab õiguste kohta asjade jaoturi seadme-side lõpp-punktid sõnumite saatmine ja vastuvõtmine.

- Klõpsake nuppu **Loo** selles vastloodud poliitika olemasolevasse loendisse lisamiseks.

![][10]

## <a name="messaging"></a>Sõnumside

**Sõnumside** sõnumside asjade jaoturi, mis muudetakse atribuutide loendi kuvamiseks nuppu. On kaks peamist tüüpi atribuudid, mida saate muuta või kopeerida: **pilveteenuste seadmesse** ja **seadme pilveteenusesse**.

- **Pilveteenuse seadmesse** sätted: see säte on kaks subsettings: **pilveteenuste seadme TTL** (--eluiga) ja sõnumite **säilitamise aeg** . Asjade jaoturi esmakordsel loomisel luuakse nii nende sätete vaikeväärtus on üks tund. Need väärtused reguleerimiseks kasutage vastavaid liugureid või tippige soovitud väärtused.

- **Seadme pilveteenusesse** sätted: see säte on mitu subsettings, mis on nimega/määratud kui asjade jaoturi on loodud ja saate kopeerida ainult muude subsettings, mis on kohandatavad. Järgmises jaotises on loetletud neid sätteid.

**Sektsioonid**: selle väärtuseks on seatud kui asjade jaoturi loomist ja selle sätte abil saate muuta.

**Sündmuse jaoturi ühilduv nimi ja lõpp-punkt**: kui the asjade jaoturi on loodud, kuvatakse sündmuse jaoturi luuakse ettevõttesiseselt võib-olla peate juurdepääsu teatud tingimustel. Selle sündmuse jaoturi ühilduv nimi ja lõpp-punkti ei saa kohandada, kuid saab kasutada kaudu nuppu **Kopeeri** .

**Säilituspoliitika aega**: üks päev vaikimisi seatud, kuid muid väärtusi rippmenüü loendi abil saab kohandada. See väärtus on päeva seadme pilveteenusesse, mitte tundi, on sarnane säte Cloud seadmesse.

**Rühmi**: tarbija rühmad on sarnaselt muude sõnumite süsteemid, mida saab kasutada andmete tõmmata teatud viisil ühendada muude rakenduste ja teenuste asjade jaoturi säte. Iga asjade jaoturi luuakse vaikimisi tarbija rühma. Siiski saate lisada või kustutada oma asjade jaoturi rühmi.

> [AZURE.NOTE] Vaikejaotise tarbija ei saa redigeerida või kustutada.

![][11]

## <a name="pricing-and-scale"></a>Hinnad ja mastaapimine

Mõne olemasoleva asjade keskuse hinnad saab muuta kaudu **hinnakirjad** sätted, v.a järgmised erandid:

- Praegune rakendamise soovitud asjade jaoturi koos tasuta SKU-ga ei saa muuta astme ühele makstud SKU-d või vastupidi.
- Seal saab ainult ühe taseme tasuta asjade jaoturi Azure'i tellimus.

![][12]

Üleminek rakenduselt jõudmine (S2 või S3) madalama taseme (S1 või S2) on lubatud ainult siis, kui konflikti pole sel päeval saadetud sõnumite arv. Näiteks kui päevas sõnumite arv ületab 400 000, siis taseme jaoks asjade jaoturi saab muuta. Siiski S1 taseme muutmisel seejärel jaoturi on rakendus sel päeval.

## <a name="delete-the-iot-hub"></a>Asjade jaoturi kustutamine

Saate sirvida asjade jaoturi, mille soovite kustutada, klõpsake nuppu **Sirvi**ja seejärel sobivat jaoturi kustutamiseks. Klõpsake kustutamiseks jaoturi jaoturi nime all nuppu **Kustuta** .

## <a name="next-steps"></a>Järgmised sammud

Järgige neid linke Azure'i asjade jaoturi haldamise kohta lisateabe saamiseks:

- [Hulgi asjade seadmete haldamine][lnk-bulk]
- [Kasutus mõõdikud][lnk-metrics]
- [Toimingute jälgimine][lnk-monitor]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]
- [Turvaline põhjusel teie asjade lahendus üles][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md