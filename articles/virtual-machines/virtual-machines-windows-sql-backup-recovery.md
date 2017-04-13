<properties
    pageTitle="Varundus ja taaste SQL serveri korral | Microsoft Azure'i"
    description="Varundus ja taaste kaalutluste andmebaaside SQL Server Azure'i Virtuaalmasinates kirjeldatakse."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Varundus ja taaste SQL Server Azure'i virtuaalmasinates

## <a name="overview"></a>Ülevaade

SQL serveri andmebaasi andmete varundamine on oluline osa strateegia kaitsta andmekao rakendus või kasutaja tõrgete tõttu. See kehtib ka SQL Server töötab klõpsake Azure'i Virtuaalmasinates (VMs).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

SQL Server Azure'i VMs töötab, saate kasutada kohalikke varundamine ja taastamine tehnoloogiad manustatud ketast, sihtkoha varukoopia faile. Siiski pole ketast, saate lisada Azure virtuaalse masina, [virtuaalse masina suurus](virtual-machines-linux-sizes.md)arv on piiratud. Olemas on ka pea kohal, ketta juhtimise silmas pidada.

Alates SQL Server 2014, saate varundamine ja taastamine Microsoft Azure'i bloobimälu. SQL Server 2016 pakub täiustatud see suvand. Lisaks andmebaasi faile, mis on talletatud Microsoft Azure'i bloobimälu, SQL Server 2016 näeb suvandi peaaegu kohene varufailide ja Azure hetktõmmiseid abil kiiresti taastab. Selles artiklis antakse ülevaade suvanditest ja täiendava teabe leiate [SQL serveri varundus ja taaste koos Microsoft Azure'i bloobimälu salvestusteenus](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Arutelu võimalusi väga suurte andmebaasi varundada, vt [Mitme-Teratavu SQL serveri andmebaasi varundamine strateegiad Azure'i Virtuaalmasinates](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Järgmistes jaotistes kaasata erinevaid versioone on Azure virtuaalse masina toetatud SQL serveri teave.

## <a name="sql-server-virtual-machines"></a>SQL serveri Virtuaalmasinates

Kui SQL serveri eksemplar töötab kohta on Azure virtuaalse masina, andmebaasi failide juba asuda andmeid kettale Azure. Azure'i bloobimälu elavad ketast. Nii põhjused varundada oma andmebaasi ja neid meetodit saate võtta muutmine veidi. Võtke arvesse järgmist. 

- Te enam ei vaja teha andmebaasi anda kaitse riist- või meediumi tõrge, sest Microsoft Azure'i pakub Microsoft Azure'i teenuse osana kaitse.

- Peate endiselt esitada kaitse kasutaja vigu või arhiivimiseks, regulatiivsetele põhjust, või halduse andmebaasi teha.

- Varufaili saate salvestada otse Azure. Lisateavet leiate järgmistest jaotistest mis SQL serveri eri versioonide juhised.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server 2016 toetab [varundus ja taaste Azure plekid](https://msdn.microsoft.com/library/jj919148.aspx) funktsioonid, SQL Server 2014. Kuid see sisaldab ka järgmisi täiustusi.

| 2016 parandamiseks               | Üksikasjad                          |
|---------------------|-------------------------------|
| **Striping**              | Kui varundate Microsoft Azure'i bloobimälu, SQL Server 2016 toetab mitut plekid lubamiseks varundada suurte andmebaaside, kuni 12.8 TB varukoopiat.      |
| **Hetktõmmise varukoopia**                | Azure'i pilte, kasutades selleks SQL serveri faili-hetktõmmise varukoopia pakub peaaegu kohene varukoopiate ja kiire taastab andmebaasi failide talletatud salvestusteenus Azure'i bloobimälu abil. See funktsioon võimaldab teil lihtsustada oma varundamine ja taastamine poliitikate. Fail – hetktõmmise varukoopia toetab punkti aja taastamine. Lisateabe saamiseks vt [Hetktõmmise varukoopiate Azure'i andmebaasi faile](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Hallatavate varukoopia plaanimine**            | SQL serveri õnnestus varukoopia Azure toetab nüüd kohandatud ajakava. Lisateavet leiate teemast [SQL serveri õnnestus varukoopia Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

SQL Server 2016 Azure'i bloobimälu kasutamisel võimalusi, juhendi leiate [õpetus: Microsoft Azure'i bloobimälu salvestusteenus funktsiooniga SQL Server 2016 andmebaase](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL serveri 2014

SQL serveri 2014 sisaldab järgmisi täiustusi.

1. **Varundus ja taaste Azure**.

 - *SQL serveri varukoopia URL* toetab nüüd SQL Server Management Studio. Võimalus Azure varundamine on nüüd saadaval SQL Server Management Studio varundus ja taaste tööülesande või hoolduse leping viisardi abil. Lisateavet leiate teemast [SQL serveri varukoopia URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL serveri õnnestus varukoopia Azure* on uusi funktsioone, mis võimaldab automatiseeritud varukoopia juhtimine. See on eriti kasulik SQL Server 2014 on Azure arvutis töötavate eksemplaride varukoopia halduse automatiseerimiseks. Lisateavet leiate teemast [SQL serveri õnnestus varukoopia Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automaatse varundamise* pakub täiendavaid automatiseerimise automaatselt lubamiseks *SQL serveri õnnestus varukoopia Azure'i* kõik olemasolevate kui ka uute andmebaaside jaoks Azure SQL serveri VM. Lisateavet leiate teemast [Automaatse varundamise SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-automated-backup.md).
 - Kõigi suvandite SQL serveri 2014 varukoopia Azure ülevaate leiate teemast [SQL serveri varundus ja taaste koos Microsoft Azure'i bloobimälu salvestusteenus](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Krüptimise**: SQL serveri 2014 toetab krüptimist varukoopia loomisel. See toetab mitu krüptimise algoritmid ja kasuta osf sert või asümmeetriline võti. Lisateabe saamiseks vt [Varundamise krüptimine](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012

SQL serveri varundus ja taaste SQL Server 2012 kohta leiate üksikasjalikumat teavet teemast [varundamine ja taastamine, SQL serveri andmebaasi (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Alates SQL Server 2012 SP1 koondvärskenduse 2, saate tagasi kuni ja Azure'i bloobimälu teenuse taastada. Selle parandamiseks saate kasutada SQL serveris, kus töötab mõni Azure virtuaalse masina või kohapealse eksemplar SQL serveri andmebaasi varundamine. Lisateabe saamiseks vt [SQL serveri varundus ja taaste Azure'i bloobimälu salvestusruumi teenusega](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Azure'i bloobimälu salvestusteenus kasutamise eeliste hulka kuuluvad võimalus mööduda 16 kettaruumi limiit manustatud ketast, haldus otsese kättesaadavus migreerimise või katastroofi taastamine muus eksemplaris Azure virtuaalne arvutis töötab SQL serveri eksemplar või kohapealse näiteks varufaili hõlpsamaks. Azure'i bloobimälu salvestusteenus, SQL serveri varufailide kasutamise eeliseid täieliku loendi leiate jaotisest *eeliste* [SQL serveri varundus ja taaste Azure'i bloobimälu salvestusruumi teenusega](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Parimaks soovitused ja tõrkeotsingu teavet leiate teemast [varundamine ja taastamine head tavad (Azure'i bloobimälu salvestusteenus)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

SQL serveri varundus ja taaste SQL Server 2008 R2, vt [varundamise ja taastamise andmebaaside SQL serveri (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL serveri varundus ja taaste SQL Server 2008, vt [varundamise ja taastamise andmebaaside SQL serveri (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Järgmised sammud

Kui kavatsete SQL Server Azure'i VM-i juurutamise, leiate järgmised õpetuses ettevalmistamise juhised: [Azure'i ressursihaldur Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md).

Kuigi varundus ja taaste abil saab andmete migreerimiseks on potentsiaalselt lihtsam andmete migreerimise teed, kus SQL Server on Azure VM. Täieliku arutelu migreerimise Valikud ja soovitused, leiate teemast [on Azure VM SQL serveri andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md).

Muud [ressursid, mis töötab SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md)läbivaatamine.
