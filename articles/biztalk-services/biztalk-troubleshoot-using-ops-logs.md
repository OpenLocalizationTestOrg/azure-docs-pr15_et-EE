<properties 
    pageTitle="BizTalki teenuste abil Logi tõrkeotsing | Microsoft Azure'i" 
    description="Tõrkeotsing BizTalki teenuste Logi abil. MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalki Services: Kasutamise Logi tõrkeotsing

## <a name="what-are-the-operation-logs"></a>Mis on toiming logid
Logi on halduse teenused funktsioon Azure klassikaline portaalis, mis võimaldab teil vaadata ajalooliste logid oma Azure'i teenuste, sealhulgas BizTalki toimingutest. See võimaldab teil seotud toimingute BizTalki teenuse tellimuse juba 180 päeva ajalooliste andmete kuvamiseks.

> [AZURE.NOTE]See funktsioon sisaldab ainult logid toimingute BizTalki teenuseid, nt teenuse käivitamise, toetab kuni, jne. Selliste toimingute jälgitakse sõltumata sellest, kas neid tehakse Azure klassikaline portaali või [BizTalki teenuse REST API-de](http://msdn.microsoft.com/library/azure/dn232347.aspx)kaudu. Toimingute kohta, mida jälgitakse halduse teenuste kaudu täieliku loendi leiate [Toimingute jälitatud abil Azure'i halduse teenused](#bizops).<br/><br/>
See ei jäädvustada logid jaoks seotud BizTalki teenuse runtime (nt sõnumi töödelda sillad jne.). Need logid vaatamiseks jälitus vaate portaalist BizTalki teenuste kasutamine. Lisateavet leiate teemast [Sõnumite jälgimine](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>Vaate BizTalki teenuste logid toiming
1. Azure'i klassikaline portaalis valige **Halduse teenused**ja seejärel valige **Logi** menüü.
2. Saate filtreerida erinevate parameetrite nagu tellimus, kuupäevavahemik, teenuse tüüp (nt BizTalki teenused), teenuse nimi või toimingu (õnnestus, nurjus) olekut logid.
3. Valige filtreeritud loendi vaatamiseks märke. Järgmisel pildil on kujutatud seotud testbiztalkservice:  ![Logi vaatamine][ViewLogs] 
4. Kui soovite vaadata veel kohta teatud toimingu, valige see rida ja allosas tegumiribal nuppu **üksikasjad** .


## <a name="bizops"></a>Toimingute jälitatud Azure halduse teenuste kaudu
Järgmises tabelis on loetletud toimingud, mida jälgitakse Azure'i halduse teenuste kasutamisel:

Toimingu nimi | Ülesanne
--- | ---
CreateBizTalkService | Uue BizTalki teenuse toiming
DeleteBizTalkService | BizTalki teenuse kustutamine toimingut
RestartBizTalkService | Toiming BizTalki teenuse taaskäivitamiseks
StartBizTalkService | Toiming BizTalki teenuse käivitamine
StopBizTalkService | Toiming BizTalki teenuse peatamine
DisableBizTalkService | Toiming BizTalki teenuse keelamine
EnableBizTalkService | Tegevust, et lubada BizTalki teenus
BackupBizTalkService | Toimingu BizTalki teenuse varundamine
RestoreBizTalkService | Toimingu BizTalki teenuse määratud varukoopia põhjal taastamine
SuspendBizTalkService | Toiming BizTalki teenuse peatada.
ResumeBizTalkService | Toimingu jätkamiseks BizTalki teenus
ScaleBizTalkService | Toiming mastaapimiseks BizTalki teenuse üles või alla
ConfigUpdateBizTalkService | Värskendamise BizTalki teenuse konfiguratsiooni toiming
ServiceUpdateBizTalkService | Täiendamine või alandada BizTalki teenuse teise versiooni toiming
PurgeBackupBizTalkService | Toimingu likvideerite varukoopiate BizTalki teenuse väljaspool säilitusperiood


## <a name="see-also"></a>Vt ka
- [Varukoopia BizTalki teenus](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalki teenuse varukoopia põhjal taastamine](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalki Services: Arendaja, Basic, Standard- ja Premiumi väljaannetes diagrammi](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalki Services: Ettevalmistamise abil Azure'i klassikaline portaal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalki Services: Ettevalmistamise olek diagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalki teenuste: ahendamine](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalki Services: Väljaandja nimi ja väljaandja võti](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
