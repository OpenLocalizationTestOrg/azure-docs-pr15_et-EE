
<properties 
   pageTitle="Azure'i SDK .net-i 2,8 väljalaskemärkmed" 
   description="Azure'i SDK .net-i 2,8 väljalaskemärkmed" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure'i SDK 2,8, 2.8.1 ja 2.8.2 .net-i jaoks

##<a name="overview"></a>Ülevaade
 
Käesolevast artiklist leiate Azure'i SDK (mis sisaldab teadaolevate probleemide ja server muudatused) väljalaskemärkmed jaoks .NET 2,8, 2.8.1 ja 2.8.2 välja. 

Uued funktsioonid ja selles versioonis tehtud värskenduste täieliku loendi leiate teemast [Azure SDK 2,8 Visual Studio 2013 ja Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) teadaanne. 

##  <a name="azure-sdk-for-net-28"></a>Azure'i SDK .net-i 2,8

### <a name="download-azure-sdk-for-net-28"></a>Laadige alla Azure SDK .NET 2,8

[Azure'i SDK .net-i 2,8 Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure'i SDK .net-i 2,8 Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>.NET 4.5.2 tugi 

####<a name="known-issues"></a>Teadaolevad probleemid

Azure'i .NET SDK 2,8 saate luua .NET 4.5.2 pilveteenuses paketid. Kuid .NET 4.5.2 framework ei installita vaikimisi Külastajate OS piltide jaanuar 2016 Külastajate OS versioon. Enne seda, 4.5.2 .NET framework allalaadimisvõimalused eraldi Külastajate OS väljalastud versiooni – November 2015 02. Teemast [Azure Külastajate OS ja SDK ühilduvus maatriksi](../cloud-services/cloud-services-guestos-update-matrix.md) lehe jälgimiseks, kui ilmub pilt.  Kui November 2015 02 pilt on vabastatud saate kasutada seda pilti, värskendades oma pilveteenuses konfiguratsioonifail faili (.cscfg). Teenuse konfiguratsiooni faili määrata osVersion atribuut ServiceConfiguration elemendi stringiga "WA-Külastajate-OS-4.26_201511-02". Kui otsustate kasutada seda pilti, siis saate enam automaatvärskenduste Külastajate OS valida. Automaatvärskendusi selle osVersion peab olema seatud "*" ja .NET 4.5.2 on saadaval ainult jaanuar 2016 automaatvärskenduste kaudu.

###<a name="azure-data-factory"></a>Azure'i andmed Factory

####<a name="known-issues"></a>Teadaolevad probleemid 

**Andmete Factory malli** projekti loomisel seotud näidisandmed, ei pruugi azure power shell script kui azure power shell versioon arvutisse installitud on pärast 0.9.8.

Seda tüüpi projekti edukalt loomiseks tuleb teil installida [azure power shell versioon 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Azure'i ressursihaldur tööriistad 

####<a name="breaking-changes"></a>Suurte muudatuste

Selles versioonis uut Azure PowerShelli cmdletid versiooni 1.0 töötamiseks on värskendatud PowerShelli skripti Azure ressursirühm projekti järgi.  Selle uue script ei tööta kaudu Visual Studio SDK 2,8 varasema versiooni kasutamisel.  

Projektide SDK varasemates versioonides loodud skriptide ei tööta jooksul Visual Studio 2,8 SDK kasutamisel.  Kõik skriptid jätkuvalt sobiv versioon Azure PowerShelli cmdletid väljaspool Visual Studio töötamine.  

2,8 SDK jaoks on vaja versiooni 1.0 Azure PowerShelli cmdlet-käsud.  Kõik muud SDK versioonid nõuavad Azure PowerShelli cmdletid 0.9.8 versiooni.  [Lisateabe saamiseks vt.](http://go.microsoft.com/fwlink/?LinkID=623011)

###<a name="web-tools-extensions"></a>Web tööriistad laiendid

####<a name="known-issues"></a>Teadaolevad probleemid

Järgmised teadaolevad probleemid käsitletakse järgmisi väljaanne.

- Rakenduse teenusega seotud pilveteenuste ja Server Explorer žest mitte tekitamiseks keskkonnas (nt Hiina Azure'i või Azure virnas kliendid) ei tööta. Klientide kannatavas nendes, võimaldab avalda profiili allalaadimisega Azure portaali avaldamise võimalus. Tulevaste väljalasete parandab žeste, näiteks "Manustamine Silur" ja "Kuva Streaming logid" Azure Hiina ja Virnlintdiagrammil kliendid. 
- Kliendid, võidakse kuvada tõrgete ajal rakenduse teenuse loomine kui ülevaateid rakenduse juurutamine nad on näiteks on Ida-USA muus piirkonnas. Need stsenaariumid portaalis teenuse rakenduse loomine ja avaldamine profiili allalaadimine võimaldab avaldamise stsenaariumid. 

###<a name="azure-hdinsight-tools"></a>Azure Hdinsightiga tööriistad

####<a name="new-updates"></a>Uued värskendused

- Saate oma taru päringut koos peaaegu ei pea kohal kaudu HiveServer2 klaster ning vaadata töö logid reaalajas.
- Uue taru tööülesande täitmise vaate te saate tõrkeid tööpäevad süvitsi uurida abil Täpsem teave ja võimalikud probleemid.

Lisateavet leiate teemast [Azure SDK 2,8 Visual Studio 2013 ja Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure'i SDK .net-i 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Teadaolevad probleemid Visual Studio 2013 ja Visual Studio 2015
 
1. Käivitatud WebJob avaldab slots saavad Kuva- ja tõrketeated ja ei määra ajakava, kuid lükake soovitud WebJob Azure. Kliendid, kes vajavad ajastatud töö saate Azure portaali häälestamiseks on WebJob ajakava. 
2. Python kliendid võivad ilmneda probleemid Silur. Meeskond on jooksvalt läbi määravad selle, kuid kui mõjutab kliendid, andke sellest teada foorumites või teadaanne ajaveebi või vabastamine kommentaarid märkmealale Microsoft. 
3. Teatud piirkondades (nt Lõuna India) klientide kogemus rakenduse teenuse ettevalmistamise tõrkeid. See on kooskõlas portaali ja klientidele, kes probleemi saate kasutada Azure portaali avaldamine regioonides geo juurdepääsu taotlemine. Kui nad regioonides kaudu juurdepääsu taotlemise Azure portaali ettevalmistamise peaks töötama. 

##<a name="azure-sdk-for-net-282"></a>Azure'i SDK .net-i 2.8.2

Pärast installi selle 2.8.2 tööriistad, kliendid võivad ilmneda järgmine probleem.         

- Kui kasutate Windows 10 ja olete installinud Internet Explorer, võite saada tõrke "Internet Explorer ei leitud".
Probleemi lahendamiseks installige Internet Exploreri dialoogiboksi Windowsi komponentide lisamine või eemaldamine kaudu.

Kui märkate, et see probleem, funktsiooni saada naeratus seda teatada.

Lisateabe saamiseks vt [selle](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) postitada.
##<a name="other-updates"></a>Muud värskendused

Muud värskendused leiate [Azure'i SDK 2,8 teadaanne postitada](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Vt ka

[Azure'i SDK 2,8 teadaanne postituse](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Tugi- ja pensionile jäämise teavet Azure SDK .net-i ja API-de jaoks](https://msdn.microsoft.com/library/azure/dn479282.aspx)

