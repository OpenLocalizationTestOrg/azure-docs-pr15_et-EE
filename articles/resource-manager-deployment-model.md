<properties
   pageTitle="Ressursihaldur ja klassikaline juurutamise | Microsoft Azure'i"
   description="Ressursihaldur juurutamise mudeli ja klassikaline erinevused kirjeldatakse (või teenuse haldus) juurutamise mudel."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure'i ressursihaldur vs klassikaline juurutus: juurutuse mudelite ja oma seisundi mõistmine

Selles teemas õpite tundma Azure'i ressursihaldur ja klassikaline juurutamise mudelid, oma ressursse, oleku ja miks oma ressursse olid juurutatud ühe või teise. Ressursihaldur ja klassikaline juurutamise mudelite tähistavad kaks erinevat juurutamine ja hallata oma Azure lahendusi. Saate nendega töötada, kuni kahte eri API ja juurutatud ressursside võib olla tähtis erinevused. Kahe mudeli pole täielikult omavahel. Selles teemas kirjeldatakse nende erinevusi.

Microsoft soovitab juurutus- ja ressursside haldamise lihtsustamiseks ressursihaldur kasutada kõiki uusi ressursse. Võimaluse korral soovitab Microsoft kasutada, et te ümberpaigutamiseks olemasoleva ressursihaldur kaudu.

