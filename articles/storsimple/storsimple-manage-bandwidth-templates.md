<properties
   pageTitle="StorSimple läbilaskevõime mallide haldamine | Microsoft Azure'i"
   description="Kirjeldab, kuidas hallata StorSimple läbilaskevõime Mallid, mis võimaldavad juhtida läbilaskevõime kulu."
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
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple halduri teenuse abil StorSimple läbilaskevõime levirühmade haldamine

## <a name="overview"></a>Ülevaade

Läbilaskevõime Mallid võimaldavad teil konfigureerida võrgu läbilaskevõime kasutuse üle mitu korda päeva ajakava andmete StorSimple seadme pilveteenusesse tasemele.

Pidurdamise ajakava läbilaskevõimet saate teha järgmist.

- Saate määrata kohandatud läbilaskevõime ajakavasid sõltuvalt töökoormus võrgu tavadega.

- Koondada haldus ja uuesti kasutamine loendites mitmes seadmes lihtne ja sujuvalt viisil.

> [AZURE.NOTE] See funktsioon on saadaval ainult StorSimple füüsilise seadmete jaoks ja mitte virtuaalse seadmete jaoks.

Läbilaskevõime mallide teie teenuse jaoks kuvatakse tabelina ja sisaldavad järgmist teavet:

- **Nimi** – määratud läbilaskevõime malli, kui dokument on loodud kordumatu nimi.

- **Plaanimine** – ajakava läbilaskevõimet antud malli arv.

- **Kasutab** – väljendatud läbilaskevõime mallide kasutamine.

Saate lehe StorSimple halduri teenuse **konfigureerimine** Azure klassikaline portaalis läbilaskevõime levirühmade haldamine.

Leiate lisateavet konfigureerida läbilaskevõime Mallid:

- Küsimused ja vastused läbilaskevõime mallide kohta
- Head tavad läbilaskevõime Mallid

## <a name="add-a-bandwidth-template"></a>Läbilaskevõime malli lisamine

Järgmiste toimingute läbilaskevõime uue malli loomine.

#### <a name="to-add-a-bandwidth-template"></a>Läbilaskevõime malli lisamine

1. Klõpsake lehe StorSimple halduri teenuse **konfigureerimine** **lisamine või redigeerimine läbilaskevõime malli**.

2. Dialoogiboksis **Malli lisamine/redigeerimine läbilaskevõime** järgmist.

   1. Valige ripploendist **malli** **Loo uus** läbilaskevõime uue malli lisamine.
   2. Määrake malli läbilaskevõime kordumatu nimi.

