<properties
   pageTitle="Teenuse struktuuri kobar ressursihaldur tutvustus | Microsoft Azure'i"
   description="Kui soovite teenuse struktuuri kobar ressursihaldur tutvustus."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Ressursihaldur kobar teenuse struktuuri tutvustus
Traditsiooniliselt haldamise süsteemide või teenuste mõeldud mõne füüsilise või virtuaalmasinates eesmärk on jätkuvalt nende teatud teenuste või süsteemide kogum. Palju suuri teenused on jaotatud "web" taseme ja "andmed" või "storage" esimese taseme võib-olla koos mõne muu eripärase komponendid, nagu vahemälu. Muud tüüpi rakendusi oleks töö taseme analüüsi või teisendus vajalikud sõnumid osana ühendatud sõnumside taseme, kus taotlusi voolas sisse ja välja. Erinevat tüüpi töökoormus kas teil on teatud masinad eesmärk on jätkuvalt selle: andmebaasi sai paari masinad pühendunud, mõned web-server. Kui kindlat tüüpi töökoormus põhjustada masinad see oli liiga kuum, siis saate lisada rohkem masinad töökoormus konfigureeritud seda tüüpi või suuremad masinad masinad mõne asendada. Lihtne. Kui masina nurjus, üldist rakenduse osa parandusfunktsiooni alumise võimsusega kuni seade võib taastada. Veel üsna lihtne, (kui see ei pruugi olla lõbusa).

Nüüd siiski vaatame Oletagem, et olete leidnud mastaapimiseks välja ja võtnud ümbriste ja/või microservice plunge vaja. Järsku leida enda kümneid, sadu või isegi tuhandeliste masinad kümneid eri tüüpi teenuseid, võib-olla sadu eri juhtudel nende teenuste, igal versioonil ühe või mitme eksemplari või koopiad kõrge kättesaadavus (HA).

Äkki keskkonna haldamise pole nii lihtne, kui mõni masinad pühendunud ühte tüüpi töökoormus haldamine. Teie serverid on virtuaalse ja pole enam nimed (saate *on* vahetada seas alates [lemmikloomad veistele](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) ju). Konfiguratsioon on väiksem masinad ja Lisateavet teenuste kohta. Spetsiaalne riistvara on nüüd minevik ja teenused on muutunud kestvad mitmele väiksem tekstilõigule kaup riistvara small jaotatud süsteemid.

Tõttu katkestamine varem monoliit, astmeline rakenduse eraldi teenustesse kaup riistvara töötab, on nüüd palju rohkem kombinatsioone lahendusvõimaluste kohta. Kes otsustab, mis tüüpi töökoormus käitamise riistvara, või kui palju? Millised töökoormus töötada ka sama riistvara ja mis on vastuolus? Kui masinat läheb... mis on ka seal töötab? Kes vastutab veenduge, et töökoormus käivitab uuesti käivitada? Tehke (virtuaalne?) arvutisse, kuni ta tagasi tuleb oodata, või kas oma töökoormus automaatselt ei üle muud seadmed ja säilitamine, kus töötab? On inimeste nõutav? Kuidas on lood sellise keskkonna uuendamine?

Arendajad ja tehtemärke elavate maailma selline, nagu me ei kavatse haldamine selle keerukuse abi vaja, kuid saate järgmise mõttes, et rentides joodud ja proovite paberi üle keerukus inimestega on pole õige vastus.

Mida teha?

## <a name="introducing-orchestrators"></a>Orchestrators tutvustus
"Orchestrator" on üldine termin tarkvara, mille abil saavad administraatorid hallata järgmist tüüpi keskkonnas. Orchestrators on komponendid, mis võtavad taotlused nagu "Soovin 5 eksemplari see teenus, mis töötab minu keskkonnas", veenduge, et see true, ja seejärel (proovige) jätke see märgituks.

Orchestrators (mitte inimestele) on, mis kiik viia, kui masina nurjus või on töökoormus lõpetab ootamatud mingil põhjusel. Enamik Orchestrators rohkem kui lihtsalt tegelema tõrge, nt aitab uue juurutuste, töötlemise uuendamine ja ressursside tarbimine, tegelemine, kuid kõik on põhjalikult säilitamise mõnda soovitud keskkonnas konfiguratsiooni oleku kohta. Soovite oskama teile öelda mõne Orchestrator, mida soovite ja on seda teha raske töö. Chronos või maraton Mesos, laevastiku, sülem, Kubernetes ja teenuse struktuuri peal on kõik näited Orchestrators (või lasta sisseehitatud). Lisateavet luuakse kogu aeg nagu keerulisi haldamise tõeline juurutuste eri tüüpi keskkonnas ja tingimuste kasvavad ja muuta.

