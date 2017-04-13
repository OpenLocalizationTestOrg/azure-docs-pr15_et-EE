<properties
   pageTitle="Lake andmesalve turvalisuse ülevaade | Microsoft Azure'i"
   description="Kuidas Azure'i andmesalve Lake on suur turvalisemad andmesalve mõistmine"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Azure'i Lake poes turvalisus

Paljud on suur andmeanalüüsi for business teadmisi, mis aitavad need nutikas otsuste ära. Ettevõtte võib olla keeruline ja reguleeritud keskkonnas, kus hulganisti mitmesuguse kasutajad. See on oluline veendumaks, et kriitilised äriandmete talletatakse turvalisemalt, õige taseme üksikkasutajate antud ettevõte. Azure'i andmesalve Lake on loodud vasta nendele nõuetele turvalisus. Selle artikli teave turvalisus võimaluste andmesalve Lake, sh:

* Autentimine
* Luba
* Võrgu eraldamise
* Andmekaitse
* Auditi

## <a name="authentication-and-identity-management"></a>Autentimis- ja identiteedi haldamine

Autentimine on toiming, mille kasutaja identiteet kinnitatud kui kasutaja suhtleb Lake andmesalve või mis tahes teenus, mis loob ühenduse Lake andmesalve. Identiteetide haldus ja autentimine, kasutab Lake andmesalve [Azure Active Directory](../active-directory/active-directory-whatis.md), täielik identiteedi ja juurdepääsu haldus pilve lahenduse, mis lihtsustab kasutajate ja rühmade haldus.

Iga Azure'i tellimus võib olla seotud Azure Active Directory eksemplar. Ainult kasutajate ja teenuse identiteedid, mis on määratletud Azure Active Directory teenust pääseb Lake andmesalve konto abil Azure portaali, käsurea tööriistad või kaudu klientrakendustes ettevõtte koostab Azure'i andmed Lake poe SDK abil. Võtme Azure Active Directory nimega kesksetest Accessi juhtelemendi süsteem on:

* Lihtsustatud identiteedi elutsükli haldus. Kasutaja või teenuse (mõne teenuse põhisumma identiteedi) saab kiiresti luua ja lihtsalt kustutada või keelates konto kataloogis kiiresti tühistada.

* Mitmikautentimise. [Mitmikautentimise](../multi-factor-authentication/multi-factor-authentication.md) pakub väärtpaberi kasutaja sisselogimist ja tehingud.

* Mis tahes klient läbi standardne avatud protokoll, näiteks OAuthi või OpenID autentimist.

* Ettevõtte kataloogiteenustega ja pilveteenuse identiteedipakkujad Federation.

## <a name="authorization-and-access-control"></a>Autoriseerimine ja juurdepääs juhtelement

Pärast Azure Active Directory autendib kasutaja, et kasutaja saab kasutada Azure andmesalve Lake, autoriseerimine juhtelementide juurde Lake andmesalve õigused. Lake andmesalve eraldab autoriseerimine kontoga seotud ja andmetega seotud järgmisel viisil:

* [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-what-is.md) Konto haldamise kohta Azure esitatud (RBAC)
* POSIX ACL poes andmetele juurdepääsuks


### <a name="rbac-for-account-management"></a>Konto haldamise RBAC

Neli lihtsa rolli määratud Lake andmesalve vaikimisi. Rollid võimaldavad erinevaid toiminguid Lake andmesalve konto Azure portaali, PowerShelli cmdlet-käskude ja REST API-de kaudu. Omaniku poole ja kaasautor rollid saab teha mitmesuguseid halduse funktsioonide konto. Saate määrata kasutajatele, kelle andmeid interaktiivselt kasutada ainult lugeja roll.

![RBAC rollid] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC rollid")

Pange tähele, et kuigi kontohaldus on määratud rolle, mõned rollid mõjutada juurdepääsu andmetele. Peate kasutama ACL-ID toimingute kohta, mida kasutaja saab teha failisüsteemi juurdepääsu piiramiseks. Järgmises tabelis on kokkuvõte andmete juurdepääsuõigusi vaikimisi rollide ja õiguste halduse.

| Rollid                    | Teabeõiguste haldus               | Andmete juurdepääsuõigused | Selgitus |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Pole roll         | Ükski                            | Reguleeritud ACL    | Kasutaja ei saa kasutada Azure PowerShelli cmdlet-käskude ja Azure portaali andmete Lake poodi sirvida. Kasutaja saab kasutada ainult käsurea tööriistad.
| Omanik  | Kõik  | Kõik  | Omanik on eeliskasutaja. Selle rolli saate hallata kõike ja on täielik juurdepääs andmetele.
| Lugeja   | Kirjutuskaitstud  | Reguleeritud ACL    | Lugeja roll saate vaadata kõik konto haldamise, nt mis kasutajale on määratud mis rolli kohta. Lugeja rolli ei saa muuta.   |
| Kaasautor              | Kõik, v.a rollide lisamine ja eemaldamine | Reguleeritud ACL    | Kaasautori roll saate mõni ettevõte, nt juurutuste ja loomise ja haldamise teatiste seonduvat hallata. Kaasautori roll ei saa lisada või eemaldada rollid.
| Kasutaja juurdepääs administraator | Lisamine ja eemaldamine rollid            | Reguleeritud ACL    | Administraaorirolliga kasutajate juurdepääsu saate hallata kasutajate juurdepääsu kontod. |

