<properties
    pageTitle="Mis on Azure automatiseerimine | Microsoft Azure'i"
    description="Siit saate teada, milline väärtus leiate Azure'i automatiseerimine ja saada vastuseid levinud küsimustele, nii et saate luua, kasutamise alustamisel tegevusraamatud ja Azure automatiseerimine DSC."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="mis on automaatika, Azure'i automaatika, azure automatiseerimine näiteid"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure'i automaatika ülevaade

Microsoft Azure'i automatiseerimine võimaldab käsitsi, pikaajalisi, vigu ja sageli korduvate ülesannete, mis on tavaliselt läbi pilve ja ettevõtte keskkonnas automatiseerimiseks kasutajate jaoks. See säästab aega ja suurendab tavalise haldustoiminguid usaldusväärsuse ja isegi koosolekut kavandava need tehakse automaatselt intervalliga. Saate protsesside tegevusraamatud abil automatiseerida või konfiguratsiooni juhtimine soovitud riik konfiguratsiooni abil automatiseerida. Selles artiklis antakse lühiülevaade Azure automatiseerimine ja leiate vastused levinud küsimustele. Saate selle teegi üksikasjalikumat teavet eri teemade kohta teiste artiklite viidata.


## <a name="automating-processes-with-runbooks"></a>Tegevusraamatud protsesside automatiseerimine

Mõne käitusjuhendi on toimingud, mis sooritavad teatud automatiseeritud protsessi Azure'i automaatika kogum. See võib olla näiteks alates virtuaalse masina ja loomine logikirjet lihtne toiming või teil võib olla keeruline käitusjuhendi, mis ühendab muude väiksemate tegevusraamatud keerukas protsess täitmiseks üle mitme ressursid või isegi mitmest pilved ja eeldusel keskkonnas.  

Näiteks teil võib on olemasoleva käsitsi protsess Kärbi SQL-andmebaasi, kui see on lähenemas maksimaalne maht, mis sisaldab mitut juhiseid, nt serveriga, andmebaasiga ühenduse loomisel, saada praeguse andmebaasi mahtu, märkige ruut, kui lävi on ületanud ja seejärel kärpida selle ja teavitamise kasutaja. Asemel käsitsi neid juhiseid iga, võiksite luua käitusjuhendi, mis oleks kõiki neid toiminguid sooritada ühe protsess. Saate alustada käitusjuhendi, sisestage nõutav teave, näiteks SQL serveri nimi, andmebaasi nimi ja adressaadi e-posti ja istuda siis, kui protsess jõuab lõpule. 


## <a name="what-can-runbooks-automate"></a>Mida saab tegevusraamatud automaatseks muuta?

Azure'i automaatika tegevusraamatud põhinevad Windows PowerShelli või Windows PowerShelli töövoo, nii et need tee midagi, mida saab teha PowerShelli. Kui rakenduse või teenuse API, siis on käitusjuhendi saate töötada see. Kui teil on rakenduse PowerShelli mooduli, saate selle mooduli laadimine Azure automatiseerimine ja lisada need cmdlet-käsud oma käitusjuhendi. Azure'i automaatika tegevusraamatud Käivita Azure'i pilves ja mis tahes pilve ressursid või välised ressursid, millele pääseb pilvest juurde. [Hübriidjuurutuse Käitusjuhendi töötaja](automation-hybrid-runbook-worker.md)kasutamisel saab käitada tegevusraamatud kohalikud andmed keskuses hallata kohalikud ressursid. 


## <a name="getting-runbooks-from-the-community"></a>Ühenduse tegevusraamatud hankimine

