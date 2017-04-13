<properties 
    pageTitle="Tööülesannete eri riikide või olekud BizTalki teenuste lubatud | Microsoft Azure'i" 
    description="Toimingud/toimingute lubatud erinevate MABS olek: peatamine, käivitamine, taaskäivitage, peatada, jätkata, kustutamine, mastaapimiseks, konfigureerimine ja varundamise häälestamine" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>



# <a name="biztalk-services-service-state-chart"></a>BizTalki Services: Teenuse olek diagramm
Sõltuvalt teenuse BizTalki hetkeseisu, on toimingud, mida saate või ei saa teha BizTalki teenuse.

Näiteks saate säte uue BizTalki teenuse Azure klassikaline portaalis. Kui see on lõpule jõudnud edukalt, BizTalki teenus on aktiivne olekus. Aktiivne olekus, saate BizTalki teenuse peatada. Kui Peata õnnestus, läheb teenuse BizTalki peatatud olekus. Kui Peata nurjub, läheb BizTalki teenuse StopFailed olekus. StopFailed olekus, saate teenuse BizTalki taaskäivitada. Kui proovite toiming, mis pole lubatud, nt elulookirjelduse teenuse BizTalki järgmine tõrge:

**Toiming pole lubatud**

BizTalki teenuse ettevalmistamise, minge [BizTalki teenuste: ettevalmistamise abil Azure klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Järgmises tabelis on loetletud toimingud või toiminguid, mida saate täita juhul, kui BizTalki teenus on teatud olekus. Mõne ✔ tähendab, et toiming on lubatud ajal liikmesriigis. Tühi kirje tähendab, et toimingut ei saa teha samal ajal liikmesriigis.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Käivitage lõpetada, uuesti, peatada, elulookirjeldus, ja kustutada toimingud
<table border="1">
<tr>
        <th colspan="15">Toiming või toimingut</th>
</tr>

<tr>
        <th rowspan="18">BizTalki teenuse olek</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Alustamine</th>
        <th>Stopp</th>
        <th>Taaskäivitage</th>
        <th>Peatada</th>
        <th>Elulookirjeldus</th>
        <th>Kustutamine</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktiivne</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Keelatud</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Peatatud</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Peatatud</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Teenuse värskendamine nurjus.</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Skaala, värskendus konfigureerimine ja varukoopia toimingud
<table border="1">
<tr>
        <th colspan="15">Toiming või toimingut</th>
</tr>

<tr>
        <th rowspan="18">BizTalki teenuse olek</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Skaala</th>
        <th>Värskendage konfigureerimine</th>
        <th>Varundus</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktiivne</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Keelatud</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Peatatud</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Peatatud</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Teenuse värskendamine nurjus.</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Vt ka
- [BizTalki Services: Ettevalmistamise abil Azure'i klassikaline portaal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalki Services: Arendaja, Basic, Standard- ja Premiumi väljaannetes diagrammi](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalki Services: Varundus ja taaste](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalki teenuste: ahendamine](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalki Services: Väljaandja nimi ja väljaandja võti](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
