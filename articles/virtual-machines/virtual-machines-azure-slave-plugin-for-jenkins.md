<properties
    pageTitle="Azure'i ori lisandmooduli kasutamise Jenkins pidev integreerimise | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse Jenkins pidev integreerimine Azure ori lisandmooduli abil."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Kuidas kasutada Azure ori lisandmooduli Jenkins pidev integratsiooniga

Saate lisandmooduli Azure ori Jenkins sätte ori sõlmed Azure kui töötab jaotatud koostab.

## <a name="install-the-azure-slave-plug-in"></a>Azure'i ori lisandmooduli installimine

1. Klõpsake armatuurlaual Jenkins **Jenkins haldamine**.

1. Klõpsake lehel **Halda Jenkins** **Lisandmoodulite haldamine**.

1. Klõpsake vahekaarti **saadaval** .

1. Tippige väljale filtri ülaltoodud loendis Saadaolevad lisandmoodulid **Azure'i** oluline lisandmoodulite loendi piiritlemiseks.

    Kui valite loendist Saadaolevad lisandmoodulid, leiate Azure'i ori lisandmooduli jaotises **kobar haldus ja jaotatud koostamine** .

1. Märkige ruut **Azure'i ori lisandmoodul** .

1. Klõpsake **ilma uuesti installimine** või **allalaadimine ja installimine pärast uuesti**.

Nüüd, kui lisandmoodul on installitud, on järgmised toimingud konfigureerida lisandmoodulit Azure tellimuse profiili ja virtuaalse masina ori sõlme loomisel kasutatava malli loomine.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Tellimuse profiili ja Azure ori lisandmooduli konfigureerimine

Tellimuse profiili, nimetatakse ka avaldada sätted on turvaline mandaadid ja peate oma arenduskeskkond Azure'i töötada täiendavat teavet sisaldav XML-faili. Azure'i ori lisandmooduli konfigureerimiseks peate.

* Teie tellimuse id
* Tellimuse serdi haldus

Need leiate oma [tellimuse profiili]. Allpool on kujutatud tellimuse profiili.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Kui olete oma tellimuse profiili, tehke järgmist konfigureerida Azure ori lisandmooduli.

1. Klõpsake armatuurlaual Jenkins **Jenkins haldamine**.

1. Klõpsake **süsteemi konfigureerimine**.

1. Kerige lehel jaotises **Cloud** leida.

1. Klõpsake **lisada uue cloud > Microsoft Azure'i**.

    ![pilveteenuse jaotis][cloud section]

    See näitab väljad, mille peate sisestama oma tellimuse üksikasjad.

    ![tellimuse konfigureerimine][subscription configuration]

1. Tellimuse profiili tellimuse id ja juhtimise sertifikaadi väärtused kopeerida ja kleepida need vastavatele väljadele.

    Tellimuse id ja haldus serdi kopeerimisel ei sisalda hinnapakkumised, mis ümbritsege väärtused.

1. Klõpsake **konfiguratsiooni kinnitamine**.

1. Kui õige on kinnitatud konfiguratsiooni, klõpsake nuppu **Salvesta**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Virtuaalse masina malli seadistamine Azure ori lisandmoodul

Malli virtuaalse masina määratleb lisandmooduli abil luua ori sõlm Azure parameetrid. Järgmistes juhistes loome malli jaoks soovitud Ubuntu virtuaalse masina.

1. Klõpsake armatuurlaual Jenkins **Jenkins haldamine**.

1. Klõpsake **süsteemi konfigureerimine**.

1. Kerige lehel jaotises **Cloud** leida.

1. **Pilveteenuse** jaotise **Lisamine Azure virtuaalse masina malli**otsimine ja seejärel klõpsake nuppu **Lisa**.

    ![vm malli lisamine][add vm template]

    See näitab, kuhu sisestada loodava malli üksikasjade väljad.

    ![tühi üldine konfigureerimine][blank general configuration]

1. Sisestage väljale **nimi** Azure pilveteenuse teenuse nimi. Kui teie sisestatud nimi viitab mõne olemasoleva pilveteenuses, ei virtuaalse masina teenuses ette valmistatud. Muul juhul kuvatakse Azure'i uue konto loomiseks.

1. Sisestage väljale **Kirjeldus** loodava malli kirjeldav tekst. See kehtib ainult kirjete ja ei kasutata ettevalmistamise virtuaalse masina.

