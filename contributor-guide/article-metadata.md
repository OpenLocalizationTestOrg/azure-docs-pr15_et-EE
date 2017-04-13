

#<a name="metadata-for-azure-technical-articles"></a>Metaandmete Azure tehnilised artiklid

Kõik Azure tehnilise artiklid sisaldavad kaks metaandmete jaotist - jaotist atribuudid ja jaotise sildid. Jaotist atribuudid ja lubab mõni veebisaidi automatiseerimine ja SEO asjade, sildid sektsioonis palju sisemise sisu teatamine. Nii jaotised on nõutav.

- [Süntaks]
- [Kasutus]
- [Atribuudid ja väärtused jaotist atribuudid]
- [Atribuudid ja väärtused jaotise Sildid]

##<a name="syntax"></a>Süntaks

Jaotist atribuudid ja kasutab järgmist süntaksit:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

Jaotise sildid kasutab järgmist süntaksit:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Kasutus

- Elemendi nime ja atribuutide nimed on tõstutundlikud.
- Funktsiooni <properties> jaotis tuleb faili esimese rea.
- Pärast iga jaotise metaandmete ja enne tagamaks, et lehe tiitel on õigesti teisendada HTML-i avaldamise käigus oma lehe tiitel, jätke tühi rida.

## <a name="attributes-and-values-for-the-properties-section"></a>Atribuudid ja väärtused jaotist atribuudid

![](./media/article-metadata/checkmark-small.png)**pageTitle**: nõutav; oluline SEO. Selle atribuudi tekst kuvatakse brauseri menüü ja otsingutulemus pealkiri. Kasutage 55 – 60 märke, kaasa arvatud tühikud ja saidi identifikaator *| Microsoft Azure'i* (kirjutatud: Triibu ruumi Microsoft Azure'i space).  Funktsiooni pageTitle peaks olema erineb funktsiooni H1.

![](./media/article-metadata/checkmark-small.png)**Kirjeldus**: nõutav; olulised SEO (tekst) ja saidi funktsioone. Kirjeldus peab olema vähemalt 125 tähemärki pikk, et 155 märki koos tühikutega. Kirjeldage oma sisu nii, et kliendid ei tea, kas valige see loendist otsingutulemid. Väärtus on:

