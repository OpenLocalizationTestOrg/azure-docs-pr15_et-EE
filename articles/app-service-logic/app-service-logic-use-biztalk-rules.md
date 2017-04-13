<properties
   pageTitle="Lisateavet ja loogika rakenduse BizTalki reeglid API rakenduse loomine | Microsoft Azure'i"
   description="Selles teemas käsitletakse BizTalki reeglid Connectori funktsioonide ja antakse juhiseid selle kasutamise kohta"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalki reeglid

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Business reeglid kapseloi poliitika ja otsuseid, mis juhib äriprotsesside. Need poliitikad võivad ametlikult määratletud Käsiraamatud, lepingute või lepingute või teadmisi või teadmisi sisalduvaid töötajad võivad olemas. Need poliitikad on dünaamiline ja muutuda üle ajal muudatuste tõttu ettevõttelepingud, määruste või muudel põhjustel.

Need poliitikad rakendamisel traditsiooniline programmeerimiskeele nõuab palju aega ja koordineerituna ja lubage osalemiseks loomine ja haldamine business poliitikate programmeerijad. BizTalki Business reeglid võimaldab kiiresti rakendada poliitika ja lahutada ülejäänud Äriprotsess. See võimaldab business poliitika mõjutavad äriprotsessi ülejäänud vajalikud muudatused teha.

##<a name="why-rules"></a>Miks reeglite

On 3 peamist põhjust BizTalki Business reeglite kasutamine Äriprotsess.

* Lahutada äriloogika rakenduse koodi kaudu
- Luba on business loogika juhtimise üle rohkem kontrolli ärianalüütikud
+ Äriloogika muudatused lähevad tootmise kiiremini

##<a name="rules-concepts"></a>Reeglite mõisted

##<a name="vocabulary"></a>Sõnavara

_Sõnavara_ on määratlused koosneb sõbralikke nimesid reegli tingimusi ja toiminguid kasutatakse arvuti objektide kogum. Sõnavara määratlused hõlbustamiseks reegleid lugeda, mõista ja inimesed kindla ärivaldkonna ühiskasutusse anda.

Sõnavara on mõeldud business semantika ja rakendamise vahel. Näiteks võib viidata andmete sidumine jaoks kinnituse oleku veeru rea teatud andmebaasis SQL-päringu arvuna. Asemel keerukate esituse selline reeglis, võite luua hoopis sõnavara määratlus, mis on seotud selle andmetega siduv, sõbralik nimi "Oleku." Seejärel saate kaasata "Status" mõni muu arv reeglite ja reegli mootori saate tuua vastavaid andmeid tabelist.

##<a name="rule"></a>Reegli

Business reeglid jagunevad deklaratiivseid laused, mis haldavad äriprotsesside läbiviimine. Reegli koosneb tingimus ja toimingud. Tingimus hinnatakse ja kui tingimus on tõene, käivitab mootori reegel üks või mitu toimingut.
Business reeglid eeskirjad on määratletud, kasutades järgmises vormingus:

_IF_ _tingimus_ _Seejärel_ _toiming_

Näide:

*Kui kogus on väiksem kui või võrdne vahendite*  
*SEEJÄREL läbiviimine tehingu ja Prindi vastuvõtmine*

See reegel määrab, kas tehing toimub, rakendades äriloogika võrdlemiseks kahe rahaliste väärtuste tehingu summa ja vahendite kujul kujul.
Saate Business reegli loomine, muutmine ja juurutamine business reeglid. Teise võimalusena saate teha eelnenud toiminguid programmiliselt.

###<a name="conditions"></a>Tingimused

Tingimus on tõene/väär (TRUE) avaldis, mis koosneb ühe või mitme predicates.
Selles näites rakendatakse summa ja vahendite põhiliste vähem kui või võrdne. See tingimus alati väärtuseks true või false.
Predicates saab kombineerida koos loogika tehtemärgid AND, OR, mitte vormi loogiline avaldis, mis on üsna potentsiaalselt suur, kuid hindab alati, kas true või false.

###<a name="actions"></a>Toimingud

Toimingud on tingimus hindamise otstarbekas tagajärjed. Kui reegli tingimus on täidetud, käivitatakse vastava toimingu või toimingute.
Selles näites on "läbiviimine tehing" ja "printimine teatis" toiminguid, mis tehakse, kui ja ainult siis, kui tingimus (sel juhul "kui kogus on väiksem kui või võrdne vahendite") on tõene.
Business reeglid raames esindavad toimingud määramine toimingute XML-i dokumentide osas.

##<a name="policy"></a>Poliitika

Poliitika on loogiline rühmitamine reeglite. Saate koostada poliitika, salvestage see, katsetage seda ja, kui olete rahul tulemuste saamiseks kasutage tootmiskeskkonnas.

