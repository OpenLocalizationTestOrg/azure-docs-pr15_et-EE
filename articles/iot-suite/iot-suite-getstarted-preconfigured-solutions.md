<properties
    pageTitle="Eelkonfigureeritud lahenduste alustamine | Microsoft Azure'i"
    description="Järgige selles õppetükis saate teada, kuidas Azure'i asjade komplekti eelkonfigureeritud lahenduse juurutamiseks."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Õpetus: Alustamine eelkonfigureeritud lahendused

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade komplekti [eelkonfigureeritud lahenduste] [ lnk-preconfigured-solutions] ühendada mitu Azure'i asjade teenuste lõpuni lahendusi, mis levinud asjade äriprotsessid rakendada. Lahendus *jälgida* eelkonfigureeritud loob ühenduse ja jälgib seadmetes. Lahenduse saate andmeid oma seadmest voo analüüsimiseks ja parandada ettevõtte tulemusi, muutes selle andmevoo automaatselt vastamine protsesside.

Selle õpetuse näidatakse, kuidas ette valmistada järelevalve Remote'i eelkonfigureeritud lahendus. See ka juhatab teid läbi põhijooned remote jälgimise lahendus. Pääsete paljude need funktsioonid, mis kasutab koos eelkonfigureeritud lahendus lahendus armatuurlaua kaudu:

![Jälgida eelkonfigureeritud lahenduse armatuurlaud][img-dashboard]

Selle õpetuse tegemiseks peate Azure aktiivne tellimus.

> [AZURE.NOTE]  Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Lahendus armatuurlaua kuvamine

Lahendus armatuurlaua võimaldab teil hallata juurutatud lahendus. Näiteks saate vaadata telemeetria, seadmete lisada ja konfigureerida reeglid.

1.  Kui soovitud ettevalmistamise on lõpule viidud ja teie eelkonfigureeritud lahendus paani näitab, **kas olete valmis**, klõpsake **käivitage** remote jälgimisega seotud lahenduse portaali avamiseks uuel vahekaardil.

    ![Käivitage eelkonfigureeritud lahendus][img-launch-solution]

2.  Vaikimisi esitab portaali lahendus *lahendus armatuurlaud*. Saate muude vaadete abil vasakpoolses menüüs.

    ![Jälgida eelkonfigureeritud lahenduse armatuurlaud][img-dashboard]

Armatuurlaua kuvatakse järgmine teave:

- Kaardi kuvatakse iga lahenduse ühendatud seadme asukoht. Lahendus esmakordsel käivitamisel on neli jäljendatud seadmed. Nagu Azure'i WebJobs rakendatakse jäljendatud seadmete ja lahendus kasutab Bingi kaartide API diagrammile teabe kaardil.
- **Telemeetria ajaloo** paan kujundab niiskuse ja temperatuuri telemeetria lähedal reaalajas ja kuvab koondandmete, nt maksimaalne, minimaalne ja Keskmine niiskus valitud seadme kaudu.
- Paani **Vaatamine** näitab tehtud alarmsündmused kui telemeetria väärtus on ületab läve. Saate määratleda oma alarmid Lisaks näidetes loodud eelkonfigureeritud lahendus.

## <a name="view-the-device-list"></a>Seadme loendi vaatamiseks

Seadmete loend kuvatakse registreeritud seadmed lahendus. Saate vaadata ja seadme metaandmete redigeerimine, lisada või eemaldada seadmete ja käsud saada seadmed.

1.  Klõpsake vasakpoolses menüüs *seadmete loend* seda lahendust **seadmed** .

    ![Armatuurlaua loendis seade][img-devicelist]

2.  Seadmete loend näitab, et on loodud ebausaldusväärsete neli jäljendatud seadmed.

3.  Klõpsake loendis seade seadme üksikasjade vaatamiseks seadme.

    ![Armatuurlaua mobiilsideseadme üksikasjad][img-devicedetails]

**Mobiilsideseadme üksikasjad** paani sisaldab kolm jaotist.

- Jaotises **toimingud** on loetletud toimingud, mida saab teha seadme. Kui keelate seadme, ei ole enam lubatud telemeetria saata või vastu võtta käske. Kui keelate seadme, saate seda siis uuesti lubada. Saate lisada reegli seostatud seade, mis käivitab helisignaal, kui telemeetria väärtus ületab läve. Lisaks saate saata käsu seade. Kui seade, loob esmalt, see ütleb lahendus käsud, saavad vastata.
- **Seadme atribuudid** jaotises loetletud metaandmete seade. Osa selle metaandmete pärineb seadet (nt tootja) ja mõned on loodud lahendus (nt loodud aeg). Saate redigeerida seadme metaandmete siit.
- **Autentimise klahvid** jaotises on loetletud autentimiseks lahenduse saate kasutada seadme võtmed.

