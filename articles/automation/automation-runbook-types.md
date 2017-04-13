<properties 
   pageTitle="Azure'i automaatika Käitusjuhendi tüübid"
   description="Kirjeldatakse tegevusraamatud, mida saate kasutada Azure automatiseerimine ja asjaolu tuleks arvesse võtta, kui kindlaks teha, millist tüüpi kasutada erinevat tüüpi. "
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure'i automaatika käitusjuhendi tüübid

Azure'i automaatika toetab nelja tüüpi tegevusraamatud, mis on põgusalt kirjeldatud järgmises tabelis.  Järgmistes jaotistes pakuvad lisateavet kindlat tüüpi, sh kaalutluste kohta, millal kasutada iga.


| Tüüp |  Kirjeldus |
|:---|:---|
| [Graafilise](#graphical-runbooks) | Windows PowerShelli ja Azure portaali loodud ja redigeeritud täielikult pildiredaktor põhjal. | 
| [Graafilise PowerShelli töövoo](#graphical-runbooks) | Windows PowerShelli töövoo põhjal ja loodud ja redigeeritud täielikult pildiredaktor Azure'i portaalis. 
| [PowerShelli](#powershell-runbooks) | Teksti käitusjuhendi Windows PowerShelli skripti põhjal.
| [PowerShelli töövoo](#powershell-workflow-runbooks) | Teksti käitusjuhendi Windows PowerShelli töövoo alusel. |


## <a name="graphical-runbooks"></a>Graafilise tegevusraamatud

[Graafilised](automation-runbook-types.md#graphical-runbooks) ja graafiline PowerShelli töövoo tegevusraamatud on loodud ja redigeeritud graafilise toimetaja Azure'i portaalis.  Saate eksportida faili ja seejärel importige need automatiseerimise mõne muu kontoga, kuid ei saa luua ega redigeerida mõnda muud tööriista abil.  Graafilise tegevusraamatud luua PowerShelli koodi, kuid te ei saa otse vaadata või muuta koodi. Graafilise tegevusraamatud ei saa teisendada ühe [tekstivorminguid](automation-runbook-types.md)ega saab teksti käitusjuhendi teisendada graafilisel kujul. Graafilise tegevusraamatud saab teisendada graafiline PowerShelli töövoo tegevusraamatud ajal impordi- ja vastupidi.

### <a name="advantages"></a>Eelised

- Saate luua minimaalsete tundmine [PowerShelli](automation-powershell-workflow.md)tegevusraamatud.
- Visuaalselt esindab halduse protsessid.
- Muud tegevusraamatud kaasamine lapse tegevusraamatud kõrge taseme töövoogude loomiseks.


### <a name="limitations"></a>Piirangud

- Käitusjuhendi väljaspool Azure portaali ei saa redigeerida.
- Võib olla vaja koodi tegevust, mis sisaldavad PowerShelli koodi keerukate loogika sooritamiseks.
- Ei saa vaadata või redigeerida otse PowerShelli koodi, mis on loodud graafilise töövoog. Pange tähele, et loote koodi tegevuse koodi saate vaadata.


## <a name="powershell-runbooks"></a>PowerShelli tegevusraamatud

PowerShelli tegevusraamatud põhinevad Windows PowerShelli.  Saate otse muuta teksti redaktori kasutamine Azure portaali käitusjuhendi kood.  Samuti saate ühenduseta tekstiredaktoris ja [käitusjuhendi importimine](http://msdn.microsoft.com/library/azure/dn643637.aspx) rakendusse Azure automatiseerimine.

### <a name="advantages"></a>Eelised

- Kõigi keerukate loogika PowerShelli koodiga ilma PowerShelli töövoo täiendavad keerulisi rakendada. 
- Käitusjuhendi käivitub kiiremini graafiline või PowerShelli töövoo tegevusraamatud kuna see ei vaja, tuleb koostada enne töötab.

### <a name="limitations"></a>Piirangud

- Peab olema tuttav PowerShelli skripti.
- [Paralleelne töötlemine](automation-powershell-workflow.md#parallel-processing) ei saa kasutada samaaegselt mitu toimingute teostamiseks.
- Ei saa kasutada [postkastid](automation-powershell-workflow.md#checkpoints) jätkamiseks käitusjuhendi tõrke korral.
- PowerShelli töövoo tegevusraamatud ja graafiline tegevusraamatud ainult võib kaasata lapse tegevusraamatud algus-AzureAutomationRunbook cmdlet, mis loob uue töö abil.

### <a name="known-issues"></a>Teadaolevad probleemid
Järgnevalt on praeguse PowerShelli tegevusraamatud teadaolevad probleemid.

- PowerShelli tegevusraamatud ei saa ei saa tuua krüptimata [muutuv varade](automation-variables.md) tühiväärtus.
- PowerShelli tegevusraamatud ei saa tuua [muutuv varade](automation-variables.md) koos *~* nimi.
- Get-protsess on PowerShellis esitatavaks käitusjuhendi krahh umbes 80 iteratsiooni järel. 
- PowerShelli käitusjuhendi võib nurjuda, kui see proovib kirjutamise väga suurt hulka andmeid väljundi voo korraga.   Saate tavaliselt töötada selle probleemi lahendamiseks, kirjutamine ainult suurte objektide töötades vajalik teave.  Näiteks saate asemel kirjutamine umbes *Get-protsess*, väljund lihtsalt nõutavad väljad *Get-protsess | Valige ProcessName CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShelli töövoo tegevusraamatud

PowerShelli töövoo tegevusraamatud on tekst tegevusraamatud [Windows PowerShelli töövoo](automation-powershell-workflow.md)alusel.  Saate otse muuta teksti redaktori kasutamine Azure portaali käitusjuhendi kood.  Samuti saate ühenduseta tekstiredaktoris ja [käitusjuhendi importimine](http://msdn.microsoft.com/library/azure/dn643637.aspx) rakendusse Azure automatiseerimine.

### <a name="advantages"></a>Eelised

- Saate rakendada kõigi keerukate loogika PowerShelli töövoo koodiga.
- Kasutage [postkastid](automation-powershell-workflow.md#checkpoints) jätkamiseks käitusjuhendi tõrke korral.
- [Paralleelne töötlemine](automation-powershell-workflow.md#parallel-processing) abil saate teha mitu toimingut paralleelselt.
- Saate lisada muid graafilise tegevusraamatud ja PowerShelli töövoo tegevusraamatud lapse tegevusraamatud kõrge taseme töövoogude loomiseks.


### <a name="limitations"></a>Piirangud

- Autor peab olema tuttav PowerShelli töövoog.
- Käitusjuhendi peab tegelema PowerShelli töövoo täiendavad keerukus näiteks [deserialized objektid](automation-powershell-workflow.md#code-changes).
- Käitusjuhendi kestab kauem kui PowerShelli tegevusraamatud käivitamiseks, kuna seda vaja enne koostada.
- PowerShelli tegevusraamatud saab kaasata lapse tegevusraamatud algus-AzureAutomationRunbook cmdlet, mis loob uue töö abil.


## <a name="considerations"></a>Kaalutlused

Võtke arvesse täiendavate järgmisega määramisel, millist tüüpi kasutada kindla käitusjuhendi.

- Ei saa teisendada kaudu graafiline tegevusraamatud teksti tüüp või vastupidi.
- Kehtivad piirangud kasutamine eri tüüpi tegevusraamatud lapse käitusjuhendi.  Lisateavet leiate [Azure'i automaatika lapse tegevusraamatud](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Järgmised sammud

- Graafilised käitusjuhendi loome kohta lisateabe saamiseks lugege teemat [graafilised loome Azure'i automaatika](automation-graphical-authoring-intro.md)
- PowerShelli ja PowerShelli erinevused mõista töövoogusid tegevusraamatud, lugege teemat [Õ Windows PowerShelli töövoo](automation-powershell-workflow.md)
- Loomise või importimise on Käitusjuhendi kohta leiate lisateavet teemast [loomise või importimise on Käitusjuhendi](automation-creating-importing-runbook.md)



