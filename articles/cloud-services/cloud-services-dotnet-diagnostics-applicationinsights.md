<properties
   pageTitle="Tõrkeotsing: pilveteenuste abil rakenduse ülevaated | Microsoft Azure'i"
   description="Saate teada, kuidas abil andmete töötlemise rakenduse ülevaated Azure'i diagnostika pilvepõhise teenuse probleemide tõrkeotsing."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Tõrkeotsing: pilveteenuste abil rakenduse ülevaated

[Azure'i SDK 2,8](https://azure.microsoft.com/downloads/) ja Azure diagnostika laiendiga 1,5 saate nüüd saata Azure'i diagnostika andmete jaoks oma pilveteenuses otse rakenduse ülevaated. Erinevat tüüpi kogutud Azure'i diagnostika logid, windows sündmuselogide, sh logid ETW logib ja nüüd saate jõudluse hinnale saadetud rakenduse ülevaated ja visualiseerimise rakenduse ülevaated portaalis UI. Kui seda kasutatakse koos rakenduse ülevaateid SDK Nüüd võite saada teadmisi mõõdikute ja pärit rakenduse kui ka ja infrastruktuur taseme andmete Azure'i diagnostika logid.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Azure'i diagnostika andmeid saata rakenduse ülevaated konfigureerimine

Järgmiste juhiste abil häälestamise pilvepõhise teenuse projekti rakenduse ülevaated Azure'i diagnostika andmeid saata.

1) Visual Studio Solution Exploreris rolli paremklõpsake ja valige **Atribuudid** avamiseks rolli kujundaja

![Lahendus Explorer rolli atribuudid][1]

2) Jaotises diagnostika rolli Designeris märkige ruut saata **diagnostika andmed rakenduse ülevaated**

![Roll designer diagnostika andmeid saata rakenduse ülevaated][2]

3) Valige kuvatavas dialoogiboksis rakenduse ülevaateid ressurss, mida soovite saata Azure diagnostika andmed. Dialoogiboks võimaldab valida mõne olemasoleva rakenduse ülevaated ressursi tellimuse või käsitsi määrata instrumentation alus on rakenduse ülevaated ressurss. Kui teil on olemasoleva rakenduse ülevaated ressurss seejärel saate luua, klõpsates nuppu **Loo uus ressurss** linki, mis avab brauseriaknas Azure klassikaline portaali kus saate luua ülevaateid ressurssi. On rakenduse ülevaated ressurss loomise kohta lisateabe saamiseks vaadake [Loo uus rakenduse ülevaated ressurss](../application-insights/app-insights-create-new-resource.md)

![Valige rakenduse ülevaateid ressurss][3]

4) Kui olete lisanud rakenduse ülevaated ressursi, talletatakse teenuse konfiguratsiooni säte nimega **APPINSIGHTS_INSTRUMENTATIONKEY**instrumentation võti selle ressursi. Saate muuta selle konfiguratsioon sätte iga teenuse konfiguratsiooni või keskkonnas, valides mõne muu konfiguratsiooni teenuse konfigureerimine rippmenüüst alla ja uue tootenumbri instrumentation konfiguratsiooni jaoks täpsustades.

![Valige teenuse konfigureerimine][4]

Visual Studio kasutab **APPINSIGHTS_INSTRUMENTATIONKEY** konfiguratsioon sätte konfigureerimiseks diagnostika laiend rakenduse ülevaated ressursi asjakohast teavet avaldamise ajal. Otsingukonfiguratsiooni säte on mugav viis määratlemisel erinevate instrumentation klahvid muu teenuse konfiguratsioone. Visual Studio näidatakse seda sätet tõlkimine ja sisestage see diagnostika laiend avaliku konfiguratsiooni avaldamisel. Konfigureerimise diagnostika laiend PowerShelliga protsessi lihtsustamiseks väljund Visual Studio ka pakett sisaldab avaliku konfiguratsiooni XML-iga vastav rakenduse ülevaated mõõteriistad aktiveerimiseks. Avaliku config failid kausta laiendid ja järgige mustri PaaSDiagnostics. <RoleName>. PubConfig.xml. Mis tahes vastavalt PowerShelli juurutuste saate kasutada seda mudelit iga konfiguratsiooni vastendamine rollid.

5) Võimaldab **saata diagnostika andmed rakenduse ülevaated** automaatselt konfigureerida Azure diagnostika saata kõik jõudluse hinnale ja Tõrkelogide taseme, mis on Azure diagnostika agent rakenduse ülevaate saamiseks kogub. Kui soovite konfigureerida täiendavaid, milliseid andmeid saadetakse rakenduse ülevaated, siis peate käsitsi iga rolli *diagnostics.wadcfgx* faili redigeerimiseks. Vt lisateavet käsitsi värskendamine konfiguratsiooni [Konfigureerimine Azure'i diagnostika rakenduse ülevaated andmeid saata](../azure-diagnostics-configure-applicationinsights.md) .

Kui pilveteenusesse on konfigureeritud Azure diagnostika andmeid saata rakenduse ülevaated saate juurutamist Azure nagu tavaliselt alustades seda kindlasti Azure diagnostika laiend on lubatud. Vt [avaldamise pilveteenus Visual Studio abil](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Rakenduse ülevaated Azure diagnostika andmete vaatamine
Azure'i diagnostika telemeetria ilmub rakenduse ülevaated ressurss, mis on konfigureeritud lubama oma pilveteenuses.

Järgnevalt on kuidas erinevate Azure diagnostika logige tüüpi kaardi rakenduse ülevaated põhimõtet:  

-  Jõudluse hinnale kuvatakse kohandatud mõõdikute rakenduses rakenduse ülevaated
-  Windowsi sündmuselogide on kujutatud jälgi ja kohandatud sündmused rakenduse ülevaated
-  Klõpsake rakenduse ülevaated jälgi on kujutatud rakenduse logide, ETW logid ja mis tahes diagnostika taristu logid.

Rakenduse ülevaated Azure diagnostika andmete vaatamine

- [Mõõdikute](../application-insights/app-insights-metrics-explorer.md) Exploreriga visualiseerida, mis tahes kohandatud jõudluse hinnale või eri tüüpi Windowsi sündmuselogi sündmuste arv.

![Kohandatud mõõdikute mõõdikute Exploreris][5]

- [Otsingu](../application-insights/app-insights-diagnostic-search.md) abil saate otsida erinevate Jälita logid saata Azure'i diagnostika. Jaoks näiteks, kui teil oli töötlemata erandi rolli, mis põhjustas roll krahh ja prügikastis seda teavet näitaks üles *rakenduse* *Windowsi sündmuselogi*kanal. Saate vaadata Windowsi sündmuselogi tõrge ja täielik virnas Jälita saamiseks erand, mis võimaldab teil otsida probleemi põhjuseks otsingufunktsiooni.

![Otsi jälgi][6]

## <a name="next-steps"></a>Järgmised sammud

- [Lisa rakenduse ülevaateid SDK oma pilveteenusesse](../application-insights/app-insights-cloudservices.md) saatke oma taotlusi, erandid, sõltuvused ja mis tahes kohandatud telemeetria andmeid. Azure'i diagnostika koos andmeid, mida te võite täieliku ülevaate oma rakenduse ja kõik sama rakenduse ülevaate ressurss.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
