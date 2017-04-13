<properties
    pageTitle="Rakenduse avaldamine remote kobar Visual Studio abil | Microsoft Azure'i"
    description="Saate teada, kuidas avaldada remote teenuse struktuuri kobar rakenduse Visual Studio abil."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Avaldamine remote kobar rakenduse Visual Studio abil

> [AZURE.SELECTOR]
- [PowerShelli](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Azure teenuse struktuuri laiend Visual Studio pakub lihtne, korduvate ja Skriptattavat viis rakenduse teenuse struktuuri arvutikobaras avaldada.

## <a name="the-artifacts-required-for-publishing"></a>Nõutav avaldamise esemeid

### <a name="deploy-fabricapplicationps1"></a>FabricApplication.ps1 juurutamine

See on PowerShelli skripti, mis kasutab avalda profiil teed parameetrina avaldamise teenuse struktuuri rakendusi. Kuna see skript on rakenduse osa, olete oodatud muutmiseks vastavalt vajadusele rakenduse.

### <a name="publish-profiles"></a>Avaldamine profiilid

Teenuse struktuuri rakendus projekti nimega **PublishProfiles** kausta sisaldab XML-failid, kus talletatakse olulise teabe avaldamiseks rakenduse, näiteks:

- Teenuse struktuuri kobar ühenduse parameetrid
- Rakenduse parameetri faili tee
- Täiendamise sätted

Vaikimisi rakenduse kaasata kaks avaldamine profiilid: Local.xml ja Cloud.xml. Saate lisada rohkem profiilid, kopeerides ja kleepides ühe vaikimisi faili.

### <a name="application-parameter-files"></a>Rakenduse parameetri failid

Teenuse struktuuri rakendus projekti nimega **ApplicationParameters** kausta sisaldab XML-i failid kasutaja määratud rakenduse manifest parameetrite väärtused. Rakenduse manifestifailidele saate parameetriga nii, et saate kasutada erinevate väärtuste juurutamise sätted. Parameterizing rakenduse kohta leiate lisateavet teemast [mitme keskkondade teenuse struktuuri haldamine](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Näitleja teenuste, tuleks koostada projekti esimene enne katset toimetaja või avalda dialoogiboksi kaudu faili redigeerimiseks. See on osa manifest-failid luuakse ajal koostamine.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Avaldada rakenduse avaldamine struktuuri teenuserakenduse dialoogiboksi kaudu

Järgmised sammud kirjeldavad kuidas avaldada rakenduse Visual Studio teenuse struktuuri tööriistad esitatud **Avaldamine struktuuri teenuserakenduse** dialoogiboksi abil.

1. Kiirmenüü struktuuri teenuserakenduse projekti, valige **Avalda...** **Avaldamine struktuuri teenuserakenduse** dialoogiboksi kuvamiseks.

    ![Funktsiooni ** avaldamine teenuse struktuuri rakenduse ** dialoogiboks][0]

    Valitud ripploendiboksi **Target profiili** fail on, kuhu salvestatakse kõik sätted, **näidata versioonid**, v.a. Saate uuesti kasutada olemasolevale profiilile või looge uus, valides käsu **< haldamine profiili >** **Target profiili** ripploendiboksi. Kui klõpsate nuppu Avalda profiili, kuvatakse selle sisu dialoogiboksi vastavaid välju. Muudatuste salvestamiseks valige **Salvesta profiili** link.    

2. Määrake jaotises **ühenduse lõpp-punkti** kohaliku või kaugandmebaasiga teenuse struktuuri klaster 's avaldamise lõpp-punkti. Lisada või muuta ühenduse lõpp-punkti, klõpsake ripploendi **Ühenduse lõpp-punkti** . Loendis kuvatakse saadaval teenuse struktuuri kobar ühenduse lõpp-punktid millele saate avaldada oma Azure'i subscription(s) põhjal. Pange tähele, et kui te pole veel sisse loginud Visual Studio, teil palutakse seda teha.

    Valige saadaolevate tellimuste ja kogumite kobar valiku dialoogiboksi abil.

    ![Funktsiooni ** dialoogiboksis valige teenuse struktuuri kobar **][1]

    >[AZURE.NOTE] Kui soovite avaldamiseks suvalise lõpp-punkti (nt tootja klaster), vt allpool olevat jaotist **avaldamise lõpp suvalise kobar** .

    Kui valite lõpp, Visual Studio kinnitatakse valitud teenuse struktuuri kobar ühendus. Kui klaster pole turvaline, Visual Studio ühendamiseks seda kohe. Kui klaster on turvaline, peate enne jätkamist kohalikus arvutis serdi installimine. Lisateabe saamiseks vaadake [konfigureerimine turvalist ühendust](service-fabric-visualstudio-configure-secure-connections.md) . Kui olete lõpetanud, klõpsake nuppu **OK** . Valitud kobar kuvatakse dialoogiboksis **Struktuuri rakenduse avaldamine** .

3. **Rakenduse parameetri faili** ripploendiboksi, otsige rakendus parameetri faili. Rakenduse parameetri failina hoiab parameetrite väärtused kasutaja määratud rakenduse Avaldamisfail. Lisamiseks või muutmiseks parameeter, klõpsake nuppu **Redigeeri** . Lisage või muutke parameetriväärtus **parameetrite** ruudustiku. Kui olete lõpetanud, klõpsake nuppu **Salvesta** .

    ![Funktsiooni ** dialoogiboksis Redigeeri parameetrite **][2]

4. **Rakenduse täiendamine** ruut abil saate määrata, kas see avaldamine toiming on versiooniuuendus. Uuele versioonile avaldamine toimingud tavaline erinevad toimingud avaldavad. Erinevused loendi leiate [Teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md) . Versioonitäienduse sätete konfigureerimiseks valige link **Versioonitäienduse sätete konfigureerimine** . Versioonitäienduse parameetri redaktori kuvatakse. Vaadake lisateavet versioonitäienduse parameetrite [konfigureerimine teenuse struktuuri rakenduse täiendamine](service-fabric-visualstudio-configure-upgrade.md) .

5. Valige **näidata versioonid...** nupp **Redigeeri versioonide** dialoogiboksi kuvamiseks. Peate värskendama, rakenduse ja teenuse versiooni täiendamiseks toimuda. Vt [teenuse struktuuri rakenduse täiendamine õpetuse](service-fabric-application-upgrade-tutorial.md) saate teada, kuidas rakenduste ja teenuste näidata mõju versiooni täiendamise käigus.

    ![Funktsiooni ** dialoogiboksis Redigeeri versioonide **][3]

    Kui rakenduste ja teenuste versioonide kasutamine semantilise Versioonimine, nt 1.0.0 või arvväärtusi 1.0.0.0 vormingus, valige suvand **rakenduste ja teenuste versioonid automaatselt värskendada** . Kui valite selle suvandi, teenuse ja rakenduse versiooni numbrit värskendatakse automaatselt iga kord, kui koodi, config või andmed paketti versioon on värskendatud. Kui soovite muuta versioonid käsitsi, tühjendage märkeruut selle funktsiooni keelata.

    >[AZURE.NOTE] Kõik paketi kirjed kuvatakse näitleja projekti esmalt koostada projekti kirjeid luua teenuse näidata failid.

6. Kui olete lõpetanud määrata kõik vajalikud sätted, klõpsake nuppu **Avalda** rakenduse valitud teenuse struktuuri klaster avaldada. Sätted, mis on rakendatud avalda protsess.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Avaldamine suvalise kobar lõpp-punkti (sh tootja kogumite)

Visual Studio avaldamise kogemus on optimeeritud remote kogumite mõne Azure tellimusega seotud avaldamine. Siiski, on võimalik avalda suvalise lõpp-punktid (nt teenuse struktuuri tootja kogumid), redigeerides otse avalda profiili XML-i. Eespool kirjeldatud kaks avaldamine profiilid pakuvad vaikimisi--**Local.xml** ja **Cloud.xml**--, kuid Tere tulemast luua täiendavaid profiilid viibite. Näiteks võite luua profiili tootja kogumite, võib-olla nimega **Party.xml**avaldamine.

Kui loote turvamata kobar, kõik, mida on vaja on kobar ühenduse endpoint, näiteks `partycluster1.eastus.cloudapp.azure.com:19000`. Sel juhul tuleks ühenduse lõpp-punkti avalda profiilis välja nägema umbes selline:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Kui loote ühenduse turvalise kobar, peate ka kliendi sert kohaliku poest kasutatakse autentimise üksikasjad. Lisateavet leiate teemast [seadistamine turvalist ühendust teenuse struktuuri arvutikobaras](service-fabric-visualstudio-configure-secure-connections.md).

  Kui profiili avalda on häälestatud, saate otsida seda dialoogiboksi avalda nagu allpool näidatud.

  ![Uus profiil avaldamine rakenduses dialoogiboks avaldamine][4]

  Pange tähele, et sel juhul avalda uue profiili viitab üks vaikimisi rakenduse parameetri faile. See on sobiv, kui soovite sama rakenduse konfigureerimise avaldamiseks mitmeid keskkonnas. Seevastu juhul, kuhu soovite eri konfiguratsioone iga keskkonnas, mille soovite avaldada, oleks mõistlik vastava rakenduse parameetri faili loomiseks.

## <a name="next-steps"></a>Järgmised sammud

Pidev integratsioon keskkonnas automatiseerida avaldamise kohta leiate teavet teemast [teenuse struktuuri pidev integreerimise häälestada](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
