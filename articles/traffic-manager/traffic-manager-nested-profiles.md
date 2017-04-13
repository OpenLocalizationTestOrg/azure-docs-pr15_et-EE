<properties
    pageTitle="Pesastatud liikluse haldur profiilid | Microsoft Azure'i"
    description="Selles artiklis selgitatakse profiilid pesastatud funktsiooni Azure'i liikluse haldur"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Pesastatud liikluse haldur profiilid

Liikluse haldur sisaldab mitmesuguseid liikluse marsruutimist meetodid, mis võimaldavad teil määrata, kuidas liikluse haldur valib, milline lõpp-punkti saavate liikluse iga lõppkasutaja. Lisateavet leiate teemast [liikluse haldur meetodite liikluse marsruutimist](traffic-manager-routing-methods.md).

Iga liikluse haldur profiili saate määrata ühe liikluse marsruutimist meetod. Kuid on stsenaariumid, mis nõuavad keerukamaid liikluse marsruutimist suurem marsruudi liikluse haldur üks profiil. Saate pesastada liikluse haldur profiilid ühendada mitu liikluse marsruutimist meetodit kasu. Pesastatud profiilid võimaldavad alistada tugi suuremad ja keerukamaid rakenduse kasutuselevõttu liikluse haldur vaikekäitumise.

Järgmised näited illustreerivad pesastatud liikluse haldur profiilid kasutamine erinevaid võimalusi.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Näide 1: Kombineerides "Jõudlus" ja "Kaalutud" liikluse marsruutimine

Oletame, et olete juurutanud rakenduse Azure järgmistes regioonides: Lääne USA, Lääne Euroopa ja Ida-Aasia. Liikluse haldur "Jõudlus" liikluse marsruutimist meetodi abil saate jagada liiklust lähemal kasutaja piirkond.

![Ühe liikluse haldur profiil][1]

Nüüd Oletame, et soovite testida enne jooksvalt rohkem suuresti värskendust teenust. Soovite kasutada "kaalutud" liikluse marsruutimist meetodit kaoga juurutamise testi-liikluse suunamiseks. Seadistage Lääne Euroopa testi juurutamise olemasoleva tootmise juurutamise kõrval.

Te ei saa ühendada nii kaalutud"ja" jõudluse liikluse-marsruutimise üks profiil. Toeta seda stsenaariumi, luua liikluse haldur profiili kaks Lääne Euroopa lõpp-punktid ja "Kaalutud" liikluse marsruutimist meetodi abil. Järgmiseks saate lisada seda profiili 'lapse' lõpp 'ema' profiili. Ema profiili endiselt kasutab jõudluse liikluse marsruutimist meetodit ja see sisaldab on globaalne juurutuste nimega lõpp-punktid.

Järgmine diagramm näitab käesoleva näite.

![Pesastatud liikluse haldur profiilid][2]

Selle konfiguratsiooni puhul ema profiili kaudu suunatud liikluse jaotab liikluse piirkondade tavalisel viisil. Lääne Euroopa, pesastatud profiili jaotab liikluse tootmise ja testi lõpp-punktid vastavalt määratud kaalu.

Kui ema profiili kasutab "Jõudlus" liikluse marsruutimist meetod, peab iga lõpp-punkti määratud asukohta. Asukoht on määratud soovitud lõpp-punkti. Saate valida Azure regiooni juurutamise kõige lähemal. Azure'i piirkondade on asukoht väärtused Internet latentsus tabel ei toeta. Lisateavet leiate teemast [liikluse haldur "Jõudlus" liikluse marsruutimist meetod](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Näide 2: Pesastatud profiilidest jälgimise lõpp-punkti

