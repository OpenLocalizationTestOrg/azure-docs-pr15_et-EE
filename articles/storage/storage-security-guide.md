<properties
    pageTitle="Azure'i salvestusruumi turvalisuse juhend | Microsoft Azure'i"
    description="Mitme meetodid turvaliseks Azure Storage, sealhulgas, kuid mitte ainult RBAC, salvestusruumi teenuse krüptimist, kliendipoolne krüptimist, SMB 3.0 ja Azure ketta krüptimise üksikasjad."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure'i salvestusruumi turvalisuse juhend

##<a name="overview"></a>Ülevaade

Azure'i salvestusruumi pakub turvalisus funktsioonid, mis koos võimaldab arendajatel luua turvalist rakendusi täielik kogum. Rollipõhine juurdepääsu reguleerimine ja Azure Active Directory abil saate ise salvestusruumi konto kinnitada. Andmeid saab turvatud vahelise rakendus ja Azure [Kliendipoolne krüptimist](storage-client-side-encryption.md), HTTPS või SMB 3.0 abil. Andmeid saate määrata automaatselt krüptimist kui kirjutatud Azure Storage [Salvestusruumi teenuse krüptimise (SSE)](storage-service-encryption.md)abil. Saate seada OS- ja ketast, mis kasutavad virtuaalmasinates olema krüptitud [Azure'i ketas](../security/azure-security-disk-encryption.md). Delegeeritud juurdepääsu andmeobjektid Azure Storage saab anda [Ühiskasutusse Accessi allkirjade](storage-dotnet-shared-access-signature-part-1.md)abil.

See artikkel annab ülevaate iga need turvalisuse funktsioonid, mida saab kasutada Azure Storage. Lingid on esitatud artiklitele, mis annab iga funktsiooni andmeid, nii et saate hõlpsasti teha täiendava uurimise iga teema.

Järgnevalt on selle artikli teemad:

