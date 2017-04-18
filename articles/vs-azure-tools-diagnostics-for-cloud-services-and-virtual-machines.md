<properties
   pageTitle="Diagnostika konfigureerida Azure pilveteenustega ja Virtuaalmasinates | Microsoft Azure'i"
   description="Kirjeldab, kuidas konfigureerida diagnostika teave silumine Azure cloude teenuste ja Visual Studio virtuaalmasinates (VMs)."
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

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Azure'i pilveteenustega ja Virtuaalmasinates diagnostika konfigureerimine

Kui soovite mõne Azure pilveteenuses või Azure virtuaalse masina tõrkeotsing, saate Azure diagnostika hõlpsam konfigureerida, Visual Studio abil. Azure'i diagnostika jäädvustab süsteemi andmed ja logimine andmed virtuaalmasinates ja käivitage oma pilveteenuses virtuaalse masina eksemplarid ja üle salvestusruumi konto teie valitud andmeid. Diagnostika Azure logimise kohta lisateabe saamiseks vaadake [diagnostika logimine veebirakenduste teenuses Azure rakenduse lubamine](./app-service-web/web-sites-enable-diagnostic-log.md) .

Selles teemas näidatakse, kuidas lubamiseks ja konfigureerimiseks Azure diagnostika Visual Studios, nii enne ja pärast juurutus, samuti Azure'i virtuaalmasinates. Samuti näitab teile valimine soovitud tüüpi diagnostika teabe kogumiseks ja kuidas pärast seda on kogutud teabe kuvamiseks.

Azure'i diagnostika saate konfigureerida järgmisel viisil:

- Diagnostika konfiguratsioonisätted **Diagnostika konfiguratsiooni** dialoogiboksi Visual Studio abil saate muuta. Sätted on salvestatud fail nimega diagnostics.wadcfgx (diagnostics.wadcfg Azure SDK 2.4 või PowerPointi varasemas versioonis). Teise võimalusena saate muuta otse konfigureerimise faili. Faili käsitsi värskendamisel konfiguratsiooni muudatused jõustuvad järgmise juurutamist pilve Azure teenuse või läbiviimisel teenuse emulaator.

- Kasutage **Cloud Explorer** või **Server Explorer** Visual Studio töötava pilveteenuses või virtuaalse masina diagnostika sätete muutmine.

## <a name="azure-26-diagnostics-changes"></a>Azure'i 2.6 diagnostika muudatused

Azure'i SDK 2.6 projektide Visual Studios, tehtud järgmised muudatused. (Need muudatused ka rakendamine Azure'i SDK uuemad versioonid)

- Kohaliku emulaator toetab nüüd diagnostika. See tähendab, et saate diagnostika andmete kogumise ja veenduge, et rakenduse on õige jälgi loomise ajal olete arendamise ja testimine Visual Studios. Ühendusstringi `UseDevelopmentStorage=true` võimaldab diagnostika andmete kogumine, kui kasutate pilvepõhise teenuse projekti Visual Studio abil Azure storage emulaator. Kõik diagnostika andmed kogutakse (arengu salvestusruumi) salvestusruumi konto.

- Diagnostika salvestusruumi konto ühendusstringi (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) on talletatud taas teenuse konfiguratsioonifail (.cscfg). Azure'i SDK 2.5 diagnostika salvestusruumi konto määratud diagnostics.wadcfgx faili.

On mõned olulised erinevused kuidas ühendusstringi töötas Azure'i SDK 2.4 ja varasemad versioonid ja kuidas see toimib Azure'i SDK 2.6 ja uuemad versioonid.

- Azure'i SDK 2.4 ja varasemate versioonide ühendusstringi kasutati runtime diagnostika lisandmoodul, salvestusruumi konto teabe edastamiseks diagnostika logid.

- Azure'i SDK 2.6 ja uuemates versioonides diagnostika ühendusstringi kasutavad Visual Studio konfigureerimiseks diagnostika laiend hoidmise konto teabe avaldamise ajal. Ühendusstringi võimaldab määrata muu teenuse konfiguratsioone, mida Visual Studio kasutab avaldamise erinevate salvestusruumi kontod. Kuna diagnostika lisandmoodul pole enam saadaval (pärast Azure SDK 2,5), ei saa .cscfg faili ise lubada diagnostika laiendamine. Teil on laiendamiseks eraldi näiteks Visual Studio või PowerShelli kaudu.

