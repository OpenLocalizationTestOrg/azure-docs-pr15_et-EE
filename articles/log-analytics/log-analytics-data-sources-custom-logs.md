<properties 
   pageTitle="Kohandatud logib Log Analytics | Microsoft Azure'i"
   description="Log Analytics koguda sündmuste tekstifailidest arvutis nii Windows ja Linux.  Selles artiklis kirjeldatakse, kuidas määratleda uue kohandatud sisse ja nende loodud OMS hoidlas kirjete üksikasjad."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-logs-in-log-analytics"></a>Kohandatud logid Log Analytics

Kohandatud logid andmeallika Log Analytics võimaldab sündmuste kogumine teksti faile arvutis nii Windows ja Linux. Paljud rakendused Logiteave asemel standard logimine teenused, nt Windowsi sündmuselogi või Logi tekstifailidest.  Kui kogunud, saate iga kirje Logi sõeluda üksikute väljade [Kohandatud väljad](log-analytics-custom-fields.md) funktsiooni Log Analyticsi abil.

![Kohandatud Logi saidikogumi](media/log-analytics-data-sources-custom-logs/overview.png)

Logifailide koguda peab vastama järgmistele kriteeriumitele.

- Logi peate olema üks kirje real või kasutada ajatempel ühte järgmistes vormingutes iga kirje alguses sobitamine.

    YYYY-MM-DD HH:MM:SS <br>
  D.M.YYYY: HH: MM: SS AM/PM <br>
  E DD, aaaa HH:MM:SS
    
- Logifaili peavad lubama ringikujulise värskenduste kuhu fail kirjutatakse uued kirjed. 

## <a name="defining-a-custom-log"></a>Kohandatud Logi määratlemine

Järgmiste toimingute abil saate määratleda kohandatud logifaili.  Sellest artiklist leiate lühiülevaade valimi lisamine kohandatud Logi lõppu kerimine

### <a name="step-1-open-the-custom-log-wizard"></a>Samm 1. Kohandatud Log viisardi avamine

Kohandatud Log viisardi OMS portaalis käivitub ja võimaldab teil määrata uue kohandatud Logi kogumiseks.

1.  OMS portaalis, avage **sätted**.
2.  Klõpsake **andmeid** ja seejärel **kohandatud logid**.
3.  Vaikimisi lükata kõik konfiguratsioonimuudatuste automaatselt kõigile.  Fluentd Andmekoguja saadetakse agentide Linuxi jaoks konfigureerimise faili.  Kui soovite muuta selle faili käsitsi iga Linux agent, siis tühjendage ruut *Rakenda all minu Linux masinad konfigureerimine*.
4.  Klõpsake nuppu **Add +** kohandatud Log viisardi avamiseks.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Samm 2. Üles- ja sõeluda valimi Logi

Alustate üleslaadimise valimi kohandatud Logi.  Viisardi sõelub ja selle faili jaoks valideerimiseks kirjete kuvamine.  Log Analytics kasutab iga kirje tuvastamiseks määratud eraldaja.

**Uue rea** on vaikimisi eraldaja ja kasutatakse logi faile, mis on ühe kirje real.  Kui rea alguses on kuupäev ja kellaaeg ühel saadaolevate arvuvormingute, saate määrata **ajatempli** eraldaja, mis toetab rohkem kui ühe rea ulatuvad kirjed. 

Kui ajatempli eraldajat kasutatakse, siis iga kirje, mis on talletatud OMS atribuudi TimeGenerated on täidetud määratud logifaili kirje kuupäev/kellaaeg.  Kui uus rida eraldajat kasutatakse, siis TimeGenerated lisatakse kuupäev ja kellaaeg, et Log Analyticsi kogutud kirje. 

>[AZURE.NOTE]Log Analytics käsitleb praegu Logi UTC ajatempli eraldaja abil kogutud kuupäev/kellaaeg.  See kiiresti muuta kasutamine agent ajavöönd. 
 
1.  Klõpsake nuppu **Sirvi** ja liikuge sirvides näidisfaili.  Pange tähele, et see võib nuppu võib olla nimega **Faili valimine** mõnes brauseris.
2.  Klõpsake nuppu **edasi**. 
3.  Kohandatud Log Wizard laadige fail üles ja loendi kirjed, mille see tuvastab.
4.  Muutke eraldaja, mida kasutatakse tuvastamine uus kirje ja valige eraldaja, mis sobib eelkõige tuvastab värskendatavad kirjed teie logifaili.
5.  Klõpsake nuppu **edasi**.

