<properties
    pageTitle="Mis on uut Azure'i virnas | Microsoft Azure'i"
    description="Mis on uut Azure'i virnas"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Mis on uut rakenduses Azure virnas Technical Preview 2
See väljaanne pakub uute funktsioonide nii rentnikud ja administraatorite jaoks.

## <a name="network"></a>Võrgu   
   - [iDNS](azure-stack-understanding-dns-in-tp2.md) pakub sisevõrk registreerimiseks ja süsteemi (DNS) eraldusvõime ilma täiendavate DNS-i taristu.
   - [Virtuaalne võrgu lüüside](azure-stack-create-vpn-connection-one-node-tp2.md) pakuvad virtuaalse Privaatvõrgu ühendus Azure suvandid või kohapealse ressursid.
   - Kasutaja määratletud marsruudib saate marsruutida võrguliikluse tulemüürid, Turve, muud seadmed ja muude teenuste kaudu.
   - Saate luua võrgu ressursse turuplatsilt.   

## <a name="storage"></a>Salvestusruumi
 - [Azure'i järjekorrad](https://msdn.microsoft.com/library/dd179353.aspx) luba usaldusväärsed ja püsivate teenuse sõnumside.
 - [Salvestusruumi analytics](https://msdn.microsoft.com/library/azure/hh343270.aspx) jäädvustada salvestusruumi jõudlusandmeid. Andmete abil saate jälgida taotlusi, analüüsida kasutamise ja diagnoosimine probleeme salvestusruumi kontoga.
 - Bloobimälu toetab [lisamine blokeerimise toiming](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Premium mälu API konto tugi.
 - Rentniku talletusmahu teenuse tugi levinud tööriistad ja SDK-d, nt Azure CLI, PowerShelli, .NET, Python ja Java SDK. 
 - [Salvestusruumi konto ühiskasutusse Accessi allkirja](https://msdn.microsoft.com/library/azure/mt584140.aspx) lubada juurdepääsu oma salvestusruumi Varundustöö delegeerimine ilma jagada oma täielik konto võti.  
 - Salvestusruumi teenuste nüüd kasutada [Rühmakontod hallatav teenus](https://technet.microsoft.com/library/hh831477.aspx) tugev väärtpaberi madal halduse pea kohal.
 - Saate nõuda kasutamata rentniku ressursid tellitavate.
 - Kustutatud salvestusruumi konto säilituspoliitikate pikkus saab konfigureerida.
 - Saate taastada kustutatud rentniku talletusmahu kontod.

## <a name="compute"></a>Arvuta
- Saate deallocate virtuaalmasinates.
- Te saate ümberkorraldamine virtuaalse masina laiendid tõrkeotsing ja konfiguratsiooni juhtimise eesmärgil.
- Virtuaalse masina ketast, saate selle suurust.
- Virtuaalmasinates võib olla mitu võrgu liidesed.

### <a name="portal-experience"></a>Portaali kogemus
 - Azure'i virnas regioonides on loogilise üksuse ulatuse ja juhtimise Azure'i virnas. Selles eelvaates saate vaadata teavet teenuseid nagu Arvuta, võrk ja salvestusruumi regiooniti.
 - Nüüd saate vaadata ka (värskendused) [azure-virnlintdiagrammil-updates.md] kasutajaliidese.

## <a name="key-vault"></a>Võtme Vault
- [Klahv Vault Azure'i virnas](azure-stack-kv-intro.md) annab turvaline juhtimine võtmed ja paroolide pilve rakendused.
- Saate kontrollida ja jälgida võtme kasutamine rakenduste ja VMs.

## <a name="billing-and-usage"></a>Arveldus- ja kasutamine
 - Kohta, kuidas oma teenuste tarbitud andmete esitamist [arveldus- ja tarbimine API-d](azure-stack-billing-and-chargeback.md) .  
 - Delegeeritud pakkujate lubada edasimüüjate Azure'i virnas teenuste pakkuda oma klientidele.

## <a name="monitoring-and-health"></a>Jälgimine ja seisund
 - Uute võimaluste saadaval portaali ja API-de jälgimine võimaldab teil aktiivselt vaadata ja hallata keskkonna teatised.  

## <a name="next-steps"></a>Järgmised sammud
- [Azure'i virnas POC arhitektuur mõistmine](azure-stack-architecture.md)      
- [Juurutamise eeltingimused mõistmine](azure-stack-deploy.md)
- [Azure'i Virnlintdiagrammil juurutamine](azure-stack-run-powershell-script.md)

  
