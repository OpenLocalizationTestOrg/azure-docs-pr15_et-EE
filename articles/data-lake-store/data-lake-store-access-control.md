<properties
   pageTitle="Ülevaade andmete Lake poes juurdepääsu reguleerimine | Microsoft Azure'i"
   description="Mõista, kuidas juurdepääsu Azure Lake poes kontroll"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Azure'i Lake poes juurdepääsu reguleerimine

Lake andmesalve rakendab juurdepääsu juhtimine mudelit, mis tuleneb HDFS, mis omakorda POSIX juurdepääsu juhtimine mudelit. Selles artiklis on toodud juurdepääsu juhtimine mudeli andmete Lake poe põhitõdesid. Lisateavet selle HDFS juurdepääsu juhtimine mudelit leiate [HDFS õiguste juhend](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Failide ja kaustade pääsuloendid

On kahte tüüpi Acess juhtelemendi loendid (ACL) – **Juurdepääs ACL-ID** ja **Vaikimisi ACL-ID**.

* **Accessi ACL** – need juhtelemendi juurdepääsu objekti. Failide ja kaustade on juurdepääs ACL-ID.

* **Vaikimisi ACL** – "Mall" ACL kausta, mis määravad kõik selles kaustas loodud tütarüksused Accessi ACL-ID-ga seostatud. Faile ei saa vaikimisi ACL-ID.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Nii Accessi ACL-ID ja vaikimisi ACL-ID on sama struktuuri.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Klõpsake emaüksus vaikimisi ACL muutmine ei mõjuta juurdepääsu ACL või vaikimisi ACL lapse üksused, mis on juba olemas.

## <a name="users-and-identities"></a>Kasutajate ja identiteedid

Iga failide ja kaustade on erinevate õiguste need identiteedid.

* Kes omavad kasutaja faili
* Kes omavad rühma
* Nimega kasutajad
* Nimega rühmad
* Ülejäänud kasutajad

Kasutajate ja rühmade identiteedid on Azure Active Directory (AAD) identiteedid nii märgitud teisiti "kasutaja", Lake andmesalve kontekstis ühte võib mõni AAD kasutaja või turberühma AAD.

## <a name="permissions"></a>Õigused

Failisüsteemi objekti õiguste on **lugemine** **kirjutamine**, **käivitamine** ja neid saab kasutada faile ja kaustu, nagu on näidatud järgmises tabelis.

|            |    Faili     |   Kausta |
|------------|-------------|----------|
| **Read (R)** | Saate lugeda faili sisu | Nõuab **lugemis** - **Execute** loendis kausta sisu.|
| **Kirjutage (W)** | Saate kirjutada või faili lisamine | Nõuab **kirjutamine ja käivitada** loomiseks lapse üksused kausta. |
| **Käivitada (X)** | Ei tähenda midagi Lake andmesalve kontekstis | Nõutav läbida tütarüksused kausta. |

### <a name="short-forms-for-permissions"></a>Lühike vormide õiguste

**RWX**kasutatakse **lugeda + kirjutamine + käivitada**. Veel tihendatud arvuline vorm olemas, kus **lugemine = 4**, **kirjutamine = 2**, ja **käivitamine = 1** ja nende summa tähistab õigused. Allpool on mõned näited.

| Arvuline vorm | Lühike vorm |      Mida see tähendab?     |
|--------------|------------|------------------------|
| 7            | RWX        | Lugege + kirjutamine + käivitada |
| 5            | R-X        | Lugege + käivitada         |
| 4            | R-        | Lugemine                   |
| 0            | ---        | Õigusi pole         |


### <a name="permissions-do-not-inherit"></a>Päri õigused

POSIX laadis mudelis kasutatavad Lake andmesalve talletatakse üksuse üksuse õigused. Teisisõnu, ei saa ema üksuste päritud õiguste üksuse.

## <a name="common-scenarios-related-to-permissions"></a>Levinumad stsenaariumid, mis on seotud õiguste

Siit leiate mõne levinud stsenaariumi, et aru saada, milliseid õigusi on vaja teatud toiminguid Lake andmesalve kontol.

### <a name="permissions-needed-to-read-a-file"></a>Faili lugemiseks vajalikud õigused

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Faili lugeda - funktsiooni helistaja peab **Loe** õigusi
* Kõigi kaustade struktuuris kausta, mis sisaldavad faili - funktsiooni helistaja peab **Execute** õigused

### <a name="permissions-needed-to-append-to-a-file"></a>Faili lisamiseks vajalikud õigused

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Lisatavatest - faili funktsiooni helistaja peab **kirjutamine** õigused
* Kõik kaustad, mis sisaldavad faili - funktsiooni helistaja peab **Execute** õigused

### <a name="permissions-needed-to-delete-a-file"></a>Faili kustutamine vajalikud õigused

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Emakausta - funktsiooni helistaja peab **kirjutamine + käivitada** õigused
* Kõigi muude kaustade failitee - funktsiooni helistaja peab **Execute** õigused

>[AZURE.NOTE] Kirjutage faili õiguste ei pea faili kustutada, kui ülaltoodud kaks tingimust on täidetud.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Kausta vajalikud õigused

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Kausta loendada - funktsiooni helistaja peab **lugemine + Execute** õigused
* Esivanem kaustade - funktsiooni helistaja peab **Execute** õigused

## <a name="viewing-permissions-in-the-azure-portal"></a>Azure'i portaalis õiguste vaatamine

Lake andmesalve konto **Andmete Explorer** tera, klõpsake nuppu **Accessi** ACL faili või kausta kuvamiseks. Klõpsake pildil Accessi ACL **mydatastore** konto **kataloogi** kausta kuvamiseks.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Pärast seda, klõpsake keelest **Accessi** **Lihtsas vaates** lihtsam kuvada.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klõpsake nuppu **Täpsem vaade** keerukamaks kuvada.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Funktsiooni administraatori

Administraatori lisamine on kõige kõigi kasutajate õigusi Lake andmesalve. Administraatori:

* **kõikide** failide ja kaustade on RWX õigused
* Saate muuta õigusi, mis tahes faili või kausta.
* Saate muuta seda, kes omavad kasutaja või mis tahes faili või kausta, kes omavad rühm.

Azure, Lake andmesalve konto on mitu Azure rollid.

* Omanikud
* Osaliste
* Lugejad
* Jne.

Kõik **omanikud** roll Lake andmesalve konto on automaatselt superkasutaja konto jaoks. Lisateavet kohta Azure rolli vastavalt juurdepääsu juhtimine (RBAC) leiate [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>Kes omavad kasutaja

Kasutaja loodud üksus on automaatselt üksusest, kes omavad kasutaja. Kes omavad kasutaja saab:

* Faili, mis on õiguste muutmine
* Kui kes omavad kasutaja on ka sihtrühma liikme saate muuta faili, mis kuulub, kes omavad rühma.

>[AZURE.NOTE] Kes omavad kasutaja teisele kuuluva faili kes omavad kasutaja **ei saa** muuta. Ainult Super-kasutajad saavad muuta faili või kausta, kes omavad kasutaja.

## <a name="the-owning-group"></a>Kes omavad rühma

POSIX ACL-ID igale kasutajale on seostatud "esmane rühma". Näiteks kasutaja "alice" võib privaatsusrühma "finance". Alice võib kuuluvad mitu rühma, kuid üks rühm on alati määratud tema esmase rühma. POSIX, kui Alice loob faili, kes omavad rühma faili on seatud tema esmase rühma, mis sel juhul on "finants".
 
Failisüsteemi uue üksuse loomisel määrab Lake andmesalve kes omavad rühma väärtus. 

* **Juhul 1** - juurkausta "/". Selle kausta loomist Lake andmesalve konto loomisel. Sel juhul on seatud kes omavad jaotises konto loonud kasutaja.
* **Juhul 2** (kõik muud kast) – uue üksuse loomisel, kes omavad rühma kopeeritakse emakausta.

Kes omavad rühma saab muuta:
* Mis tahes Super-kasutajad
* Kes omavad kasutaja, kes omavad kasutaja on ka target rühma liige.

## <a name="access-check-algorithm"></a>Accessi sisse algoritmi

Järgmisel joonisel tähistab juurdepääsu sisse algoritmi Lake andmesalve kontode jaoks.

![Algoritmi Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Mask ja "tõhusa õiguste"

**Maski** on RWX väärtus, mida kasutatakse **nimega kasutaja**, **rühma omamise**ja **rühma nimega** juurdepääsu piirata juurdepääsu kontrollimine algoritmi täites. Siin on põhimõtteid mask jaoks. 

* Mask loob "tõhusa õiguste", st muudab õiguste ajal juurdepääsu kontrollimine.
* Mask saate otse redigeerida faili omanik ja mis tahes Super-kasutajad.
* Mask on võimalus luua tõhus õiguste õiguste eemaldamine. Õiguste maski **ei saa** lisada kehtivaid õigusi. 

Vaatame mõned näited. Allpool on seatud mask **RWX**, mis tähendab, et mask ei saa eemaldada kõik õigused. Pange tähele, et tõhusa õiguste nimega kasutaja, kes omavad rühma ja rühma nimega ei mõjuta ajal juurdepääsu sisse.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Järgmises näites on seatud mask **R-X**. Jah, see **lülitatakse välja kirjutusõiguse** **nimega kasutaja**, **rühma omamise**ja **rühma nimega** Accessi ajal kontrollida.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Viide, siit leiate Azure'i portaalis mask faili või kausta kuvamiskoha.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Uue Lake andmesalve konto, vaikimisi mask juurdepääsu ACL ja vaikimisi ACL juurkausta ("/"), kuni RWX.

## <a name="permissions-on-new-files-and-folders"></a>Uute failide ja kaustade õiguste

Kui olemasoleva kausta all on loodud uue faili või kausta, klõpsake emakausta ACL vaikimisi määrab:

* Lapse kausta vaikimisi ACL ja juurdepääsu ACL
* Lapse faili juurdepääsu ACL (failid on vaikimisi ACL)

### <a name="a-child-file-or-folders-access-acl"></a>Lapse faili või kausta juurdepääsu ACL

Lapse faili või kausta loomisel kopeeritakse vanema vaikimisi ACL lapse faili või kausta juurdepääsu ACL. Ka juhul, kui **teise** kasutaja on vanema vaikimisi ACL RWX õigused, see on eemaldatakse lapse üksuse juurdepääsu ACL.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Enamikul juhtudel ülaltoodud teave on kõik, mida tuleks vaja teada, kuidas lapse üksuse juurdepääsu ACL määratakse. Siiski kui tuttavaid POSIX süsteemid ja soovite põhjalikumat mõista, kuidas see teisendus saavutamiseni, jaotisest [Umask isiku rolli juurdepääsu ACL uusi faile ja kaustu luua](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) selle artikli.
 

### <a name="a-child-folders-default-acl"></a>Lapse kausta vaikimisi ACL

Jaotises emakaustalt alamkaustas loomisel kopeeritakse vanem kausta vaikimisi ACL üle, kui see on lapse kausta vaikimisi ACL.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Täpsemad Teemad ACL andmete Lake poes mõistmine

Järgnevalt on paar täpsemalt teemade aidata teil mõista, kuidas ACL määratakse Lake andmesalve faile või kaustu.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Umask's roll juurdepääsu ACL uute failide ja kaustade loomine

POSIX-ühilduv süsteemis Üldine mõiste on selle umask on 9-bitine väärtus õigust **omamise kasutaja**, **rühma omamise**ja **muude** lapse uue faili või kausta juurdepääsu ACL teisendamiseks kasutatava emakausta kohta. Mõne umask bittide tuvastada, milliseid bittide lapse üksuse juurdepääsu ACL väljalülitamiseks. Seega on kasutatud valikuliselt vältimiseks paljundamine õiguste omamise kasutaja, rühma, omamise ja muud.
  
HDFS süsteemis on umask on tavaliselt kogu saidi konfigureerimist, mis kontrollib administraatorid. Lake andmesalve kasutab mõnda **konto hõlmavaid umask** , mida ei saa muuta. Järgmine tabel näitab andmesalve Lake umask.

| Kasutaja rühma  | Säte | Uue lapse üksuse juurdepääsu ACL mõju |
|------------ |---------|---------------------------------------|
| Kasutaja omamise | ---     | Mingit mõju                             |
| Rühma omamise| ---     | Mingit mõju                             |
| Muud       | RWX     | Eemalda Read + kirjutamine + käivitada         | 

Järgmisel pildil kuvatakse see umask toiming. Mõju on eemaldada **lugeda + kirjutamine + käivitada** **teise** kasutaja jaoks. Kuna selle umask ei määranud bittide **omamise kasutaja** ja **rühma omamise**, need õigused on ümber.

![Lake andmesalve ACL-ID](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>Sticky bitine

Sticky bitine funktsioon POSIX failisüsteemi keerukamaks. Lake andmesalve kontekstis, on tõenäoline, et sticky natuke on vaja.

Järgmises tabelis antakse ülevaade sticky bitise andmete Lake poes tööpõhimõte.

| Kasutaja rühma         | Faili    | Kausta |
|--------------------|---------|-------------------------|
| Sticky bitine **väljas** | Mingit mõju   | Mingit mõju           |
| Sticky bitise **ON**  | Mingit mõju   | Takistab igaüks, välja arvatud **Super-kasutajate** ja selle **kasutaja omamise** lapse üksuse kustutamise või selle lapse üksuse ümbernimetamine.               |

Azure'i portaalis sticky bitine ei kuvata.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Levinud küsimused andmete Lake poes ACL-ID

Siin on mõned küsimused, tulla sageli ACL andmete Lake poes suhtes.

### <a name="do-i-have-to-enable-support-for-acls"></a>Kas ma pean ACL-ID toe lubamine

Ei. Juurdepääsu reguleerimine kaudu ACL-ID on alati Lake andmesalve konto.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Milliseid õigusi on vaja rekursiivselt Kustuta kaust ja selle sisu?

* Emakausta peab olema **kirjutamine + käivitada**.
* Kausta välja jätta ja iga kaust, nõuab **lugeda + kirjutamine + käivitada**.
>[AZURE.NOTE] Kustutage failid kaustast nõuab kirjutamine failidega. Lisaks juurkausta "/" **ei** kustutata.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Kes on seatud omanik ja faili või kausta?

Faili või kausta looja saab omanik.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Kes on seatud kes omavad rühmana, faili või kausta loomist?

See kopeeritakse kes omavad rühma, mille all on loodud uue faili või kausta emakausta.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ma olen kes omavad kasutaja faili, kuid mul pole vaja RWX õigused. Mida teha?

Kes omavad kasutaja saab muuta lihtsalt ise mis tahes neile vajalikule RWX õiguste andmiseks faili õigused.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Kas andmesalve Lake toetab pärimise ACL-ID?

Ei.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Mis on maski ja umask vaheline erinevus?

| maski | umask|
|------|------|
| Atribuut **maski** on saadaval faili või kausta. | **Umask** on atribuut Lake andmesalve konto. Niisiis, on ainult ühe umask Lake andmesalve.    |
| Faili või kausta atribuudi maski saate muuta kes omavad kasutaja või rühma kes omavad faili või mõne superkasutaja. | Atribuudi umask ei saa muuta kõigi kasutajate isegi administraatori poolt. See on muutumatu, järele konstandi väärtus.|
| Maski atribuuti kasutatakse ajal juurdepääsu kontrollimine algoritmi käitusajal kindlaks teha, kas kasutaja on õigus sooritada toimingut faili või kausta. Roll mask on luua "tõhusa õiguste" ajal juurdepääs sisse. | Funktsiooni umask ei kasutata ajal juurdepääsu kontrollimine üldse. Accessi uued lapse üksused kausta ACL määratlemiseks kasutatakse funktsiooni umask. |
| Mask on 3-bitine RWX väärtus, mis kehtib nimega kasutaja, rühma nimega ja kes omavad kasutaja ajal juurdepääs sisse.| Funktsiooni umask on 9 bitist väärtus, mis kehtib kes omavad kasutaja, kes omavad rühmitamine ja muud uue lapse.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Kust saada lisateavet POSIX juurdepääsu juhtimine mudelit?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS õiguse juhend](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX KKK](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [ACL kaudu juurdepääsu juhtimine loetletud Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Vt ka

* [Azure'i andmesalve Lake ülevaade](data-lake-store-overview.md)

* [Azure'i andmeanalüüsi Lake kasutamise alustamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





