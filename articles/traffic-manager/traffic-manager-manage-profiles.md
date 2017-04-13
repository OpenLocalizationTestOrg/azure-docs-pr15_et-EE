<properties
    pageTitle="Azure'i liikluse haldur profiilide haldamine | Microsoft Azure'i"
    description="See artikkel aitab teil luua, keelata, lubamine, kustutamine ja Azure liikluse haldur profiili ajaloo kuvamine."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Azure'i liikluse haldur profiil on haldamine

Juhtida liiklust pilveteenustega või veebisaidi lõpp-punktid jaotuse abil liikluse haldur profiilid meetodite liikluse marsruutimist. Selles artiklis selgitatakse, kuidas luua ja hallata neid profiilid.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Abil kiiresti luua liikluse haldur profiili loomine

Saate kiiresti luua liikluse haldur profiili abil kiiresti luua Azure klassikaline portaalis. Kiire loomine võimaldab teil luua lihtsa konfiguratsioonisätted profiilid. Siiski ei saa kasutada kiiresti luua sätete kogumi lõpp-punktid (pilveteenustega ja veebisaidid), Tõrkesiirde võimaldamaks Tõrkesiirde liikluse marsruutimise meetodit või jälgimise sätted, nt. Pärast profiili loomist saate konfigureerida Azure klassikaline portaali neid sätteid. Liikluse haldur toetab kuni 200 lõpp-punktid profiili kohta. Enamik kasutamise stsenaariumid nõuda vaid mõned lõpp-punktid.

### <a name="to-create-a-traffic-manager-profile"></a>Liikluse haldur profiili loomine

