<properties
    pageTitle="Kuidas kasutada Hudson bloobimälu | Microsoft Azure'i"
    description="Kirjeldab, kuidas kasutada Hudson koos Azure'i bloobimälu hoidla Koosta artefakte."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Lahenduse Hudson pidev integreerimine Azure Storage kasutamine

## <a name="overview"></a>Ülevaade

Järgmine teave näitab, kuidas kasutada bloobimälu hoidla Koosta esemeid loodud lahenduse Hudson pidev integratsioon (CI) või allalaaditavad failid allikana kasutatavad Koosta protsess. Üks stsenaariumid, kus soovite leida see kasulik on, kui te kasutate kodeerimise dünaamilised arengu keskkonnas (Java või muude keelte kasutamine), järgud töötab vastavalt pidev integratsioon ja vajate hoidla oma Koosta esemeid, nii, et võib, näiteks ettevõtte töötajad, oma klientidele ühiskasutusse või säilitamiseks arhiivi.  Teine stsenaarium on, kui teie koostamine töö enda jaoks on vaja muid faile, näiteks sõltuvused allalaadimiseks osana sisestusmeetodi koostamine.

Selles õpetuses kasutate Azure salvestusruumi lisandmoodulite jaoks Hudson CI Microsofti loodud.

## <a name="introduction-to-hudson"></a>Hudson tutvustus ##

Hudson võimaldab pidev integratsioon tarkvara projekti, mis võimaldab arendajatel hõlpsasti integreerida oma koodi muudatusi ja on järgud koostanud automaatselt ja sageli, tootlikkuse arendajad. Järgud on versiooniga ja Koosta esemeid saab erinevate hoidlate üles laadida. Selles artiklis näitab, kuidas kasutada Azure'i bloobimälu Koosta esemeid hoidla. See näitab ka Azure'i bloobimälu sõltuvused allalaadimine.

