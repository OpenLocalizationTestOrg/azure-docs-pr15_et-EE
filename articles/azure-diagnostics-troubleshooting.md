<properties
    pageTitle="Azure'i diagnostika tõrkeotsing"
    description="Tõrkeotsing Azure'i pilveteenustega, Virtuaalmasinates Azure diagnostika kasutamisel ja "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure'i diagnostika tõrkeotsing
Tõrkeotsingu teavet Azure'i diagnostika abil. Azure'i diagnostika kohta leiate lisateavet teemast [Azure diagnostika ülevaade](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure'i diagnostika ei käivitu

Diagnostika koosneb kahest osast: külaline agent lisandmoodul ja jälgimisega seotud agent. Saate kontrollida logifailide **DiagnosticsPluginLauncher.log** ning **DiagnosticsPlugin.log** miks diagnostika ei käivitu teavet.  
  
Pilveteenuses roll, külaline agent lisandmooduli logifailid asuvad: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

Klõpsake soovitud Azure virtuaalse masina, logifailid külaline agent lisandmooduli asuvad: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Logifailide Viimane rida sisaldab välju kood.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Selle lisandmooduli annab väljumist järgmisi koode:

Väljuge kood|Kirjeldus
---|---
0|Edu.
-1|Üldine tõrge.
-2|Ei saa laadida korduvkasutatavate fail.<p>See on sisemine tõrge, mida tuleks ainult siis, kui külaline agent lisandmooduli käivitit käsitsi rakendatakse, valesti, VM.
-3|Konfiguratsioonifail diagnostika ei saa laadida.<p><p>Lahendus: See on tulemus õigest skeemi valideerimine konfiguratsioonifail. Lahendus on pakkuda konfiguratsioonifail, mis vastab skeemiga.
-4|Jälgimisega seotud agent diagnostika muus eksemplaris juba kasutab kohaliku ressursi kataloogi.<p><p>Lahendus: Määrake muu väärtuse **LocalResourceDirectory**.
-6|Külalisena agent lisandmooduli käivitit proovitakse uuesti käivitada diagnostika sobimatu mõne käsurea.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui külaline agent lisandmooduli käivitit käsitsi rakendatakse, valesti, VM.
-10|Diagnostika lisandmooduli väljunud töötlemata erandi.
-11|Külalisena agent ei saanud luua protsessi käivitamine ja jälgimisega seotud agent jälgimine.<p><p>Lahendus: Veenduge, et süsteemi piisavalt ressursse on saadaval uute protsesside käivitamiseks.<p>
-101|Vigaste argumendid helistamisel diagnostika lisandmoodul.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui külaline agent lisandmooduli käivitit käsitsi rakendatakse, valesti, VM.
-102|Lisandmooduli protsess on ei saa ise lähtestada.<p><p>Lahendus: Veenduge, et süsteemi piisavalt ressursse on saadaval uute protsesside käivitamiseks.
-103|Lisandmooduli protsess on ei saa ise lähtestada. Konkreetselt ei saa luua puuraidur objekti.<p><p>Lahendus: Veenduge, et süsteemi piisavalt ressursse on saadaval uute protsesside käivitamiseks.
-104|Ei saa laadida antud külaline agent korduvkasutatavate faili.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui külaline agent lisandmooduli käivitit käsitsi kutsumisel, valesti, VM.
-105|Lisandmooduli diagnostika ei saa avada diagnostika konfiguratsioonifail.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui diagnostika lisandmooduli käsitsi rakendatakse, valesti, VM.
-106|Konfiguratsioonifail diagnostika ei saa lugeda.<p><p>Lahendus: See on tulemus õigest skeemi valideerimine konfiguratsioonifail. Nii, et lahendus on pakkuda konfiguratsioonifail, mis vastab skeemiga. Saate otsida XML, mis saadetakse kausta *%SystemDrive%\WindowsAzure\Config* VM diagnostika laiendamine. Avage vastav XML-failis ja **Microsoft.Azure.Diagnostics**, siis **xmlCfg** välja Otsing. Andmed on base64 kodeeritud nii, et peate [dekodeerida,](http://www.bing.com/search?q=base64+decoder) XML, mis on laaditud diagnostika kuvamiseks.<p>
-107|Ressursi directory jõustamine jälgimisega seotud agent ei sobi.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui jälgimise agent käsitsi rakendatakse, valesti, VM.</p>
-108    |Ei saa teisendada diagnostika konfiguratsiooni faili jälgimise agent konfiguratsioonifail.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui diagnostika lisandmooduli kutsumisel käsitsi sobimatu konfiguratsioon faili.
-110|Üldine diagnostika konfiguratsiooni tõrge.<p><p>See on sisemine tõrge, mida tuleks ainult siis, kui diagnostika lisandmooduli kutsumisel käsitsi sobimatu konfiguratsioon faili.
-111|Ei saa käivitada jälgimisega seotud agent.<p><p>Lahendus: Veenduge, et piisavalt süsteemi ressursid on saadaval.
-112|Üldine tõrge


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostika andmed on Azure salvestusruumi pole sisse logitud
Azure'i diagnostika talletab kõik andmed Azure Storage.

Kõige levinum põhjus puuduvad sündmuse andmed on valesti määratletud salvestusruumi kontoteave.

Lahendus: Vea diagnostika konfiguratsiooni fail ja diagnostika uuesti installida.
Kui probleem ei lahene, pärast uuesti installida diagnostika laiend, siis võib-olla peate silumine veelgi järgi otsides selle jälgimise agent vigu. Enne sündmuse andmed on üles laaditud salvestusruumi kontole salvestatakse see selle LocalResourceDirectory.

Pilveteenuse teenuse rolli selle LocalResourceDirectory on:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Funktsiooni LocalResourceDirectory Virtuaalmasinates on:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Kui puuduvad failid LocalResourceDirectory kausta, jälgimisega seotud agent ei saa käivitada. Selle põhjuseks tavaliselt sobimatu konfiguratsioonifail, sündmus, mis tuleks esitada soovitud CommandExecution.log.

Kui jälgimise Agent on edukalt sündmuse andmete kogumise kuvatakse iga sündmuse Otsingukonfiguratsiooni failis määratletud .tsf failid. Jälgimine Agent logib oma tõrgete failis MaEventTable.tsf. Selle faili sisu kontrollimiseks saate tabel2csv rakenduse .tsf faili teisendamine komaga eraldatud values(.csv) faili:

Pilveteenuse rolli:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*% SystemDrive %* pilvepõhise teenuse rollile on tavaliselt D:

Virtuaalne arvutisse:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Ülaltoodud käsud loob funktsiooni log faili *maeventtable.csv*, mille saate avada ja tõrge sõnumite kontrollimine.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnostika andmete tabeleid ei leitud
Azure'i salvestusruumi Azure diagnostika andmeid tabelite nimega alljärgnev kood:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Siin on näide:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

See loob tabeli 4:

Sündmuse|Tabeli nimi
---|---
pakkuja = "prov1" &lt;sündmuse id = "1" /&gt;|WADEvent + MD5("prov1") + "1"
pakkuja = "prov1" &lt;sündmuse id = "2" eventDestination = "dest1" /&gt;|WADdest1
pakkuja = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
pakkuja = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2
