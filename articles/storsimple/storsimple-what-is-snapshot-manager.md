<properties 
   pageTitle="Mis on StorSimple hetktõmmise haldur? | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple hetktõmmise juhataja, selle arhitektuur ja selle funktsioonid."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="what-is-storsimple-snapshot-manager"></a>Mis on StorSimple hetktõmmise haldur?

## <a name="overview"></a>Ülevaade

StorSimple hetktõmmise Manager on Microsoft Management Console (MMC) lisandmoodul, mis lihtsustab andmekaitse ja varukoopia juhtimine Microsoft Azure'i StorSimple keskkonnas. StorSimple hetktõmmise halduriga, saate hallata Microsoft Azure'i StorSimple andmed ja pilveteenuste andmekeskuse lahendusena ühe integreeritud salvestusruumi seega lihtsustamine varukoopia protsesside ja vähendada.

See ülevaade tutvustab StorSimple hetktõmmise ülemus, selle funktsioonid ja selgitatakse Microsoft Azure'i StorSimple roll. 

Kogu Microsoft Azure'i StorSimple süsteemi, sh StorSimple seade, ülevaate StorSimple halduri teenuse, StorSimple hetktõmmise halduri ja StorSimple adapterit SharePointi, leiate [StorSimple 8000 sarja: hübriid pilve salvestusruumi lahendus](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- StorSimple hetktõmmise halduri abil ei saa hallata Microsoft Azure'i StorSimple virtuaalse massiivi (tuntud ka kui StorSimple kohapealse virtuaalne seadmed).
>
>- Kui plaanite installida StorSimple 2 StorSimple seadmes, kindlasti uusim versioon StorSimple hetktõmmise halduri alla laadima ja installima **enne installimist StorSimple 2**. Uusima versiooni StorSimple hetktõmmise Manager on tagasiühilduvad ja töötab koos kõigi avaldatud versioonid Microsoft Azure'i StorSimple. Kui kasutate eelmise versiooni StorSimple hetktõmmise Manager, peate värskendage seda (teil pole vaja desinstallida enne uue versiooni installimist eelmise versiooni).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple hetktõmmise halduri otstarve ja arhitektuur

StorSimple hetktõmmise Manager pakub keskse halduskonsooli, mille abil saate luua süsteemset, punkti /-kellaaja varukoopiaid kohalik ja pilveteenuse andmed. Näiteks saate konsooli:

- Konfigureerimine, varundamine ja mahud kustutamine.
- Helitugevuse rühmi, et tagada konfigureerimine varundatud andmete on rakenduse ühtsete.
- Andmete varundamise lülitame ajakava varukoopia poliitikate haldamine
- Luua kohaliku ja pilte, mida saab pilveteenuses ja kasutatakse avariitaastet cloud.

StorSimple hetktõmmise ülemus toob registreeritud VSS pakkuja Host rakenduste loendit. Klõpsake rakenduse ühtsete varukoopiate loomiseks kasutatud rakenduse mahud kontrollib ja soovitab helitugevuse rühmade konfigureerimine. StorSimple hetktõmmise halduri kasutab neid helitugevuse rühmade varukoopiaid, mis on rakenduse süsteemset loomiseks. (Rakenduse järjepidevuse olemas, kui kõik seostuvad failid ja andmebaasid on sünkroonitud ja tähistada rakenduse teatud punktis ajas tõene olek.) 

StorSimple hetktõmmise halduri varukoopiate vormis jäädvustada ainult muudatused pärast viimast varundamist suureneva hetktõmmiseid luua. Selle tulemusena varukoopiate nii palju salvestusruumi vaja ja saab luua ja kiiresti taastada. StorSimple hetktõmmise halduri kasutab Windows helitugevuse vari Copy Service (VSS) tagamaks, et hetktõmmiseid rakenduse ühtsete andmete talletamiseks. (Lisateabe saamiseks minge Windows draivi vari teenus jaotise integreerimine.) StorSimple hetktõmmise halduriga, saate luua varukoopia ajakava või teha kohe varukoopiaid vastavalt vajadusele. Kui teil on vaja andmete taastamine varukoopia, StorSimple hetktõmmise halduri abil saate valida kohalik kataloogiga või pilve pilte. Azure'i StorSimple taastab ainult andmeid, mis on vaja vajadusel, mis välistab viivitused andmete kättesaadavus ajal Taasta toimingud).