- Konfigureerimise diagnostika laiend PowerShelliga protsessi lihtsustamiseks Visual Studio paketi väljund sisaldab iga rolli pikendamise diagnostika avaliku konfiguratsiooni XML-i. Visual Studio kasutab diagnostika ühendusstringi asustamiseks Esita avaliku konfiguratsiooni salvestusruumi kontoteave. Avaliku config failid kausta laiendid ja järgige mustri PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Mis tahes vastavalt PowerShelli juurutuste saate kasutada seda mudelit iga konfiguratsiooni vastendamine rollid.

- Ühendusstringi .cscfg failis kasutatakse ka [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040) diagnostika andmetele juurdepääsuks nii, et see võidakse kuvada vahekaardil **jälgimine** . Ühendusstringi on vaja konfigureerida portaalis Paljusõnaline jälgimisega seotud andmete kuvamiseks.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migreerimine projektide Azure'i SDK 2.6 ja uuemad versioonid

Kui migreerimine Azure'i SDK 2,5 Azure'i SDK 2.6 või uuem versioon, kui oleksite diagnostika salvestusruumi konto .wadcfgx määratud, siis see jääb sinna. Ära kasutada eri salvestusruumi kontode jaoks eri storage konfiguratsiooni, peate ühendusstringi käsitsi lisamine projekti. Kui plaanite migreerida projekti Azure'i SDK 2.4 või mõnes varasemas versioonis Azure'i SDK 2.6, säilitatakse diagnostika ühendusstringi. Siiski, Pange tähele, kuidas ühendusstringi käsitletakse Azure'i SDK 2.6 määratud eelmises jaotises muudatused.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Kuidas Visual Studio määrab diagnostika salvestusruumi konto

- Kui diagnostika ühendusstringi määratud .cscfg faili, Visual Studio kasutab seda konfigureerida diagnostika laiend avaldamisel ja avaliku XML-i failid loomisel ajal pakendit.

- Kui määratud pole diagnostika ühendusstringi .cscfg faili, siis Visual Studio langeb tagasi konfigureerimine diagnostika laiend, kui avaldamise ning luues avaliku konfiguratsiooni XML-failid, kui pakendamine salvestusruumi konto määratud .wadcfgx faili abil.

- Ühendusstringi diagnostika .cscfg faili ülimuslik salvestusruumi konto .wadcfgx faili. Kui diagnostika ühendusstringi määratud .cscfg faili, siis Visual Studio mis kasutab ja ignoreerib .wadcfgx salvestusruumi konto.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Mida tähendab "Update arengu salvestusruumi ühenduse stringid..." ruut teha?

Ruut **Värskenda arengu salvestusruumi ühendusstringi diagnostika-ja vahemälu Microsoft Azure'i salvestusruumi konto mandaati Microsoft Azure avaldamisel** annab teile mugavam värskendada mis tahes arengu salvestusruumi konto ühendusstringi määratud ajal avaldamise Azure storage kontoga.

Oletagem näiteks, et see ruut, kui valite ja diagnostika ühendusstringi määrab `UseDevelopmentStorage=true`. Projekti avaldamisel Azure'i automaatselt värskendada Visual Studio salvestusruumi konto avalda viisardis määratud diagnostika ühendusstring. Juhul, kui reaal salvestusruumi konto oli määratud diagnostika ühendusstring, siis selle konto kasutatakse selle asemel.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Azure'i SDK 2.4 ja varasemate ja Azure SDK 2,5 ja uuemad versioonid diagnostika funktsioonide erinevused

Kui värskendate oma projekti Azure'i SDK 2.4 Azure'i SDK 2,5 või uuem versioon, mida tuleks silmas pidada järgmist diagnostika funktsioonide erinevused.

- **Konfiguratsiooni API -d on aegunud** -diagnostika programmiline konfigureerimine on saadaval Azure SDK 2.4 või varasemates versioonides, kuid on aegunud Azure'i SDK 2,5 ja uuemad versioonid. Kui teie diagnostika konfiguratsiooni on praegu määratletud kood, peate konfigureerida need sätted nullist migreeritud projekti selleks diagnostika töötamist jätkata. Azure'i SDK 2.4 diagnostika konfiguratsioonifail on diagnostics.wadcfg ja diagnostics.wadcfgx Azure SDK 2,5 ja uuemad versioonid.

- **Diagnostika puhul pilveteenuste teenuserakenduste saab konfigureerida ainult tasemel rolli ei astme.**

- **Iga kord, kui juurutate oma rakenduse diagnostika konfiguratsiooni värskendatakse** – see võib põhjustada võrdse probleemid, kui muudate diagnostika konfiguratsioonile Server Explorer ja seejärel Juurutage uuesti oma rakenduse.

