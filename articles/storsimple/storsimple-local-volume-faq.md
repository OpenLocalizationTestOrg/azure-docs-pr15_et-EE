<properties 
   pageTitle="Kohalik StorSimple kinnitatud mahud FAQ | Microsoft Azure'i"
   description="Alt leiate vastused korduma kippuvatele küsimustele StorSimple kohalikult kinnitatud maht."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Kohalik StorSimple kinnitatud maht: korduma kippuvad küsimused (KKK)

## <a name="overview"></a>Ülevaade

Järgmised küsimused ja vastused, mida peate võib-olla loomisel StorSimple kohalikult kinnitatud helitugevust, teisendamise astmeline kohalikult kinnitatud helitugevuse (ja vastupidi), või varundamine ja taastamine kohalikult kinnitatud helitugevust.

Küsimused ja vastused on jaotatud järgmiste kategooriate

- Kohalik kinnitatud helitugevuse loomine
- Kohalik kinnitatud varundamine
- Astmeline helitugevuse teisendamine kohalikult kinnitatud maht
- Kohalik kinnitatud helitugevuse taastamine
- Probleemse kohalikult kinnitatud maht

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Küsimused kohalikult kinnitatud helitugevuse loomise kohta

**K.** Mis on maksimaalne maht kohalikult kinnitatud helitugevuse, saate loodud 8000 sarja seadmetes?

**A** saate ettevalmistamise kohalikult kinnitatud mahud 8,5 TB või astmeline mahud 8100 seadme 200 TB. Suuremate 8600 seadmes, saate ettevalmistamise kohalikult kinnitatud mahud 22,5 TB või astmeline mahud kuni 500 TB.

**K.** Hiljuti täiendanud 8100 seadmes Update 2 ja luua kohalikult kinnitatud maht katsel maksimummaht saadaval on ainult 6 TB ja pole 8,5 TB. Miks ei saa luua mõne 8,5 TB maht?

**A** saate ettevalmistamise kohalikult kinnitatud mahud kuni 8.5 TB või mitmetasandilise mahud 8100 seadme 200 TB. Kui teie seade juba on mitmetasandilise maht, siis ruumi loomise kohalikult kinnitatud helitugevus on proportsionaalselt väiksem kui see lubatud. Näiteks kui 100 TB astmeline maht on 8100 seadmes, (see on pool astmeline võimsus) juba ette valmistatud, siis Kohalik maht, mida saate luua 8100 seadme maksimummaht vastavalt vähendatakse 4 TB (umbes poole maksimaalse kohalikult kinnitatud maht).

Kuna kohaliku ruumi seadmes kasutatakse astmeline mahud töökomplekt majutada, ruumi loomise kohalikult kinnitatud helitugevuse on vähendatud, kui seade on mitmetasandilise maht. Seevastu loomise kohalikult kinnitatud helitugevuse proportsionaalselt vähendab astmeline mahud vaba ruumi. Järgmises tabelis on kokkuvõte saadaval astmeline võimsus seadmetes 8100 ja 8600 kohalikult kinnitatud mahud loomisel.

|Kohalik kinnitatud mahud ettevalmistatud võimsus|Saadaval võimet olema ette valmistatud astmeline maht - 8100|Saadaval võimet olema ette valmistatud astmeline maht - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176,5 TB | 477.8 TB|
|4 TB | 105,9 TB | 411,1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | NA | 277.8 TB |
|15 TB | NA | 166.7 TB |
|22,5 TB | NA | 0 TB |


**K.** Miks on kohalik kinnitatud helitugevuse loomise toiming? 

**V-SSE.** Kohalik kinnitatud maht on paksult ette valmistatud. Luua ruumi kohaliku astme seadme, võivad teatud andmed majandusanalüüsi büroost olemasolevate astmeline köidete lükata pilveteenusesse ebausaldusväärsete ajal. Ja kuna see sõltub maht on ette valmistatud olemasolevad andmed seadmesse ja läbilaskevõime pilveteenusesse, luua kohaliku maht kuluv aeg võib olla mitu tundi.

