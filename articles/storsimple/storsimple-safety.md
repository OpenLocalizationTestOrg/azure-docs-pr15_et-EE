<properties 
   pageTitle="Turvalisuse StorSimple seadme | Microsoft Azure'i"
   description="Kirjeldatakse turvalisuse põhimõtted, juhised ja kasutuse ning selgitatakse, kuidas turvaline installida ja kasutada seadme StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="safely-install-and-operate-your-storsimple-device"></a>Turvaline installimiseks ja käitamiseks StorSimple seadme

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png)
![lugeda turvalisuse teade ikooni](./media/storsimple-safety/IC740885.png) **lugeda ja -teabega**

Kogu ohutus teavet selle artikli, mis kehtib Microsoft Azure StorSimple seadme lugeda. Hoidke kõigi prinditud juhendid tarniti StorSimple seadmega hilisemaks. Järgige juhiseid õigesti häälestada, kasutada ja hooldus selle toote saate suurendada märgatavat kahju või surma või seadme või seadmete tõrge. [Sellest juhendist allalaaditav versioon](http://www.microsoft.com/download/details.aspx?id=44233) on saadaval.

## <a name="safety-icon-conventions"></a>Turvalisus ikooni põhimõtted

Siit leiate ikoonid, mille leiate, kui vaatate ettevaatusabinõusid tuleb järgida ja Microsoft Azure StorSimple seadme.

| Ikoon  | Kirjeldus  |
|:------|:-------------| 
|![Ohtu ikooni](./media/storsimple-safety/IC740879.png) **ohtu!**|Näitab ohtlikku olukorda, mille eiramise tulemuseks surma või märgatav kahju. See märksõna on piiratud kõige olukordades.| 
|![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) **hoiatus!**|Näitab ohtlikku olukorda, mis eiramise, võivad põhjustada surma või märgatav kahju.|
|![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) **ettevaatlik!**|Näitab ohtlikku olukorda, mis eiramise, võivad põhjustada kerge või mõõdukas kahju.|
|![Pange tähele ikooni](./media/storsimple-safety/IC740881.png) **teade:**|Näitab, et olulised, kuid mitte ohtu seotud teavet.|
|![Elektrilöögi ikooni](./media/storsimple-safety/IC740882.png) **elektrisüsteemi elektrilöögiohu** |Kõrgepinge|
|![Raske ikooni](./media/storsimple-safety/IC740883.png) **raske**| |
|![Pole kasutaja kasutuskõlblikud osad ikooni](./media/storsimple-safety/IC740879.png) **pole kasutaja kasutuskõlblikud osad**|Juurdepääs, välja arvatud väljaõppe.|
|![Lugeda turvalisuse teade ikooni](./media/storsimple-safety/IC740885.png)**Lugege esmalt kõiki juhiseid**| |
|![Näpunäide ohtu ikooni](./media/storsimple-safety/IC740886.png) **Näpunäide ohtu**| |


## <a name="handling-precautions"></a>Käsitsemise

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) ![raske ikooni](./media/storsimple-safety/IC740883.png) **hoiatus!** 


Vähendamiseks kahju:

- Täielikult konfigureeritud ruum võib kaaluda kuni 32 kg (70 kg); Proovige ise tõstke.
- Enne ruumi, alati tagada kaks inimest käsitlema kaalust. Pange tähele, et proovite tõstke see paksus üks inimene saate säilitada kahju.
- Tõstke ruumi, pidemete Power ja jahutus moodulid (PCMs) asuvad üksuse taga. Need on mõeldud, et võtta kaal.

## <a name="connection-precautions"></a>Ühenduse meetmed

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) ![elektrilöögi ikooni](./media/storsimple-safety/IC740882.png) **hoiatus!**

Vähendamiseks tõenäosuse kahju, elektrilöögi või surma:

- Kui tootja AC mitmest allikast, eemaldage kõik pakkumise power jaoks täiesti eraldi.

- Eemaldage üksuse jäädavalt enne, kui teisaldate need või kui arvate, et see on saanud kahjustada mis tahes viisil.

