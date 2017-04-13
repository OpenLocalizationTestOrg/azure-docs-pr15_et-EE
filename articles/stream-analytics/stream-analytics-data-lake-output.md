<properties
    pageTitle="Voo Analytics andmesalve väljundi | Microsoft Azure'i"
    description="Autentimise ja luba Azure'i andmed Lake poe voo Analytics töö konfigureerimine"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Voo Analytics andmesalve väljund

Voo Analytics töö toetavad väljundi mitu võimalust, üks on [Azure andmesalve Lake](https://azure.microsoft.com/services/data-lake-store/). Azure'i andmesalve Lake on suur andmete analüütilisel töökoormus on kogu ettevõtte hyper skaala hoidla. Lake andmesalve võimaldab teil mis tahes suurus, tüüp ja manustamisest kiirust funktsionaalseid ja uurimusliku analytics andmete talletamiseks.

## <a name="authorize-a-data-lake-store-account"></a>Lubada Lake andmesalve konto

1.  Lake andmesalve valimisel väljundina, mis on Azure haldusportaali teil palutakse lubada oma olemasoleva Lake andmesalve kasutamist või andmete Lake poe eelvaate Azure'i klassikaline portaali kaudu juurdepääsu taotlemiseks.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Kui teil juba on juurdepääs andmesalve Lake, klõpsake "Lubada nüüd" ja lühikest aega lehe avaneb "Loa ümber suunata...", mis näitab. Lehe suletakse automaatselt ja ilmub leht, mis võimaldab teil konfigureerida Lake andmesalve väljundi abil.

Kui teil on pole registreerunud andmete Lake Store eelvaade, taotluse algatada "Registreeruge kohe" lingi järgimiseks või järgige [Alustamine juhiste saamiseks](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Lake andmesalve väljundi atribuutide konfigureerimine

Kui teil on autenditud Lake andmesalve konto, saate konfigureerida oma Lake andmesalve väljundi atribuudid. Järgmises tabelis on atribuutide nimesid ja nende kirjeldus oma Lake andmesalve väljundi konfigureerimiseks loendit.

<table>
<tbody>
<tr>
<td><B>ATRIBUUDI NIMI</B></td>
<td><B>KIRJELDUS</B></td>
</tr>
<tr>
<td>Väljundi alias (pseudonüüm)</td>
<td>See on kasutatud päringuid suunamiseks selle andmete Lake Store päringuväljund sõbralik nimi.</td>
</tr>
<tr>
<td>Lake andmesalve konto</td>
<td>Kui saadate oma väljundi salvestusruumi konto nimi. Saate esitatakse ripploendi Lake andmesalve kontod, millele portaali sisse logitud kasutajale on juurdepääs.</td>
</tr>
<tr>
<td>Tee eesliite muster [<I>valikulised</I>]</td>
<td>Faili tee kirjutada oma faile määratud Lake Poe kontoga kasutada. <BR>{date} {aeg}<BR>Näide 1: folder1/logid / {date} / {time}<BR>Näide 2: folder1/logid / {date}</td>
</tr>
<tr>
<td>Kuupäevavormingu [<I>valikulised</I>]</td>
<td>Kui eesliide Path kasutatakse kuupäeva luba, saate valida, kus on korraldatud failide kuupäevavormingu. Näide: PP/AAAA/MM</td>
</tr>
<tr>
<td>Kellaajavorming [<I>valikulised</I>]</td>
<td>Kui aega luba kasutatakse eesliite tee, määrata failide korraldust kellaajavormingu. Praegu on ainus toetatud väärtus HH.</td>
</tr>
<tr>
<td>Sündmuse serialiseerimisvormingus</td>
<td>Andmete serialiseerimisvormingus. JSON-, CSV- ja Avro on toetatud.</td>
</tr>
<tr>
<td>Kodeerimine</td>
<td>Kui CSV- või JSON vormindamiseks on kodeerimine peab olema määratud. UTF-8 on ainus toetatud kodeerimise vorming sel ajal.</td>
</tr>
<tr>
<td>Eraldaja</td>
<td>Rakendatav vaid CSV sariväljaanne. Voo Analytics toetab levinud eraldajaid mitmesuguseid jaotamine CSV-faili andmed. Toetatud väärtused on koma, semikoolon, ruumi, menüüd ja vertikaaljoon.</td>
</tr>
<tr>
<td>Vormindamine</td>
<td>Rakendatav vaid JSON sariväljaanne. Eraldatud kriipsjoon väljund vormindatud lastes iga JSON objekti, eraldades need uue rea. Massiivi määrab vormindatakse väljund massiivi JSON objektid.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Pikendamise Lake andmesalve autoriseerimine

Praegu on piirang kus autentimise luba peab olema käsitsi värskendatud kõigi nende projektide Lake andmesalve väljundi 90 päeva järel. Samuti peate uuesti autentida Lake andmesalve konto, kui olete parooli muutnud, kuna teie töö loodud või viimase autenditud. Selle probleemi sümptom on töö väljund ja on vaja uuesti luba, mis näitab, Logi tõrget.

Selle probleemi lahendamiseks töötava tööpäevad lõpetada ja minge oma Lake andmesalve väljundi. Klõpsake linki "Pikendamine autoriseerimine" ja lühikest aega lehe avaneb "Loa ümber suunata...", mis näitab. Lehe suletakse automaatselt ja kui õnnestus, näitab "Autoriseerimine on edukalt uuendatud". Saate seejärel "Salvesta" lehe allosas nuppu tuleb ja taaskäivitada oma töö lõpetanud viimast andmekao vältimiseks saate jätkata.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
