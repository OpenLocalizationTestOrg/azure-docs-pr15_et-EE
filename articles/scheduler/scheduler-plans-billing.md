<properties
 pageTitle="Lepingud ja arvelduse Azure ajasti"
 description="Lepingud ja arvelduse Azure ajasti"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Lepingud ja arvelduse Azure ajasti

## <a name="job-collection-plans"></a>Töö saidikogumi lepingud

Töö saidikogumid on Azure ajasti tasustatav üksus. Töö saidikogumid sisaldavad töökohtade arv ja tulevad kolme lepingud – tasuta, Standard ja Premium –, mida on kirjeldatud allpool.

|**Töö saidikogumi kavandamine**|**Max # töökohtade töö saidikogumi kohta.**|**Max Korduvus**|**Max töö saidikogumid tellimuse kohta**|**Piirangud**|
|:---|:---|:---|:---|:---|
|**Tasuta**|5 töö töö saidikogumi kohta.|Üks kord tunnis. Ei saa käivitada töö sagedamini kui üks kord tunnis|Tellimust on lubatud kuni 1 tasuta töö saidikogumi|[HTTP väljaminev autoriseerimine objekti](scheduler-outbound-authentication.md) ei saa kasutada
|**Standard**|50 töö töö saidikogumi kohta.|Üks kord minutis. Ei saa käivitada sagedamini üks minut tööde haldamine|Tellimust on lubatud kuni 100 standard töö saidikogumid|Juurdepääs täielik funktsioonikomplekt Scheduler.|
|**P10 Premium**|50 töö töö saidikogumi kohta.|Üks kord minutis. Ei saa käivitada sagedamini üks minut tööde haldamine|Tellimust on lubatud kuni 10 000 P10 Premium töö saidikogumid. Lisateavet <a href="mailto:wapteams@microsoft.com">võtke meiega ühendust</a> .|Juurdepääs täielik funktsioonikomplekt Scheduler.|
|**20 Premium**|1000 töö töö saidikogumi kohta.|Üks kord minutis. Ei saa käivitada sagedamini üks minut tööde haldamine|Tellimust on lubatud kuni 10 000 20 Premium töö saidikogumid. Lisateavet <a href="mailto:wapteams@microsoft.com">võtke meiega ühendust</a> .|Juurdepääs täielik funktsioonikomplekt Scheduler.|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Uuendamine ja alandamisega töö saidikogumi lepingutest

Võite versiooniuuenduse või alandada töö saidikogumi leping igal ajal tasuta, Standard, ja lepingute vahel. Juhul, kui kasutuselevõttu tasuta töö kogumi, on allavahetamise võib nurjuda ühel järgmistest põhjustest:

- Tasuta töö saidikogumi juba olemas tellimuse
- Töö saidikogumi tööd on suurem Korduvus, kui töökohtade tasuta töö saidikogumite jaoks lubatud. Lubatud tasuta töö kogumi suurima kordumist on üks kord tunnis.
- Töö saidikogumi on rohkem kui 5 tööde haldamine
- Töö saidikogumi tööd on HTTP- või HTTPS toimingut, mis kasutab [HTTP väljaminev autoriseerimine objekti](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Arveldus- ja Azure lepingud

Tellimusi ei võeta tasuta töö saidikogumid. Kui teil on rohkem kui 100 standard töö saidikogumid (10 standard arvelduse ühikud), siis on paremad olema töö kõigi saidikogumite premium leping.

Kui teil on ühe tavalise töö kogumine ja ühe premium töö saidikogumi, olete hind ühe standard arvelduse üksus _ja_ ühe premium arvelduse üksus. Aktiivse töö saidikogumid, mis on seatud standard-või premium; arvu põhjal Scheduler teenuse arved See on põhjalikumalt kirjeldatud järgmistes.

## <a name="standard-billable-units"></a>Standard arveldatavate üksused

Standard tasustatav üksus võib sisaldada kuni 10 standard töö saidikogumid. Kuna standard töö saidikogumi võib olla kuni 50 töökohta töö saidikogumi, üks standard arvelduse üksus võimaldab tellimust on kuni 500 töö – kuni peaaegu 22 miljonit töö täitmised kuus.

Kui teil on 1 ja 10 standard töö kogumite, kuvatakse arve 1 standard arvelduse üksuse. Kui teil on 11-20 standard töö kogumite, kuvatakse arve jaoks 2 standard arvelduse üksused. Kui teil on 21 ja 30 standardne töö saidikogumite vahel, saate arveldamine toimub 3 standard arvelduse üksuste jne.

## <a name="p10-premium-billable-units"></a>P10 Premium arveldatavate ühikud

P10 premium tasustatav üksus võib sisaldada kuni 10 000 P10 premium töö saidikogumid. Kuna P10 premium töö saidikogumi võib olla kuni 50 töökohta töö saidikogumi, premium arvelduse ühiku võimaldab tellimust on kuni 500 000 töö – kuni peaaegu 22 miljonit töö täitmised kuus.

Kui teil on 1 kuni 10 000 premium töö kogumite, kuvatakse arve 1 P10 premium arvelduse üksuse. Kui teil on 10,001 ja 20 000 premium töö saidikogumite vahel, saate arveldamine toimub 2 P10 premium arvelduse üksuste jne.

Seega P10 premium töö saidikogumid on samu funktsioone, mis standard töö saidikogumid, kuid pakuvad hinna piiri juhuks, kui teie rakendus nõuab palju tööd saidikogumid.

## <a name="p20-premium-billable-units"></a>20 Premium arveldatavate ühikud

20 premium tasustatav üksus võib sisaldada kuni 5000 20 premium töö saidikogumid. Kuna 20 premium töö saidikogumi võib olla kuni 1000 töökohta töö saidikogumi, premium arvelduse ühiku võimaldab tellimust on kuni 5 000 000 töö – kuni peaaegu 220 miljardit töö täitmised kuus.

20 premium töö saidikogumid pakub samu funktsioone nagu P10 premium töö saidikogumid, kuid toetab ka suurem arv töökohtade töö kogumise kohta ja suurem koguarvu töö üldine kui P10 premium, mis võimaldab teil veel skaleeritavus.

## <a name="billing-and-active-status"></a>Arveldus- ja aktiivne olek

Töö saidikogumid on alati aktiivne, kui tellimuse kogu on läinud mõned ajutised keelatud olekusse tõttu arvete küsimused. Kuidas tagada, et töö saidikogumi on teile arve ainult on kas määraks _tasuta_ leping või kustutamiseks töö saidikogumi.

Kuigi saate keelata kõik tööd töö saidikogumi ühekordne sees, ei muuda arveldustoe töö saidikogumi olekut – töö saidikogumi on _endiselt_ arveid. Samuti tühja töö saidikogumid käsitletakse aktiivne ja arve.

## <a name="pricing"></a>Hinnad

Üksikasjad, hindade kohta leiate teemast [Scheduler hinnad](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Vt ka


 [Mis on ajasti?](scheduler-intro.md)

 [Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Azure'i Scheduler kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Azure'i Scheduler piirangud, vaikesätted ja kuvatavad tõrkekoodid](scheduler-limits-defaults-errors.md)

 [Azure'i Scheduler väljaminevat autentimist](scheduler-outbound-authentication.md)