###<a name="policy-composition"></a>Poliitika koosseis

Saate koostada poliitikate reeglid portaalis. Poliitika võib sisaldada meelevaldselt suurte reeglikomplekti, kuid tavaliselt koostate poliitika reeglitest, mis on seotud kindla business domeeni rakendus, mida kasutate poliitika kontekstis.

###<a name="policy-testing"></a>Poliitika testimine
Enne selle kasutamist tootmiskeskkonnas, saate oma poliitika katse tõhusalt teha. Reeglite portaali abil saate esitada lähteandmed poliitika, käivitage poliitika ja selle väljundi kuvamine. Väljund sisaldab logid, reegli täitmise tingimus hindamise tulemusena tekkivad väljundid.

##<a name="sample-scenario---insurance-claims"></a>Valimi stsenaarium - kindlustusnõuete
Võtame valimi stsenaarium ja tutvustavad, kuna me koostamine äriloogika sama.

![Asetekst][1]

Väga lihtne kindlustusnõuete stsenaariumi avaldaja leiab tema kindlustusnõude (kaudu mis tahes klient, nagu veebisaidi, telefonis rakendus jne). Taotluste taotleda saab saata ettevõtte taotluste töötlemise üksus ja töötlemise tulemi põhjal, nõude võib olla kas kinnitatud tagasi lükatud või saadetud mööda käsitsi töötlemine.
Taotluste töötlemise ühiku meie stsenaarium oleks üks hõlmab äriloogika süsteem. Võtaks lähemalt vaadata selle üksuse, siis me näeme järgmist:

![Asetekst][2]

Andke meile kohe kasutada Business reegleid rakendada selle äriloogika.


##<a name="creation-of-rules-api-app"></a>Reeglite Api rakenduse loomine


1. Azure'i portaali sisselogimine
2. Valige -> uus turuplats ja seejärel otsige *BizTalki reeglid*
3. Valige tulemite loendist BizTalki reeglid. Avaneb tera BizTalki reeglid
4. Klõpsake nuppu *Loo* ![asetekst][3]
1. Uue tera, mis avab, sisestage järgmine teave:  
    1. Nimi – anna reeglid API rakenduse nimi
    1. Rakenduse teenusleping valida või luua uue rakenduse teenuse kavandamine
    1. Hinnad taseme – valige hinnakirjad taseme, kui soovite, et see rakendus asu
    1. Ressursirühm – saate valida või luua kui rakendus peaks asu ressursirühm
    2. Tellimuse – valige soovitud tellimus, mida soovite kasutada
    1. Asukoht – valige geograafiline asukoht, kuhu soovite rakenduse kasutusele võtta.
4.  Valige *Loo*. Mõne minuti jooksul BizTalki reeglid API rakenduse loomisel.

##<a name="vocabulary-creation"></a>Sõnavara loomine
Pärast BizTalki reeglid API rakenduse loomist oleks järgmise juhise juurde luua sõnavara. Eeldatakse, et arendaja on rohkem levinud persona teha seda kasutada. Siit saate teada, kuidas seda teha:


1. Käivitada oma BizTalki reeglite API rakenduse portaalist lahkumiseks valige Sirvi -> API rakendused - ><Your Rules API App>. See viib teid reeglid API rakenduse armatuurlaua sarnaneb allpool:

   ![Asetekst][4]

2. Valige "Sõnavara definitsioonid". Näitab, sõnavara loome Kuva 3. Valige "lisa" hakata uue sõnavara määratlusi.
On 2 tüüpi sõnavara määratlused praegu toetatud – Literaalmärgid ning XML-i.

##<a name="literal-definition"></a>Sõnasõnaline määratlus
1.  Pärast nupu "Lisa", "Lisada määratlus" uus tera avab. Sisestage järgmised väärtused.
  1.    Nimi – mis tahes erimärkideta oodatakse ainult tähti ja numbreid. See peaks olema kordumatu olemasoleva loendi sõnavara määratlus.
  2.    Kirjeldus – valikuline välja.
  3.    Tippige määratlus – on 2 tüüpi toetatud. Selles näites Literaalmärgid valimine
  4.    Andmetüüp – see võimaldab kasutajatel valida määratluse andmetüüpi. Hetkel on toetatud andmetüübid 4: ma.  Stringi – need väärtused peavad olema kantud jutumärkide ("näide String")  
    II. Kahendmuutujaga – see võib olla kas tõene või väär  
    III.    Arv – võib olla mis tahes kümnendarvu  
    IV. Kuupäeva ja kellaaja – see tähendab, et selle def on tippige kuupäev tüüp. Andmed tuleb sisestada selles vormingus – KK/PP/AAAA hh:mm:ss AM\PM  
  5. Sisestusmeetodi – see on väärtus definitsiooni sisestamiseks. Siin sisestatud väärtused peavad vastama valitud andmetüüpi. Võite sisestada ühe väärtuse, komad või vahemiku väärtuste abil soovitud märksõna *abil*eraldatud väärtuste kogumist. Näiteks saate sisestada kordumatu väärtus 1; kogum 1, 2, 3; või vahemikus 1 kuni 5. Pange tähele, et vahemiku toetatakse ainult arvud.
  6. Klõpsake nuppu *OK*.