![StorSimple hetktõmmise halduri arhitektuur](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple hetktõmmise halduri arhitektuur** 

## <a name="support-for-multiple-volume-types"></a>Helitugevuse mitut tüüpi tugi

Saate konfigureerida ja varundamiseks kogused järgmist tüüpi StorSimple hetktõmmise ülemus: 

- **Põhilised mahud** -lihtsa helitugevus on ühe sektsioon lihtsa kettale. 

- **Lihtne mahud** -lihtne draiv on dünaamiline maht, mis sisaldab ühe dünaamilise kettalt kettaruumi. Lihtne draiv koosneb ühe piirkonna kettale või mitme regioonid, mis on omavahel seotud samale kettale. (Saate luua lihtsa mahud ainult dünaamiline ketast.) Lihtne maht ei ole salliv viga.

- **Dünaamilised draivid** -dünaamiline helitugevus on loodud dünaamiline ketas maht. Dünaamiliste ketast kasutada andmebaasi maht, mis on esitatud teavet dünaamiline kettale arvuti. 

- **Dünaamilised draivid koos peegeldus** -dünaamiline mahud koos peegeldus on ehitatud RAID 1 arhitektuur. RAID 1, identse andmed on kirjutatud kahe või enama kettal, toodavad peegelpilt kogum. Lugemise taotluse saab käsitleda, mis tahes ketas, mis sisaldab soovitud andmed.

- **Kobar jagatud mahud** – kobar-jagatud mahud (CSV-de kaitse), mitu sõlmed tõrkesiirdeklastrite saab korraga lugeda või kirjutada samale kettale. Tõrkesiirde ühe sõlme teise sõlme võivad tekkida kiiresti, ilma ketas omandiõiguse või paigaldus, demontaaž ja eemaldamise maht muuta. 

>[AZURE.IMPORTANT] Mitte segada CSV-de kaitse ja mitte-CSV-de kaitse sama hetktõmmis. CSV-de kaitse ja mitte-CSV-de kaitse sisse hetktõmmise ei toetata. 
 
StorSimple hetktõmmise halduri abil saate taastada kogu rühmade või klooni üksikud mahud ja taastada üksikuid faile.

- [Maht- ja helitugevuse rühmad](#volumes-and-volume-groups) 
- [Varukoopia tüübid ja varukoopia poliitika](#backup-types-and-backup-policies) 

StorSimple hetktõmmise halduri funktsioonide ja nende kasutamise kohta leiate lisateavet teemast [StorSimple hetktõmmise halduri kasutajaliides](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Maht- ja helitugevuse rühmad

StorSimple hetktõmmise halduriga, loote mahud ja konfigureerida neid helitugevuse rühmadesse. 

StorSimple hetktõmmise halduri kasutab helitugevuse rühmad luua varukoopiaid, mis on rakenduse süsteemset. Rakenduse järjepidevuse olemas, kui kõik seostuvad failid ja andmebaasid on sünkroonitud ja tähistada rakenduse teatud punktis ajas tõene olek. Helitugevuse rühmad (mis on ka *järjepidevuse rühmad*) aluseks on varukoopia või taastada töö.

Helitugevuse rühmad pole sama, mis helitugevuse ümbriste. Helitugevuse container sisaldab ühiskasutusse anda pilveteenuses salvestusruumi konto ja muud atribuudid, näiteks krüptimise ja läbilaskevõime kulu draivid. Ühe helitugevuse container võib sisaldada kuni 256 õhukesed ettevalmistatud StorSimple draividel. Helitugevuse ümbriste kohta lisateabe saamiseks minge [oma helitugevuse ümbriste haldamine](storsimple-manage-volume-containers.md). Helitugevuse rühmad on maht, mida saate konfigureerida, et hõlbustada varukoopia toimingute kogumid. Kui valite kahest osast, mis kuuluvad uus draiv ümbriste, paigutada ühte rühma ja seejärel looge selle rühma helitugevuse varukoopia poliitika, iga maht varundatakse maht ümbrises, hoidmise kontot kasutades.

>[AZURE.NOTE] Helitugevuse rühma kõik maht peab pärinema ühe pilvepõhise teenuse pakkujalt.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Windowsi draivi vari teenus integreerimine

StorSimple hetktõmmise halduri kasutab Windows helitugevuse vari Copy Service (VSS) rakenduse ühtsete andmete talletamiseks. VSS hõlbustab rakenduse järjepidevuse suhtlemine VSS-rakendusi koordineerimiseks suureneva hetktõmmiseid luua. VSS tagab rakenduste ajutiselt passiivsed või üle vaadata, kui on tehtud pilte. 

StorSimple hetktõmmise halduri rakendamise VSS töötab SQL Server ja üldise NTFS maht. Protsess on järgmine: 

1. Lisamine, mis on tavaliselt andmete haldamine ja kaitse lahendus (nt StorSimple hetktõmmise Manager) või varunduse rakendus, käivitab VSS ja palub selle kirjutaja tarkvara sihtrakenduses teabe kogumine.

2. VSS kontaktide kirjutaja komponent kirjeldus andmete toomiseks. Kirjutaja tagastab andmeid varundada kirjeldust. 

3. VSS signaale kirjutaja ettevalmistamiseks rakenduse varundamiseks. Kirjutaja valmistab andmeid varundamise avatud tehingud, täites värskendamine tehingute registrid jne, ja seejärel teavitab rakendust

4. VSS juhendab kirjutaja ajutiselt peatada rakenduse andmete poed ja veenduge, et andmed kirjutada maht ajal vari koopia luuakse. Selles etapis tuleb andmete vastavust ja võtab rohkem kui 60 sekundi järel.

5. VSS juhendab pakkuja vari koopia loomiseks. Pakkujad, mis võib olla tarkvara või riistvara-põhise, hallata mahud, mis on praegu töötavate ja luua vari koopiad neid vajadusel. Pakkuja loob varju Kopeeri ja teavitab VSS, kui see on lõpule viidud.

6. VSS kontaktide kirjutaja teavitamise I/O jätkamiseks rakendus ja kinnitage I/O katkestatud edukalt vari koopia loomise ajal. 

7. Kui Kopeeri õnnestus, tagastab VSS imperatiivsus päringu funktsiooni Kopeeri asukohta. 

8. Kui andmed on kirjutatud ajal varju Kopeeri on loodud, siis varundamine on vastuolus. VSS kustutab varju Kopeeri ja teavitab imperatiivsus päringu. Saate imperatiivsus päringu korrake varundamist automaatselt või teavitamise administraator selle hiljem uuesti proovida.

Järgmisel joonisel näha.

![VSS protsess](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windowsi draivi vari teenus protsess** 

## <a name="backup-types-and-backup-policies"></a>Varukoopia tüübid ja varukoopia poliitika

StorSimple hetktõmmise halduriga, saate andmete varundamine ja salvestada selle kohalikult ja pilveteenuses. Abil saate StorSimple hetktõmmise halduri andmete varundamine kohe või varukoopia poliitika abil saate luua ajakava tegemine varukoopiate automaatselt. Varukoopia poliitikate ka lubada – saate määrata, kui palju hetktõmmiseid säilitatakse. 

### <a name="backup-types"></a>Varukoopia tüübid

StorSimple hetktõmmise halduri abil saate luua varukoopiate järgmist tüüpi:

- **Hetktõmmiseid kohalik** – kohaliku hetktõmmiseid on helitugevuse andmeid, mis on talletatud StorSimple seadme punkti /-kellaaja koopiad. Tavaliselt saab seda tüüpi varukoopia luua ja kiiresti taastada. Kohaliku hetktõmmise saate kasutada nii, nagu teeksite kohaliku varukoopia.

- **Pilveteenuse hetktõmmiseid** – pilveteenuses hetktõmmiseid on punkti /-kellaaja koopiad helitugevuse andmed, mis talletatakse pilves. Pilveteenuse hetktõmmise võrdub hetktõmmise kopeeritud erinev, väljapoole salvestusruumi süsteem. Pilveteenuse hetktõmmiseid on eriti kasulik Avariijärgne taaste stsenaariumid.

### <a name="on-demand-and-scheduled-backups"></a>Nõudmisel ja ajastatud varukoopiaid

StorSimple hetktõmmise halduriga, saate algatada ühekordse varukoopia luua kohe või varukoopia poliitika abil saate ajastada korduva varukoopia toimingud.

Varukoopia poliitika on automatiseeritud reeglite, mille abil saate ajastada korrapäraste varukoopiate plaanimine. Varukoopia poliitika võimaldab määratleda sagedus ja parameetrite võtmise hetktõmmiste köite rühma. Poliitika abil saate määrata algus- ja aegumise kuupäevade, kellaaegade, sagedus ning säilitamise nõuded nii kohalikud ja Pilveteenusest hetktõmmiseid luua. Poliitika rakendatakse kohe pärast selle määratlete. 

Saate konfigureerida või taaskonfigureerimine varukoopia poliitikate vajaduse korral StorSimple hetktõmmise haldur. 

Saate konfigureerida iga varukoopia poliitika, mille loote järgmist teavet:

- **Nimi** – valitud varukoopia poliitika kordumatu nimi.

- **Tippige** -tüüpi varukoopia poliitika; kohaliku hetktõmmise või pilveteenuse hetktõmmis.

- **Helitugevuse rühma** – valitud varukoopia poliitika on määratud köite rühma.

- **Säilitus** -säilitamise varukoopia eksemplaride arv. Kui **Kõik** ruut säilitatakse kõik varukoopiaid kuni varukoopiaid ruumala maksimaalne arv, mida poliitika nurjuda ja kuvatakse tõrketeade. Teise võimalusena saate määrata mitmesuguseid varukoopiate säilitamise (vahemikus 1 kuni 64).

- **Kuupäev** – varukoopia poliitika loomise kuupäev.

Minge varukoopia poliitikate konfigureerimise kohta leiate [Kasutamine StorSimple hetktõmmise halduri loomise ja haldamise poliitikate varukoopia](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Varundustöö jälgimise ja halduse

Saate jälgida ja hallata eelseisvad, ajastatud ja lõplikus varukoopia StorSimple hetktõmmise ülemus. Lisaks StorSimple hetktõmmise Manager pakub kuni 64 lõplikus varukoopiate kataloogiga. Kataloogi abil saate mahud või üksikuid faile otsimiseks ja taastamiseks. 

Minge jälgimise varundamise kohta leiate teavet [Kasutamine StorSimple hetktõmmise halduri vaadata ja hallata varundamise](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Järgmised sammud

- Lisateave [halduris StorSimple hetktõmmise hallata oma StorSimple lahendus](storsimple-snapshot-manager-admin.md).

- Laadige alla [StorSimple hetktõmmise haldur](https://www.microsoft.com/download/details.aspx?id=44220).
