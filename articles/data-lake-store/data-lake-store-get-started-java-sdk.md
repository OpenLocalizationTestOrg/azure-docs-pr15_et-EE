<properties
   pageTitle="Rakenduste arendamise andmete Lake poe Java SDK abil | Microsoft Azure'i"
   description="Rakenduste arendamise Azure'i andmed Lake poe Java SDK abil"
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
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Azure'i Lake andmesalve Java kasutamise alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saate teada, kuidas kasutada Azure'i andmed Lake poe Java SDK põhiliste toimingute näiteks luua kaustu, üles- ja allalaadimine andmefailid jne. Andmete Lake kohta leiate lisateavet teemast [Azure andmesalve Lake](data-lake-store-overview.md).

Pääsete Java SDK API dokumendid Azure'i andmed Lake poe veebisaidil [Azure'i andmed Lake poe Java API dokumendid](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Eeltingimused

* Java arenduskomplekt (JDK 7 või uuem versioon, kasutades Java 1.7 või uuem versioon)
* Azure'i andmesalve Lake konto. Järgige veebisaidil [Alustamine Azure'i Lake andmesalve Azure'i portaalis](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Selle õpetuse kasutab Maven koostamine ja projekti sõltuvused. Kuigi on võimalik koostada Koosta süsteem nagu Maven või Gradle kasutamata, nende süsteemide teha on palju lihtsam hallata sõltuvused.
* (Valikuline) Ja nagu [Huvitav idee](https://www.jetbrains.com/idea/download/) või [Eclipse](https://www.eclipse.org/downloads/) IDE või sarnast.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Kuidas teha autentida, kasutades Azure Active Directory?

Selles õpetuses kasutame Azure AD Rakenduse kliendi salajane toomiseks on Azure Active Directory luba (teenusest autentimine). Selle märgiks abil luua objekti Lake andmesalve kliendi toimingute faili ja kataloogi toimingute tegemiseks. Juhised autentida Azure'i andmed Lake poe kaudu kliendi salajane me järgmiste toimingute üksikasjalik:

1. Mõni Azure AD veebirakenduse loomine
2. Saate tuua kliendi ID, kliendi salajane ja Turbeloa lõpp-punkti Azure AD veebirakenduse jaoks.
3. Accessi veebirakenduse Azure AD konfigureerimine Lake andmesalve faili/kaust, kuhu soovite juurde pääseda Java rakenduse loomisel.

Järgmiste toimingute juhised leiate teemast [Active Directory rakenduse loomine](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory pakub märgiks toomiseks kasutada ka muid võimalusi. Saate valida muu autentimise kaebuste vastavalt teie stsenaarium, näiteks rakendus töötab brauseri, töölauarakendus jaotatud rakenduse või serveri rakendus kohapealse või mõne Azure virtuaalse masina. Võite valida ka eri tüüpi mandaat, nt paroolide, sertide, autentimine 2 jne. Lisaks saate Azure Active Directory kasutajaid kohapealse Active Directory sünkroonimine pilve. Lisateavet leiate teemast [Azure Active Directory autentimise stsenaariumid](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Java rakenduse loomine

Kood Kuulake saadaval [github](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) kõnnib teid läbi loomise protsess failide talletamine, ühendades faile, faile alla laadida ja mõned poes failide kustutamine. Selle artikli jaotises teile põhiosa koodi kaudu.

1. Projekti Maven [mvn arhetüüp](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) käsurea kaudu või kasutades mõnda IDE loomine. Juhised abil huvitav Java projekti loomise kohta leiate teemast [siin](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Juhised Eclipse abil projekti loomise kohta leiate teemast [siin](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Lisage järgmised sõltuvused Maven **pom.xml** faili. Lisage järgmised koodilõigu saate teksti soovitud ** \</versioon >** sildi ja ** \</projekti >** silt:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Esimese sõltuvus on kasutada andmete Lake poe SDK (`azure-datalake-store`) maven hoidla. Teine sõltuvus (`slf4j-nop`) on täpsustada, millised logimine framework selle rakenduse kasutamise. Andmete Lake poe SDK kasutab [slf4j](http://www.slf4j.org/) logimine fassaad, mis võimaldab teil valida mitme levinud logimine raamistiku, nt log4j, Java logimine logback jne, või pole logimine. Selle näite puhul, keelame logimine, seega kasutame **slf4j-nop** sidumine. Muud logimissuvandite rakenduse kasutamiseks lugege teemat [siin](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Rakenduse koodi lisamine

On kolm põhiosa koodi.

1. Saada Azure Active Directory luba

2. Luba abil saate luua Lake andmesalve kliendi.

3. Lake andmesalve kliendi abil saate toiminguid teha.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Samm 1: Hankida on Azure Active Directory luba.

Andmete Lake poe SDK pakub mugav meetodid, mis võimaldavad teil vaja rääkida Lake andmesalve konto turvalisus märkide saada. Siiski SDK mandaat, et kasutada ainult järgmised võimalused. Saate kasutada muul viisil, et luba samuti, nagu [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)või oma kohandatud koodi abil.

Andmete Lake poe SDK abil saate hankida luba Active Directory veebirakenduse varem loodud, kasutage staatilise meetodid `AzureADAuthenticator` klassi. **Täitke-siin** asendada tegelike väärtuste Azure Active Directory veebirakenduse jaoks.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Samm 2: Looge objekti Azure'i andmesalve Lake kliendi (ADLStoreClient)

Objekti [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) loomine nõuab – saate määrata Lake andmesalve konto nimi ja Azure Active Directory luba viimases etapis loodud. Pange tähele, et Lake andmesalve konto nimi peab olema täielik domeeninimi. Näiteks Asendage **Täitke-siin** umbes **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Samm 3: Kasutamine ADLStoreClient faili ja kataloogi toimingute tegemiseks

Alljärgnev kood sisaldab mõningaid levinud toiminguid, näiteks pikad. Saate vaadata täielik [andmete Lake poe Java SDK API dokumentide](https://azure.github.io/azure-data-lake-store-java/javadoc/) **ADLStoreClient** objekti näha muid toiminguid.
 
Pange tähele, et faile lugeda ja kirjutada, kasutades standard Java voogu. See tähendab, et saate kiht mis tahes Java voogu peal Lake andmesalve voogu kasu standard Java funktsioone (nt Prindi voogu vormindatud väljund, või mis tahes tihendamise või krüptimise voogu lisafunktsioone peal, jne.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Samm 4: Koostamine ja käivitage rakendus

1. Käivitada seest on IDE, otsige üles ja klõpsake nuppu **Käivita** . Kasutage Maven käitamiseks [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Andes autonoomse jar, mida saate käitada käsurea kaudu ehitamine purki kõik sõltuvused sisalduv [Maven komplekti lisandmooduli](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html)abil. Pom.xml [näide lähtekoodi github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) on näide sellest, kuidas seda teha.


## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
