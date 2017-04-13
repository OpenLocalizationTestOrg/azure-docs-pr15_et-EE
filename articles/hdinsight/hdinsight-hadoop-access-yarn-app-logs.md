<properties
    pageTitle="Accessi Hadoopi LÕNG rakenduse logib programmiliselt | Microsoft Azure'i"
    description="Accessi rakenduse logib programmiliselt Hadoopi kobar Hdinsightiga sisse."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Accessi LÕNG rakenduse logib Windowsi-põhiste Hdinsightiga

See teema selgitab, kuidas juurde pääseda logid LÕNG (veel mõne muu ressursi läbirääkija) rakendusi, mis on lõpule jõudnud, Hadoop klaster rakenduses Windows Azure Hdinsightiga

> [AZURE.NOTE] Selle dokumendi teave kehtib ainult Windowsi-põhiste Hdinsightiga kogumite. Teavet juurdepääs LÕNG logib Linux-põhine Hdinsightiga kogumite, kohta leiate [LÕNG Accessi rakenduste logib Linux-põhine Hadoopi Hdinsightiga](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Eeltingimused

- Windowsi-põhiste Hdinsightiga kobar.  Lugege teemat [loomine Windowsi-põhiste Hadoopi kogumite Hdinsightiga sisse](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>LÕNG ajaskaala Server

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">LÕNG ajaskaala serveri</a> teave üldise lõplikus rakenduste kui ka raamistiku kohased teenuserakenduse teabe kahe eri liideste kaudu. Täpsemalt:

* Üldise rakenduse teabe Hdinsightiga kogumite ja on lubatud versiooniga 3.1.1.374 või uuem versioon.
* Raamistiku konkreetse rakenduse teabe osa ajaskaala Server pole praegu saadaval Hdinsightiga kogumite.


Üldist teavet rakenduste sisaldab järgmist tüüpi andmete.

* Rakenduse ID, rakenduse ainuidentifikaator
* Rakenduse käivitanud kasutaja
* Katsete lõpuleviimiseks rakenduse kohta
* Ümbriste, kasutada mis tahes rakenduse katse

Oma Hdinsightiga kogumite, klõpsake seda teavet talletatakse Azure'i ressursihaldur ajalugu salvestada vaikimisi ümbrises vaikimisi Azure Storage konto. Üldise andmed lõplikus rakendused saate tuua REST API kaudu:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>LÕNG rakenduste ja logid

LÕNG toetab mitut programmeerimise mudelite (MapReduce on üks neist) lahutamine ressursside haldamine kaudu rakenduse plaanimine ja jälgimine. Seda tehakse globaalne *ResourceManager* (RM), kohta töötaja sõlm *NodeManagers* (uute liikmesriikide) ja rakenduse kohta *ApplicationMasters* (AMs) kaudu. Rakenduse kohta EL negotsieerib ressursid (CPU, mälu, vaba, võrgu) soovitud RM. töötavad rakenduse jaoks Uute liikmesriikide anda need ressursid, mis on antud *ümbriste*RM töötab. AM vastutab ümbriste, on RM. poolt määratud edenemise jälgimine Rakendus võib olla vaja palju ümbriste sõltuvalt rakenduse.

Lisaks rakendusele võib koosneda mitme *rakenduse avaldab* lõpetamiseks juuresolekul jookseb või mõne EL suhtlemine vähenemist ja mõne RM. Seega ümbriste antakse teatud katse rakenduse. Mõttes ümbris pakub põhiühik LÕNG rakenduse töö kontekst ja ümbris raames tehtud töö sooritatakse sõlme ühe töötaja kohta, mis on eraldatud ümbris. Vt [LÕNG põhimõtet] [ YARN-concepts] edaspidiseks kasutamiseks.

Probleemne Hadoopi rakenduste silumine on logid (ja seotud container logid). LÕNG raamistik kena kogumiseks, liitmise ja talletamise logid koos [Log koondamine] [ log-aggregation] funktsiooni. Funktsiooni Log koondamine muudab juurdepääsul logid rohkem tarkadeks, sest see üle kõik ümbriste töötaja sõlme logide liitmise ja salvestab need ühe liidetud logifaili ühe töötaja sõlme failisüsteemi vaikimisi, kui rakendus on lõpule viidud. Rakenduse võib kasutada sadu või tuhandetele ümbriste, kuid alati liidetakse kõik ümbriste käivitada ühe töötaja sõlme logisid, ühte faili, tulemuseks ühe töötaja sõlme teie rakendus kasutab üks logifail. Log koondamine on vaikimisi Hdinsightiga kogumite kohta (versiooni 3.0 ja uuemad), ja liidetud logid leiate vaikimisi ümbrises, klaster järgmises asukohas:

    wasbs:///app-logs/<user>/logs/<applicationId>

Asukoht, *kasutaja* on rakenduse käivitanud kasutaja nimi ja *applicationId* on rakenduse määratud LÕNG RM. ainuidentifikaator

Liidetud logid pole otse loetav, nagu need on kirjutatud mõne [TFile][T-file], [kahendvormingus] [ binary-format] indekseeritud container järgi. LÕNG CLI tööriistade abil dump need logid või huvide ümbriste lihttekstina. Saate vaadata nende logid nagu lihttekstiks, käivitades järgmised lõnga käske otse kobar sõlmed (pärast selle kaudu RDP ühenduse):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>LÕNG ResourceManager kasutajaliides

LÕNG ResourceManager UI käivitatakse kobar headnode ja pääseb Azure portaali armatuurlaud. 

1. [Azure'i portaali](https://portal.azure.com/)sisse logima. 
2. Vasakul menüüs, klõpsake nuppu **Sirvi**, klõpsake **Hdinsightiga kogumite**, klõpsake Windowsi-põhiste klaster, mis soovite juurdepääsu LÕNG logid.
3. Klõpsake menüü top **armatuurlaud**. Teile kuvatakse lehe uues brauseris avada vahekaardil nimega **Hdinsightiga päringu konsooli**.
4. Klõpsake **Hdinsightiga päringu konsooli**, **Lõng UI**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
