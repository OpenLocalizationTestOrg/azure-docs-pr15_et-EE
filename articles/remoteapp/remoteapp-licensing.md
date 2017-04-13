<properties
    pageTitle="Azure RemoteApp litsentsimise | Microsoft Azure'i"
    description="Siit saate teada, kuidas töötab litsentsimine Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Hulgilitsentsimise tööpõhimõtted Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Nii, et olete häälestanud oma Azure RemoteApp teenus, mis on loodud mallide, ja on avaldada rakenduste kasutajatele. Kuid on veel üks asi aru saada: litsentsimine. Kuidas lihtsalt hulgilitsentsimise töö RemoteApp ja rakendused RemoteAppi kaudu ühiskasutusse?

RemoteApp nõua Windows litsentsid või Remote'i töölaua Cal. Tellimuse hoolitseb RemoteApp side enda. (Vaadake üksikasju [paketid](https://azure.microsoft.com/pricing/details/remoteapp)).

Kui kasutate pilte, mis sisaldab teie tellimus, saate jagada ühte eraldi litsentsi automaatselt ilma, et pilt installitud rakendused. Näiteks kui kasutate Windows Server 2012 R2 malli pilti jälgitavaid, saate jagada System Center Endpoint Protection kasutajatega. Ainult erandid Sellel reeglil on Office 365 ProPlus, mis nõuab eraldi tellimus, ja Office 2013, mis ei saa ühiskasutusse anda tootmise saidikogumi.

Kui soovite kasutada Office 365 malli pilt, mis on kaasas RemoteApp, peate *olemasoleva* Office 365 ProPlusi plaani. Sama kehtib kõigi Office 365 rakenduste avaldatavatesse kohandatud malli abil. Peate aktiveerima rakendused oma tellimus. See kehtib nii prooviversiooni ja makstud tellimused. Kui soovite kasutada Office 365 malli pilti prooviperioodil *ja te pole veel tellimust*, minge lehele Office 365 prooviversiooni tellimuse jaoks [registreeruda](https://go.microsoft.com/fwlink/p/?LinkID=403802) . Vt [Kuidas RemoteApp ja Office koos toimivad?](remoteapp-o365.md) lisateabe saamiseks.

Kui prooviperioodil, te ei soovi, et Office 365 prooviversiooni tellimuse, võite kasutada Office 2013 Professional Plusi malli pilti, mis sisaldab RemoteApp. Selle malli pilti saab kasutada ainult 30 päeva ja ei saa ümber tasuliseks saidikogumi.

Muude rakenduste peate veenduge, et teil on litsents rakenduse ühiskasutus.

See on mõistlik õige? Saate avaldada mis tahes rakendus, mida teil on ühiskasutusse anda. Ja veenduge, et teil tõesti on õigus oma programmide ühiskasutus peate.

Pange tähele, et te ei saa kasutada CAL või hulgilitsents lepingu cloud saidikogumi. *Saate* kasutada hulgilitsents lepingu oma hübriid saidikogumi (v.a Office) rakenduste aktiveerimiseks. Peate lihtsalt oma malli pilti hulgilitsents meedia installimine. Järgige teavet rakenduse installimiseks litsentside kaugtöölaua keskkonnas tarnija poole.