![Asetekst][5]
##<a name="xml-definition"></a>XML-i määratlus
Kui sõnavara tüüp on valitud XML-i, järgmised sisendina peab olema määratud  
  lisamine.    Skeemi – see klõpsates avatakse uus tera võimaldab kasutaja valida juba üleslaaditud skeemid või võimaldab teil laadida uus loend.
b.    XPATH – see sisestusmeetodi saab lukustamata alles pärast skeemi valimise eelmises etapis. Selle klõpsamisel kuvatakse skeem, mis on valitud ja võimaldab kasutajal valige sõlm, mille sõnavara määratlus tuleb luua.  
  c.    FACT – järgmist sisestusviisi tuvastab, millisele objektile andmeid soovite suunatakse reeglid mootori töötlemiseks. See on ka täpsemate atribuutide ja on vaikimisi valitud XPATH ema. FACT muutub eriti oluline Aheldamise ja saidikogumi stsenaariumid.

![Asetekst][6]

### <a name="add-bulk"></a>Hulgi lisada.
Eespool kirjeldatud toimingud jäädvustatud kogemus sõnavara määratluste loomine. Kui loodud, kuvatakse loendi vorm loodud sõnavara määratlusi. Ei oleks võimalik luua mitme määratlused sama skeemiga asemel korrates eespool kirjeldatud toimingud iga kord. See on, kus hulgi lisada võimalus muutub väga kasulik.
Valige "Lisa hulgi" viib teid uue tera. Kõigepealt tuleb valida skeemi, mille mitme määratlused on loodud. Valige see avatakse uus laba, lubades, kas saate valida loendist juba üleslaaditud skeemid või lubades üles laadida uus.
Nüüd saab lukustamata XPATHS atribuuti. Valige see avatakse skeemi Viewer, kus saate valida mitme sõlmed.
Mitme määratlusi loodud nimed vaikimisi valitud sõlme nimi. Neid saab muuta alati pärast loomist.

![Asetekst][7]

##<a name="policy-creation"></a>Poliitika loomine
Kui arendaja on loodud vajalik sõnavara, eeldatakse, et selle ärianalüütik oleks üks loomise Business poliitikate Azure portaali kaudu.  
    1. Klõpsake rakenduse reeglid loonud, on poliitika lens, mis läheb kasutaja poliitika loomise lehele klõpsake nuppu.  
    2. Sellel lehel kuvatakse nimekirja, mis on selle kindla reeglid rakenduse. Analüüsija saate lisada uue poliitika lihtsalt poliitika nime tippimist ja tabab menüü kaks korda. Mitu poliitika saate asuda reeglid API ühest rakendusest.  
    3. Valige loodud poliitika võtab kasutaja poliitika üksikasjade lehe kus näha reeglid, mis on poliitika.  
    ![Aseteksti][8]
 4.  Valige "Lisa", et lisada uus reegel. See viib teid uus laba.

##<a name="rule-creation"></a>Reegli loomine
Reegel on saidikogumi tingimus ja toiming. Toimingud on täidetud, kui tingimus annab tulemiks true. Reegli loomine tera, andke (poliitika) reegli nimi ja kirjeldus (valikuline).
Väljal tingimust (kui) saab keerukate tingimusvormingu aruannete loomiseks. Järgnevalt on märksõnad, mis on toetatud.  
1.  Ja – tingimusvormingu tehtemärk  
2.  Või – tingimusvormingu tehtemärk  
3.  Kas\_ei\_olemas  
4.  on olemas  
5.  FALSE  
6.  on\_võrdne\_abil  
7.  on\_suurem\_kui  
8.  on\_suurem\_kui\_võrdne\_abil  
9.  on\_sisse  
10. on\_vähem\_kui  
11. on\_vähem\_kui\_võrdne\_abil  
12. on\_ei\_sisse  
13. on\_ei\_võrdne\_abil  
14. mod  
15. True  

Väljal Action(THEN) võib sisaldada mitut laused, ühe rea loomiseks toiminguid, mida tuleb sooritada kohta. Järgnevalt on märksõnad, mis on toetatud.  
1.  võrdub  
2.  FALSE  
3.  True  
4.  peatada  
5.  mod  
6.  null  
7.  värskendamine  

