<properties 
   pageTitle="Värskenduste installimine StorSimple virtuaalse massiiv | Microsoft Azure'i"
   description="Kirjeldab portaali ja kiirparandus meetodil värskenduste rakendamine StorSimple virtuaalse massiiv web UI abil"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Teie StorSimple virtuaalse massiiv värskenduste installimine

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse värskenduste installimine oma StorSimple virtuaalse massiiv ja Azure klassikaline portaali kaudu kohaliku web UI etapid. Peate tarkvara värskenduse või oma StorSimple virtuaalse massiiv ajakohasena hoidmiseks rakendada. 

Pidage meeles, mis installimisel värskenduse või kiirparandus seadme taaskäivitamist. Võttes arvesse, et StorSimple virtuaalse massiiv on ühe sõlm seadet, mis tahes I/O poolelioleva katkestati ja seadme kogemusi tööseisakute. 

Enne mõne värskenduse soovitame võtta ühtlasi ühenduseta ja mahud hostis esimese ja seejärel soovitud seade. See vähendab võimalust andmete rikkumist.

> [AZURE.IMPORTANT] Kui teil on värskendus 0,1 või GA tarkvara versioonid, peate kasutama 0,3 värskenduse installimiseks kiirparandus meetodit kohaliku veebi Kasutajaliidese kaudu. Kui teil on värskendus 0,2, soovitame Azure klassikaline portaali kaudu installida.

## <a name="use-the-local-web-ui"></a>Kohalik veebi kasutajaliides 
 
Kohalik web Kasutajaliidese kasutamisel on kaks toimingut.

- Laadige värskendus või kiirparandus
- Installige värskendus või kiirparandus

### <a name="download-the-update-or-the-hotfix"></a>Laadige värskendus või kiirparandus

Järgmiste toimingute tarkvara värskendus alla Microsofti värskenduste kataloogist.

#### <a name="to-download-the-update-or-the-hotfix"></a>Värskenduse või kiirparandus alla laadida

1. Käivitage Internet Explorer ja liikuge [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Kui kasutate Microsoft Update'i kataloogi selles arvutis esimest korda, klõpsake nuppu **installida** Microsoft Update'i kataloogi lisandmooduli installimiseks küsimise.
  
3. Sisestage otsinguväljale Microsoft Update'i kataloogi teabebaasi (Knowledge Base) arvu kiirparandus, mida soovite alla laadida. Sisestage **3182061** Update 0,3 ja seejärel klõpsake nuppu **Otsi**.

    Näiteks kuvatakse loend kiirparandus **StorSimple Virtual massiiv Update 0,3**.

    ![Otsing kataloog](./media/storsimple-ova-install-update-01/download1.png)

4. Klõpsake nuppu **Lisa**. Värskenduse lisatakse ostukorvi.

5. Klõpsake nuppu **Kuva**.

6. Klõpsake nuppu **Laadi alla**. Määrake või **sirvige** kohaliku asukohta, kus soovite kuvada allalaadimise. Värskendused on alla laaditud määratud asukohta ja sama nimega värskenduse alamkaust. Kausta saab kopeerida võrgu osa, mis on kättesaadav seadmest.

7. Avage kaust, kopeeritud, kuvatakse faili Microsoft Update'i eraldiseisev pakett `WindowsTH-KB3011067-x64`. Installige värskendus või kiirparandus kasutatakse seda faili.


### <a name="install-the-update-or-the-hotfix"></a>Installige värskendus või kiirparandus

Enne värskenduse või kiirparandus installimise, veenduge, et teil on värskenduse või kiirparandus alla laaditud, kas kohalik hostis või võrgukettal kaudu. 

Kasutage seda meetodit värskenduste installimine GA töötavad seadmesse või värskendada 0.1 Tarkvara versioonid. See toiming võtab vähem kui 2 minutit. Järgmiste toimingute abil installige värskendus või kiirparandus.


#### <a name="to-install-the-update-or-the-hotfix"></a>Värskenduse või paik installida

1. Kohaliku web UI, minge **hooldustööde** > **Tarkvara värskendus**.

    ![seadme värskendamine](./media/storsimple-ova-install-update-01/update1m.png)

2. **Värskendage faili tee**, sisestage faili nimi värskenduse või kiirparandus. Värskenduse või kiirparandus installifail saate sirvida ka kui võrgukettal. Klõpsake nuppu **Rakenda**.

    ![seadme värskendamine](./media/storsimple-ova-install-update-01/update2m.png)

3.  Kuvatakse hoiatus. Kuna see on üks sõlm seadmest, kui värskendus on rakendatud seadme taaskäivitamist ja tööseisakute. Klõpsake ikooni Otsi.

    ![seadme värskendamine](./media/storsimple-ova-install-update-01/update3m.png)

4. Värskendus käivitub. Pärast seadet värskendatud selle taaskäivitamist. Kohaliku UI pole kättesaadav selle kestus.

    ![seadme värskendamine](./media/storsimple-ova-install-update-01/update5m.png)

5. Kui taaskäivitamine on lõpule jõudnud, on võetud **logige sisse** lehele. Veenduge, et seadme tarkvara on värskendatud kohaliku Web UI, minge **hooldustööd** > **Tarkvara värskendus**. Kuvatud tarkvara versiooni peaks olema **10.0.0.0.0.10288.0** värskenduse 0,3.

    > [AZURE.NOTE] Me aruande veidi erinevalt kohalik Web Kasutajaliidese ja Azure klassikaline portaali tarkvara versioonid. Näiteks kohaliku web UI aruannete **10.0.0.0.0.10288** ja Azure klassikaline portaali aruannete **10.0.10288.0** sama versioon. 

    ![seadme värskendamine](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Azure'i klassikaline portaali kasutamine

Kui värskendus 0,2, Soovitame installida Azure klassikaline portaali kaudu värskendused. Portaali protseduur nõuab kasutaja skannida, laadige alla ja installige värskendused. See toiming võtab aega umbes 7 minutit lõpuleviimiseks. Järgmiste toimingute abil installige värskendus või kiirparandus.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Kui installimine on lõpule jõudnud (nagu on näidatud 100% töö oleku järgi), avage **seadmed > hooldus > tarkvaravärskendused**. Kuvatud tarkvara versiooni peaks olema 10.0.10288.0.

![seadme värskendamine](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Järgmised sammud

Lisateave [teie StorSimple virtuaalse massiiv haldamise](storsimple-ova-web-ui-admin.md)kohta.
