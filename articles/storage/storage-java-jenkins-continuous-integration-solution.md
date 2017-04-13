<properties 
    pageTitle="Azure'i salvestusruumi funktsiooniga Jenkins pidev integratsioon lahenduse | Microsoft Azure'i" 
    description="Selle õpetuse näitab, kuidas kasutada Azure'i bloobimälu teenuse hoidla koostada esemeid loodud Jenkins pidev integratsioon lahenduse." 
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

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Lahenduse Jenkins pidev integreerimine Azure Storage kasutamine

## <a name="overview"></a>Ülevaade

Järgmine teave näitab, kuidas kasutada bloobimälu hoidla Koosta esemeid loodud lahenduse Jenkins pidev integratsioon (CI) või allalaaditavad failid allikana kasutatavad Koosta protsess. Üks stsenaariumid, kus soovite leida see kasulik on, kui te kasutate kodeerimise dünaamilised arengu keskkonnas (Java või muude keelte kasutamine), järgud töötab vastavalt pidev integratsioon ja vajate hoidla oma Koosta esemeid, nii, et võib, näiteks ettevõtte töötajad, oma klientidele ühiskasutusse või säilitamiseks arhiivi. Teine stsenaarium on kui tööpäevad Koosta ise nõuab muid faile, näiteks sõltuvused allalaadimiseks sisestusmeetodi koostamine osana.

Selles õpetuses kasutate Azure salvestusruumi lisandmoodulite jaoks Jenkins CI Microsofti loodud.

## <a name="overview-of-jenkins"></a>Jenkins ülevaade ##

Jenkins võimaldab pidev integratsioon tarkvara projekti, mis võimaldab arendajatel hõlpsasti integreerida oma koodi muudatusi ja on järgud koostanud automaatselt ja sageli, tootlikkuse arendajad. Järgud on versiooniga ja Koosta esemeid saab erinevate hoidlate üles laadida. See teema näitab, kuidas kasutada Azure'i bloobimälu koostamine esemeid hoidla. See näitab ka Azure'i bloobimälu sõltuvused allalaadimine.