Liikluse haldur jälgib aktiivselt iga teenuse lõpp-punkti. Kui lõpp on vigane, suunab liikluse haldur kasutajatel säilitada oma teenuse kättesaadavust Alternatiivne lõpp-punktid. Lõpp-punkti jälgimine ja Tõrkesiirde käitumine kehtib kõikide meetodite liikluse marsruutimist. Lisateavet leiate teemast [Liikluse haldur lõpp-punkti jälgimine](traffic-manager-monitoring.md). Lõpp-punkti jälgimine toimib erinevalt pesastatud profiilid. Pesastatud profiilid, ema profiili ei teha seisundikontrollid lapsele otse. Selle asemel kasutatakse üldist tervist lapse profiili arvutamiseks seisundi lapse profiili lõpp-punktid. Selle teabe seisund on levitatud üles pesastatud profiili hierarhia. Ema profiili see kokkuvõtliku seisund kindlaks, kas otsese liikluse tütarüksuste profiili. Täielikud üksikasjad käesoleva artikli jaotisest [KKK](#faq) näidata seisundi jälgimine pesastatud profiilid.

Tagasi eelmises näites, Oletame, et tootmise juurutamise Lääne Euroopa nurjub. Vaikimisi 'lapse' profiili suunab kogu liikluse juurutamise test. Kui test juurutamise ka ei õnnestu, määratleb ema profiili, et lapse profiili peaksid saama liikluse, kuna kõik lapse lõpp-punktid on vigane. Seejärel jaotab ema profiili-liikluse muude piirkondade.

![Pesastatud profiili Tõrkesiirde (vaikekäitumine)][3]

Teil võib olla selle korraldusega rahul. Või siis peaksite olema kõik liikluse Lääne-Euroopa on nüüd testi juurutusega piiratud alamhulk liikluse asemel. Seisundi test juurutus, olenemata sellest, mida soovite muude piirkondadele üle nurjuda, kui tootmise juurutamise Lääne Euroopa nurjub. See Tõrkesiirde lubamiseks saate määrata 'MinChildEndpoints' parameetri lapse profiili konfigureerimisel lõpp ema profiili nimega. Parameetri määratleb väikseima arvu lapse profiili saadaval lõpp-punktid. Vaikeväärtus on '1'. Selle stsenaariumi korral, määrake MinChildEndpoints väärtuse 2. Sellest väiksem, ema profiili leiab kogu tütarüksuste profiil saadaval ja suunab liikluse soovitud lõpp-punktid.

Järgmine joonis kujutab selle konfiguratsiooni:

![Pesastatud profiili Tõrkesiirde koos 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>"Prioriteet" liikluse marsruutimist meetodit jaotab kogu liikluse ühe lõpp-punkti. Seega on vähe eesmärki muu '1' MinChildEndpoints säte tütarüksuste profiili.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Näide 3: Tähtsuse Tõrkesiirde regioonide "Jõudlus" liikluse marsruutimine

"Jõudlus" liikluse marsruutimist meetodit vaikekäitumine on mõeldud vältimiseks üle peale edasi lähima lõpp-punkt- ja põhjustada tõrkeid kaskaadlaadistiku sarja. Kui lõpp nurjub, kogu liiklus, mis tuleks suunata selle lõpp-punkti on ühtlaselt abil soovitud lõpp-punktid kõigis piirkondades.

!["Jõudlus" liikluse vaikimisi Tõrkesiirde marsruutimine][5]

Siiski Oletame, et te eelistate Lääne Euroopa liikluse Tõrkesiirde Lääne USA ja ainult suunaks liikluse muude piirkondadele, kui on saadaval nii lõpp-punktid. Saate luua see lahendus lapse profiiliga "Prioriteet" liikluse marsruutimist meetod.

!["Jõudlus" liikluse eelis Tõrkesiirde marsruutimine][6]

Kuna Lääne Euroopa lõpp-punkti on suurem kui Lääne USA lõpp-punkti prioriteet, saadetakse kõik liikluse Lääne Euroopa lõpp-punkti võrgusoleku nii lõpp-punktid. Kui Lääne Euroopa nurjub, suunatakse selle liikluse Lääne US. Pesastatud profiiliga, suunatakse liikluse Ida-Aasia ainult siis, kui nii Lääne Euroopa ja Lääne USA nurjuda.

Korrake seda kõigi piirkondade. Asendage kõigi kolme lõpp-punktid ema profiili kolme lapse profiilid, iga esitada tähtsuse Tõrkesiirde jada.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Näide 4: Juhtimine "Jõudlus" liikluse marsruutimist mitme lõpp-punktid piirkonna vahel

Oletame, et 'jõudlus"liikluse marsruutimist meetodit kasutatakse profiili, mis sisaldab rohkem kui ühe lõpp-punkti kindla ala. Vaikimisi suunatakse piirkonnas liikluse ühtlaselt üle saadaval piirkonna kõik lõpp-punktid.

!["Jõudlus" liikluse marsruutimist-piirkonna liikluse jaotuse (vaikekäitumine)][7]

Selle asemel Lääne Euroopa liita mitme lõpp-punktid, nende lõpp-punktid on ümbritsetud eraldi lapse profiili. Lapse profiili lisatakse ainult lõpp-Lääne Euroopa ema. Lapse profiili sätted saate määrata, võimaldades prioriteet või kaalutud liikluse marsruutimist selle piirkonnas Lääne Euroopa liikluse jaotuse.

!["Jõudlus" liikluse kohandatud-piirkonna liikluse jaotuse marsruutimine][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Näide 5: Lõpp-punkti kohta jälgimisega seotud sätted

Oletame, et kasutate liikluse haldur sujuvalt migreerida liikluse aadressilt pärand kohapealse veebisaidi majutada Azure pilvepõhist versiooni. Pärand saidi jaoks, mida soovite kasutada avalehe URI saidi seisundi jälgimine. Kuid uue pilvepõhise versioon on rakendamise kohandatud jälgimise lehe (tee "/ monitor.aspx"), mis sisaldab täiendavat kontrolli.

![Liikluse haldur lõpp-punkti jälgimine (vaikekäitumine)][9]

Liikluse haldur profiili jälgimisega seotud sätted rakenduvad kogu lõpp-punktid sees üks profiil. Pesastatud profiilid, saate eri lapse profiili ühe koha määratlemine eri jälgimisega seotud sätteid.

![Liikluse haldur lõpp-punkti kohta lõpp-punkti sätetega jälgimine][10]

## <a name="faq"></a>KKK

### <a name="how-do-i-configure-nested-profiles"></a>Kuidas seadistada pesastatud profiilid?

Pesastatud liikluse haldur profiilid saab konfigureerida nii Azure'i ressursihaldur ja klassikaline Azure'i REST API Azure PowerShelli cmdlet-käskude ja platvormidel Azure'i CLI käskude abil. Need on toetatud ka uue Azure portaali kaudu. Ei toetata klassikaline portaalis.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Mitu kihti pesastamise teeb liikluse sõim toetab?

Saate pesastada kuni 10 taset profiilid. "Tsüklid ei ole lubatud.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Muud tüüpi lõpp-punkti saate segada pesastatud tütarüksuste profiilid sama liikluse haldur profiili?

Jah. Puuduvad piirangud kohta, kuidas saate ühendada sees profiili eri tüüpi lõpp-punktid.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Kuidas taotleda arvelduse mudel pesastatud profiilid?

Pole negatiivset mõju pesastatud profiili kasutamiseks hinnad on.

Liikluse haldur arveldus koosneb kahest osast: lõpp-punkti seisundikontrollid ja miljoneid DNS-i päringud

- Lõpp-punkti seisundikontrollid: on tasuta lapse profiili lõpp ema profiili konfigureerimisel. Lõpp-punktid tütarüksuste profiili jälgimine on arve tavapärasel viisil.
- DNS-i päringud: iga päringu arvestatakse ainult üks kord. Päring ema profiili, mis tagastab lõpp lapse profiili arvestatakse ainult ema profiili suhtes.

Üksikasjadega tutvumiseks vaadake [liikluse haldur hinnakirjad lehe](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Kas jõudluse mõju pesastatud profiilide?

Ei. On pole jõudluse tekivad pesastatud profiilid.

Nimeserverite liikluse haldur läbida profiili hierarhia ettevõttesiseselt iga DNS-i päringu töötlemise ajal. DNS-i päringu ema profiili võite saada vastuse DNS-i lõpp-punkti koos lapse profiili. Kas kasutate üks profiil või pesastatud profiilid, kasutatakse ühe CNAME-kirje. Ei ole vaja CNAME-kirje iga profiili hierarhia loomiseks.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Kuidas liikluse haldur arvutada pesastatud lõpp-punkti ema profiili seisundi?

Ema profiili ei teha otse lapsele seisundikontrollid. Selle asemel kasutatakse üldist tervist lapse profiili arvutamiseks seisundi lapse profiili lõpp-punktid. See teave on levitatud pesastatud profiili hierarhia määratlemiseks seisundi pesastatud lõpp-punkti üles. Ema profiili kasutab seda liidetud seisund kindlaks teha, kas liiklus saab suunata lapse.

Järgmises tabelis kirjeldatakse käitumist liikluse haldur seisund kontrollib pesastatud lõpp-punkti.

|Lapse profiili kuvari olek|Ema lõpp-punkti kuvari olek|Märkmete|
|---|---|---|
|Keelatud. Lapse profiili on keelatud.|Peatatud|Ema lõpp-punkti olek on peatatud, pole keelatud. Keelatud olekusse on mõeldud mis näitab, et olete keelanud ema profiilis lõpp-punkti.|
|Halvenenud. Lõpp-punkti vähemalt ühe profiili on Degraded olekus.| Online: Online lõpp-punktid lapse profiili arv on vähemalt MinChildEndpoints väärtus.<BR>CheckingEndpoint: Online'i pluss CheckingEndpoint lõpp-punktid lapse profiili arv on vähemalt MinChildEndpoints väärtus.<BR>Halvenenud: teisiti.|Liikluse suunatakse oleku CheckingEndpoint lõpp. Kui MinChildEndpoints on liiga suur, lõpp-punkti alati halvenenud.|
|Võrgus. Lõpp-punkti vähemalt ühe profiili on võrguühenduse. Pole lõpp-punkti on Degraded olekus.|Vt.||
|CheckingEndpoints. Lõpp-punkti vähemalt ühe profiili on "CheckingEndpoint". Pole lõpp-punktid on "Online" või "halvenenud|Sama mis eelmine.||
|Passiivsed. Kõik lapse profiili lõpp-punktid on peatatud või keelatud või seda profiili on pole lõpp-punktid.|Peatatud||


## <a name="next-steps"></a>Järgmised sammud

Lisateavet [liikluse haldur tööpõhimõtted](traffic-manager-how-traffic-manager-works.md)

Siit saate teada, kuidas [liikluse haldur profiili loomine](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

