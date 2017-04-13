<properties
   pageTitle="IIS-i logib Log Analytics | Microsoft Azure'i"
   description="Internet Information Services (IIS) salvestab kasutaja tegevuste logifailid Log Analytics kogunud.  Selles artiklis kirjeldatakse, kuidas konfigureerida IIS-i logide kogumine ja nende loodud OMS hoidlas kirjete üksikasjad."
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

# <a name="iis-logs-in-log-analytics"></a>IIS-i logib Log Analytics
Internet Information Services (IIS) salvestab kasutaja tegevuste logifailid Log Analytics kogunud.  

![IIS-i logid](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Logib IIS-i konfigureerimine
Log Analytics kogub kirjeid nii, et peate [IIS-i jaoks logimine konfigureerimine](https://technet.microsoft.com/library/hh831775.aspx)IIS-i, loodud logifailide.

Log analüüsi ainult toetab W3C vormingus salvestatud failide IIS-i ja ei toeta kohandatud välju või IIS Täpsem logimine.  
Log Analytics kogub logid NCSA või IIS-i kohalikke vormingus.

Konfigureerida IIS-i logid Log Analytics [andmete menüü analüüsi sätted](log-analytics-data-sources.md#configuring-data-sources).  On pole konfigureerimine vajalik erinevalt valimine **kogumise W3C vormingut IIS-i logifailid**.

Soovitame, et kui lubate IIS-i Logi saidikogumi, peaksite konfigureerima IIS-i Logi edastamiseks säte iga serveris.


## <a name="data-collection"></a>Andmete kogumine

Log Analytics kogub IIS-i Logi kirjed iga ühendatud andmeallikast ligikaudu iga 15 minuti järel.  Agent salvestab iga kogutud kaudu sündmuste logi selle asemel.  Kui agent läheb ühenduseta, siis Log Analytics kogub sündmusi, kus see viimati pooleli jäi, isegi juhul, kui need sündmused on loodud agent võrguühenduseta oleku ajal.


## <a name="iis-log-record-properties"></a>IIS-i kirje atribuudid

IIS-i kirjed on teatud tüüpi **W3CIISLog** ja atribuutide järgmises tabelis on:

| Atribuut | Kirjeldus |
|:--|:--|
| Arvuti | Arvuti, mis on kogutud sündmuse nimi. |
| cIP | Kliendi IP-aadress. |
| csMethod | Näiteks GET või POST taotluse meetod. |
| csReferer | Saidi, et kasutaja jälgitavad lingi praeguse saidi kaudu. |
| csUserAgent | Kliendi tüüp brauseris. |
| csUserName | Autenditud kasutaja, mis serveri nimi. Anonüümsete kasutajate tähistab sidekriips. |
| csUriStem | Target (sihtkoht), nt veebilehele taotluse. |
| csUriQuery | Päringu, kui mis tahes, et kliendi proovib teha. |
| ManagementGroupName | Toiminguhalduri agentide haldus rühma nimi.  Muude töötajatega, see on AOI -\<tööruumi ID\> |
| RemoteIPCountry | Riik, kus kliendi IP-aadress. |
| RemoteIPLatitude | Laiuskraad kliendi IP-aadressi. |
| RemoteIPLongitude | Kliendi IP-aadressi pikkuskraad. |
| scStatus | HTTP olekukoodi. |
| scSubStatus | Tõrkekood SubStatus. |
| scWin32Status | Windowsi olekukoodi. |
| sIP | Veebiserver IP-aadress. |
| SourceSystem  | OpsMgr |
| Spordialade | Pordi serveri klient ühendatud. |
| sSiteName | IIS-i saidi nimi. |
| TimeGenerated | Kuupäev ja kellaaeg logisse kandmise. |
| TimeTaken | Kestuse millisekundites taotluse. |

## <a name="log-searches-with-iis-logs"></a>Log otsinguid IIS-i logid

Järgmises tabelis antakse eri ülevaade log päringuid, mis tuua IIS-i kirjed.

| Päringu | Kirjeldus |
|:--|:--|
| Tüüp = IISLog | Kõigi IIS-i kirjed. |
| Tüüp = IISLog EventLevelName = tõrge | Kõigi Windowsi sündmuste ja tõsidust tõrge. |
| Tüüp = W3CIISLog & #124; Mõõta count() cIP | Kliendi IP-aadress logige Count of IIS-i kirjed. |
| Tüüp = W3CIISLog csHost = "www.contoso.com" & #124; Mõõta count() csUriStem | Count of IIS-i Logi www.contoso.com hosti URL-i kirjeid. |
| Tüüp = W3CIISLog & #124; Mõõt Sum(csBytes) arvuti & #124; ülemine 500000| Kokku baiti saadud iga IIS-i arvutisse. |

## <a name="next-steps"></a>Järgmised sammud

- Log Analytics koguda muude [andmeallikate](log-analytics-data-sources.md) analüüsi jaoks konfigureerida.
- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks.
- Konfigureerida teatiste Log Analytics aktiivselt märku leitud IIS-i logid olulised tingimused.
