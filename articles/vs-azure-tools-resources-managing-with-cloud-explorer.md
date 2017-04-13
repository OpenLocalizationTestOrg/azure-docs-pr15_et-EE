<properties 
   pageTitle="Azure'i ressursside Cloud Exploreriga | Microsoft Azure'i"
   description="Saate teada, kuidas vaadata ja hallata Azure ressursse Visual Studio Cloud Exploreriga."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Azure'i ressursside Cloud Exploreriga haldamine

##<a name="overview"></a>Ülevaade

Cloud Explorer on mõeldud teile kergemini ja kiiremini sirvida ja hallata oma Visual Studio IDE Azure ressursse. Saate, näiteks abil Web Appi [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040) või brauseris avada, või lisada Silur, või saate bloobimälu container atribuutide kuvamine ja avage see bloobimälu Container Editoris.

Pilveteenuse Exploreri põhineb Azure ressursi halduri virnas, nagu [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040). See mõistab nagu Azure ressursi rühmad ja Azure teenused, nt loogika ja API rakendused ja see toetab [Rollipõhine juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md) (RBAC). Azure'i ressursse, mis on lisatud või mida on muudetud vaatamiseks valige Cloud Exploreri tööriistaribal nuppu **Värskenda** .

Cloud Explorer on installitud Visual Studio tööriistad osana Azure'i SDK 2,7. 

## <a name="prerequisites"></a>Eeltingimused

- Visual Studio 2015 RTM.

- Visual Studio tööriistad Azure SDK. 
- Teil peab olema ka Azure'i konto ja olema seda vaadata Azure ressursse Cloud Exploreris sisse logitud. Kui teil pole ühte, saate luua konto vaid paar minutit. Kui teil on MSDN-i tellimuse, leiate [Azure'i kasu MSDN tellijad](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Muul juhul vaadata [tasuta prooviversiooni konto loomine](https://azure.microsoft.com/pricing/free-trial/).

- Kui Cloud Explorer ei kuvata, saate seda vaadata **View**käsku **Muud Windows,** **Cloud Exploreri** menüüriba.

## <a name="manage-azure-accounts-and-subscriptions"></a>Azure'i kontod ja tellimuste haldamine

Oma Azure ressursse Cloud Exploreris kuvamiseks peate ühe või mitme aktiivse tellimuste Azure'i konto sisse logida. Kui teil on rohkem kui üks Azure'i konto, saate lisada need Cloud Explorer ja valige Cloud Explorer Ressursivaade kaasatavate tellimused.

Kui te pole varem kasutanud enne Azure'i või pole vajalikud kontod lisatakse Visual Studios, palutakse teil seda teha.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Azure'i kontod Cloud Explorer lisamiseks

1. Valige tööriistaribal Cloud Exploreri sätted.

1. Valige link **Lisa konto** . Logige sisse Azure'i konto nime, kelle ressursid, mida soovite vaadata. Äsja lisatud kontole valitakse rippmenüüst konto valijas. Konto jaoks tellimused kuvatakse jaotises konto.

    ![Azure'i tellimuste lisamine](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Azure'i tellimused valimine](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Märkige ruudud konto tellimused, mida soovite Sirvi ja seejärel klõpsake nuppu **Rakenda** .

    Azure'i ressursid valitud tellimused kuvatakse Cloud Explorer.

## <a name="to-remove-an-azure-account"></a>Azure'i konto eemaldamine

1. Valige **fail**, klõpsake menüüribal **Konto sätted** .

1. Dialoogiboksis **Konto sätted** jaotises **Kõik kontod** **eemaldada** käsu valimine kõrval konto eemaldada. Teate, et see käsk ainult sel juhul eemaldatakse konto Visual Studio – see ei mõjuta Azure'i konto ise.

## <a name="view-resource-types-or-groups"></a>Ressursi tüüp või rühmade

Vaadata oma Azure ressursse, saate valida, kas **Ressursi tüüpi** või **Ressursside** vaade.

![Ressursside vaade ripploend](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **Ressursi tüübid** , mis on ka levinud vaate, mida kasutatakse [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040), kuvatakse oma Azure ressursse, nt veebirakendusi, salvestusruumi kontod ja virtuaalmasinates tüübi järgi kategoriseeritud. See toiming sarnaneb serveri Exploreris kuvatakse kuidas Azure ressurssidele.

- Ressursivaade rühmad kategooria Azure ressursside need on seostatud Azure ressursirühma.

 
    Ressursirühma on kogum Azure ressursse, tavaliselt kasutavad konkreetse rakenduse. Azure'i ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Saate vaadata ja liikuge ressursid

Azure'i ressursside liikumine ja selle teabe kuvamine Cloud Exploreris, laiendage üksuse tüüp või seotud ressursirühm ja seejärel valige ressursi. Ressursi valimisel kuvatakse teave kahest järgmisest Cloud Explorer allosas.

![Ressursi vaate valimine](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- Vahekaart **toimingud** kuvatakse tehtavad toimingud Cloud Exploreris ressurss. Näete saadaval toimingute kohta kiirmenüü ressursi.

- Ressursi, nt selle tüübi, lokaadi ja ressursside rühmas on seostatud atribuudid kuvatakse **Atribuudid** vahekaardil.

Iga ressursi on **avatud portaalis**toiming. Selle toimingu valimisel kuvab Cloud Explorer valitud ressursi [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040). See funktsioon on eriti mugav liikumine sügavalt pesastatud ressursid.

Täiendavad toimingud ja kinnisvarahindade võidakse kuvada ka Azure ressursside põhjal. Näiteks veebirakenduste ja loogika rakendused on ka **brauseris avatud** ja **manustamine Silur** **avatud portaali**Lisaks toimingud. Toimingud, et avada redaktorid kuvatakse, kui valite salvestusruumi konto bloobimälu, järjekorda või tabel. Azure'i rakendused on **URL-i** ja **olek** atribuudid, samas kui salvestusruumi ressursid on stringi võti ja ühenduse atribuudid.

## <a name="search-resources"></a>Otsingu ressursid

Azure'i konto tellimuste ressursid teatud nimega leidmiseks sisestage nimi Cloud Exploreri otsinguväljale.

![Pilveteenuse Exploreris ressursid otsimine](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Kui märkide sisestamine väljale Otsing, kuvatakse ainult ressursse, mis neid märke vastavad ressursi puu.

