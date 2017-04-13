
<properties
    pageTitle="Office'i kasutamise Azure RemoteAppi | Microsoft Azure'i" 
    description="Siit saate teada, kuidas Office ja Azure RemoteApp koos töötada"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Office'i kasutamise Azure RemoteAppi

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Teil on kaks võimalust Office'i rakenduste Azure RemoteApp hosting: Office 365 ProPlus või Office 2013 Professional Plusi prooviversioon.

**Kuule, kas teadsite, meil on uus, on parem artikkel, mis asendab selle kiiresti? Vaadake, [Kuidas kasutada Office 365 tellimuse Azure RemoteApp](remoteapp-officesubscription.md). See hõlmab kogu teave, mida vajate Office 365 + Azure RemoteApp abil.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Saate luua RemoteApp kogum abil teenusekomplekti Office 365 ProPlus malli pilti. See suvand võimaldab teil oma Office 365 teenuse hõlmavad RemoteApp. Olemasoleva tellimuse plaani peab teil ja teie kasutajad peavad olema Office 365 ProPlusi teenuse, kas eraldi litsentsitud või teenuse kaudu Office 365 lepingud.

RemoteApp toetab Office 365 ühiskasutatavas arvutis aktiveerimisega. Kui teil lubada ühiskasutatavas arvutis aktiveerimisega ja installimiseks [Office'i juurutamise tööriista](http://www.microsoft.com/download/details.aspx?id=36778) kasutamine, Office 365 ProPlus installib ilma on aktiveeritud. Kui kasutaja märgid kollektsiooni, mis sisaldab Office 365, kontrollib Office kuvamiseks, kui kasutaja on Office 365 ProPlus ette valmistatud. Kui nii, et Office'i ajutiselt aktiveerib Office 365 ProPlus – selle aktiveerimise ei lahene, kuni see kasutajate märke kokku teenuse.

Office 365 ühiskasutatavas arvutis aktiveerimisega kasutamiseks peate [Kohandatud malli](remoteapp-create-custom-image.md) loomine ja installige Office 365 ProPlus, jälgima [neid juhiseid](https://technet.microsoft.com/library/dn782858.aspx).

Saate hallata oma kasutajate Office 365 litsentse [Office 365 halduskeskuse kaudu](https://portal.office365.com/). Lugege lisateavet [Office 365 teenuse lepingud](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plusi prooviversioon
Ajal RemoteApp 30-päevast prooviversiooni, saate Office 2013 Professional Plusi (prooviversioon) malli pilti RemoteApp saidikogumi loomiseks. Saate määrata selle nende Azure Active Directory töö kontod või Microsofti kontot kasutades prooviversiooni saidikogumi kasutajad. Täiendavad tellimust on nõutav.

See on suurepärane võimalus alustada rehvid ja saada hea tunne Office'i RemoteApp. Kuid see suvand on mõeldud hindamise ja testimise ainult. Pilt Office 2013 Professional Plusi (prooviversioon) malli abil loodud RemoteApp saidikogumid ei saa olla transitioned tekitamise režiimi ja keelatakse prooviperioodi lõpus.

## <a name="switching-from-trial-to-production"></a>Üleminek rakenduselt prooviversiooni tootmisele
30-päevane tasuta prooviversiooni käivitamisel märkme portaali jaotises RemoteApp ütleb teile, kui kaua on jäänud kohtuprotsessi enne üleminekut tasutud konto. Saate oma konto aktiveerida ja lingi kaudu selle teate tekitamise režiimi aktiveerimine.

Kui teie konto aktiveerimiseks see mõjutab RemoteApp saidikogumid kontole.

- Saidikogumite, kus töötab Windows Server 2012 R2 või Office 365 ProPlusi malli pildid on üleminek tootmise sujuvalt. Kõik kasutaja andmed ja sätted, sh poolelioleva, jäävad samaks.
- Kui olete kohandatud malli pildid alla laadinud, saidikogumite neid pilte kasutada ka sujuv kasutamisele.
- Office 2013 Professional Plusi (prooviversioon) malli pilti on mõeldud ainult hindamiseks. Saidikogumite töötamise selle malli pilti ei saa transitioned tootmisele. Kasutusele "keelatud" olekus.


Kui te pole üleminek tekitamise režiimi prooviversiooni aegumine, keelatakse oma RemoteApp saidikogumid. Ärge muretsege – sätete ja kasutajate andmed salvestatakse veel 90 päeva, nii saate aktiveerida teenust ja aktiveerige tekitamise režiimi andmete kaotamata.
