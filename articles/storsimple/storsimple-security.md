<properties 
   pageTitle="StorSimple turvalisus | Microsoft Azure'i" 
   description="Kirjeldatakse turvalisus ja privaatsus funktsioone, mis kaitsevad StorSimple teenuse, seadme ja andmete kohapealne ja pilveteenuses." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple turvalisus ja andmekaitse

## <a name="overview"></a>Ülevaade

Turvalisus on suur probleem kõigile, kes vastu uue tehnoloogia, eriti siis, kui tehnoloogia kasutatakse konfidentsiaalse või kaitstud andmetega. Kui hindate erinevaid tehnoloogiaid, peate arvesse võtma suurenevad riskid ja kulud andmekaitse. Microsoft Azure'i StorSimple pakub nii turvalisus ja privaatsus lahenduse andmekaitse, aitavad tagada: 

- **Konfidentsiaalsusega** – ainult volitatud üksuste saate vaadata oma andmeid. 
- **Terviklus** – ainult volitatud üksused saate muuta või kustutada.

Microsoft Azure'i StorSimple lahendus koosneb neljast põhikomponendist, mis omavahel suhelda.

- **StorSimple halduri teenuse majutatud Microsoft Azure** -halduse teenuse ettevalmistamise StorSimple seade ja konfigureerimiseks kasutatav.
- **StorSimple seadme** – füüsilise seadmega oma andmekeskuse installitud. Kõik majutajate ja klientidele, et luua andmete StorSimple seadmega ühendada ja seadme haldab andmed ja teisaldab selle Azure pilveteenusesse vastavalt vajadusele.
- **Klientide/hosts ühendatud** – klientidele oma taristu StorSimple seadmega ühendada ning andmeid, mida tuleb kaitsta luua.
- **Salvestusruumi** – andmete talletuskoht Azure'i pilves asukoht.

Järgmistes jaotistes kirjeldatakse StorSimple turbefunktsioonid, mis aitab kaitsta iga salvestatud need andmed ja nende komponentide. See hõlmab ka küsimusi, millele te peate võib-olla kohta Microsoft Azure'i StorSimple turbe- ja vastavate vastused loendit.

## <a name="storsimple-manager-service-protection"></a>StorSimple halduri teenuse kaitse

StorSimple halduri teenus on halduse teenuse Microsoft Azure majutatud ja kõik teie asutuses on hangitud StorSimple seadmete haldamiseks kasutatavad. Pääsete StorSimple halduri teenuse ettevõtte mandaadi abil veebibrauseri kaudu Azure'i klassikaline portaali sisse logima. 