## <a name="orchestration-as-a-service"></a>Korraldamise teenus
Töö Orchestrator jooksul teenuse struktuuri kobar käsitletakse peamiselt kobar ressursihaldur järgi. Teenuse struktuuri kobar ressursihaldur on üks süsteemi teenuste teenuse struktuuri ja käivitatakse automaatselt iga kobar.  Üldiselt kobar ressursihaldur töö on jaotatud kolm osa:

1. Reeglite jõustamine
2. Keskkonna optimeerimine
3. Muud protsessid abistamine

### <a name="what-it-isnt"></a>Mida see pole
Traditsiooniline N taseme – veebirakendustes oli alati mõned mõiste "Laadi koormusetasakaalustusteenuse", tavaliselt edaspidi Network laadi koormusetasakaalustusteenuse (NLB) või mõne rakenduse Laadi koormusetasakaalustusteenuse (ALB) olenevalt sellest, kus see l võrgu virnas. Mõned koormus soolise on nagu F5's BigIP pakkumise vastavalt riistvara, teised on nagu Microsofti tarkvara NLB. Muud keskkonnas võite näha umbes HAProxy see roll. Töö koormusetasakaalustuseks on need arhitektuurides veendumaks, et kõik erinevad kodakondsuseta esiosa masinad või erinevates arvutites klaster vastuvõtmine (umbes) sama palju tööd. See erinevad, saata iga erinevate kõne eri server, seansi kinnitamine/kleepunud, et tegelik hindamine ja kõne eraldatud eeldatavaid ja praeguse masina laadi põhinevaid strateegiad.

Pange tähele, et see on parim tagamiseks süsteem, mis web taseme jäid ligikaudu tasakaalustatud. Andmete taseme tasakaalustamiseks olnud täielikult erinevad ja sõltuvad soovitud andmed salvestusruumi, tavaliselt keskjoondamist andmete sharding ümber, vahemällu talletamine, andmebaasi hallatavad vaated ja salvestatud toimingute jne.

Kuigi need strateegiad osa on huvitav, teenuse struktuuri kobar ressursihaldur ei ole midagi nagu võrgu laadi koormusetasakaalustusteenuse või vahemälu. Võrgu laadimine koormusetasakaalustusteenuse tagab on ees lõpetatakse tasakaalustavad liikluse teisaldamine, kus töötavad teenused, teenuse struktuuri kobar ressursihaldur võtab täielikult eri strateegia – põhimõtteliselt, teenuse struktuuri viib *teenuste* , kus nad mõistlik kõige (ja eeldab, et liikluse või laadi jälgimiseks). See võib olla näiteks sõlmed, mis on praegu külmad, kuna teenuseid, mis on seal ei tee palju tööd kohe, või mis olid kustutatud või mujale teisaldatud. Teine näide kobar ressursihaldur saanud teisaldada ka teenuse eemale arvutisse, mis on uuendada või mis on ülekoormatud tõttu on kühvli tarbimine teenused, mis olid töötavate. Kuna kobar ressursihaldur vastutab teenuste ümber (ei paku võrguliikluse kui teenused on juba) teisaldada, võrrelduna, mida soovite leida, klõpsake võrgu laadi koormusetasakaalustusteenuse oluliselt erinev funktsioonide kogum sisaldab ja töötab tagada ressursid klaster kasutatakse ka erinevas strateegiad.

## <a name="next-steps"></a>Järgmised sammud
- Kobar ressursihaldur arhitektuur ja teave voo kohta lisateabe saamiseks vaadake [artiklit](service-fabric-cluster-resource-manager-architecture.md)
- Ressursihaldur kobar on palju võimalusi klaster kirjeldamiseks. Kui soovite teada saada lisateavet nende väljamöllimine see artikkel [kirjeldab teenuse struktuuri kobar](service-fabric-cluster-resource-manager-cluster-description.md)
- Teenuste konfigureerida saadaolevate suvandite kohta lisateabe saamiseks vaadake teemat teema kohta muude kobar ressursihaldur konfiguratsioone saadaval [Services konfigureerimise kohta lugege](service-fabric-cluster-resource-manager-configure-services.md)
- Mõõdikud on, kuidas struktuuri kobar Service ressursi sõim haldab tarbimine ja võimsus klaster. Saate teada, vaadake lisateavet nende ja kuidas konfigureerida neid [selles artiklis](service-fabric-cluster-resource-manager-metrics.md)
- Ressursihaldur kobar töötab teenuse struktuuri haldamise võimalusi. Lisateavet selle kohta, et integratsioon, lugege [seda artiklit](service-fabric-cluster-resource-manager-management-integration.md)
- Kohta, kuidas kobar ressursihaldur haldab ja saldo laadi klaster teada saada, vaadake artikkel [tasakaalustamiseks laadimine](service-fabric-cluster-resource-manager-balancing.md)
