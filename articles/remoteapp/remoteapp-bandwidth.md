
<properties 
    pageTitle="Prognoosimiseks Azure RemoteApp võrgu läbilaskevõime kasutuse | Microsoft Azure'i"
    description="Lugege oma Azure RemoteApp saidikogumite ja rakendused võrgu läbilaskevõime nõuete kohta."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Azure RemoteApp võrgu läbilaskevõime kasutuse prognoosimiseks 

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp kasutab Kaugtöölaua protokolli (RDP) suhelda Helistamine ja Azure pilveteenuse ja kasutajate vahel. Sellest artiklist leiate juhised, saate selle võrgu kasutamine prognoosimiseks ja hindate potentsiaalselt võrgu läbilaskevõime kasutuse Azure RemoteApp kasutaja kohta.

Kasutaja kohta läbilaskevõime kasutuse prognoosimine on väga keerukas ja nõuab mitme rakenduse korraga töötava multitegumtöötlus stsenaariumide kus rakendused võivad mõjutada üksteise tegevust vastavalt oma võrgu läbilaskevõime nõudmisel. Isegi tüüp (nt Mac klient ja HTML5 klient) kaugtöölaua klient võib kaasa tuua eri läbilaskevõime tulemid. Selleks et saaksite need tüsistused läbige, me pausi kasutamise stsenaariumid mitmeks kopeerida tegelike stsenaariumid levinud kategooriaid. (Kui tegelike stsenaarium on mõistagi segu kategooriaid ja kasutajale erineb.)

Enne kui me minna edasi - peab Pange tähele, et saaksime endale RDP annab hea suurepäraseid versiooni jaoks kõige kasutamise stsenaariumid võrkudes latentsus all 120 ms ja läbilaskevõime üle 5 MB – See põhineb RDP on võimalus dünaamiliselt muuta võrgu läbilaskevõime ja läbilaskevõime hinnanguline rakenduse abil. See artikkel läheb kaugemale "Enamik kasutamise stsenaariumid" vaadata selle serva, kus stsenaariumid alustada end ja kasutuskogemuse hakkab halvendada.

Nüüd vaadake järgmisi artikleid üksikasjad, sh mõjutavad tegurid, alusjoone soovitusi ja mida me ei sisalda meie hinnangute jaoks.

- [Kuidas teha võrgu läbilaskevõime ja -kvaliteeti ning kogemusi töötavad koos?](remoteapp-bandwidthexperience.md)
- [Oma võrgu läbilaskevõime kasutuse mõne levinud stsenaariumi testimine](remoteapp-bandwidthtests.md)
- [Kiirülevaate juhiseid, kui teil pole aega ega võimalus testimine](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Mis on me koos?

Läbi vaadates pakutud testide ja meie üldiselt (ja küll üldise) soovitused arvestage, et on mitmeid tegureid, mida me ei pidanud. Näiteks kasutaja kogemus tüsistused esitatud asümmeetriline Laadi üles ja laadige läbilaskevõime. Enamik WiFi-võrkudes asümmeetriline laad mõjutab Lisaks jõudlus ja kasutajale kasutuskogemuse taju. Interaktiivne stsenaariumid võib järgmise etapi liikluse prioriteetne madalam eelnev, mis võivad kaotsi video või heli paneelid arvu suurendada ja seega mõjutada kasutaja seisukohalt voogesituse kogemus. Käivitage oma katsete näha, milline on hea oma konkreetsete juhul ja võrgu kasutamine.

Kuigi me arutada seadme ümbersuunamine, me ei arvestata läbilaskevõime mõju põhjustatud manustatud seadmetest salvestusruumi, printerite, skanneri, Vali veebikaamera ja muud USB-seadmed võrguliiklust. Nende seadmete mõju tavaliselt naelu läbilaskevõime vajadustele ajutiselt ja kaob selle tööülesande lõpetamisel olekuaruanne. Kuid kui sageli lõpule jõudnud, mis nõuavad läbilaskevõime võib olla väga märgatav.

Me ka arutada, kuidas ühe kasutaja võib mõjutada teiste kasutajate sama võrgustikus. Näiteks ühe kasutaja tarbimine 4K video 100 MB/s võrgus võib oluliselt mõjutada teised kasutajad üritavad sama ülesanne selle samas võrgus. Kahjuks saab järk-järgult raskem samaaegne kasutamine levinud või kõike soovituse kohta, kuidas süsteem täidab aggregate anda mõju määramiseks. Me ei saa öelda on protokolli tehnoloogial teeb parimal viisil kasutada võrgu läbilaskevõime, et see on selle piirangud.