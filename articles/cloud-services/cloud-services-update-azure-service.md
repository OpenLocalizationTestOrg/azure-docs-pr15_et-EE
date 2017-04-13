<properties
pageTitle="Kuidas värskendada pilveteenus | Microsoft Azure'i"
description="Saate teada, kuidas värskendada pilveteenustega Azure. Siit saate teada, kuidas mõnda pilveteenusesse uuendatud tulu kättesaadavuse tagamiseks."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Kuidas värskendada pilveteenus

## <a name="overview"></a>Ülevaade
10 000 jalga, värskendamine pilveteenuses, sh rollid ja Külastajate OS, on kolme järgmist toimingut. Kõigepealt tuleb üles laadida kahendfaile ja failid uue pilveteenuses või Opsüsteemi versioon. Järgmiseks jätab Azure Arvuta ja ressursid pilveteenuses, mis põhinevad uude cloud versiooni jaoks. Lõpuks Azure'i teostab jooksva versiooniuuenduse ajal värskendada rentniku uus versioon või Külastajate OS, säilitades teie kättesaadavust. Selles artiklis käsitletakse see viimane toiming – jooksva versiooniuuenduse üksikasju.

## <a name="update-an-azure-service"></a>Azure'i teenuse värskendamine

Azure'i korraldab oma rolli aknad loogilise rühmitustega nimetatakse täiendamine domeenide (UD). Täiendamine domeenide (UD) on loogiline rolli aknad, mida on värskendatud rühmana.  Azure'i värskenduste pilv teenuse ühe UD korraga, mis võimaldab eksemplari muude UDs serveeritakse liikluse jätkamiseks.

