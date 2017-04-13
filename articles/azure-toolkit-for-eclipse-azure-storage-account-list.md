<properties
    pageTitle="Azure'i salvestusruumi kontode loend"
    description="Azure'i tööriistakomplekt kasutamise Eclipse salvestusruumi Kontosätete haldamine"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Azure'i salvestusruumi kontode loend #

Azure'i salvestusruumi kontod lubada allalaadimine asukohad, kus saate kasutada oma JDK, rakenduste serveri ja suvalise komponendid, samuti talletamiseks olek kasutamisel vahemällu. Eclipse säilitab loendit on oma projekte oma Eclipse tööruumis teadaolevad salvestusruumi kontod. **Salvestusruumi kontod** dialoogiboks, mille abil saate hallata selle loendi sees Eclipse, avamiseks klõpsake **akna**, käsku **eelistused**, laiendage **Azure**ja klõpsake **Salvestusruumi kontod**.

Järgmisel joonisel on kujutatud dialoogiboks **Salvestusruumi kontod** .

![][ic719496]

Selle dialoogiboksi saate avada ka **kontod** lingil dialoogibokside puhul kasutada salvestusruumi kontod, nt mõni järgmistest:

* **Serveri konfiguratsiooni** dialoogiboksi vahekaarti **JDK** .
* **Serveri konfiguratsiooni** dialoogiboksi vahekaarti **Server** .
* **Lisage komponent** dialoogiboks.
* **Vahemälu** dialoogiboks atribuudid.

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Importida oma salvestusruumi kontod abil avalda sätete fail ##

1. Sees dialoogiboksis **Salvestusruumi kontod** nuppu **Impordi failist AVALDA-sätted**.
2. (Selle juhise vahele jätta juba salvestatud avalda sätete fail oma kohalikus arvutis.) Klõpsake dialoogiboksis **Impordi tellimuse teave** **AVALDA-sätted faili alla laadida**. Kui te pole veel Azure'i kontosse sisse logitud, palutakse teil sisse logida. Seejärel palutakse Salvesta on Azure avaldada sätete fail. (Võite ignoreerida tulemuseks juhiseid sisselogimise lehti – need on Azure portaalis esitatud ja on mõeldud Visual Studio kasutajatele.) Salvestage see oma arvutisse.
3. Endiselt dialoogis **Tellimuse teabe** , klõpsake nuppu **Sirvi** , valige avalda sätete fail, mille varem salvestatud kohalikult ja klõpsake nuppu **Ava**.
4. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Impordi tellimuse teave** .

## <a name="to-create-a-new-storage-account"></a>Mäluruumi uue konto loomine ##

1. Sees dialoogiboksis **Salvestusruumi kontod** nuppu **Lisa**.
2. Sees dialoogiboksis **Salvestusruumi konto lisamine** nuppu **Uus**.
3. Sees **Uue salvestusruumi konto** dialoogiboksis Määrake väärtused järgmist.
    * Salvestusruumikonto nimi.
    * Salvestusruumi konto asukoht.
    * Salvestusruumi konto kirjeldus.
    * Tellimus, kuhu salvestusruumi konto kuulub.
4. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Uue salvestusruumi konto** .

Võib kuluda mitu minutit salvestusruumi konto luua. Pärast selle loomist sulgemiseks klõpsake nuppu **OK** dialoogiboksis **Konto salvestusruumi lisamine** ja salvestusruumi kontosse lisatakse saadaolevat kontode loend.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Olemasoleva salvestusruumi konto lisamine loendisse ##

1. Kui teil on juba Azure storage konto, luua on **luua uus jaotis salvestusruumi konto** eespool toodud juhiseid järgides. (Teise võimalusena saate luua uue konto salvestusruumi [Azure'i haldusportaal][].)
2. Sees dialoogiboksis **Salvestusruumi kontod** nuppu **Lisa**.
3. Sisestage sees dialoogiboksis **Konto salvestusruumi lisamine** **nimi** ja **Kiirklahv**väärtused. Konto nimi ja Accessi võti peab olema Azure storage konto. **Salvestusruumi** jaotise [Azure haldusportaali][] abil saate vaadata oma salvestusruumi konto nimed ja võtmed. Oma **Konto salvestusruumi lisamine** dialoogiboksi sarnanema järgmisega.

    ![][ic719497]

4. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Konto lisamine salvestusruumi** .

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Muuta salvestusruumi konto kasutamiseks Uus kiirklahv ##

1. Klõpsake dialoogiboksis **Salvestusruumi kontod** sees salvestusruumi konto, mida soovite redigeerida, ja seejärel klõpsake nuppu **Redigeeri**.
2. Dialoogiboksis **Redigeeri salvestusruumi konto kiirklahv** sees muuta **Võti** väärtus.
3. Sulgemiseks klõpsake nuppu **OK** dialoogiboksis **Redigeeri salvestusruumi konto võti** .

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Säilitatakse Eclipse loendist salvestusruumi konto eemaldamine ##

1. Klõpsake dialoogiboksis **Salvestusruumi kontod** sees salvestusruumi konto, mida soovite redigeerida, ja seejärel klõpsake käsku **Eemalda**.
2. Klõpsake nuppu **OK** , kui teil palutakse salvestusruumi konto eemaldada.

>[AZURE.NOTE] Salvestusruumi konto **Salvestusruumi kontod** dialoogiboksi abil eemaldada ainult eemaldab selle salvestusruumi kontod vaadatav sees Eclipse loendit. See ei eemalda Azure tellimuselt salvestusruumi konto. Lisaks salvestusruumi konto võib uuesti loendis kuvatavate pärast Eclipse laadib uuesti oma tellimuse üksikasjad.

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure'i haldusportaal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
