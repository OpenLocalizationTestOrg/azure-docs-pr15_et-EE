<properties
    pageTitle="Luua Azure RemoteApp pilt | Microsoft Azure'i"
    description="Lisateavet piltide jaoks Azure RemoteApp loomise võimalusi"
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



# <a name="create-an-azure-remoteapp-image"></a>Luua Azure RemoteApp pilt

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp kasutab piltide ootelepanekuks rakendused oma kasutajatega ühiskasutusse. (Me võtta oma pilt ja selle abil luua VMs – see on kasutajate juurdepääs, kui nad logige Azure RemoteApp.) Loomiseks on Azure RemoteApp saidikogumi oma valiku rakendusi, kas see on pilv või hübriid, alustate loomise pilt nende installitud rakendustega. Looge saidikogumi, mis kasutab selle pildi, on saidikogumi kasutajate määramine ja nende kasutajate rakenduste avaldamine.

Teil on mitu võimalust luua või piltide abil. Põhiline [nõue](remoteapp-imagereqs.md) pilt on see käivitamine Windows Server 2012 R2 installitud kaugtöölaua seansi hosti (RDSH) roll. Kuidas saada mis on, kus asjad huvitav.

Kui tegemist on pildid on teil järgmised võimalust.

- Saate importida ja kasutada [pildi vastavalt Azure virtuaalne arvutis](remoteapp-image-on-azurevm.md)on. See on hea ärivaldkonna rakendused, mis vajavad kohandatud sätteid. Saate kohandada rakenduse töötamiseks pilt.
- Saate [luua ja üles laadida kohandatud pildi](remoteapp-create-custom-image.md). See on hea, kui teil on juba pildiks, mida kasutate kohapealse Kaugtöölauateenuseid juurutamiseks.
- Saate valida ühe RemoteApp teie tellimus sisaldab [malli pilti](remoteapp-images.md) . Need pildid on loodud ja säilitada RemoteApp meeskonna ja sisaldavad teatud standardse rakenduste (nt Office'i tarkvarakomplekt), mida saate teha kättesaadavaks kasutajatele. Pange tähele, et tootmise säte saab kasutada ainult Office 365 Pro Plusi pilt.

Sõltumata sellest, kus saate oma pildi või kuidas te selle loote, peaksite veenduge, et tagada, et teie rakendus töötab ka RemoteApp [rakenduse nõuded](remoteapp-appreqs.md) mõista. Seejärel järgmise sammuna tuleb [pilveteenuses](remoteapp-create-cloud-deployment.md) või [hübriid](remoteapp-create-hybrid-deployment.md) saidikogumi loomine.
