<properties
    pageTitle="Azure'i rakendust Service head tavad"
    description="Siit saate teada, head tavad ja Azure'i rakendust Service tõrkeotsing."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Azure'i rakendust Service head tavad

Selles artiklis antakse ülevaade headest tavadest [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714)abil. 

## <a name="colocation"></a>Colocation
Kui Azure ressursse, nt web appi ja andmebaasi lahenduse koostamise asuvad eri regioonide efektid võivad olla järgmised:

*  Suurem latentsus suhtlemine ressursid
*  Rahaliste kulude väljaminev liiklus andmete edastamiseks rist-piirkond nagu [Azure hinnakirjad lehe](https://azure.microsoft.com/pricing/details/data-transfers).

Piirkonna paigutamine on Azure ressursse, nt web appi ja kasutada sisu või andmete hoidke andmebaasi või salvestusruumi konto lahenduse koostamise jaoks kõige paremini. Veenduge, et ressursid loomisel nad on Azure piirkonna juhul, kui teil on teatud ettevõtetele või kujunduse põhjus neid ei saa. Rakendus App teenuse teisaldamiseks andmebaasi sama piirkonna kasutamine soovitud [rakendus teenuse kloonimine funktsiooni](app-service-web-app-cloning-portal.md) Premium rakenduse teenuse leping rakenduste praegu saadaval.   

## <a name="memoryresources"></a>Kui peab olema rakendused oodatust rohkem mälu
Kui märkate rakenduse tarbib jälgimise kaudu näidatud oodatust rohkem mälu või teenuse soovitused kaaluge [rakenduse teenuse automaatne probleemilahendus funktsiooni](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Üks suvanditest automaatne probleemilahendus funktsiooni võtab kohandatud toiminguid, mis on aluseks on mälu. Toimingud jooksul ringlussevõtu töötaja protsessi korrata käed kaudu meiliteatised uurimise kaudu mälutõmmis, et – kohapealsete vähendamiseks. Automaatne probleemilahendus saab konfigureerida ja sõbralik kasutajaliides, nagu on kirjeldatud aadressil sellest ajaveebipostitusest [Rakenduse teenuse toe saidi laiend](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)web.config kaudu.   

## <a name="CPUresources"></a>Kui peab olema rakendused oodatust rohkem CPU
Kui märkate, et rakendus tarbib CPU rohkem kui oodatud või kogemusi korrata CPU diagrammi näidatud jälgimise kaudu või teenuse soovitused kaaluge ülespoole või skaleerimist välja rakenduse teenusleping. Kui teie taotlus on statefull, ülespoole on ainus saadaolev variant, samal ajal kui teie taotlus on kodakondsuseta, skaleerimise väljas teile rohkem paindlikkust ja suurema skaala võimalusi. 

Lisateavet "statefull" vs "kodakondsuseta" rakenduste võite vaadata seda videot: [kavandamise Scalable End End mitmekihilise rakendus Microsoft Azure'i Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Rakenduse teenuse skaleerimist ja autoscaling suvandite kohta lisateabe saamiseks lugege: [skaala Web Appi teenuses Azure rakendus](web-sites-scale.md).  

## <a name="socketresources"></a>Kui turvasoklite ressursid on tühjad
Levinud põhjused kurnav TCP Väljaminevad ühendused on kliendi teegid, mida ei ole rakendatud uuesti kasutada TCP-ühenduste või kõrgema taseme protokolli HTTP - ülalhoidmise ei kasutada näiteks puhul kasutada. Palun vaadake üle iga viidatud rakendused oma rakenduse teenuse leping tagamaks, et need on konfigureeritud või koodi tõhusa korduskasutuseks väljaminevate ühenduste juurde teekide dokumentatsioonist. Ka siis järgige teegi dokumentatsiooni proper loomine ja väljalaske- või Kettapuhastus ärahoidmine ühendused lekib. Kui sellist kliendi teekide juurdlust, mis asuvad edenemise mõju võib leevendada skaleerimist välja mitmes eksemplaris.  

## <a name="appbackup"></a>Kui teie rakendus varundamine käivitamise puudumisel
Kaks kõige levinumad põhjused, miks rakenduse varukoopia nurjub: sobimatu talletusmahu ja sobimatud andmebaasi konfigureerimine. Tõrkeid ilmneda tavaliselt salvestusruumi või andmebaasi ressursid muudatused ja muudatuste kohta, kuidas juurdepääs järgmiste ressursside (nt mandaat värskendatud andmebaasi valitud varukoopia sätted). Varukoopiate tavaliselt Käivita ajakava ja nõua juurdepääsuks salvestusruum (kirjutamine varundatud failide) ja andmebaaside (kopeerimise ja sisu varukoopia kaasatavate lugemine). Tulemus ei õnnestu juurde pääseda, kas need ressursid oleks ühtsete varukoopia tõrge. 

Kui varukoopia tõrkeid ilmneda, palun vaadake üle viimase tulemused, et aru saada, millist tüüpi jätmine toimub. Puhul salvestusruumi juurdepääsu tõrkeid, vaadake üle ja kasutada varukoopia konfiguratsiooni salvestusruumi sätteid värskendada. Andmebaasi juurdepääsu tõrkeid, puhul läbi vaadata ja värskendada oma ühendused stringide osana rakenduse sätted; jätkake seejärel värskendada teie varukoopia konfiguratsiooni õigesti kaasata vajalikud andmebaasid. Rakenduse varundamise kohta lisateabe saamiseks leiate [Azure'i rakendust Service veebirakenduse varundamine](web-sites-backup.md) dokumentatsiooni.

## <a name="nodejs"></a>Kui uus Node.js rakendused on juurutatud Azure'i rakendust Service
Azure'i rakendust Service vaikimisi konfiguratsioon Node.js rakendused on mõeldud vastavalt vajadustele kõige levinum rakenduste. Kui konfigureerimine oma Node.js rakenduse kasu isikupärastatud häälestamine jõudluse parandamiseks või optimeerimine ressursikasutus CPU/mälu/võrgu ressursid, võib läbi vaadata meie parimate tavade ja tõrkeotsingutoiminguid. Dokumentatsiooni selles artiklis kirjeldatakse iisnode sätted võib-olla vaja konfigureerida oma Node.js rakenduse, kirjeldab erinevaid võimalusi või küsimusi, et teie rakendus võib vastastikuste ja näidatakse, kuidas nende probleemide lahendamiseks: [parimate tavade ja tõrkeotsingu juhend Azure'i rakendust Service sõlm rakendusi](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


