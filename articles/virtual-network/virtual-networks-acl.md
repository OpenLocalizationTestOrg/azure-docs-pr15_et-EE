<properties
   pageTitle="Milleks on võrgu juurdepääsu juhtimine loend (ACL)?"
   description="Lisateavet ACL-ID"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Mis on lõpp pääsuloendi (ACL)?

Lõpp-punkti juurdepääs juhtelemendi loend (ACL) on saadaval Azure juurutamise turvalisuse parandamiseks. Mõne ACL pakub võimalus valikuliselt lubada või keelata liikluse virtuaalse masina näitaja. Selle paketi filtreerimise võimalus pakub väärtpaberi. Saate määrata võrgu ACL jaoks ainult lõpp-punktid. Te ei saa määrata mõne ACL virtuaalse võrgu või teatud alamvõrku, mis sisalduvad virtuaalse võrgu jaoks.

> [AZURE.IMPORTANT] Soovitatav on kasutada võrgu turberühmad (NSGs) asemel ACL-ID, kui vähegi võimalik. NSGs kohta leiate lisateavet teemast [mis on võrgu turberühma?](virtual-networks-nsg.md).

PowerShelli või selle haldusportaali abil saab konfigureerida ACL-ID. Võrgu ACL konfigureerimiseks PowerShelli abil, vt [Haldamine Accessi juhtelement on loetletud (ACL) jaoks lõpp-punktid PowerShelli abil](virtual-networks-acl-powershell.md). Konfigureerimine võrgus ACL haldusportaali abil, vaadake, [Kuidas lõpp üles virtuaalse masina](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Võrgu ACL-ID abil saate teha järgmist:

- Valikuliselt lubada või keelata liiklust remote alamvõrgu IPv4 aadress vahemiku virtuaalse masina Sisestuskeel lõpp-punkti põhjal.

- Musta IP-aadressid

- Mitme reeglite kohta virtuaalse masina lõpp-punkti loomine

- Saate määrata kuni 50 ACL reeglid virtuaalse masina lõpp-punkti kohta.

- Kasutage reegli tellimine tagamiseks on antud virtuaalse masina lõpp-punkti (madalaim suurimani) on rakendatud õige reeglikomplekti

- Saate määrata mõne kindla remote alamvõrgu IPv4 aadress ACL.

## <a name="how-acls-work"></a>Kuidas töötavad ACL-ID

Mõne ACL on objekti, mis sisaldab loendit reeglid. Kui loote mõnda ACL ja rakendada selle virtuaalse masina lõpp-punkti, paketi filtreerimine toimub host sõlme oma VM. See tähendab, et liiklus IP-aadressid on filtreeritud host sõlm kattuvad ACL reeglite asemel oma VM. See takistab teie VM väärtuslik protsessori kulutada paketi filtreerimine.

Virtuaalse masina loomisel paigutatakse vaikimisi ACL, et blokeeri kogu sissetulev liiklus. Juhul, kui lõpp on loodud (port 3389), siis ACL on muudetud võimaldamiseks kogu sissetulev liiklus selle lõpp-punkti jaoks vaikesätteid. Mis tahes remote alamvõrgu sissetulev liiklus lubatakse siis selle lõpp-punkti ja tulemüüri ettevalmistamise on nõutav. Kõik muud pordid on blokeeritud sissetulev liiklus juhul, kui need pordid on loodud lõpp-punktid. Vaikimisi on lubatud väljaminev liiklus.

**Näiteks vaikimisi ACL tabel**

| **Reegel #** | **Remote alamvõrgu** | **Lõpp-punkti** | **Luba/Keela.** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Luba      |

## <a name="permit-and-deny"></a>Lubada ja keelamiseks

Valikuliselt saate lubada või keelata võrguliiklust virtuaalse masina Sisestuskeel näitaja, luues reeglid, mis määravad "luba" või "keelata". See on oluline tähele, et vaikimisi lõpp loomisel kogu liikluse on lubatud lõpp-punkti. Seetõttu on oluline aru saada, kuidas luba/Keela reeglite loomine ja paigutada need proper tähtsuse järjekorras, kui soovite Varundustöö juhtida võrguliiklust, mis lubate virtuaalse masina lõpp-punkti saavutamiseks.

Millega tuleks arvestada punkte.

1. **Pole ACL –** Vaikimisi lõpp loomisel me luba kõik lõpp-punkti jaoks.

1. **Luba-** Kui lisate ühte või mitut "luba" vahemikud, on kõik muud vahemikud on vaikimisi keelamisega seotud. Ainult pakette lubatud IP vahemikku saab suhelda virtuaalse masina lõpp-punkti.

1. **Keela-** Lisage üks või mitu "keelamiseks" vahemikud, kui annate vaikimisi kõik muud liikluse vahemikud.

1. **Lubada ja keelata - kombinatsioon** Saate kasutada kombinatsiooni "luba" ja "keelamiseks", kui soovite leida teatud IP vahemikus lubatud või keelatud.

## <a name="rules-and-rule-precedence"></a>Reeglid ja reeglite järjestuse kohta

Võrgu ACL-ID saate häälestada, klõpsake teatud virtuaalse masina lõpp-punktid. Näiteks saate määrata RDP lõpp-punkti virtuaalse masina millised lukud alla juurdepääsu teatud IP-aadresse loomisaeg võrk ACL. Järgmises tabelis antakse ülevaade võimalus anda juurdepääsu avaliku virtuaalse IP (VIP) on teatud vahemikus lubada juurdepääsu RDP. Kõik muud serveri IP-d on keelatud. Jälgime *madalaim ülimuslik* reeglite järjekorda.

### <a name="multiple-rules"></a>Mitme reeglid

Järgmises näites, kui soovite lubada RDP lõpp-punkti juurdepääs ainult kahe avaliku IPv4 aadressid (65.0.0.0/8 ja 159.0.0.0/8), võite saavutada seda, määrates kaks *lubada* reeglid. Sel juhul, kuna RDP luuakse vaikimisi virtuaalse masina jaoks, mida soovite lukustada juurdepääsu RDP port remote alamvõrgu põhjal. Järgmises näites on kuvatud võimalus anda juurdepääsu avaliku virtuaalse IP (VIP) on teatud vahemikus lubada juurdepääsu RDP. Kõik muud serveri IP-d on keelatud. See toimib, kuna võrgu ACL-ID saate häälestada mõne kindla virtuaalse masina lõpp-punkti ja juurdepääs keelatud vaikimisi.

**Näide – mitme reeglid**

| **Reegel #** | **Remote alamvõrgu** | **Lõpp-punkti** | **Luba/Keela.** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Luba      |
| 200    | 159.0.0.0/8   | 3389     | Luba      |

### <a name="rule-order"></a>Reegli järjestuses

Kuna lõpp-punkti jaoks saab määrata mitu reeglid, tuleb selleks, et määrata, millist reeglit ülimuslik reeglid korraldada nii, et. Reeglite järjekorda määrab järjestuse. Võrgu ACL järgige *madalaim ülimuslik* reeglite järjekorda. Alltoodud näites valikuliselt antakse port 80 lõpp-punkti juurdepääs ainult teatud IP-aadresside vahemikud. See konfigureerimiseks meil Keela reegli (reegel \# 100) aadresside 175.1.0.1/24 ruumi. Teise reegli on määratud seejärel järjestuse 200, mis lubab juurdepääsu jaotises 175.0.0.0/8 kõik aadressid.

**Näide – reeglite järjestuse kohta**

| **Reegel #** | **Remote alamvõrgu** | **Lõpp-punkti** | **Luba/Keela.** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Keelamiseks        |
| 200    | 175.0.0.0/8   | 80       | Luba      |

## <a name="network-acls-and-load-balanced-sets"></a>Võrgu ACL-ID ja tasakaalustatud komplekti laadimine

Võrgu ACL-ID saate määrata koormus tasakaalustatud määramine (LB Set) lõpp-punkti. Kui mõni ACL jaoks on määratud LB kogum, võrgu ACL rakendatakse kõik Virtuaalmasinates, et LB seadmine. Näiteks kui LB kogum on loodud "Port 80" ja LB kogum sisaldab 3 VMs, võrgu ACL loomisaeg lõpp-punkti "Pordi 80" ühe VM rakendatakse automaatselt teiste VMs.

![Võrgu ACL-ID ja tasakaalustatud komplekti laadimine](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Järgmised sammud

[Kuidas hallata juurdepääsu juhtimine loendid (ACL) lõpp-punktide jaoks PowerShelli abil](virtual-networks-acl-powershell.md)
