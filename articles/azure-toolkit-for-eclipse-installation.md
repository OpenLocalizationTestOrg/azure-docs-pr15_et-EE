<properties
    pageTitle="Azure'i tööriistakomplekti installimisel Eclipse | Microsoft Azure'i"
    description="Siit saate teada, kuidas installida Azure tööriistakomplekt Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure'i tööriistakomplekt Eclipse installimine

Azure'i tööriistakomplekt Eclipse pakub mallide ja funktsioone, mis võimaldavad kerge vaevaga luua, arendamise, katsetamine ja juurutada Azure rakendustes, mis kasutavad Eclipse arenduskeskkond. Azure'i tööriistakomplekt Eclipse on avatud allika projektiga, mille lähtekood on saadaval jaotises MIT litsentsi Projektisaidi github järgmine URL:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Järgmised sammud näitavad, kuidas installida Azure tööriistakomplekt Eclipse.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Azure'i tööriistakomplekt Eclipse installimiseks

1. Käivitage Eclipse.

1. Eclipse avanemisel klõpsake menüü **Spikker** ja seejärel klõpsake käsku **Installi uus tarkvara**, nagu on näidatud järgmisel joonisel.

    ![Azure'i tööriistakomplekt Eclipse installimine][01]

1. Tippige dialoogiboksi **Saadaval tarkvara** tekstiväljal **töötamine** **http://dl.microsoft.com/eclipse** , millele järgneb **sisestusklahvi (Enter** ).

1. **Nime** paanil kontrollida **Azure'i tööriistakomplekt Eclipse**ja tühjendage **kõik värskendada saidid ajal kontakti installida vajalik tarkvara leidmiseks**. Ekraanil kuvatakse järgmine:

    ![Azure'i tööriistakomplekt Eclipse installimine][02]

1. **Azure'i tööriistakomplekt Eclipse**laiendamisel kuvatakse järgmised üksused:

    * **Rakenduse ülevaateid lisandmooduli Java**: see osa, mis võimaldab kasutada Azure telemeetria logimine ja analüüsi oma rakendused ja teenused serveri eksemplari.
    * **Azure'i juurdepääsu juhtimine teenuste Filter**: See komponent toetab rakenduse kasutajad Azure ACS autentimine, lubada ühekordse sisselogimise stsenaariumid ja externalizing identiteedi loogika rakenduse kaudu.
    * **Azure'i levinud lisandmoodul**: See komponent pakub vajalikud muude komponentide tööriistakomplekt levinud funktsioonid.
    * **Azure'i Exploreri Eclipse**: See komponent pakub vajalikud muude komponentide tööriistakomplekt levinud funktsioonid.
    * **Azure'i lisandmooduli Eclipse Java**: See komponent toetab arendamise projektid, mis aitavad luua, testige ja juurutada taotlused Microsoft Azure'i pilveteenuste Eclipse ja käsurea kaudu.
    * **Azure'i Web Appsi lisandmoodul Java**: See komponent toetab Java veebirakenduste ümbriste Microsoft Azure'i Web Appi kasutamise kohta.
    * **SQL Server Microsoft JDBC draiveri 4.2**: See komponent pakub JDBC API SQL serveri ja Microsoft Azure'i SQL-andmebaasi Java platvormi ettevõtte väljaanne 8.
    * **Paketi Apache Qpid kliendi teekide jaoks JMS**: See komponent pakub JMS kliendi komponent Apache Qpid projekti lubamiseks rakenduse kasutamiseks AMQP sõnumside Microsoft Azure.
    * **Microsoft Azure'i teekide java pakett**: See komponent annab API-d juurdepääsu Microsoft Azure'i teenuseid, näiteks salvestusruumi, teenuse siini, teenuse käitusaja jne.

1. Klõpsake nuppu **edasi**. (Kui tööriistakomplekt installimisel tekib ebatavalised viivitused, veenduge, et **kõik värskendada saidid ajal kontakti installida vajalik tarkvara leidmiseks** on märkimata.)

1. Klõpsake dialoogiboksis **Installida üksikasjad** nuppu **Järgmine**.

    ![Vaadake üle installimisjuhised Alustusjuhendi][03]

1. Litsentsi lepingute tingimused läbi vaadata dialoogiboksis **Läbivaatus litsentside** . Kui nõustute litsentsi kokkulepete tingimusi, klõpsake nuppu **nõustun litsentsi lepingute tingimustega** ja klõpsake siis nuppu **valmis**. (Ülejäänud juhised endale nõustumine lepingute litsentsi. Kui nõustute litsentsi kokkulepete tingimusi, sulgege installitoimingut.)

    ![Läbivaatus litsentside][04]

    Eclipse on alla ja installige vajalikud paketid.

    ![Installimise edenemine][05]

1. Kui teil palutakse taaskäivitage Eclipse installimise lõpuleviimiseks, klõpsake nuppu **Jah**.

    ![Taaskäivitage küsimus][06]

## <a name="see-also"></a>Vt ka

Vaadake lisateavet Azure tööriistakomplektid Java interaktiivset järgmisi linke:

- [Azure'i tööriistakomplekt Eclipse]
  - *Azure'i tööriistakomplekti installimisel Eclipse (selles artiklis)*
  - [Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]
  - [Mis on uut rakenduses Azure tööriistakomplekt Eclipse]
- [Azure'i tööriistakomplekt huvitav jaoks]
  - [Azure'i tööriistakomplekt huvitav installimisega]
  - [Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]
  - [Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

<!-- URL List -->

[Azure'i tööriistakomplekt Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure'i tööriistakomplekt huvitav jaoks]: ./azure-toolkit-for-intellij.md
[Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Azure'i tööriistakomplekt huvitav installimisega]: ./azure-toolkit-for-intellij-installation.md
[Mis on uut rakenduses Azure tööriistakomplekt Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks]: ./azure-toolkit-for-intellij-whats-new.md

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

