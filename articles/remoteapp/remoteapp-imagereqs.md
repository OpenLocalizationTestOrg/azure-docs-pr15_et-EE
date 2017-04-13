
<properties
    pageTitle="Azure RemoteApp pilt nõuded | Microsoft Azure'i"
    description="Siit saate teada, koos Azure RemoteApp piltide loomise nõuete kohta"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Nõuded Azure RemoteApp pildid

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp kasutab Windows Server 2012 R2 pilt majutada kõik programmid, mida soovite oma kasutajatega ühiselt kasutada. Luua kohandatud pildi, saate alustada olemasoleva pildi või [looge uus](remoteapp-create-custom-image.md).

> [AZURE.TIP] Kas teadsite, et Azure RemoteApp tellimuse kaudu pääsete juurde Windows Server 2012 R2 pilt Azure VM Galerii, mille abil saate luua oma malli pilti? [Märkige ruut selle välja](remoteapp-image-on-azurevm.md).  


Pilt, mida saab kasutada Azure RemoteAppi üles laadida nõuded on:


- Kohandatud rakendused ei salvestada andmete pilt. Neid pilte on ja peaks sisaldama ainult rakendusi.
- Pilti ei sisalda andmeid, mis võivad olla kaotsi.
- Pildi suurus peaks olema kordne MB. Kui proovite üles pilt, mis ei ole, nurjub üles.
- Pildi suurus peab olema 127 GB või väiksem.
- See peab olema VHD faili (VHDX faile ei toeta praegu).
- Funktsiooni VHD ei tohi genereerimine 2 virtuaalse masina.
- Funktsiooni VHD võib olla fikseeritud suurus või dünaamiliselt laiendada. Dünaamiliselt laiendada VHD on soovitatav, kuna kulub vähem aega Azure kui fikseeritud suurus VHD faili üles laadida.
- Ketas tuleb lähtestada abil soovitud juhtslaidi buutimine Record (MBR) eraldamine laad. GUID partition (GPT) partition tabelilaadi ei toetata.
- Funktsiooni VHD peab sisaldama Windows Server 2012 R2 üks install. See võib olla mitu draivi, kuid ainult ühe, mis sisaldab Windowsi installi.
- Kaugtöölaua seansi hosti (RDSH) roll ja Töölauakogemuse funktsioon peab olema installitud.
- Roll Remote'i töölaua ühenduse ta peab *ei* saanud installida.
- Krüptitud (EFS) peab olema keelatud.
- Pilt peab olema SYSPREPed abil parameetrid **/OOBE / generalize /shutdown** (Ärge kasutage parameetrit **/mode:vm** ).
- Üleslaadimise oma VHD hetktõmmise ahelas kaudu ei toetata.

Azure RemoteApp piltide loomise kohta lisateabe saamiseks vaadake [Azure RemoteApp pildi loomine](remoteapp-imageoptions.md) .
