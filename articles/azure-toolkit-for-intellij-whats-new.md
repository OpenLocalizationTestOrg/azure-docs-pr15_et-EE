<properties
    pageTitle="Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks | Microsoft Azure'i"
    description="Teave huvitav Azure'i tööriistakomplekt uusimaid funktsioone."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks

## <a name="azure-toolkit-for-intellij-releases"></a>Azure'i tööriistakomplekt huvitav väljalasete jaoks

See artikkel sisaldab teavet eri väljalasete ja Azure tööriistakomplekt uusimate värskendustega huvitav.

> [AZURE.NOTE] Olemas on ka on Azure tööriistakomplekt Eclipse IDE jaoks. Lisateabe saamiseks lugege teemat [Azure tööriistakomplekt Eclipse].

### <a name="august-26-2016"></a>26 August 2016

Azure'i tööriistakomplekt jaoks huvitav - August 2016 väljaanne sisaldab järgmisi täiustusi.

* **Kohandatud JDK jaotuse**. Azure'i tööriistakomplekt jaoks huvitav toetab nüüd täpsustades ja teenuse juurutamisel oma Azure Web Appis container mõni suvalise JDK versioon.
  - Lisaks JDKs, mis on esitatud Azure, saate ka valida laia valikut kättesaadavaks Azure Azul süsteemide Suulu OpenJDK versioonid.
  - Kui laadite see ZIP-failina salvestusruumi konto, saate määrata ka oma JDK jaotuse.
* **Azure'i Exploreri vaade täiustused**:
  - Virtuaalse masina juhtimine kasutades Azure uue ressursihaldur mudel tugi: loendisse, loomine ja kustutamine ressursi halduri vastavalt virtuaalmasinates IDE lahkumata.
  - Salvestusruumi bloobimälu kontohaldus täiendab olemasolevaid funktsioone haldamise "classic" salvestusruumi kontod Azure ressursihaldur kasutamise tugi.
* **Microsoft JDBC draiveri SQL serveri 6.0**. Selle värskenduse sisaldab JDBC uusimad draiverid Microsoft SQL Server (v6.0), mis on nüüd sisalduv nimega teegis, mida saate hõlpsasti lisada oma Java projektide, asendades varasem versioon.

### <a name="june-29-2016"></a>29 juuni 2016

Azure'i tööriistakomplekt jaoks huvitav - juuni 2016 väljaanne sisaldab järgmisi täiustusi.

* **Java 8 nõue**. Azure'i tööriistakomplekt huvitav jaoks nõuab nüüd Java 8, kuigi see nõue on ainult tööriistakomplekt - rakenduste edasi kasutada Java kõik versioonid, mida toetavad Azure.
* **Uusim Java JDKs tugi**. Java JDKs uusimad versioonid toetavad kohe tööriistakomplekti Azure'i huvitav.
* **Azure'i SDK v2.9.1 tugi**. Azure'i SDK uusim versioon on nüüd minimaalne eelsete nõutav Azure abivahendi jaoks huvitav jaoks.
* **Integreeritud näidised**. Azure'i tööriistakomplekt jaoks huvitav on nüüd mitu valimi rakendust, et arendajad alustamine.
* **Hdinsightiga tööriista integreerimine**. Azure Hdinsightiga tööriistad on nüüd ühendatud Azure'i tööriistakomplekt huvitav jaoks. Lisateavet leiate teemast [Hdinsightiga tööriistade lisandmooduli jaoks huvitav].
* **Remote silumine Java Web Apps**. Azure'i tööriistakomplekt jaoks huvitav toetab nüüd Java veebirakenduste teenuses Azure rakenduse remote silumine.

### <a name="april-12-2016"></a>12 aprill 2016

Azure'i tööriistakomplekt jaoks huvitav – 2016 aprilli väljaanne sisaldab järgmisi täiustusi.

* **Azure'i SDK v2.9.0 tugi**. Azure'i SDK uusim versioon on nüüd miinimum eelse nõutav Azure abivahendi jaoks huvitav jaoks.
* **Mitmesugused kasutatavuse, Azure Web Appi tugiteabe seotud tundlikkuse ja jõudluse täiustused**. Arvu jõudluse optimeerimise kuidas tööriistakomplekt suhtleb Azure tulemi paindlikumaks UI.
* **Mõne olemasoleva veebirakenduse container Azure huvitav kustutada**. Azure'i tööriistakomplekt jaoks huvitav võimaldab nüüd kustutada mõne olemasoleva Azure Web container huvitav lahkumata.

## <a name="see-also"></a>Vt ka ##

Vaadake lisateavet Azure tööriistakomplektid Java interaktiivset järgmisi linke:

- [Azure'i tööriistakomplekt Eclipse]
  - [Azure'i tööriistakomplekt Eclipse installimine]
  - [Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]
  - [Mis on uut rakenduses Azure tööriistakomplekt Eclipse]
- [Azure'i tööriistakomplekt huvitav jaoks]
  - [Azure'i tööriistakomplekt huvitav installimisega]
  - [Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]
  - *Mis on uut rakenduses Azure tööriistakomplekt jaoks huvitav (selles artiklis)*

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

<!-- URL List -->

[Azure'i tööriistakomplekt Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure'i tööriistakomplekt huvitav jaoks]: ./azure-toolkit-for-intellij.md
[Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Azure'i tööriistakomplekt Eclipse installimine]: ./azure-toolkit-for-eclipse-installation.md
[Azure'i tööriistakomplekt huvitav installimisega]: ./azure-toolkit-for-intellij-installation.md
[Mis on uut rakenduses Azure tööriistakomplekt Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547

[Hdinsightiga tööriistade lisandmooduli jaoks huvitav]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
