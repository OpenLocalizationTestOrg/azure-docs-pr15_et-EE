<properties 
   pageTitle="Azure'i rakendus viisard avaldamine | Microsoft Azure'i"
   description="Azure'i rakendus viisard avaldamine"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Azure'i rakendus viisard avaldamine

## <a name="overview"></a>Ülevaade

Kui teil tekib veebirakenduse Visual Studios, saate avaldada rakenduse hõlpsam on Azure pilveteenuses **Avaldada Azure taotluse** viisardi abil. Esimene jaotis selgitab juhiseid tuleb enne, kui kasutate viisard ja ülejäänud jaotistes kirjeldatakse funktsioone viisardi lõpuleviimine.

>[AZURE.NOTE] See teema on juurutamise pilveteenustega ei veebisaitide abil. Teenuse juurutamisel veebisaitide kohta leiate teavet teemast [juurutamise on Azure veebisaidi](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Eeltingimused

Azure'i veebirakenduses avaldamiseks peab teil olema Microsofti konto ja Azure tellimuse ja teil on oma veebirakenduse seostada on Azure pilveteenuses. Kui olete need tööülesanded juba lõpetanud, võite jätkata järgmise jaotise juurde.

1. Microsofti konto ja Azure tellimuse hankimine. Võite proovida tasuta üks kuu tasuta Azure'i tellimus [siin](https://azure.microsoft.com/pricing/free-trial/)

1. Azure'i pilveteenus ja salvestusruumi konto luua. Seda saab teha Server Explorer Visual Studio või [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)kaudu.

1. Lubage oma veebirakenduse Azure. Visual Studio Azure avaldada oma veebirakenduse lubamiseks peate seostada Azure pilveteenuse teenuse projekti Visual Studios. Seotud pilvepõhise teenuse projekti loomiseks oma veebirakenduse projekti kiirmenüü avamine ja valige Teisenda, **teisendamine Azure pilveteenuse teenuse Project**.

1. Pärast pilvepõhise teenuse project lisatakse teie lahendus, uuesti sama kiirmenüü avamine ja klõpsake nuppu **Avalda**. Rakenduste Azure lubamise kohta leiate lisateavet teemast [kohta: migreerimine ja avaldada veebirakenduse on Azure pilveteenusesse Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Ärge unustage Visual Studio alustamine administraatori identimisteave (Käivita administraatorina).

1. Kui olete rakenduse avaldada, Azure pilveteenuse teenuse projekti kiirmenüü avamine ja klõpsake nuppu **Avalda**. Järgmised toimingud Kuva avaldada Azure taotluse viisard.

## <a name="choosing-your-subscription"></a>Tellimuse valimine

### <a name="to-choose-a-subscription"></a>Tellimuse valimiseks

1. Enne viisardi kasutada esimest korda, peate sisse logima. Valige link **Logi sisse** . Küsimisel Azure portaali sisse logida ja esitada oma Azure kasutajanimi ja parool. 

    ![See on üks publishing wizard ekraanid](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    Tellimuste loendi kuvab teie kontoga seostatud tellimustega. Võite näha tellimused tellimust faile varem imporditud.

1. Valige loendis **Valige tellimuse** tellimuse selle juurutamiseks kasutada.

   Kui valite **< Halda >**, kuvatakse dialoogiboks **Tellimuste haldamine** ja soovi korral saate tellimust ja kasutajale konto, mida soovite kasutada. Vahekaardil **kontod** kuvatakse kõigi kontode ja **tellimuste** menüü kuvatakse kõik selle kontoga seotud tellimuste. Võite ka piirkonnas, millest kasutada Azure ressursse, kui ka loomise või importimise serdid tellimuse Azure portaalist. Kui importisite ühtegi tellimust tellimuse faili, kuvatakse seotud serdid vahekaardil **serdid** . Kui olete lõpetanud, klõpsake nuppu **Sule** .

    ![Tellimuste haldamine](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Tellimuse fail võib sisaldada rohkem kui üks tellimus.

1. Valige jätkamiseks nuppu **edasi** . 

    Kui teie tellimus pole mis tahes pilveteenustega, peate looma pilveteenus Azure majutada oma projekti. Kuvatakse dialoogiboks **loomine pilveteenuses ja salvestusruumi konto** .

    Määrake pilveteenusesse jaoks uus nimi. Nimi peab olema kordumatu Azure. Määrake piirkond või osaleja rühma andmekeskuses, mis on teie või enamik kliendid lähedal. Uue salvestusruumi konto, mis loob teie pilveteenuses Azure kasutatakse ka selle nime.

1. Muuta sätteid soovite selle juurutamiseks ja seejärel selle avaldada, valides (järgmisest jaotisest leiate täpsemat teavet eri sätted) nuppu **Avalda** . Kontrollige sätteid enne avaldamise, valige nuppu **edasi** .

    >[AZURE.NOTE] Kui valisite avalda selles juhises, saate jälgida olekut selle juurutamine Visual Studios.

**Azure'i teenuserakenduse avaldamine** viisardi abil saate muuta nii levinud ja täpsemate sätete juurutamine. Näiteks saate juurutada testimiskeskkonnas rakenduse enne seda sätet. Järgmisel joonisel on **Levinud sätted** vahekaarti Azure juurutamine.

![Levinud sätted](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Konfigureerida oma sätete avaldamine

### <a name="to-configure-the-publish-settings"></a>Avalda sätete konfigureerimine

1. Tehke loendis **pilveteenuses** järgmist ühte järgmistest.

   1. Ripploendiboksi, valida mõne olemasoleva pilveteenuses. Teenuse andmete center asukoht kuvatakse. Peaksite see asukoht üles märkida ja veenduge, et teie konto talletuskoht on sama andmekeskuse.

    1. Valige **Loo uus** Azure majutatakse pilveteenus loomiseks. Dialoogiboksis **Loomine pilveteenuses** teenuse nimi ja seejärel määrake piirkond või osaleja rühma majutada selle pilveteenuses soovitud andmekeskuse asukoha määramiseks. Nimi peab olema kordumatu Azure.

1. Valige loendis **keskkonna** **tootmise** või **lavastus**. Valige lavastus keskkonnas, kui soovite oma testimiskeskkonnas rakenduse juurutamine. Saate teisaldada oma rakenduse tootmiskeskkonda hiljem.

1. Valige loendis **koostada konfigureerimine** **silumine** või **väljaanne**.

1. Valige loendist **teenus** **Cloud** või **kohaliku**.

    Valige **Kõikide rollide kaugtöölaua lubamiseks** märkige ruut, kui soovite teenuse kaugühenduse. See suvand kasutatakse peamiselt tõrkeotsing. Kui valite selle ruudu, kuvatakse dialoogiboks **Remote Desktop konfiguratsiooni** . Valige sätted linki konfiguratsiooni muutmine.

    Märkige ruut **Luba Web juurutada kõigi web rollide** web juurutamise teenuse lubamiseks. Kaugtöölaua selle funktsiooni kasutamiseks tuleb lubada. Lisateavet leiate teemast [[avaldamise pilveteenus Azure tööriistade abil](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). Web juurutada kohta leiate lisateavet teemast [[avaldamise pilveteenus Azure tööriistade abil](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx).

1. Valige vahekaardil **Täpsemad sätted** . **Juurutamise sildi** välja jätta vaikenime või sisestage nimi enda valitud Meilikausta. Kuupäeva lisamiseks juurutamise sildi, jätke ruut märgitud.

    ![Kolmas ekraan Publishing Wizard.](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Valige loendist **salvestusruumi konto** salvestusruumi konto, mida soovite kasutada seda juurutamiseks. Võrrelge oma pilveteenuses ja salvestusruumi konto jaoks soovitud andmekeskuste asukohad. Ideaalvariandis mitte järgmistesse kohtadesse peaksid olema samad.

    >[AZURE.NOTE] Azure storage konto salvestab rakenduse juurutamine jaoks pakkimine. Kui rakendus on juurutatud, eemaldatakse paketi salvestusruumi konto.

1. Märkige ruut **juurutamise värskendada** , kui soovite juurutada ainult värskendatud komponendid. Seda tüüpi juurutamise võib olla kiirem kui täielik juurutamine. Valige **sätted** link peaks avama **juurutamise sätete värskendamine** dialoogiboksis järgmisel joonisel. 

    ![Juurutamise sätted](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Saate valida, kas värskendamine juurutamiseks suureneva või samaaegne kaks võimalust. Mõne astme juurutamine värskendab juurutatud ühel ajal, nii, et teie taotlus on võrgus ja saadaval kasutajatele. Samaaegne juurutamise värskendatakse juurutatud läbivalt kogu korraga. Samaaegne värskendus on kiirem kui astmeline värskendamine, kuid kui valite selle suvandi, rakenduse ei pruugi olla saadaval värskendamise käigus.

    Peaks valima kõrval olev ruut, kui juurutamise ei saa värskendada, täielik juurutamise teha, kui soovite täielik juurutamise toimub kui ka rakendamine nurjus. Täielik juurutamise lähtestab virtuaalse IP (VIP) aadressi pilveteenusesse. Lisateavet leiate teemast [kohta: säilitamise konstandi virtuaalse IP-aadressi jaoks mõnda pilveteenusesse](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Silumine teenust, märkige ruut **Luba IntelliTrace** või kui teil on juurutamine **silumine** konfigureerimine ja soovite oma pilveteenuses Azure silumine, märkige ruut **Luba Kaug kõigi rollide** Kaug silumine teenuste juurutamine.

2. Profiili rakendus, märkige ruut **Luba profiili** ja valige **sätted** linki profiilide suvandite kuvamiseks. 


    >[AZURE.NOTE] Peate kasutama Visual Studio Ultimate lubamiseks IntelliTrace või taseme suhtluse profiilide (otsa) ja te ei saa kasutada mõlemat korraga.

    Lisateavet leiate teemast [silumine avaldatud pilveteenuses IntelliTrace ja Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) ja [testimine pilveteenus jõudlus](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Valige **Järgmine** vaadata rakenduse kokkuvõtteleht.

## <a name="publishing-your-application"></a>Rakenduse avaldamine

1. Saate luua avaldamise profiili sätted, et olete valinud. Näiteks võite luua testimiskeskkonnas üks profiil ja teine tootmiseks. Seda profiili salvestamiseks valige **Salvesta** ikoon. Viisard loob profiili ja salvestab selle projekti Visual Studio. Profiili nimi muutmiseks avage **Target profiili** loend ja valige **< Halda >**.

    ![Kokkuvõte Kuva Publishing Wizard.](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Avaldamise profiili kuvatakse Visual Studio Solution Exploreris ja profiili sätete kirjutada .azurePubxml-laiendiga faili. Sätted salvestatakse atribuutide XML-sildid.

1. Valige käsk **Avalda** rakenduse avaldada. Saate jälgida protsessi oleku **väljundi** aknas Visual Studios.

## <a name="see-also"></a>Vt ka

[Kuidas: migreerimine ja Visual Studio on Azure pilveteenusesse veebirakenduse avaldamine](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Avaldamise pilveteenus Azure tööriistade abil](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Avaldatud pilveteenuses IntelliTrace ja Visual Studio silumine](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Testimine pilveteenus jõudlus](https://msdn.microsoft.com/library/azure/hh369930.aspx)

