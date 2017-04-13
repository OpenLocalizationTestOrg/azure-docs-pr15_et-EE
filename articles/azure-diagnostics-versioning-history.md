<properties
    pageTitle="Azure'i diagnostika versiooniajalugu"
    description="Selgitus muudatuste erinevaid versioone Azure diagnostika nimega saadetud erinevaid versioone, Microsoft Azure'i SDK-d."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Microsoft Azure'i diagnostika versiooniajalugu

Uus Azure diagnostika? Teemast [Azure diagnostika ülevaade](azure-diagnostics.md).

Iga versioon Azure SDK teenusega tavaliselt Azure'i diagnostika uue versiooniga. Järgmises tabelis kirjeldatakse seostatud SDK väljaanne Azure'i SDK ja Azure diagnostika versioonid.



Azure'i SDK versioon | Azure'i diagnostika versioon | Mudel
--- | --- | ---
1.x      | 1.0 | lisandmoodul
2.0 – 2.4| 1.0 | "
2,5      | 1.2 | laiend
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1,5 | "


Uusim versioon on 1,5, mis saadetakse koos Azure SDK 2,8. Azure'i diagnostika laiend, mis on SDK versiooni kasutatakse ainult kohaliku emulaator stsenaariumid. Mis tahes juurutatud rakendus kasutab automaatselt uusima versiooni käivitamisel Azure, olenemata sellest, kes on ehitatud SDK rakenduse versiooni. Juhul, kui installite uusima Azure'i SDK, ei pruugi teil olla kõik tööriistu, mille abil saate kasutada uusi funktsioone.

Funktsioonid ja muudatused, mis on kirjeldatud allpool.

## <a name="azure-sdk-28"></a>Azure'i SDK 2,8
Azure'i SDK 2,8 lisatud võimalus saata diagnostika andmete [Rakenduse ülevaated](./application-insights/app-insights-cloudservices.md) hõlpsam diagnoosida probleeme rakenduse samuti ja infrastruktuur tase.

## <a name="azure-26-diagnostics-changes"></a>Azure'i 2.6 diagnostika muudatused

Azure'i SDK 2.6 pilveteenusesse projekte Visual Studios, tehtud järgmised muudatused. (Need muudatused ka rakendamine Azure'i SDK uuemad versioonid)

- Kohaliku emulaator toetab nüüd diagnostika. See tähendab, et saate diagnostika andmete kogumise ja veenduge, et rakenduse on õige jälgi loomise ajal olete arendamise ja testimine Visual Studios. Ühendusstringi `UseDevelopmentStorage=true` võimaldab diagnostika andmete kogumine, kui kasutate pilvepõhise teenuse projekti Visual Studio abil Azure storage emulaator. Kõik diagnostika andmed kogutakse (arengu salvestusruumi) salvestusruumi konto.

- Diagnostika salvestusruumi konto ühendusstringi (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) on talletatud taas teenuse konfiguratsioonifail (.cscfg). Azure'i SDK 2.5 diagnostika salvestusruumi konto määratud diagnostics.wadcfgx faili.

On mõned olulised erinevused kuidas ühendusstringi töötas Azure'i SDK 2.4 ja varasemad versioonid ja kuidas see toimib Azure'i SDK 2.6 ja uuemad versioonid.

- Azure'i SDK 2.4 ja varasemate versioonide ühendusstringi kasutati runtime diagnostika lisandmoodul, salvestusruumi konto teabe edastamiseks diagnostika logid.

- Azure'i SDK 2.6 ja uuemates versioonides diagnostika ühendusstringi kasutavad Visual Studio konfigureerimiseks diagnostika laiend hoidmise konto teabe avaldamise ajal. Ühendusstringi võimaldab määrata erinevaid konfiguratsioone, mida Visual Studio kasutab avaldamise erinevate salvestusruumi kontod. Kuna diagnostika lisandmoodul pole enam saadaval (pärast Azure SDK 2,5), ei saa .cscfg faili ise lubada diagnostika laiendamine. Teil on laiendamiseks eraldi näiteks Visual Studio või PowerShelli kaudu.

- Konfigureerimise diagnostika laiend PowerShelliga protsessi lihtsustamiseks Visual Studio paketi väljund sisaldab iga rolli pikendamise diagnostika avaliku konfiguratsiooni XML-i. Visual Studio kasutab diagnostika ühendusstringi asustamiseks Esita avaliku konfiguratsiooni salvestusruumi kontoteave. Avaliku config failid laiendid kausta ja järgige mustri PaaSDiagnostics. <RoleName>. PubConfig.xml. Mis tahes vastavalt PowerShelli juurutuste saate kasutada seda mudelit iga konfiguratsiooni vastendamine rollid.

- Ühendusstringi .cscfg failis kasutatakse ka Azure portaali diagnostika andmetele juurdepääsuks nii, et see võidakse kuvada vahekaardil **jälgimine** . Ühendusstringi on vaja konfigureerida portaalis Paljusõnaline jälgimisega seotud andmete kuvamiseks.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migreerimine projektide Azure'i SDK 2.6 ning hiljem

Kui migreerimine Azure'i SDK 2,5 Azure'i SDK 2.6 või uuem versioon, kui oleksite diagnostika salvestusruumi konto .wadcfgx määratud, siis see jääb sinna. Ära kasutada eri salvestusruumi kontode jaoks eri salvestusruumi konfiguratsioone, peate ühendusstringi käsitsi lisamine projekti. Kui plaanite migreerida projekti Azure'i SDK 2.4 või mõnes varasemas versioonis Azure'i SDK 2.6, säilitatakse diagnostika ühendusstringi. Siiski, Pange tähele, kuidas ühendusstringi käsitletakse Azure'i SDK 2.6 määratud eelmises jaotises muudatused.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Kuidas Visual Studio määrab diagnostika salvestusruumi konto

- Kui diagnostika ühendusstringi määratud .cscfg faili, Visual Studio kasutab seda avaldamisel, ja avaliku XML-i failid loomisel ajal pakendit diagnostika laiend konfigureerimine.

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