## <a name="send-a-command-to-a-device"></a>Käsu saada seadme

Seadme üksikasjade paanil kuvatakse kõik käsud konkreetse seadme toetava ja võimaldab teil saata käske seadmega. Kui seadme käivitumisel saadab teavet käsud, mis toetab lahenduse kasutamist.

1.  Klõpsake **käske** seadme üksikasjade paanil valitud seadme.

    ![Armatuurlaua seadme käsud][img-devicecommands]

2.  Valige loendist käsk **PingDevice** .

3.  Valige **käsk saada**.

4.  Käsk käsu ajaloo olekut saate vaadata.

    ![Armatuurlaua käsk olek][img-pingcommand]

Lahendus jälitab iga käsu saadab olekut. Algselt tulem on **pooleli**. Kui seade aruanded, kas see on täidetud käsu, tulemus on seatud **edu**.

## <a name="add-a-new-simulated-device"></a>Lisa uus jäljendatud seade

Eelkonfigureeritud lahenduse juurutamisel automaatselt ette neli valimi seadmete näete seadme loendis. Need seadmed on *jäljendatud seadmed* on Azure WebJob töötab. Jäljendatud seadmete hõlbustavad eksperimenteerida eelkonfigureeritud lahendus vajamata juurutamiseks real, füüsilise seadmed. Kui soovite ühendage seade otse lahendus, vt [ühenduse loomine Remote seadme jälgimise eelkonfigureeritud lahendus] [ lnk-connect-rm] õpetuse.

Järgmised toimingud näitab teile, kuidas lisada jäljendatud seadme lahendus:

1.  Liikuge tagasi loendis seade.

2.  Klõpsake nuppu **+ Add A Device** alumises vasakus nurgas seadme lisamiseks.

    ![Eelkonfigureeritud lahendus seadme lisamine][img-adddevice]

3.  Klõpsake nuppu **Lisa uus** **Modelleerida seadme** paani.

    ![Määrake armatuurlaua uue mobiilsideseadme üksikasjad][img-addnew]
    
    Lisaks saate luua uue jäljendatud seadme, võite sisestada ka füüsilise seadmega kui otsustate luua **Kohandatud seade**. Lisateavet lahenduse füüsiliste seadmete ühenduse loomise kohta leiate teemast [ühenduse loomine remote jälgimise eelkonfigureeritud lahendus asjade komplektile seadme][lnk-connect-rm].

4.  **Andke oma seadme ID määratlemine**valige ja sisestage kordumatu seadme ID nimi, nt **mydevice_01**.

5.  Klõpsake nuppu **Loo**.

    ![Salvestage uus seade][img-definedevice]

6. Klõpsake **Lisa jäljendatud seade**sammus 3 **valmis** naasmiseks loendis seade.

7. Saate vaadata teie seade **töötab** seadme loendis.

    ![Vaade uus seade seadme loendis][img-runningnew]

