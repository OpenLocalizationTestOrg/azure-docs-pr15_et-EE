<properties
    pageTitle="Teatis jaoturi Turve"
    description="See teema selgitab Azure teatis jaoturi Turve."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Turvalisus

##<a name="overview"></a>Ülevaade

Selles teemas kirjeldatakse Azure'i teatis jaoturi mudelit. Kuna teatis jaoturi on teenuse siini üksus, rakendatakse turvalisus mudelit ning teenuse siini. Lisateavet leiate teemadest [Siini autentimise](https://msdn.microsoft.com/library/azure/dn155925.aspx) .

##<a name="shared-access-signature-security-sas"></a>Ühiskasutusega juurdepääsu allkirja Turve (SAS) 

Teatis jaoturi rakendab mõne üksuse tasandi kava nimetatakse SAS (ühiskasutusse Accessi signatuur). Selle kava lubab sõnumside üksuste kuni 12 autoriseerimine reeglid nende kirjeldus, mis annavad olemi deklareerida.

Iga reegli puhul sisaldab nimi, võtmeväärtuse (ühiskasutusega salajane) ja õigused, nagu on selgitatud jaotises "Turvalisus nõuded." Teate jaoturi loomisel kaks reeglid luuakse automaatselt: ühes kuulata õigused (mis kasutabfunktsiooni klientrakenduses rakendus) ja kõik õigused (mis kasutab rakenduse taustväärtus).

Täites registreerimise juhtimine kliendi rakendustest, kui teabe saata teatised ei ole tundliku sisuga (nt ilm värskendused), teate jaoturi levinud viivat on võtmeväärtuse reegli kuulata ainult juurdepääsu anda kliendi rakendus ja anda reegli täielik juurdepääs võtmeväärtuse rakenduse taustväärtus.

See ei ole soovitatav manustate võtmeväärtuse Windowsi poe kliendi rakendustes. Nii, et vältida manustamine klahv väärtus on kliendi rakendus toomiseks rakenduse taustväärtus käivitamisel.

On oluline, et aru saada, et võti kuulata juurdepääsu võimaldab kliendi rakendus, mis tahes silti registreeruda. Kui teie rakendus peab piirata registreerimise kindla siltide teatud klientidele (näiteks sildid tähistada kasutaja ID-d), siis teie rakendus kirjutamata tegema registreerimise. Lisateavet leiate teemast registreerimise juhtimine. Pange tähele, et sel viisil kliendi rakendus pole otsest juurdepääsu teatise jaoturi.

##<a name="security-claims"></a>Turvalisuse nõuded

Sarnaselt muude üksuste, teate jaoturi toimingud on lubatud kolm turvalisus nõuete: kuulata, saata ja hallata.

| Nõue | Kirjeldus | Lubatud |
|-------|-------------|--------------------|
| Kuulake | Luua või uuendada, lugeda ja kustutada ühe registreerimine | Registreerimise loomine või värskendamine<br><br>Lugege registreerimine<br><br>Lugege registreerumine jaoks pide<br><br>Registreerimise kustutamine |
| Saatmine | Sõnumeid saata teate jaoturi | Sõnumi saatmine |
| Haldamine | CRUDs teatis jaoturi (sh värskendamine PNS mandaadid ja turbe klahvid) ja siltide põhjal lugege registreerimine | Teatise loomine ja värskendamine/lugemine/kustutamine jaoturi<br><br>Lugege registreerimise sildi järgi |


Teatis jaoturi vastu antud Microsoft Azure'i juurdepääsu reguleerimine sõned ja signatuuri märgid, mis on loodud konfigureeritud otse teate jaoturi jagatud võtmed.