- **Konfiguratsioonifail diagnostika ei koodis on konfigureeritud Azure SDK-2,5 ja uuemad versioonid, krahhi puistab** – kui teil on krahh puistab koodi konfigureeritud, peate käsitsi edastamiseks konfiguratsiooni kood konfiguratsioonifail, kuna krahh puistab pole migreerimisel Azure'i SDK 2.6 üle kanda.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Luba diagnostika pilvepõhise teenuse projektides neid enne juurutamist

Visual Studio, saate diagnostika rollid, mis töötavad Azure, kui käivitate emulaator enne juurutamist selle teenuse andmete kogumise. Konfiguratsioonifailis diagnostics.wadcfgx salvestatakse kõik muudatused diagnostika sätete Visual Studios. Nende sätete konfigureerimine määrata salvestusruumi konto, kus diagnostika andmed salvestatakse teie pilveteenuses juurutamisel.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Enne juurutamine Visual Studios diagnostika lubamiseks

1. Klõpsake kiirmenüü käsku rolli, mis teile huvi pakub, valige **Atribuudid**ja valige soovitud roll **atribuutide** aknas vahekaarti **konfigureerimine** .

1. Jaotises **diagnostika** veenduge, et oleks märgitud ruut **Luba diagnostika** .

    ![Juurdepääs suvand Luba diagnostika](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Salvestusruumi konto, kuhu soovite diagnostika andmete talletamise määramiseks klõpsake nuppu kolmikpunkti (…). Valige salvestusruumi konto saab diagnostika andmete talletuskoht asukoht.

    ![Määrake salvestusruumi konto, mida soovite kasutada](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. Dialoogiboksis **Salvestusruumi ühendusstringi loomine** määrata, kas soovite ühenduse loomiseks kasutada Azure salvestusruumi emulaator Azure tellimuse või käsitsi sisestatud mandaat.

    ![Dialoogiboks talletusmahu konto](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Kui valite Microsoft Azure'i salvestusruumi emulaator suvand ühendusstring on seatud UseDevelopmentStorage = true.

  - Kui valite selle oma tellimust, saate valida Azure'i tellimus, mida soovite kasutada ja konto nimi. Saate valida Azure tellimuste haldamine nuppu Halda kontosid.

  - Kui valite suvandi käsitsi sisestatud mandaat, palutakse teil sisestage nimi ja klahvi Azure'i konto, mida soovite kasutada.

1. **Diagnostika konfiguratsiooni** dialoogiboksi kuvamiseks klõpsake nuppu **Konfigureeri** . Iga vahekaardi (välja arvatud **üldist** ja **Log kataloogid**) tähistab kogute diagnostika andmeallikas. Vaikimisi vahekaarti **üldist**, pakub andmete kogumise diagnostika järgmistest suvanditest: **tõrked ainult**, **kogu teave**ja **kohandatud leping**. Vaikesuvand **ainult tõrkeid**, võtab salvestusruumi vähemalt summa, sest seda ei saa üle kanda hoiatused või jälgimise sõnumeid. Kogu teabe suvand enamik teave üle ja seetõttu on kõige kallim variant osas salvestusruumi.

    ![Luba Azure diagnostika- ja konfigureerimine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. Selle näite puhul valige suvand **kohandatud lepingu** saate kohandada kogutud andmete.

1. Väljale **Ketta kvoodi MB** saate määrata, kui palju ruumi soovite jaotada teie salvestusruumi konto diagnostika andmed. Kui soovite, saate muuta vaikeväärtus.

1. Valige iga vahekaardi diagnostika andmed, mida soovite koguda selle **lubamine üleviimine <log type> ** ruut. Näiteks, kui soovite koguda logid, märkige ruut **Luba logid edastamise** , **Rakenduse logide** vahekaardil. Määrake ka muud teavet iga diagnostika andmetüüpi. Jaotisest **konfigureerimine diagnostika andmeallikate** käesoleva teema konfiguratsiooni teavet iga vahekaardi.

1. Kui olete lubanud diagnostika andmeid soovite, klõpsake nuppu **OK** .

1. Käivitage Azure pilveteenuse teenuse projekti Visual Studios nagu tavaliselt. Kui kasutate rakendust, salvestatakse lubatud Logiteave määratud Azure storage konto.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Luba diagnostika Azure'i virtuaalmasinates

Visual Studio, saate valida teabe kogumine diagnostika Azure'i virtuaalmasinates.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Lubamiseks diagnostika Azure'i virtuaalmasinates

1. **Server Explorer**, valige Azure sõlm ja seejärel ühenduse Azure tellimuse, kui olete juba pole ühendatud.

1. Laiendage **Virtuaalmasinates** . Saate luua uue virtuaalse masina, või valige üks, mis on juba olemas.

1. Kiirmenüü virtuaalse masina, mis teile huvi pakub, klõpsake nuppu **konfigureerimine**. See näitab dialoogiboksi virtuaalse masina konfigureerimine.

    ![Azure'i virtuaalarvuti konfigureerimine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Kui see pole juba installitud, lisage Microsoft Agenti diagnostika laiendamine. Sellele laiendile võimaldab Azure virtuaalse masina diagnostika andmete kogumine. Installitud laiendid loendis Valige valige saadaval laiend rippmenüü ja seejärel valige Microsoft Agenti diagnostika.

    ![Installimist Azure virtuaalse masina laiendamine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Muud diagnostika laiendid on teie virtuaalmasinates saadaval. Lisateabe saamiseks lugege teemat Azure VM laiendid ja funktsioonid.

1. Koos laiendiga ja **diagnostika konfiguratsiooni** dialoogiboksi kuvamiseks klõpsake nuppu **Lisa** .

1. Valige nupp **konfigureerimine** Määrake salvestusruumi konto ja seejärel klõpsake nuppu **OK** .

    Iga vahekaardi (välja arvatud **üldist** ja **Log kataloogid**) tähistab kogute diagnostika andmeallikas.

    ![Luba Azure diagnostika- ja konfigureerimine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Vaikimisi vahekaarti **üldist**, pakub andmete kogumise diagnostika järgmistest suvanditest: **tõrked ainult**, **kogu teave**ja **kohandatud leping**. Vaikesuvand **ainult tõrkeid**, võtab salvestusruumi vähemalt summa, sest seda ei saa üle kanda hoiatused või jälgimise sõnumeid. Suvandi **kogu teabe** enamik teave üle ja seetõttu on kõige kallim variant osas salvestusruumi.

1. Selle näite puhul valige suvand **kohandatud lepingu** saate kohandada kogutud andmete.

1. Väljale **Ketta kvoodi MB** saate määrata, kui palju ruumi soovite jaotada teie salvestusruumi konto diagnostika andmed. Kui soovite, saate muuta vaikeväärtus.

1. Valige iga vahekaardi diagnostika andmed, mida soovite koguda selle **lubamine üleviimine <log type> ** ruut.

    Näiteks, kui soovite koguda logid, märkige ruut **Luba logid edastamise** , **Rakenduse logide** vahekaardil. Määrake ka muud teavet iga diagnostika andmetüüpi. Jaotisest **konfigureerimine diagnostika andmeallikate** käesoleva teema konfiguratsiooni teavet iga vahekaardi.

1. Kui olete lubanud diagnostika andmeid soovite, klõpsake nuppu **OK** .

1. Salvestage värskendatud projekti.

    Kuvatakse sõnumi aknas **Microsoft Azure'i tegevuste Logi** virtuaalse masina uuendamist.

## <a name="configure-diagnostics-data-sources"></a>Diagnostika andmeallikate konfigureerimine

Kui olete lubanud diagnostika andmete kogumine, saate valida täpselt milliseid andmeallikaid, mida soovite koguda ja millist teavet kogutakse. Järgmises loendis sakke dialoogiboksi **diagnostika konfigureerimine** ja mida iga konfiguratsiooni suvand tähendab.

### <a name="application-logs"></a>Logid

**Logid** sisaldavad veebirakenduse diagnostika andmed. Kui soovite jäädvustada logid, märkige ruut **Luba edastamise logid** . Te saate suurendada või vähendada kui rakenduse logid on üle minutite arv salvestusruumi kontole, muutes **Edastamine perioodi (min)** väärtust. Saate muuta ka teabe jäädvustata Logi Log taseme väärtust. Näiteks saate valida **Verbose** Lisateave või valida **kriitiline** jäädvustada ainult kriitilisi tõrkeid. Kui teil on teatud diagnostika pakkuja, mis eraldab logid, saate need hõivata, lisades teenusepakkuja GUID **Pakkuja GUID** välja.

  ![Logid](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Logid kohta lisateabe saamiseks vaadake [diagnostika logimine veebirakenduste teenuses Azure rakenduse lubamine](./app-service-web/web-sites-enable-diagnostic-log.md) .

### <a name="windows-event-logs"></a>Windowsi sündmuste logid

Kui soovite jäädvustada Windows sündmuselogide, märkige ruut **Luba edastamine Windows sündmuselogide** . Te saate suurendada või vähendada kui sündmuse logid on üle minutite arv salvestusruumi kontole, muutes **Edastamine perioodi (min)** väärtust. Märkige ruudud soovitud tüüpi sündmusi, mida soovite jälgida.

  ![Sündmuselogide](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Kui kasutate Azure SDK 2.6 või uuem versioon ja soovite kohandatud andmeallika määramine, sisestage see on **<Data source name>** tekst väljale ja seejärel valige selle kõrval nuppu **Lisa** . Andmeallikas on lisatud diagnostics.cfcfg faili.

Kui kasutate Azure SDK 2,5 ja soovite määrata kohandatud andmeallika, saate seda funktsiooni `WindowsEventLog` osas on diagnostics.wadcfgx faili, nagu järgmises näites.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Jõudluse hinnale

Jõudluse counter teave aitab teil süsteemi kitsaskohti ja täpsustada süsteemi ja rakenduste jõudlust. Lisateabe saamiseks vaadake [loomine ja kasutamine jõudluse hinnale Azure'i rakenduses](https://msdn.microsoft.com/library/azure/hh411542.aspx) . Kui soovite jäädvustada jõudluse hinnale, märkige ruut **Luba edastamise jõudluse hinnale** . Te saate suurendada või vähendada kui sündmuse logid on üle minutite arv salvestusruumi kontole, muutes **Edastamine perioodi (min)** väärtust. Märkige ruudud soovitud jõudluse väidab, et soovite jälgida.

  ![Jõudluse hinnale](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Jälgida jõudlust vastu, mis pole loendis, sisestage see soovitatud süntaksi abil ja seejärel valige nupp **Lisa** . Operatsioonisüsteem virtuaalse masina määratleb mis jõudluse hinnale saate jälgida. Süntaksi kohta lisateabe saamiseks lugege teemat [täpsustades Counter tee](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Taristu logid

Kui soovite jäädvustada taristu logid, mis sisaldavad teavet Azure diagnostika taristu, RemoteAccess mooduli ja RemoteForwarder mooduli, märkige ruut **Luba edastamise taristu logid** . Te saate suurendada või vähendada kui logid on üle minutite arv salvestusruumi kontole, muutes edastamine perioodi (min) väärtust.

  ![Diagnostika taristu logid](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Lisateabe saamiseks vaadake [Logimine andmete kogumise Azure'i diagnostika abil](https://msdn.microsoft.com/library/azure/gg433048.aspx) .

### <a name="log-directories"></a>Log kataloogide

Kui soovite jäädvustada log kataloogid, mis sisaldavad andmete log kataloogide Internet Information Services (IIS) taotluste, nurjunud taotluste või kaustad, mida saab valida, märkige ruut **Luba edastamine Log kataloogid** . Te saate suurendada või vähendada kui logid on üle minutite arv salvestusruumi kontole, muutes **Edastamine perioodi (min)** väärtust.

Saate valida soovite koguda, nt **IIS-i logid** ja **Nurjus taotlemine** logid logid ruudud. Salvestusruumi container vaikenimed on olemas, kuid soovi korral saate muuta nimed.

Samuti saate hõivata logid suvalisest kaustast. Ainult määratud tee jaotises **Log absoluutne kataloogist** ja seejärel valige nupp **Lisa kataloogi** . Kui soovite määratud ümbriste on kaetud logid.

  ![Log kataloogide](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW logid

Kui kasutate [Windowsi sündmuse jälitamise](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (ETW) ja soovite hõivata ETW logid, märkige ruut **Luba edastamise ETW logid** . Te saate suurendada või vähendada kui logid on üle minutite arv salvestusruumi kontole, muutes **Edastamine perioodi (min)** väärtust.

Sündmuste on jäädvustatud sündmuse allikatest ja sündmuse manifestid teie määratud. Sündmuse allikas määramiseks sisestage jaotise **Allikad sündmuse** nimi ja seejärel klõpsake nuppu **Lisa sündmus allikas** . Samuti saate määrata mõni sündmus manifesti **Sündmus manifestid** jaotises ja seejärel nuppu **Lisa sündmus näidata** .

  ![ETW logid](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  ASP.net-i on toetatud ETW framework tunde [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) nimeruumi. Microsoft.WindowsAzure.Diagnostics nimeruumi, mis pärib ja laieneb standard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) klassid, mis võimaldab kasutada [System.Diagnostics.aspx] (logimise raamistik Azure keskkonnas https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). Lisateabe saamiseks vt [võtta juhtelement, logimine ja jälitamine Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) ja [Azure pilveteenustega ja Virtuaalmasinates diagnostika lubamine](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Puistab ootamatult sulguda

Kui soovite jäädvustada teavet kui jookseb rolli eksemplari, märkige ruut **Luba edastamise ootamatult sulguda puistab** . (Seetõttu ASP.net-i tegeleb enamik erandid, see on tavaliselt kasulik ainult töötaja rolli.) Saate suurendamiseks või vähendamiseks krahhi puistab pühendunud salvestusruumi, muutes **Directory kvoodi (%)** väärtust. Saate muuta salvestusruumi-ümbrisest, kus talletatakse krahh puistab ja saate valida, kas soovite hõivata **täis** - või **Mini** dump.

Praegu jälitatud protsesside on loetletud. Märkige ruudud protsesside, mida soovite hõivata. Muu protsess loendisse lisada, sisestage protsessi nime ja seejärel klõpsake nuppu **Lisa protsess** .

  ![Puistab ootamatult sulguda](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Vt [võtta juhtelement, logimine ja jälitamine Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) ja [Microsoft Azure'i diagnostika osa 4: kohandatud logimine komponendid ja Azure diagnostika 1.3 muudatuste](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) lisateabe saamiseks.

## <a name="view-the-diagnostics-data"></a>Diagnostika andmete vaatamine

Kui olete kogunud pilveteenus või virtuaalse masina diagnostika andmeid, saate seda vaadata.

### <a name="to-view-cloud-service-diagnostics-data"></a>Pilveteenuse teenuse diagnostika andmete kuvamiseks

1. Juurutada oma pilveteenuses nagu tavaliselt ja seejärel käivitage see.

1. Teie salvestusruumi konto saate vaadata tabelite või aruanne, mis loob Visual Studio diagnostika andmed. Aruande andmete vaatamiseks avage **Cloud Explorer** või **Server Explorer**, sõlm rolli, mis teile huvi pakub kiirmenüü avamine ja valige **Diagnostika andmete kuvamine**.

    ![Diagnostika andmete kuvamine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Aruanne, mis näitab olemasolevad andmed kuvatakse.

    ![Microsoft Azure'i diagnostika aruande Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Kui kõige värskemate andmete ei kuvata, peate ootama edastamine perioodi lisamisest.

    Valige andmete värskendamiseks kohe lingi **värskendamine** või valige intervall **Värskenda automaatselt** ripploendiboksi olema andmeid värskendatakse automaatselt. Tõrge andmete eksportimiseks klõpsake nuppu **eksport CSV-faili** loomiseks saate avada arvutustabelisse CSV-vormingus faili.

    **Cloud Explorer** või **Server Explorer**, avage salvestusruumi konto, mis on seotud juurutamise.

1. Avage diagnostika tabelid tabeli vaaturis ning seejärel vaadake andmed, mida te kogutud. IIS-i logid ja kohandatud logid, saate avada bloobimälu ümbrises. Järgmises tabelis vaadates leiate tabeli või bloobimälu ümbris, mis sisaldab andmeid, mis teile huvi pakub. Lisaks selle logifaili andmeid, tabeli kirjed sisaldavad EventTickCount, DeploymentId, rolli ja RoleInstance, mis aitavad teil tuvastada, milliseid virtuaalse masina ja rolli genereeritud andmed ja kui. 

  	|Diagnostikaandmete|Kirjeldus|Asukoht|
  	|---|---|---|
  	|Logid|Logide, mis teie kood tekitab helistades meetodite System.Diagnostics.Trace klassi.|WADLogsTable|
  	|Sündmuselogide|Andmed on Windowsi sündmuselogide virtual masinad. Windows salvestab need logid teavet, kuid rakenduste ja teenuste ka abil tõrgetest või Logiteave.|WADWindowsEventLogsTable|
  	|Jõudluse hinnale|Kogute andmeid mis tahes Jõudluseloenduri, mis on saadaval virtual arvutisse. Operatsioonisüsteem pakub jõudluse hinnale, mis sisaldavad paljude statistika nagu mälu kasutus- ja protsessor aeg.|WADPerformanceCountersTable|
  	|Taristu logid|Need logid luuakse diagnostika taristu, ise.|WADDiagnosticInfrastructureLogsTable|
  	|IIS-i logid|Need logid kirje web nõuab. Kui teie pilveteenuses saab palju liikluse, need logid võib olla üsna pikk, nii, et te peaksite kogumiseks ja salvestamiseks andmed ainult siis, kui neid vajate.|Saate otsida nurjus taotluse logib bloobimälu ümbrises jaotises wad-iis-failedreqlogs jaotises tee seda juurutus, rolli ja eksemplari. Saate otsida wad-iis-logfiles täieliku logid. WADDirectories tabelis tehakse iga faili kirjeid.|
  	|Puistab ootamatult sulguda|Selle teabe leiate kahendarvu pildid oma pilveteenuses protsess (tavaliselt töötaja roll).|container wad purustada-tegeleb bloobimälu|
  	|Kohandatud logifailid|Logide andmeid, mis te eelmääratletud.|Saate määrata koodi kohandatud logifailide asukoha teie salvestusruumi konto. Näiteks saate määrata kohandatud bloobimälu ümbrises.|

1. Kui mis tahes tüüpi andmete kärbitakse, võite proovida puhvri andmete jaoks suureneva tüüp või lühendamine intervalli andmete edastamine arvutist virtual salvestusruumi kontole.

1. (valikuline) Andmete likvideerite salvestusruumi kontole aeg-ajalt vähendamiseks üldine.

1. Kui teete täielik juurutus, diagnostics.cscfg faili (.wadcfgx jaoks Azure'i SDK 2,5) värskendatakse Azure ja oma pilveteenuses sõnumi salvestab teie diagnostika konfiguratsiooni muudatusi. Kui te, selle asemel värskendada olemasoleva juurutuse, .cscfg faili ei värskendata Azure. Diagnostika sätete, saate muuta ikka küll, järgmise jaotise juhiseid järgides. Täielik juurutamise läbimiseks ja olemasoleva juurutuse värskendamise kohta leiate lisateavet teemast [Azure rakendus viisard avaldamine](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Virtuaalse masina diagnostika andmete kuvamiseks

1. Valige otseteemenüü virtuaalse masina **Diagnostika andmete kuvamine**.

    ![Azure'i virtual kohapeal diagnostika andmete kuvamine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Avatakse aken **diagnostika Kokkuvõte** .

    ![Azure virtuaalse masina diagnostika Kokkuvõte](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Kui kõige värskemate andmete ei kuvata, peate ootama edastamine perioodi lisamisest.

    Valige andmete värskendamiseks kohe lingi **värskendamine** või valige intervall **Värskenda automaatselt** ripploendiboksi olema andmeid värskendatakse automaatselt. Tõrge andmete eksportimiseks klõpsake nuppu **eksport CSV-faili** loomiseks saate avada arvutustabelisse CSV-vormingus faili.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Pärast juurutuse pilvepõhise teenuse diagnostika konfigureerimine

Kui te tegeleme pilveteenus probleem selle käivitatud, võiksite koguda andmeid, mis ei enne määratud algselt juurutatud roll. Sel juhul saate alustada koguda andmeid serveri Exploreri sätete abil. Saate konfigureerida diagnostika ühekordsest või kõik eksemplarid rolli, olenevalt sellest, kas avate dialoogiboksi diagnostika konfigureerimine kiirmenüü eksemplari või roll. Kui konfigureerite rolli sõlm, kõik muudatused jõustuvad kõik eksemplarid. Kui konfigureerite sõlme eksemplari, mis tahes muudatused rakenduvad ainult juhul.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Töötava pilveteenuses diagnostika konfigureerimine

1. Server Explorer, laiendage **Pilveteenustega** ja seejärel laiendage sõlmed leidmiseks roll või eksemplar, mida soovite uurida või mõlemad.

    ![Diagnostika konfigureerimine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. Kiirmenüü on eksemplari sõlm või rolli sõlm nuppu **Värskenda diagnostika sätted**ja valige diagnostika sätted, mida soovite koguda.

    Sätete konfigureerimise kohta leiate teavet teemast **konfigureerimine diagnostika andmeallikate** selles teemas. Lisateavet selle kohta, kuidas diagnostika andmete kuvamiseks Kuva **diagnostika andmed** selles teemas.

    Kui soovite muuta andmete kogumine **Server**Explorer, need muudatused kehtivad seni, kuni te täielikult ümberkorraldamine oma pilveteenuses. Kui te kasutate vaikimisi avaldamine sätted, muudatused üle ei kirjutata, kuna vaikimisi avaldamine säte on värskendada olemasoleva juurutamise, mitte ei täielik positsioonide. Veenduge, et sätted tühjendage juurutamise ajal, avage menüü avalda viisardi **Täpsemad sätted** ja tühjendage ruut **juurutamise värskendada** . Kui te ümberkorraldamine see ruut on tühjendatud, sätete uuesti need .wadcfgx (või .wadcfg) faili vastavalt atribuutide redaktori rolli kaudu. Kui värskendate oma juurutuse, hoiab Azure'i vana sätted.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Azure'i pilveteenuste tõrkeotsing

Kui teil tekib probleeme pilvepõhise teenuse projektide, rolli, mis jääb saatmata "hõivatud" olek, nt korduvalt ringlusse või põhjustab Sisemine serveritõrge, on tööriistad ja tehnikate abil saate diagnoosimine ja nende seotud probleemide lahendamine. Levinud probleemide ja lahenduste teatud näiteid, samuti ülevaade põhimõtet ja tööriistu, mida kasutatakse diagnoosimine ja selliste vigade, vt [Azure'i PaaS arvutada diagnostika andmed](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>K & v

**Mis on puhvri suurus ja kui palju see peaks olema?**

Iga virtuaalse masina näiteks kvootide piirata palju Diagnostikaandmete saate talletatud kohalik failisüsteemis. Lisaks saate määrata puhvri suurus iga tüüpi Diagnostikaandmete, mis on saadaval. Selle puhvri suurus toimib nagu ka üksikute piirmäära andmete tüüpi. Saate määrata, märkides ruudu dialoogiboksi allservas, eraldatud ja mälu, mis jääb. Kui määrate suuremat puhvrid või rohkem andmetüüpe, tuleb teil lähenemine eraldatud. Saate muuta eraldatud muutes diagnostics.wadcfg/.wadcfgx konfigureerimise faili. Diagnostika andmed on salvestatud sama failisüsteemi rakenduse andmeid, nii et kui teie rakendus kasutab palju kettaruumi, mida ei tohiks suurendada diagnostika eraldatud.

**Mis on perioodi edastamine ja kaua peaks olema?**

Perioodi edastamine on aeg, mis sisaldab möödub andmete vahel. Pärast iga edastamine andmed teisaldatakse kohaliku failisüsteemi kaudu virtual arvutisse konto salvestusruumi tabelid. Kui kogutud andmeid ületab enne edastamine perioodi kvoodi, vanemate andmete kõrvale. Võite vähendamiseks edastamine perioodi, kui olete andmete kaotsimineku, kuna andmete ületab puhvri suurus või eraldatud.

**Ajavööndi, mis on selle ajatemplid?**

Funktsiooni ajatemplid on andmekeskuse, mis hostib oma pilveteenuses ajavöönd. Järgmised kolm ajatempli veerud log tabelite kasutatakse.

  - **PreciseTimeStamp** on ETW ajatempli sündmus. St aja sündmuse klient.

  - **AJATEMPLI** on PreciseTimeStamp üles sagedus äärist ümardada. Nii, et teie üleslaadimine sagedus on 5 minutit ja sündmuse kellaaeg 12:00:17, AJATEMPLI korral 00:15:00.

  - **Ajatempli** on ajatempli, kus üksus on loodud Azure tabelis.

**Kuidas hallata kulud, kui diagnostikateabe kogumise?**

Vaikesätete (**Logi tase** **vea** ja **edastamine perioodi** väärtuseks **1 minutid**) on loodud maksumus minimeerimiseks. Arvuta kulude suureneb kui veel Diagnostikaandmete või vähendada edastamine. Ei koguda rohkem andmeid, kui peate, kuid ärge unustage keelata andmete kogumine, kui teil pole enam vaja. Saate alati selle uuesti lubada, isegi käitusajal, nagu on näidatud eelmises jaotises.

**Kuidas koguda nurjus taotluse logid IIS-i?**

Vaikimisi ei IIS-i kogu nurjus taotluse logid. Saate konfigureerida IIS koondamiseks, kui redigeerite oma web rolli fail.

**Ma ei saa RoleEntryPoint meetodid, nt OnStart Jälita teavet. Mis valesti on?**

WAIISHost.exe, mitte IIS-i kontekstis nimetatakse RoleEntryPoint viise. Seetõttu konfiguratsiooni teave web.config, mis tavaliselt jälgimine võimaldab ei kehti. Selle probleemi lahendamiseks .config faili lisamine projekti web rolli ja väljundi komplekti, mis sisaldab RoleEntryPoint koodi vastavaks faili nimi. Vaikimisi web rolli projekti oleks WAIISHost.exe.config .config faili nimi. Seejärel lisage järgmised read seda faili.

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Nüüd aknas **Atribuudid** atribuudi **väljundkausta Kopeeri** **alati**koopia.

## <a name="next-steps"></a>Järgmised sammud

Diagnostika Azure logimise kohta leiate lisateavet teemast [Azure pilveteenustega ja Virtuaalmasinates lubamine diagnostika](./cloud-services/cloud-services-dotnet-diagnostics.md) - ja [diagnostika logimine veebirakenduste teenuses Azure rakenduse lubamine](./app-service-web/web-sites-enable-diagnostic-log.md).
