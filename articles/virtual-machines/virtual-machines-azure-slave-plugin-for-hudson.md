<properties
    pageTitle="Azure'i ori lisandmooduli kasutamise Hudson pidev integreerimise | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse Hudson pidev integreerimine Azure ori lisandmooduli abil."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Kuidas kasutada Azure ori lisandmooduli Hudson pidev integratsiooniga

Azure'i ori lisandmooduli Hudson võimaldab ettevalmistamise ori sõlmed Azure, kui töötab jaotatud koostab.

## <a name="install-the-azure-slave-plug-in"></a>Azure'i ori lisandmooduli installimine

1. Klõpsake armatuurlaual Hudson **Hudson haldamine**.

1. Klõpsake lehel **Halda Hudson** **Lisandmoodulite haldamine**.

1. Klõpsake vahekaarti **saadaval** .

1. Klõpsake nuppu **Otsi** ja tippige **Azure'i** oluline lisandmoodulite loendi piiritlemiseks.

    Kui valite loendist Saadaolevad lisandmoodulid, leiate Azure'i ori lisandmooduli jaotises **kobar haldus ja jaotatud koostada** **teistele** menüüs.

1. Märkige ruut **Azure'i ori**lisandmooduli.

1. Klõpsake nuppu **Installi**.

1. Taaskäivitage Hudson.

Nüüd, kui lisandmoodul on installitud, oleks järgmiste juhiste juurde ja Azure tellimuse profiili lisandmooduli konfigureerimine ja luua malli, mida kasutatakse loomisel ori sõlme VM.

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

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Kui olete tellimuse profiili, järgige neid juhiseid lisandmooduli Azure ori konfigureerimine.

1. Klõpsake armatuurlaual Hudson **Hudson haldamine**.

1. Klõpsake **süsteemi konfigureerimine**.

1. Kerige lehel jaotises **Cloud** leida.

1. Klõpsake **lisada uue cloud > Microsoft Azure'i**.

    ![Lisage uus pilveteenuses][add new cloud]

    See näitab väljad, mille peate sisestama oma tellimuse üksikasjad.

    ![profiili konfigureerimine][configure profile]

1. Tellimuse profiili tellimuse id ja juhtimise sertifikaadi kopeerida ja kleepida need vastavatele väljadele.

    Tellimuse id ja juhtimise serti kopeerimisel **ei** kaasata hinnapakkumised, mis ümbritsege väärtused.

1. Klõpsake nuppu **Kinnita konfigureerimise**kohta.

1. Kui konfiguratsioon on edukalt kinnitatud, klõpsake nuppu **Salvesta**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Virtuaalse masina malli seadistamine Azure'i ori lisandmoodul

Malli virtuaalse masina määratleb lisandmooduli kasutada, et luua ori sõlm Azure parameetrid. Järgmistes juhistes saadame luua malli jaoks soovitud Ubuntu VM.

1. Klõpsake armatuurlaual Hudson **Hudson haldamine**.

1. Klõpsake **süsteemi konfigureerimine**.

1. Kerige lehel jaotises **Cloud** leida.

1. **Pilveteenuse** jaotise **Lisamine Azure virtuaalse masina malli** otsimine ja klõpsake nuppu **Lisa** .

    ![vm malli lisamine][add vm template]

1. Määrake pilvepõhise teenuse nimi väljale **nimi** . Kui teie määratud nimi viitab mõne olemasoleva pilveteenuses, ei VM teenuses ette valmistatud. Muul juhul kuvatakse Azure'i uue konto loomiseks.

1. Sisestage väljale **Kirjeldus** loodava malli kirjeldav tekst. See teave on mõeldud ainult dokumentide kasutamiseks ja ei kasutata ettevalmistamise VM.

1. Sisestage väljale **siltide** **linux**. Sildi loodava malli tuvastamiseks ja kasutatakse edaspidi viitamiseks malli Hudson töö loomisel.

1. Valige ala, kus luuakse VM.

1. Valige sobiv suurus VM.

1. Määrake salvestusruumi konto, kus luuakse VM. Veenduge, et see on sama piirkonna kasutate pilveteenusesse. Kui soovite luua uue salvestusruumi, saate jätke see väli tühjaks.

1. Säilituspoliitika aja määrab enne Hudson kustutate mõne jõude ori minutite arv. Jätke see vaikimisi väärtus 60.

1. **Kasutus**, valige vastav tingimus, kui kasutatakse selle ori sõlme. Nüüd, valige **selle sõlme võimalikult palju Utilize**.

    Selles etapis teie vormi näeks mõnevõrra sarnane:

    ![malli config][template config]

1. **Pildi pere** või Id teil määrata, millised süsteemipildi installitakse teie VM. Saate valida loendist Pildi peredele, või määrata kohandatud pildi.

    Kui soovite valida loendist Pildi peredele, sisestage perekonnanimi pilt (tõstutundlik) esimene märk. Näiteks tippige **U** avab Ubuntu Server peredele loendit. Kui valite loendist, kasutatakse Jenkins kui ettevalmistamise oma VM selle süsteemipildi selle pere uusim versioon.

    ![OS pere loend][OS family list]

    Kui teil on kohandatud pilt, mida soovite kasutada, selle asemel, sisestage nimi, et kohandatud pilt. Kohandatud pildi nimed ei kuvata loendis nii, et teil on tagada, et nimi on õigesti sisestatud.    

    Tippige selle õpetuse **U** Ubuntu piltide loend ja valige **Ubuntu Server 14.04 LTS**.

1. **Käivitage meetod**, valige **SSH**.

1. Allpool skripti kopeerida ja kleepida **käivitamise skripti** välja.

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

    **Käivitamise skript** käivitatakse VM loomise järel. Selles näites skripti installib Java, git ja ant.

1. Väljad **kasutajanimi** ja **parool** , sisestage oma VM loodud administraatorikonto oma eelistatud väärtused.

1. Klõpsake **Malli veenduge,** et kontrollida, kas teie määratud parameetrid on lubatud.

1. Klõpsake **Salvesta**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Hudson töö, mis töötab ori sõlm Azure loomine

Selles jaotises kuvatakse loomise Hudson tööülesande ori sõlme Azure töötavad.

1. Klõpsake armatuurlaual Hudson **Uus töökoht**.

1. Sisestage nimi töö loomisel.

1. Töö tüüp, valige **koostamine - laadi tarkvara tööd**.

1. Klõpsake nuppu **OK**.

1. Valige lehel töö konfiguratsiooni **piira, kus saate käivitada selle projekti**.

1. Valige **sõlm ja sildi menüü** ja valige **linux** (me sildi loomisel määrata virtuaalse masina malli eelmises jaotises).

1. **Koostamine** jaotises käsku **lisa koostada samm** ja valige **Käivita shell**.

1. Järgmise skripti, asendades **{github konto nime}**, **{projekti nime}**ja **{oma projekti directory}** väärtused, redigeerida ja teksti ala, mis kuvatakse redigeeritud kirjutuskaitstuks.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Klõpsake **Salvesta**.

1. Otsige Hudson armatuurlaual saate just loodud ja klõpsake ikooni **ajakava ehitada** töö.

Hudson seejärel ori sõlm, loonud eelmises jaotises malli abil saate luua ja käivitada skripti Koosta juhises selle tööülesande määratud.

## <a name="next-steps"></a>Järgmised sammud

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/
[tellimuse profiil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