Täiendamine domeenide vaikearvu on 5. Saate määrata täiendamine domeenide erinevat hulka, sh upgradeDomainCount atribuut teenuse definitsioonifail (.csdef). UpgradeDomainCount atribuudi kohta lisateabe saamiseks vt [WebRole skeemi](https://msdn.microsoft.com/library/azure/gg557553.aspx) või [WorkerRole skeem](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Kohapealse värskenduse üks või mitu rollide täitmisel teenust Azure värskendab komplekti rolli aknad vastavalt täiendamine domeeni, millesse nad kuuluvad. Azure'i värskenduste kõik eksemplarid antud täiendamine domeeni – peatada need, nende värskendamine, et need tagasi võrgus – liigub järgmise domeeni peale. Lõpetades ainult praegune täiendamine domeeni töötavate eksemplaride, Azure'i saab kontrollida, kas värskendus ilmneb abil võimalikult töötava teenusega. Lisateavet leiate artiklist [Kuidas värskenduse tulu](#howanupgradeproceeds) selle artikli.

> [AZURE.NOTE] Terminite, **värskendada** ja **uuendada** on veidi teistsugused tähendust kontekstis Azure, neid kasutada vaheldumisi protsesside ja selle dokumendi funktsiooni kirjeldused.

Teenust tuleb määratleda vähemalt kaks eksemplari rolli, värskendatakse selle rolli kohapealne ilma tööseisakute. Kui teenus sisaldab ainult ühte eksemplari ühes rolli, teenust saadaval juhul kuni kohapealse värskendamine on lõpule jõudnud.

Selles teemas käsitletakse Azure värskenduste kohta järgmine teave:

-   [Lubatud teenusemuudatusest ajal värskendus](#AllowedChanges)
-   [Kuidas uuendada tulu](#howanupgradeproceeds)
-   [Tagasipööramine värskendust.](#RollbackofanUpdate)
-   [Mitme muteeruv on poolelioleva juurutuse toimingute alustamist](#multiplemutatingoperations)
-   [Jaotuse versioonitäienduse domeenides rollid](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Lubatud teenusemuudatusest ajal värskendus
Järgmine tabel sisaldab teenuse lubatud muudatuste ajal värskendust.

|Muudatuste tegemine lubatud hosting teenuste ja rollide|Installige värskendus|Etapiviisilise (VIP vahetamine)|Kustuta ja uuesti juurutamine|
|---|---|---|---|
|Opsüsteemi versioon|Jah|Jah|Jah
|.NET usaldustase|Jah|Jah|Jah|
|Virtuaalse masina suurus<sup>1</sup>|Jah<sup>2</sup>|Jah|Jah|
|Kohaliku sätted|<sup>2</sup> suurendamine|Jah|Jah|
|Lisamine või eemaldamine rollid teenus|Jah|Jah|Jah|
|Kindla rolliga eksemplaride arv|Jah|Jah|Jah|
|Arvu või tüübi teenuse lõpp-punktid|Jah<sup>2</sup>|Ei|Jah|
|Nimed ning väärtused sätete konfigureerimine|Jah|Jah|Jah|
|Väärtused (kuid mitte nimed) sätete konfigureerimine|Jah|Jah|Jah|
|Lisage uus serdid|Jah|Jah|Jah|
|Muutke olemasoleva serdid|Jah|Jah|Jah|
|Uue koodi juurutamine|Jah|Jah|Jah|
<sup>1</sup> Suuruse muutmine ainult alamhulk suurused saadaval pilveteenusesse.

<sup>2</sup> Nõuab Azure'i SDK 1,5 või uuemad versioonid.

> [AZURE.WARNING] Virtuaalse masina suuruse muutmise hävitab kohalikud andmed.


Ajal värskendust ei toeta järgmisi üksusi:

-   Rolli nime muutmine. Eemalda ja seejärel lisage roll uue nimega.
-   Täiendamine domeeni arvu muutmine.
-   Kohalikud ressursid suuruse vähendamine.

Kui teete teiste värskenduste oma teenuse määratlus, nt suuruse kohaliku ressursi, peate tegema VIP Vaheta update asemel. Lisateabe saamiseks vt [Juurutamise vahetada](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Kuidas uuendada tulu
Saate otsustada, kas soovite värskendada kõik teie teenust rollid või kindla rolliga teenus. Mõlemal juhul on kõik eksemplarid iga rolli, mis uuendatakse ja esimese täiendamine domeeni kuuluvad lõpetanud, täiendanud ja tagasi toonud online. Kui nad on tagasi võrgus, on eksemplarid teise täiendamine domeeni lõpetanud, uuendatud ja tagasi toonud online. Pilveteenus võib olla kõige ühe versiooniuuenduse ajal aktiivne. Uuele versioonile on alati läbi vastu pilveteenusesse uusim versioon.

Järgmine diagramm näitab, kuidas versioonitäienduse tulu kui teostate kõik rollid teenus.

![Teenuse] (media/cloud-services-update-azure-service/IC345879.png "Teenuse")

See Järgmine diagramm näitab, kuidas värskenduse tulu kui teostate ainult ühe roll.

![Roll täiendamine] (media/cloud-services-update-azure-service/IC345880.png "Roll täiendamine")  

> [AZURE.NOTE] Teenuse täiendamisel ühe eksemplari mitmes eksemplaris tuuakse teenust ajal versioonitäienduse sooritatakse tõttu viis Azure uuendamine teenuseid. Teenuse taseme lepingu tagada teenuse kättesaadavus kehtib ainult teenuseid, mitu eksemplari juurutatud. Järgmises loendis kirjeldatakse, kuidas iga ketas andmeid mõjutavad iga Azure'i teenus versioonitäienduse stsenaariumi.
>
>VM taaskäivitage arvuti:
>
-   C: alles
-   D: alles
-   E: alles
>
>Portaali taaskäivitage arvuti:
>
-   C: alles
-   D: alles
-   E: hävitatud
>
>Portaali Reimage:
>
-   C: alles
-   D: hävitatud
-   E: hävitatud

>Versiooniuuenduse:
>
-   C: alles
-   D: alles
-   E: hävitatud
>
>Sõlm migreerimise:
>
-   C: hävitatud
-   D: hävitatud
-   E: hävitatud

>Pange tähele, et, ülaltoodud loendis E: ketas tähistab selle rolli juurkausta ketas ei tohiks kõva kodeeritud. Selle asemel kasutada % RoleRoot % keskkonna muutuja tähistada ketas.

>Minimeerimiseks on tööseisakute ühekordse teenuse täiendamisel lavastus server uue mitme eksemplari teenuse juurutamise ja teha VIP vahetamine.

Ajal automaatvärskenduse, Azure struktuuri kontrolleril hindab perioodiliselt pilveteenuses määrata, millal on järgmine UD käima seisundi. See hindamine kohta rolli alusel tehakse ja leiab ainult eksemplarid uusim versioon (st eksemplarid, mis on juba kõndis UDs). Kontrollib, kas väikseima arvu rolli aknad iga rolli jõudnud terminal korda.

### <a name="role-instance-start-timeout"></a>Roll eksemplari käivitamine ajalõpp
Struktuuri kontrolleril ootab iga rolli eksemplar käivitatud olekus jõuda 30 minutit. Kui ajalõpp kestus kaitseaega, edasi struktuuri kontrolleril kõndimine järgmise rolli eksemplariga.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Tagasipööramine värskendust.
Azure'i pakub paindlikkust haldamise teenused võimaldavad teil kontaktiga täiendavad toimingud on teenuse pärast algse värskenduse taotlus aktsepteeritakse Azure struktuuri kontrolleril ajal värskendust. A tagasipööramine saab ainult kui värskendus (konfiguratsiooni muutmine) või versioonitäiendus on **pooleli** olekus juurutamise. Värskenduse või versiooniuuenduse loetakse pooleli kui seal on vähemalt üks teenus, mis pole veel uue versiooni värskendatud eksemplari. Testimiseks a tagasipööramine on lubatud, märkige ruut RollbackAllowed lipu, tagastatud [Saada juurutus](https://msdn.microsoft.com/library/azure/ee460804.aspx) -ja [Saada Cloud atribuutide](https://msdn.microsoft.com/library/azure/ee460806.aspx) väärtuseks on seatud väärtusele tõene.

> [AZURE.NOTE] See on mõistlik tagasipööramine **kohapealse** värskenduse või täiendada, kuna VIP Vaheta täienduste kaasata, asendades ühe kogu käivitatud eksemplari teenust teise.

Tagasipööramine on pooleli värskendus on järgmised tagajärjed juurutamise kohta:

-   Esinemisvõimaluse rolli, mis oli veel värskendatud või täiendatud uus versioon on värskendatud või täiendatud, kuna need eksemplarid on juba target teenuse versioon.
-   Esinemisvõimaluse rolli, mis oli juba värskendatud või uue versiooni teenuse paketi versioonile (\*.cspkg) faili või teenuse konfigureerimine (\*.cscfg) faili (või mõlemad failid) selliseks, nagu need failid versiooniuuenduse-eelne versioon.

See funktsionaalselt on esitatud järgmised funktsioonid:

-   [Tagasipööramine värskendada või täiendamine](https://msdn.microsoft.com/library/azure/hh403977.aspx) tegevus, mis saab kutsuda konfiguratsiooni värskendamise (vallandanud helistaja [Muutmine juurutuse konfigureerimise](https://msdn.microsoft.com/library/azure/ee460809.aspx)) või versiooniuuenduse (käivitab helistaja [Täiendamine juurutamise](https://msdn.microsoft.com/library/azure/ee460793.aspx)) Kui teenus, mis pole veel uue versiooni värskendatud on vähemalt üks eksemplar.
-   Lukus elemendi ja RollbackAllowed elemendi, tagastatud vastuse keha [Saada juurutus](https://msdn.microsoft.com/library/azure/ee460804.aspx) -ja [Saada Cloud atribuutide](https://msdn.microsoft.com/library/azure/ee460806.aspx) osana.
    1.  Lukus element võimaldab teil tuvastada, millal muteeruv toimingut saate kasutada antud juurutuse.
    2.  RollbackAllowed element võimaldab teil tuvastada, millal [Tagasipööramine värskendamine või täiendamine](https://msdn.microsoft.com/library/azure/hh403977.aspx) toimingu saab kutsuda antud juurutuse.

    A tagasipööramine täitmiseks on nii on lukus ja RollbackAllowed elemendid. Piisab, et kinnitada, et RollbackAllowed on seatud väärtusele tõene. Tagastatakse ainult need elemendid kui nende meetodite millele abil määratud taotluse header "x-ms-versioon: 2011-10-01" või uuem versioon. Versioonimise päiste kohta leiate lisateavet teemast [Teenuse haldus Versioonimine](https://msdn.microsoft.com/library/azure/gg592580.aspx).

On olukordi, kus tagasipööramine värskendust või täiendamine ei toetata, need on järgmised:

-   Kohalikud ressursid – kui värskenduse suureneb kohalikud ressursid rolli Azure'i platvormi vähendamine ei luba tagasipööramine. 
-   Kvoodi piirangud – kui värskendus on skaala alla võite enam toiming on piisavalt Arvuta kvoodi tagasipööramine toimingu lõpuleviimiseks. Iga Azure'i tellimus on seostatud kvoodi, mis täpsustab, mis saab tarbitud kõik majutatud teenused, mis kuuluvad selle tellimuse maksimaalne arv. Kui läbimiseks tagasipööramine antud värskenduse oleks panna oma tellimus üle piirmäära, siis mis a tagasipööramine pole lubatud.
-   Võistluse tingimus – kui algse värskendamine on lõpule viidud, a tagasipööramine pole võimalik.

Näiteks kui värskendus tagasipööramine võib olla kasulik on, kui kasutate [Versiooni täiendamine juurutamine](https://msdn.microsoft.com/library/azure/ee460793.aspx) toiming käsitsi režiim, määr, kus majutatud suurte kohapealne uuendada oma Azure'i teenus on võetud välja juhtelemendile.

Plaanida versiooniuuenduse ajal kõne [Täiendamine juurutamise](https://msdn.microsoft.com/library/azure/ee460793.aspx) käsitsi režiimis ja alustage käima täiendamine domeenide. Kui mingil hetkel, nagu jälgida täiendamist, üles märkida mõne rolli aknad esimese täiendamine domeenide, mida uurite on hanguda, saate helistada [Tagasipööramine värskendamine või täiendamine](https://msdn.microsoft.com/library/azure/hh403977.aspx) toiming juurutamise, mis jätab puutumata eksemplarid, mis oli pole veel uuendatud ja tagasipööramine eksemplarid, mis oli versioonile eelmise teenuse paketi ja konfigureerimise kohta.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Mitme muteeruv on poolelioleva juurutuse toimingute alustamist
Mõnel juhul võite mitu samaaegne muteeruv toimingud on poolelioleva juurutuse algatada. Näiteks saate teostada teenuse värskenduse ja selle värskenduse on võetakse kasutusele kogu teenust, mida soovite muuta mõne, nt värskenduse tagasi pöörata eri värskenduse või isegi kustutada juurutamise. Kaasuse, kus seda võib vaja olla on, kui uuele versioonile üle viidud sisaldab lollakas koodi, mis põhjustab täiendatud rolli näiteks korduvalt krahhi. Sel juhul ei saa Azure struktuuri kontrolleril rakendamisel edu, mis täiendada, kuna täiendatud Domeen pole piisavalt arvul on terve. See olek on edaspidi *kinni juurutamise*. Te saate linti selle eemaldamiseks juurutamise tagasipööramine värskenduse või värske värskendus on ei õnnestu peale ühe.

Kui taotluse värskendada või teenuse ei ole esitatud Azure struktuuri kontrolleril, võite alustada muteeruv järgnevate toimingute. Seega teil on ootama algse toimingu lõpuleviimiseks enne alustamist teise muteeruv toiming.

Alustamist teise värskendamist samal esimese värskenduse täidab sarnaselt tagasipööramine toiming. Kui teine värskenduse automaatselt, esimese täiendamine domeeni täiendatakse kohe, võib viia eksemplarid versioonitäienduse mitu domeeni, on ühenduseta samas kohas, aeg.

Muteeruv toimingud on järgmised: [Muutmine juurutuse konfigureerimise](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Täiendamine juurutamine](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Värskenduste juurutamise oleku](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Kustutada juurutamise](https://msdn.microsoft.com/library/azure/ee460815.aspx)ja [Tagasipööramine värskendamine või täiendamine](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Kaks toimingut, [Saada juurutus](https://msdn.microsoft.com/library/azure/ee460804.aspx) - ja [Saada Cloud atribuutide](https://msdn.microsoft.com/library/azure/ee460806.aspx)tagastada lukus lipu, mida saate määrata, kas muteeruv toimingut saate kasutada antud juurutuse uurida.

Kõne nende meetodite versioon, mis tagastab lukus lipu, et peate seadma taotluse header "x-ms-versioon: 2011-10-01" või uuem versioon. Versioonimise päiste kohta leiate lisateavet teemast [Teenuse haldus Versioonimine](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Jaotuse versioonitäienduse domeenides rollid
Azure'i jaotab eksemplari rolli ühtlaselt üle teatud arv versioonitäienduse domeenid, mida saab konfigureerida osana teenuse definitsioonifail (.csdef). Täiendamine domeenide maksimaalne arv on 20 ja vaikimisi on 5. Teenuse määratluse faili muutmise kohta leiate lisateavet teemast [Azure teenuse definitsiooni skeemi (.csdef fail)](cloud-services-model-and-package.md#csdef).

Näiteks kui teie roll on kümme eksemplari, iga uuendamise domeeni sisaldab vaikimisi kaks korda. Kui teie roll on 14 eksemplari, siis neli täiendamine domeenide sisaldavad kolm eksemplari ja viienda domeeni sisaldab kahte.

Versioonitäienduse domeenid on tähistatud 0-põhine index: esimese täiendamine domeeni ID on 0 ja teine täiendamine domeeni ID on 1 ja jne.

Järgmine diagramm näitab, kuidas teenuse, kui kaks rollid sisaldab jagatakse kui teenus määratleb kaks versioonitäienduse domeeni. Teenus töötab kaheksa eksemplari web roll ja üheksa eksemplari töötaja roll.

![Täiendamine domeenide jaotamine] (media/cloud-services-update-azure-service/IC345533.png "Jaotuse täiendamine domeenide")

> [AZURE.NOTE] Pange tähele, et Azure'i määrab, kuidas eksemplarid eraldatakse versioonitäienduse domeenides. Ei saa määrata, millised eksemplarid on eraldatud millist domeeni.

## <a name="next-steps"></a>Järgmised sammud
[Kuidas hallata pilveteenustega](cloud-services-how-to-manage.md)  
[Kuidas jälgida pilveteenustega](cloud-services-how-to-monitor.md)  
[Kuidas seadistada pilveteenustega](cloud-services-how-to-configure.md)  
