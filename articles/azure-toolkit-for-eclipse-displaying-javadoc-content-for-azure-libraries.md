<properties
    pageTitle="Azure'i teekide paketi Java Eclipse Javadoc sisu kuvamine"
    description="Kuidas kuvada Javadoc sisu on Eclipse Azure'i teekide jaoks."
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Azure'i teekide paketi Java Eclipse Javadoc sisu kuvamine #

Azure'i teekide Java Javadoc sisu saab vaadata oma Eclipse keskkonnas Javadoc sisu Azure'i teekide Java seostamise kaudu. Järgmised toimingud näitavad, kuidas kasutada seda Eclipse funktsioonile.

See toiming eeldab, et olete lisanud juba oma Koosta tee Azure'i teek Java.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Azure'i teekide Java Eclipse Javadoc sisu kuvamiseks ##

* Eclipse's Project Exploreri, projekti jaotises **Teegid viidatud** avada kontekstimenüü Azure'i teegi Java JAR. Näiteks **microsoft windowsazure-api 0.1.0.jar** (versiooninumber võib olla erinev, sõltuvad sellest, millist versiooni installitud).
* Klõpsake nuppu **Atribuudid**.
* Klõpsake vasakpoolsel paanil dialoogiboks **Atribuudid** jooksul **Javadoc kohta**. Kuvatakse dialoogiboks **Javadoc asukoht** .
* Saate määrata **Javadoc URL-i**või mõne **Javadoc arhiivi**.
    * Kui soovite määrata **Javadoc URL-i**, saate kasutada URL-id, nt **http://dl.windowsazure.com/javadoc** või **http://dl.windowsazure.com/storage/javadoc**.
    * Kui otsustate kasutada **Javadoc arhiivi**, saate määrata välise faili või tööruumifail.
    Tehke valik ja Sirvi/kinnitada vastavalt vajadusele. Järgmises näites seostab Azure'i teekide Java on vastava Javadoc JAR alla laaditud kohalik kaust nimega **c:\MyAzureJARs**.
    ![][ic553487]
* *Valikuline*: klõpsake nuppu **Valideeri**. Võimalikud probleemid Javadoc JAR saanud siin kuvada.
* Klõpsake nuppu **OK**.

Kui seostatud teeki, Javadoc sisu peaks olema kuvatud teie Eclipse IDE sees. Näiteks kui `blob` on määratletud tüüp `CloudBlockBlob` oma koodi järgmine on näide Javadoc sisu, mis kuvatakse siis, kui tipite `blob.acquireLease` kood:

![][ic553488]

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png