Lisateavet Hudson saate aadressil [Hudson koosolek](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Bloobimälu teenuse kasutamise eelised ##

Majutada oma dünaamilised arengu Koosta esemeid bloobimälu teenuse kasutamise eelised on järgmised.

- Koosta esemeid ja/või allalaaditavad sõltuvused kättesaadavuse.
- Kui teie lahendus Hudson CI, laaditakse teie Koosta esemeid jõudlust.
- Kui teie klientide ja partnerite alla oma Koosta esemeid jõudlust.
- Juhtida kasutaja juurdepääsupoliitikaid versiooniga valida anonüümse juurdepääsu, aegumise põhineva ühiskasutusega juurdepääsu allkirja juurdepääsu, Privaatne, access jne.

## <a name="prerequisites"></a>Eeltingimused ##

Peate oma Hudson CI lahendusega bloobimälu teenust kasutada järgmist:

- Hudson pidev integratsioon lahenduse.

    Kui teil pole praegu Hudson CI lahenduse, käivitada Hudson CI lahenduse, kasutades järgmist:

    1. Java lubatud arvutisse alla laadida Hudson sõda <http://hudson-ci.org/>.
    2. Käivitage käsku Käsuviip, mis on avatud kausta, mis sisaldab Hudson sõda Hudson sõda. Näiteks kui laadisite versioon 3.1.2:

        `java -jar hudson-3.1.2.war`

    3. Avage brauseris `http://localhost:8080/`. See avab Hudson armatuurlaud.

    4. Pärast esimest kasutamist Hudson, täitke Alghäälestus veebisaidil `http://localhost:8080/`.

    5. Pärast algset, Hudson sõda käivitatud eksemplari tühistamine, Hudson sõda uuesti alustada ja uuesti avada Hudson armatuurlaual `http://localhost:8080/`, mille abil saate installida ja konfigureerida Azure Storage lisandmoodul.

        Ajal tüüpiline Hudson CI lahenduse loodaks käivitamiseks, kui teenus ei tööta Hudson sõda käsurea piisab selles õpetuses.

- Azure'i konto. Saate registreeruda <http://www.azure.com>Azure'i konto.

- Azure storage konto. Kui te pole veel salvestusruumi konto, saate luua ühe abil juhiseid [Loo konto salvestusruumi](storage-create-storage-account.md#create-a-storage-account).

- Tundmine Hudson CI lahendus on soovitatav, kuid ei pea, kui kasutatakse järgmist sisu kuvamiseks, kui nimega hoidla bloobimälu teenuse kasutamise Hudson CI juhiseid koostada esemeid tavalise näide.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Kuidas kasutada teenust bloobimälu Hudson CI ##

Hudson bloobimälu teenuse kasutamiseks peate Azure Storage lisandmooduli installimine, konfigureerimine salvestusruumi kontoga lisandmoodul ja seejärel looge pärast Koosta toiming, mis on lisatud teie Koosta esemeid oma konto salvestusruumi. Järgmistes jaotistes kirjeldatakse neid juhiseid.

## <a name="how-to-install-the-azure-storage-plugin"></a>Kuidas installida Azure Storage lisandmoodul ##

1. Klõpsake armatuurlaualt Hudson **Hudson haldamine**.
2. Klõpsake lehel **Halda Hudson** **Lisandmoodulite haldamine**.
3. Klõpsake vahekaarti **saadaval** .
4. Valige **teisi**.
5. Valige jaotises **Artefakt Uploaders** **lisandmoodul Microsoft Azure'i Tabelimäluga**.
6. Klõpsake nuppu **Installi**.
7. Kui installimine on lõpule jõudnud, taaskäivitage Hudson.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Kuidas konfigureerida Azure Storage lisandmooduli salvestusruumi kontoga ##

1. Klõpsake armatuurlaualt Hudson **Hudson haldamine**.
2. Klõpsake lehel **Halda Hudson** **Süsteemi konfigureerimine**.
3. Klõpsake jaotises **Microsoft Azure'i salvestusruumi konto konfigureerimine** :

    lisamine. Sisestage oma salvestusruumikonto nimi, mille saate hankida [Azure portaali](https://portal.azure.com).

    b. Sisestage salvestusruumi konto, [Azure portaali](https://portal.azure.com)Lisaks võimalik saada.

    c. Kui kasutate avaliku Azure'i pilves, kasutage vaikeväärtust, milleks **bloobimälu teenuse lõpp-punkti** URL-i. Kui kasutate muu Azure pilveteenuse, kasutada lõpp-punkti [Azure portaali](https://portal.azure.com) konto salvestusruumi.

    d. Klõpsake nuppu **Valideeri salvestusruumi identimisteabe** salvestusruumi konto kinnitada.

    e. [Valikulised] Kui teil on täiendavat salvestusruumi kontod, mida soovite teha kättesaadavaks oma Hudson CI, klõpsake nuppu **rohkem mäluruumi kontosid lisada**.

    f. Klõpsake sätete salvestamiseks **Salvesta** .

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Pärast koostamine toiming, mis on lisatud teie koostamine esemeid oma salvestusruumi konto loomise kohta ##

Juhis eesmärgil esmalt vajame tööd, mis on luua mitu faili ja seejärel lisage pärast Koosta tegevuse laadige failid üles oma salvestusruumi konto loomiseks.

1. Hudson armatuurlaualt, klõpsake nuppu **Uus töökoht**.
2. Töö **MyJob**nime, klõpsake **- Laadi tarkvara töö koostamine**ja seejärel klõpsake nuppu **OK**.
3. Töö konfiguratsiooni jaotises **koostamiseks** klõpsake nuppu **lisa koostada samm** ja valige **käivitada Windows partii käsk**.
4. **Käsu**kasutada järgmisi käske.

        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt

5. Klõpsake töö konfiguratsiooni jaotises **Koosta järgmised toimingud** **esemeid Microsoft Azure'i bloobimälu üles laadida**.
6. **Salvestusruumikonto nimi**, valige salvestusruumi konto, mida soovite kasutada.
7. **Container nimi**, määrake container nimi. (S.o ümbris luuakse kui see pole juba olemas, kui Koosta esemeid laaditakse.) Saate kasutada keskkonna muutujaid, nii et see näide Sisestage **${JOB_NAME}** nimeks ümbrises.

    **Näpunäide.**

    All **käsk** jaotis kui sisestasite skripti **käivitada Windows partii käsk** on link tuvasta Hudson keskkonna muutujaid. Lisateavet keskkonnas muutuv nimede ja kirjelduste seda linki. Pange tähele, et keskkonna muutujate sisaldada erimärke, nt **BUILD_URL** keskkonna muutuja, ei ole lubatud container nimena või levinud virtuaalse tee.

8. Selles näites nuppu **Uus ümbris avalikuks vaikimisi** . (Kui soovite kasutada privaatne container, peate juurdepääsu lubamiseks ühiskasutusega juurdepääsu allkirja loomine. Mis on selles artiklis ei käsitleta. Saate selle kohta leiate lisateavet ühiskasutusega juurdepääsu allkirjade veebisaidil [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Valikulised] Klõpsake nuppu **Puhasta container enne üleslaadimist** , kui soovite container kustutatud sisu enne koostamine esemeid on üles laaditud (jätta märkimata kui te ei soovi puhastada ümbris sisu).
10. **Loendi esemeid üles laadida**, sisestage * *teksti /*.txt**.
11. **Levinud virtuaalse tee üleslaaditud artefakte**, sisestage **${koostada\_ID} / ${koostada\_arv}**.
12. Klõpsake sätete salvestamiseks **Salvesta** .
13. Hudson armatuurlaua, klõpsake **Koostada nüüd** **MyJob**käivitamiseks. Uurige konsooli väljundi olek. Olekuteade Azure Storage kaasatakse konsooli väljundi koostamine pärast toimingu käivitamisel üles laadida Koosta esemeid.
14. Pärast edukat lõpetamist töö, saate uurida, avades avaliku bloobimälu Koosta esemeid.

    lisamine. [Azure'i portaali](https://portal.azure.com)sisse logima.

    b. Klõpsake **salvestusruumi**.

    c. Klõpsake salvestusruumi konto nimi, mida kasutasite Hudson.

    d. Klõpsake **ümbriste**.

    e. Klõpsake ümbris nimega **myjob**, mis on töö nimi, kui olete loonud Hudson töö määratud väike versioon. Container nimed ja bloobimälu nimed on väiketähed (ja tõstutundlik) Azure Storage. Peaksite nägema loendis plekid jaoks ümbris nimega **myjob** **hello.txt** ja **date.txt**. Kopeerige URL, kas need üksused ja selle oma brauseris avada. Kuvatakse kujul koostamine artefakt tekstifail, mis on üles laaditud.

Töö saab luua ainult üks pärast Koosta toiming, mille lisatud esemeid Azure'i bloobimälu. Pange tähele, et pärast koostamine ühe toimingu üles laadida esemeid Azure'i bloobimälu saate määrata, erinevaid faile (sh metamärkide) ja teed, kus failide **Loendi esemeid üles laadida** kasutades eraldajana semikoolonit. Näiteks oma Hudson Koosta toodab JAR failid ja TXT failid oma tööruumi **luua** kausta, kui soovite üles laadida nii Azure'i bloobimälu, kasutage järgmist väärtus **Esemeid loendis üles laadida** : **koostamine /\*laiendid; koostamine /\*.txt**. Kahekordne-koolon süntaksi abil saate määrata tee kasutada bloobimälu nimi. Näiteks, kui soovite, et saada üles laaditud **kahendfaile** bloobimälu tee ja TXT failide abil saate hankida üles laadida, kasutades **Märkused** bloobimälu tee purgid, kasutage järgmist väärtus **Esemeid loendis üles laadida** : **koostamine /\*. jar::binaries; koostamine /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Kuidas luua Koosta toimingut, mis laadib Azure'i bloobimälu ##

Järgmised toimingud Kuva konfigureerimine koostamine samm Azure'i bloobimälu üksuste alla. See oleks kasulik, kui soovite kaasata oma koostamine, näiteks purgid, mida hoiate Azure'i bloobimälu üksusi.

1. Töö konfiguratsiooni jaotises **koostamiseks** klõpsake nuppu **lisa koostada samm** ja valige **Azure'i bloobimälu alla laadida**.
2. **Salvestusruumikonto nimi**, valige salvestusruumi konto, mida soovite kasutada.
3. **Container nimi**, määrake ümbris, mille soovite alla laadida plekid nimi. Saate kasutada keskkonna muutujate.
4. **Bloobimälu nimi**, määrake bloobimälu nimi. Saate kasutada keskkonna muutujate. Samuti saate tärniga, metamärgina pärast teie määratud initsiaal(ID) bloobimälu nime. Näiteks **projekti\* ** soovite määrata kõik plekid, mille nimi algab stringiga **projekti**.
5. [Valikulised] **Tee alla laadida**, määrake Hudson arvutisse, kuhu soovite faile alla laadida Azure'i bloobimälu tee. Saate kasutada ka keskkonna muutujate. (Kui väärtuse ette ei **tee alla laadida**, failid Azure'i bloobimälu laaditakse projekti tööruumi.)

Kui teil on soovitud Azure'i bloobimälu alla, saate luua täiendavaid Koosta juhiseid.

Kui käivitate ehitada, märkige ruut Koosta ajalugu konsooli väljundi või vaadata oma allalaadimiskoht näha, kas teie arvates plekid on laaditud.

## <a name="components-used-by-the-blob-service"></a>Kasutatavad bloobimälu teenuse komponendid ##

Järgmine annab ülevaate bloobimälu teenuse komponendid.

- **Salvestusruumi konto**: Azure Storage kõigi juurdepääs on valmis salvestusruumi konto kaudu. See on kõrgeima taseme nimeruumi juurdepääsuks plekid. Konto võib sisaldada piiramatu arvu ümbriste, kui tema kokku maht on 100-TB.
- **Container**: ümbris pakub rühmitamise plekid kogumi. Kõik plekid peab olema ümbris. Konto võib sisaldada ümbriste piiramatu arv. Ümbris saate talletada plekid piiramatu arv.
- **Bloobimälu**: faili tüüp ja suurus. On kahte tüüpi plekid, mida saab salvestada Azure Storage: Blokeeri ja lehe plekid. Enamik failid on blokeerimine plekid. Ühe ploki bloobimälu võib olla kuni 200 GB suuruseid. Selle õpetuse kasutab Blokeeri plekid. Lehe plekid bloobimälu mõnda muud tüüpi, võib olla kuni 1 TB suuruse ja tõhusam on kui vahemike baiti faili muudetakse sageli. Plekid kohta leiate lisateavet teemast [mõistmine Blokeeri plekid, plekid lisada, ja lehe plekid](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-i vorming**: plekid on adresseeritavad URL-i järgmises vormingus:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`

    (Ülaltoodud vorming kehtib avalikule Azure pilveteenusesse. Kui kasutate muu Azure pilveteenuse, kasutamine lõpp-punkti [Azure portaali](https://portal.azure.com) määratlemiseks oma URL-i lõpp-punkti.)

    Eeltoodud vormingus `storageaccount` tähistab salvestusruumi konto nime `container_name` tähistab oma container nime ja `blob_name` tähistab vastavalt oma bloobimälu nime. Sees container nime, võib olla mitu teed, eraldades need kaldkriips, **/**. Selles õpetuses näide container nimi oli **MyJob**, ja **${koostada\_ID} / ${koostada\_arv}** kasutati levinud virtuaalse tee, tulemuseks bloobimälu, võttes URL järgmisel kujul:

    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Järgmised sammud

- [Vastavad Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
- [Azure'i salvestusruumi SDK Java](https://github.com/azure/azure-storage-java)
- [Azure'i salvestusruumi kliendi SDK viide](http://dl.windowsazure.com/storage/javadoc/)
- [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)

Lisateavet leiate teemast ka [Java Arenduskeskus](https://azure.microsoft.com/develop/java/).