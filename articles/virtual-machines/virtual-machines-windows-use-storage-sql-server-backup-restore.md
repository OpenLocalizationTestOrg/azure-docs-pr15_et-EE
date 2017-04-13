<properties
    pageTitle="Kuidas kasutada Azure storage SQL serveri varundamiseks ja taastamiseks | Microsoft Azure'i"
    description="Saate teada, kuidas varundada SQL Server Azure'i salvestusruumi. Selgitatakse eelised Azure Storage SQL-andmebaasi varundada."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Kasutada Azure Storage SQL serveri varundus ja taaste

## <a name="overview"></a>Ülevaade

SQL Server 2012 SP1 CU2 alustades, saate nüüd kirjutada SQL serveri varukoopia otse Azure'i bloobimälu salvestusteenus. Selle funktsiooni abil saate tagasi kuni ja Azure'i bloobimälu teenus on asutusesisese SQL serveri andmebaasi või mõne Azure virtuaalse masina SQL serveri andmebaasi taastada. Varundus Cloud pakub kättesaadavus, piiramatu geo kopeeritud väljapoole salvestusruumi ja hõlpsamaks andmete migreerimine pilvest eelised. Varundus ja taaste laused saate väljastada Transact-SQL-i või SMO abil.

SQL Server 2016 tutvustab uusi võimalusi; [fail – hetktõmmise varukoopia](http://msdn.microsoft.com/library/mt169363.aspx) abil saate teha peaaegu kohene varukoopiate ja väga kiiresti taastab.

See teema selgitab, miks te võite valida salvestusruumi Azure SQL-i varufailide ja seejärel kirjeldatakse seotud osade. Saate selle artikli lõpus vahendite juhendavad tutvustused ja lisateavet, et teie SQL serveri varukoopia ja selle teenuse kasutamise alustamine.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Varufailide SQL Server Azure'i bloobimälu teenuse kasutamise eelised

On mitu probleeme teil kui SQL serveri varundada. Neid probleeme kaasata mäluhaldus salvestamise tõrge, väljapoole salvestusruumi ja riistvara konfiguratsiooni juurdepääsu riski. Paljude neid probleeme käsitletakse varufailide SQL Server Azure'i bloobimälu Store'i abil. Võtke arvesse järgmisi eeliseid.

- **Lihtne kasutamine**: talletamine varukoopiate Azure plekid võib olla mugav, paindlik ja hõlpsalt juurde pääsemiseks väljapoole suvand. Väljapoole salvestusruum oma SQL serveri varukoopia loomine võib olla sama lihtne kui muutmine oma olemasolevate skriptide/projektide **VARUNDAMISE URL-i** süntaksi kasutama. Väljapoole salvestusruumi tuleks tavaliselt piisavalt tootmise andmebaasi asukoht vältimiseks ühe katastroofi, mis võivad mõjutada nii väljapoole ja tootmise andmebaasi asukohad. Valides [geo-juhendist oma Azure plekid](../storage/storage-redundancy.md), peate mõne eest kiht kaitse õnnetuste, mis võivad mõjutada kogu piirkonna.
- **Varundamise arhiivi**: The Azure'i bloobimälu teenus pakub parem alternatiiv sageli kasutatud lindi suvand arhiivida varukoopiad. Lindi salvestusruumi võib vaja füüsiline transport on väljaspool süsteem ja mõõdud meediumi kaitsta. Azure'i bloobimälu oma varukoopiate talletamine pakub kohe, mis on väga saadaval, ja on vastupidav arhiivimine suvand.
- **Hallatavad riistvara**: on ei pea kohal riistvara haldamise Azure'i teenustega. Azure'i teenuste haldamine riistvara ja geo-dispersioonanalüüs ette koondamise ja kaitse riistvara tõrkeid.
- **Piiramatu salvestusruum**: Azure plekid otsese varukoopia võimaldades teil on juurdepääs peaaegu piiramatu salvestusruum. Teise võimalusena kuni varundamine on Azure virtuaalse masina kettal on seadme suurusest piirangud. Pole ketast, saate lisada ka Azure virtuaalse masina varufailide arv on piiratud. See on 16 ketast suurtele eksemplari ja vähem väiksemate eksemplarid.
- **Varundus-saadavus**: Azure plekid talletatavad varukoopiate saadaval igal pool ja igal ajal ja pääseb hõlpsalt taastab mõne asutusesisese SQL serveri või mõne muu SQL Server töötab rakenduses on Azure virtuaalse masina, ilma vajaduseta andmebaasi lisada/eemaldada või allalaadimise ja lisades selle VHD.
- **Maksumus**: ainult kasutatava teenuse eest maksta. Saab kulutõhus väljapoole ja varukoopia arhiivi valikuna. Leiate [Azure'i hinnakirjad kalkulaator](http://go.microsoft.com/fwlink/?LinkId=277060 "Kalkulaator hinnad")ja [Azure hinnad artiklis](http://go.microsoft.com/fwlink/?LinkId=277059 "hinnad artiklis") lisateabe saamiseks.
- **Salvestusruumi hetktõmmiseid**: kui andmebaasi failid on Azure'i bloobimälu ja kasutate SQL Server 2016, saate teha peaaegu kohene varukoopiate ja väga kiiresti taastab [faili-hetktõmmise varukoopia](http://msdn.microsoft.com/library/mt169363.aspx) .

Täpsema teabe saamiseks vt [SQL serveri varundus ja taaste Azure'i bloobimälu salvestusruumi teenusega](http://go.microsoft.com/fwlink/?LinkId=271617).

Järgmised kaks jaotist Tutvustage Azure'i bloobimälu salvestusteenus, sh nõutavad SQL Serveri komponendid. See on oluline mõista komponendid ja edukalt kasutada varundamine ja taastamine salvestusteenus Azure'i bloobimälu oma suhtlus.

## <a name="azure-blob-storage-service-components"></a>Azure'i bloobimälu salvestusruumi teenuse komponendid

Kui varundate Azure'i bloobimälu salvestusteenus kasutatakse Azure järgmised komponendid.

| Komponent               | Kirjeldus                          |
|---------------------|-------------------------------|
| **Salvestusruumi konto** | Salvestusruumi konto on kõigi salvestusruumi teenuste alguspunkti. Azure'i bloobimälu teenuse juurdepääsu esmalt Looge konto Azure Storage. Azure'i bloobimälu salvestusteenus kohta lisateabe saamiseks vaadake, [Kuidas kasutada salvestusteenus Azure'i bloobimälu](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** | Ümbris pakub rühmitamise plekid kogumi ja saate talletada plekid piiramatu arv. Kirjutada SQL Server Azure'i bloobimälu teenusesse varukoopia peab teil vähemalt juurkausta ümbris loodud. |
| **Bloobimälu** | Faili tüüp ja suurus. Plekid on adresseeritavad URL-i järgmises vormingus: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Lehe plekid kohta leiate lisateavet teemast [mõistmine plokk ja lehe plekid](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Serveri komponendid

Kui varundate Azure'i bloobimälu salvestusteenus kasutatakse SQL serveri järgmised komponendid.

| Komponent               | Kirjeldus                          |
|---------------------|-------------------------------|
| **URL-I** | URL-i lisamine ühtse identifikaator (URL) kordumatu varukoopia faili saate määrata. URL-i kasutatakse faili SQL serveri varukoopia nimi ja asukoht. URL-i peavad osutama tegelik bloobimälu, mitte ainult ümbrises. Kui soovitud bloobimälu olemas, on loodud. Kui mõne olemasoleva bloobimälu pole määratud, varundus nurjub, kui selle > koos VORMINGU suvand on määratud. Järgmine on näide määrate VARUNDAMISE käsk URL: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS on soovitatav, kuid mitte nõutav. |
| **Mandaadi** | Teave, mida on vaja ühenduse loomiseks ja autentida Azure'i bloobimälu salvestusteenus on salvestatud mandaati.  Selleks SQL serveri kirjutamise varukoopiate Azure'i bloobimälu või Taasta selle, tuleb luua SQL serveri mandaadi. Lisateavet leiate teemast [SQL serveri identimisteavet](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Kui te ei soovi kopeerida ja varukoopia faili üleslaadimine Azure'i bloobimälu salvestusteenus, peate kasutama lehe bloobimälu tüüp teie salvestusruumi suvand kui kavatsete kasutada seda faili Taasta toimingud. Blokeeri bloobimälu tüübist taastamine nurjub viga.

## <a name="next-steps"></a>Järgmised sammud

1. Looge Azure'i konto, kui teil pole veel üks. Kui te hindate Azure, kaaluge [tasuta prooviversioon](https://azure.microsoft.com/free/).

1. Minge kaudu järgmised õppetükid, mis teile, luues salvestusruumi konto ja taastamise abil.

    - **SQL serveri 2014**: [õpetus: Microsoft Azure SQL serveri 2014 varundus- ja Bloobivahemälu salvestusteenus](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [õpetus: Microsoft Azure'i bloobimälu salvestusteenus funktsiooniga SQL Server 2016 andmebaase](https://msdn.microsoft.com/library/dn466438.aspx)

1. Vaadata täiendavaid dokumente, alustades [SQL serveri varundus ja taaste Microsoft Azure'i bloobimälu salvestusruumi teenusega](https://msdn.microsoft.com/library/jj919148.aspx).

Kui teil on probleeme, vaadake teema [SQL serveri varukoopia URL-i põhitõed ja tõrkeotsing](https://msdn.microsoft.com/library/jj919149.aspx).

Muud SQL serveri varundamine ja taastamine suvandid leiate teemast [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