- Sisestage pakkumise juhtmed turvaliste elektrisüsteemi maa-ühendus. Veenduge, et ruumi lendude keelamiseks vastab riigi- ja kohaliku enne rakendamist power.

- Veenduge, et power ühendus katkestatakse alati enne eemaldamist on PCM kaudu ruumi.

- Lisandmooduli power pakkumise kaabli abil klõpsake on peamised ühendage seade lahti, võttes arvesse, et tagada turvasoklite väljundi seadmed asuvad ja kergesti juurdepääsetavad.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) ![elektrilöögi ikooni](./media/storsimple-safety/IC740882.png) **hoiatus!**

Tõenäosuse ülekuumenemise vähendamiseks või tule elektrisüsteemi ühendused:

- Annab sobiva allika elektrisüsteemi ülekoormuse kaitse üksikasjalikud andmed tehnilise kirjelduse nõuetele.

- Ärge kasutage adressaadipiirangute juhtmed ("Y" viib).

- Kohaldatavaid, emissiooni ja termilise nõuete täitmiseks pole tiitellehed kõrvaldamisel ja kõik lahed peavad olema täidetud järgmised lisandmooduli moodulid või drive tühjad.

- Veenduge, et seadet kasutatakse tootja määratud viisil. Selle kasutamisel ei tootja määratud viisil, võib kahjustatud seadmed kaitse.

![Pange tähele ikooni](./media/storsimple-safety/IC740881.png) **teade:**

Teie seadme ja toote vältimaks proper toimingu:

- RJ45 pordid taga seade on ainult Ethernet ühenduse. Need peab olema ühendatud side võrgus.

- Kindlasti installige seade sektsioon, kuhu mahub ees-tagasi jahutuse kujundus.

- Kõik lisandmooduli moodulid ja tühja tahvlite kuuluvad süsteemi ruumi. Need peab eemaldada ainult siis, kui asendusketta saab kohe lisada. Süsteemi peab ei saa käivitada ilma kõik moodulid või tühjad kohas.

## <a name="rack-system-precautions"></a>Sektsioon süsteemi meetmed

Kui ühendate määratud sektsioon, mis on kabineti pidada turvalisuse järgmistele nõuetele.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) ![Näpunäide ohtu ikooni](./media/storsimple-safety/IC740886.png) **hoiatus!**
 
Näpunäide kahju tõenäosuse vähendamiseks üle:

- Sektsioon kujundus toetama installitud Kirjalisandid kogukaal ja peaks sisaldama stabiliseerivaid funktsioonid sobiv raami takistada ladestamise või on lükata üle installimine või tavalise kasutamise ajal.

- Soovitud sektsioon laadimisel täitke raami alt üles ja ülevalt alla tühi.

- Libistage välja üle rohkem kui üks ruum korraga, et vältida eest raam ümberkukkumist.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) ![elektrilöögi ikooni](./media/storsimple-safety/IC740882.png) **hoiatus!**

Vähendamiseks tõenäosuse kahju, elektrilöögi või surma:

- Raami peaks olema turvalised elektrisüsteemi jaotuse. See peab võimaldama üle praeguse kaitse ruumi ja ei saa laiendada Kirjalisandid installitud koguarvuga. Elektrisüsteemi power tarbimine hinnangute näidatud nimeplaadile jälgida.

- Elektrisüsteemi jaotuse süsteemi peab võimaldama usaldusväärne põhjusel eest raam iga ümbrist.

- Kujundus on elektrisüsteemi tuleb arvestada kogu lekke praeguse kõik power tarvikute sisse kõigi Kirjalisandid kaudu. Pange tähele, et iga power pakkumise iga ruumi põhjusel lekkevool mA 1,0 kuni 60 Hz, 264 voldid. Raami võib olla vaja siltideta koos "kõrge LEKKEVOOL. Maa (maa) ühendus on oluline ühenduse tarne."

- Raam, kui konfigureeritud Kirjalisandid, peab vastama turvalisuse nõuete UL 60950-1 ja IEC 60950-1/EN 60950-1.