**K.** Kui kaua võtab luua kohalikult kinnitatud maht?

**V-SSE.** Kuna kohalik kinnitatud maht on paksult ette valmistatud, mõned olemasolevate andmete põhjal astmeline mahud võib lükata pilveteenusesse ebausaldusväärsete ajal. Seetõttu kohalikult kinnitatud draivi loomiseks kuluv aeg sõltub mitme tegureid, suurus, maht, seadme ja läbilaskevõime andmeid. Värskelt installitud seade, mis on mahud, aega, et luua kohalikult kinnitatud maht on umbes 10 minutit Teratavu andmeid. Siiski loomine kohaliku mahud võib kuluda mitu tundi seade, mis kasutab eespool kirjeldatud tegurite põhjal.

**K.** Soovin luua kohalikult kinnitatud maht. Kas mõni peaks olema teadlik head tavad?

**V-SSE.** Kohalik kinnitatud mahud sobivad töökoormus, mis nõuavad kohalikud andmed alati garantiid ja on tundliku loomuga Cloud latentsused. Käsitledes kohaliku maht, mis tahes teie töökoormus kasutamist, arvestage järgmist:

- Kohalik kinnitatud maht on paksult ette valmistatud ja loomine kohaliku maht mõju astmeline mahud vaba ruumi. Seetõttu soovitame alustada väiksemas mahud ja skaala kuni, kui teie salvestusruumi nõue suureneb.

- Ettevalmistamise kohaliku maht on toiming, mis võivad hõlmata lükkamine olemasolevate andmete põhjal astmeline mahud pilveteenusesse. Seetõttu võib jõudlus olla vähendatud nende draividel.

- Ettevalmistamise kohaliku maht on aeganõudev toiming. Aja üle arvestuse pidamiseks seotud sõltub sellest, mitu tegurid: helitugevus on ette valmistatud, andmed ja läbilaskevõime suurust. Kui teie olemasoleva maht on varundatud ei pilveteenusesse, on aeglasem draivi loomist. Soovitame võtate cloud hetktõmmiseid oma olemasolevate köidete enne, kui olete ette kohaliku helitugevuse.
 
- Saate teisendada olemasolevate astmeline köidete kohalikult kinnitatud mahud ja selle muutmine hõlmab ettevalmistamise ruumi seadme jaoks tulemuseks kohalikult kinnitatud ruumala (lisaks langetamine astmeline andmete [kui] pilvest). Ka see on toiming, mida me olete eespool teguritest. Soovitame varundada oma olemasolevate köidete teisendamine enne kui protsessi isegi aeglasem kui olemasolev maht on varundatud. Teie seade võib ka jõudlus vähendatud käigus.
    
