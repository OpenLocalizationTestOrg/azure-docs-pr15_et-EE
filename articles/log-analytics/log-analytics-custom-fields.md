<properties
   pageTitle="Kohandatud välju Log Analytics | Microsoft Azure'i"
   description="Kohandatud väljad funktsiooni Log Analytics võimaldab teil luua oma otsitavate väljade OMS andmeid atribuutide kogutud kirje lisamine.  Selles artiklis kirjeldatakse protsess, et luua kohandatud väli ja üksikasjaliku selgituse leiate valimi sündmus."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Kohandatud välju Log Analytics

**Kohandatud välju** funktsiooni Log Analytics võimaldab teil laiendada olemasolevate kirjete OMS hoidlas oma otsitavate väljade lisamise teel.  Kohandatud välju täidetakse automaatselt ekstraktimist muid atribuute, mis on sama kirje andmete põhjal.

![Kohandatud välju ülevaade](media/log-analytics-custom-fields/overview.png)

Näiteks alltoodud valimi kirje on kasulik andmete varju jäävad sündmuse kirjeldus.  Ekstraktimiseks andmed üheks eraldi atribuudid teeb selle kättesaadavaks sortimise ja filtreerimise selliste tegevuste jaoks.

![Otsing nuppu Logi](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] Eelvaates on piiratud 100 kohandatud välju oma tööruumis.  Selle piirmäära laiendatakse, kui see funktsioon jõuab üldiselt kättesaadav.

## <a name="creating-a-custom-field"></a>Kohandatud välja loomine

Kui loote kohandatud väli, Log Analytics aru, milliseid andmeid väärtusega asustamiseks kasutada.  See kasutab tehnoloogiat Microsoft Researchi nimetatakse FlashExtract kaudu kiiresti tuvastada andmed.  Oleks vaja, saate sisestada juhised konkreetsete, õpib Log Analytics andmete kohta, mida soovite ekstraktida näited, mis pakub.

Järgmised jaotised sisaldavad juhiseid luua kohandatud väli.  Selles artiklis allosas on valimi eraldamine lühiülevaade.

> [!NOTE] Kohandatud väli on täidetud määratud kriteeriumidele vastavate kirjete lisamisel OMS andmesalve, nii, et see kuvataks ainult kirjete pärast kohandatud väli on loodud.  Kohandatud väli ei lisata kirjed, mis on juba andmesalve loomisel.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Samm 1-leida kirjed, mis on kohandatud väli
Esimene samm on kirjeid, mida saate kohandatud välja selgitada.  Saate alustada [standard log otsing](log-analytics-log-searches.md) ja seejärel valige kirje tegutseda mudelit, mis Log Analytics õppida.  Kui määrate, et te ei kavatse ekstrakti andmed kohandatud väljale, **Välja eraldamine viisard** on avatud, kus valideerimiseks ja täpsustada kriteeriumid.

2. Minge **Log otsimine** ja kasutamine [päringu kirjete toomiseks](log-analytics-log-searches.md) , mis on kohandatud väli.
2. Valige kirje kasutavate Log Analytics tegutseda mudeli ekstraktimiseks andmetega asustada kohandatud väli.  Tuvastate andmed, mida soovite selle kirje ekstraktida ja Log Analytics kasutab seda teavet kohandatud väli sarnaselt kõigi kirjete asustamiseks loogika määratlemiseks.
3. Valige **väljad väljavõte**teksti vara kirje vasakule klõpsake nuppu.
4. **Väli eraldamine viisard on avatud**ja teie valitud kirje kuvatakse veerus **Põhi näide** .  Kohandatud väli määratletud need kirjed, mille atribuute, mis on valitud sama väärtusega.  
5. Kui valikut ei ole täpselt, mida soovite, valige lisaväljade piiritlemine kriteeriumide.  Väljaväärtuste kriteeriumide muutmiseks peate tühistamine ja valige mõni muu kirje sobitamine soovitud kriteeriumid.