Juurdepääs teenusele StorSimple halduri nõuab, et teie asutuses on Azure tellimuse, mis sisaldab StorSimple. Tellimuse reguleerib funktsioone, millele pääsete juurde Azure klassikaline portaalis. Kui teie asutus ei saa Azure tellimuse ja soovite rohkem teada neile, leiate [Azure'i organisatsiooni kasutajaks](../active-directory/sign-up-organization.md). 

Kuna StorSimple halduri teenuse majutab Azure'is, on kaitstud Azure turbefunktsioonid. Microsoft Azure'i, turbefunktsioonid kohta lisateabe saamiseks minge [Microsoft Azure'i usalduskeskuses](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>StorSimple seadme kaitse

StorSimple seade on kohapealse hübriid mäluseade, mis sisaldab välkdraivid (SSD) ja kõvaketta draivid (kõvaketast) koos liigsete kontrollerid ja automaatne Tõrkesiirde võimalused. Kontrolörid Halda salvestusruumi tiering, hoidke praegu kasutatud (või kuum) andmete (StorSimple seadme või kohapealse serverid), kohalikku talletusruumi liikudes vähem sageli kasutatud andmeid pilve.

Ainult autoriseeritud StorSimple seadmete StorSimple halduri teenust Azure tellimuse loodud lubatud. Lubada seadme, peate selle registreerima StorSimple halduri teenuse esitada teenuse registreerimise võti. Teenuse registreerimise võti on 128-bitine juhusliku võti loodud Azure klassikaline portaalis. 

![Teenuse registreerimise võti](./media/storsimple-security/ServiceRegistrationKey.png)

Saate teada, kuidas saan teenuse registreerimise võti, minge [Samm 2: saada teenuse registreerimise võti](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

Teenuse registreerimise võti on pikk võti, mis sisaldab 100 + märke. Saate kopeerida võti ja salvestage see tekstifail turvalises asukohas nii, et saate seda kasutada täiendavaid seadmeid vastavalt vajadusele lubada. Kui teenus registreerimise võti on kadunud pärast esimese seadme registreerimist, saate luua uue tootenumbri StorSimple halduri teenuse. See ei mõjuta olemasolevate seadmete toimimist. 

Pärast seadet on registreeritud, kasutab sõned Microsoft Azure'i suhelda. Pärast seadme registreerimine ei kasutata teenuse registreerimise võti.

> [AZURE.NOTE] Soovitame, et pärast iga kasutamist taastada teenuse registreerimise võti.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Kaitsta teie StorSimple lahendus paroolide kaudu

Paroolid on oluline osa arvuti turvalisuse ja kasutatakse ulatuslikult StorSimple lahenduse tagada, et teie andmed on juurdepääs ainult autoriseeritud kasutajad. StorSimple abil saate konfigureerida järgmises paroolide:

- StorSimple seadme administraatori parool
- Väljakutse käepigistus autentimise Protocol (CHAP) algataja ja target paroolid
- StorSimple hetktõmmise halduri parooli

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShelli StorSimple ja StorSimple seadme administraatori parool

Windows PowerShelli StorSimple on käsurea liides, mille abil saate hallata StorSimple seade. Windows PowerShelli StorSimple on funktsioonid, mis võimaldab teil oma seadme registreerimine, konfigureerida võrgu liidese teie seadmes, teatud tüüpi värskenduste installimine, tõrkeotsing seadme tugi seansi kasutades ja seadme muuta. Pääsete juurde Windows PowerShelli StorSimple ühenduse järjestikune konsooli seadmesse või remoting Windows PowerShelli abil.

PowerShelli remoting saab teha üle HTTPS või HTTP. Kui kaughalduse https on lubatud, saate tuleb alla laadida kaughalduse sert seadmest ja installige see kaugtöölaua klient. PowerShelli remoting kohta lisateabe saamiseks minge [kaugühenduse teel ühenduse StorSimple seadme](storsimple-remote-connect.md).

Pärast Windows PowerShelli StorSimple ühenduse seadme kasutamiseks peate seadme administraatori parooli, et seadmesse sisse logida.

![Seadme administraatori parool](./media/storsimple-security/DeviceAdminPW.png)

Järgmiste juhiste täitmisel pidage meeles järgmist.

- Kaughalduse on vaikimisi välja lülitatud. StorSimple halduri teenuse abil saate selle taas lubada. Kui turvalisuse põhitõde Kaugpöördusteenuse peaks olema lubatud ainult seda vaja on tegelikult ajavahemikus.
- Kui muudate parooli, ärge unustage Kaugpöördusteenuse kõigi kasutajate teavitada, et nad ei koge kui ootamatud ühenduvuse kadu.
- StorSimple halduri teenus ei saa laadida olemasolevad paroolid: see ainult kustutan. Soovitame talletada paroole kindlas kohas, et teil on, kui see on unustatud parooli lähtestamiseks. Kui vajate parooli lähtestamiseks, kindlasti enne lähtestamist kõik kasutajad teatada. 

Järjestikune ühenduse seadme abil pääsete juurde kasutajaliidese Windows PowerShelli. Võite kasutada ka selle kaugühenduse teel, kasutades HTTP- või HTTPS-i, mis võimaldab suurema turvalisuse tagamiseks. HTTPS pakub kõrgemad väärtpaberi järjestikust või HTTP-ühenduse kaudu. Siiski HTTPS kasutamiseks te peate esmalt installima sert on juurdepääs seadme klientarvutis. Saate alla laadida Kaugpöördusteenuse sertifikaadi StorSimple halduri teenuse lehelt seadme konfigureerimine. Kui Kaugpöördusteenuse sert on kadunud, peate alla laadida uut serti ja seda levitada kõigile klientidele, kes on volitatud kasutama kaughalduse.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Väljakutse käepigistus autentimise Protocol (CHAP) algataja ja target paroolid

CHAP on autentimise skeem, mis kasutavad StorSimple seadme remote klientide identiteedi kontrollimine. Kinnitamine põhineb ühiskasutusega parooli. CHAP võib olla ühesuunalise (Ühesuunalised) või vastastikune (kahesuunaline). Target (StorSimple seadme) autendib ühesuunalise CHAP, kus algataja (host). Vastastikune või tagant CHAP nõuab sihtkohas autentida algataja ja seejärel algataja autentida sihtkohas. Kas meetodit kasutada saab konfigureerida oma StorSimple.

Arvestage järgmisi CHAP konfigureerimisel:

- CHAP kasutajanimi peab sisaldama vähem kui 233 märki.
- CHAP parooli peab olema 12 – 16 märki. Proovite kasutada rohkem kasutajanime või parooli tulemusena hostis Windowsi autentimist tõrke.
- Te ei saa kasutada sama parool CHAP algataja nii CHAP Target (sihtkoht).
- Pärast parooli seadmiseks saate seda muuta, kuid seda ei saa tuua. Kui parool on muutunud, kindlasti kõik Kaugpöördusteenuse kasutajad teavitada, et nad saavad ühendada StorSimple seade.

CHAP ja seda oma StorSimple lahenduse jaoks konfigureerimise kohta lisateabe saamiseks külastage [Konfigureerimine Peatükk StorSimple seadme](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple hetktõmmise halduri parooli

StorSimple hetktõmmise Manager on Microsoft Management Console (MMC) lisandmoodulit, mis kasutab helitugevuse rühmad ja Windowsi draivi teenus vari rakenduse ühtsete varukoopiate loomiseks. Lisaks saate luua varukoopia ajakavasid ja klooni või taastada mahud StorSimple hetktõmmise halduri.

Seade kasutada StorSimple hetktõmmise halduri konfigureerimisel peavad teil esitada StorSimple hetktõmmise halduri parool. See on esimene seatud parool Windows PowerShelli StorSimple registreerimise käigus. Parooli saate ka määrata ja muuta StorSimple halduri teenuse. See parool autendib seadme StorSimple hetktõmmise halduriga.

![StorSimple hetktõmmise halduri parooli](./media/storsimple-security/SnapshotMgrPassword.png)

StorSimple hetktõmmise halduri parooli peab olema 14-15 märgid ja peab sisaldama 3 või enama suurtähtedeks, väiketähtedeks, arvulised ja teisiti märkide kombinatsioon. Pärast StorSimple hetktõmmise halduri parooli seadmiseks saate seda muuta, kuid seda ei saa tuua. Kui muudate parooli, kindlasti remote kõigi kasutajate.

Lisateabe saamiseks StorSimple hetktõmmise halduri, minge [mis on StorSimple hetktõmmise haldur?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Parooli heade tavade

Soovitame kasutada järgmised juhised aitavad tagada, et StorSimple paroolid on tugev ja hästi kaitstud:

- Oma parooli muuta iga kolme kuu. Paroolide muutmise jõustatakse kord aastas.
- Kasutage keerukad paroolid. Lisateabe saamiseks minge [luua keerukamat parooli ja neid kaitsta](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Kasuta alati erinevad paroolid puhul eri juurdepääsu; iga teie määratud paroolide peavad olema kordumatud.
- Ei jaga paroolid kõigile, kellel on õigus pääseda StorSimple seade.
- Rääkida ees teised parooli või vihje parooli vorming.
- Kui kahtlustate, et konto või parool on rikutud, ettekande teie teabe turvalisus osakonna poole.
- Kõik paroolid käsitleda tundliku loomuga, konfidentsiaalset teavet. 

## <a name="storsimple-data-protection"></a>StorSimple andmekaitse

Selles jaotises kirjeldatakse StorSimple turbefunktsioonid, mis kaitsevad töödeldavate andmete ja talletatud andmed.

Muude jaotiste kohaselt, kasutatakse paroolide lubada ja autentimiseks enne, kui nad pääseksid juurde teie StorSimple lahendus. Mõne muu turvalisus tasu on andmete kaitsmine volitamata ajal see kantakse üle salvestusruumi süsteemi vahel, ja siis, kui see on salvestatud. Järgmistes jaotistes kirjeldatakse andmete kaitse funktsioonid koos StorSimple.

> [AZURE.NOTE] Korduste eemaldamise pakub täiendavaid kaitse StorSimple seadme ja Microsoft Azure storage talletatud andmed. Kui andmed on deduplicated, objektide andmeid hoitakse eraldi saab vastendada ja neile juurde pääseda metaandmeid: talletusmahu taseme konteksti pole saadaval töötajatel helitugevuse struktuuri, failisüsteemis või faili nimi aluseks olevad andmed.

## <a name="protect-data-flowing-through-the-service"></a>Liigub läbi teenuse andmete kaitsmine

StorSimple halduri teenuse peamine eesmärk on hallata ja konfigureerida StorSimple seade. Microsoft Azure'i StorSimple halduri teenuse töötab. Azure'i klassikaline portaali abil seadme konfiguratsiooni andmete sisestamine ja Microsoft Azure'i kasutab StorSimple halduri teenuse andmete saatmiseks seade. StorSimple kasutab süsteemi asümmeetriline võtme paari tagada Azure teenuse kompromissi tulemuseks ei salvestatud teavet kompromissi. 

![Andmete krüptimine flight](./media/storsimple-security/DataEncryption.png)

Asümmeetriline süsteemi aitab kaitsta andmeid, mis liigub sujuvalt teenuse kaudu järgmiselt:

1. Andmete krüptimise sert, mida kasutatakse asümmeetriline avalike ja privaatvõ võti paari seadmes on loodud ja kasutatakse andmete kaitsmiseks. Klahvid luuakse siis, kui esimene seade on registreeritud. 
2. Andmete krüptimise serti klahvid eksporditakse isikuandmete vahetus (pfx) faili, mis on kaitstud teenuse andmete krüptovõtme, mis on tugev 128-bitise oluline CEIP loodud esimese seadme registreerimise käigus.
3. Avalik võti sert on kättesaadav turvaliselt StorSimple halduri teenusega ja privaatvõti jääb seade.
4. Andmete sisestamise teenus on krüptitud avalik võti ja lahtikrüptitud abil salvestatud seadme privaatvõti tagada, et Azure'i teenus ei saa dekrüptida andmed, mis on liiguvad seadmele.

Teenuse andmete krüptovõtme luuakse ainult esimese seadme registreeritud teenust. Edaspidised kõigis seadmetes, mis on registreeritud teenuse tuleb kasutada sama teenuse andmete krüptovõtme. 

> [AZURE.IMPORTANT] 
> 
> See on väga oluline teenuse andmete krüptovõtme koopia teha ja salvestage see turvalises asukohas. Teenuse andmete krüptovõtme koopia tuleb säilitada nii, et see pääseb volitatud isiku ja seadme administraator saab hõlpsasti edastada.
>
> Teenuse andmete krüptovõtme katkemisel Microsofti toe töötaja saate toomiseks, tingimusel, et teil on vähemalt üks seade online olekus. Soovitame teil muuta teenuse andmete krüptovõtme pärast tuuakse. Juhised leiate [teenuse andmete krüptovõtme muutmine](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

**Teenuse andmete krüptovõtme muutmine** valikut teenuse armatuurlaual saate muuta teenuse andmete krüptovõtme ja vastavaid andmeid krüptimise serti. Veenduge, et andmete turvalisus on rikutud pole, peate kasutama füüsilise StorSimple seadme krüptovõtme teenuse andmete muutmiseks. Muutmise krüptimise võtmed nõuab, et kõigis seadmetes värskendatakse uue tootenumbri. Soovitame, et muuta võti kõigis seadmetes võrgusoleku. Kui seade ühenduseta, saab muuta oma võtmed erineval ajal. Seadmete aegunud abil saab käivitada varukoopiate, kuid neid ei saa andmete taastamine, kuni võti on värskendatud. Lisateabe saamiseks lugege [teenuse StorSimple halduri armatuurlaual kasutamine](storsimple-service-dashboard.md).

Teenuse andmete krüptovõtme ja andmete krüptimise serti ei lõpe. Siiski, soovitame teil muuta teenuse andmete krüptovõtme aastas, et vältida privaatvõtme.

## <a name="protect-data-at-rest"></a>Ülejäänud andmete kaitsmine

StorSimple seadme haldab andmete panete astme kohalikult ja pilves, olenevalt sageduse kasutamise. Kõik host masinad, mis on ühendatud andmeid saata seade, mis liigub andmeid pilves, vastavalt vajadusele. Andmed on üle seadme pilveteenusesse turvaliselt Interneti kaudu. Igas seadmel ühe iSCSI target, mis toob esile kõik ühiskasutusega mahud seadmes. Kõik andmed on krüptitud enne saatmist Cloud salvestusruumi. 

![Pilveteenuse salvestusruumi krüptovõtme](./media/storsimple-security/CloudStorageEncryption.png)

Tagada ja selle teisaldada pilve andmete terviklus, StorSimple võimaldab määratleda pilveteenuse salvestusruumi krüptimise võtmed järgmiselt:

- Saate määrata pilvepõhise salvestusruumi krüptovõtme helitugevuse container loomisel. Võti ei saa muuta või hiljem lisada. 
- Kõik mahud helitugevuse ümbrises sama krüptovõtme ühiskasutusse anda. Kui soovite teatud maht krüptimise erineva vormi, soovitame luua uus helitugevuse ümbris, et maht majutada.
- Pilveteenuse salvestusruumi krüptovõtme sisestamisel StorSimple halduri teenuse võti on krüptitud avalik osa teenuse andmete krüptovõtme abil ning seejärel saadetakse seade.
- Pilveteenuse salvestusruumi krüptovõtme on talletatud suvalist kohta teenuses ja teada üksnes seade.
- Pilveteenuse salvestusruumi krüptovõtme määramine pole kohustuslik. Saate saata andmed, mis on krüptitud seadmele juures.

### <a name="additional-security-best-practices"></a>Suurema turvalisuse tagamiseks head tavad

- Tükeldatud liikluse: iSCSI SAN kasutaja liiklus ettevõtte LAN juurutamine täiesti eraldatud võrgu ja abil VLANs, kus füüsilise eraldamise ei ole võimalik eristada. Sihtotstarbeline võrgu iSCSI Storage tagab ohutus- ja oma ettevõtte jaoks kriitilise tähtsusega andmete jõudlust. Segamine salvestus- ja kasutajale liikluse üle ettevõtte LAN pole soovitatav, saate suurendada latentsus ja põhjustada tõrkeid võrgu.

- Host-side võrgu turbe tagamiseks kasutada võrgu liidesed, mis toetavad TCP/IP Mahalaetav Engine (TOE). TOE vähendab protsessorit töötlemine TCP võrguadapteri.

## <a name="protect-data-via-storage-accounts"></a>Salvestusruumi kontode kaudu andmete kaitsmine

Iga Microsoft Azure'i tellimus, saate luua ühe või mitme salvestusruumi kontod. (Salvestusruumi konto pakub kordumatut nimeruumi Azure'i pilves talletatud andmetega töötamiseks.) Salvestusruumi konto juurdepääsu kontrollib salvestusruumi kontoga seotud tellimust ja Accessi võtmed. 

Salvestusruumi konto loomisel Microsoft Azure'i loob kaks 512-bitine salvestusruumi kiirklahvide, millest üks on kasutada autentimiseks kui StorSimple seadme nimetus salvestusruumi konto. Pange tähele, et ainult üks neist klahvidest on kasutusel. Muu sisestage toimub reserve, mis võimaldab teil pööramiseks võtmed perioodiliselt. Klahvid pööramiseks teisese võtme aktiivseks ja seejärel kustutage primaarvõtme. Seejärel saate luua uue tootenumbri kasutamiseks järgmise pöörde ajal. (Turvalisuse põhjustel nõuavad palju andmekeskuste võtme pööre.) 

Soovitame, et järgite järgmisi olulisi pööre head tavad:

- Salvestusruumi konto klahvid regulaarselt, et tagada, et teie salvestusruumi konto on korrad volitamata tuleb muuta.
- Aeg-ajalt Azure administraator muuta või taastada põhi- või klahvi abil otse juurdepääsemiseks salvestusruumi konto salvestusruumi jaotise Azure klassikaline portaali.


## <a name="protect-data-via-encryption"></a>Kaitse andmete krüptimise teel

StorSimple kasutab järgmist krüptimise algoritmid talletatud andmete kaitsmiseks või reisil oma StorSimple lahenduse komponendid.

| Algoritmi | Võtme pikkus | Protokollid/rakenduste/kommentaarid |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | Konfiguratsiooni andmed, mis saadetakse seadme krüptimiseks kasutatakse Azure klassikaline portaali v1.5 RSA PKCS 1: näiteks salvestusruumi konto identimisteave StorSimple seadme konfiguratsiooni ja Pilveteenusest salvestusruumi krüptimise võtmed. |
| AES       | 256        | AES koos CBC kasutatakse krüptimiseks avalik osa teenuse andmete krüptovõtme enne saatmist Azure klassikaline portaali StorSimple seadme kaudu. Seda kasutatakse ka StorSimple seadme andmete krüptimiseks enne saatmist pilveteenuse salvestusruumi konto andmeid. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtuaalse seadme turvalisus

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Korduma kippuvad küsimused (KKK)

Järgnevalt mõned küsimused ja vastused turbe- ja Microsoft Azure'i StorSimple.

**Q:** Minu teenus on kahjustatud. Mida tuleks minu järgmised toimingud?

**A:** Teenuse andmete krüptovõtme ja salvestusruumi konto klahvid salvestusruumi konto, mida kasutatakse tiering andmete jaoks tuleks kohe muutmine. Juhiste saamiseks avage. 

- [Krüptovõtme teenuse andmete muutmine](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Võtme pöördenurka salvestusruumi kontod](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Mul on uus StorSimple seade, mis küsib teenuse registreerimise võti. Kuidas neid kuulata?

**A:** See võti on loodud StorSimple halduri teenuse esmakordsel loomisel. Ühenduse loomiseks seadme StorSimple halduri teenuse kasutamisel saate vaadata või taastada teenuse registreerimise võti Lühijuhend teenus. Uue teenuse registreerimise võtme genereerimine ei mõjuta olemasoleva registreeritud seadmed. Juhiste saamiseks avage.

- [Saate vaadata või taastada teenuse registreerimise võti](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Olen kaotanud minu andmed krüptovõtme. Mida teha?

**A:** Pöörduge Microsofti klienditoe poole. Saate sisse logimiseks tugi seansi oma seade ja aidata teil tuua võti (kui vähemalt üks seade on veebis). Kohe pärast teenuse andmete krüptovõtme saamiseks peaks muuta seda, et tagada, et uue tootenumbri on teada ainult teile. Juhiste saamiseks avage.

- [Krüptovõtme teenuse andmete muutmine](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Mõne seadme jaoks teenuse andmete krüptimise võtme muutmine lubatud, kuid ei käivitu protsessi võtme muuta. Mida teha?

**A:** Möödunud aja, pead reauthorize seadme jaoks teenuse andmete krüptimise võtme muutmine ja uuesti alustama.

**Q:**  Krüptovõtme teenuse andmed muutunud, kuid ma ei saanud värskendada muudes seadmetes 4 tunni jooksul. Kas ma pean uuesti alustada?

**A:** 4 tunni jooksul on ainult alustamist muutmine. Pärast värskenduse alustamiseks volitatud StorSimple seadme luba on lubatud kuni on värskendatud kõigis seadmetes.

**Q:** Meie StorSimple administraator on selle ettevõtte vasakule. Mida teha?

**A:** Muuta ja mis lubavad juurdepääsu StorSimple seadme paroolide lähtestamine ja muutke teenuse andmete krüptovõtme tagamaks, et uute andmetega ei ole volitamata töötajatele teada. Juhiste saamiseks avage.

- [StorSimple halduri teenuse abil saate muuta paroole storsimple](storsimple-change-passwords.md)
- [Krüptovõtme teenuse andmete muutmine](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Seadme StorSimple CHAP konfigureerimine](storsimple-configure-chap.md)

**Q:** Soovin anda host, mis ühendab StorSimple seadme StorSimple hetktõmmise halduri parooli, kuid parooli pole saadaval. Mida teha?

**A:** Kui olete parooli unustanud, peaksite looma uue. Klõpsake kindlasti kõik olemasolevatele kasutajatele teavitada, et parool on muutunud ja neid tuleks värskendada oma klientidele kasutamiseks uus parool. Juhiste saamiseks avage.

- [StorSimple hetktõmmise halduri parooli muutmine](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Seadme autentida](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Windows PowerShelli kaugjuurdepääs StorSimple sert on muudetud seadmes. Kuidas uuendada oma Kaugpöördusteenuse kliendid?

**A:** Saate alla laadida uut serti StorSimple halduri teenuse ja seejärel andke seda, et kliendid Kaugpöördusteenuse poes sert olema installitud. Juhiste saamiseks avage.

- [Cmdlet-käsu impordi-sert](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Minu andmetel on kaitstud kui StorSimple halduri teenuse on rikutud?

**A:** Teenuse konfiguratsiooni andmed on alati krüptitud oma avalik võti kuvatava veebibrauseris. Kuna teenus pole juurdepääsu privaatvõti, ei saa teenuse andmeid näha. Kui StorSimple halduri teenuse on rikutud, on mingit mõju, kui palju on talletatud StorSimple halduri teenuse võtmed.

**Q:** Kui keegi saab juurdepääsu andmete krüptimise serti, on minu andmetel on rikutud?

**A:** Microsoft Azure'i talletab krüptitud vormingus kliendi andmete krüptovõtme (pfx-fail). Kuna pfx-fail on krüptitud ja StorSimple teenus pole teenuse andmete krüptovõtme dekrüptida pfx-fail, lihtsalt saada juurdepääsu pfx-fail ei avalda mis tahes saladusi.

**Q:** Mis juhtub, kui valitsuse üksuse palub Microsoft oma andmete jaoks?

**A:** Kuna kõik andmed on krüptitud teenuse ja privaatvõti säilitatakse seadmega, valitsuse üksuse küsida andmeid kliendi. 

## <a name="next-steps"></a>Järgmised sammud

[Deploy StorSimple seadme](storsimple-deployment-walkthrough.md).
 
