<properties
    pageTitle="DevTest Labs mõisted | Microsoft Azure'i"
    description="Siit saate teada, DevTest Labs ja kuidas seda teha seda hõlpsalt luua, hallata ja jälgida Azure'i virtuaalmasinates põhimõtted"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

#<a name="devtest-labs-concepts"></a>DevTest Labs mõisted

> [AZURE.NOTE]
> See artikkel on osa 3 3 osa sarjast:
> 
> 1. [Mis on DevTest Labs?](devtest-lab-overview.md)
> 1. [Miks DevTest Labs?](devtest-lab-why.md)
> 1. **[DevTest Labs mõisted](devtest-lab-concepts.md)**

##<a name="overview"></a>Ülevaade

Järgmine loend sisaldab võtme DevTest Labs mõisted ja määratlused:

##<a name="artifacts"></a>Esemeid
Esemeid kasutatakse juurutada ja konfigureerida rakenduse pärast VM on ette valmistatud. Esemeid võib olla:

- Tööriistad, mida soovite installida VM – nt agentide, viiuldaja ja Visual Studio.
- Toimingud, mida soovite käivitada VM – näiteks kloonimine repo.
- Rakendused, mida soovite testida.

Esemeid on [Azure ressursihaldur](../azure-resource-manager/resource-group-overview.md) JSON failid, mis sisaldavad juhiseid, et teha juurutamise ja rakendada konfigureerimine. 

##<a name="artifact-repositories"></a>Artefakt hoidlate
Artefakt hoidlate on git hoidlate, kus esemeid on märgitud. Sama artefakt hoidlate saate lisada mitu labs ettevõtte lubamine taaskasutuse ja ühiskasutus.

## <a name="base-images"></a>Base pildid
Base kujutised on VM tööriistad ja sätted eelinstallitud ja konfigureerinud kiiresti luua VM. Saate VM, valides mõne olemasoleva base ja lisamise artefakt installida oma testi agent ette valmistada. Seejärel ettevalmistatud VM alusena salvestada, et alus saab kasutada ilma installida test agent iga VM ettevalmistamine.

##<a name="formulas"></a>Valemid 
Valemid, lisaks base pilte, sisestage kiiresti VM ettevalmistamise süsteem. Valemi DevTest Labsissa on atribuut vaikeväärtuste lab VM loomiseks kasutatud loend. Valemitega VMs atribuudid – näiteks pilti, VM suurus, virtuaalse võrgu ja esemeid – sama valiku saab luua automaatselt iga kord, kui nende atribuutide määramiseks ilma. VM: valemi loomisel vaikeväärtused saab kasutada-on või muudetud.

##<a name="caps"></a>Suurtähelukk
Suurtähelukk on teie lab suhtes minimeerimiseks. Näiteks saate piirata VMs saab luua kasutaja kohta, või lab arvu ülempiir.

##<a name="policies"></a>Poliitika
Poliitika aidata kontrollida oma lab maksumus. Näiteks saate luua poliitika VMs määratletud ajakava põhjal automaatselt sulgeda.

##<a name="security-levels"></a>Turvalisuse tasemed
Turvaline juurdepääs määratakse Azure Role-Based juurdepääsu juhtimine (RBAC). Mõistmaks, kuidas access töötab, aitab see luba, rollid ja mille ulatus RBAC määratletud erinevuste mõistmine. 

- Luba - luba on määratletud juurdepääsu teatud toimingu (nt lugemis-juurdepääsu kõik virtuaalmasinates). 
- Roll – roll on kogumi õigused, mida saab rühmitatud ja kasutajale määratud. Näiteks *tellimuse omanik* roll on juurdepääs kõigi ressursside tellimus. 
- Ulatus – mille ulatus on hierarhia on Azure ressurss hierarhia - nt ressursirühma, ühte lab või kogu tellimuse).
 
Piires DevTest Labs, on kahte tüüpi rollid kasutaja õiguste määramiseks: lab omanik ja kasutajale lab.

- Lab omaniku - lab omanik on juurdepääs mis tahes lab ressursse. Seetõttu lab omanik saab muuta poliitika, lugeda ja kirjutada mis tahes VMs, virtuaalse võrgu muutmine jne. 
- Lab kasutaja - lab kasutaja saavad vaadata kõik lab ressursse, näiteks VMs, poliitika ja virtuaalse võrgu, kuid ei saa muuta poliitikate või teiste kasutajate loodud mis tahes VMs. 


Et näha, kuidas luua kohandatud rollid DevTest Labs, lugege artiklit, [teatud lab poliitikate kasutusõiguste andmine](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Kuna otsinguulatuste hierarhilise, kui kasutaja on õigused teatud ulatusega, antakse need automaatselt need õigused iga hõlmas madalama taseme ulatuses. Näiteks, kui kasutaja on määratud roll tellimuse omanik, siis need on juurdepääs tellimus, kõik ressursse, mis sisaldavad kõik virtuaalmasinates, kõik virtuaalse võrgu ja kõik labs. Seetõttu tellimuse omanik pärib automaatselt lab omanik roll. Vastand pole täidetud. Lab omanikul juurdepääsu lab, mis on väiksem ulatus, kui tellimus. Seetõttu ei saa lab omanik virtuaalmasinates või virtuaalse võrgu või mis tahes ressursse, mis on väljaspool lab.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

##<a name="next-steps"></a>Järgmised sammud

[Lab DevTest Labsissa loomine](devtest-lab-create-lab.md)