[Käitusjuhendi Galerii](automation-runbook-gallery.md#runbooks-in-runbook-gallery) sisaldab tegevusraamatud Microsofti ja ühenduse, mida saate kasutada muutmata teie keskkonnas või need teie enda kohandamine. Need on kasulik teada, kuidas luua oma tegevusraamatud viidetena. Saate isegi kaasa oma tegevusraamatud, mis te arvate, et teised kasutajad kasulik galeriisse. 


## <a name="creating-runbooks-with-azure-automation"></a>Azure'i automaatika tegevusraamatud loomine 

Saate [luua oma tegevusraamatud](automation-creating-importing-runbook.md) algusest peale ise luua või muuta tegevusraamatud [Käitusjuhendi Galerii](http://msdn.microsoft.com/library/azure/dn781422.aspx) oma nõuded. On kolme erineva [käitusjuhendi tüübid](automation-runbook-types.md) , mida saate valida vastavalt teie vajadustele ja PowerShelli kogemus. Kui soovite töötada PowerShelli koodi, saate kasutada [PowerShelli käitusjuhendi](automation-runbook-types.md#powershell-runbooks) või redigeerite ühenduseta või [teksti redaktor](http://msdn.microsoft.com/library/azure/dn879137.aspx) Azure'i portaalis [PowerShelli töövoo käitusjuhendi](automation-runbook-types.md#powershell-workflow-runbooks) Kui eelistate redigeerimiseks on käitusjuhendi ilma aluseks koodi, saate luua [graafilise käitusjuhendi](automation-runbook-types.md#graphical-runbooks) abil [pildiredaktor](automation-graphical-authoring-intro.md) Azure'i portaalis. 

Kas eelistate vaadates lugemist? Heitke pilk selle video Microsoft Ignite seanss mai 2015 all. Märkus: Mõisted ja funktsioone, mis on kirjeldatud seda videot, aga õige Azure automatiseerimine on jõudnud palju, kuna see video on salvestatud, nüüd on rohkem UI Azure'i portaalis, ning toetab täiendavaid võimalusi.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Toimingute automatiseerimine konfiguratsiooni juhtimine soovitud riik konfigureerimine 

[PowerShelli DSC](https://technet.microsoft.com/library/dn249912.aspx) on halduse platvorm, mis võimaldab teil hallata, juurutada ja Jõusta konfiguratsioon füüsilise hosts ja virtuaalmasinates deklaratiivseid PowerShelli süntaksi kasutamine. Saate määratleda konfiguratsioone keskse DSC tõmmata serveris, kus target masinad automaatselt tuua ja rakendada. DSC annab PowerShelli cmdlet-käsud, mille abil saate hallata konfiguratsioone ja ressursid.  

[Azure'i automaatika DSC](automation-dsc-overview.md) on pilvepõhine lahendus PowerShelli DSC, mis osutab enterprise keskkonna jaoks nõutav.  Saate hallata oma DSC ressursside Azure automatiseerimine ja rakendada konfiguratsioone virtuaalse või füüsilise masinad, mis toomiseks DSC tõmmata Server Azure'i pilveteenuses.  See sisaldab ka aruandlusteenuste, mis teatavad, et saate olulised sündmused sõlmed on määratud piirid väljunud ja kui uue konfiguratsiooni on rakendatud. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Luua oma DSC konfiguratsioone Azure automatiseerimine

[DSC konfiguratsioone](automation-dsc-overview.md#azure-automation-dsc-terms) määrake soovitud sõlme.  Mitme sõlmed saate rakendada sama konfiguratsiooni kinnitamaks, et need kõik säilitamiseks on identne olekus.  Saate luua abil teksti konfiguratsiooni editor oma kohalikus arvutis ja seejärel importige Azure'i automaatika, kus te saate koostada ja rakendada selle sõlmed.


## <a name="getting-modules-and-configurations"></a>Saada moodulid ja konfiguratsioone 

Saate [PowerShelli moodulid](automation-runbook-gallery.md#modules-in-powershell-gallery) , mis sisaldab cmdlet-käsud, mille abil saate oma tegevusraamatud ja DSC konfiguratsioone [PowerShelli Galerii](http://www.powershellgallery.com/). Saate käivitada selle galerii Azure'i portaalis ja moodulid otse Azure automatiseerimine importida või saate alla laadida ja neid käsitsi importida. Te ei saa installida moodulid otse Azure portaali, kuid võite alla laadida installige need muu moodul tavapärasel viisil. 


## <a name="example-practical-applications-of-azure-automation"></a>Näide praktiliste rakenduste Azure automatiseerimine. 

Järgnevalt on vaid mõned näited, mis on selle kohta, milliseid automatiseerimise stsenaariumid Azure automatiseerimine. 

* Luua ja kopeerige virtuaalmasinates erinevate Azure'i tellimused. 
* Ajakava faili kopeerib kohalikust arvutist mõne Azure'i bloobimälu ümbrises. 
* Näiteks keelamiseks taotleb kliendi soovitud Teenustõkkerünnak tuvastamisel ülesandeid automatiseerida. 
* Veenduge, et masinad pidevalt joondamiseks konfigureeritud turbepoliitika.
* Pidev juurutamise rakenduse koodi üle pilve ja tööruumide taristu haldamine 
* Koostada Active Directory kogumis Azure lab keskkonnas. 
* Kui DB on lähenemas maksimaalne maht, kärpige SQL-i andmebaasi tabelisse. 
* Azure'i veebisait keskkonna sätete värskendamine kaugühenduse teel. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Kuidas Azure automatiseerimine seotud muude automaatika tööriistad?

[Teenuse automatiseerimine (SMA)](http://technet.microsoft.com/library/dn469260.aspx) eesmärk on privaatne pilveteenuses haldamisega seotud toiminguid automatiseerida. See on installitud kohalikult oma andmekeskuse [Microsoft Azure'i paketi](https://www.microsoft.com/en-us/server-cloud/)osana. SMA ja Azure automatiseerimine kasutada sama käitusjuhendi vormingu põhjal Windows PowerShelli ja Windows PowerShelli töövoo, kuid SMA ei toeta [graafiline tegevusraamatud](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) on mõeldud kohapealse ressursside automatiseerimine. See kasutab eri käitusjuhendi vormingut, kui Azure automatiseerimine ja teenuse halduse automatiseerimine ja on graafilist liidest nõudmata, mis tahes skriptimise tegevusraamatud loomiseks. Selle tegevusraamatud koosnevad tegevuste integreerimine tuvastamine, mis on kirjutatud spetsiaalselt Orchestrator kaudu. 


## <a name="where-can-i-get-more-information"></a>Kust leida lisateavet? 

Hulgaliselt ressursse on saadaval, saate lisateavet Azure automatiseerimine ja oma tegevusraamatud loomise kohta. 

* **Azure'i automaatika teegis** on, kust on kohe. See teek artikleid esitada täieliku dokumendid, konfigureerimise ja haldamise Azure'i automaatika ja loome oma tegevusraamatud. 
* [Azure'i PowerShelli cmdlet-käskude](http://msdn.microsoft.com/library/jj156055.aspx) leiate teavet Windows PowerShelli kaudu Azure'i toimingute automatiseerimiseks. Töö ressurssidega Azure abil tegevusraamatud need cmdlet-käsud. 
* [Juhtimise](https://azure.microsoft.com/blog/tag/azure-automation/) ajaveebis uusima teabe Azure automatiseerimine ja muid tehnoloogiaid Microsofti. Tellida peaks selle ajaga sammu pidamine – tarkvara ja Azure automatiseerimine meeskonna ajaveebi. 
* [Automaatika Foorum](http://go.microsoft.com/fwlink/p/?LinkId=390561) võimaldab teil esitada esitlusekohta küsimusi Azure automatiseerimine Microsoft ja automatiseerimise ühenduse lahendada. 
* [Azure'i automatiseerimine cmdlettide](https://msdn.microsoft.com/library/mt244122.aspx) leiate automatiseerimiseks haldustoimingute kohta. See sisaldab haldamiseks automatiseerimise kontod, varad, tegevusraamatud, DSC cmdlet-käsud.


## <a name="can-i-provide-feedback"></a>Saate anda tagasisidet? 

**Palun andke meile tagasisidet!** Kui otsite lahenduse Azure automatiseerimine käitusjuhendi või mõne integreerimine mooduli postitamiseks skripti, mis taotlemine skripti keskele. Kui teil on tagasiside või funktsiooni taotlusi Azure'i automaatika, postitage need [Kasutaja kõneposti](http://feedback.windowsazure.com/forums/34192--general-feedback). Tänud! 


