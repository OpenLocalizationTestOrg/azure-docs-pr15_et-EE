<properties 
    pageTitle="Kuidas hallata Azure Redis vahemälu | Microsoft Azure'i"
    description="Saate teada, kuidas näiteks taaskäivitage arvuti ja ajakava värskendused haldustoimingute tegemine Azure'i Redis vahemälu"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Kuidas hallata Azure Redis vahemälu

Selles teemas kirjeldatakse, kuidas nagu taaskäivitus ja ajastamine värskendusi Azure'i Redis vahemälu eksemplaride haldustoimingute tegemine.

>[AZURE.IMPORTANT] Sätted ja selles artiklis kirjeldatud funktsioonid on saadaval ainult Premium taseme vahemälu jaoks.


## <a name="administration-settings"></a>Haldamise sätted

Azure'i Redis vahemälu **halduse** sätted võimaldavad järgmised haldustoimingute sooritamiseks vahemälu Premiumi jaoks. Juurde pääseda haldamise sätted, klõpsake **sätted** või **Kõik sätted** tera **sätted** jaotisse **haldus** Redis vahemälu blade ja kerimisriba.

![Haldus](./media/cache-administration/redis-cache-administration.png)

-   [Taaskäivitage arvuti](#reboot)
-   [Ajakava värskendused](#schedule-updates)

## <a name="reboot"></a>Taaskäivitage arvuti

**Taaskäivitage** tera võimaldab teil ühe või mitme sõlmed vahemälu taaskäivitage. See võimaldab teil testida rakenduse paindlikkust tõrke korral.

![Taaskäivitage arvuti](./media/cache-administration/redis-cache-reboot.png)

Kui teil on premium vahemälu rühmitamise lubatud, saate valida milliseid shards vahemälu taaskäivitamist.

![Taaskäivitage arvuti](./media/cache-administration/redis-cache-reboot-cluster.png)

Ühe või mitme sõlmed vahemälu taaskäivitamiseks valige soovitud sõlmed ja klõpsake **arvuti taaskäivitada**. Kui teil on premium vahemälu rühmitamise lubatud, valige shard(s) taaskäivitage ja seejärel klõpsake nuppu **taaskäivitage**. Mõne minuti pärast uuesti võrgus valitud node(s) taaskäivitage arvuti ja on paar minutit hiljem.

Klientrakendused mõju oleneb tehtud node(s), mis taaskäivitamist.

-   **Põhi** - kui juhtslaidi sõlm taaskäivitatakse, Azure'i Redis vahemälu ei üle koopia sõlme ja soodustab seda juhtslaidi. See Tõrkesiirde ajal võib olla lühike ajavahemik, mis ei pruugi ühendused vahemällu.
-   **Ori** – kui ori sõlm taaskäivitatakse, on tavaliselt ei mõjuta vahemälu klientidele.
-   **Mõlemad Juhtlehed ja ori** – kui mõlemad vahemälu sõlmed on rebooted, kõik andmed on kadunud vahemälu ja ühenduste vahemälu fail kuni esmane sõlm tuleb võrguühenduse taastamisel. Kui olete konfigureerinud [andmete püsimine](cache-how-to-premium-persistence.md), taastatakse Viimane varukoopia kui vahemälu tuleb tagasi online. Pange tähele, et kõik vahemälu kirjutab, et pärast Viimane varukoopia kaotsi.
-   **Node(s) premium vahemälu abil rühmitamise lubatud** - node(s) premium vahemälu taaskäivitamisel abil rühmitamise lubatud, käitumise on sama, kui taaskäivitamist node(s)-rühmitatud vahemälu.


>[AZURE.IMPORTANT] Taaskäivitage arvuti kättesaadav ainult Premium taseme vahemälu.

## <a name="reboot-faq"></a>Taaskäivitage KKK

-   [Millised sõlm peaks taaskäivitage testimiseks minu rakendus?](#which-node-should-i-reboot-to-test-my-application)
-   [Kas vahemälu tühjendamiseks klientrakenduse kaudu saate arvuti taaskäivitada?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Kaotsi andmete minu vahemälust kui uuesti teha?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Kas minu vahemälu PowerShelli, CLI või muude Haldusriistad saate arvuti taaskäivitada?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Taaskäivitage arvuti funktsiooni saate kasutada mis hinnad astme?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Millised sõlm peaks taaskäivitage testimiseks minu rakendus?

Taaskäivitage rakenduse vahemälu esmane sõlme vastu tõrge paindlikkust testimiseks **juhtslaidi** sõlm. Taaskäivitage rakenduse teisene sõlm suhtes tõrge paindlikkust testimiseks **ori** sõlm. Taaskäivitage oma taotluse vastu kokku tõrge vahemälu paindlikkust testimiseks **nii** sõlmed.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Kas vahemälu tühjendamiseks klientrakenduse kaudu saate arvuti taaskäivitada?

Jah, kui taaskäivitamist vahemälu kustutatakse kõik klientrakenduse kaudu. See võib kasulik juhul, kui kõik kliendi ühendused kasutatakse, näiteks loogika viga või klientrakenduse vea tõttu. Iga hinnakirjad taseme on erinevad [Kliendi ühenduse piirangud](cache-configure.md#default-redis-server-configuration) erineva suurusega ja kui need piirangud on jõudnud, aktsepteeritakse pole veel klientrakenduse kaudu. Taaskäivitus vahemälu võimaldab tühjendage kõik klientrakenduse kaudu.

>[AZURE.IMPORTANT] Kui teie kliendi ühendused on kasutatud loogika viga või oma kliendi koodi viga, võtke arvesse, et StackExchange.Redis automaatselt uuesti, kui Redis sõlm on tagasi online. Kui aluseks olev probleem ei lahene, jätkab klient ühendused ära kasutada.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Kas kaotan andmete minu vahemälust kui uuesti teha?

Kui soovitud **juhtslaidi** ja **ori** sõlmed taaskäivitamist kõigi andmete vahemälu (või selle Kildu, kui kasutate premium vahemälu abil rühmitamise lubatud) läheb kaotsi. Kui olete konfigureerinud [andmete püsimine](cache-how-to-premium-persistence.md), taastatakse Viimane varukoopia kui vahemälu tuleb tagasi online. Pange tähele, mis tahes vahemälu kirjutab varukoopia pärast ilmnenud kaduma.

Kui üks sõlmed taaskäivitamist andmed pole tavaliselt kaotsi, kuid ikka võib olla. Näiteks kui taaskäivitatakse juhtslaidi sõlm ja vahemälu kirjutamine on pooleli, vahemälu kirjutamine andmed on kadunud. Andmete kaotsimineku teise stsenaarium oleks kui ühe sõlme taaskäivitamist ja muu sõlm juhtub tõrke tõttu, samal ajal minna. Andmete kaotsimineku võimalike põhjuste kohta leiate lisateavet teemast [Redis mu andmed on kadunud?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Kas minu vahemälu PowerShelli, CLI või muude Haldusriistad saate arvuti taaskäivitada?

Jah, PowerShelli juhised teemast [taaskäivitage Redis vahemälu](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Taaskäivitage arvuti funktsiooni saate kasutada mis hinnad astme?

Taaskäivitage arvuti on saadaval ainult lisatasu hinnad taseme.

## <a name="schedule-updates"></a>Ajakava värskendused

**Ajakava värskenduste** tera võimaldab teil määrata hoolduse jaoks vahemälu. Akna hooldustööde pole määratud, tehtud värskendusi Redis server ajal see aken. Pange tähele, et hooldustööde akna kehtib ainult Redis server värskendused ja pole mis tahes Azure'i värskenduse või värskendab vahemälu majutavad VMs operatsioonisüsteemi.

![Ajakava värskendused](./media/cache-administration/redis-schedule-updates.png)

Hoolduse määramiseks märkige soovitud päeva ja määrake hooldustööd aknas start tund iga päev ja klõpsake nuppu **OK**. Pange tähele, et hooldustööde akna aeg on UTC-vormingus. 

>[AZURE.NOTE] Värskenduste hooldustööd aken vaikimisi on 5 tundi. See väärtus ei ole konfigureeritav Azure portaali, kuid saate konfigureerida selle PowerShelli abil soovitud `MaintenanceWindow` cmdlet-käsu [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) parameeter. Lisateavet leiate teemast [saate õnnestus ajastatud värskendused PowerShelli, CLI või muude Haldusriistad?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Värskenduste FAQ ajastamine

-   [Kui tehke värskenduste ilmneda juhul, kui ma ei kasuta funktsiooni ajakava värskendusi?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Mis tüüpi värskendused aken kavandatud hooldustööde käigus tehtud?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Kas ma saan hallatava ajastatud värskendused PowerShelli, CLI või muude Haldusriistad?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Millised hinnad astme ajakava värskenduste funktsiooni saate kasutada?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Kui tehke värskenduste ilmneda juhul, kui ma ei kasuta funktsiooni ajakava värskendusi?

Kui te ei määra hoolduse, saab igal ajal teha värskendused.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Mis tüüpi värskendused aken kavandatud hooldustööde käigus tehtud?

Ainult Redis serveri aken kavandatud hooldustööde käigus tehtud värskendused. Hoolduse akna ei kehti Azure värskenduste või VM operatsioonisüsteem värskendused.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kas ma saan hallatava ajastatud värskendused PowerShelli, CLI või muude Haldusriistad?

Jah, saate hallata oma ajastatud värskendused järgmist PowerShelli cmdlet-käskude abil.

-   [Get-AzureRmRedisCachePatchSchedule.](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Uue AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Uue AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Eemalda – AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Millised hinnad astme ajakava värskenduste funktsiooni saate kasutada?

Ajakava värskendused on saadaval ainult lisatasu hinnad taseme.

## <a name="next-steps"></a>Järgmised sammud

-   Lisateavet [Azure Redis vahemälu premium taseme](cache-premium-tier-intro.md) funktsioonidega tutvumine.