### <a name="step-2---perform-initial-extract"></a>Samm 2 – teha algse ekstrakti.
Kui olete kindlaks kirjeid, mida on kohandatud väli, saate määrata andmed, mida soovite eraldada.  Log Analytics kasutab seda teavet sarnased sarnaseid kirjete tuvastamiseks.  Pärast selle juhise saab kinnitamiseks tulemused ja täiendavaid üksikasju Log Analytics analüüsimisel kasutada.

1. On valimi kirjet, mida soovite kohandatud välja teksti esile tõsta.  Seejärel ilmub dialoogiboks välja nimi ja algse ekstrakti täita.  Märkide ** \_CF** lisatakse automaatselt.
2. Klõpsake **ekstraktida** kogutud kirjete analüüsi sooritamiseks.  
3. **Kokkuvõte** ja **Otsingutulemite** jaotised nii, et te saate kontrollida täpsuse ekstrakti tulemite kuvamiseks.  **Kokkuvõte** kuvatakse kriteeriumid, mis tähistavad kirjed ja loendada iga tuvastatud andmete väärtused.  **Otsingutulemite** pakub kriteeriumidele vastavate kirjete üksikasjalik loend.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Samm 3 – ekstrakti täpsuse kontrollimine ja kohandatud välja loomine

Kui tegite on algse ekstrakti, kuvatakse Log analüüsi tulemuste, mis on juba kogutud andmete põhjal.  Kui tulemused välja täpne seejärel saate luua kohandatud väli pole veel töötamise.  Kui ei, siis saate täpsemalt piiritleda tulemid, et Log Analytics saate parandada oma loogika.

2.  Kui mõni algse ekstrakti väärtuste valesti täidetud, **redigeerida** ebatäpne kirje kõrval olevat ikooni ja valige käsk **Muuda selle esiletõstmine** , et muuta valiku.
3.  Kirje kopeeritakse all **Main näide**jaotisse **täiendavad näited** .  Log Analytics mõista on teinud valiku abil saate reguleerida siin esile tõstetud.
4.  Klõpsake nuppu **ekstrakti** kasutama uut teavet hinnata olemasolevate kirjete.  Kirjete põhjal selle uue ärianalüüsi äsja muudetud muus võib muuta tulemused.
5.  Jätkake seni, kuni kõik kirjed ekstrakti õigesti tuvastada asustamiseks uue kohandatud välja andmete parandused lisamiseks.
6. Kui olete tulemusega rahul, klõpsake nuppu **Salvesta eraldamiseks** .  Kohandatud väli on nüüd määratletud, kuid see ei lisata kõik kirjed veel.
7.  Oodake, kuni kogutud ja käivitage uuesti log otsing on määratud kriteeriumidele vastavaid uusi kirjeid. Uusi kirjeid peaks olema kohandatud väli.
8.  Kohandatud välja nagu kirje atribuudi kasutada.  Saate selle abil andmete rühmitamine ja liitväärtuse ja isegi kasutada andes uusi ülevaateid.


## <a name="viewing-custom-fields"></a>Kohandatud välju vaatamine
Kõigi kohandatud väljade loendit saate vaadata oma haldus jaotises **sätted** paani OMS armatuurlaua.  Valige **andmed** ja seejärel **kohandatud väljade** loendi kõigi kohandatud väljade tööruumis.  

