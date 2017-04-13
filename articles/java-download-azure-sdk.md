<properties 
    pageTitle="Azure'i SDK java allalaadimine" 
    description="Saate teada, kuidas alla laadida Azure'i SDK Java, proovi kood Azure'i Tookit Eclipse Maven projektide ja installimise põhilised juhised ette." 
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

# <a name="download-the-azure-sdk-for-java"></a>Azure'i SDK java allalaadimine

Käesolevast artiklist leiate juhised allalaadimist ja installimist Azure teekide Java.

**Märkus:** Azure'i teekide Java on jaotatud [Apache'i, versioon 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure'i teekide Java - käsitsi allalaadimine

Käsitsi installida Azure teekide Java, klõpsake <http://go.microsoft.com/fwlink/?LinkId=690320> alla ZIP-fail, mis sisaldab kõiki teekide ja kõik sõltuvused.

Kui olete oma arvutisse alla zip-fail, sisu ekstraktimiseks ja kasutage ühte järgmistest suvanditest JAR failide lisamine projekti:

* Saate importida Eclipse Java projekti JAR failid.

* Konfigureerida Java projekti **Koostada tee** Eclipse tee JAR faile lisada.

Koosta teed Eclipse häälestamise kohta leiate üksikasjalikumat teavet artiklist [Java koostada tee] Eclipse veebisaidil.

**Märkus:** Vt license.txt ja ThirdPartyNotices.txt fail ZIP sees litsents ja muud teavet.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure'i teekide Java - hoone Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Samm 1 - häälestada oma projekti Maven kasutamiseks koostamine

Luua Maven projektide Eclipse kasutada Azure teekide Java, juhiste [Alustamine: Azure'i haldamine teekide Java] [ maven-getting-started] artikkel. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Samm 2 – nõutav sõltuvusi Maven sätete konfigureerimine

Kui Koosta Maven kasutamiseks on konfigureeritud projekti, saate lisada soovitud vajalikud sõltuvused süntaksi kasutamine pom.xml faili nagu järgmises näites. Pange tähele, et teil pole vaja lisada iga sõltuvus, mis on loetletud Järgnevas näites; peate ainult teatud sõltuvused, mis nõuab projekti lisama.

> [AZURE.NOTE] Iga `<version>` elemendi järgmised valimis, asendage "n.n.n" kohatäited selles näites arvudega kehtiv versioon, mida saab [Azure'i teekide hoidla Maven kohta].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure'i tööriistakomplekt Eclipse installimine

Sellest jaotisest leiate põhilised juhised installimist Azure tööriistakomplekt Eclipse; üksikasjalikud juhised leiate teemast [Azure tööriistakomplekt Eclipse installimine].

### <a name="prerequisites"></a>Eeltingimused

1. [Mis on uut rakenduses Azure'i tööriistakomplekt Eclipse] artiklis toodud Windows operting süsteemid.
1. Macintoshi või Linuxi süsteemide operting [mis on uut rakenduses Azure'i tööriistakomplekt Eclipse] artiklis toodud.
1. Eclipse IDE Java EE arendajatele Indigo või uuem versioon. See saab alla laadida <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Installi põhitoimingud

1. Valige Eclipse, klõpsake menüü **Spikker** **Installi uus tarkvara**.
1. Sisestage asukoht saidi <http://dl.microsoft.com/eclipse> ja vajutage sisestusklahvi **Enter**.
1. Valige eksporditavad üksused olema installitud ja klõpsake nuppu **valmis**.

Azure'i tööriistakomplekt Eclipse kasutab Azure SDK uusim versioon. See saate alla laadida, kasutades <http://go.microsoft.com/fwlink/?LinkID=252838>Web platvormi Installer (WebPI). Kui teil pole installitud, kui loote esimese Azure juurutamise projekti Azure'i tööriistakomplekt Eclipse automaatselt installida Azure SDK sobiv versioon.

## <a name="see-also"></a>Vt ka

[Azure'i tööriistakomplekt Eclipse]

[Azure'i tööriistakomplekt Eclipse installimine] 

[Tere tulemast maailma rakenduste loomise Azure Eclipse]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i teekide hoidla Maven kohta]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java koostada tee]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Mis on uut rakenduses Azure tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333
