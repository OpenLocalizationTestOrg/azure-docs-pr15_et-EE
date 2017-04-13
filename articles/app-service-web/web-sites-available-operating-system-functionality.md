<properties 
    pageTitle="Operatsioonisüsteemi funktsionaalsuse teenuses Azure rakendus" 
    description="Lisateavet OS funktsioonid saadaval veebirakenduste, mobiilirakenduse taustaprogrammid ja Azure'i rakendust Service API rakendused" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Operatsioonisüsteemi funktsionaalsuse teenuses Azure rakendus #

Selles artiklis kirjeldatakse levinud võrdlusalus operatsioonisüsteemi funktsioone, mis on saadaval teenuses [Azure rakendus](http://go.microsoft.com/fwlink/?LinkId=529714)töötab kõik rakendused. See funktsioon sisaldab faili, võrk ja registri juurdepääs, ja diagnostika logid ja sündmusi. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>Rakenduse teenuse leping astme

Rakenduse teenus töötab kliendi rakenduste mitme rentniku majutusteenuse keskkonnas. Juurutatud astme **tasuta** ja **ühiskasutuses** rakendusi käivitada töötaja protsesside ühiskasutusega virtuaalmasinates, samal ajal **Standard** -ja **Premium** astme rakendusi käivitada virtual machine(s) Sihtotstarbeline spetsiaalselt ühe kliendiga rakendused.

Kuna teenuse rakendus toetab skaleerimise tõrgeteta erinevate vahel, turvalisuse konfiguratsiooni jõustatud rakendust Service rakenduste jääb samaks. See tagab, et rakenduste ei äkki käituvad teisiti, puudumisel ootamatud võimalused, kui rakenduse teenusleping aktiveerib ühe taseme teise.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Arengu raamistiku

Rakenduse teenuse hinnakirjad astme kontrollida Arvuta ressursse (CPU, vaba talletusruumi, mälu ja võrgu sealt) rakendustele summa. Siiski framework funktsioonid rakendustele laius jääb samaks sõltumata skaleerimise astme.

Teenuse rakendus toetab paljusid arengu raamistiku, sh ASP.net-i, klassikaline ASP, node.js, PHP ja Python - mis kõik käivitada laiendid IIS-i sees. Lihtsustamiseks ja normaliseerimine turvalisuse konfiguratsiooni, rakendused rakendus käivitada tavaliselt erinevate arengu raamistiku oma vaikesätted. Ühe lähenemisviisi konfigureerimise rakenduste võis API pindala ja funktsioonid iga üksiku arendamise raamistik kohandamiseks. Rakenduse teenus võtab rohkem üldine lähenemine selle asemel, võimaldades operatsioonisüsteemi funktsionaalsust sõltumata rakenduse arendamise raamistik levinud võrdlusalus.

Järgmistes jaotistes on operatsioonisüsteem funktsioonid rakenduse teenuse rakendustele üldist liiki.

<a id="FileAccess"></a>
##<a name="file-access"></a>Accessi fail

Erinevate draivid on olemas rakendus teenuses kohaliku draivid ja võrgu kaudu.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Kohalikud draivid

Keskmes, rakenduse teenus on teenus, töötavad taristu Azure'i PaaS (platvorm teenus). Selle tulemusena kohaliku draivid, "seotud" virtuaalse masina on sama ketas iga töötaja rolli Azure töötab. See hõlmab operatsioonisüsteemi ketta (D:\ ketas), rakenduse ketas, Azure'i paketi cspkg faile ainult kasutab rakendust Service (ja kättesaamatuks kliendid) ja "kasutaja" ketas (C:\ ketas), mille suurus sõltub VM suurust.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Võrgu draivid (ehk jagab UNC)

Üks kordumatu aspektide rakenduse teenus, mis muudab rakendusejuurutuse ja hooldus lihtne on kogu kasutaja sisu talletatakse UNC ühtlasi kogum. Selle mudeli kaardid kenasti sisu salvestusruumi kohapealse web hosting keskkonnas, mis on mitu koormusetasakaalustusega serverites kasutatavad levinud muster. 

Rakenduse teenuses, on loodud iga andmekeskuse UNC aktsiate arv. Kasutaja sisu kõik klientide iga andmekeskuse protsendina eraldatakse iga UNC jagada. Lisaks kogu sisu jaoks sama UNC alati panna ühe kliendi tellimuse faili ühiskasutusse anda. 

Pange tähele, kuidas cloud services töö tõttu, teatud virtuaalse masina eest majutusteenuse UNC share muutub aja jooksul. Tagatakse, et UNC ühtlasi olema paigaldatud erinevate virtuaalmasinates, nagu need tuuakse üles tavapärase cloud tegevuse käigus. Seetõttu kunagi peaks rakenduste teha raske koodiga oletused, et arvuti teave faili üldise Nimetamistava teed püsivad aja jooksul. Selle asemel nad peaksid kasutama mugav *faux* absoluutne tee rakenduse teenus pakub **D:\home\site** . See faux absoluutne tee annab kaasaskantav, rakenduse ja-kasutaja-diagnostika meetodi oma rakenduse jaoks. **D:\home\site**abil ühte saate ühiskasutusega failide edastamine rakenduse app ilma uus absoluutne tee konfigureerimine iga edastamine.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Rakenduse juurdepääsu faili tüüpi

Iga kliendi tellimuse on reserveeritud kataloogi struktuuri teatud UNC võrgukettal andmekeskuse sees. Klient võib olla mitu rakendused loodud teatud andmekeskuse nii, et kõik kuuluva ühe kliendi tellimuse kataloogid on loodud sama UNC ühiskasutusse anda. Osa võib sisaldada kataloogide sisu, tõrge ja diagnostikalogid ja varasemates versioonides loodud andmeallika juhtelemendi rakendus. Oodatud viisil, kliendi rakenduse kataloogid on saadaval käitusajal rakenduse rakenduse koodi lugemis- ja jaoks.

Manustatud virtuaalse masina, mis töötab rakendus kohaliku draividel rakenduse teenuse jätab hulk ruumi konkreetse rakenduse ajutiseks kohaliku säilitamiseks kettal C:\. Kuigi rakendus on täielik lugemis-ja kirjutamisõigusega juurdepääsu oma ajutist kohaliku salvestusruumi, selle salvestusruumi tõesti pole mõeldud otse rakenduse koodi kasutavad. Pigem eesmärk on anda ajutiste failide salvestusruumi IIS-i ja web rakenduse raamistiku. Rakenduse teenuse piirab ka ajutine kohaliku talletusmahtu saadaval iga rakendusse, et takistada üksikuid rakendusi tarbimine liigse hulga kohaliku faili salvestusruumi.

Kaks näidet, kuidas teenuse rakendus kasutab ajutist kohaliku salvestusruumi on ASP.net-i ajutiste failide kausta ja IIS-i kataloogi tihendatud faile. ASP.net-i koostamise süsteemi kasutab "Ajutised failid ASP.net-i" kataloogi ajutine koostamine vahemälu asukoht. IIS-i kasutab kausta "IIS-i ajutine tihendatud Files" tihendatud vastuse väljundi talletamiseks. Mõlemat tüüpi fail kasutus (kui ka teistele) on aadressirida rakenduse teenuses iga rakenduse ajutine kohaliku salvestusruumi. See ümberjaotamise tagab, et funktsioon jätkub ootuspäraselt.

Konkreetse rakenduse App teenuses töötab juhusliku kordumatu madal suurte õigustega töötaja protsessi identiteet nimega "rakenduskausta identiteedi", siinkirjeldatud täpsemaks: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Rakenduse koodi kasutab seda isikut lihtsa kirjutuskaitstud juurdepääsu operatsioonisüsteemi ketta (D:\ ketas). See tähendab, et rakenduse koodi saate loendisse levinud kataloogi struktuurid ja lugeda levinud faile operatsioonisüsteemi ketas. Kuigi see võib olla mõnevõrra laialdane taseme juurdepääsu, sama kataloogide ja failide juurdepääsetavat, kui olete ette töötaja roll on Azure majutatud teenuse ja vt draivi sisu. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Faili Accessi mitmes eksemplaris üle

Home kataloogi sisu rakenduse ja rakenduse koodi kirjutada selle. Kui rakendus töötab mitmes eksemplaris, ühiskasutuses home directory hulgas kõik aknad nii, et kõik eksemplarid näha samas kaustas. Nii, näiteks, kui rakenduse salvestab üleslaaditud failid home directory, need failid on kõik eksemplarid kohe saadaval. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Võrgupääs
Rakenduse koodi saate kasutada TCP/IP ja UDP vastavalt Protokollid teha väljaminev network Internet puuetega inimestele juurdepääsetavate lõpp, mis edastavad välisteenused ühendused. Rakendusi saate kasutada neid sama Protokollid teenuste Azure ja #151; näiteks luues HTTPS ühendusi SQL-andmebaasiga ühenduse loomiseks.

Olemas on ka piiratud võimalus rakenduste ühe kohaliku loopback ühendust luua, ja on rakenduse selle kohaliku loopback turvasoklite kuulata. See funktsioon on olemas peamiselt rakendused, millel on kuulata kohaliku loopback sockets osana funktsioonide lubamine. Pange tähele, et iga rakenduse näeb "Privaatne" loopback ühendus; rakenduse "A" ei saa kuulata kohaliku loopback turvasoklite, loodud rakenduse "B".

Marsruutimine on toetatud ka nimega on omavahel protsess side (IPC) süsteemi vahel eri protsessid töötavad üheskoos rakendus. Näiteks IIS-i FastCGI mooduli tugineb marsruutimine üksikute protsessid töötavad PHP lehtede koordineerimiseks.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Koodi täitmine, protsesside ja mälu
Nagu varem, käivitage rakenduste madal suurte õigustega töötaja protsesside abil soovitud juhusliku rakenduskausta identiteedi sees. Rakenduse koodi on juurdepääs mälu ruumi, mis on seotud töötaja protsessi nagu mis tahes lapse protsessid, mis võivad kudenud CGI protsesside või muud rakendused. Siiski ühe appi ei pääse mälu või muus rakenduses andmed isegi siis, kui see on virtuaalne samasse arvutisse.

Rakenduste saab käitada skriptide või lehtede toetatud web arengu raamistik kirjutatud. Rakenduse teenus ei konfigureerida web framework piiratuma režiimi sätted. Näiteks rakenduse teenuse ASP.net-i rakendusi käivitada "" täielikud asemel piiratuma usalda režiim. Web raamistik, sh nii klassikaline ASP ja ASP.net-i, saate helistada-protsess COM komponendid (kuid kontorist protsessi COM komponendid) ADO (ActiveX-andmeobjektid), mis on registreeritud vaikimisi opsüsteemis Windows nt.

Rakendusi saate kudema ja käivitada suvalise koodi. See on lubatud teha näiteks kudema käsk shell või käivitage PowerShelli skripti rakenduse. Siiski isegi juhul, kui koodi ja protsesside saate kudenud rakendusest, käivitatava programmi ja skriptide on endiselt piiratud ema rakenduse rakenduskausta antavad õigused. Näiteks rakenduse kudema täitmisfaili, mis toodab väljaminev HTTP-kõne, kuid, et sama käivitatava katse ei saa avada virtuaalse masina kaudu oma NIC. IP-aadress Madal suurte õigustega kood helistamise väljaminev võrgu on lubatud, kuid proovite virtuaalse masina võrgu sätete konfigureerimiseks nõuab administraatoriõigusi.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Diagnostika logid ja sündmused
Logiteave on veel mõni andmeid mõni proovivad juurde pääseda. Logiteave saadaval töötavad rakenduse teenuse kood tüüpi sisaldab diagnostika ja rakendus, mis on samuti hõlbus juurdepääs rakenduse genereeritud Logiteave. 

Näiteks W3C HTTP logid loodud aktiivne rakendus on saadaval, kas log kataloogi ühiskasutus võrgukoha loodud rakenduse, või bloobimälu saadaval, kui kliendi on häälestatud W3C logimise salvestusruumi. Viimane variant võimaldab suure hulga logid ilma ületamise seostatud võrgukettal faili salvestuslimiidi koguda.

Laadis, reaalajas diagnostika teabe .net-i rakendustest saab logida kasutades .net-i jälgimine ja diagnostika taristu, suvandid kirjutada Jälita teavet kas app võrgukettal, sõna või bloobimälu salvestusruumi asukohta.

Diagnostika logimine ja jälgimine, mis pole saadaval rakendused on Windows ETW sündmused ja levinud Windows sündmuselogide (nt süsteem, rakenduse ja turve sündmuselogide). Kuna ETW Jälita teave võib olla vaadatav hõlmav (koos õige ACL-ID), ETW sündmustele reageerida lugemis- ja on blokeeritud. Arendajate märgata helistab API lugeda ja kirjutada ETW sündmused ja levinud Windows sündmuselogide kuvatakse töötada, kuid selle põhjuseks on asjaolu rakenduse teenus on "scam" kõnesid, et need kuvataks õnnestub. Tegelikult on rakenduse koodi puudub juurdepääs sündmuse andmed.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Registri juurdepääs
Rakendused on kirjutuskaitstud juurdepääs palju (kuid mitte kõiki) registri virtuaalse masina need töötavad. Praktikas, tähendab see registrivõtmed, mis võimaldavad kirjutuskaitstud juurdepääsu kasutajate rühma pääseb rakendused. Registri, mis on praegu ei toetata lugemis- või kirjutuspääs ühelt alalt on selle HKEY\_praeguse\_kasutaja taru.

Registri kirjutusõiguse on blokeeritud, sh juurdepääsu kasutaja registrivõtmeid. Rakenduse seisukohalt registri kirjutusõiguse peaks kunagi olla viidatud Azure keskkonnas Kuna rakendusi saate (ja tehke) saada viiakse üle erinevate virtuaalmasinates. Ainult püsivate kirjutatav salvestusruumi, mida saate sõltub rakendus on talletatud rakenduse teenuse UNC ühtlasi iga rakenduse kataloogi sisu struktuur. 

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