![Kohandatud väljad](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Kohandatud välja eemaldamine
On kaks võimalust kohandatud välja eemaldamiseks.  Esimene on iga välja suvandi **Eemalda** , kui täielik loend on eespool kirjeldatud.  Muud meetodit on tuua kirje ja klõpsake väljast vasakul asuvat nuppu.  Menüü on võimalus kohandatud välja eemaldamiseks.

## <a name="sample-walkthrough"></a>Valimi kiirtutvustus

Järgmises jaotises tutvustatakse täieliku näide luua kohandatud väli.  Selles näites ekstraktib Windows sündmused, mis näitab, et teenuse olek muutmise teenuse nimi.  See tugineb sündmused, mis on loodud teenuse juhtimise haldur süsteemi Logi Windowsi arvuti.  Kui soovite järgige selles näites, peate olema [koguda teavet sündmuste logi süsteem](log-analytics-data-sources-windows-events.md).

Me sisestage järgmine päring kaudu teenuse juhtimise haldur sündmuse ID 7036, mis on sündmus, mis näitab, et teenuse käivitamine või peatamine on kõik sündmused tagastamiseks.

![Päringu](media/log-analytics-custom-fields/query.png)

Me valige sündmus ID 7036 ükskõik millist kirjet.

![Kirje allikas](media/log-analytics-custom-fields/source-record.png)

Soovime teenuse nimi, mis kuvatakse **RenderedDescription** atribuudi ja valige selle atribuudi kõrval asuvat nuppu.

![Väljad väljavõte](media/log-analytics-custom-fields/extract-fields.png)

**Väli eraldamine viisard** on avatud ja **EventLog** ja **EventID** väljad on valitud veerus **Main näide** .  See näitab, et kohandatud väli kindlaks sündmuste logi süsteemi, 7036 ID-ga.  See on piisavalt, et me ei pea muude väljade valimiseks.

![Peamised näide](media/log-analytics-custom-fields/main-example.png)

Me esile tõsta teenuse **RenderedDescription** atribuudi nimi ja **teenuse** abil saate tuvastada teenuse nimi.  Kohandatud välja nimi **Service_CF**.

![Väli tiitel](media/log-analytics-custom-fields/field-title.png)

Näeme, et teenuse nimi on määratletud õigesti mõned kirjed, kuid mitte teiste jaoks.   **Otsingutulemite** näitavad, et **WMI jõudluse adapterit** nime ei olnud valitud.  **Kokkuvõte** näitab, et nelja kirje **DPRMA** teenusega valesti sisalduv on saadaval täiendav word ja kahe kirje tuvastatud **Moodulid Installer** asemel **Moodulid valikut Windows Installer**.  

![Otsingutulemid](media/log-analytics-custom-fields/search-results-01.png)

Alustame **WMI jõudluse adapterit** kirje.  Me klõpsake selle redigeerimisikooni ja seejärel **Muuda see esile tõstetud**.  

![Esiletõstmine muutmine](media/log-analytics-custom-fields/modify-highlight.png)

Esiletõstu kaasata **WMI** sõna ja seejärel käivitage uuesti ekstrakti suurendamine  

![Täiendavad näide](media/log-analytics-custom-fields/additional-example-01.png)

Me näeme, et **WMI jõudluse adapterit** kirjeid on parandatud ja Logi Analytics kasutada ka seda teavet õige kirjete **Mooduli valikut Windows Installer**.  Me näeme **Kokkuvõte** jaotises kuigi see **DPMRA** on endiselt ei tuvastatud õigesti.

![Otsingutulemid](media/log-analytics-custom-fields/search-results-02.png)

Leidke kirje DPMRA teenuse- ja sama toimingu abil saate lahendada seda kirjet.

![Täiendavad näide](media/log-analytics-custom-fields/additional-example-02.png)

 Kui soovitud eraldamine käitamiseks näeme, et kõik meie tulemused on nüüd täpne.

![Otsingutulemid](media/log-analytics-custom-fields/search-results-03.png)

Me näeme, et **Service_CF** on loodud, kuid ei ole veel kõik kirjed lisanud.

![Algne loendamine](media/log-analytics-custom-fields/initial-count.png)

Pärast teatud aja möödumist nii uus sündmuste kogutakse, me näeme, et et **Service_CF** väli on nüüd lisatud kirjete sobivad meie kriteeriumid.

![Tulemused](media/log-analytics-custom-fields/final-results.png)

Kohandatud välja nagu kirje atribuudi nüüd saate kasutada.  Kujutamiseks, saame luua päring, mis pakuvad uue **Service_CF** välja milliseid teenuseid on kõige aktiivse kontrollimiseks.

![Päringu alusel rühmitamine](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsinguid](log-analytics-log-searches.md) koostamiseks päringute kohandatud väljade jaoks kriteeriumide abil.
- Jälgida [kohandatud logifailid](log-analytics-data-sources-custom-logs.md) , mis teil sõeluda, kasutades kohandatud välju.