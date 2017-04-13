<properties
    pageTitle="Azure'i rakenduse teenusega ClearDB MySQL-i andmebaaside KKK | Microsoft Azure'i"
    description="Vastused levinud küsimustele Azure'i rakendust Service ClearDB MySQL-i andmebaaside kasutamise kohta."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Azure'i rakenduse teenusega ClearDB MySQL-i andmebaasid KKK

Sellest artiklist leiate vastused levinud küsimustele abil ja osta ClearDB MySQL-i andmebaaside Azure Web Apps.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Millised võimalused kas MySQL-i Azure on?

Teil on mitu võimalust:

* [ClearDB ühiskasutusse antud MySQL-andmebaasiga](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL-i Premium kogumite](/marketplace/partners/cleardb-clusters/cluster/)

* [Töötab ka Azure VM MySQL-i kobar](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Ühe eksemplari MySQL-i töötab ka Azure VM](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB on on MySQL-i majutusteenuse ja MySQL-i taristu haldab teie eest. Kui käivitate oma MySQL-i kobar või andmebaasi kohta on Azure virtuaalse masina, peate häälestamine MySQL-i server ja plaastritega ajakohastada.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Vaja krediitkaarti veebirakenduse + MySQL-i malli Azure'i turuplatsil?

See sõltub tellimusest, mida te kasutate. Järgnevalt mõned sagedamini kasutatavad tellimuse tüübid.

* [Maksa](/offers/ms-azr-0003p/): nõuab krediitkaardi, ja kui ostate krediitkaardilt oma makstud MySQL-andmebaasiga.

* [Tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/): sisaldab krediiti kasutamiseks koos Microsoft Azure'i teenuseid, kuid ei luba ostmine kolmanda osapoole ressursid. Muu tootja teenused või makstud MySQL-andmebaasiga peate kasutama lubatud krediitkaardi tellimuse ostmiseks. Web Apps, saate luua tasuta ClearDB MySQL-i andmebaasist.

* [MSDN-i tellimuse](/pricing/member-offers/msdn-benefits/) ja **MSDN-i arendaja testi maksta ajal minna**: sarnane tasuta prooviversioon MSDN-i tellimuse peab teil olema osta makstud MySQL-i lahendus ClearDB krediitkaarti.

* [Ettevõtte lepingu (EA)](/pricing/enterprise-agreement/): EA kliendid on arve esitada oma EA iga kvartali kõigi ostude Azure'i turuplatsi (kolmanda osapoole) eraldi, konsolideeritud arvel. Teil on arve väljaspool rahaliste kohustuse mis tahes turuplatsi ostmiseks. Pange tähele, et sel ajal, ei ole Azure'i poe saadaval klientidele, Aserbaidžaan, Horvaatia, Norra ja Puerto Rico. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): saate luua ainult tasuta ClearDB andmebaaside Web Apps. Ei ole piiratud arvu tasuta ClearDB MySQL-i andmebaasid saate luua. Pange tähele, et see teenus on mõeldud ainult prooviversiooni pole, mida kasutatakse tootmise veebirakenduste, on tasuta andmebaasid.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Miks mult $3,50 Web Appi + MySQL-i Azure'i turuplatsilt

Andmebaasi vaikevalik on Titan, mis on $3.50. Me ei kuva maksumus andmebaasi loomise ajal ja kogemata ostate te ei kavatse andmebaasi. Me üritavad võimalus täiustada, kuid seni peab enne nuppu **Loo** ja ressursside kasutamise alustamist kontrollige teie valitud hinnakirjad astme web appi ja andmebaasi.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Azure'i virtual oma arvutis töötab MySQL-i. Minu Azure web app saate ühendada oma andmebaasi?