-   [Juhtimise lennuk turvalisus](#management-plane-security) – turvaliseks konto salvestusruumi

    Juhtimise lennuk koosneb ressursside abil saate hallata oma salvestusruumi konto. Selles jaotises me rääkida Azure'i ressursihaldur juurutamise mudeli ja Rollipõhine juurdepääsu reguleerimine (RBAC) abil juurdepääsu oma salvestusruumi kontod. Samuti räägitakse hallata oma salvestusruumi konto võtmed ja kuidas neid uuesti looma.

-   [Andmete lennuk turvalisus](#data-plane-security) – teie andmetele juurdepääsu turvamine

    Selles jaotises vaatame jäetud tegelik andmetele juurdepääsu konto salvestusruumi plekid, faile, järjekorrad ja tabelid, nt kaudu ühiskasutusse antud juurdepääs signatuurid ja talletatud Juurdepääsupoliitikaid. Me hõlmab nii teenusetaseme SAS ja konto taseme SAS. Samuti vaatame, kuidas piirata juurdepääsu teatud IP-aadress (või vahemik, IP-aadressid), kuidas piirata protokoll HTTPS-i kasutada ja ootamata seda aegumise ühiskasutusse Accessi signatuuri tühistamine.

-   [Krüptimine](#encryption-in-transit)

    Selles jaotises kirjeldatakse, kuidas kaitsta andmeid, kui teisaldate või sealt välja Azure Storage. Me rääkida HTTPS ja krüptimine kasutab SMB 3.0 Azure'i Failikettad soovitatav kasutamise kohta. Võtame kliendipoolne krüptimist, mis võimaldab teil enne, kui see on üle salvestusruumi lisamine klientrakenduse andmete krüptimiseks ja andmeid dekrüptida, kui see on salvestusruum kokku üle vaadata.

-   [Ülejäänud krüptimist](#encryption-at-rest)

    Me rääkida salvestusruumi teenuse krüptimise (SSE) ja kuidas saate salvestusruumi konto lubamine, tulemuseks plokk plekid, lehe plekid, ja lisab automaatselt on krüptitud kui kirjutatud Azure Storage plekid. Ka vaatame, kuidas saate Azure'i ketta krüptimise ja uurige põhilised erinevused ja ketta krüptimise võrreldes SSE võrreldes kliendipoolne krüptimise juhtudel. Me vaatame korraks FIPS vastavuse USA valitsuse arvutites.

-   Auditilogi juurdepääsu Azure salvestusruumi [Salvestusruumi Analytics](#storage-analytics) abil

    Selles jaotises kirjeldatakse salvestusruumi analytics logid teabe otsimine päringu jaoks. Vaatame Heitke pilk reaal salvestusruumi analytics logiandmed ja vaadake, kuidas leida kas nõude salvestusruumi konto võti juurdepääs ühiskasutusse antud allkirjaga või anonüümselt, ja kas see õnnestus või mitte.

-   [Brauseripõhine kliendid CORS kasutamise lubamine](#Cross-Origin-Resource-Sharing-CORS)

    Selles jaotises räägib, kuidas lubada rist-päritolu ressursside jagamise (CORS). Me rääkida domeenide juurdepääs, ja kuidas hakkama Azure Storage sisse ehitatud CORS võimaluste kohta.

##<a name="management-plane-security"></a>Juhtimise lennuk turvalisus

Juhtimise lennuk koosneb salvestusruumi konto ise mõjutavad toimingud. Näiteks saate saate luua salvestusruumi konto kustutada, tellimuse salvestusruumi kontode loendi hankimine, tuua salvestusruumi konto klahvid või taastada salvestusruumi konto võtmed.

Kui loote uue salvestusruumi konto, valige juurutamise mudeli klassikaline või ressursihaldur. Klassikaline mudel luua ressursid Azure ainult võimaldab konsulaatides tellimus ja omakorda salvestusruumi konto.

Sellest juhendist keskendub ressursihaldur mudelit, mis on soovitatav vahendid salvestusruumi kontode loomine. Funktsiooni ressursihaldur salvestusruumi kontod, mitte annab juurdepääsu kogu tellimus, saate hallata juurdepääsu rohkem piiratud tasemel halduse kaugusel Rollipõhine juurdepääsu reguleerimine (RBAC) abil.

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Kuidas tagada teie salvestusruumi konto Rollipõhine juurdepääsu reguleerimine (RBAC)

Vaatame rääkida mis RBAC on ja kuidas seda kasutada. Iga Azure'i tellimus on Azure Active Directory. Kasutajad, rühmad ja selle kausta kaudu saate anda juurdepääsu haldamine ressursid Azure'i tellimus, millest ressursihaldur juurutamise mudeli kasutamine. See on edaspidi Rollipõhine juurdepääsu reguleerimine (RBAC). Selle juurdepääsu haldamiseks saate kasutada [Azure portaali](https://portal.azure.com/), [Azure'i CLI tööriistad](../xplat-cli-install.md), [PowerShelli](../powershell-install-configure.md)või [Azure salvestusruumi ressursi pakkuja REST API-d](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Mudeliga ressursihaldur saate panna salvestusruumi konto ressursi rühma ja juhtelemendi Accessi pidi halduse teatud salvestusruumi konto Azure Active Directory abil. Näiteks saate anda kindlatele kasutajatele juurdepääsu salvestusruumi konto võtmed, samal ajal teiste kasutajate salvestusruumi konto teavet vaadata, kuid ei pääse salvestusruumi konto võtmed.

####<a name="granting-access"></a>Juurdepääsu andmine

Juurdepääs on antud vastav RBAC rolli määramine kasutajate, rühmade ja rakenduste õige ulatuses. Kogu tellimuse juurdepääsu määrate tellimuse tasemel. Saate anda kogu ressursside kohta ressursirühma õiguste andmine ressursirühma ise järgi. Saate määrata ka teatud rollide teatud ressursse, näiteks salvestusruumi kontod.

Siin on põhipunktid, mida peate teadma kasutamise RBAC juurdepääsu Azure Storage konto haldamise toimingute kohta.

-   Kui määrate juurdepääsu, saate määrata sellise tõrke ilmnemisel rolli konto, mida soovite on juurdepääs. Saate määrata toimingute abil saate hallata salvestusruumi konto, kuid mitte andmeobjektid kontole. Näiteks saate anda õiguse toomiseks atribuutide salvestusruumi konto (nt koondamine), aga mitte container või andmete ümbris bloobimälu sees.

-   Keegi on juurdepääs andmeobjektid salvestusruumi konto, saate talle anda õiguse lugeda salvestusruumi konto võtmed ja kasutaja saate need võtmed plekid, järjekorrad, tabelite ja failide juurde pääseda.

-   Kindla kasutajakonto kasutajate rühma või mõne kindla rakenduse saab määrata rollid.

-   Iga rolli on loendi ja toimingud ei toimingud. Näiteks virtuaalse masina kaasautori roll on "listKeys" toiming, mis võimaldab salvestusruumi konto klahvid lugeda. Kaastöö on näiteks värskendamine Active Directory kasutajate juurdepääs "Pole toimingud".

-   Salvestusruumi rollid kaasata (kuid mitte ainult) järgmist:

    -   Omanik – need saate hallata kõik, sh juurdepääs.

    -   – Ta saab teha kaasautor midagi omanik saab, v.a määramine Accessi. Keegi Sellel rollil vaadata ja taastada salvestusruumi konto võtmed. Salvestusruumi konto abil, siis pääsevad andmetele.

    -   Lugeja – need saate vaadata teavet salvestusruumi konto, välja arvatud saladusi. Näiteks, kui määrate abil lugemise õigused salvestusruumi konto kellelegi, salvestusruumi konto atribuutide vaatamiseks, kuid nad ei saa muuta atribuudid või vaadata salvestusruumi konto võtmed.

    -   Salvestusruumi konto kaasautor – need Halda salvestusruumi konto – neid saab lugeda, selle tellimuse ressursi rühmad ja ressursse, loomine ja haldamine tellimuse ressursi rühma juurutuste. Need pääsete juurde ka salvestusruumi konto võtmed, mis omakorda tähendab, et nad pääsevad andmete lennuk.

    -   Kasutaja juurdepääs administraator – need saate hallata kasutajate juurdepääsu salvestusruumi konto. Näiteks need mõne kindla kasutaja lugeja juurdepääsu lubada.

    -   Virtuaalse masina kaasautor – nad suudavad virtuaalmasinates, kuid mitte salvestusruumi kontoga on ühendatud. Selle rolli loetleda salvestusruumi konto võtmed, mis tähendab, et kasutaja, kellele määrate selle rolli saate värskendada andmeid lennuk.

        Selleks, et luua virtuaalse masina kasutaja, neil on vastav VHD faili salvestusruumi konto loomiseks. Selleks, et nad peavad saama tuua salvestusruumi konto võti ja andke seda ühenduse loomise VM API-ga. Seetõttu peab neil see õigus nii, et nad saavad loendi salvestusruumi konto võtmed.

- Määratleda kohandatud rollid on funktsioon, mis võimaldab koostada toimingute loendist saadaolevad toimingud, mis võivad täita Azure ressursid kogum.

- Kasutajal on loodud oma Azure Active Directory enne, kui määrate rolli neile.

- Saate luua aruande, kes andis/tühistatud millist liiki juurdepääsu ja neid sealt kellele ja PowerShelli või Azure CLI võimalusi.

####<a name="resources"></a>Ressursid

-   [Azure Active Directory Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)

    Selles artiklis selgitatakse Azure Active Directory rolli vastavalt juurdepääsu reguleerimine ja kuidas see toimib.

-   [RBAC: Sisseehitatud rollid](../active-directory/role-based-access-built-in-roles.md)

    See artikkel üksikasjad kõik saadaval RBAC sisseehitatud rollid.

-   [Ressursihaldur kasutuselevõtu ja juurutamise klassikaline mõistmine](../resource-manager-deployment-model.md)

    Selles artiklis selgitatakse ressursihaldur juurutus- ja klassikaline juurutamise mudeleid ja selgitatakse jaotiste ressursihaldur ja ressursi kasutamise eelised

-   [Azure Arvuta, võrgu ja mälu pakkujad Azure ressursihaldur jaotises](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    Selles artiklis selgitatakse, kuidas Azure'i arvutada, võrgu ja mälu pakkujad töötavad ressursihaldur mudeliga.

-   [Rollipõhine juurdepääsu reguleerimine REST API-ga haldamine](../active-directory/role-based-access-control-manage-access-rest.md)

    Selles artiklis kirjeldatakse haldamiseks RBAC REST API kasutamise kohta.

-   [Azure'i salvestusruumi ressursi pakkuja REST API viide](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    See on viite API-de abil saate hallata oma salvestusruumi konto programmiliselt.

-   [Arendaja juhend auth Azure'i ressursihaldur API-ga](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    Selles artiklis kirjeldatakse ressursihaldur API-de abil tehke järgmist.

-   [Rollipõhine juurdepääsu reguleerimine Microsoft Azure Ignite kaudu](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    See on link video kanali 9 2015 MS Ignite konverentsi. Selle seansi rääkida need juurdepääs haldus ja Azure aruandlusvõimalused ja analüüsida head tavad ümber juurdepääsu Azure tellimuste Azure Active Directory abil.

###<a name="managing-your-storage-account-keys"></a>Hallata oma salvestusruumi konto võtmed

512-bitine stringide loodud Azure, et koos salvestusruumikonto nimi saab kasutada andmete objektide talletatakse salvestusruumi juurde, nt plekid järjekorda sõnumite failid on Azure failide ühiskasutus ja tabeli üksused on salvestusruumi konto võtmed. Salvestusruumi klahvid juhtelementide juurdepääs andmete punkti salvestusruumi konto jaoks juurdepääsu kontrollimine.

Iga salvestusruumi konto on kaks võtit "klahvi 1" ja "Võti 2" [Azure portaali](http://portal.azure.com/) ja PowerShelli cmdlet-käsud. Neid saab uuesti abil käsitsi mitu võimalust, sealhulgas, kuid mitte ainult, kasutades [Azure portaali](https://portal.azure.com/), PowerShelli, Azure'i CLI või programmiliselt .NET salvestusruumi kliendi teek või Azure Storage teenuste REST API abil.

On mis tahes mitmel põhjusel uuesti looma oma salvestusruumi konto võtmed.

-   Teil võib taastada neid regulaarselt turvalisuse põhjustel.

-   Teie salvestusruumi konto võtmed soovite taastada, kui keegi õnnestus sissemurdmist rakenduse ja tuua klahvi, mis on kõva või salvestatud konfiguratsioonifailis, andes neile täielik juurdepääs kontole salvestusruumi.

-   Võtme regenereerimiseks teise puhul on kui teie meeskond on kasutusel salvestusruumi Explorer rakendus, mis säilitab salvestusruumi konto võti ja üks meeskonna liikmed. Rakenduse soovite tööd jätkata, andes neile juurdepääsu kontole salvestusruumi pärast seda, kui nad on läinud. See on tegelikult peamine põhjus nende loodud konto taseme ühiskasutusse Accessi allkirjad – saate kasutada ka konto taseme SAS asemel talletamise kiirklahvide konfiguratsioonifailis.

####<a name="key-regeneration-plan"></a>Võtme regenereerimine kavandamine

Te ei soovi taastada just kasutate ilma mõned planeerimine võti. Kui te seda teha, võib lõigata kõik Accessi salvestusruumi konto, mis võib põhjustada suuri häireid. See on, miks on kaks võtit. Peaks üks klahv korraga uuesti looma.

Saate taastada oma klahvid, kindlasti, peate loendit kõigi rakenduste sõltuvad salvestusruumi konto, samuti muud teenused, mida kasutate Azure. Näiteks kui kasutate Azure Media Servicesi, mis sõltuvad teie salvestusruumi konto, peate uuesti sünkroonimise kiirklahvide teenusega meediumi pärast saate taastada võti. Kui kasutate kõik rakendused nagu salvestusruumi explorer, peate pakuvad ka nende rakenduste uus võtmed. Pange tähele, et kui teil on VMs, mille VHD failid salvestusruumi konto, nad ei mõjuta regenereeruvate salvestusruumi konto klahvide abil.

Saate taastada oma klahvid Azure'i portaalis. Kui klahvid on uuesti nad saavad võtta kuni 10 minutit salvestusruumi teenustes sünkroonimist.

Kui olete valmis, siin on üksikasjalikult, kuidas peaks muuta tootevõtit üldine protsess. Sel juhul eeldatakse, et kasutate praegu Key 1 ja te ei kavatse muuta kõik selle asemel kasutada klahvi 2.

1.  Klahv 2, et tagada turvaline taastada. Saate seda teha Azure'i portaalis.

2.  Kõikide rakenduste salvestusruumi võti talletuskoht muuta salvestusruumi võtit kasutada Key 2 uus väärtus. Testige ja avaldada rakendus.

3.  Pärast kõigi rakenduste ja teenuste on üles ja töötab edukalt, taastada Key 1. See tagab, et igaüks, kellele olete selgesõnaliselt ei andnud uue tootenumbri ei ole enam salvestusruumi kontole.

Kui kasutate praegu Key 2, saate kasutada sama toimingut, kuid tühistada soovitud nimesid.

Paari päeva, saate migreerida muutmise konkreetse rakenduse kasutamiseks uue tootenumbri ja avaldada. Kui kõik on valmis, tuleks seejärel minge tagasi ja vana võtme taastada, nii et see ei tööta enam.

Teine võimalus on panna [Azure'i klahvi Vault](https://azure.microsoft.com/services/key-vault/) nimega salajase salvestusruumi konto võti ja rakenduste tuua võti seal on. Seejärel kui taastada võti ja Azure klahvi Vault värskendada, rakendusi ei pea mis tuleb kuna nad saab valida üles uue tootenumbri hoidlast Azure'i võti automaatselt. Pange tähele, et teil lugeda võti iga kord, kui teil on vaja see rakendus või saate selle mälu vahemälu ja kasutamisel, ei ole tuua võti uuesti hoidlast Azure'i võti.

Azure'i klahvi Vault abil lisab ka teise taseme turvalisus teie salvestusruumi võtmed. Kui seda meetodit kasutada siis, teil kunagi on salvestusruumi võtme kõva konfiguratsioonifailis, mis eemaldab seda võimalust kellegi juurdepääsu teatud loata võtmed.

Mõne muu Azure'i klahvi Vault kasutamise eeliseid on teie võtmed Azure Active Directory abil saate määrata ka Accessi. See tähendab, et saate anda üksikud rakendusi, mis on vaja tuua Azure'i klahvi hoidlast võtmed ja teada, et muudes rakendustes ei saa kiirklahvide ilma neid õiguse andmiseks spetsiaalselt juurdepääsu.

Märkus: on soovitatav kasutada ainult üks kõikide rakenduste samal ajal. Kui kasutate Key 1 kohati ja Key 2 teised, ei saa pöörata oma klahvid ilma mõned kaotamise Accessi rakenduse.

####<a name="resources"></a>Ressursid

-   [Azure'i salvestusruumi kontode kohta](storage-create-storage-account.md#regenerate-storage-access-keys)

    Selles artiklis antakse ülevaade salvestusruumi kontod ja käsitletakse vaatamiseks, kopeerimise ja regenereeriv salvestusruumi kiirklahvide.

-   [Azure'i salvestusruumi ressursi pakkuja REST API viide](https://msdn.microsoft.com/library/mt163683.aspx)

    See artikkel sisaldab linke teatud artikleid toomine salvestusruumi konto võtmed ja regenereeriv salvestusruumi konto klahvide abil REST API Azure'i konto jaoks. Märkus: Kui see on ressursihaldur salvestusruumi kontode jaoks.

-   [Toiminguid salvestusruumi kontod](https://msdn.microsoft.com/library/ee460790.aspx)

    Selles artiklis salvestusruumi Service Manager REST API viite sisaldab linke teatud artikleid toomine ja regenereeriv salvestusruumi konto klahvide abil REST API-ga. Märkus: See on klassikaline salvestusruumi kontod.

-   [Et võti haldus – abil Azure AD Azure Storage andmetele juurdepääsu haldamine](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    Selles artiklis kirjeldatakse, kuidas juurdepääsu oma Azure Storage klahvid Azure'i klahvi võlvkelder Active Directory abil. Samuti kujutatakse on Azure automatiseerimine töö abil saate taastada klahvid üks kord tunnis.

##<a name="data-plane-security"></a>Andmete lennuk turvalisus

Andmete lennuk turvalisuse viitab andmete objekte, mis on talletatud Azure Storage – plekid, järjekorrad, tabelite ja failide turvaline meetodid. Oleme näinud viise andmete ja turvalisuse ajal andmete krüptimiseks, aga kuidas lähete kohta, mis võimaldab juurdepääsu objektide?

Sellise tõrke ilmnemisel on kaks võimalust ise andmeobjektid juurdepääsu kontrollimine. Salvestusruumi konto klahvid juurdepääsu kontrollimine on esimene ja teine abil ühiskasutusse Accessi allkirjade juurdepääsu teatud andmeobjektid teatud aja jooksul.

Märkus erandiks on, et te avaliku juurdepääsu lubamiseks abil oma plekid säte juurdepääsu tase ümbris, mis hoiab plekid vastavalt sellele. Kui access seadmine nõus bloobimälu või Container võimaldab avaliku lugemisõigus plekid selle ümbrises. See tähendab, et igaüks, kellel on bloobimälu selle ümbrises osutab URL-i saate selle avada brauseri kaudu ühiskasutusse antud juurdepääs signatuuri või ilma salvestusruumi konto võtmed.

###<a name="storage-account-keys"></a>Salvestusruumi konto võtmed

Salvestusruumi konto võtmed on 512-bitine stringide loodud Azure'i, mis koos salvestusruumikonto nimi saab kasutada andmete objektide talletatakse salvestusruumi juurde.

Näiteks saate lugeda plekid, kirjutada järjekordadesse, tabelite loomine ja muutmine failid. Paljude neid toiminguid saab teha Azure portaali kaudu või kasutades ühte järgmistest palju salvestusruumi Exploreri rakendusi. Samuti saate kirjutada koodi REST API või ühte salvestusruumi kliendi teekide abil teha järgmisi toiminguid.

Nagu jaotises [Haldus lennuk turvalisus](#management-plane-security), antakse täielik juurdepääs andes Azure tellimuse salvestusruumi klahvid klassikaline salvestusruumi konto juurdepääsu. Salvestusruumi klahvid Azure'i ressursihaldur näidise salvestusruumi konto juurdepääsu saab kontrollida Rollipõhine juurdepääsu reguleerimine (RBAC).

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Kuidas Delegaadi juurdepääs teie konto kaudu ühiskasutusse antud juurdepääs signatuurid ja talletatud Juurdepääsupoliitikaid objektidele

Ühiskasutusse antud juurdepääs allkiri on string, mis sisaldab Turbeloa, mis võivad seotud URI, mis võimaldab volitatud esindaja juurdepääsu salvestusruumi objektid ja määrake piiranguid nagu õiguste ja kuupäeva/kellaaja vahemik juurdepääsu.

Saate anda juurdepääsu plekid, ümbriste, järjekorda sõnumeid, failide ja tabeleid. Tabelitega, saate tegelikult anda juurdepääs vahemiku üksuste tabelis, määrates sektsiooni ja rida olulisi vahemikud, millele soovite kasutajal on juurdepääs. Näiteks, kui teil on salvestatud sektsiooni võti geograafiliste riigi andmeid, võib kasutajale anda juurdepääsu Californias ainult andmed.

Teise näite korral võib anda veebirakenduse SAS luba, mis võimaldab seda kirjutada kirjete järjekorda ja anda töötaja rolli rakenduse toomine sõnumite kuhjuda ja töödelda sümboolse SAS. Või saate anda ühe kliendi SAS luba nad kaudu üles laadida pilte bloobimälu ümbris paanide lisamine ja anda web rakendusel lugema neid pilte. Mõlemal juhul ei ole probleeme – iga rakenduse saab anda oma tööülesannete täitmiseks juurdepääs. See on võimalik abil ühiskasutusse Accessi allkirjad.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Miks soovite kasutada ühiskasutusse Accessi allkirjad

Miks peaksite kasutama mõne SAS asemel ainult annab teie salvestusruumi konto võti, mis on nii palju lihtsam välja? Annab teie salvestusruumi konto võti välja on jagada oma salvestusruumi kingdom võtmed. Seda määrab täielik juurdepääs. Keegi teine võib teie abil ja konto salvestusruumi oma kogu teeki üles laadida. Samuti võib asendamine Nastixi versioonide failide või varastada teie andmeid. Annab ära piiramatu salvestusruum konto on midagi, mida ei tohiks võtta kergelt.

Ühiskasutusse antud juurdepääs allkirjad, saate anda mõnda muud klienti lihtsalt piiratud aja jooksul kasutamiseks vajalikud õigused. Näiteks, kui keegi on üleslaadimise on bloobimälu oma kontole, saate anda need kirjutamisõigused lihtsalt piisavalt aega üles laadida bloobimälu, (sõltuvalt mahu bloobimälu, muidugi). Ja kui muudate meelt, saate tühistada juurdepääsu.

Lisaks saate määrata, et taotlusi abil soovitud SAS piirduvad IP-aadress või IP-aadresside vahemiku Azure'i välise. Saate nõuda, taotlused on tehtud, kasutades kindlat protokolli (HTTPS või HTTP-või HTTPS). See tähendab, et kui soovite lubada HTTPS liiklust, saate seada nõutav protokoll HTTPS ainult ja HTTP liikluse on blokeeritud.

####<a name="definition-of-a-shared-access-signature"></a>Ühiskasutusega juurdepääsu allkirjaga

Ühiskasutusse antud juurdepääs signatuuri on lisatud URL-i suunatud ressursi päringu parameetrite kogum

mis annab teavet lubatud juurdepääs ja aeg, mille jaoks on lubatud juurdepääs. Siin on näide; selle URI pakub lugemisõigus on bloobimälu viis minutit. Teate, et SAS päringu parameetrite tuleb URL-kodeeringuga, nagu koolon (:) või tühiku % 20% 3.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Kuidas ühiskasutusse Accessi allkirja autendib Azure'i salvestusteenus

Kui soovitud salvestusteenus saab taotluse, Sisestuskeel päringu parameetrid ja loob signatuuri meetodil sama nimega helistaja programm. See võrdleb kahe allkirjad. Kui nad nõus, siis soovitud salvestusteenus saate kontrollida salvestusruumi versiooni veenduge, et see on kehtiv, kontrollige, kas määratud aknas sees on praeguse kuupäeva ja kellaaja, veenduge, et juurdepääs nõutav vastab taotlus jne.

Näiteks ülaltoodud meie URL, kui URL on osutab faili asemel mõne bloobimälu taotlus oleks nurjuda, kuna seda määrab, et ühiskasutusse antud juurdepääs allkirja jaoks soovitud bloobimälu. Kui kutsutud ülejäänud käsu lisamine bloobimälu värskendamiseks, ei Kuna ühiskasutusse Accessi allkirja saate määrata, et ainult lugemisõigus on lubatud.

####<a name="types-of-shared-access-signatures"></a>Ühiskasutusega juurdepääsu allkirjade tüübid

-   Teenusetaseme SAS saab juurdepääsu teatud ressursid salvestusruumi konto. Mõned näited on plekid ümbrises, allalaadimine on bloobimälu, üksuse tabeli värskendamine, lisada sõnumite järjekorda või faili üleslaadimisel failikettale loendi toomine.

-   Mõne konto taseme SAS saab juurdepääsu midagi, mida saate kasutada teenusetaseme SAS. Peale selle saate anda ressursse, mis ei ole lubatud teenusetaseme SASga, näiteks võimalust luua ümbriste, tabelite, järjekorrad ja failiketastel suvandid. Samuti saate määrata mitu teenustele juurdepääsu korraga. Näiteks võib anda kellegi juurdepääsu nii plekid ja failide salvestusruumi kontole.

####<a name="creating-an-sas-uri"></a>Sellise SAS URI loomine

1.  Saate luua ka sihtotstarbelise URI nõudmisel, kõik päringu parameetrite määratlemine iga kord, kui.

    See on väga paindlik, kuid kui teil on parameetrid, mis on sarnane iga kord, kui loogiline kogumi, kasutades talletatud juurdepääsu poliitika on parem.

2.  Saate luua salvestatud Accessi poliitika kogu container, failikettal, tabel või järjekorda. Seejärel saate seda alusena loote SAS URI-d. Õigused, mis on talletatud Juurdepääsupoliitikaid alusel saate hõlpsasti tühistada. Teil võib olla kuni 5 poliitikate määratletud iga container, järjekorda, tabel või faili ühiskasutusse andmine.

    Näiteks kui te ei kavatse on palju inimesi lugeda teatud ümbrises plekid, võib luua salvestatud Accessi poliitika, mis ütleb "anna lugemisõigus" ja muud sätted, millest saab sama iga kord, kui. Seejärel saate luua SAS URI, mis on talletatud Accessi poliitika sätete abil ja täpsustades aegumise kuupäeva/kellaaja. See eelis, et see on teil pole määrata kõik parameetrid päringu iga kord.

####<a name="revocation"></a>Tühistamine

Oletame, et teie SAS on rikutud, või soovite muuta selle tõttu regulatiivsetele nõuetele või ettevõtte turvalisus. Kuidas saate tühistada juurdepääsu ressursile, et SAS abil? See sõltub sellest, kuidas olete loonud SAS URI.

Kui kasutate sihtotstarbelise URI, on teil kolm võimalust. Saate probleemi SAS sõned lühike aegumise poliitika ja muude kaoks lihtsalt oodata. Saate ümber nimetada või kustutada ressurss (eeldades, et luba oli rakendatud üksikobjektiks). Saate muuta salvestusruumi konto võtmed. Viimase suvand võib olla suur mõju, olenevalt sellest, mitu teenused on kasutusel salvestusruumi konto, ja ilmselt pole midagi, mida soovite teha mõned planeerimine ilma.

Kui kasutate SAS, mis on tuletatud talletatud juurdepääsu poliitika, eemaldada juurdepääsu tühistada talletatud juurdepääsu poliitika – just seda saate muuta nii, et see on juba aegunud või saate selle eemaldada täielikult eemaldada. See rakendatakse kohe ja muudab iga SAS talletatud Accessi poliitika abil loodud kehtetuks. Värskendamine või eemaldamine Accessi salvestatud poliitika võib mõju inimestele juurdepääs selle kindla container failikettal, tabelis või järjekorra kaudu SAS, kui kliendid kirjutada nii, et nad taotleda uut SAS kui vana kehtivuse, kuid see töötab korralikult.

Kuna kasutades SAS, mis on talletatud juurdepääsu poliitika tuletatud annab teile võimalus tühistada selle SAS kohe, on soovitatav alati kasutada salvestatud Juurdepääsupoliitikaid võimalusel parim.

####<a name="resources"></a>Ressursid

Üksikasjalikumat teavet abil ühiskasutusse Accessi signatuurid ja talletatud juurdepääsu poliitikad, koos näidetega, lugege järgmisi artikleid:

-   Need on viide artikleid.

    -   [Teenuse SAS](https://msdn.microsoft.com/library/dn140256.aspx)

        Selles artiklis antakse ülevaade mõne failide plekid, järjekorda sõnumeid, tabeli vahemike ja teenusetaseme SAS abil.

    -   [Teenuse SAS ehitamine](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Konto SAS ehitamine](https://msdn.microsoft.com/library/mt584140.aspx)

-   Need on õpetused kasutades .net-i kliendi teek luua ühiskasutusse Accessi signatuurid ja talletatud Juurdepääsupoliitikaid.

    -   [Ühiskasutusega juurdepääsu allkirjad (SAS) abil](storage-dotnet-shared-access-signature-part-1.md)

    -   [Ühiskasutusse antud juurdepääs allkirju osa 2: Loomine ja kasutamine on SAS teenusega bloobimälu](storage-dotnet-shared-access-signature-part-2.md)

        See artikkel sisaldab selgitus SAS mudeli meilisignatuuridega ühiskasutusse antud juurdepääs, ja soovitused parim kasutamine SAS. Ka on antud luba tühistamine.

-   IP Address (IP ACL) juurdepääsu piiramine

    -   [Mis on lõpp pääsuloendi (ACL)?](../virtual-network/virtual-networks-acl.md)

    -   [Teenuse SAS ehitamine](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        See on viide artikkel teenusetaseme SAS; See sisaldab IP ACLing näide.

    -   [Konto SAS ehitamine](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        See on viide artikkel konto taseme SAS; See sisaldab IP ACLing näide.

-   Autentimine

    -    [Azure'i salvestusruumi teenuste autentimine](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Ühiskasutusse antud juurdepääs allkirjade alustamine õpetus

    -   [SAS alustamine õpetus](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Krüptimine

###<a name="transport-level-encryption--using-https"></a>Transport taseme krüptimise – HTTPS-i abil

Teise etapi Azure Storage andmete turvalisuse tagamiseks võtke on krüptida kliendi ja Azure Storage vahel. Esimene soovitus on Kasuta alati [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -protokolli, mis tagab turvaline side avaliku Interneti kaudu.

Kasutage HTTPS alati kui helistades REST API-d või juurdepääs ei esita salvestusruumi. Lisaks **Ühiskasutusse antud juurdepääs allkirjad**, mida saate kasutada delegaadi juurdepääsu Azure Storage objektidele, lisada suvandi määramaks, et ühiskasutusse antud juurdepääs allkirjad, tagada, et igaüks, saates lingid koos SAS märkide kasutab proper protokolli kasutamisel saab kasutada ainult HTTPS-protokolli.

####<a name="resources"></a>Ressursid

-   [Azure'i rakendust Service rakenduse HTTPS-i lubamine](../app-service-web/web-sites-configure-ssl-certificate.md)

    Selles artiklis kirjeldatakse Azure'i veebirakenduse HTTPS lubamise kohta.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Azure'i Failikettad ajal krüptimise abil

Azure'i failide talletamine toetab HTTPS kasutamisel REST API-ga, kuid on lisatud veel tavaliselt kasutatakse SMB osa VM. SMB 2.1 ei toeta krüptimist, et ühendused on lubatud ainult samas piirkonnas Azure. Siiski SMB 3.0 toetab krüptimist ja saab kasutada serveriga Windows Server 2012 R2, Windows 8, Windows 8.1 ja Windows 10, võimaldades rist-piirkonna juurde ja isegi Accessi töölauaversiooni.

Teate, et ajal Azure'i Failikettad saab kasutada Unix, Linux SMB klient veel ei toeta krüptimist, nii, et juurdepääs on lubatud ainult mõne Azure piirkonnas. Krüptimise Linuxi tugi on juhised Linux arendajad vastutab SMB funktsioonid. Kui keegi lisab krüptimist, on teil sama võimalus juurdepääsuks nagu Windows Azure'i failikettal, Linux.

####<a name="resources"></a>Ressursid

-   [Kuidas kasutada Linux Azure'i failide talletamine](storage-how-to-use-files-linux.md)

    Selles artiklis kirjeldatakse Azure'i võrguketta seinale Linuxi ja üles/alla laadida failid.

-   [Alustamine Windows Azure'i failide talletamine](storage-dotnet-how-to-use-files.md)

    Selles artiklis antakse ülevaade Azure'i failikettad ja kuidas mount ja kasutada neid PowerShelli ja .net-i abil.

-   [Kollased Azure failide talletamine](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    Selles artiklis teatab üldine kättesaadavus Azure'i failide talletamine ja pakub SMB 3.0 krüptimise tehnilised üksikasjad.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Kliendipoolne krüptimise abil kaitsta andmeid, mida saadate salvestusruum

Teine võimalus, mis aitab teil tagada, et teie andmed on turvaline ajal ühest kliendi rakendus ja salvestusruumi on kliendipoolne krüptimine. Andmed on krüptitud enne Azure Storage üle. Kui andmete toomisel Azure Storage dekrüptida andmed pärast selle vastu kliendi poolel. Isegi kui andmed on krüptitud, läheb kogu kaabel, soovitame kasutada ka HTTPS-i andmete terviklus kontrolle, mis aitavad leevendada võrgu vigu, mis mõjutavad andmete terviklus sisseehitatud, kui see.

Kliendipoolne krüptimise on ka meetodi krüptimiseks oma andmete ülejäänud, andmed on salvestatud krüptitud kujul. Me rääkida see Täpsemalt jaotises krüptimise [ülejäänud juures](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Ülejäänud krüptimist

On kolme Azure funktsioone, mis pakuvad krüptimist ja ülejäänud. Azure'i ketta krüptimise kasutatakse OS- ja ketast IaaS Virtuaalmasinates krüptimiseks. Muude kahe-kliendipoolne krüptimise ja SSE – on nii andmete Azure Storage krüptimiseks kasutatavat. Vaatame kõik need, ja seejärel tehke võrdlus ja kui igaüks saab kasutada.

Saate kasutada kliendipoolne krüptimise teel (mis on talletatud ka salvestusruumi krüptitud kujul) andmete krüptimiseks, eelistate lihtsalt kasutada HTTPS ülemineku ajal ja kuidagi krüptitakse automaatselt, kui see on talletatud andmed. On kaks võimalust selleks--Azure'i ketta krüptimise ja -SSE. Üks kasutatakse andmete OS- ja kettale, mis kasutavad VMs otse krüptimiseks ja teine krüptimiseks Azure'i bloobimälu kirjutatud andmed kasutatakse.

###<a name="storage-service-encryption-sse"></a>Salvestusruumi teenuse krüptimise (SSE)

SSE saate nõuda, et teenuse salvestusruumi automaatselt krüptida Azure'i mäluruumi kirjutamisel. Kui peate lugema andmed Azure Storage, see dekrüptitakse salvestusruumi teenus võttis enne tagastatakse. See võimaldab teil kaitsta teie andmeid ilma koodi muuta või lisada koodi kõik rakendused.

See on kogu salvestusruumi kontole säte. Saate lubada ja selle funktsiooni keelata, muutes selle sätte väärtuse. Selleks, saate Azure portaali, PowerShelli, Azure'i CLI, salvestusruumi ressursi pakkuja REST API või .NET salvestusruumi kliendi teek. SSE on vaikimisi välja lülitatud.

Sel ajal, kasutatakse krüptimise võtmed haldab Microsoft. Me algselt võtmed ja hallata turvaline säilitamine võtmed kui ka tavaline pööre sisemise Microsofti poliitika määratletud. Tulevikus saad võimalus hallata oma krüptimise võtmed, ja migreerimise tee kaudu Microsoft hallatava võtmed kliendi hallatava võtmed.

See funktsioon on saadaval ressursihaldur juurutamise mudeli abil loodud Standard- ja Premium salvestusruumi kontod. SSE kehtib ainult blokeerida plekid, lehe plekid, ja lisab plekid. Muud tüüpi andmete, sh tabeleid, järjekorrad ja faile, ei saa krüptida.

Andmed on ainult krüptitud SSE on lubatud, kui andmed on kirjutatud bloobimälu. Lubamine või keelamine SSE ei mõjuta olemasolevaid andmeid. Teisisõnu, kui see krüptimist, seda ei minge tagasi ja krüptimine andmeid, mis on juba olemas; Samuti on see dekrüptida andmed, mida juba olemas, kui keelate SSE.

Kui soovite selle funktsiooni kasutamiseks klassikaline salvestusruumi kontoga, saate luua uue ressursihaldur salvestusruumi konto ja kopeerige andmed uuele kontole AzCopy abil. 

###<a name="client-side-encryption"></a>Kliendipoolne krüptimine

Me mainitud kliendipoolne krüptimist, kui arutada töödeldavate andmete krüptimine. See funktsioon võimaldab teil oma kliendi rakenduse enne saatmist kaabel üle kirjutada Azure Storage ja programmiliselt dekrüptida andmete allalaadimine Azure Storage pärast andmete programmiliselt krüptimiseks.

See anda krüptimine, kuid pakub krüptimist ja ülejäänud funktsiooni. Pange tähele, et kuigi andmed on krüptitud teel, endiselt soovitame kasutada HTTPS ära kasutada sisseehitatud andmete usaldusväärsuse kontrolli, mis aitavad leevendada võrgu vigu, mis mõjutavad andmete terviklus.

Näide, kus saate kasutada on, kui teil on veebirakendus, mis talletab plekid ja laadib plekid ning soovite olla võimalikult turvaliseks taotlus ja andmed. Sel juhul tuleks kasutada kliendipoolne krüptimine. Liikluse vahel klient ja Azure'i bloobimälu teenus sisaldab krüptitud ressurssi ja keegi saab tõlgendamine töödeldavate andmete ja Lahustage selle sisse oma isiklike plekid.

Kliendipoolne krüptimise on ehitatud Java ja .net-i salvestusruumi kliendi teegid, mis omakorda kasutavad Azure'i klahvi Vault API, mistõttu on üsna lihtne, saate rakendada. Krüptimine ja dekrüptimine andmeid kasutab ümbriku tehnika ning see talletab metaandmete kasutatavaid krüptimise iga salvestusruumi objekt. Näiteks plekid, see talletab selle bloobimälu metaandmete järjekordade jaoks see lisab selle iga sõnumi järjekorda.

Krüptimine ise, saate luua ja hallata oma krüptimise võtmed. Saate kasutada ka klahvid loodud Azure'i salvestusruumi kliendi teek, või lasta Azure'i klahvi Vault luua võtmed. Muutmiseks saate talletada oma kohapealse võtme salvestusruumi või saate need on Azure klahvi võlvkelder talletada. Azure'i klahvi Vault võimaldab juurdepääsu Azure klahvi võlvkelder saladusi kindlatele kasutajatele Azure Active Directory abil. See tähendab, et igaüks, mitte ainult Azure klahvi Vault lugeda ja tuua kasutate kliendipoolne krüptimise võtmed.

####<a name="resources"></a>Ressursid

-   [Krüptimiseks ja dekrüptida plekid Microsoft Azure Storage Azure'i klahvi Vault abil](storage-encrypt-decrypt-blobs-key-vault.md)

    Selles artiklis näitab, kuidas kasutada kliendipoolne krüptimist Azure'i klahvi Vault, sh selle KEK loomise ja salvestada selle autoriloomingut PowerShelli kaudu.

-   [Kliendipoolne krüptimise ja Azure võtme Vault Microsoft Azure Storage](storage-client-side-encryption.md)

    Selles artiklis annab kliendipoolne krüptimise selgitus ja antakse ülevaade salvestusruumi kliendi teek krüptimiseks ja dekrüptida ressursid neljast talletusmahu teenuste kasutamisel. Samuti räägib Azure'i klahvi Vault.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Azure'i ketta krüptimine krüptimiseks kasutatavat oma virtuaalmasinates ketast

Azure'i ketta krüptimine on uus funktsioon, mis on praegu eelvaade. See funktsioon võimaldab teil OS ketast ja andmete ketast kasutada, on IaaS virtuaalse masina krüptimiseks. Windowsi jaoks, krüptitud draivid industry-standard BitLockeri krüptimistehnoloogiat. Linuxi, on ketast on krüptitud DM-Crypt tehnoloogia abil. See on integreeritud Azure klahvi Vault abil saate määrata ja hallata ketta krüptimise võtmed.

Azure'i ketta krüptimise lahendus toetab kolme kliendi krüptimise järgmistel juhtudel:

-   Uus IaaS VMs loodud VHD kliendi krüptitud failide ja kliendi avaldatud krüptimise võtmed, mida talletatakse Azure'i klahvi Vault krüptimise lubamine.

-   Luba krüptimise uus IaaS VMs loodud Azure'i turuplatsilt.

-   Luba krüptimise olemasoleva IaaS VMs Azure juba töötab.

>[AZURE.NOTE] Juba töötab Azure Linux VMs või uue Linuxi VMs loodud: Azure'i turuplatsi pildid, OS ketta krüptimise praegu ei toetata. Krüptimist Linux vms OS maht on toetatud ainult VMs, mis olid krüptitud kohapealse ja Azure üles laadida. See piirang kehtib ainult OS ketas; krüptimist andmemahtudega Linux VM on toetatud.

Lahendus toetab järgmisi IaaS vms jaoks avaliku eelvaateversiooniga, kui Microsoft Azure lubatud.

-   Integreerimine Azure võtme Vault

-   Standard- [A, D ja G sarja IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) mudeli abil loodud IaaS VMs krüptimise lubamine

-   Kõik Azure avaliku [piirkondade](https://azure.microsoft.com/regions/)

See funktsioon tagab, et teie virtual arvuti kettale kõik andmed on krüptitud Azure Storage korral.

####<a name="resources"></a>Ressursid

-   [Azure'i ketta krüptimise Windowsi ja Linux IaaS Virtuaalmasinates](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    Selles artiklis käsitletakse eelvaateversiooniga Azure'i ketta krüptimise ja pakub link allalaadimiseks raamatus.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Azure'i ketta krüptimist, Kagu ja kliendipoolne krüptimise võrdlus

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs ja nende VHD failid

Kasutatavad IaaS VMs ketast, soovitame kasutada Azure ketta krüptimine. Saate sisse lülitada SSE krüptimiseks kasutatakse neid ketast Azure Storage tagasi VHD-faile, kuid ainult krüptib äsja kirjutatud andmed. See tähendab, et kui loote VM ja seejärel lubada SSE salvestusruumi kontol, mis hoiab VHD faili, ainult muudatuste krüptitud, mitte algse VHD faili.

Kui loote VM pildi kasutamise Azure'i turuplatsilt Azure'i sooritab [madal Kopeeri](https://en.wikipedia.org/wiki/Object_copying) pildi kontole salvestusruumi Azure Storage ja pole krüptitud isegi juhul, kui teil on lubatud SSE. Pärast seda VM loob ja käivitab värskendamine pilt, hakkavad SSE andmete krüptimine. See on parim Azure'i ketta krüptimise loodud: Azure'i turuplatsi pildid, kui soovite, et need täielikult krüptitud VMs kohta.

Kui te tuua eelnevalt krüptitud VM Azure asutusesisesest, saab üles laadida krüptimise võtmed Azure'i klahvi Vault ja edaspidi selle VM, mida kasutasite kohapealse krüptimine. Azure'i ketta krüptimine on lubatud käsitlema seda stsenaariumi.

Kui teil on asutusesisesest VHD-krüptitud, saate selle üleslaadimiseks Galerii kohandatud pildina ja VM see säte. Kui te seda ressursihaldur mallide kasutamine, saate selle sisselülitamiseks Azure'i ketta krüptimist, kui see käivitub VM esitada.

Kui lisate andmed kettale ja kinnitage VM, saate lülitada Azure'i ketta krüptimise sellel andmed asuvad. See esmalt andmete ketta kohalikult krüptida, ja teenuse haldus kiht tehke rongile kirjutamine vastu salvestusruumi nii salvestusruumi sisu on krüptitud.

####<a name="client-side-encryption"></a>Kliendipoolne krüptimine####

Kliendipoolne krüptimine on kõige ebaturvalisem viis, krüptimist, kuna see krüptib see enne teel ja krüptib veebisaidil ülejäänud andmed. Siiski ei nõua koodi lisamiseks rakenduste abil salvestusruumi, mida te ei soovi teha. Sel juhul saate kasutada HTTPs teel ja SSE andmete jaoks ülejäänud andmete krüptimiseks.

Kliendipoolne krüptimist, kus saate krüptida tabeli üksused, järjekorda sõnumeid ja plekid. SSE, kus saate krüptida ainult plekid. Kui peate tabeli ja järjekorda andmete krüptimist, kasutage kliendipoolne krüptimine.

Kliendipoolne krüptimise haldab täiesti rakendus. See kõige turvalisem lähenemine on, kuid nõuavad programmiline muudatusi teha rakenduse ja kasutusele võtme haldamise protsessid. Saate kasutage seda soovite eest turvalisus ajal, kui soovite oma talletatud andmed krüptida.

Kliendipoolne krüptimine on veel laadi klient ja teil on konto nii teie skaleeritavus plaanid, eriti siis, kui olete krüptimise ja suunamine palju andmeid.

####<a name="storage-service-encryption-sse"></a>Salvestusruumi teenuse krüptimise (SSE)

SSE haldab Azure Storage. Kasutades SSE ei paku töödeldavate andmete turvalisus, kuid see krüptida on kirjutatud Azure Storage. Ei ei mõjuta jõudlus selle funktsiooni kasutamisel.

Saate ainult krüptida Blokeeri plekid, lisa plekid ja lehel plekid SSE abil. Kui soovite krüptida tabeli andmeid või järjekorda andmeid, võiksite kasutada kliendipoolne krüptimine.

Kui teil on arhiivi või teegi VHD faile, mida luua uue virtuaalmasinates alusena kasutada, saate salvestusruumi uue konto loomine, SSE lubamine ja seejärel selle konto VHD failide üleslaadimiseks. Azure Storage krüptitud VHD-faile.

Kui teil on Azure ketta krüptimise ketast VM ja hoides VHD failide salvestusruumi kontol lubatud SSE lubatud, see töötab korralikult; selle tulemusena äsja kirjutatud andmed on krüptitud kaks korda.

##<a name="storage-analytics"></a>Salvestusruumi Analytics

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Salvestusruumi Analyticsi abil saate jälgida autoriseerimine tüüp

Iga salvestusruumi konto, saate lubada Azure salvestusruumi Analytics logimine ja mõõdikute andmete talletamiseks. See on suurepärase tööriista kasutamine, kui soovite kontrollida jõudluse mõõdikute salvestusruumi konto või tuleb tõrkeotsing salvestusruumi konto, sest teil on jõudlusega probleeme.

Mõne muu näete logid salvestusruumi analytics andmete on autentimise meetodit kasutada keegi salvestusruumi juurdepääsemisel. Näiteks näete koos bloobimälu, kui neid kasutada ühiskasutusse Accessi signatuuri või salvestusruumi konto abil või bloobimälu, juurde on avalik.

See võib olla väga kasulik, kui teil on kindlalt Valve juurdepääsu salvestusruumi. Näiteks bloobimälu saate määrata kõik selle ümbriste privaatne ja rakendada kogu rakenduste SAS teenuse kasutamist. Seejärel saate regulaarselt, et näha, kui teie plekid pääseb juurde klahvidega salvestusruumi konto, mis võivad viidata rikkumise turvalisus, või plekid on avalikud, kuid need ei tohiks logid.

####<a name="what-do-the-logs-look-like"></a>Kuidas logid välja näevad?

Kui lubate talletusruumi konto mõõdikute ja logimine Azure portaali kaudu, hakkavad analytics andmete kiiresti koguda. Logimine ja mõõdikute iga teenuse jaoks on eraldi; logimine on kirjutada ainult siis, kui seal on tegevuse salvestusruumi konto, ajal mõõdikud logitakse iga minut, iga tund või iga päev, olenevalt sellest, kuidas seadistada.

Blokeeri plekid ümbrises, nimega $logs salvestusruumi konto on talletatud logid. Kui salvestusruumi Analytics on lubatud, luuakse automaatselt see ümbris. Kui see ümbris on loodud, ei saa kustutada, kuigi saate kustutada selle sisu.

Jaotises $logs ümbris, on iga teenuse jaoks kausta ja seejärel on alamkaustad aasta ja kuu/päev/tunni jaoks. Klõpsake jaotises tunni, lihtsalt nummerdatud logid. See on kataloogi struktuuri on näha:

![Failide kuvamine](./media/storage-security-guide/image1.png)

Iga taotluse Azure Storage on sisse logitud. Siin on hetktõmmise logifaili, kus esimene mõned väljad.

![Logifaili hetktõmmis](./media/storage-security-guide/image2.png)

Näete, et saate jälgida mis tahes tüüpi salvestusruumi konto kõned logid.

####<a name="what-are-all-of-those-fields-for"></a>Mis on kõigi nende väljade jaoks?

On loendis allapoole ressursside artikkel sisaldab palju väljaloendi logid ja neid kasutatakse. Siin on väljaloendi järjestuses:

![Logifaili väljad hetktõmmis](./media/storage-security-guide/image3.png)

Me huvitatud GetBlob ja kuidas need on kinnitatud, kirjeid nii, et peame toiming-tüüpi "Get-bloobimälu" kirjeid otsida ja vaadata taotluse olek<sup>(4 veerus)</sup> ja autoriseerimine-tüüpi<sup>(8 veerus)</sup> .

Näiteks ülaltoodud loendis mõnele esimesele reale taotlus-olek on "Edu" ja luba-tüüpi "autenditakse". See tähendab, et kutse on kinnitatud, kasutades salvestusruumi konto võti.

####<a name="how-are-my-blobs-being-authenticated"></a>Kuidas on minu plekid on kinnitatud?

Meil on kolmes, mida me huvitatud.

1.  Funktsiooni bloobimälu on avalik ja see on kättesaadav, URL-i ilma ühiskasutusse Accessi allkirja. Sel juhul taotlus-olek on "AnonymousSuccess" ja luba-tüüp on "anonüümne".

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124 37; **anonüümse**; mystorage...

2.  Funktsiooni bloobimälu on privaatne ja kasutati ühiskasutusse Accessi allkirjastatud. Sel juhul taotlus-olek on "SASSuccess" ja luba-tüüp on "sas".

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**, 200; 416; 64; **sas**; mystorage...

3.  Funktsiooni bloobimälu on privaatne ja salvestusruumi võti kasutatud pääsete neile juurde. Sel juhul taotlus-olek on "**edu**" ja luba-tüüpi on "**autenditud**".

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Edu**, 206; 59; 22; **autenditud**; mystorage...

Microsoft sõnumi Analyzer abil saate vaatamiseks ja analüüsimiseks need logid. See sisaldab otsingu ja filtreerimise võimalusi. Näiteks võite otsida GetBlob kas kasutamine on, mida oodata, st veendumaks, et keegi on juurdepääs teie salvestusruumi konto eksikombel eksemplarid.

####<a name="resources"></a>Ressursid

-   [Salvestusruumi Analytics](storage-analytics.md)

    Selles artiklis on esitatud ülevaade salvestusruumi analüüsi- ja kuidas neid lubada.

-   [Salvestusruumi Analytics Logi vorming](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    Selles artiklis illustreerib salvestusruumi Analytics Logi vorming ja üksikasjade saadaolevad väljad, sh autentimistüüp, mis näitab liiki autentimine taotluse kasutada.

-   [Salvestusruumi konto Azure portaali jälgimine](storage-monitor-storage-account.md)

    Selles artiklis kirjeldatakse mõõdikute jälgimine ja logimine salvestusruumi konto konfigureerimine.

-   [Azure'i talletusruumi mõõdikute ja logimine, AzCopy ja sõnumi Analyzer abil lõpuni – tõrkeotsing](storage-e2e-troubleshooting.md)

    Selles artiklis räägib tõrkeotsingu abil salvestusruumi Analytics ja näidatakse, kuidas kasutada Microsoft sõnumi Analyzer.

-   [Microsoft sõnumi analüsaator opsüsteem juhend](https://technet.microsoft.com/library/jj649776.aspx)

    Selles artiklis on viide Microsoft sõnumi Analyzer ja mis sisaldab linke õpetuse, lühijuhend ja funktsioonide kokkuvõte.

##<a name="cross-origin-resource-sharing-cors"></a>Rist-päritolu ressursside jagamise (CORS)

###<a name="cross-domain-access-of-resources"></a>Domeenide juurdepääsu ressursid

Kui üks domeen töötab veebibrauseri teeb ressursi HTTP-päring muu domeeni, seda nimetatakse cross päritolu HTTP-päring. Näiteks HTML-lehele pakutakse contoso.com taotleb majutatud fabrikam.blob.core.windows.net jpeg. Turvalisuse huvides piirata brauserid rist-origin HTTP päringuid sees skriptide, nt JavaScript algatada. See tähendab, et kui mõned JavaScripti koodi veebilehele contoso.com taotleb selle jpeg fabrikam.blob.core.windows.net sisse, brauser ei luba taotluse.

Mida see tuleb teha Azure Storage? Ka siis, kui talletate staatilise vara, näiteks JSON- või XML-andmefailide bloobimälu salvestusruumi kontot nimega Fabrikam, varade domeeni saab fabrikam.blob.core.windows.net ja contoso.com veebirakenduse ei saa neile juurde pääseda JavaScripti, kuna domeenid erinevad. See on true, kui proovite ühte Azure'i salvestusruumi teenuste – näiteks Tabelimälu – kõne mis tagastavad JSON andmete töötlemiseks JavaScripti kliendi poolt.

####<a name="possible-solutions"></a>Võimalikud lahendused

Üks võimalus probleemi lahendamiseks on kohandatud domeen, nt "storage.contoso.com" fabrikam.blob.core.windows.net määramiseks. Probleem on selles, et saate määrata ainult kohandatud domeeni ühe salvestusruumi kontole. Mida teha, kui vara on talletatud mitme salvestusruumi konto?

Teine viis probleemi lahendamiseks on on veebirakenduse toimida proxy salvestusruumi kõnede jaoks. See tähendab, et kui bloobimälu faili üleslaadimisel, veebirakenduse oleks kas kohalik kirjutada ja seejärel kopeerige see bloobimälu või see lugeda kõik mällu ja seejärel bloobimälu kirjutada. Vaheldumisi võiks kirjutada sihtotstarbeline veebirakenduse (nt Veebiteenuste) mis lisatud failide kohalikult ja kirjutab need bloobimälu. Mõlemal juhul tuleb konto on skaleeritavus, kindlaks teha, kui seda funktsiooni.

####<a name="how-can-cors-help"></a>Kuidas aitab CORS?

Azure'i salvestusruumi võimaldab teil lubada CORS – Cross Origin ressursside ühiskasutus. Iga salvestusruumi konto, saate määrata domeenid, millele pääsete juurde ressursside kohta salvestusruumi konto. Näiteks meie puhul eespool kirjeldatud, saame CORS lubamine fabrikam.blob.core.windows.net salvestusruumi konto ja konfigureerida nii, et lubada juurdepääsu contoso.com. Seejärel web rakenduse contoso.com pääsevad otse ressursside kohta fabrikam.blob.core.windows.net.

Pange tähele, et te102827792ei CORS lubab juurdepääsu, kuid ei võimalda autentimist, mis on vaja jaoks kõik mitte-avalik Accessi salvestusruumi ressursside. See tähendab, et plekid pääseb juurde ainult kui need on avalikud või teie signatuur ühiskasutusse Accessi annab teile vastav õigus. Tabelite, järjekorrad ja failide puudub avaliku juurdepääsu ja nõuavad on SAS.

Vaikimisi on keelatud CORS kõik teenused. Saate lubada CORS REST API-ga või salvestusruumi kliendi teek helistamiseks määrata poliitikad teenuse meetodite abil. Kui te seda teha, saate kaasata CORS reegli, mis on XML-i. Siin on näide CORS reegli, mis on seatud salvestusruumi konto atribuutide seadmine toimingu bloobimälu teenuse abil. See toiming Azure Storage salvestusruumi kliendi teek või REST API-de abil saate teha.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Tähendus iga rea kohta:

-   **AllowedOrigins** See teada, millised-sobitamine domeenid saate taotleda ja vastu võtta andmete salvestusruumi teenuse. See on kirjas, et nii contoso.com ja fabrikam.com saab taotleda andmete bloobimälu teatud salvestusruumi konto. Saate ka seda metamärkide abil (\*) Kui soovite lubada juurdepääsu taotluste kõik domeenid.

-   **AllowedMethods** See on viise (HTTP taotluse verbide), mida saab kasutada taotluse tegemisel loendit. Selles näites on lubatud ainult panna ja saada. Saate seada seda metamärkide (\*) Kui soovite lubada kõik meetodid, mida kasutatakse.

-   **AllowedHeaders** See on origin domeeni saab määrata taotluse tegemisel taotluse päised. Selles näites kõik metaandmete päised x-ms-metaandmete, alustades x ms metaandmete sihtrühma ja x ms-metaandmete abc on lubatud. Metamärk (\*) näitab, et kõik päise alguses määratud eesliitega on lubatud.

-   **ExposedHeaders** See teada, millised vastus päised peaks esitatud taotluse väljaandja brauseri. Selles näites, mis tahes päis, alustades "x-ms - metaandmete-" on avatud.

-   **MaxAgeInSeconds** See on on maksimaalne brauseri vahemälu eelkontrolli SUVANDID päringu aeg. (Eelkontrolli taotluse kohta lisateabe saamiseks vaadake esimeses artiklis allpool.)

####<a name="resources"></a>Ressursid

CORS ja selle lubamise kohta lisateabe saamiseks vaadake järgmisi ressursse.

-   [Rist-päritolu ressursside jagamise (CORS) Azure Storage teenust Azure.com tugi](storage-cors-support.md)

    Selles artiklis antakse ülevaade CORS ja reeglid erinevad salvestusruumi teenuste seadmise kohta.

-   [Rist-päritolu ressursside jagamise (CORS) Azure Storage teenust MSDN-i tugi](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    See on viide CORS tugi Azure salvestusruumi teenuste dokumentatsioonist. See sisaldab linke artiklitele, rakendades iga salvestusteenus ja näide ja selgitused CORS faili iga element.

-   [Microsoft Azure'i salvestusruumi: Tutvustus CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    See on link algse ajaveebi artikkel väljakuulutamist CORS ja näitab, kuidas seda kasutada.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Korduma kippuvad küsimused Azure Storage turvalisuse kohta

1.  **Kuidas saan kontrollida terviklus plekid ma olen suunamine või sealt välja Azure Storage, kui ma ei saa kasutada HTTPS-protokolli?**

    Kui mingil põhjusel peate kasutama HTTP asemel HTTPS ja te töötate Blokeeri plekid, saate kasutada MD5 kontrollimine aidata terviklust plekid üle minna. See aitab kaitse võrgu/transport layer vigu, kuid mitte tingimata vahendaja eest.

    Kui kasutate HTTPS, mis pakub taseme turvalisuse, siis kasutades MD5 kontrollimine on üleliigsed ja tarbetu.
    
    Lisateabe saamiseks lugege teemat [Azure'i bloobimälu MD5 ülevaade](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Kuidas on lood FIPS-vastavuse USA valitsuse?**

    Funktsiooni USA Federal teabe töötlemise Standard (FIPS) määratleb cryptographic algoritmide Federal USA valitsuse arvutisüsteemide tundliku teabe kaitse kinnitama kasutamiseks. Lubada FIPS režiimi Windows server või töölaua ütleb OS et kasutataks ainult FIPS kinnitatud cryptographic algoritmide kohta. Kui rakendus kasutab mittevastavate algoritmide kohta, kas rakendustes katkestada. With.NET versiooni 4.5.2 või suurem, rakenduse automaatselt aktiveerib krüptograafia algoritmid FIPS ühilduv algoritmid kasutada, kui arvuti on FIPS-režiimis.

    Microsoft jätab iga kliendi otsustada, kas lubada FIPS-režiimis. Meie arvates, puudub mõjuv põhjus klientidele, kes ei kuulu valitsuse määrus lubamiseks vaikimisi FIPS-režiimis.

    **Ressursid**

-   [Miks me ei soovitavad "FIPS Mode" enam](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Ajaveebi selles artiklis antakse ülevaade FIPS ja selgitab, miks nad ei luba FIPS režiim vaikimisi.

-   [FIPS 140 valideerimine](https://technet.microsoft.com/library/cc750357.aspx)

    Sellest artiklist leiate teavet selle kohta, kuidas Microsofti toodete ja cryptographic moodulid järgima standardit FIPS, Saksamaa USA valitsuse.

-   ["Süsteemi krüptograafia: kasutage FIPS ühilduv algoritmid krüptimine, räsi ja allkirjastamise" security settings mõju Windows XP-s ja uuemad versioonid Windowsi](https://support.microsoft.com/kb/811833)

    Selles artiklis kirjeldatakse FIPS režiimi vanemate Windows arvutite kasutamise kohta.
