<properties 
    pageTitle="Kuidas konfigureerida andmete püsimine Premium Azure'i Redis vahemälu jaoks" 
    description="Saate teada, kuidas konfigureerida ja hallata andmeid püsimine Premium taseme Azure'i Redis vahemälu eksemplaride" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Kuidas konfigureerida andmete püsimine Premium Azure'i Redis vahemälu jaoks

Azure'i Redis vahemälu on erinevate vahemälu pakkumisi, mis pakuvad paindlikkust vahemälu maht ja funktsioone, sh uus Premium taseme valik.

Azure'i Redis vahemälu premium taseme sisaldab funktsioone, nt rühmitamise, püsimine ja virtuaalse võrgu tugi. Selles artiklis kirjeldatakse, kuidas konfigureerida püsimine premium Azure'i Redis vahemälu eksemplari.

Muude premium vahemälu funktsioonide kohta leiate teavet teemast [Azure Redis vahemälu Premium taseme tutvustus](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Mis on andmete püsimine?
Redis püsimine võimaldab Redis talletatavad andmed säilivad. Saate võtta hetktõmmiseid ja varundage andmed, kus saate laadida riistvara tõrke korral. See on suur eelis Basic või Standard taseme, kus kõik andmed on talletatud mälu ja andmekadu vahemälu sõlmed alla paiknemise tõrke korral ei saa. 

Azure'i Redis vahemälu pakub Redis püsimine [RDB mudeli](http://redis.io/topics/persistence), kus andmed on salvestatud Azure storage konto abil. Püsimine konfigureerimisel Azure'i Redis vahemälu püsib hetktõmmise Redis vahemälu Redis kahendvormingus kettale konfigureeritav varukoopia sageduse alusel. Kui esineb katastroofiline sündmus, mis keelab nii põhi-kui ka koopia vahemälu, taastamiseni vahemälu, kasutades viimase hetktõmmis.

Püsimine saab konfigureerida keelest **Uue Redis vahemälu** vahemälu loomise ajal ja **sätted** enne olemasoleva premium vahemälu jaoks.

## <a name="create-a-premium-cache"></a>Premium vahemälu luua

Vahemälu luua ja konfigureerige püsimine, sisselogimise [Azure portaali](https://portal.azure.com) ja klõpsake nuppu **Uus**->**andmete + salvestusruumi**>**Redis vahemälu**.

![Redis vahemälu luua][redis-cache-new-cache-menu]

Püsimine konfigureerimiseks esmalt valige üks **Premium** vahemälu **Valige oma hinnakirjad taseme** tera.

![Valige oma hinnakirjad taseme][redis-cache-premium-pricing-tier]

Kui hinnad taseme lisatasu on valitud, klõpsake **Redis püsimine**.

![Redis püsimine][redis-cache-persistence]

Järgmises jaotises toodud juhised on kirjeldatud, kuidas konfigureerida Redis püsimine uue premium vahemälu. Kui Redis püsimine on konfigureeritud, klõpsake nuppu **Loo** uus premium vahemälu luua Redis püsimine.

## <a name="configure-redis-persistence"></a>Redis püsimine konfigureerimine

Redis püsimine on konfigureeritud **Redis andmete püsimine** enne. Jaoks uue vahemälu, on see blade juurde käigus vahemälu loomine eelmises jaotises kirjeldatud. Olemasoleva vahemälu, jaoks pääseb **Redis andmete püsimine** tera jaoks vahemälu keelest **sätted** .

![Redis sätted][redis-cache-settings]

Redis püsimine lubamiseks klõpsake **lubatud** lubamiseks RDB (Redis andmebaasi) varukoopia. Redis püsimine varem lubatud premium vahemälu keelamiseks klõpsake nuppu **keelatud**.

Valige varukoopia intervalli konfigureerimiseks rippmenüü loendist **Varukoopia sagedus** . Valikute hulka **15 minutit**, **30 minutit**, **60 minutit**, **6 tunni**, **12 tundi**ja **24 tundi**. Ajavahemik algab lugedes ette kui eelmine varukoopia toiming on edukalt lõpule jõudnud, ja kui see saabumisega algatatakse uus varukoopia.

Klõpsake **Salvestusruumi konto** valimine salvestusruumi konto, mida soovite kasutada, ja valige kas **primaarvõti** või **teisese võtme** **Salvestusruumi klahvi** ripploendist kasutada. Peate valima salvestusruumi konto sama piirkonna vahemälu ja **Premium salvestusruumi** konto pole soovitatav, kuna premium mälu on suurem läbilaskevõime. 

>[AZURE.IMPORTANT] Kui salvestusruumi võti püsimine konto jaoks on uuesti, peab teie **Salvestusruumi klahvi** ripploendist rechoose soovitud klahvi.

![Redis püsimine][redis-cache-persistence-selected]

Klõpsake püsimine konfiguratsiooni salvestamiseks nuppu **OK** .

Järgmise varundamise (või uue vahemälu esimese varukoopia) on algatatud pärast varukoopia sagedus intervall kaitseaega.



## <a name="persistence-faq"></a>Püsimine KKK

Järgmine loend sisaldab vastused korduma kippuvatele küsimustele Azure'i Redis vahemälu püsimine.

-   [Lubada püsimine varem loodud vahemälu?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Varunduse sagedust saate muuta pärast vahemälu luua?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Miks mul varukoopia sagedus 60 minutit on rohkem kui 60 minutit varukoopiate vahel?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Mis juhtub vana varukoopiate uue varukoopia tegemisel?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Mis juhtub, kui mul on mastaabitud, mille suurust ja taastatakse varukoopia enne selle toimingu skaleerimise?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Lubada püsimine varem loodud vahemälu?

Jah, Redis püsimine saab konfigureerida nii vahemälu loomine ja olemasoleva premium vahemälu.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Varunduse sagedust saate muuta pärast vahemälu luua?

Jah, saate muuta **andmete püsimine Redis** enne varukoopia sagedus. Lisateabe saamiseks vaadake [konfigureerimine Redis püsimine](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Miks mul varukoopia sagedus 60 minutit on rohkem kui 60 minutit varukoopiate vahel?

Varunduse sagedust intervall ei käivitu kuni eelmise varundamist on lõpule viidud. Kui varukoopia sagedus on 60 minutit ja kulub varukoopia protsessi 15 minutit edukalt lõpule, järgmise varundus ei käivitu kuni 75 minutit pärast eelmise varukoopia algusaeg.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Mis juhtub vana varukoopiate uue varukoopia tegemisel?

Kõik varukoopiate, välja arvatud uusimat kustutatakse automaatselt. Selle kustutamist võib juhtuda siis kohe, kuid mitte on vanemad varukoopiad lõputult hoitakse.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Mis juhtub, kui mul on mastaabitud, mille suurust ja taastatakse varukoopia enne selle toimingu skaleerimise?

-   Kui teil on mastaabitud suuremaks, on ei mõjuta.
-   Kui teil on mastaabitud väiksemaks ja teil on kohandatud [andmebaaside](cache-configure.md#databases) säte, mis on suurem kui [andmebaaside piirata](cache-configure.md#databases) jaoks oma uue suuruse, nende andmebaaside andmete pole võimalik taastada. Lisateavet leiate teemast [on mu kohandatud andmebaaside säte mõjutab ajal skaleerimist?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Kui teil on mastaabitud väiksemaks ja väiksem ootele panekuks viimase varukoopia andmed pole piisavalt ruumi, välja klahvid tõstetud taastamine käigus, tavaliselt abil [allkeys-lru](http://redis.io/topics/lru-cache) väljatõstmine poliitika.

## <a name="next-steps"></a>Järgmised sammud
Saate teada, kuidas kasutada rohkem vahemälu lisafunktsioonidele.

-   [Azure'i Redis vahemälu Premium taseme tutvustus](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
