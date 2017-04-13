<properties
   pageTitle="Hankige sama Office 365 Azure'i RemoteAppi mis tahes seadmes | Microsoft Azure'i"
   description="Saate teada, kuidas jagada mõne Office 365 rakenduse kasutajate Azure RemoteApp abil."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Hankige sama Office 365 Azure'i RemoteAppi mis tahes seadmes

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Käesolevas artiklis hõlmab mis tahes seadmes oma ettevõtte Office 365 juurutamise. Teie kasutajad pääsevad sama ja UI kasutusvõimalusi: Android, Apple ja Windows.

Kas saavutada seda Azure RemoteApp hosting skaala võimalik virtuaalmasinates Azure, mida kasutajad saavad ühenduse Office 365 abil. Selleks väärtuseks virtuaalmasinates kutsume kogumik"pilve".

## <a name="create-a-cloud-collection"></a>Pilveteenuse saidikogumi loomine

Esmalt pärast loomist Azure'i konto, liikuge **RemoteApp** , klõpsates linki vasakus servas.
![Näitab Azure RemoteApp Azure'i portaalis](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Seejärel jätkata, klõpsates nuppu **Uus** alumises ja "kiire loomine" kogumi. Sisestage nimi, piirkond, tellimus, leping ja pilt "Office Proffesional 2013" Pakume.
![Dialoogiboksi loomine](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Kui olete lõpetanud vormi saidikogumi loomisprotsessi peab algama. See võib kuluda kuni tund või nii.

![Ootel](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Kui protsess on lõpule jõudnud, näeb umbes selline. Kui me klõpsake **avaldamise** näeme, et enamik Office'i rakendused on avaldatud meile juba.
![Saidikogumi loomine](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Avaldatud rakendused](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Selles etapis saate lisada kasutajaid, millele juurdepääsu selle saidikogumi **Kasutajate juurdepääsu**klõpsamisega.
![Kasutajate juurdepääsu konfigureerimine](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Nüüd Proovime nüüd välja ühenduse Office 365!

## <a name="connect-to-office-365"></a>Office 365 ühendamine

Me pea üle [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), liikuge kerides allapoole ja nuppu **allalaadimine kliendid** töötate seadmesse installida Azure RemoteApp kliendi. Allpool kuvatõmmised on Windowsi jaoks.

Kui käivitub küsitakse logige sisse oma Microsofti kontoga (varem "Live ID"), kasutage sama nimega Azure'i konto kohe. Kui olete sisse logitud sisse tuleks näha uue kutsete teatisi, klõpsake seal ja peaksite nägema umbes üks loendi allpool. Mis vastab Azure'i konto omaniku e-posti kutse vastu võtta.

![Uue kutse](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Kuidas see välja näeb, kui seal on uus kutsed.

![Aktsepteerige rakenduse](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Kui te näete Office'i rakenduste Azure RemoteApp kliendi kutse vastu võtta.

![Rakenduste loend](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Kui klõpsate mõnda neist rakendus peab algama Azure virtuaalse masina ja teil peaks olema valmis! Nautige!

![käivitamine](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![PowerPointi](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
