<properties
   pageTitle="Hallata oma virtuaalse masina pilt Azure'i turuplatsi | Microsoft Azure'i"
   description="Üksikasjalikud juhised, kuidas hallata oma virtuaalse masina pildi Azure turuplatsilt pärast algse avaldamist."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Azure'i turuplatsil virtuaalse masina pakutakse pärast juhend

Selles artiklis selgitatakse, kuidas saate värskendada reaalajas virtuaalse masina pakkumine Azure'i turuplatsil. Lisaks juhendab teid protsessi ühe või mitme uute SKU-de lisamine olemasoleva pakkumise ja reaalajas virtuaalse masina pakkumine või SKU eemaldamine Azure'i turuplatsilt.

Kui pakkumine/SKU-ga on etapiviisilise [Azure portaali](http://portal.azure.com), ei saa muuta järgmised väljad:

- **Pakkuda identifikaator:** [Avaldamise portaali -> Virtuaalmasinates-teie pakkumise -> Vali > menüüd VM pildid -> pakkuda identifikaator]
- **SKU identifikaator:** [Avaldamise portaali -> Virtuaalmasinates-teie pakkumise -> Vali > Menüü -> SKU-de jaoks lisamiseks mõne SKU]
- **Publisheri Namespace:** [Avaldamise portaali Virtuaalmasinates -> -> -> vahekaart meile oma ettevõtte (leitud jaotises "Samm 2 registreerida teie ettevõtte") -> Teavita kiirtutvustus Publisheri Namespace -> Namespace]

Kui pakkumine/SKU-ga on loetletud [Azure'i turuplatsi](http://azure.microsoft.com/marketplace), ei saa muuta järgmised väljad:

- **Pakkuda identifikaator:** [Avaldamise portaali -> Virtuaalmasinates-teie pakkumise -> Vali > menüüd VM pildid -> pakkuda identifikaator]
- **SKU identifikaator:** [Avaldamise portaali -> Virtuaalmasinates-teie pakkumise -> Vali > Menüü -> SKU-de jaoks lisamiseks mõne SKU]
- **Publisheri Namespace:** [Avaldamise portaali -> Virtuaalmasinates -> Menüü -> kiirtutvustus öelda meile kohta teie ettevõtte (leitud jaotises etappi 2 Register) Publisheri Namespace -> Namespace]
- **Pordid** [Avaldamise portaali -> Virtuaalmasinates-teie pakkumise -> Vali > menüüd VM pildid -> avatud pordid]
- **Hinnad on loetletud SKU(s) muutmine**
- **Arved on loetletud SKU(s) mudeli muutmine**
- **Arveldus aladele loetletud SKU(s) eemaldamine**
- **Andmete kettale loetletud SKU(s) arvu muutmine**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. Kuidas värskendada on SKU tehnilised andmed

Saate lisada uue versiooni loetletud SKU ja uuesti avaldamiseks teie pakkumise allpool esitatud juhiste järgi.

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **VM pildid** .
4. **SKU-de jaoks** jaotisest menüüd **VM pildid** , otsige üles SKU-ga, mida soovite värskendada.
5. Pärast seda, lisada uue versiooni arvu kasutatava SKU ja klõpsake nuppu **"+"** . Uus versioon peaks olema X.Y.Z vorming, kus X ja Y-Z on täisarvud. Suureneva tuleks ainult versiooni muudatusi.
6. **OS VHD URL** väljale URI loodud operatsioonisüsteemi VHD ühiskasutusega juurdepääsu signatuuri lisamine ja salvestage muudatused.

    >[AZURE.IMPORTANT] Te ei saa lisandus/decrement andmete kettale arvu loetletud SKU-ga. Peate looma uue SKU sel juhul. Vaadake jaotist [3. Kuidas lisada uue SKU jaotises on loetletud pakkumine](#3-how-to-add-a-new-sku-under-a-live-offer) üksikasjalikud juhised.

7. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas lugege seda [link](marketplace-publishing-vm-image-test-in-staging.md)
8. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamine et LÜKAKE abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2 Kuidas värskendada mittetehnilistest üksikasju pakkumise või mõne SKU

Saate värskendada selle mittetehnilistest (turundus, juriidilised, toetavad, kategooriad) oma SKU Azure'i turuplatsil või reaalajas pakkumise üksikasjad.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 pakkumise kirjeldus ja logo värskendamine

Saate värskendada pakkumise üksikasjade ja uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid.

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake **TURUNDUSTEGEVUSE** menüü.
4. Klõpsake nuppu **Inglise (USA)** .
5. Klõpsake menüü vasakul küljel klõpsake vahekaarti **üksikasjad** . Menüü **andmed** jaotises *Kirjeldus* saate värskendada pakkumise pealkirja, pakub kokkuvõte, pakub pikk kokkuvõte ja muudatuste salvestamiseks.

    >[AZURE.NOTE] Hoolikalt järgmistest värskendamise ajal kuvatakse SKU üksikasjad.
    **Sisestage jaotises pakkumise kirjeldus ja SKU kirjeldus teksti. Sisestage jaotises SKU pealkiri ja pikk Kokkuvõte pakkumine teksti. Sisestage jaotises SKU pealkiri ja pakkumise Kokkuvõte teksti.**

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. Menüü **andmed** jaotises *logod* saate värskendada logod. Siiski tagada, et logod järgige [juhiseid Azure'i turuplatsi](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (vt jaotist samm 1: pakuvad turuplatsi turundusmaterjalide sisu -> üksikasjad -> Azure'i turuplatsi Logo suunised).

    >[AZURE.NOTE] Ikooni pilt pole kohustuslik. Te ei soovi üles pilt ikooni. Siiski üleslaaditud pilt ikooni, siis ei tehta kustutamiseks avalda portaal. Sel juhul peate järgima [pilt ikooni juhised](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (vt jaotist samm 1: pakuvad turuplatsi turundusmaterjalide sisu -> üksikasjad -> täiendavad juhised Hero logo banner).

7. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas palun vaadake seda [linki](marketplace-publishing-vm-image-test-in-staging.md).
8. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Värskenduse kirjeldus SKU-ga

Saate värskendada SKU üksikasjad ja uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid.

1. [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel **TURUNDUSTEGEVUSE** menüü nuppu.
4. Klõpsake nuppu **Inglise (USA)** .
5. Klõpsake menüü vasakul küljel klõpsake vahekaarti **lepingud** . **Lepingute** menüü jaotises *SKU-de jaoks* saate värskendada SKU pealkirja, SKU kokkuvõte ja SKU kirjeldus üksikasjad ja salvestage soovitud muudatused.

    >[AZURE.NOTE] Hoolikalt järgmistest värskendamise ajal kuvatakse SKU üksikasjad. **Sisestage jaotises pakkumise kirjeldus ja SKU kirjeldus teksti. Sisestage jaotises kasutatava SKU pealkiri ja pikk Kokkuvõte pakkumine teksti. Sisestage jaotises SKU pealkiri ja pakkumise Kokkuvõte teksti.**

6. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas palun vaadake Järgmine link
7. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 muuta olemasolevaid linke või uue linkide lisamine

Saate muuta olemasolevaid linke või uue linkide lisamine ja seejärel uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid:

1. [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel **TURUNDUSTEGEVUSE** menüü nuppu.
4. Klõpsake nuppu **Inglise (USA)** .
5. Klõpsake menüü vasakul küljel klõpsake vahekaarti **lingid** .
6. Kui soovite uue lingi lisamiseks, *lingid* jaotisest klõpsake nuppu **Lisa LINK** . Avab dialoogiboksi *"lisa Link"* . Selle dialoogiboksi saate lisada lingi tiitel ja URL-i välju ja salvestage soovitud muudatused. Saate sisestada mis tahes link, mis sisaldab teavet, mis võib aidata kliente.
7. Kui soovite värskendada või kustutada olemasoleva lingi, siis valige vastav link ja klõpsake nuppu Redigeeri või Kustuta.

    >[AZURE.NOTE] Veenduge, et lingid, mis on sisestatud selle jaotise töötavad õigesti, järgmiste linkide hankimine kinnitatud teie taotlus tootmisprotsessi jooksul.

8. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas palun vaadake seda [linki](marketplace-publishing-vm-image-test-in-staging.md).
9. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 muuta valimi olemasoleva pildi või lisada uue näidispilt

Mõne olemasoleva valimi piltide muutmine või uue valimi piltide lisamine ja seejärel uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid.

>[AZURE.NOTE] Ainult ühe näidise pilt kuvatakse [https://portal.azure.com](https://portal.azure.com).

1. [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel **TURUNDUSTEGEVUSE** menüü nuppu.
4. Klõpsake nuppu **Inglise (USA)** .
5. Klõpsake menüü vasakul küljel klõpsake vahekaarti **Proovi PILTIDEGA** .
6. Kui soovite lisada uue näidispilt, seejärel jaotises *Proovi piltidega* klõpsake nuppu **Laadige uus pilt** ja salvestage muudatused.

    >[AZURE.NOTE] Sh proovi pilt on valikuline samm.

7. Kui soovite värskendada või olemasoleva valimi pildi kustutamiseks ja seejärel otsige üles sobiv näidispilt ja seejärel klõpsake nuppu **Asenda pilt** või Kustuta vastavalt sellele.

8. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas palun vaadake seda [linki](marketplace-publishing-vm-image-test-in-staging.md).
9. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamine et LÜKAKE abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 juriidilise sisu värskendada.

Saate värskendada juriidilise sisu ja uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid.

1. [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel **TURUNDUSTEGEVUSE** menüü nuppu.
4. Klõpsake nuppu **Inglise (USA)** .
5. Klõpsake menüü vasakul küljel klõpsake vahekaarti **juriidiline** . *Juriidiline* jaotises saate värskendada oma poliitikate/kasutustingimused. Sisestage või kleepige poliitikate/terminite *Kasutustingimused* tekstiväli ja salvestage soovitud muudatused.
6. Märkide arvu piiramine juriidiline kasutustingimused on 1 000 000 märki.
7. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas lugege seda [link](marketplace-publishing-vm-image-test-in-staging.md)
8. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 värskendamiseks tugi

Saate värskendada tugiteenuse teabe ja uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid.

1. [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **tugi** .
4. Jaotises *Pöörduge tehnilise* **toe** menüü saate värskendada kontaktandmed. Need andmed, mida kasutatakse sisesuhtluse partnerite ja Microsofti vahel ainult.
5. Jaotises *Klienditoe* **tugiteenuste** menüü saate värskendada tugiteenuste kontaktandmed, näiteks **nimi, e-posti, telefoni** ja **Tugiteenuste URL**. Need andmed, mida kasutatakse sisesuhtluse partnerite ja Microsofti vahel ainult.

    >[AZURE.NOTE] Kui soovite sisestada vaid e-posti toe, Sisestage jaotises **Klienditugi** fiktiivne telefoninumber. Sel juhul kasutatakse selle asemel esitatud postkasti.

6. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas lugege seda [link](marketplace-publishing-vm-image-test-in-staging.md)
7. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 värskendada kategooriad

Saate värskendada oma pakkumise jaoks jaotise kategooriad ja uuesti avaldamiseks teie pakkumise, järgides allolevaid juhiseid.

1. [Avaldamise portaali](https://publish.windowsazure.com) sisse logida
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **Kategooriad** .
4. Jaotises *Kategooriad* saate värskendada oma pakkumise jaoks kategooriad ja salvestage soovitud muudatused. Saate valida kuni viis kategooriad Azure'i turuplatsi Galerii.
5. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas lugege seda [link](marketplace-publishing-vm-image-test-in-staging.md)
6. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. Kuidas lisada uue SKU jaotises on loetletud pakkumine

Allpool esitatud juhiste järgi saate lisada oma reaalajas pakkumine jaotises uute SKU:

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **SKU-de jaoks** . Seejärel klõpsake nuppu **ADD A SKU-ga**.  Avaneb dialoogiboks uus. Sisestage väiketähtedes SKU kood. Märkeruudu arvelduse model(BYOL) tuua-oma-enda jaoks kui soovite uute SKU BYOL arvelduse mudeli avaldamine. Muul juhul tühjendage ruut BYOL. Seejärel klõpsake dialoogiboksi uute SKU loomiseks märkige märgi. Kui te ei ole valida BYOL arvelduse mudel uute SKU, siis arvelduse mudel automaatselt seatakse tunni jaoks uute SKU-ga. Kui soovite lubada 30days tasuta prooviversioon kord tunnis arvelduse mudeli, seejärel klõpsake suvandi "Ühe kuu" "on tasuta prooviversioon saadaval?". Muul juhul valige "PROOVIVERSIOONI ei". [Märkus: suvandi "on saadaval tasuta prooviversioon?" kuvatakse ainult juhul, kui te pole valinud BYOL dialoogiboksi uute SKU loomisel.]

    >[AZURE.IMPORTANT] Suvand "Peida selles SKU turuplatsilt, kuna see tuleks alati osta kaudu lahenduse malli" peaks olema märgitud "Jah" ainult juhul, kui teil on kinnitatud avaldamise lahenduse malli pakkumine Azure'i turuplatsilt. Muul juhul see suvand alati märkida "Ei".

4. Nüüd klõpsake menüü vasakul küljel klõpsake menüüd **VM pildid** ja teada saada uute SKU, mille olete loonud.
5. Uute SKU häälestanud, vaadake selle [lingi](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) juhised selle etapi 5.
6. Turundus materjal uute SKU lisamiseks vt jaotist samm 1: pakuvad turuplatsi turundustegevuse sisu -> üksikasjad -> Järgmine [link](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content)punkti arvude 2 – 5.
7. Uute SKU hinnateavet lisamiseks vt jaotist 2.1. Määrake oma VM hinnad selle [link](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Pärast muudatuste tegemist avage menüü **AVALDA** ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas lugege seda [link](marketplace-publishing-vm-image-test-in-staging.md)
9. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamine et LÜKAKE abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. Kuidas muuta andmete kettale count loetletud SKU

Te ei saa lisandus/decrement andmete kettale arvu loetletud SKU-ga. Sel juhul tuleb teil on Loo uus SKU-ga. Vaadake jaotist [3. Kuidas lisada uue SKU reaalajas pakkuda](#3-how-to-add-a-new-sku-under-a-live-offer) üksikasjalikud juhised.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. Kuidas kustutada loetletud pakkumine Azure'i turuplatsilt

On jälgib, mille eest eemaldada reaalajas pakkumine taotluse korral on vaja. Järgige alltoodud juhised tugimeeskonnalt loetletud pakkumine eemaldamiseks Azure'i turuplatsi juhiseid:

1.  Tõsta tugi Piletite, kasutades seda [link](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681)
2.  Probleemi tüüp valige nimega **"Haldamine pakkumisi"** ja valige kategooria nimega **"Muutmine pakkumise ja/või SKU juba valmistamisel"**
3.  Taotluse esitamine

Tugimeeskonnal juhendab teid pakkumine/SKU kustutamise protsessi.

>[AZURE.NOTE] Saate kustutada pakkumine alati alles siis projekti olek (, mitte LAVASTUS või tootmise), klõpsates menüü **ajalugu** nuppu **HÜLGA mustand** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. Kuidas kustutada loetletud SKU Azure'i turuplatsilt

Allpool esitatud juhiste järgi saate kustutada Azure'i turuplatsilt loetletud SKU-ga.

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Paanilt vasakul küljel klõpsake vahekaardil **SKU-de jaoks** .
4. Valige SKU-ga, mille soovite kustutada, ja klõpsake nuppu Kustuta see SKU vastu.
5. Kui see on valmis, liikuge lindimenüü AVALDA avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada Azure'i turuplatsil pakkumine.
6. Kui pakkumine saab uuesti avaldatud Azure'i turuplatsi, kustutatakse kasutatava SKU Azure'i turuplatsi ja Azure portaali kaudu.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. Kuidas kustutada praeguse versiooni loetletud SKU Azure'i turuplatsilt

Allpool esitatud juhiste järgi saate kustutada Azure'i turuplatsilt loetletud SKU praeguse versiooni. Kui protsess on lõpule jõudnud, kuvatakse varasema versiooni kasutatava SKU tagasi pöörata.

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2.  Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3.  Paani vasakul küljel klõpsake menüüd **VM pildid** .
4.  Valige SKU nime, kelle soovite kustutada, ja klõpsake nuppu Kustuta selle versiooni suhtes praeguse versiooni.
5.  Kui see on valmis, liikuge lindimenüü **AVALDA** avalda portaali ja klõpsake nuppu **Taotlemine kinnitamine et LÜKAKE abil tootmise** uuesti avaldada Azure'i turuplatsil pakkumine.
6.  Kui pakkumine saab uuesti avaldatud Azure'i turuplatsi, kustutatakse praegune versioon loetletud SKU Azure'i turuplatsi ja Azure portaali kaudu. Kasutatava SKU pööratakse tagasi varasema versiooni.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. Kuidas taastada tootmise väärtuste loendi hind
Muutunud hinnad on loetletud SKU (või mul on eemaldatud arvelduse aladele loetletud SKU-ga). Kuna see ei toeta Azure'i turuplatsi, soovin taastada minu muudatuste toodangu väärtused. Kuidas saavutada seda?

Järgige alltoodud juhiseid:

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **HINNAKIRJAD** .
4. Valige vahekaardil hinnakirjad piirkonnas hinnad nime, kelle soovite lähtestada.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. Korral SKU-de jaoks kord tunnis arvelduse mudel, lähtestada kõigi südamike hinnad, kui need on valmistamisel piirkonna jaoks. SKU-de BYOL arvelduse mudel, kättesaadavaks kasutatava SKU piirkonna vastu SKU jaotises EXTERNALLY-LICENSED (BYOL) SKU-saadavus ruut kontrollides (vt pildil).

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Nüüd klõpsake nuppu **AUTOPRICE muud turgude vastavalt sees hinnad tolli Ameerika Ühendriikides**.

    >[AZURE.NOTE] Nupu sildi võivad olla erinevad sõltuvalt piirkond, mille olete valinud. Kuna oleme valinud selle dokumendi loomisel Ameerika Ühendriigid, seega nupp kirjaga nagu "Automaatne hind teiste turgude hindade Ameerika Ühendriikides" pildil.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Avatakse viisard automaatselt hind. Esimesel lehel kuvatakse valiku alus market. Tehke jaotises teie ja klõpsates nuppu **"->"** järgmisele lehele liikumine.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Kuvatakse suvand tuuma ja lepingu valimise lehel 2. Valige soovitud lepingud ja südamikud ning "->" nuppu.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Lehe 3 kuvab riikides/piirkondades. Klõpsake nuppu Lülita kõik märkige kõigi piirkondade või käsitsi märkige ruudud nende piirkond. Klõpsake järgmisele lehele liikumiseks nuppu "->".

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Lk 4 kuvatakse nende. Klõpsake juhiste lõpuleviimiseks nuppu valmis. Viisardi lähtestab hinnad valiku alusel.

11. Nüüd liikuge hinnakirjad vahekaarti ja klõpsake nuppu "Kuva kokkuvõte ja muudatused".
Valige "Projekti" jaotises "Vaade versioon" ja "Loomine" jaotises "Võrrelda" (vt pildil). Kui näete hinnakirjad vahet, see tähendab, et hinnad on taastatud toodangu väärtused edukalt.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Pärast muudatuste tegemist avage menüü AVALDA ja klõpsake nuppu **PUSH LAVASTUS**. Üksikasjalikud juhised testimine teie pakkumise lavastus keskkonnas lugege seda [link](marketplace-publishing-vm-image-test-in-staging.md)
13. Kui te oma pakkumine lavastus testinud, liikuge vahekaardile AVALDA avalda portaali ja klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. Kuidas taastada arvelduse andmemudel toodangu väärtused
Vahetasin on loetletud SKU arvelduse mudel. Kuna see ei toeta Azure'i turuplatsi, soovin taastada minu muudatuste toodangu väärtused. Kuidas saavutada seda?

Järgige alltoodud juhiseid:

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **SKU-de jaoks** .
4. Klõpsake nuppu Redigeeri tagasipööramine arvelduse mudel. Kuvatakse aken. Märkige või tühjendage ruut **'arveldamine ja litsentsid on valmis väliselt: Azure'i (ehk tuua teie enda litsents)'** vastavalt sellele.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Üks kord palun vaadake vastust küsimuse 8 tagasi pöörduda, on hinnad selles dokumendis.
6. Pärast seda, et liikuda lindimenüü **AVALDA** avalda portaali ja tõuketeatised pakkumise lavastus proovimislink. Üks kord on tehtud pakkumine testimine ja seejärel klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. Kuidas taastada nähtavuse säte loetletud SKU tootmise väärtus.

Järgige alltoodud juhiseid:

1. [Avaldamise portaali](https://publish.windowsazure.com)sisse logida.
2. Liikuge **VIRTUAALMASINATES** menüü ja valige oma pakkumine.
3. Klõpsake menüü vasakul küljel klõpsake vahekaarti **SKU-de jaoks** .
4. Valige oma SKU ja nähtavuse säte SKU tootmise väärtusega, taastada.

    ![Joonis](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Üks kord on tehtud muudatused ja seejärel klõpsake nuppu **Taotlemine kinnitamise abil PUSH abil tootmise** uuesti avaldada oma pakkumine Azure'i turuplatsil.

## <a name="see-also"></a>Vt ka
- [Alustamine: Kuidas Azure'i turuplatsi pakkumise avaldamine](marketplace-publishing-getting-started.md)
- [Aruandlusteenuste müüja ülevaateid mõistmine](marketplace-publishing-report-seller-insights.md)
- [Mõistmine väljamakse teatamine](marketplace-publishing-report-payout.md)
- [Kuidas muuta oma pilve lahenduste pakkuja edasimüüja huvi](marketplace-publishing-csp-incentive.md)
- [Kuvatakse Marketplace'ist avaldamise levinud probleemide tõrkeotsing](marketplace-publishing-support-common-issues.md)
- [Kui avaldaja tootetugi](marketplace-publishing-get-publisher-support.md)
- [Loomise VM pilt asutusesisese](marketplace-publishing-vm-image-creation-on-premise.md)
- [Töötab Windows Azure'i eelvaade portaali loomine](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