8. Saate vaadata ka jäljendatud telemeetria armatuurlaual uue seadme kaudu:

    ![Vaate telemeetria uue seadme kaudu][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Seadme metaandmete redigeerimine

Kui seade loob esmalt lahenduse, saadab metaandmete lahenduse kasutamist. Seadme metaandmete lahenduse armatuurlaua redigeerimisel saadab uue metaandmete väärtused seade ja talletab lahenduse DocumentDB andmebaasi uued väärtused. Lisateavet leiate teemast [seadme identiteedi registri ja DocumentDB][lnk-devicemetadata].

1.  Liikuge tagasi loendis seade.

2.  Valige uude seadmesse **Seadmete loend**ja seejärel klõpsake nuppu **Redigeeri** **Seadme atribuutide**redigeerimiseks:

    ![Seadme metaandmete redigeerimine][img-editdevice]

3. Kerige alla ja muuta ja vales. Klõpsake nuppu **seadme registri muudatuste salvestamine**.

    ![Seadme metaandmete redigeerimine][img-editdevice2]

4. Liikuge tagasi armatuurlaud, seadme asukoht on muutunud kaardil:

    ![Seadme metaandmete redigeerimine][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Uue seadme reegli lisamine

Puuduvad reeglid uue seadme just lisasite. Selles jaotises saate lisada reegli, mis käivitab helisignaal, kui uuele seadmele esitatud temperatuur ületab 47 kraadi. Enne alustamist, Pange tähele, et ajaloo telemeetria armatuurlaual uue seadme kuvatakse seadme temperatuur ei tohi ületada 45 kraadi.

1.  Liikuge tagasi loendis seade.

2.  Valige uue seadme **Seadmete loend**ja seejärel klõpsake nuppu **Lisa reegel** seadme reegli lisamine.

3. Saate luua reegli, mis kasutab **temperatuur** andmeväli ja kasutab **AlarmTemp** väljund kui temperatuur ületab 47 kraadi:

    ![Seadme reegli lisamine][img-adddevicerule]

4. Klõpsake nuppu **Salvesta ja Kuva reeglite** muudatuste salvestamiseks.

5.  Klõpsake **käske** uus seade seadme üksikasjade paanil.

    ![Seadme reegli lisamine][img-adddevicerule2]

6.  Valige loendist käsk **ChangeSetPointTemp** ja 45 **SetPointTemp** määramine. Seejärel klõpsake **Käsku Saada**.

    ![Seadme reegli lisamine][img-adddevicerule3]

7.  Liikuge tagasi lahenduse armatuurlaud. Mõne aja pärast kuvatakse uue kirje paanil **Vaatamine** , kui esitatud uude seadmesse temperatuur ületab läve 47-kraadise.

    ![Seadme reegli lisamine][img-adddevicerule4]

8. Saate vaadata ja redigeerida oma reeglid armatuurlaua lehel **reeglid** :

    ![Loendi seadme reeglid][img-rules]

9. Saate vaadata ja redigeerida kõiki toiminguid, mida võib võtta vastuseks reegli armatuurlaua lehel **toimingud** :

    ![Seadme loenditoimingud][img-actions]

> [AZURE.NOTE] See on võimalik määrata toiminguid, mida saate meilisõnumeid saata meilisõnumile või SMS vastuseks reegli või integreerida ärivaldkonna süsteemis või [Loogika rakenduse]kaudu[lnk-logic-apps]. Lisateabe saamiseks lugege teemat [ühenduse loogika rakendus oma Azure'i asjade komplekti jälgida eelkonfigureeritud lahenduse][lnk-logicapptutorial].

## <a name="other-features"></a>Muud funktsioonid

Abil saate otsida lahenduse portaali teatud omadustega, nt mudeli number seadmete jaoks:

![Seadme otsimine][img-search]

Saate keelata seadme ja kui see on keelatud, saate selle eemaldada.

![Keelake ja seadme eemaldamine][img-disable]

## <a name="behind-the-scenes"></a>Taustal

Eelkonfigureeritud lahenduse juurutamisel juurutuse protsess loob mitme ressursid Azure tellimuse valitud. Saate vaadata nende ressursside Azure [portaali][lnk-portal]. Juurutamise protsessi loob **ressursirühm** nime järgi saate valida oma eelkonfigureeritud lahenduse nimega:

![Eelkonfigureeritud lahenduse Azure'i portaalis][img-portal]

Iga ressursi sätete vaatamiseks valige see loendist ressursside ressursirühma.

Saate vaadata ka lähtekoodi eelkonfigureeritud lahendus. Remote jälgimisega seotud eelkonfigureeritud lahenduse kood on [asjade azure remote jälgimise] [ lnk-rmgithub] GitHub hoidla:

- **DeviceAdministration** kaust sisaldab armatuurlaua.
- **Simulaatorit** kaust sisaldab jäljendatud seade.
- **EventProcessor** kaust sisaldab tagaandmebaas protsessi, mis tegeleb sissetulevate telemeetria.

Kui olete lõpetanud, saate kustutada eelkonfigureeritud lahenduse [azureiotsuite.com] Azure tellimusest[ lnk-azureiotsuite] saidi. See sait võimaldab teil hõlpsasti kustutada ressursse, mis olid ette valmistatud eelkonfigureeritud lahenduse loomisel.

> [AZURE.NOTE] Tagamaks, et kustutate kõik seotud eelkonfigureeritud lahenduse, kustutage see [azureiotsuite.com] [ lnk-azureiotsuite] saidi ja lihtsalt kustutada ressursirühm portaalis.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui juurutamist töötamise eelkonfigureeritud lahenduse, saate jätkata alustamine asjade komplekti lugedes järgmistest artiklitest:

- [Jälgida eelkonfigureeritud lahenduse kiirtutvustus][lnk-rm-walkthrough]
- [Ühendage seade järelevalve Remote'i eelkonfigureeritud lahendus][lnk-connect-rm]
- [Azureiotsuite.com saidiõigused][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