### <a name="step-3-add-log-collection-paths"></a>Samm 3. Logi saidikogumi teed lisamine

Peate määratlema ühe või mitme teed agent, kus on võimalik leida kohandatud Logi sisse.  Kas saate sisestada kindla tee ja logifaili nimi või saate selle tee metamärgina nimi.  See toetab uue faili loomine iga päev või kui ühe faili jõuab kindla suurusega rakendusi.  Saate sisestada ka mitu teed puhul luuakse üks logifail.

Näiteks rakenduse luua logifail iga päev sisaldab nime nagu log20100316.txt kuupäevaga. Mustri sellist Logi jaoks võib olla *log\*.txt* mis tuleks rakendada mis tahes logifaili pärast kasutaja nime panemise värviskeemi.

Järgmises tabelis antakse ülevaade mõne kehtiv mustrite määramiseks muu logifailid. 

| Kirjeldus | Tee |
|:--|:--|
| Kõik failid *C:\Logs* .txt-laiendiga Windows agent | C:\Logs\\\*.txt |
| Kõik failid *C:\Logs* nimega alustades log ja .txt laiend Windows agent | C:\Logs\log\*.txt |
| Kõik failid */var/log/audit* .txt-laiendiga Linux agent | /var/log/audit/*.txt |
| Kõik failid */var/log/audit* alustades log ja Linux agent laiendi .txt nimega | /var/log/audit/log\*.txt |
  

1.  Valige Windowsi või Linuxi määrata, millist tee vormingut soovite lisada.
2.  Tippige soovitud tee ja valige soovitud **+** nupp.
3.  Korrake toimingut iga täiendava teed.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Samm 4. Sisestage nimi ja kirjeldus Logi

Eespool kirjeldatud, kasutatakse teie määratud nimi log tüüp.  Alati lõpeb _CL eristada seda kohandatud log.

1.  Tippige nimi Logi sisse.  Funktsiooni ** \_CL** järelliite esitataks automaatselt.
2.  Lisage soovi korral ka **Kirjeldus**.
3.  Klõpsake nuppu **Järgmine** kohandatud log määratluse salvestada.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Juhis 5. Kinnitage kohandatud logid kogutakse
Võib kuluda kuni tund lähteandmete uue kohandatud Logi Log Analytics kuvada.  See algab kogumiseks kirjete kaudu leitud tee logid teie määratud punkti määratletud kohandatud Logi.  See ei Säilita kohandatud log loomise ajal üleslaaditud kirjeid, kuid kogub juba olemasolevate kirjete see otsib logifaile.

Kui Log Analytics hakkab, kohandatud Logi kogumiseks, saab selle kirjete Log otsing.  Kasutage andis päringu **Tüüp** kohandatud logi nime.

>[AZURE.NOTE] Kui atribuudi RawData puudub otsing, peate sulgeda ja uuesti avada brauseris.

### <a name="step-6-parse-the-custom-log-entries"></a>Juhist 6. Kohandatud logi kannete sõeluda

Kogu Logi kirje salvestatakse ühe atribuudiga **RawData**.  Tõenäoliselt soovite eraldada erinevate osade iga üksiku kirje talletatud atribuudid sisenemise teave.  Mida teha, kasutades [Kohandatud väljad](log-analytics-custom-fields.md) funktsiooni Log Analyticsi.

Üksikasjalikud juhised sõelumine kohandatud Logi kirje siin on esitatud.  Vaadake seda teavet [Kohandatud väljad](log-analytics-custom-fields.md) dokumentatsioonist.

## <a name="disabling-a-custom-log"></a>Kohandatud Logi keelamine

Kohandatud log määratlus ei saa eemaldada, kui see on loodud, kuid saate selle keelata, eemaldades kõik selle saidikogumi teed.

1.  OMS portaalis, avage **sätted**.
2.  Klõpsake **andmeid** ja seejärel **kohandatud logid**.
3.  Kohandatud log määratluse keelamiseks kõrval nuppu **üksikasjad** .
4.  Iga saidikogumi teed kohandatud log määratluse eemaldada.


