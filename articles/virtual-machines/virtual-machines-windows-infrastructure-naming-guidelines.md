<properties
    pageTitle="Taristu nime panemise juhised | Microsoft Azure'i"
    description="Lisateavet selle võtme ja rakendamist nime panemise juhised Azure taristu teenused."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Taristu nime juhised

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

See artikkel keskendub mõistmist, kuidas lähenemine nimereeglid oma erinevate Azure ressursid koostamiseks ressursside loogiline ja hõlpsasti hulga kogu teie keskkonnas.

## <a name="implementation-guidelines-for-naming-conventions"></a>Nimereeglid rakendamise juhised

Otsuseid:

- Mis on teie nimereeglid Azure ressursse?

Ülesanded:

- Määratleda kinnitab kasutada oma ressursse üle säilitada.
- Määratleda salvestusruumi kontonimed antud nõue neid globaalselt kordumatu.
- Dokumendi nimetamistava kasutada ja jagada kõik osapooled üle juurutuste järjepidevuse tagamiseks.

## <a name="naming-conventions"></a>Nimereeglid

Enne luua midagi Azure peaks olema kohas hea nimetamistava. Nimereeglistik tagab, et kõik ressursid on prognoositavad nimi, mis aitab madalam haldus koormust nende ressursside haldamine.

Võite järgida määratud nimereeglid kogu ettevõtte jaoks või teatud Azure tellimuse või konto jaoks määratletud. Kuigi see on lihtne üksikisikutele organisatsioonides peidetud eeskirjad töötamisel Azure ressursse, kui meeskond peab Azure projekti kallal töötamiseks, et mudeli skaala ka.

Leppida kogumi nimereeglid ette. On mõned kaalutlused nimereeglid käsitletavad mis määrab reeglite.

## <a name="affixes"></a>Kinnitab

Nagu saate vaadata määratleda nimereeglistik, sisaldab ühe asja, kas selle kinnitada on veebisaidil.

- Nime (eesliide) algusesse
- Nime (järelliide)

Näiteks siin on kaks võimalikku nimed ressursirühm kasutamiseks on `rg` kinnitada:

- RG Web Appis (eesliide)
- Web Appis-Rg (järelliide)

Kinnitab võib viidata erinevaid aspekte, mis kirjeldavad teatud ressursid. Järgmises tabelis on mõned näited tavaliselt kasutatakse.

| Proportsioonid                               | Näited                                                               | Märkmete                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Keskkonnas                          | Arendaja stg, joogid                                                         | Sõltuvalt eesmärki ja iga keskkonna nimi.                                                     |
| Asukoht                             | usw (Lääne USA), kasutage (Ida-USA 2)                                         | Andmekeskuse või-piirkonna organisatsiooni piirkonnast.                               |
| Azure'i komponent, teenuse või toote | RG ressursirühma, VNet virtuaalse võrgu jaoks                        | Sõltuvalt sellest, mis toetab ressursi toote.                                          |
| Roll                                 | SQL-i, ora, sp IIS-i                                                      | Sõltuvalt virtuaalse masina roll.                                                              |
| Eksemplari                             | 01, 02, 03, jne.                                                       | Ressursid, mis on rohkem kui ühe eksemplari. Näiteks koormusetasakaalustusega pilveteenus web serverites. |


Kui teie nimereeglid, veenduge, et kindlat tüüpi ressursi ja milles positsiooni (eesliide vs järelliide) kasutada mis kinnitab selgelt teatada.

## <a name="dates"></a>Kuupäevad

Sageli on oluline kindlaks määrata ressursi nimi loomise kuupäev. Soovitame AAAAKKPP kuupäeva vormingus. Selles vormingus tagab, et täielik kuupäev on salvestatud, vaid ka selle kahe ressursse, kelle nimed erinevad ainult kuupäeva sorditakse tähestikulises järjestuses ja kronoloogilises järjekorras samal ajal.

## <a name="naming-resources"></a>Nime andmise ressursid

Määratlege iga ressursi nimereeglistik rakenduses, mis peaks olema reegleid, et määrata, kuidas määrata nimed iga ressurss, mis on loodud. Reegleid tuleks rakendada igat tüüpi ressursse, näiteks:

- Tellimused
- Kontod
- Salvestusruumi kontod
- Virtuaalne võrkude
- Alamvõrku
- Kättesaadavus komplektid
- Ressursi rühmad
- Virtuaalmasinates
- Lõpp-punktid
- Võrgu turberühmad
- Rollid

Veenduge, et nimi pakub piisavalt teavet, et otsustada, millist ressursi see viitab, kasutage kirjeldavad nimed.

## <a name="computer-names"></a>Arvuti nimed

Kui loote virtuaalse masina (VM), nõuab Microsoft Azure'i VM nimi, kuni 15 märki kasutatava ressursi nimi. Azure'i kasutab operatsioonisüsteem VM sama nimi. Siiski nende nimed ei pruugi alati olla sama.

Juhuks, kui .vhd pilt failist, mis juba sisaldab operatsioonisüsteem on loodud VM, Azure VM nimi võib erineda soovitud VM operatsioonisüsteemi arvuti nimi. Juhul saate lisada raskusaste Seetõttu soovitame VM haldus. Azure'i VM ressurssi määrata arvuti nimi, mida saate määrata selle VM operatsioonisüsteem sama nimi.

Soovitame, et Azure'i VM nimi on sama, mis on aluseks oleva operatsioonisüsteemi arvuti nimi.

## <a name="storage-account-names"></a>Salvestusruumi konto nimed

Salvestusruumi kontod on teisiti eeskirjad nende nimed. Saate kasutada ainult väiketähti ja numbreid. Lisateabe saamiseks vaadake [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) . Lisaks salvestusruumi konto nimi koos core.windows.net, peaks olema globaalselt kehtiv, kordumatu DNS-i nimi. Näiteks kui salvestusruumi konto nimi on mystorageaccount, tulemuseks järgmised DNS-i nimed peavad olema kordumatud:

- mystorageaccount.blob.Core.Windows.net
- mystorageaccount.table.Core.Windows.net
- mystorageaccount.Queue.Core.Windows.net


## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 