Tingimus ja toiming ruudud pakuvad IntelliSense'i aitavad Autor reegli kiiresti. Seda saab käivitada pihta klahvikombinatsiooni ctrl + tühik või hakanud just tippida. Märksõnade sobitamine tipitud märgid automaatselt alla filtreeritud ja näidatud. IntelliSense'i aken näitab kõigi märksõnade ja sõnavara määratlusi.
![Asetekst][9]

##<a name="explicit-forward-chaining"></a>Konkreetsete Aheldamise edasi
Konkreetsete edasi Aheldamise juhul, kui kasutajad ümber hinnata vastuseks teatud toimingute reeglite BizTalki reeglid toetab need võib põhjustada see teatud märksõnade abil. Järgnevalt on märksõnad, mis on toetatud.  
   1.   Värskendage <vocabulary definition> – selle märksõna hindab kõik reeglid, mis on määratud sõnavara määratluse kasutamine oma tingimust.  
   2.   Peatada – selle märksõna lõpetab reegli kõik täitmised

##<a name="enabledisable-rules"></a>Enable\Disable reeglid
Iga reegli poliitika saate lubada või keelata. Vaikimisi on lubatud kõik reeglid. Keelatud reegleid ei poliitika käitamise ajal käivitada. Enable\Disable reegleid saab teha kas reegli keelest otse – käsud on saadaval tera kohal asuvat käsuriba või poliitikast, kontekstimenüü (Paremklõpsake reegli) on võimalus enable\disable.

##<a name="rule-priority"></a>Reegli prioriteet
Kõik reeglid poliitika rakendatakse järjestuses. Täitmise prioriteet määratakse poliitika paiknemise järjekorras. See prioriteet saab muuta lihtsalt pukseerige reegel.

##<a name="test-policy"></a>Poliitika testimine
Testida poliitika tera "Testida poliitika" käsu abil saate oma poliitikate testida. See blade näete nimekirja sõnavara mõisted, mida kasutatakse poliitika, mis nõuavad kasutaja sisendist. Kasutajad saavad käsitsi lisada need oma test stsenaarium sisendit väärtused. Vaheldumisi kasutajad valida ka importimiseks testimine XMLs sisendina. Kui küljed on, test saate käivitada ja iga sõnavara määratluse väljundeid kuvatakse veerus väljundi lihtne võrdlus. Ärianalüütik sõbralik logid vaatamiseks klõpsake "Kuva logid" vaatamiseks täitmise logid. Logide salvestamiseks "Salvesta väljundi" suvand on saadaval talletada kõik testida seotud andmete analüüsimiseks sõltumatu.

## <a name="using-rules-in-logic-apps"></a>Loogika rakendustes reeglite kasutamine
Kui poliitika on autoriks ja katsetada, on nüüd kasutusvalmis. Saate luua uue rakenduse loogika, valides loogika rakenduste portaali avalehe vasakus servas. Kui teie loogika rakendus on loodud, käivitage see ja seejärel valige *päästikute ja toimingud*. Seejärel saate valida malli *loomine algusest peale* . Järgige loogika rakenduse BizTalki reeglid API rakenduse lisamiseks. Kui see on tehtud, saab valida, millised reeglid API rakenduse target (toiming). Hõlmavad poliitika, mis tuleb sooritada. Valige kindla poliitika, pärast mida vaja sisendeid poliitika peab olema antud. Kasutajad saavad kasutada reeglite API rakenduse tagapool väljund edasise otsuste tegemiseks.

## <a name="using-rules-via-apis"></a>Reeglite kaudu API-de kasutamine
Reeglite API rakenduse saate kasutada ka suurel hulgal API-de kasutamine. See viis kasutajate ei ole piiranud lihtsalt loogika rakenduste kasutamise ja saate kasutada reeglid mis tahes rakenduse järgi ülejäänud helistamist. Täpse REST API-de saadaval saab vaadata, klõpsates "API määratlusega" lens reeglid armatuurlaual.

![Asetekst][10]

Järgmine on näide sellest, kuidas võib kasutada seda API c#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Pange tähele, et ülaltoodud reeglid API rakenduse selle väärtuseks "Avaliku Anon" turbesätted. See saab valida API rakenduse abil - kõik sätted -> rakenduse sätted -> juurdepääsutase.

![Asetekst][11]

## <a name="editing-vocabulary-and-policy"></a>Sõnavara ja poliitika redigeerimine
Üks peamisi eeliseid kasutamisel on, et muudatused äriloogika saab lükata tootmise palju kiiremini. Sõnavara ja poliitikate tehtud muudatused rakendatakse kohe valmistamisel. Liikuge sirvides vastav sõnavara definitsiooni või poliitika ja tehke soovitud muudatused jõustuvad on lihtsalt peab kasutaja.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