1. Väljale **veerusildid** loodava malli tuvastamiseks ja kasutatakse edaspidi viitamiseks malli Jenkins töö loomisel. Põhjus, sisestage sellele väljale **linux** .

1. Klõpsake loendis **piirkond** piirkond, kus luuakse virtuaalse masina.

1. Klõpsake loendis **Virtuaalse masina suurus** sobiv suurus.

1. Määrake väljal **Salvestusruumikonto nimi** salvestusruumi konto, kus luuakse virtuaalse masina. Veenduge, et see on sama piirkonna kasutate pilveteenusesse. Kui soovite luua uue salvestusruumi, saate jätke see väli tühjaks.

1. Säilituspoliitika aja määrab enne Jenkins kustutate mõne jõude ori minutite arv. Jätke see vaikimisi väärtus 60. Saate valida ka ori kustutamise asemel hoopis, kui see on jõudeolekus sulgeda. Selleks, et ruut **Sulgumist ainult (ärge kustutage) pärast säilitamise aeg** .

1. Klõpsake loendis **kasutamise** korral tingimus, kui selle ori sõlme kasutatakse. Nüüd, klõpsake **selle sõlme võimalikult palju Utilize**.

    Selles etapis teie vorm peaks välja nägema mõnevõrra umbes järgmine:

    ![kontrollpunkt üldine malli config][checkpoint general template config]

    Järgmiseks tuleb opsüsteemi pilti, mida soovite luua oma orja üksikasjad.

1. **Pildi pere või Id** väljale teil määrata, millised süsteemipildi installitakse teie arvutisse virtual. Saate valida loendist Pildi peredele, või määrata kohandatud pildi.

    Kui soovite valida loendist Pildi peredele, sisestage perekonnanimi pilt (tõstutundlik) esimene märk. Näiteks tippige **U** avab Ubuntu Server peredele loendit. Valige loendist, kasutatakse Jenkins kui ettevalmistamise virtual arvuti selle süsteemipildi selle pere uusim versioon.

    ![OS pilt loendi näidis][OS Image list sample]

    Kui teil on kohandatud pilt, mida soovite kasutada, selle asemel, sisestage nimi, et kohandatud pilt. Kohandatud pildi nimed ei kuvata loendis nii, et teil on tagada, et nimi on õigesti sisestatud.

    Selles õpetuses tippige **U** avab Ubuntu piltide loendit ja klõpsake **Ubuntu Server 14.04 LTS**.

1. Klõpsake loendis **Käivita meetod** **SSH**.

1. Allpool skripti kopeerige ja kleepige see väljale **Käivitamise skript** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Käivitamise skript käivitatakse virtuaalse masina loomise järel. Selles näites skripti installib Java, Git ja ant.

1. **Kasutajanime** ja **parooli** kinnitus parool, sisestage oma eelistatud väärtused virtual arvutis loodud administraatorikonto.

1. Klõpsake **Malli kinnitamine** , et kontrollida, kas teie määratud parameetrid on lubatud.

1. Klõpsake nuppu **Salvesta**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Jenkins töö, mis töötab ori sõlm Azure loomine

Selles jaotises kuvatakse loomise Jenkins tööülesande ori sõlme Azure töötavad. Peate olema oma projekti github jälgida mööda.

1. Jenkins armatuurlaual nuppu **Uus üksus**.

1. Sisestage loote tööülesande nimi.

1. Projekti tüüp, klõpsake **Freestyle projekti**.

1. Klõpsake nuppu **Ok**.

1. Valige lehel tööülesande konfigureerimine **piira, kus saab käitada selle projekti**.

1. Sisestage väljale **Sildi avaldis** **linux**. Eelmises jaotises lõime me nimega **linux**, mis on, mis määrame ära siin ori malli.

1. **Koostamine** jaotises käsku **lisa koostada samm** ja valige **Käivita shell**.

1. Järgmise skripti, asendades **(GitHub konto nimi)**, **(projekti nime)**ja **(oma projekti directory)** väärtused, redigeerida ja teksti ala, mis kuvatakse redigeeritud kirjutuskaitstuks.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Klõpsake nuppu **Salvesta**.

1. Jenkins armatuurlaual kursorit äsja loodud tööülesanne ja klõpsake rippnoolt tööülesande suvandite kuvamiseks.

1. Klõpsake **nüüd koostada**.

Jenkins seejärel eelmises tööetapis loodud malli abil luua ori sõlm ja Koosta juhises selle tööülesande määratud skripti käivitada.

## <a name="next-steps"></a>Järgmised sammud

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/
[tellimuse profiil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png