![Pange tähele ikooni](./media/storsimple-safety/IC740881.png) **teade:**

Teie süsteemi sektsioon proper jahutamiseks:

- Tagada, et sektsioon kujundus arvesse maksimaalne ruumi ümbritseva temperatuuri 35 kraadi (95 ° f).

- Süsteemi juhitakse rõhu, tagumine heitgaaside installimisega (tagasi rõhk loodud sektsioon uksed ja takistused ei ületa 5 Pascal [0,5 mm veesammast]).

## <a name="power-cooling-module-pcm-precautions"></a>Power jahutus mooduli ühenduse meetmed

Kasutatav seade on loodud kaks PCMs töötamiseks. Iga soovitud PCMs on pakkumise ja ventilaator kahe-telg. Kriitiline tingimuse ajal süsteem võimaldab ühe toite jätkates toiminguid ei tööta. Kahe PCMs (ja seega power tarvikute) peab alati olema installitud. Ühe PCM ei paku liigsete power. Seetõttu isegi ühe PCM tõrge võib põhjustada tööseisakute või võimalike andmete kaotsimineku.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) ![elektrilöögi ikooni](./media/storsimple-safety/IC740882.png) **hoiatus!**

Vähendamiseks tõenäosuse kahju, elektrilöögi või surma:

- Ärge eemaldage hõlmab PCM. On on õige kasutamine sees. Funktsiooni PCM ja saada asendamine, [pöörduge Microsofti klienditoe poole](https://msdn.microsoft.com/library/azure/dn757750.aspx).

![Pange tähele ikooni](./media/storsimple-safety/IC740881.png) **teade:**

Teie seadme ja toote vältimaks proper toimingu:

- Asendate nurjunud PCM 24 tunni jooksul. Pärast soovitud PCM eemaldamist asendamine asendamine 10 minuti jooksul pärast eemaldamist lõpule viia.

- Eemaldage on PCM kui lugemisprogrammi saab installida kohe. Ruumi ei tohi juhtida ilma kõik moodulid kohas.

## <a name="electrostatic-discharge-esd-precautions"></a>Elektrostaatiliste (ESD) meetmed

![Pange tähele ikooni](./media/storsimple-safety/IC740881.png) **teade:**

Ettevaatusabinõusid ESD seotud.

- Veenduge, et olete installinud ja märgitud sobiva antistaatilise rannet või ülaltpoolt kinnitamine.

- Jälgige kõik tavaliste ESD ettevaatusabinõud käsitlemisel moodulid ja komponendid.

- Vältige backplane komponendid ja mooduli konnektorid.

- ESD kahju ei kuulu garantii.

## <a name="battery-disposal-precautions"></a>Aku kõrvaldamise meetmed

Teenuse power kasutab teisiti aku ajal ajutine lühiajaline toitekatkestuste mälu sisu kaitsta. Asuvat koht on mõlema funktsiooni PCM. Pidage meeles asuvat kohta järgmine teave.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png) **hoiatus!**

Vähendamiseks lühikesed, plahvatus, kahju, või surma:

- Kasutuselt kasutatud vastavalt piirkondlike.

- Lahti, purustada, või intensiivsuskaardi kohal 60 kraadi (140 ° f) või koospõletamine. PCM kasutage ainult kaasasolevat isegi Kasutage muu tööiga võivad kujutada ohtu või plahvatus.

- Kaitse end suurtähelukk kasutamine patareid, kui need eemaldatakse välja.

![Pange tähele ikooni](./media/storsimple-safety/IC740881.png) **teade:**

Kui saatmine või muul viisil transportimiseks patareid air, IATA Liitium aku juhised dokumendi saadaval [http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx](http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx) jälgimine

Pärast nende ohutus märkused ülevaatamist on lahti, koguda ja kaabel seadme järgmiste juhiste juurde.

## <a name="next-steps"></a>Järgmised sammud

- 8100 seadme, avage [seadme StorSimple 8100](storsimple-8100-hardware-installation.md)installimiseks.

- 8600 seadme, avage [seadme StorSimple 8600](storsimple-8600-hardware-installation.md)installimiseks.