Jah. Saate luua ühenduse oma veebirakenduse andmebaasi seni, kuni teie Azure VM andis Kaugpöördusteenuse veebirakendusse. Lisateavet leiate teemast [Virtual arvutisse installida MySQL](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Kus on riigid ClearDB Premium MySQL-i kogumite toetatud?

[ClearDB Premium MySQL-i kogumite](/marketplace/partners/cleardb-clusters/cluster/) on saadaval kõikides piirkondades Azure kogu maailmas, välja arvatud India, Austraalia, Brasiilia Lõuna ja Hiina.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Saate luua uue kobar enne andmebaasi loomiseks ClearDB premium kobar lahendusega?

Ei, luua tühja ClearDB kogumite ei toetata. Azure'i portaalis võimaldab teil luua andmebaasid kobar, mis võivad luua uus klaster samal ajal.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Kas ma tuleb hoiatada, kui proovin ClearDB MySQL-andmebaasiga, kus on kasutusel kustutamiseks üks minu rakendused?

Ei, Azure ei hoiatab teid, kui kustutate turuplatsi ostmine, mis sõltub rakenduse.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Piirkonnad saate luua ClearDB andmebaasidega?

Azure'i turuplats pole saadaval klientidele, kes osalesid Aserbaidžaan, Horvaatia, Norra või Puerto Rico. ClearDB ei ole saadaval järgmistes regioonides.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Millist hinnakirjad taseme valida tootmise web appi ja andmebaasi?

Kasutage Basic või hinnakirjad jõudmine Web Apps. ClearDB, soovitame Saturn või Jupiter leping. Vaadake üle funktsioonid ja piirangud iga hinnakirjad taseme nii [Veebirakendustes](/pricing/details/app-service/) ja [ClearDB MySQL-i andmebaaside](/marketplace/partners/cleardb/databases/) valida üks, mis vastab teie vajadustele.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Kuidas uuendada oma ClearDB andmebaasi ühe lepingult teisele?

[Azure'i portaalis](https://portal.azure.com)saate skaalal ClearDB majutusteenuse ühisandmebaasiga. Lugege lisateavet selle [artikli](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) . Me praegu ei toeta versiooniuuenduse jaoks ClearDB Premium kogumite Azure'i portaalis.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Ma ei näe minu ClearDB andmebaasi Azure portaali?

Kui ClearDB andmebaasi Azure ressursihaldur või [Uue Azure portaali](https://portal.azure.com)abil koostamiseks see ei ole nähtav, [Vana Azure portaali](https://manage.windowsazure.com). Töö – selle ümber on andmebaasi käsitsi linkimiseks web appi. Samamoodi kui ClearDB andmebaasi loomine [vana portaali](https://manage.windowsazure.com) te ei saa vaadata oma andmebaasi [Uue Azure portaali](https://portal.azure.com). Ei, ei tööta ümber viimase stsenaariumi.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Kes ühendust tugiteenuste kui minu andmebaas ei tööta?

Kontakti [ClearDB toetavad](https://www.cleardb.com/developers/help/support) iga andmebaasi seotud probleemid. Saatke neile Azure tellimuse teabe valmis.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Saate luua täiendavate kasutajate minu ClearDB MySQL-i andmebaasi kobar lahenduse? 

Ei. Te ei saa luua täiendavaid kasutajad, kuid saate luua uute andmebaaside klaster ClearDB andmebaasi.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Kas Basic/Pro sarja andmebaaside olema täiendada kohapealne sarnane planeetide lepingute täna ClearDB portaali?

Jah, tavaline sarja andmebaasid võivad olla uuendatud kohapealne (tavaline 60 kaudu lihtne 500). Pro sarja võib olla uuendatud kohapealne (Pro 125 kaudu Pro 1000) välja arvatud Pro 60. Me ei toeta praegu Pro 60 loodud andmebaasi täiendamine. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Kui migreerida minu ressursid ühe tellimuselt teisele, kas minu ClearDB MySQL-andmebaasiga saada ka viiakse? 

Ressursi migreerimise täitmisel tellimustes rakendada teatud [piirangud](./app-service-web/app-service-move-resources.md) . ClearDB MySQL-i andmebaas on kolmanda osapoole teenusele ja seega ei saada viida Azure tellimuse migreerimisel. Kui haldate MySQL-i andmebaasi migreerimine Azure ressursid enne migreerimise, saab selle keelata oma ClearDB MySQL-i andmebaasid. Käsitsi migreerimine andmebaaside esmalt ja seejärel migreerimist Azure tellimuse veebirakenduse jaoks. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Üleminek ClearDB andmebaasi krediitkaardi tellimuse EA tellimusse?

Olemasolevate ClearDB andmebaaside kasutamine olemasoleva tellimustega seotud krediitkaardi. Tellimuse EA, peate oma andmete migreerimiseks uue andmebaasi kasutamiseks tehke järgmist.

* Uue andmebaasi ClearDB EA tellimus osta.
* Uue andmebaasi andmete migreerida.
* Uue andmebaasi kasutama rakendust värskendada.
* Kustutage vana ClearDB andmebaasi.

Uue veebirakenduse loomine MySQL-i (ClearDB) või loomisel MySQL-andmebaasiga (ClearDB), valite tellimuse määratleb, kuidas maksate teenus. EA tellimusega me ei blokeeri kolmanda osapoole teenused, nagu ClearDB Azure'i portaalis hankimine. EA tellimused on väljaspool rahaliste kohustuse arve ja on arve kvartali ja võlgu. Kliendi EA oleks häälestada makseviis, nt krediitkaart mis tahes muu turuplatsi teenuste eest maksta.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Kui näete ClearDB ressursside EA tellimuse eest?

Otsest EA klientide puhul on nähtav ettevõtteportaali Azure'i turuplatsi kulude. Pange tähele, et kõik turuplatsi ostude ja tarbimine on väljaspool rahaliste kohustuse arve on arve kvartali ja võlgu. EA kliendid on kolmanda osapoole teenusepakkujatele maksma ja saate seda teha, võimaldades makseviis, nt krediitkaart EA oma kontoga.

INDIRECT EA kliente saate otsida oma Azure'i turuplatsi tellimusi ettevõtteportaali lehel **Tellimuste haldamine** , kuid hinnad on peidetud. Klientide pöörduma oma LSP Lisateavet turuplatsiga kulud.

Muu tootja teenused Azure'i turuplatsi juurdepääsu saab hallata oma EA Azure'i registreerimine administraatorid. Need saate keelata või uuesti lubada juurdepääsu 3 tootja ostude haldamine kontod ja tellimuste poest ettevõtteportaali jaotises kontod.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Kes ühendust küsimusteks ettevõttetellimuse ClearDB teenuste kohta minu EA tellimus?

Pöörduge seoses arveldamine oma EA registreerimine jaotises [Ettevõtte klienditoe poole](http://aka.ms/AzureEntSupport) . EA portaali tugimeeskonna on oma küsimusele vastata või probleemi lahendamiseks.

 



## <a name="more-information"></a>Lisateave

[Azure'i turuplats KKK](/marketplace/faq/)