Lisateavet Jenkins saate aadressil [Jenkins koosolek](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Bloobimälu teenuse kasutamise eelised ##

Majutada oma dünaamilised arengu Koosta esemeid bloobimälu teenuse kasutamise eelised on järgmised.

- Koosta esemeid ja/või allalaaditavad sõltuvused kättesaadavuse.
- Kui teie lahendus Jenkins CI, laaditakse teie koostamine esemeid jõudlust.
- Kui teie klientide ja partnerite alla oma koostamine esemeid jõudlust.
- Juhtida kasutaja juurdepääsupoliitikaid versiooniga valida anonüümse juurdepääsu, aegumise põhineva ühiskasutusega juurdepääsu allkirja juurdepääsu, Privaatne, access jne.

## <a name="prerequisites"></a>Eeltingimused ##

Peate oma Jenkins CI lahendusega bloobimälu teenust kasutada järgmist:

- Jenkins pidev integratsioon lahenduse.

    Kui teil pole praegu Jenkins CI lahenduse, käivitada Jenkins CI lahenduse, kasutades järgmist:

    1. Java lubatud arvutisse alla laadida jenkins.war <http://jenkins-ci.org>.
    2. Käsuviip, mis on avatud kausta, mis sisaldab jenkins.war, käivitage:

        `java -jar jenkins.war`

    3. Avage brauseris `http://localhost:8080/`. See avab Jenkins armatuurlaua, mille abil saate installida ja konfigureerida Azure Storage lisandmoodul.

        Ajal tüüpiline Jenkins CI lahenduse loodaks käivitamiseks, kui teenus ei tööta Jenkins sõda käsurea piisab selles õpetuses.

- Azure'i konto. Saate registreeruda <http://www.azure.com>Azure'i konto.

- Azure storage konto. Kui te pole veel salvestusruumi konto, saate luua ühe abil juhiseid [Loo konto salvestusruumi](storage-create-storage-account.md#create-a-storage-account).

- Tundmine Jenkins CI lahendus on soovitatav, kuid ei pea, kui kasutatakse järgmist sisu kuvamiseks, kui nimega hoidla bloobimälu teenuse kasutamise Jenkins CI juhiseid koostada esemeid tavalise näide.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Kuidas kasutada teenust bloobimälu Jenkins CI ##

Jenkins bloobimälu teenuse kasutamiseks peate Azure Storage lisandmooduli installimine, konfigureerimine salvestusruumi kontoga lisandmoodul ja seejärel looge pärast Koosta toiming, mis on lisatud teie Koosta esemeid oma konto salvestusruumi. Järgmistes jaotistes kirjeldatakse neid juhiseid.

## <a name="how-to-install-the-azure-storage-plugin"></a>Kuidas installida Azure Storage lisandmoodul ##

1. Klõpsake armatuurlaualt Jenkins **Jenkins haldamine**.
2. Klõpsake lehel **Halda Jenkins** **Lisandmoodulite haldamine**.
3. Klõpsake vahekaarti **saadaval** .
4. Märkige jaotises **Artefakt Uploaders** **lisandmoodul Microsoft Azure'i Tabelimäluga**.
5. Klõpsake nuppu **ilma uuesti installimine** või **allalaadimine ja installimine pärast uuesti**.
6. Taaskäivitage Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Kuidas konfigureerida Azure Storage lisandmooduli salvestusruumi kontoga ##

1. Klõpsake armatuurlaualt Jenkins **Jenkins haldamine**.
2. Klõpsake lehel **Halda Jenkins** **Süsteemi konfigureerimine**.
3. Klõpsake jaotises **Microsoft Azure'i salvestusruumi konto konfigureerimine** :
    1. Sisestage oma salvestusruumikonto nimi, mille saate hankida [Azure portaali](https://portal.azure.com).
    2. Sisestage salvestusruumi konto, [Azure portaali](https://portal.azure.com)Lisaks võimalik saada.
    3. Kui kasutate avaliku Azure'i pilves, kasutage vaikeväärtust, milleks **bloobimälu teenuse lõpp-punkti** URL-i. Kui kasutate muu Azure pilveteenuse, kasutada lõpp-punkti [Azure portaali](https://portal.azure.com) konto salvestusruumi. 
    4. Klõpsake nuppu **Valideeri salvestusruumi identimisteabe** salvestusruumi konto kinnitada. 
    5. [Valikulised] Kui teil on täiendavat salvestusruumi kontod, mida soovite teha kättesaadavaks oma Jenkins CI, klõpsake nuppu **rohkem mäluruumi kontosid lisada**.
    6. Klõpsake sätete salvestamiseks **Salvesta** .

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Kuidas luua pärast koostamine toiming, mis laaditakse teie koostamine esemeid salvestusruumi kontole ##

Juhis eesmärgil esmalt vajame tööd, mis on luua mitu faili ja seejärel lisage pärast Koosta tegevuse laadige failid üles oma salvestusruumi konto loomiseks.

1. Armatuurlaualt Jenkins nuppu **Uus üksus**.
2. Töö **MyJob**nime, klõpsake **- Laadi tarkvara projekti koostamine**ja seejärel klõpsake nuppu **OK**.
3. Töö konfiguratsiooni jaotises **koostamiseks** klõpsake nuppu **lisa koostada samm** ja valige **käivitada Windows partii käsk**.
4. **Käsu**kasutada järgmisi käske.

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. Töö konfiguratsiooni jaotises **Koosta järgmised toimingud** nuppu **Lisa pärast Koosta toiming** ja valige **Laadi üles esemeid Azure'i bloobimälu**.
6. **Salvestusruumikonto nimi**, valige salvestusruumi konto, mida soovite kasutada.
7. **Container nimi**, määrake container nimi. (S.o ümbris luuakse kui see pole juba olemas, kui Koosta esemeid laaditakse.) Saate kasutada keskkonna muutujaid, nii et see näide Sisestage **${JOB_NAME}** nimeks ümbrises.

    **Näpunäide.**
    
    All **käsk** jaotis kui sisestasite skripti **käivitada Windows partii käsk** on link tuvasta Jenkins keskkonna muutujaid. Lisateavet keskkonnas muutuv nimede ja kirjelduste seda linki. Pange tähele, et keskkonna muutujate sisaldada erimärke, nt **BUILD_URL** keskkonna muutuja, ei ole lubatud container nimena või levinud virtuaalse tee.

8. Selles näites nuppu **Uus ümbris avalikuks vaikimisi** . (Kui soovite kasutada privaatne container, peate juurdepääsu lubamiseks ühiskasutusega juurdepääsu allkirja loomine. Mis on selles teemas väljapoole. Saate selle kohta leiate lisateavet ühiskasutusega juurdepääsu allkirjade veebisaidil [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Valikulised] Klõpsake nuppu **Puhasta container enne üleslaadimist** , kui soovite container kustutatud sisu enne Koosta esemeid on üles laaditud (jätta märkimata kui te ei soovi puhastada ümbris sisu).
10. **Loendi esemeid üles laadida**, sisestage * *teksti /*.txt**.
11. **Levinud virtuaalse tee üleslaaditud artefakte**, sisestage selle õpetuse eesmärgil **${koostada\_ID} / ${koostada\_arv}**.
12. Klõpsake sätete salvestamiseks **Salvesta** .
13. Armatuurlaua Jenkins, klõpsake **Koostada nüüd** **MyJob**käivitamiseks. Uurige konsooli väljundi olek. Olekuteade Azure Storage kaasatakse konsooli väljundi koostamine pärast toimingu käivitamisel üles laadida Koosta esemeid.
14. Eduka töö lõpetanud, saate uurida, avades avaliku bloobimälu esemeid koostamine.
    1. [Azure'i portaali](https://portal.azure.com)sisse logida.
    2. Klõpsake **salvestusruumi**.
    3. Klõpsake salvestusruumi konto nimi, mida kasutasite Jenkins.
    4. Klõpsake **ümbriste**.
    5. Klõpsake ümbris nimega **myjob**, mis on töö nimi, kui olete loonud Jenkins töö määratud väike versioon. Container nimed ja bloobimälu nimed on väiketähed (ja tõstutundlik) Azure laos. Peaksite nägema loendis plekid jaoks ümbris nimega **myjob** **hello.txt** ja **date.txt**. Kopeerige URL, kas need üksused ja selle oma brauseris avada. Kuvatakse kujul Koosta artefakt tekstifail, mis on üles laaditud.

Töö saab luua ainult üks pärast koostamine toiming, mille üleslaadimist esemeid Azure'i bloobimälu. Pange tähele, et pärast koostamine ühe toimingu esemeid üleslaadimiseks Azure'i bloobimälu saate määrata, erinevaid faile (sh metamärkide) ja teed, kus failide **Loendi esemeid üles laadida** kasutades eraldajana semikoolonit. Näiteks oma Jenkins koostamine toodab JAR failid ja TXT failid oma tööruumi **luua** kausta, kui soovite üles laadida nii Azure'i bloobimälu, kasutage järgmist väärtus **Esemeid loendis üles laadida** : **koostamine /\*laiendid; koostamine /\*.txt**. Kahekordne-koolon süntaksi abil saate määrata selle tee kasutada bloobimälu nimi. Näiteks, kui soovite, et saada üles laadida **kahendfaile** bloobimälu tee ja TXT failide abil saate hankida üles laadida, kasutades **Märkused** bloobimälu tee purgid, kasutage järgmist väärtus **Esemeid loendis üles laadida** : **koostamine /\*. jar::binaries; koostamine /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Kuidas luua Koosta toimingut, mis laadib Azure'i bloobimälu ##

Järgmised toimingud Kuva konfigureerimine koostamine samm Azure'i bloobimälu üksuste alla. See oleks kasulik, kui soovite kaasata oma Koosta üksusi, näiteks purgid, mida hoiate Azure'i Bloobivahemälu salvestusruumi.

1. Töö konfiguratsiooni jaotises **koostamiseks** klõpsake nuppu **lisa koostada samm** ja valige **Azure'i bloobimälu alla laadida**.
2. **Salvestusruumikonto nimi**, valige salvestusruumi konto, mida soovite kasutada.
3. **Container nimi**, määrake ümbris, mille soovite alla laadida plekid nimi. Saate kasutada keskkonna muutujate.
4. **Bloobimälu nimi**, määrake bloobimälu nimi. Saate kasutada keskkonna muutujate. Samuti saate tärniga, metamärgina pärast teie määratud initsiaal(ID) bloobimälu nime. Näiteks **projekti\* ** soovite määrata kõik plekid, mille nimi algab stringiga **projekti**.
5. [Valikulised] **Tee alla laadida**, määrake Jenkins arvutisse, kuhu soovite faile alla laadida Azure'i bloobimälu tee. Saate kasutada ka keskkonna muutujate. (Kui väärtuse ette ei **tee alla laadida**, failid Azure'i bloobimälu laaditakse projekti tööruumi.)

Kui teil on soovitud Azure'i bloobimälu alla, saate luua täiendavaid Koosta juhiseid.

Kui käivitate ehitada, märkige ruut Koosta ajalugu konsooli väljundi või vaadata oma allalaadimiskoht näha, kas teie arvates plekid on laaditud.  

## <a name="components-used-by-the-blob-service"></a>Kasutatavad bloobimälu teenuse komponendid ##

Järgmine annab ülevaate bloobimälu teenuse komponendid.

- **Salvestusruumi konto**: Azure Storage kõigi juurdepääs on valmis salvestusruumi konto kaudu. See on kõrgeima taseme nimeruumi juurdepääsuks plekid. Konto võib sisaldada piiramatu arvu ümbriste, kui tema kokku maht on 100-TB.
- **Container**: ümbris pakub rühmitamise plekid kogumi. Kõik plekid peab olema ümbris. Konto võib sisaldada ümbriste piiramatu arv. Ümbris saate talletada plekid piiramatu arv.
- **Bloobimälu**: faili tüüp ja suurus. On kahte tüüpi plekid, mida saab salvestada Azure Storage: Blokeeri ja lehe plekid. Enamik failid on blokeerimine plekid. Ühe ploki bloobimälu võib olla kuni 200GB suuruseid. Selle õpetuse kasutab plokk plekid. Lehe plekid bloobimälu mõnda muud tüüpi, võib olla kuni 1TB suuruse ja tõhusam on kui vahemike baiti faili muudetakse sageli. Plekid kohta leiate lisateavet teemast [mõistmine Blokeeri plekid, plekid lisada, ja lehe plekid](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-i vorming**: plekid on adresseeritavad URL-i järgmises vormingus:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (Ülaltoodud vorming kehtib avalikule Azure pilveteenusesse. Kui kasutate muu Azure pilveteenuse, kasutamine lõpp-punkti [Azure portaali](https://portal.azure.com) määratlemiseks oma URL-i lõpp-punkti.)

    Eeltoodud vormingus `storageaccount` tähistab salvestusruumi konto nime `container_name` tähistab oma container nime ja `blob_name` tähistab vastavalt oma bloobimälu nime. Sees container nime, võib olla mitu teed, eraldades need kaldkriips, **/**. Selles õpetuses näide container nimi oli **MyJob**, ja **${koostada\_ID} / ${koostada\_arv}** kasutati levinud virtuaalse tee, tulemuseks bloobimälu, võttes URL järgmisel kujul:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Järgmised sammud

- [Vastavad Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure'i salvestusruumi SDK Java](https://github.com/azure/azure-storage-java)
- [Azure'i salvestusruumi kliendi SDK viide](http://dl.windowsazure.com/storage/javadoc/)
- [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)

Lisateavet leiate teemast ka [Java Arenduskeskus](https://azure.microsoft.com/develop/java/).