1. **Juurutada oma pilveteenustega ja veebisaidid tootmiskeskkonnast.** Pilveteenustega kohta leiate lisateavet teemast [Pilveteenustega](http://go.microsoft.com/fwlink/p/?LinkId=314074). Veebisaitide kohta leiate lisateavet teemast [veebisaiti](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Azure'i klassikaline portaali sisse logida.** Klõpsake nuppu **Uus** madalama vasakule portaali, klõpsake **võrguteenuste > liikluse haldur**, ja seejärel klõpsake nuppu profiili konfigureerimise alustamiseks **Kiiresti luua** .
3. **DNS-i eesliide konfigureerimine.** Pange profiili liikluse haldur kordumatu DNS-i eesliide nimi. Saate määrata ainult eesliite liikluse haldur domeeni nimi.
4. **Valige tellimus.** Valige sobiv Azure'i tellimus. Iga profiili on ühe tellimusega seostatud. Kui teil on ainult üks tellimus, seda suvandit ei kuvata.
5. **Valige liikluse marsruutimise meetod.** Valige **liikluse marsruutimist poliitika**liikluse marsruutimise meetod. Marsruutimise meetodite liikluse kohta leiate lisateavet teemast [liikluse haldur liikluse marsruutimise meetodid](traffic-manager-routing-methods.md).
6. **Klõpsake "Loo" profiili loomiseks**. Profiili konfigureerimine on lõpetatud, saate otsida oma profiili liikluse haldur paanil Azure klassikaline portaalis.
7. **Azure'i klassikaline portaalis konfigureerimine lõpp-punktid, jälgimine ja täiendavad sätted.** Abil kiiresti luua ainult konfigureerib põhisätted. See on vaja konfigureerida täiendavaid sätteid, nt lõpp-punktid ja lõpp-punkti Tõrkesiirde tellimuse loendit.


## <a name="disable-enable-or-delete-a-profile"></a>Keelata, lubada või profiili kustutamine

Olemasolevale profiilile saate keelata, nii et liikluse haldur ei viita kasutaja taotlused konfigureeritud lõpp-punktid. Kui keelate liikluse haldur profiili, profiili ja profiili sisalduvat teavet jäävad samaks ja kasutajaliidese liikluse haldur saab redigeerida.  Viited jätkata, kui profiili uuesti lubada. Azure'i klassikaline portaalis liikluse haldur profiili loomisel on see automaatselt lubatud. Kui profiili ei ole enam vaja, võite selle kustutada.

### <a name="to-disable-a-profile"></a>Profiili keelamine

1. Kui kasutate kohandatud domeeninime, muuta nii, et see oleks enam teie haldur liikluse profiilile CNAME-kirje Interneti DNS-i serverisse.
2. Liikluse lõpetab suunatud liikluse haldur profiili sätete kaudu lõpp-punktid.
3. Valige eemaldatav profiil, mida soovite keelata. Lehel liikluse Manager esile tõsta, klõpsates veeru kõrval profiili nimi profiili. Pange tähele, et nimi profiili või nime kõrval olevat noolt klõpsates avatakse profiili lehel sätted.
4. Pärast profiili valimist klõpsake lehe allosas **Keela** .

### <a name="to-enable-a-profile"></a>Profiili lubamiseks

1. Valige eemaldatav profiil, mida soovite keelata. Lehel liikluse Manager esile tõsta, klõpsates veeru kõrval profiili nimi profiili. Pange tähele, et nimi profiili või nime kõrval olevat noolt klõpsates avatakse profiili lehel sätted.
2. Pärast profiili valimist klõpsake nuppu **Luba** lehe allosas.
3. Kui kasutate kohandatud domeeninime, luua ressursi CNAME-kirje Interneti DNS-i server oma domeeni nimi profiili liikluse haldur.
4. Liikluse suunatakse uuesti lõpp-punktid.

### <a name="to-delete-a-profile"></a>Profiili kustutamine

1. Veenduge, et DNS-i ressursikirje Interneti DNS-i serverisse enam kasutab ressursi CNAME-kirje, mis viitab domeeninimi halduri liikluse profiili.
2. Valige eemaldatav profiil, mida soovite keelata. Lehel liikluse Manager esile tõsta, klõpsates veeru kõrval profiili nimi profiili. Pange tähele, et nimi profiili või nime kõrval olevat noolt klõpsates avatakse profiili lehel sätted.
3. Pärast profiili valimist klõpsake nuppu **Kustuta** lehe allosas.

## <a name="view-traffic-manager-profile-change-history"></a>Vaate liikluse haldur profiili muutuste ajalugu

Muutuste ajalugu saate vaadata oma liikluse haldur profiili Azure klassikaline portaalis teenuste haldus.

### <a name="to-view-your-traffic-manager-change-history"></a>Liikluse haldur muutuste ajaloo vaatamine

1. Klõpsake vasakul paanil Azure klassikaline portaali **Halduse teenused**.
2. Klõpsake lehel halduse teenused **Logi**.
3. Klõpsake lehel Logi saate filtreerida muutuste ajalugu teie haldur liikluse profiili vaatamiseks. Klõpsake oma filtreerimise suvandite valimise järel märge tulemuste vaatamiseks.

   - Kõigi oma profiilide muutuste vaatamiseks märkige oma tellimust ja ajavahemiku ja seejärel valige **Liikluse haldur** otseteemenüüs **Tüüp** .
   - Profiili nime järgi filtreerimiseks **Teenuse nimi** väljale profiili nimi tippige või valige see kiirmenüü.
   - Iga üksiku muutuse üksikasjade vaatamiseks valige rida muutmisega, mida soovite vaadata, ja klõpsake **üksikasjade** lehe allservas. Klõpsake aknas **Toimingu üksikasjad** saate vaadata XML-esitus API objekti, mis on loodud või värskendatud toimingu käigus.

## <a name="next-steps"></a>Järgmised sammud

- [Lõpp-punkti lisamine](traffic-manager-endpoints.md)
- [Tõrkesiirde marsruutimise meetod konfigureerimine](traffic-manager-configure-failover-routing-method.md)
- [Round jaan marsruutimise meetod konfigureerimine](traffic-manager-configure-round-robin-routing-method.md)
- [Jõudluse marsruutimise meetod konfigureerimine](traffic-manager-configure-performance-routing-method.md)
- [Osutage domeeninimi halduri liikluse ettevõtte Interneti-domeeni](traffic-manager-point-internet-domain.md)
- [Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)