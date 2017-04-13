<properties
   pageTitle="Juurdepääs Azure'i Virtuaalmasinates Server Explorer | Microsoft Azure'i"
   description="Saada ülevaade sellest, kuidas vaadata, luua ja hallata Azure'i virtuaalmasinates (VM) Server Explorer Visual Studios."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Juurdepääs Explorerist Server Azure'i Virtuaalmasinates

Visual Studio serveri Exploreri abil saate kuvada oma korraldaja Azure'i virtuaalmasinates teabe.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Juurdepääs virtuaalmasinates Server Explorer

Kui teil on majutatud Azure'i virtuaalmasinates, pääsete neile Server Explorer. Peate esmalt sisselogimiseks Azure tellimuse mobiilsideseadmete teenuste kuvamiseks. Logige sisse, Azure sõlme kiirmenüü avamine Server Explorer ja valige **ühenduse loomine Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Kui soovite oma virtuaalmasinates kohta teabe saamine

1. Server Explorer, valida virtuaalse masina ja valige oma aken atribuudid kuvamiseks klahv F4.

    Järgmine tabel näitab, millised atribuudid on saadaval, kuid need on kõik kirjutuskaitstud. Saate muuta nende [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Atribuut|Kirjeldus|
  	|---|---|
  	|DNS-i nimi.|URL-i abil virtuaalse masina Interneti-aadressi.|
  	|Keskkonnas|Virtuaalse masina, selle atribuudi väärtus on alati tootmise.|
  	|Nimi|Virtuaalse masina nimi.|
  	|Suurus|Suurus virtuaalse masina, mis kajastab mälu ja vaba ruumi, mis on saadaval. Lisateabe saamiseks lugege teemat Kuidas: konfigureerimine virtuaalse masina suurused.|
  	|Olek|Väärtused sisaldavad alustades, käivitatud, peatamine, peatamiseni ja toomine olek. Kui toomine olek kuvatakse, praegune olek pole teada. Selle atribuudi väärtuste erinevad väärtused, mida kasutatakse [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).|
  	|SubscriptionID|Tellimuse ID Azure'i kontosse. [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885) saate seda teavet kuvada, kui tellimuse atribuutide vaatamine.|

1. Valige soovitud lõpp-punkti sõlm ja seejärel vaadata aken **Atribuudid** .

1. Järgmises tabelis kirjeldatakse saadaval atribuutide lõpp-punktid, kuid need on kirjutuskaitstud. Lõpp-punktide virtuaalse masina lisamiseks või muutmiseks kasutada [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|Atribuut|Kirjeldus|
  	|---|---|
  	|Nimi|Identifikaatori lõpp-punkti.|
  	|Privaatne Port|Võrgupääs ettevõttesisese rakenduse port.|
  	|Protocol (protokoll)|Protokoll, mida kasutab transpordikihi selle lõpp-punkti, kas TCP või UDP.|
  	|Avalik Port|Port, mida kasutatakse rakenduse juurdepääsu.|

## <a name="next-steps"></a>Järgmised sammud

Visual Studio Azure rollid kasutamise kohta lisateabe saamiseks vt [Kaugtöölaua abil Azure'i rollid](vs-azure-tools-remote-desktop-roles.md).