- See tekst võidakse kuvada kirjeldus või Google otsingutulemustes abstraktsed lõik.
- See tekst kuvatakse [artiklis index tulemused](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**teenuste**: artikleid, mis käsitlevad teenuse jaoks vajaliku. See väärtus on ofter edaspidi "servce nälkjas". Loetle kõik rakendatav teenused, komadega. Saate loendi esimese teenuse, suunatakse navigeerimise lingiridade lehe ja mis on diplayed lehe vasakpoolsel navigeerimisribal.

Määrata nii teenuste väärtus documentationCenter väärtus artiklites, suunatakse teenuste väärtus on lingiridade abil. Täiendavad väärtused, mille saate loendis kuvatakse avaldatud artiklis sildid. Väärtused:

- aktiivne-kataloog
- aktiivne-directory-b2c
- aktiivne-directory-ds
- rakenduse-service\api
- API-haldamine
- rakenduse-teenus
- rakenduse-servic\mobile
- rakenduse-service\web
- rakenduse-service\logic
- rakenduste portaali
- Rakenduse ülevaated
- automatiseerimine
- Azure'i portaal
- Azure'i ressursihaldur
- Azure'i-virnas
- varundus
- paketi
- parimate
- BizTalki-teenused
- vahemälu
- CDN-ID
- pilveteenustega
- andmete toomine
- andmete hankimise
- andmeanalüüsi lake
- andmesalve lake
- devtest-lab
- DNS-i
- documentdb
- expressroute
- sündmuse-jaoturi
- hdinsightiga
- asjade Interneti-jaoturi
- klahv-vault
- Laadi-koormusetasakaalustusteenuse
- seadme-õppe
- turuplats
- Media Servicesi
- Mobiil – kaasamine
- Mobiil-teenused
- mitmikautentimine
- teatis – jaoturi
- Töö ülevaated
- toimingute-halduse-suite
- powerapps
- taastamine – haldur
- redis-vahemälu
- RemoteApp
- Teabeõiguste haldus
- ajasti
- Otsing
- turbe-center
- teenuse-siini
- teenuse struktuuri
- saidi taastamine
- SQL-andmebaas
- SQL-andmebaas
- SQL-i teatamine
- salvestusruumi
- pood
- StorSimple
- voo-analytics
- liikluse haldur
- virtuaalne masinad
- virtuaalse võrgu
- Visual studio online
- VPN-lüüsi
- veebisaidid

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: nõutav arendaja-kesksele artiklite parim esiletõstetud on Arenduskeskus kaudu. Määrake ühe Arenduskeskus või keelt, mida see artikkel kehtib. Navigeerimise lingiridade lehe, suunatakse teie loendis väärtus. Määrata nii teenuste väärtus documentationCenter väärtus artiklites, suunatakse teenuste väärtus on lingiridade abil. Väärtused:

- **.net-i**
- **nodejs**
- **Java**
- **php**
- **Python**
- **Ruby**
- **mobiil**: aegunud. Asendage teatud mobiilne platvorm.
- **iOS**: Verifing selle uue väärtuse
- **Android**: kinnitatava selle uue väärtuse
- **Windowsi**: kinnitatava selle uue väärtuse
- **xamarin**: kinnitatava selle uue väärtuse

![](./media/article-metadata/checkmark-small.png)**autorid**: nõutav, vaid ühe väärtuse. Loetle GitHub konto esmane autori või artiklis VKE. Atribuudi draivid on kaasrida avaldatud artiklist. Loetle ainult üks vaatamata mitmuse atribuudi nimi.

![](./media/article-metadata/checkmark-small.png)**haldur**: nõutav, kui olete Microsofti kaasautori. Loetle meilipseudonüümi sisu avaldamise halduri tehnoloogia ala. Kui olete ühenduse kaasautor, soovitud atribuuti, kuid ärge seda tühja nii, et saaksime saavad täita.

![](./media/article-metadata/checkmark-small.png)**Editor**: ei kasutata. Kasutage seda muul eesmärgil.

![](./media/article-metadata/checkmark-small.png)**Sildid**: valikuline. Sisaldama ainult juhul, kui soovite lubada link jaotises artiklis lingirea lehele artiklis index (http://azure.microsoft.com/documentation/articles/) prefiltered loendi artiklitele, mis vastavad ühele kinnitatud väärtusest. Need väärtused on mõeldud võimalda kui sisu rühmitamine pole teenuse kohased rühmitada sisu. Silte saate sisestada ka siltideta, mis näitab, et see artikkel kehtib tehnoloogia virnas. See väärtus **ei** toeta vabakujulise silte ja hashtags; saidil peab olema lubatud sildid. Mitme väärtuse sildid komadega eraldatult üks artiklis on võimalik pakkuda. Kinnitatud väärtused on:

  - arhitektuur
  - Azure'i ressursihaldur
  - Azure'i Teenusehaldus
  - Arveldamine
  - MySQL-i

![](./media/article-metadata/checkmark-small.png)**märksõnade**: valikuline. SEO champs ainult kasutamiseks. Eraldi tingimused komadega. **Küsige oma SEO champ enne, kui muudate või kustutate sisu selle artikli teemad, mis sisaldab järgmiste terminite tähendust.** Atribuudi kirjete märksõnade SEO champ on suunatud ja otsing asukoha parandamiseks on jälgimine. Märksõnad ei muuda avaldatud HTML-vormingus. Valideerimine ei nõua see atribuut.

## <a name="attributes-and-values-for-the-tags-section"></a>Atribuudid ja väärtused jaotise Sildid

![](./media/article-metadata/checkmark-small.png)**MS.Service**: nõutav. Saate määrata Azure'i teenus, tööriist või funktsioon, mis see artikkel kehtib. Lehel ühe väärtuse.

 Kui lehe kehtib mitmes teenuses, valige teenus, mis on sellega kõige otse kehtib; näiteks artikkel, mis kasutab rakenduse majutada veebilehtedel näidata teenuse siini funktsioonid peaks olema **teenuse-siini** väärtuse, mitte **veebisaitide**. Kui lehe kehtib mitmes teenuses võrdselt, valige **mitu**. Kui lehe ei kehti mis tahes teenustele (see on harv), valige **NA**.

 - **aktiivne-kataloog**
 - **aktiivne-directory-b2c**
 - **aktiivne-directory-ds**
 - **API-haldamine**
 - **rakenduse-teenus**: kehtib ainult rakenduse teenuse üldine kontseptuaalne materjali
 - **rakenduse-teenus-api**
 - **rakenduse-teenus-loogika**
 - **rakenduse-teenus-mobile**
 - **rakendus – teenus – veebi**
 - **Rakenduse ülevaated**
 - **rakenduste portaali**
 - **automatiseerimine**
 - **Azure'i ressursihaldur**
 - **Azure'i – turvalisus**
 - **Azure'i-virnas**
 - **varundus**
 - **paketi**
 - **parimate**
 - **BizTalki-teenused**
 - **Arveldamine**
 - **vahemälu**
 - **CDN-ID**
 - **pilveteenustega**
 - **andmete toomine**
 - **andmesalve lake**
 - **andmeanalüüsi lake**
 - **devtest-lab**
 - **expressroute**
 - **hdinsightiga**
 - **asjade internet**
 - **asjade Interneti-jaoturi**
 - **klahv-vault**
 - **seadme-õppe**
 - **turuplats**: Azure'i turuplatsi artikleid
 - **Media Servicesi**
 - **Mobiil – kaasamine**
 - **Mobiil-teenused**
 - **mitmikautentimine**
 - **mitme**: lehe kehtib mitmes teenuses võrdselt
 - **na**: lehe ei kehti mis tahes teenustele (harv)
 - **teatis – jaoturi**
 - **Töö ülevaated**
 - **powerapps**
 - **taastamine – haldur**
 - **redis-vahemälu**
 - **RemoteApp**
 - **Teabeõiguste haldus**
 - **ajasti**
 - **turbe-center**
 - **teenuse-siini**
 - **teenuse struktuuri**
 - **saidi taastamine**: varem taastamise teenused
 - **SQL-andmebaas**
 - **SQL-andmebaas**
 - **SQL-i teatamine**
 - **salvestusruumi**
 - **talletage**: Azure'i poe kaudu pakutavaid artikleid
 - **StorSimple**
 - **liikluse haldur**
 - **virtuaalne masinad**
 - **virtuaalse võrgu**
 - **Visual studio online**
 - **VPN-lüüsi**
 - **veebisaidid**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: nõutav. Saate määrata programmeerimiskeel, mis see artikkel kehtib. Lehel ühe väärtuse.

 Kui lehe kehtib võrdselt kaks programmeerimise keelt, valige **mitu**. Kui lehe peamiselt kontseptuaalne ja selle sisu on üldiselt mitut programmeerimise keele puhul rakendatav, valige **mitu**. Kui leht on suunatud pole arendajad ja programmeerimise keele kohaldamine pole oluline, valige **NA**. **Ülejäänud-api** abil saate tuvastada REST API teemasid.

 - **CPP**
 - **DotNet**
 - **Java**
 - **JavaScripti**
 - **mitme**: lehe kehtib programmeerimise mitmes keeles võrdselt.
 - **na**: leht on suunatud arendajate ja ei ole eriti mis tahes programmeerimise keeled.
 - **nodejs**
 - **eesmärk-c**
 - **php**
 - **Python**
 - **ülejäänud-api**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: nõutav. Sisestage üksikasjad teema. Enamik uusi lehti, mis on loodud osaliste on artikli või viide.

 - **artikkel**: kontseptuaalne teema, õpetuse, funktsioon juhend või muud-viide artiklis

 - **turunduskampaania-lehekülg**: ainult Azure.com.  Leht, mis on spetsiaalselt sihtleht, jaoks väliste kampaaniat ja pole esmane saidi IA kaasatud  Ei tohi kasutada dokumentatsiooni artikleid või tavalise dokumendi lehekülgi algus.  Näited: azure.microsoft.com/develop/net/aspnet/; Azure.microsoft.com/Develop/Mobile/iOS/

 - **arendaja keskele-avalehel**: Azure.com ainult.  Arendaja on keskele avalehele, nt/arendada/net /

 - **Get-alustamine artikkel**: artiklitele, mis on esiletõstetud vasakpoolsel navigeerimisribal teenuse alustamine või ülevaade jaotises määramine.

 - **pilt-artikkel**: "pilt" õpetuse, mis on mõeldud teenuse on Sissejuhatus või funktsiooni, et külastajad teenuse kasutamist kiiresti alustada saab ja draivid tasuta prooviversioon Logi-up ja MSDN-i kordi. Määrake selle väärtuseks ainult artiklitele, mis on esiletõstetud ülaosas dokumentatsiooni sihtleht teie teenuse jaoks.

 - **avalehe**: ülemise taseme dokumentide avalehele. Meil on ainult kaks: azure.microsoft.com/documentation/ ja msdn.microsoft.com/library/azure/

 - **registri lehte**: teise taseme lehekülgi programmeerimise keeled, teenuste või funktsioonide algus. Need on laiali Azure.com ja teek ja kasutatakse sissepääsud jäävates täpsemale teabele. Näited: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **Infograafik-lehekülg**: ainult Azure.com. Leht, kus on Brausitav infographic või plakat, näiteks http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **viide**: An API viide lehe (sh REST API-ga) või PowerShelli cmdleti viide leht

 - **home-teenus**: ainult Azure.com.  Dokumendi teenuse Avaleht, nt /documentation/services/virtual-machines /

 - **saidi jaotis-Avaleht**: Azure.com ainult. "Avalehel" kindlat tüüpi sisu azure.com näited: http://azure.microsoft.com/documentation/infographics/ http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **video-lehekülg**: ainult Azure.com.  Leht, kus video, näiteks http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/ funktsioonid

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: nõutav. Saate määrata suunata platvorm, näiteks Windows, Linux, Windows Phone, iOS-i, Android või teisiti vahemälu platvormid. Lehel ühe väärtuse. See väärtus on enamiku teemade peale mobile ja virtuaalmasinates **NA** .

 - **vahemälu--roll**
 - **vahemälu-mitu**
 - **vahemälu-redis**
 - **vahemälu teenuse**
 - **vahemälu ühiskasutuses**
 - **Command liik kasutajaliidese**
 - **Ibiza**: sisu, mis kasutab Ibiza portaali. Kasutage seda ainult juhul, kui arutatakse funktsioon on saadaval nii Ibiza portaali ja praeguse portaali.
 - **mobiil-android**: Azure.com ainult praegu
 - **mobiil – HTML-vormingus**: Azure.com ainult praegu
 - **mobiil – ios**: Azure.com ainult praegu
 - **mobiil-kindle**: Azure.com ainult praegu
 - **Mobiil – mitme**
 - **mobiil-nokia-x**: Azure.com ainult praegu
 - **mobiil – phonegap**: Azure.com ainult praegu
 - **mobiil – sencha**: Azure.com ainult praegu
 - **Mobile windows**: Azure.com ainult praegu; Windowsi universaalne
 - **Windowsi mobiiltelefoni**
 - **Mobiil – Windowsi poest**
 - **mobiil – xamarin**: Azure.com ainult praegu; Xamarin kõik platvormid
 - **mobiil-xamarin-android**: Azure.com ainult praegu
 - **ios-mobiil-xamarin**: Azure.com ainult praegu
 - **mitme**: lehe kehtib mitme kaudu võrdselt
 - **na**: on platvormi Tabelitunnus on pole rakendatav selle lehe
 - **PowerShelli**
 - **VM-linux**
 - **VM-mitu**
 - **VM – windows**
 - **VM-windows sharepoint**
 - **VM windows – sql server**
 - **vs-– alustamine**: tuvastab VS alustamine lehe rühma. Sildi lisatud 12/1/14.
 - **vs-juhul-juhtunud**: tuvastab lehe VS kasutamise alustamiseks on kadunud. Sildi lisatud 12/1/14.

![](./media/article-metadata/checkmark-small.png)**MS.Workload**: nõutav. Saate määrata Azure'i töökoormus, mis kehtib lehe. Ainult ühe artiklis ühe väärtuse.

**Värskendage 8/6/15** Ms.workload väärtus on mõne xls, mitte väärtuse .md failis on vastendatud. Ms.workload väärtus on vaja siiski seni, kuni see funktsioon saab värskendada. Nüüd on kavandamist töötavad.  Kasutage **"na"** väärtusena now.

![](./media/article-metadata/checkmark-small.png)**MS.Date**: nõutav. Määrab kuupäeva artikli oli viimati muudetud asjakohasuse, täpsuse, õige kuvatõmmiseid ja tööpäeva lingid. Sisestage kuupäev vormingus KK/PP/AAAA. Kuupäev kuvatakse ka avaldatud artiklist viimati värskendatud kuupäeva.

![](./media/article-metadata/checkmark-small.png)**MS.Author**: nõutav. Saate määrata kaitstavate seotud teema. Sisemise aruanded (nt värskus) seda väärtust kasutama artikli õige kaitstavate seostada. Mitme väärtuse määramiseks peaks need semikoolonitega eraldada. Kas Microsoft pseudonüümid või täielik e-posti aadressid on lubatud. Pikkus võib olla pikem kui 200 märki.


###<a name="contributors-guide-links"></a>Osaliste juhend lingid

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)


<!--Anchors-->
[Süntaks]: #syntax
[Kasutus]: #usage
[Atribuudid ja väärtused jaotist atribuudid]: #attributes-and-values-for-the-properties-section
[Atribuudid ja väärtused jaotise Sildid]: #attributes-and-values-for-the-tags-section