Kui olete uue ressursi Manager, mida soovite Lugege esmalt läbi [Azure'i ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md)määratletud terminid.

## <a name="history-of-the-deployment-models"></a>Juurutamise mudelite ajalugu

Azure'i algselt esitatud ainult klassikaline juurutamise mudel. Selle mudeli iga ressursi olemas iseseisvalt; puudus võimalus seotud ressursid rühmitada. Selle asemel pidite käsitsi tehtud lahenduste või rakenduste häälestamine ressursside jälgida ja pidage meeles, et hallata koordineeritud. Lahenduse juurutamiseks pidite loomine iga ressursi eraldi klassikaline portaali kaudu või luua skripti, mis juurutatud kõik ressursid õiges järjestuses. Lahenduse kustutamiseks pidite kustutada iga ressurss ükshaaval. Saate hõlpsasti ei rakendamine ning värskendada juurdepääsupoliitikaid juhtelemendi seotud ressursid. Lõpetuseks, mida ei saa rakendada silte, et ressursse, mis sildi nende tingimustega, mille abil saate jälgida oma ressursse ja arveldust hallata.

Azure'i sisse 2014 ressursihaldur, mida lisada mõiste ressursirühma. Ressursirühma on ümbris jagavad ühist elutsükli ressursid. Ressursihaldur juurutamise mudeli pakub mitmeid eeliseid.

- Saate juurutada, hallata ja jälgida kõiki teenuseid teie lahendus rühmana, mitte eraldi töötlemise järgmisi teenuseid.
- Saate korduvalt juurutada lahendus kogu elutsükli ja usaldada oma ressursid on juurutatud ühtsete olekus.
- Saate rakendada juurdepääsu reguleerimine kõigile ressurssidele ressursirühma ja nendes sätestatud nõuetele rakendatakse automaatselt, kui lisatakse uus ressursid ressursirühma.
- Saate rakendada siltide ressursid kõik teie tellimus ressursside loogiliselt korraldamiseks.
- JavaScript Object märke (JSON) abil saate määratleda infrastruktuuri oma lahenduse. JSON-fail on tuntud ressursihaldur malli.
- Saate määratleda sõltuvuste ressursid nii, et need on juurutatud õiges järjestuses.

Ressursihaldur lisamisel lisati kõik ressursid tagasiulatuvalt vaikerühmad ressursi. Kui loote ressursi klassikaline kasutamise nüüd kaudu, ressursi luuakse automaatselt selle teenuse rühmas vaikimisi ressursi isegi juhul, kui te pole määranud selle ressursirühma juurutamise juures. Siiski lihtsalt olemasolevate ressursirühma ei tähenda, et ressursi teisendada ressursihaldur mudel. Vaatame, kuidas käsitleb iga teenuse kaks juurutamise mudelite järgmises jaotises. 

## <a name="understand-support-for-the-models"></a>Mõista mudelid tugi 

Otsustamisel, milline juurutamise mudeli kasutada oma ressursse, on kolm stsenaariumid, millega peaks arvestama.

1. Teenuse toetab ressursihaldur ja sisaldab ainult ühte tüüpi.
2. Teenuse toetab ressursihaldur, kuid pakub kahte tüüpi - üks ressursihaldur ja üks Classic. See stsenaarium kehtib ainult virtuaalmasinates, salvestusruumi kontod ja virtuaalse võrgu.
3. Teenust ei toeta ressursihaldur.

Vaadake, kas teenuse toetab ressursihaldur, lugege teemat [ressursihaldur toetatud pakkujad](resource-manager-supported-services.md).

Kui soovite kasutada teenust ei toeta ressursihaldur, peate edasi klassikaline juurutamise abil.

Kui teenus toetab ressursihaldur ja **pole** virtuaalse masina, salvestusruumi konto või virtuaalse võrgu, saate kasutada mis tahes rahulik ressursihaldur.

Virtuaalmasinates, salvestusruumi kontod ja virtuaalne võrkude, kui ressursi luuakse – klassikaline juurutus, peate edasi töötada klassikaline toimingute kaudu. Kui virtuaalse masina, salvestusruumi konto või virtuaalse võrgu loodi ressursihaldur kasutamise kaudu, peate edasi ressursihaldur toimingute abil. Seda vahet pääsevad selgem, kui teie tellimus sisaldab erinevatest allikatest, mis on loodud ressursihaldur ja klassikaline juurutamise kaudu. See kombinatsioon ressursid saate luua ootamatud tulemused ressursside ei toeta samu toiminguid.

Mõnel juhul ressursihaldur käsk saate tuua teavet ressursi klassikaline juurutamise abil loodud või saab teha näiteks klassikalises ressursside teisaldamine teise ressursirühm haldustoimingut. Kuid sellisel juhul ei tohiks anda mulje, et tüüp toetab ressursihaldur toimingud. Oletame näiteks, et teil on virtuaalse masina klassikaline juurutamise loodud sisaldava ressursirühma. Kui käivitate ressursihaldur PowerShelli järgmine käsk:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Tagastab see virtuaalse masina:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Ressursihaldur cmdlet-käsk **Get-AzureRmVM** tagastab ainult virtuaalmasinates juurutatud ressursihaldur kaudu. Järgmine käsk ei tagasta virtuaalse masina loodud klassikaline kasutamise kaudu.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Ainult ressursid ressursihaldur tugi siltide abil loodud. Te ei saa rakendada klassikalises ressursside sildid.

## <a name="resource-manager-characteristics"></a>Ressursihaldur omadused

Et aidata teil mõista kaks mudelit, vaatame ressursihaldur tüüpi omadusi:

- Loodud [Azure portaali](https://portal.azure.com/)kaudu.

     ![Azure'i portaal](./media/resource-manager-deployment-model/portal.png)

     Arvuta, salvestusruumi ja Networking ressursse, on teil võimalus kasutada ressursihaldur või klassikaline juurutamise. Valige **Ressursihaldur**.

     ![Ressursihaldur juurutamine](./media/resource-manager-deployment-model/select-resource-manager.png)

- Loodud ressursihaldur versiooni Azure PowerShelli cmdlet-käskude abil. Need käsud on *Tegusõna-AzureRmNoun*vorming.

        New-AzureRmResourceGroupDeployment

- Loodud [Azure'i ressursihaldur REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) ülejäänud toimingute kaudu.

- Loodud läbi Azure'i CLI käsud **arm** režiimis.

        azure config mode arm
        azure group deployment create 

- Ressursi tüüp ei sisalda nimi **(klassikaline)** . Järgmisel pildil on kujutatud **salvestusruumi**konto tüüp.

    ![web app](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klassikaline juurutamise omadused

Te teate ka klassikaline juurutamise mudeli Teenusehaldus mudel.

Klassikaline juurutamise mudeli loodud ressursside ühiskasutus järgmised omadused:

- [Klassikaline portaali](https://manage.windowsazure.com) kaudu loodud

     ![Klassikaline portaal](./media/resource-manager-deployment-model/classic-portal.png)

     Või Azure portaali ja määrake **klassikaline** juurutamise (Arvuta, salvestusruumi ja võrgunduse) jaoks.

     ![Klassikaline juurutamine](./media/resource-manager-deployment-model/select-classic.png)

- Loodud Teenusehaldus versioon Azure PowerShelli cmdlet-käskude abil. Need käsk nimed on *Tegusõna-AzureNoun*vorming.

        New-AzureVM 

- Loodud [Teenuse haldamine REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) ülejäänud toimingute kaudu.
- Loodud läbi Azure'i CLI käsud **asm** režiimis.

        azure config mode asm
        azure vm create 

- Ressursi tüüp sisaldab nimi **(klassikaline)** . Järgmisel pildil on kujutatud tüübi **talletusruumi**kontona (klassikaline).

    ![klassikaline tüüp](./media/resource-manager-deployment-model/classic-type.png)

Azure portaali abil saate hallata ressursse, mis on loodud klassikaline kasutamise kaudu.

## <a name="changes-for-compute-network-and-storage"></a>Arvuta, võrgu ja salvestamise muudatused

Järgmine diagramm kuvab Arvuta, võrgu ja salvestusruumi ressursid juurutatud ressursihaldur kaudu.

![Ressursihaldur arhitektuur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Võtke arvesse järgmisi ressursse vahelisi seoseid.

- Kõik ressursid eksisteerivad ressursirühma.
- Virtuaalse masina sõltub teatud salvestusruumi konto, määratletud salvestusruumi ressursi pakkuja salvestada oma ketast bloobimälu (nõutav).
- Virtuaalse masina viitab teatud NIC, mis on määratletud (nõutav) ressursi pakkuja ja saadavus seadmine määratletud Arvuta ressursi pakkuja (valikuline).
- NIC viitab virtuaalse masina määratud IP-aadress (nõutav), alamvõrgu virtuaalse võrgu jaoks virtuaalse masina (nõutav) ja võrgu turberühma (valikuline).
- Alamvõrgu virtuaalse võrgustikus viited võrgu turberühma (valikuline).
- Laadi koormusetasakaalustusteenuse eksemplari viited taustväärtus kogumi IP-aadressid, mis sisaldavad virtuaalse masina (valikuline) NIC ja viidete lisamine laadi koormusetasakaalustusteenuse avalik või privaatne IP-aadress (valikuline).

Siin on komponendid ja nende seoste klassikaline juurutamiseks.

![klassikaline arhitektuur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Klassikaline lahendus hosting virtuaalse masina sisaldab järgmist:

- Nõutav pilveteenuses, mis toimib ümbris majutusteenuse virtuaalmasinates (Arvuta). Virtuaalmasinates automaatselt pakutakse kasutajaliidese kaart (NIC) ja Azure määratud IP-aadress. Lisaks sisaldab pilveteenusesse välise koormuse koormusetasakaalustusteenuse eksemplar, avaliku IP-aadress ja vaikimisi lõpp-punktid Remote'i töölaua- ja remote PowerShelli liikluse Windowsi-põhiste virtuaalmasinates ja Linux-põhine virtuaalmasinates Secure Shell (SSH) liikluse lubamiseks.
- Nõutav salvestusruumi konto, mis talletab VHDs virtuaalse masina, sh operatsioonisüsteemi, ajutised ja täiendavad andmed ketast (salvestusruumi) jaoks.
- Valikuline virtuaalse võrgu, mis toimib täiendava ümbrises, kus saate luua subnetted struktuuri ja määrata alamvõrgu, kus asub virtuaalse masina (võrgus).

Järgmises tabelis kirjeldatakse, kuidas nendega Arvuta, võrgu ja salvestusruumi ressursi pakkujad muudatuste:

 Üksuse | Klassikaline | Ressursihaldur
 ---|---|---
| Pilveteenuses Virtuaalmasinates jaoks |  Pilveteenuses oli ümbris hoides virtuaalmasinates, mis nõutavate kättesaadavus platvormi ja Koormusetasakaalustuseks. | Pilveteenuses pole enam vaja luua uue näidise virtuaalse masina objekti. |
| Virtuaalne võrkude | Virtuaalse võrk on vabatahtlik virtuaalse masina. Kui, ei saa koos ressursihaldur juurutatud virtuaalse võrgu. | Virtuaalne arvuti nõuab virtuaalse võrk, mis on kasutusele võetud ressursihaldur abil. |
| Salvestusruumi kontod | Virtuaalse masina jaoks on vaja salvestusruumi konto, mis talletab operatsioonisüsteemi ajutiste ja täiendavate andmete ketast VHDs. | Virtuaalse masina jaoks on vaja salvestusruumi konto bloobimälu oma ketast talletamiseks. |
| Kättesaadavus komplektid | Kättesaadavus platvormi on tähistatud konfigureerimine on Virtuaalmasinates sama "AvailabilitySetName". Suurim lubatud arv viga domeenid on 2. | Kättesaadavus on esitatud Microsoft.Compute pakkuja ressursi. Virtuaalmasinates, mis nõuavad kõrge-saadavus tuleb kaasata kättesaadavus seadmine. Suurim lubatud arv viga domeenid on nüüd 3. |
| Osaleja rühmad | Osaleja rühmad oli virtuaalse võrgu loomine. Siiski kehtestamine virtuaaltöölaua piirkondliku võrke, mis ei vaja enam. |Osaleja rühmade mõistet pole lihtsustamiseks API-de esitatud Azure ressursihaldur kaudu. |
| Laadi tasakaalustamiseks    | Pilveteenus loomine pakub on peidetud laadi koormusetasakaalustusteenuse Virtuaalmasinates, juurutatud. | Laadi koormusetasakaalustusteenuse on esitatud Microsoft.Network pakkuja ressurss. Esmane võrgu liidese tuleb koormus tasakaalustatud Virtuaalmasinates peaks viitamine laadi koormusetasakaalustusteenuse. Koormus soolise võib olla sise- või. Laadi koormusetasakaalustusteenuse eksemplari viited taustväärtus kogumi IP-aadressid, mis sisaldavad virtuaalse masina (valikuline) NIC ja viidete lisamine laadi koormusetasakaalustusteenuse avalik või privaatne IP-aadress (valikuline). [Lugege lisateavet.](../articles/resource-groups-networking.md) |
|Virtuaalne IP-aadress | Pilveteenustega kuvatakse vaikimisi VIP (virtuaalne IP-aadress) VM lisatakse pilveteenusesse. Virtuaalne IP-aadress on seostatud peidetud laadi koormusetasakaalustusteenuse aadress.    | Avaliku IP-aadress on esitatud Microsoft.Network pakkuja ressurss. Avaliku IP-aadress võib olla staatiline (reserveeritud) või dünaamiline. Dünaamiliste avaliku IP-d saab määrata laadi koormusetasakaalustusteenuse. Avaliku IP-d saab muuta turvaliseks turberühmad. |
|Reserveeritud IP-aadress|   Saate reserveerida Azure IP-aadress ja seostada pilveteenuses tagamaks, et IP-aadress on sticky.   | "Staatiline" režiimis saab luua avaliku IP-aadressi ning pakub sama võimalus "Reserveeritud IP-aadress". Staatilise avaliku IP-d saab määrata ainult laadimise koormusetasakaalustusteenuse kohe. |
|Avaliku IP-aadress (PIP) VM kohta. | Avaliku IP-aadressid ka võib olla seotud VM otse. | Avaliku IP-aadress on esitatud Microsoft.Network pakkuja ressurss. Avaliku IP-aadress võib olla staatiline (reserveeritud) või dünaamiline. Siiski ainult dünaamiline avaliku IP-d saab määrata võrgu liidese saada avaliku IP VM kohta kohe. |
|Lõpp-punktid| Sisestuskeel lõpp-punktid vaja konfigureerida Virtual arvutis olevat avada teatud portide Ühenduvus. Üks levinud viise ühenduse virtuaalmasinates valmis häälestades Sisestuskeel lõpp-punktid. | Klõpsake koormus soolise saavutada sama võimalus võimaldada lõpp-punktid teatud pordid ühenduse VMs kohta saab konfigureerida NAT sissetulevad reeglid. |
|DNS-i nimi.| Pilveteenus saaksin peidetud globaalselt kordumatu DNS-i nimi. Näide: `mycoffeeshop.cloudapp.net`. | DNS-i nimi on valikulised parameetrid avaliku IP-aadressi ressursi määratud. FQDN on järgmises vormingus - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Võrgu liidesed | Esmane ja teisene võrgu kasutajaliidese ja selle atribuute on määratletud võrgukonfiguratsioon virtuaalse masina. | Võrgu liidese on esitatud Microsoft.Network pakkuja ressurss. Võrgu liidese elutsükli ei ole seotud virtuaalse masina. See viitab virtuaalse masina määratud IP-aadress (nõutav), alamvõrgu virtuaalse võrgu jaoks virtuaalse masina (nõutav) ja võrgu turberühma (valikuline). |

Erinevate juurutamise mudelite põhjal virtuaalne võrkude ühendamise kohta leiate teemast [ühenduse loomine virtuaalse võrgu portaalis eri juurutamise mudelite põhjal](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Migreerimine klassikalises ressursside haldur

Kui olete valmis oma ressursse klassikaline juurutamise migreerimine ressursihaldur juurutus, siis lugege:

1. [Tehnika deep dive platvormi toetatud migratsiooni klassikaline: Azure'i ressursihaldur](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Toetatud platvormi migreerimise IaaS ressursside klassikaline Azure'i ressursihaldur](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migreerimine IaaS ressursid klassikaline Azure'i ressursihaldur Azure PowerShelli abil](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Migreerimine IaaS ressursid klassikaline Azure'i ressursihaldur Azure'i CLI abil](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Saate luua virtuaalse masina Azure'i ressursihaldur abil saate juurutada virtuaalse võrgu või klassikaline juurutamise abil loodud salvestusruumi konto?**

See ei toetata. Ei saa kasutada Azure ressursihaldur juurutamine virtuaalse masina virtual võrku, mis on loodud, kasutades klassikaline juurutamise.

**Saate luua virtuaalse masina Azure'i ressursihaldur kasutaja pildilt, Azure'i teenuse haldus API-de abil loodud?**

See ei toetata. Siiski saate kopeerida VHD failide salvestusruumi konto, mis on loodud teenuse haldus API-de kasutamine ja Azure ressursihaldur kaudu luua uue konto lisamine.

**Mis on selle mõju oma tellimust kvoot?**

Kvootide virtuaalmasinates, virtuaalne võrkude ja salvestusruumi kontod loodud Azure'i ressursihaldur on eraldi muude piirmäärasid. Iga tellimuse saab kvootide abil uue API-de ressursside loomine. Lugege lisateavet täiendavate kvootide kohta [siin](../articles/azure-subscription-service-limits.md).

**Saate kasutada automatiseeritud skriptide minu ettevalmistamise virtuaalmasinates, virtuaalne võrkude ja salvestusruumi kontod ressursihaldur API-de kaudu jätkata?**

Kõik automatiseerimine ja skriptide, mille olete varem olemasoleva virtuaalmasinates loodud Azure'i Teenusehaldus režiimi virtuaalse võrgu jaoks tööd jätkata. Skriptide tuleb kasutada uue skeemi loomine sama ressursside kaudu ressursihaldur režiimi värskendada.

**Kui leiate Azure'i ressursihaldur mallide näited**

Täielik kogum starter malle leiate [Azure'i ressursihaldur Kiirjuhend Mallid](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Järgmised sammud

- Mall, mis määratleb virtuaalse masina loomine tegemiseks salvestusruumi konto ja virtuaalse võrgu, vt [ressursihaldur Mall ülevaade](resource-manager-template-walkthrough.md).
- Käskude malli kasutamise kohta leiate teemast [Deploy rakenduse Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).