## <a name="data-collection"></a>Andmete kogumine

Log Analytics kogub uued kirjed iga kohandatud log ligikaudu iga 5 minuti järel.  Agent, siis salvestab iga kaudu kogutud logifaili oma kohale.  Agent läheb ühenduseta teatud aja jooksul, siis Log Analytics kogub kirjeid, kus see viimati pooleli jäi, isegi juhul, kui need kirjed on loodud agent võrguühenduseta oleku ajal.

Logi kirje kogu sisu on kirjutatud ühe atribuudiga **RawData**.  Saate seda sõeluda mitme atribuudid, mida saab analüüsida ja otsida eraldi, määratledes [Kohandatud välju](log-analytics-custom-fields.md) , kui olete loonud kohandatud Logi.


## <a name="custom-log-record-properties"></a>Kirje kohandatud atribuudid

Kohandatud kirjed on teie logi nime ja atribuutide tüüp järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| TimeGenerated | Kuupäev ja kellaaeg, Log Analytics kogutud kirje.  Kui Logi kasutab ajaline eraldaja siis on kirje kogutud aeg. |
| SourceSystem  | Kirje tüüp on kogutud. <br> OpsManager – Windows agent, kas otse ühendust luua või SCOM <br> Linux – kõik Linuxi agentide  |
| RawData             | Täielik kogutud kirje tekst. |
| ManagementGroupName | SCOM agentide haldus rühma nimi.  Muude töötajatega, see on AOI -\<tööruumi ID\> |


## <a name="log-searches-with-custom-log-records"></a>Log otsinguid kohandatud kirjed

Kohandatud logid kirjed on talletatud OMS hoidla nii nagu mis tahes muust andmeallikast kirjeid.  Kui määratlete log, nii et saate atribuudi tüüp otsingus tuua kogutud teatud Logi kirjed varustavate nimele vastavad tüüp on neil.

Järgmises tabelis leiate muu näited log otsinguid, et tuua kohandatud logid kirjed.

| Päringu | Kirjeldus |
|:--|:--|
| Tüüp = MyApp_CL | Kohandatud kõigi sündmuste logi nimega MyApp_CL. |
| Tüüp = MyApp_CL Severity_CF = tõrge | Kõik sündmused kohandatud Logi nimega MyApp_CL väärtus on *tõrge* kohandatud väli nimega *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Valimi kiirtutvustus lisamine kohandatud Logi

Järgmises jaotises antakse ülevaade olulisematest punktidest näiteks luua kohandatud Logi.  Kogutud valimi Logi on üks kirje igal real, alustades kuupäev ja kellaaeg ja seejärel komaga eraldatud väljade kood, olek ja teade.  Mitme valimi kirjed on näidatud allpool.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Üles- ja sõeluda valimi Logi

Pakume ühte logifailid ja sündmusi, mis on koguda näevad.  Sel juhul uus rida on piisavalt eraldaja.  Kui üks kirje Logi võib ühendada mitu rida küll, siis ajatempli eraldaja kasutada.

![Üles- ja sõeluda valimi Logi](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Logi saidikogumi teed lisamine

Logifailide asub *C:\MyApp\Logs*.  Uue faili luuakse nimi, mis sisaldab kuupäeva mustri *appYYYYMMDD.log*iga päev.  Oleks piisavalt mustri selle logi *C:\MyApp\Logs\\\*.log*.

![Logi saidikogumi tee](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Sisestage nimi ja kirjeldus Logi

Kasutame *MyApp_CL* ja tippige nimi, **Kirjeldus**.

![Logi nime](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Kinnitage kohandatud logid kogutakse

Kasutame päringu *Tüüp = MyApp_CL* abil kogutud Logi kõik kirjed.

![Log päring pole kohandatud väljadega](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Kohandatud logi kannete sõeluda

Kasutame kohandatud väljad määratleda *EventTime*, *kood*, *oleku*ja *sõnumi* väljad ja me näeme päringu tagastatud kirjete vahe.

![Logi päringu kohandatud väljadega](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Järgmised sammud

- [Kohandatud välju](log-analytics-custom-fields.md) abil saate kirjeid kohandatud Logi sõeluda üksikuid välju.
- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks. 