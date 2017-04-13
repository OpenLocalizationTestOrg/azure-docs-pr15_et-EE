<properties 
    pageTitle="Teenuse siini hinnad ja arveldamine | Microsoft Azure'i"
    description="Teenuse siini hinnad struktuuri ülevaade."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Teenuse siini hinnad ja arveldamise kohta

Teenuse siini on saadaval [Basic, Standard, ja](service-bus-premium-messaging.md) astme. Soovi korral saate iga teenuse siini nimeruumi, mille loote teenuse kiht ja selle taseme valiku kehtib üle kõik üksused, mis on loodud selle nimeruumi.

>[AZURE.NOTE] Üksikasjalikku teavet praeguse teenuse siini hinnad, leiate [Azure'i teenus siini hinnad lehe](https://azure.microsoft.com/pricing/details/service-bus/)ja [Teenuse siini KKK](service-bus-faq.md#service-bus-pricing).

Teenuse siini kasutab järgmised kaks meetrit järjekorrad ja Teemad/tellimused:

1. **Sõnumside toimingud**: määratletud API kõned vastu järjekorda või teema/tellimus teenuse lõpp-punktid. Arvesti asendab sõnumeid saata või vastu esmane ühik tasustatav kasutus järjekorrad ja Teemad/tellimused.

2. **Vahendajaks ühendused**: määratletud maksimaalne arv püsivestluse ühendused avamine järjekorrad, teemade või tellimuste antud üks tund valimite perioodi jooksul. Arvesti rakendatakse ainult Standard astme, saate avada täiendavad andmeühenduste (varem, ühendused olid piiratud 100 järjekorda/teema/tellimuse kohta) nominaalne kohta ühenduse eest.

**Standardse** taseme tutvustab lõpetanud hinnad jaoks järjekorrad ja tulemuseks maht põhinevad allahindluse 80% kõrgeimal kasutus tasemel Teemad/tellimused toimingud. Olemas on ka Standard astme alus tasuta $ 10 kuus, mis võimaldab teil kuus täiendava tasuta kuni 12,5 miljonit toiminguid.

**Premium** taseme pakub ressursside eraldamise CPU ja mälu kihis nii, et iga kliendi töökoormus töötab eraldi. Selle ressursi container nimetatakse, *sõnumid üksus*. Iga premium nimeruum eraldatakse sõnumside vähemalt ühe üksuse. Saate osta 1, 2 või 4 iga teenuse siini Premium nimeruum sõnumside mõõtühikud. Ühe töökoormus või üksus võib ühendada mitu sõnumside üksuste ja sõnumside ühikute saate muuta, kuigi arveldamine on 24-tunnise või iga päev määr kulud. Tulem on teie teenuse siini-põhise lahenduse hõlpsalt prognoosida ja korratavad jõudlust. Mitte ainult on selle jõudluse hõlpsalt prognoosida ja saadaval, kuid see on ka kiirem. Azure'i teenus siini Premium sõnumside koostab kasutusele Azure'i sündmuse jaoturi salvestusruumi mootor. Nii Premium, on palju kiirem kui Standard taseme tipp jõudlus.

Pange tähele, et standard alus tasu nõutakse ainult üks kord kuus Azure tellimuse kohta. See tähendab, et pärast ühe Standard- või Premium taseme teenuse siini nimeruum loomist saab luua nii palju täiendavad Standard või Premium taseme nimeruumid soovitud jaotises sama Azure tellimuse, ilma, et tekiks alus lisatasud.

Kõik olemasolevad teenuse siini nimeruumid loodud enne 1 November 2014 olid automaatselt paigutatud Standard taseme. See tagab, et teil jätkuvalt kõik funktsioonid, mis on praegu saadaval teenuse siini juurde. Seejärel saate [Azure klassikaline portaali][] alandada tavaline astme soovi.

Järgmises tabelis on toodud põhi- ja Standard/Premium astme otstarbekas erinevused.

|Võimalus|Tavaline|Standard/Premium|
|---|---|---|
|Järjekorrad|Jah|Jah|
|Ajastatud sõnumid|Jah|Jah|
|Teemade/tellimused|Ei|Jah|
|Releed|Ei|Jah|
|Tehinguid|Ei|Jah|
|De dubleerimine|Ei|Jah|
|Seansid|Ei|Jah|
|Suur sõnumid|Ei|Jah|
|ForwardTo|Ei|Jah|
|SendVia|Ei|Jah|
|Vahendatud ühendused (kaasa arvatud)|100 teenuse siini nimeruum|1000 Azure tellimuse kohta|
|Vahendatud ühendused (lisatud liigses lubatud)|Ei|Jah (tasustatav)|

## <a name="messaging-operations"></a>Sõnumside toimingud

Uue hinnakirjad mudeli osana muutub arveldus järjekorrad ja Teemad/tellimused. Need üksused on üleminek arveldamine ühe sõnumi toimingus arveldus. "Toiming" viitab mis tahes API kõne vastu on järjekorda või teema/tellimus teenuse lõpp-punkti. See hõlmab haldus, saatmise/vastuvõtu ja seansi olek toimingud.

|Toimingu tüüp|Kirjeldus|
|---|---|
|Haldus|Loomine, lugemine, värskendamine, kustutamine (CRUD) järjekorrad või Teemad/tellimused.|
|Sõnumside|Sõnumite saatmine ja vastuvõtmine järjekorrad või Teemad/tellimused.|
|Seansi olek|Saada või säte seansi olek järjekorda või teema/tellimus.|

Kehtivad järgmised hinnad alates 1 November 2014:

|Tavaline|Maksumus|
|---|---|
|Toimingud|$0.05 miljonit toimingud|

|Standard|Maksumus|
|---|---|
|Base tasuta|$10/ kuu|
|Esmalt 12,5 miljonit toimingute kohta kuus|Sisalduv|
|12,5-100 miljonit toimingute kohta kuus|$0,80 miljonit toimingute kohta.|
|100 miljonit - 2 500 toimingute kohta kuus|0,50 $ miljonit toimingud|
|Üle 2 500 miljoni toimingute kohta kuus|$0,20 miljonit toimingute kohta.|

|Premium|Maksumus|
|---|---|
|Iga päev|$11.13 kindla sõnumi ühiku kohta|

## <a name="brokered-connections"></a>Vahendatud ühendused

*Brokered ühendused* majutada kliendi mustreid hõlmavad suure hulga "püsivalt ühendatud" saatjad/vastuvõtjad järjekorrad, teemade või tellimused. Püsivalt ühendatud saatjad/vastuvõtjad on need, mis ühenduse loomiseks kasutada AMQP või HTTP, mitte null vastuvõtmine ajalõpp (nt HTTP pikk küsitlused). HTTP saatja ja vastuvõtja koos mõne kohe ajalõpp ei looda vahendatud ühendused.

Varem järjekorrad ja Teemad/tellimuste oli limiit 100 samaaegseid ühendusi URL-i kohta. Praeguse arvelduse värviskeemi eemaldab URL-i kohta järjekorrad ja Teemad/tellimused ja rakendab kvootide ja mõõtmine vahendatud ühendustele teenuse siini nimeruum ja Azure tellimuse tasandil.

Tavaline astme sisaldab ja on tingimata piiratud, 100 vahendatud ühendus teenuse siini nimeruum kohta. See arv ülaltoodud ühendused on tagasi lükatud tavaline astme. Standardse taseme kohta nimeruum eemaldab- ja loendab liitväärtuse vahendatud ühenduse kasutamine Azure'i tellimus üle. Standardse taseme, 1000 vahendatud ühendused Azure'i tellimuse kohta on lubatud ilma täiendava tasuta (lisaks base eest). Kasutades rohkem kui 1000 vahendatud ühendused kokku üle Standard-astme teenuse siini nimeruumid Azure tellimuse sisse, saadetakse teile arve lõpetanud ajakava, nagu on näidatud järgmises tabelis.

|Vahendatud ühendused (Standard taseme)|Maksumus|
|---|---|
|Esimese kuu/1000|Base tasuta kaasas|
|1000 100 000 kuus|0,03 $ ühenduse kuus|
|100 000 500 000 kuus|$0.025 ühenduse/kuus|
|500 000/kuu|$0.015 ühenduse/kuus|

>[AZURE.NOTE] 1000 vahendatud ühendused on kaasas Standard sõnumside esimese taseme (kaudu base eest) ja saab jagada järjekorrad, teemade ja seotud Azure tellimuse tellimustest.

<br />

>[AZURE.NOTE] Arveldamine on maksimaalne arv samaaegseid ühendusi ja on ees tunni põhjal 744 tundi kuus.

|Premium taseme
|---|
|Vahendatud ühendused ei võeta Premium astme.|

Vahendatud ühenduste kohta lisateabe saamiseks vt käesoleva teema jaotisest [KKK](#faq) .

## <a name="relay"></a>Edastamine

Releed on saadaval ainult Standard taseme nimeruumid. Muul juhul hinnad ja ühenduse kvootide jaoks releed ei muudeta. See tähendab, et releed jätkub palju sõnumeid (mitte toimingud), ja seda relee tundi.

|Relee hinnad|Maksumus|
|---|---|
|Relay tundi|$0,10 iga tunni järel 100 edastamine|
|Sõnumite|$0,01 iga 10 000 sõnumite jaoks|

## <a name="faq"></a>KKK

### <a name="how-is-the-relay-hours-meter-calculated"></a>Kuidas arvutatakse Relay tundi arvesti?

[Teemat.](service-bus-faq.md#how-is-the-relay-hours-meter-calculated)

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Mis on vahendajaks ühendused ja kuidas teha I saamiseks tuleb neid?

Vahendatud ühendus on määratletud ühte järgmistest:

1. Jaoks AMQP ühenduse kliendi teenuse siini järjekorda või teema/tellimus.

2. HTTP-kõne saada sõnumi teenuse siini teema või järjekorda, mis sisaldab vastuvõtu ajalõpp väärtus on suurem kui null.

Teenuse siini kulude maksimaalne arv samaaegseid vahendatud ühendusi, mis ületab kaasatud (Standard astme 1000). Peaks mõõdetud üks kord tunnis, 744 tunnid kuud jagamisel ees ja arvelduse kuu jooksul liita. Arveldusperioodi eest ühtlase kord tunnis peaks summa lõpus on rakendatud kaasatud kogus (1000 vahendatud ühenduste kohta kuus).

Näiteks:

1. Iga 10 000 seadmete loob ühe AMQP ühenduse kaudu ja saab käsud teenuse siini teema. Seadmete saata telemeetria sündmuste sündmuse jaoturiga. Kui kõigis seadmetes ühendust 12 tundi iga päev, kuvatakse järgmine ühenduse kõnetasud (lisaks muude teenuste siini teema maksude): 31 päeva 10 000 ühendused *12 tundi* / 744 = 5000 vahendajaks ühendused. Pärast igakuine summa 1000 vahendatud ühendused, oleks tasuda 4000 vahendatud ühendused, mis moodustab 0,03 $ $120 kokku vahendatud ühendust.

2. 10 000 seadmed saadud sõnumeid teenuse siini järjekorda HTTP, milles null ajalõpp kaudu. Kui kõigis seadmetes ühendust 12 tundi iga päev, näete järgmist ühenduse kulude (lisaks muude teenuste siini kulude): 10 000 HTTP-vastuvõtmine ühendused *12 tundi päevas* 31 päeva / 744 tundi = 5000 vahendajaks ühendused.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Tehke vahendatud ühenduse kõnetasud järjekorrad ja Teemad/tellimuste?

Jah. Puuduvad ühenduse maksud saatmise sündmuste HTTP sõltumata sellest, mitu süsteemide või seadmete kaudu. Http kaudu ajalõpp, mis on suurem kui null, nimetatakse "pikk küsitlused," sündmuste vastuvõtmine loob vahendatud ühenduse kulud. AMQP ühendused luua vahendatud ühenduse kulude sõltumata sellest, kas ühendused kasutada saata ega vastu võtta. Pange tähele, et 100 vahendatud ühendused on lubatud tasuta Basic nimeruumi. See on ka vahendatud ühendused Azure'i tellimuse jaoks lubatud maksimaalne arv. Esimene 1000 vahendatud ühendused üle kõik Standard nimeruumid rakenduses Azure tellimuse kaasatakse tasuta (lisaks base eest). Kuna see on piisavalt katta palju-teenusest sõnumside stsenaariumid, vahendatud ühenduse kulude tavaliselt alles oluline, kui kavatsete kasutada AMQP või HTTP pikk küsitlused suure hulga kliendid; Näiteks, et saavutada tõhusam sündmuse streaming või kahesuunalise suhtlemise palju seadmeid või teenuserakenduse eksemplaride lubamine.

## <a name="next-steps"></a>Järgmised sammud

- Teenuse siini hindade kohta leiate lisateavet teemast [Azure teenuse siini hinnad lehe](https://azure.microsoft.com/pricing/details/service-bus/).

- Mõned levinud KKK ümber teenuse siini hinnad ja arveldamise kohta teemast [Teenuse siini KKK](service-bus-faq.md#service-bus-pricing) .

[Azure'i klassikaline portaal]: http://manage.windowsazure.com