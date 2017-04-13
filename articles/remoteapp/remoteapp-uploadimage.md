
<properties
    pageTitle="Kohandatud pildi üleslaadimine Azure RemoteApp jaoks | Microsoft Azure'i"
    description="Saate teada, kuidas Azure RemoteApp jaoks kohandatud pildi üleslaadimine"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Azure RemoteApp jaoks kohandatud pildi üleslaadimine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Nüüd, kui olete loonud kohandatud malli pilti või värskendatud muudatustega, peate selle pildi üleslaadimine Azure RemoteApp pildi teeki. Järgige neid juhiseid.


## <a name="before-you-start"></a>Enne alustamist

1.      Kontrollige oma kohandatud pildi vastab soovitud [pilt](remoteapp-imagereqs.md) ja [rakenduse nõudeid](remoteapp-appreqs.md).
2.      Installige [Azure'i PowerShelli mooduli](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Samm-sammult kohta, kuidas kohandatud pildi üleslaadimine

1.      Avage Azure'i haldusportaal ja liikuge lehele, RemoteApp.
2.      Klõpsake menüü **malli pildid** **üles laadida** lehe allosas.
4.      Sisestage sõbralik nimi oma pilt ja määrake konto talletuskoht. Veenduge, et asukoht on teie RemoteApp saidikogumi samasse asukohta või asukoht, kuhu soovite luua.
5.      Vastava viiba kuvamisel skripti kohalikku Arvutisse alla.
6.      Tekstiväljale oma lõikelauale kopeerida käsu parameetrid.
7.      Avage administraatoriõigustes Windows PowerShelli aken.
8.      Liikuge samas kaustas, kuhu laadisite skripti administraatoriõigustes Windows PowerShelli aknas.
9.      Kleepige kopeeritud käsk ja vajutage sisestusklahvi **Enter**.

    Alustab üleslaadimine ja kestus sõltub palju tegureid, sh teie võrgu kiirusest ja pildi suurus

11.    Kui teie üleslaadimine ei õnnestu võrk pidevalt või asju tõttu sellist, saate jätkata alati saate algas üleslaadimine. Elulookirjelduse üleslaadimine, käivitage uuesti, kasutades sama käsurea skripti.

> [AZURE.WARNING] Kunagi muuta skripti üles. Selleks et pilt vastaks pilt nõuded ja rakenduse nõuded on rakendatud teatud kontrollid.

## <a name="common-problems"></a>Levinud probleemid

- Veenduge, et Windows PowerShelli, mitte Azure PowerShelli kasutamine. Peate installima Azure PowerShelli moodul, kuna teatud moodulid on vaja jooksul üleslaadimine.
- Mitte kunagi muuta skripti, valideerimised on mugav.
- Kui vhd faili saab lukustades ajal üles, kopeerige fail või teisaldada selle uus asukoht ja proovi upload uuesti. Võib olla mõne Windowsi protsessi, mis takistab üles.  