Juhised leiate teemast [määrata kasutajad või turberühmad Lake andmesalve kontod](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Kasutamise toimingute operatsioonisüsteemides faili ACL-ID

Lake andmesalve on hierarhilise failisüsteemi nagu Hadoopi jaotatud faili süsteemi (HDFS) ja see toetab [POSIX ACL-ID](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Seda määrab lugemine (r), kirjutage (w) ja käivitada (ressursid omanik roll, rühma omanikud ja teiste kasutajate ja rühmade õiguste x). Andmete Lake poe avaliku eelvaates (väljaanne) ACL-ID on lubatud juurkausta, alamkaustad ja üksikuid faile. ACL-ID, mida saate rakendada juurkausta rakendada ka kõik lapse kaustad ja failid.

Soovitatav on määratleda [turberühmad](../active-directory/active-directory-accessmanagement-manage-groups.md)abil mitme kasutaja jaoks ACL-ID. Turberühma kasutajate lisamine ja seejärel määrata selle turberühma ACL faili või kausta. See on kasulik, kui soovite kohandatud juurdepääsu, kuna olete lisada kuni üheksa kirjete kohandatud juurdepääsu piiratud. Paremini turvaline talletatud Lake andmesalve Azure Active Directory turberühmad abil andmete kohta lisateabe saamiseks lugege teemat [kasutaja või turberühma ACL Azure'i andmesalve Lake failisüsteemi määramine](data-lake-store-secure-data.md#filepermissions).

![Standard- ja kohandatud loend Accessi] (./media/data-lake-store-security-overview/adl.acl.2.png "Standard- ja kohandatud loend Accessi")

## <a name="network-isolation"></a>Võrgu eraldamise

Kasutage andmete Lake poodi juurdepääsu haldamine aitab teie andmete poodi võrgu tasemel. Saate luua tulemüürid ja IP-aadresside vahemiku määratlemine oma usaldusväärsete klientidele. IP-aadresse, saate Lake andmesalve ühendada ainult klientidele, mis on määratletud vahemikus IP-aadress.

![Tulemüüri sätted ja IP-juurdepääs] (./media/data-lake-store-security-overview/firewall-ip-access.png "Tulemüüri sätted ja IP-aadress")

## <a name="data-protection"></a>Andmekaitse

Veenduge, et oma ettevõtte jaoks kriitilise tähtsusega andmed on turvaline kogu elutsükli soovite ettevõtted. Töödeldavate andmete, kasutab Lake andmesalve industry-standard transpordikihi Turve (TLS) protokolli andmete vahel klient ja Lake andmesalve turvalisuse tagamiseks.

Andmekaitse ülejäänud andmete jaoks on saadaval tulevaste versioonidega.

## <a name="auditing-and-diagnostic-logs"></a>Auditi- ja diagnostika logid

Saate kasutada Auditeerimispoliitika või diagnostika logid, olenevalt sellest, kas otsite logid haldamisega seotud või andmetega seotud tegevuste jaoks.

*  Haldamisega seotud Azure'i ressursihaldur API-de kasutamine ja on uurinud Azure portaali kaudu auditilogid.
*  Andmetega seotud tegevuste WebHDFS REST API-de kasutamine ja on uurinud Azure portaali kaudu diagnostikalogid.

### <a name="auditing-logs"></a>Auditi logid

Määruste täitmiseks ettevõtte võib vaja piisavalt täiendamist kui seda on vaja minna konkreetseid juhtumeid sisse. Lake andmesalve on sisseehitatud jälgimine ja auditeerimine ja selle logib kõik konto tööd.

Konto haldamise täiendamist, vaadata ja valida veerud, mida soovite sisse logida. Samuti saate eksportida auditilogid Azure Storage.

![Auditilogide] (./media/data-lake-store-security-overview/audit-logs.png "Auditilogide")

### <a name="diagnostic-logs"></a>Diagnostikalogid

Saate määrata andmepääsu täiendamist Azure'i portaalis (jaotises diagnostika sätted) ja looge Azure'i bloobimälu salvestusruumi konto logid talletamiseks.

![Diagnostikalogid] (./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnostikalogid")

Pärast diagnostika sätete konfigureerimiseks saate vaadata vahekaardil **Diagnostikalogid** logid.

Azure'i andmed Lake poe diagnostikalogid töötamise kohta leiate lisateavet teemast [Accessi andmed Lake poe diagnostikalogid](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Kokkuvõte

Ettevõtte kliendid nõuda andmete analytics pilve platvorm, mis on turvaline ja lihtne kasutada. Azure'i andmesalve Lake on loodud nendele nõuetele kaudu identiteetide haldus ja autentimise kaudu integreerimine Azure Active Directory, ACL-põhine loa, võrgu eraldamise andmete krüptimine ajal kui ka ülejäänud (peagi tulevikus) aadress ja audit.

Kui soovite näha Lake andmesalve uued funktsioonid, saatke meile tagasisidet [andmete Lake poe UserVoice Foorum](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Vt ka

- [Azure'i andmesalve Lake ülevaade](data-lake-store-overview.md)
- [Lake andmesalve kasutamise alustamine](data-lake-store-get-started-portal.md)
- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