3. Määratleda **ajakava läbilaskevõimet**. Ajakava loomiseks tehke järgmist.

   1. Valige ripploendist, päeva nädala ajakava on konfigureeritud lubama. Saate valida mitu päeva, valides loendist vastav päeva enne ruudud.
   2. Valige suvand **Kogu päeva** , kui ajakava täidetakse kogu päeva. Kui see suvand on märgitud, saate enam **Alguskellaaeg** või **Lõpuaeg**on. Ajasta algab 12:00 AM 11:59 PM.
   3. Valige ripploendist, **Alguskellaaeg**. See on kui alustab ajakava.
   4. Valige ripploendist, on **Lõppkellaaeg**. See on kui katkestab ajakava.

         > [AZURE.NOTE] Kattuvate ajakava pole lubatud. Kui algus- ja lõpuaegade tulemuseks kattuva graafiku, kuvatakse tõrketeade selle kohta.

   5. Määrake **läbilaskevõime määr**. See on selle läbilaskevõime megabitti sekundis (MB) kasutavad StorSimple seadet toimingutega pilveteenuses (nii üles- ja allalaadimised). Lisage arv vahemikus 1 kuni 1000 selle välja jaoks.

   6. Klõpsake ikooni Otsi ![sisse ikooni](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Mall, millele olete loonud lisatakse lehe service **konfigureerimine** läbilaskevõime mallide loendi.

    ![Läbilaskevõime uue malli loomine](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Klõpsake lehe allosas **salvestada** ja seejärel nuppu **Jah** kinnituse küsimisel. See säästab konfiguratsioonimuudatuste, et olete teinud.

## <a name="edit-a-bandwidth-template"></a>Läbilaskevõime malli redigeerimine

Järgmiste toimingute redigeerimiseks läbilaskevõime malli.

### <a name="to-edit-a-bandwidth-template"></a>Kui soovite läbilaskevõime malli redigeerimine

1. Klõpsake **malli lisamine või redigeerimine läbilaskevõime**.

2. Dialoogiboksis **Malli lisamine/redigeerimine läbilaskevõime** järgmist.

   1. Valige ripploendist **malli** olemasoleva läbilaskevõime malli, mida soovite muuta.
   2. Tehke soovitud muudatused. (Saate muuta olemasolevaid sätteid.)
   3. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Kuvatakse muudetud malli läbilaskevõime mallide loendi lehe service konfigureerimine.

3. Muudatuste salvestamiseks klõpsake lehe allosas **salvestada** . Klõpsake nuppu **Jah** kinnituse küsimisel.

> [AZURE.NOTE] Teil pole lubatud muudatuste salvestamiseks, kui redigeeritud ajakava kattub olemasoleva ajakava läbilaskevõimet malli, mida muudate.

## <a name="delete-a-bandwidth-template"></a>Läbilaskevõime malli kustutamine

Järgmiste toimingute läbilaskevõime malli kustutamine.

#### <a name="to-delete-a-bandwidth-template"></a>Läbilaskevõime malli kustutamine

1. Tabeli loendit teenust läbilaskevõime Mallid, valige mall, mida soovite kustutada. Äärmiselt paremal valitud mall kuvatakse ikooni Kustuta (**x**). Klõpsake kustutatava malli ikooni **x** .

2. Teil palutakse kinnitust. Jätkamiseks klõpsake nuppu **OK** .

Kui mall on mis tahes draivid kasutavad, mitte lubatakse teil selle kustutada. Näete teadet, et mall on kasutusel. Sõnumi dialoogiboks kuvatakse soovitab teie malli viiteid kõrvaldamisel.

Saate kustutada kogu viiteid malli juurdepääs **Helitugevuse ümbriste** lehe ja muutke helitugevuse ümbriste, mis seda malli kasutada nii, et nad kasutavad teise malli või kohandatud või määramata läbilaskevõime sätet kasutavad. Kui kõik viited on eemaldatud, saate kustutada mall.

## <a name="use-a-default-bandwidth-template"></a>Läbilaskevõime vaikemalli kasutamiseks

Vaikemalli läbilaskevõime on esitatud ja kasutatakse helitugevuse ümbriste vaikimisi jõustamiseks läbilaskevõime juhtelementide kasutamisel pilve. Vaikemalli ühtlasi kasutajatele, kes luua oma malle valmis viitena. See vaikemalli üksikasjad on:

- **Nimi** – piiramatu ööks ja nädalavahetustel

- **Plaanimine** – ühe ajakava esmaspäevast reedeni, mis kehtib läbilaskevõime määra 1 MB/s 00-5 PL seadme kellaaeg. Nädala ülejäänud seatakse piiramatu ribalaiust.

Vaikemalli saab redigeerida. Selle malli (sh redigeeritud versioonid) kasutamist jälitatakse.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Algavad määratud aja jooksul kogu päeva läbilaskevõime malli loomine

Järgige luua ajakava, mis algab määratud ajal ja töötab kogu päeva. Näites Ajasta käivitab hommikul kell 9 ja kestab kuni 9: 00 järgmisel hommikul. See on oluline märkida, mis antud ajakava jaoks algus- ja lõpuaegade peavad mõlemad sisaldama sama 24-tunnise ajakava ja ei saa ühendada mitu päeva. Kui teil on vaja seadistada läbilaskevõime malle, mitu päeva ulatuvad, peate kasutada mitu ajakava (nagu on näidatud).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Kogu päeva läbilaskevõime malli loomiseks

1. Saate luua ajakava, mis algab hommikul kell 9 ja kestab kuni keskööst.

2. Saate lisada mõne muu ajakava. Konfigureerige teise ajakava käivitamiseks keskööst kuni hommikul 9 kohta.

3. Salvestage läbilaskevõime mall.

Kombineeritud ajakava siis oma valimise ajal käivitub ja käivitada kogu päeva.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Küsimused ja vastused läbilaskevõime mallide kohta

**Q**. Mis juhtub, kui teil on vahepeal loendites läbilaskevõime juhtelemendid? (Ajakava pole enam kasutuses ja teine pole veel alanud.)

**A**. Sel juhul pole läbilaskevõime juhtelementide töötab. See tähendab, et seade saate andmeid pilv tiering piiramatu ribalaius.

**Q**. Te saate muuta läbilaskevõime mallide ühenduseta seadmes

**A**. Te ei saa muuta mahud ümbriste läbilaskevõime malle, kui vastav seade on ühenduseta.

**Q**. Saate seotud helitugevuse container kui seotud mahud on ühenduseta ribalaiuse malli?

**A**. Saate muuta seostatud helitugevuse ümbris, mille maht on ühenduseta ribalaiuse malli. Pange tähele, et, kui maht on ühenduseta, andmed kuvatakse sõltuvad seadmest pilveteenusesse.

**Q**. Kas te vaikemalli kustutada?

**A**. Kuigi saate kustutada vaikemalli, ei ole mõistlik teha. Jälitatakse vaikemalli, sh redigeeritud versioonide kasutamist. Jälgimise andmed on analüüsida ja aja jooksul kasutatakse parandamiseks vaikemalli.

**Q**. Kuidas määrata, et teie läbilaskevõime Mallid on vaja muuta?

**A**. Üks märke, peate läbilaskevõime mallide muutmine on käivitamisel kuvatakse võrgu aeglustada või õhuklapp mitu korda päevas sisse. Sel juhul jälgida i/o jõudlus ja võrgu läbilaskevõime diagrammide vaadates võrgu salvestusruumi ja kasutamist.

Võrgu läbilaskevõime andmete tuvastamine kellaaeg ja helitugevuse ümbriste, ilmneb võrgu kitsaskohaks. Kui see juhtub, kui andmed on on mitmetasandilise pilveteenusesse (saada see teave I/O jõudlust kõik helitugevuse ümbriste seadme Cloud), siis peate oma helitugevuse ümbriste seostatud läbilaskevõime mallide muutmine.

Pärast seda, kui kasutusel on muudetud malle, peate võrgu uuesti märgatavalt latentsused jälgimine. Kui need on endiselt olemas, siis pead uuesti oma läbilaskevõime mallid.

**Q**. Mis juhtub, kui mitu helitugevuse ümbriste oma seadmes on koosolekut kavandava et kattuvad, kuid erinevad piirangud kehtivad kõigi?

**A**. Oletame, et teil on 3 helitugevuse ümbrisega seade. Ajakava, mis on seotud nende ümbriste täielikult kattuvad. Iga nende ümbriste, läbilaskevõime piirangud, mida kasutatakse on 5, 10 ja 15 Mbps vastavalt. Kui väljundtoimingute toimuvad kõik need ümbriste samal ajal, miinimum 3 läbilaskevõime piirangute võib rakendatud: sel juhul 5 Mbps, sest need väljaminevat I/O taotlusi ühiskasutus samas järjekorras.

## <a name="best-practices-for-bandwidth-templates"></a>Head tavad läbilaskevõime Mallid

Järgige neid StorSimple seadme head tavad:

- Seadme lubamiseks muutuv ahendamine võrgu läbilaskevõime seadme erineval päeva konfigureerimine läbilaskevõime mallid. Need läbilaskevõime Mallid kasutamisel koos varukoopia ajakavasid tõhus saate kasutada täiendavaid võrgu läbilaskevõime cloud toimingute ajal aegsasti.

- Tegelik suurus ja juurutamise vajaliku aja eesmärk (RTO) põhjal kindla juurutamiseks nõutav ribalaius arvutada.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
