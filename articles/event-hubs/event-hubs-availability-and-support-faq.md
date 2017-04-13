<properties 
    pageTitle="Sündmuse jaoturi korduma kippuvad küsimused (KKK) | Microsoft Azure'i"
    description="Sündmuse jaoturi KKK."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Sündmuse jaoturi KKK

Sündmuse jaoturi pakub suuremahuliste tarbimine, püsimine ja suure läbilaskevõimega andmeallikast pärinevate andmete sündmuste töötlemine ja/või miljoneid seadmed. Kui seotud teenuse siini järjekorrad ja teemad, võimaldab sündmuse jaoturi püsivate juhtimis- ja juurutuste [Internet, asjade](https://azure.microsoft.com/services/iot-hub/) stsenaariumid.

Selles artiklis käsitletakse hinnateavet ja leiate vastused korduma kippuvatele küsimustele sündmuse jaoturi kohta:

## <a name="pricing-information"></a>Hinnateavet

Lisateavet sündmuse jaoturi hinnad, näeksid [sündmuse jaoturi hinnakirjad üksikasjad](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Kuidas arvutatakse sündmuse jaoturi sissepääsu sündmuste?

Iga sündmuse saadetud sündmuse jaoturiga loeb arveldatavate sõnumi. *Sissepääsu sündmus* on määratletud üksuse andmete, mis on väiksem või võrdne 64 KB. Mõni sündmus, mis on väiksem või võrdne suurusega 64 KB loetakse ühe arveldatavate sündmus. Kui sündmus on suurem kui 64 KB, arvutatakse tasustatav sündmuste arv sündmuse suuruse, hulgidiagrammides 64 KB. Näiteks saadetakse sündmuse jaoturi 8 KB sündmuse eest tuleb tasuda ühe sündmus, kuid 96 KB sõnumi sündmuse jaoturi on tasulised kui sündmustest.

Sündmuste tarbitud sündmuse jaoturis nimega ka toimingute ja kõned, nagu postkastid juhtida, ei loendata arveldatavate sissepääsu sündmused, kuid lisada kuni läbilaskevõime ühiku toetus.

## <a name="what-are-event-hubs-throughput-units"></a>Mis on sündmuse jaoturi läbilaskevõime üksuste?

Saate valida konkreetselt sündmuse jaoturi läbilaskevõime üksused, Azure portaali või sündmuse jaoturi ressursi halduri mallide abil. Läbilaskevõime üksuste rakendada kõigi sündmuste jaoturi mõni sündmus jaoturi nimeruumi ja läbilaskevõime iga üksuse annab nimeruumi järgmisi võimalusi.

- Kuni 1 MB sekundis sissepääsu sündmused (saadetud mõni sündmus jaoturiga sündmused), kuid pole enam kui 1000 sissepääsu sündmused, toimingute või kontrollida API kõned sekundis.

- Kuni 2 MB sekundis sealt sündmused (sündmused tarbitud mõni sündmus keskuse kaudu).

- Sündmuse salvestusruumi (vaikimisi 24-tunnise säilitusperiood jaoks piisavad) 84 GB.

Sündmus jaoturi läbilaskevõime üksused on arve tunni, põhineb antud tunni jooksul üksuste maksimumarv.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Kuidas jõustatakse sündmuse jaoturi läbilaskevõime ühiku piirangud?

Kui kokku sissepääsu jõudlus või kokku sissepääsu sündmuse määr üle kõik sündmuse jaoturi nimeruumi ületab liitväärtuse läbilaskevõime ühiku mahupiirangud, saatjad on rakendus ja vastu võtta, mis näitab, et sissepääsu kvoodi ületamise tõrkeid.

Kui kokku sealt jõudlus või kokku sündmuse sealt määr üle kõik sündmuse jaoturi nimeruumi ületab liitväärtuse läbilaskevõime ühiku mahupiirangud, vastuvõtjad on rakendus ja saada tõrketeateid, mis näitab, et sealt piirmäär on ületatud. Sissepääsu ja sealt kvootide jõustatud eraldi nii, et saatja, võivad põhjustada sündmus energiatarbimist aeglustada ega saate vastuvõtja takistada sündmuste saatmist jaoturiga soovitud sündmus.

Pöörake tähelepanu sellele läbilaskevõime ühiku valiku sõltumatu sündmuse jaoturi sektsioonide arvu. Kuigi iga sektsiooni pakub maksimaalse läbilaskevõime 1 MB teise sissepääsu (koos kuni 1000 sündmuste sekundis) kohta ja 2 MB kohta teise sealt, on fikseeritud tasuta sektsioonid ise. Tasu on liidetud läbilaskevõime ühikute kõik sündmuse jaoturi mõni sündmus jaoturi nimeruumi kohta. See muster abil saate luua piisavalt sektsioonid eeldatava koormus tugi oma ilma mis tahes läbilaskevõime tasu kuni sündmuse laadi süsteemi tegelikult nõuab suurema läbilaskevõime arvude ja ilma muuta struktuuri ja arhitektuur teie süsteemi koormus suureneb.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Kas läbilaskevõime üksused, mida saab valida arv on piiratud?

On 20 läbilaskevõime ühiku kohta nimeruumi vaikimisi kvoot. Saate taotleda suuremat piirmäära läbilaskevõime üksused, esitades tugi Piletite. Limiidile 20 läbilaskevõime ühiku kimbud on saadaval 20 – 100 läbilaskevõime üksused. Pange tähele, et abil rohkem kui 20 läbilaskevõime ühikut eemaldab võimalus ilma esitamise tugi Piletite läbilaskevõime üksuste arvu muutmine.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Kas säilitades sündmuse jaoturi sündmuste üle 24 tunni tasu?

Sündmuse jaoturi Standard taseme luba sõnumi säilituspoliitika perioodide rohkem kui 24 tundi, kuni 30 päeva. Kui salvestatud sündmuste arv maht ületab salvestusruumi toetus valitud läbilaskevõime ühikute (ühiku läbilaskevõime 84 GB), suurus, mis ületab toetus on lisandu avaldatud Azure'i bloobimälu salvestusruumi määra. Salvestusruumi toetus igas üksuses läbilaskevõime katab kõik salvestusruumi kulud säilitamise tähtaegu 24 tunni jaoks (vaikimisi) ka siis, kui kasutada läbilaskevõime ühiku maksimaalne sissepääsu toetus.

## <a name="what-is-the-maximum-retention-period"></a>Mis on maksimaalne säilitusperiood?

Sündmuse jaoturi Standard taseme toetab praegu kuni säilituspoliitika 7 päeva jooksul. Pange tähele, et sündmuse jaoturi ei ole mõeldud püsivalt andmesalve. Säilituspoliitika perioodide üle 24 tunni on mõeldud stsenaariumid, kus on mugav kordus voogu sündmuse sisse sama süsteemid; näiteks koolitada või uue arvuti õ mudeli aluseks olemasolevad andmed kinnitamine.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Kuidas sündmuse jaoturi mälumaht arvutatud ja maksta?

Salvestatud kõik sündmused, sh kõik sündmuse jaoturi, sisemine pea kohal, sündmuse päiste või ketta salvestusruumi struktuurid kogumaht mõõtmisel kogu päeva. Päeva lõpus arvutatakse tippväärtus mälumaht. Igapäevane salvestusruumi toetus arvutatakse väikseima arvu (iga läbilaskevõime üksus pakub kohandus 84 GB) päeva jooksul valitud läbilaskevõime üksused. Kui kogumaht ületab arvutatud igapäevane salvestusruumi toetus, on liigse salvestusruumi arve, kasutades Azure'i bloobimälu salvestusruumi (tempos **Kohalikult liigsete salvestusruumi** ).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Saate kasutada ühte AMQP ühendust saata ja vastu võtta sündmuse jaoturi ja teenuse siini järjekorrad/teemade?

Jah, kui kõik sündmuse jaoturi, järjekorrad ja teemade on sama nimeruumi. Saate rakendada kahesuunalise, vahendatud Ühenduvus mitmed seadmed, kus subsecond latentsused, tulusate ja väga paindlik viisil.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Tehke sündmuse jaoturi rakendamine vahendatud ühenduse kulude?

Saatjad jaoks ühenduse kõnetasud ainult AMQP protokolli kasutamisel. Puuduvad ühenduse maksud saatmise sündmuste HTTP sõltumata sellest, mitu süsteemide või seadmete kaudu. Kui kavatsete kasutada AMQP (nt saavutamine tõhusam sündmuse streaming või kahesuunaline ühendus asjade juhtimis- ja stsenaariumide lubamine), vaadake lisateavet selle kohta, mida kujutab endast vahendatud ühenduse ja kuidas need on Mahupõhised [teenuse siini hinnad teabe](https://azure.microsoft.com/pricing/details/service-bus/) lehe.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Mis vahe on sündmuse jaoturi põhi- ja Standard astme?

Sündmuse jaoturi Standard taseme pakub funktsioone kasutada sündmuse jaoturi lihtsa ja mõned konkurentsianalüüsi süsteemid. Need funktsioonid on rohkem kui 24 tundi ja võimalus kasutada ühte AMQP ühendust käsud saada suure hulga subsecond latentsused seadmed, samuti saata telemeetria nende seadmetest sündmuse jaoturi säilitamise tähtaegu. Funktsioonide loendi leiate teemast [sündmuse jaoturi hinnakirjad üksikasjad](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Geograafiline kättesaadavus

Sündmuse jaoturi on saadaval järgmistes regioonides:

|Geo|Piirkondade|
|---|---|
|Ameerika Ühendriigid|USA Kesk Ida-USA, Ida-USA 2, Lõuna-, Kesk-USA, Lääne USA|
|Euroopa|Põhja-Euroopa Lääne Euroopa|
|Aasia Vaikse ookeani piirkonna|Ida-Aasia, Kagu-Aasia|
|Jaapan|Jaapan Ida, Jaapan Lääne|
|Brasiilia|Brasiilia Lõuna|
|Austraalia|Austraalia Ida Austraalia kodutee|

## <a name="support-and-sla"></a>Tugi- ja SLA

Tehniline tugi sündmuse jaoturi on saadaval [kogukonna veebifoorumite](https://social.msdn.microsoft.com/forums/azure/home)kaudu. Arvelduste ja tellimustega dokumendihalduse toe pakutakse tasuta.

Vaadake lisateavet meie SLA [Tase lepingute](https://azure.microsoft.com/support/legal/sla/) lehte.

## <a name="next-steps"></a>Järgmised sammud

Sündmuse jaoturi kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Sündmuse jaoturi ülevaade][].
- Täieliku [valimi rakendus, mis kasutab sündmuse jaoturi][].

[Sündmuse jaoturi ülevaade]: event-hubs-overview.md
[valimi rakendus, mis kasutab sündmuse jaoturi]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