Lisateavet [kohalikult kinnitatud helitugevuse](storsimple-manage-volumes-u2.md#add-a-volume) loomiseks

**K.** Korraga saab luua mitu kohalikult kinnitatud draivi?

**V-SSE.** Jah, kuid kohalikult kinnitatud helitugevuse loomine ja laiendamine töö töödeldakse järjest.

Kohalik kinnitatud maht on paksult ette valmistatud ja see nõuab loomine kohaliku ruumi (see võib põhjustada olemasolevaid andmeid astmeline mahud lükata pilveteenusesse ebausaldusväärsete ajal). Seetõttu, kui ettevalmistamise tööd on pooleli, muud kohaliku helitugevuse loomine tööd on pandud järjekorda kuni selle töö on valmis.

Samamoodi kui olemasoleva kohaliku draivi on laiendamist või astmeline helitugevuse teisendatakse kohalikult kinnitatud helitugevust, siis luua uus kohalik kinnitatud maht on ootele eelmise töö lõpetamiseni. Kohalik kinnitatud helitugevuse mahu laiendamine hõlmab olemasoleva kohaliku ruumi draiv laiendamiseks. Astmeline, et kohalikult kinnitatud üleminekul helitugevuse hõlmab ka loomine kohaliku ruumi jaoks tulemuseks kohalikult kinnitatud helitugevust. Nende toimingute, loomine või laiendamine kohaliku ruumi nii kaua töötab töö.

Azure'i StorSimple halduri teenuse **töö** lehel saate vaadata need tööd. Töö, mis aktiivselt töötlemise värskendatakse pidevalt kajastamiseks ettevalmistamise tühikut edenemine. Ülejäänud kohalikult kinnitatud helitugevuse tööd on märgitud töötab, kui, kuid nende edu on seiskunud ja on valitud fotodest nii, et need olid järjekorda.

**K.** Kustutatud kohalikult kinnitatud helitugevust. Miks ma ei näe kajastuksid vaba ruumi, looge uus draiv katsel taastatud ruumi? 

**V-SSE.** Kui kustutate kohalikult kinnitatud helitugevust, ei värskendata uue mahud ruumi kohe. StorSimple halduri teenuse värskendab kohaliku ruumi ligikaudu iga tund. Soovitame teil oodata üks tund enne, kui proovite luua uus draiv.

**K.** Kohalik kinnitatud mahud toetavad cloud seadmele?

**V-SSE.** Kohalik kinnitatud maht ei toetata cloud seadmele (8010 ja 8020 seadmete StorSimple virtuaalse seadme varem edaspidi).

**K.** Kasutada Azure PowerShelli cmdletid luua ja hallata kohalikult kinnitatud maht? 

**V-SSE.** Ei, ei saa luua kohalikult kinnitatud mahud Azure PowerShelli cmdlet-käskude (mis tahes Helitugevuse loote Azure PowerShelli kaudu mitmetasandilise) kaudu. Soovitame ka ei kasutada Azure PowerShelli cmdletid atribuute kohalikult kinnitatud maht, kuna see on soovimatu mõjuta helitugevuse tüüp juurde astmeline.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Küsimused kohalikult kinnitatud helitugevuse varundamine

**K.** Kas kohalik hetktõmmiste kohalikult kinnitatud mahud toetatud?

**V-SSE.** Jah, võite kohaliku hetktõmmiste teie kohalikult kinnitatud draivid. Siiski, soovitame teil regulaarselt varundada oma kohalikult kinnitatud mahud koos cloud hetktõmmiseid tagamaks, et teie andmed on kaitstud võimalikku katastroof.

**K.** On mis tahes suunised haldamise kohaliku hetktõmmiseid kohalikult kinnitatud maht?

**V-SSE.** Sagedased kohaliku hetktõmmiseid kõrval andmeid piimapütt kohalikult kinnitatud mahu kõrge võib põhjustada kohaliku ruumi seadme kiiresti ja tulemuseks andmete astmeline maht, et pilveteenuses lükata. Seetõttu soovitame teil minimeerida kohaliku hetktõmmiste arvu.

**K.** Sain teatise selle kohta, et kohaliku hetktõmmiseid kohalikult kinnitatud mahud võib kehtetuks. Kui see juhtub?

**V-SSE.** Sagedased kohaliku hetktõmmiseid kõrval andmeid piimapütt kohalikult kinnitatud mahu kõrge võib põhjustada kohaliku ruumi kasutada kiiresti seadmes. Kui seade kohaliku tasemete seas on suur koormus, muutumas täielik seade võib põhjustada on laiendatud cloud katkestuste ja sissetulevate kirjutab helitugevuse võib põhjustada kehtetuks tõmmised (nagu pole ruumi olemas värskendamiseks hetktõmmiseid viidata vanemate plokid andmeid, mis on kirjutatakse üle). Sellisel juhul jätkavad kantava kirjutab maht, kuid kohaliku hetktõmmiseid võib olla sobimatu. Pole ei mõjuta teie olemasoleva pilvepõhise hetktõmmiseid.

Teatiste hoiatus on märku, et sellisel juhul saate tekkida ja veenduge, et saate adressaate sama õigeaegne läbivaatamine teie kohaliku hetktõmmiseid ajakavasid harvem kohaliku hetktõmmiste tegemiseks või kustutamise vanemate kohaliku pilte, mis pole enam vaja.

Kui kohalik tõmmised on kehtetuks, saate teavet teatise, et teatud varukoopia poliitika kohaliku tõmmised on antud kehtetuks ajatemplid kohaliku pilte, mis olid kehtetuks loendi kõrval. Neid pilte saab automaatselt kustutatud ja te pole enam neid vaadata **Varundamise kataloogid** lehel Azure'i klassikaline portaalis.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Astmeline helitugevuse teisendamine kohalikult kinnitatud helitugevuse seotud küsimused

**K.** Ma olen jälgides mõne seadme aeglane teisendamisel astmeline helitugevuse kohalikult kinnitatud maht. Miks on see nii juhtub? 

**V-SSE.** Teisendamine hõlmab kahte toimingut: 

  1. Helitugevuse ettevalmistamise ruumi seadme jaoks soovitud kiiresti-et--ümber kohalikult kinnitatud.
  2. Astmeline andmete allalaadimisega pilveteenuses tagada kohaliku tagab.

Need juhised on pikk töötab teisendamise seade ja läbilaskevõime andmete maht suurus sõltub toiminguid. Nagu teatud andmed majandusanalüüsi büroost olemasolevate astmeline köidete võib kõrvalmõju pilveteenusesse osana ebausaldusväärsete, seadme võib jõudlus olla vähendatud selle aja jooksul. Lisaks teisendamine võib olla aeglasem kui:

- Olemasolevate köidete pole on varundatud pilveteenusesse; seega soovitame teil varundada teie mahus enne alustamist teisendus.

- Läbilaskevõime ahendamise poliitika rakendamist, mis võivad piirata läbilaskevõime pilveteenusesse; Seetõttu soovitame teil on spetsiaalne 40 Mbps või rohkem ühenduse pilveteenusesse.

- Teisendamine võib kuluda mitu tundi tõttu mitme tegureid, mis on selgitatud. Seetõttu soovitame, et te seda toimingut-peaks aegadel või vältimiseks mõju end tarbijatele koosneva nädalavahetuse.

[Astmeline helitugevuse kohalikult kinnitatud mahu teisendamise](storsimple-manage-volumes-u2.md#change-the-volume-type) kohta lisateabe saamiseks

**K.** Helitugevuse teisendamine toimingut saate tühistada?

**V-SSE.** Ei, ei saa te Loobu üks kord algatatud teisendamine toiming. Nagu eelmisele küsimusele, pidage meeles, et võib ilmneda käigus ja järgiksid parimaid tavasid, kui plaanite oma tulemus ülaltoodud võimalike jõudlusprobleemide.

**K.** Mis juhtub minu helitugevuse kui teisendamine toiming nurjub?

**V-SSE.** Helitugevuse teisendamine võib nurjuda cloud ühenduvusprobleemide tõttu. Seadme lõpuks teisendamine pärast õnnestu proovib astmeline andmete pilvest sarja lõpetada. Stsenaariumi puhul on helitugevuse tüüp jätkuvalt andmeallika helitugevuse tüüp enne teisendamine, ja:

- Kriitiline teade tõstetakse märku mahu teisendamise tõrge. Lisateavet [teatiste seotud kohalikult kinnitatud mahud](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Kui teisendate mõne astmeline kohalikult kinnitatud helitugevuse, jätkab helitugevuse astmeline atribuutidesse eksponeerida, nagu andmete võib leiduda veel pilve. Soovitame, et ühenduvuse probleemide lahendamiseks ja seejärel proovige uuesti teisendamine.
 
- Samamoodi kui üleminekul kohalikult kinnitatud astmeline helitugevuse nurjub, kuigi helitugevuse tähistatakse kohalikult kinnitatud helitugevuse, seda toimivad astmeline helitugevuse (Kuna andmed võivad olla pilveteenusesse jäänud). Siiski jätkavad hõivata ruumi kohaliku astme seade. Siia ei ole saadaval muude kohalikult kinnitatud maht. Soovitame, et te toimingut helitugevuse teisendamine on lõpule viidud ja saate taastatud seadme kohaliku ruumi.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Küsimused kohalikult kinnitatud helitugevuse taastamine

**K.** Kohalik kinnitatud müügimahud kohe taastada?

**V-SSE.** Jah, taastatakse kohalikult kinnitatud mahud kohe. Kui metaandmeid mahu tõmmatakse pilvest taastetoimingu osana, Internetis ja selle hosti pääseb. Kohaliku garantiid helitugevuse andmeid ei ole olemas, kuni kõik andmed on alla laaditud pilvest, ja võivad ilmneda vähendada, jõudluse need draividel taastamine kestel.

**K.** Kui kaua võtab taastamine kohalikult kinnitatud maht?

**V-SSE.** Kohalik kinnitatud maht on taastatud kiire ja esitada online kui helitugevuse metaandmeid tuuakse pilves, kuigi andmete maht on jätkuvalt taustal alla laadida. Selle Viimane osa taastetoimingu--saada tagasi kohaliku garantiid--andmete maht on toiming ja võib kuluda mitu tundi uuesti teha kohalikud andmed. Sama lõpuleviimiseks kuluv aeg sõltub sellest, mitu tegureid, näiteks taastatakse helitugevuse suurust ja läbilaskevõime. Kui algse helitugevuse, kus on kustutatud, suunatakse rohkem aega loomine kohaliku ruumi seadme taastetoimingu osana.

**K.** Mul on vaja minu olemasoleva kohalikult kinnitatud helitugevuse taastamiseks mõne vanema hetktõmmise (võtta, kui maht on mitmetasandilise). Helitugevuse taastatakse nagu mitmetasandilise sel juhul?

**V-SSE.** Ei, taastatakse helitugevuse nimega kohalikult kinnitatud helitugevust. Kuigi hetktõmmise kuupäevad aega, kui maht on mitmetasandilise, taastamisel olemasolevate köidete, StorSimple alati kasutab helitugevuse tüüpi kettal, kui see on praegu olemas.

**K.** Ma laiendatud minu kohalikult kinnitatud helitugevuse viimati, kuid nüüd soovin andmed taastada, kui maht on väiksemad kuupäeva ja kellaaja. Kas suuruse taastamine praeguse ja peate laiendamine helitugevuse suurust, kui taastamine on lõpule viidud?

**V-SSE.** Jah, taastamine ei mahu suuruse muutmine ja peate laiendamine helitugevuse mahu pärast taastamine on lõpule viidud.

**K.** Saate muuta maht tüüpi taastamisel?

**A.** Ei, ei saa muuta helitugevuse tüüp taastamisel.

- Salvestatud hetktõmmise tüübiks taastatakse maht, mis on kustutatud.

- Olemasolevate köidete taastatakse vastavalt oma praeguse tüüp, olenemata kasutatavast salvestatud hetktõmmise (vt eelmise kaks küsimust).
 
**K.** Minu kohalik kinnitatud helitugevuse taastamiseks tuleb, kuid ma valisin on vale punkti aja hetktõmmis. Saate tühistada praeguse taastetoimingu?

**V-SSE.** Jah, saate tühistada toimingu käimas taastamine. Helitugevuse olek kuvatakse olekusse taastamine alguses tagasi pöörata. Siiski mis tahes ajal taastamine pooleli maht tehtud kirjutab lähevad kaotsi.

**K.** Minu alustatud taastamine toimingu ühele minu kohalikult kinnitatud mahud ja nüüd näen hetktõmmise minu mahajäämus kataloogi, mille ma ei mäletama loomine. Milleks seda kasutatakse?

**V-SSE.** See on ajutine hetktõmmise, mis on loodud enne taastetoimingu ja kasutatakse tagasipööramine juhuks, kui taastamine on tühistatud või ei. Kustuta selles hetktõmmise; see kustutatakse automaatselt, kui taastamine on lõpule viidud. See käitumine võib ilmneda juhul, kui tööpäevad taastamine on kinnitatud ainult kohalikult mahud või kombinatsiooni kohalikult kinnitatud ja astmeline mahu. Kui taastamine töö sisaldab ainult astmeline mahtu, siis seda käitumist ei toimu.

**K.** Saate ma klooni kohalikult kinnitatud helitugevuse?

**V-SSE.** Jah, saate. Siiski kuvatakse kloonitud nimega astmeline helitugevuse kohalikult kinnitatud maht, vaikimisi. Lisateavet klooni [kohalikult kinnitatud helitugevuse](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Probleemse kohalikult kinnitatud helitugevuse seotud küsimused

**K.** Mul on vaja kasutada oma seadme mõne muu seadme. Minu kohalik kinnitatud maht on üle kohalikult kinnitatud või mitmetasandilise nurjus?

**V-SSE.** Sõltuvalt seadme target tarkvara versiooni, kuvatakse kohalik kinnitatud mahud nurjunud üle nimega.

- Kohalik kinnitatud kui target seade töötab StorSimple 8000 seeria värskendada 2
- Mitmetasandilise kui target seade töötab StorSimple 8000 seeria värskendada 1.x
- Mitmetasandilise kui target seade seadme pilveteenuse (tarkvara versiooni uuendamine 2 või värskendada 1.x)

Lisateavet [Tõrkesiirde ja DR kohalikult kinnitatud mahud kogu versiooni](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**K.** Kohalik kinnitatud mahud kohe taastatakse ajal avariitaastet (DR)?

**V-SSE.** Jah, kohalik kinnitatud maht on taastatud kiire Tõrkesiirde ajal. Kui Tõrkesiirde selle toimingu käigus tõmmatakse pilvest mahu metaandmeid, tuuakse online target seadmes ja pääseb selle hosti. Samal ajal helitugevuse andmeid jätkuvalt taustal alla laadida ja võib jõudlus olla vähendatud nende draividel on tõrkesiire kestel.

**K.** Tõrkesiirde töö valmis, kuidas saate jälgida edenemisega kohalikult kinnitatud maht, mis on taastatud seadme target näha?

**V-SSE.** Tõrkesiirde ajal Tõrkesiirde töö on märgitud lõpuleviiduks üks kord kõik Tõrkesiirde määramine maht on kiire taastatud ja esitada online target seadmes. See hõlmab kohalikult kinnitatud maht, mis võib nurjus kohaliku garantiid andmed on ainult siiski saadaval kui mahu kõik andmed on alla laaditud. Saate iga kohalik kinnitatud maht üle jälgimine vastavate taastamine tööd, mis on loodud osana on tõrkesiire nurjunud selle edenemist jälgida. Kohalik kinnitatud mahud luuakse ainult need üksikute taastamine tööd.

**K.** Saate muuta maht tüüpi Tõrkesiirde ajal?

**V-SSE.** Ei, ei saa muuta draivi tüüp Tõrkesiirde ajal. Kui te ei suuda üle füüsilise mõnda muud seadet, kus töötab StorSimple 8000 seeria värskendada 2, mahud on olla nurjus üle helitugevuse tüüp, mis on talletatud hetktõmmis. Kui suuda üle seadme versioon, vaadake kohal küsimus helitugevuse tippige pärast Tõrkesiirde.

**K.** Ebaõnnestuda üle helitugevuse container kohalikult kinnitatud mahud cloud seadmele?

**V-SSE.** Jah, saate. Kohalik kinnitatud maht on nurjus üle nimega astmeline maht. Lisateavet [Tõrkesiirde ja DR kohalikult kinnitatud mahud kogu versiooni](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


