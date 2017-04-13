<properties
    pageTitle="Vabastage märkmete Azure BizTalki teenuste jaoks | Microsoft Azure'i BizTalki teenused"
    description="Teadaolevad probleemid, mis on loetletud Azure'i BizTalki teenuste jaoks" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Azure'i BizTalki teenuste väljalaskemärkmed

Microsoft Azure'i BizTalki teenuste väljalaskemärkmed sisaldavad teadaolevad probleemid selles versioonis.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Mis on uut rakenduses BizTalki teenuste November värskendamine
* Ülejäänud krüptimist saab lubada BizTalki teenuste portaalis. Vt [ülejäänud BizTalki teenuste portaali aadressil krüptimine](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Värskenduste ajalugu

### <a name="october-update"></a>Oktoober värskendamine

* Ettevõtte kontod on toetatud:  
 * **Stsenaarium**: registreeritud BizTalki teenuse juurutamine Microsofti kontoga (nt user@live.com). Selle stsenaariumi korral ainult Microsofti Account kasutajad saavad hallata BizTalki teenuse BizTalki teenuste portaalis. Organisatsioonikonto ei saa kasutada.  
 * **Stsenaarium**: registreeritud ettevõttekontoga Azure Active Directory juurutuse BizTalki teenuse (nt user@fabrikam.com või user@contoso.com). Selle stsenaariumi korral saate ainult Azure Active Directory kasutajad sama ettevõttes BizTalki teenuste portaalis BizTalki teenust hallata. Microsofti kontot ei saa kasutada.  
* Kui loote BizTalki teenuse Azure klassikaline portaalis, on registreeritud automaatselt BizTalki teenuste portaalis.
 * **Stsenaarium**: Azure'i klassikaline portaali sisse logite BizTalki teenuse loomine ja seejärel valige **Halda** väga esimest korda. BizTalki teenuste portaali avamisel BizTalki teenuse automaatselt registreerib ja ongi valmis oma juurutuste.  
 Lugege teemat [registreerimisel ja värskendamine BizTalki teenuse juurutamise BizTalki Services portaal](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>August 14 värskendamine
* Lepingu ja bridge lahutamine – partneri lepingud ja sillad nüüd sidumata BizTalki teenuste portaalis. Nüüd loote lepingud ja sillad eraldi ja käitusajal sillad lahendamiseks lepingu EDI sõnumi väärtuste põhjal. Vaadake teemat [Azure BizTalki teenuste lepingute loomine](https://msdn.microsoft.com/library/azure/hh689908.aspx), [loomine EDI bridge, mis BizTalki teenuste portaalis](https://msdn.microsoft.com/library/azure/dn793986.aspx), [Loo AS2 bridge, mis BizTalki teenuste portaalis](https://msdn.microsoft.com/library/azure/dn793993.aspx), ja [Kuidas lahendada sillad lepingute käitusajal?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Võimalus luua lepingute Mallid on peatatud.  
* Saada-side lepingu, nüüd saate määrata erinevaid eraldaja komplekti iga skeemi. Selle konfiguratsiooni on määratletud saada lepinguga protokolli sätted. Lisateabe saamiseks lugege teemat [loomine ja X12 lepingu Azure'i BizTalki teenuste](https://msdn.microsoft.com/library/azure/hh689847.aspx) ja [loomine EDIFACT lepingu Azure'i BizTalki teenused](https://msdn.microsoft.com/library/azure/dn606267.aspx). Samuti lisatakse kaks uut üksust TPM OM API samal otstarbel. Vt [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) ja [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standardse XSD importida, sh saadud tüüpi, on nüüd toetatud. Lugege [kasutamine standard XSD sisuosast oma kaartidel](https://msdn.microsoft.com/library/azure/dn793987.aspx) ja [Kasutada saadud tüüpi vastendamise stsenaariumid ja näited](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 toetab uue mikrofoni algoritmide sõnumi allkirjastamiseks ja uus krüptimise algoritmide kohta. Vaadake [luua AS2 lepingu Azure BizTalki teenused](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Leida probleemid

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Ühenduvusprobleemide pärast BizTalki Services portaal värskendamine

  Kui teil on BizTalki teenuste portaali avamine ajal BizTalki teenused on üle viidud versioonile teenuse muudatused pöörata, võite näo ühenduvusprobleemide BizTalki Services portaal.  
  Lahendusena võib taaskäivitage brauser, brauseri vahemälu kustutada või alustada portaali privaatsusrežiim.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE ei leia artefakt, kui klõpsate tõrketeade või Hoiatus BizTalki teenuste projektis
Installige selle Visual Studio 2012 Update 3 RC 1 probleemi lahendamiseks.  

### <a name="custom-binding-project-reference"></a>Kohandatud sidumine projekti viite
Kaaluge BizTalki teenuste projekti lahenduse Visual Studios järgmistes olukordades:  
* Sama lahenduse Visual Studios on BizTalki teenuste projekti- ja kohandatud sidumine projekti. BizTalki teenuse project on viide selle kohandatud sidumine projekti faili.
* BizTalki teenuse projekt on kohandatud sidumine/käitumine DLL viide.

"Koostate' lahenduse Visual Studios edukalt. Seejärel 'taasloomine või puhastada lahendus. Pärast seda, taastada või puhastada uuesti järgmine tõrge:  
  Ei saa kopeerida faili <Path to DLL> "bin\Debug\FileName.dll" abil. Protsess ei pääse faili "bin\Debug\FileName.dll", kuna see on kasutusel mõni muu protsess.  

#### <a name="workaround"></a>Lahendus
* Kui [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) on installitud, on teil järgmisest kahest suvandist.

  * Taaskäivitage Visual Studios, või

  * Taaskäivitage lahendus. Seejärel teha ainult Koosta lahendus.  

* Kui [Visual Studio 2012 värskenduse 3](https://www.microsoft.com/download/details.aspx?id=39305) pole installitud, avage tegumihaldur, klõpsake vahekaardil Protsessid, valige MSBuild.exe protsess ja seejärel klõpsake nuppu Lõpeta protsess.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Marsruutimise BasicHttpRelay lõpp-punktid ei toetata sillad ja BizTalki Services portaal kui mitteprinditavaid märke on esiletõstetud HTTP päistena

Kui kasutate mitteprinditavaid märke liigendatud atribuudid osana sõnumite jaoks, ei saa neid sõnumeid suunatakse relay sihtkohta, mis kasutavad BasicHttpRelay sidumine. Lisaks esiletõstetud atribuutide mis on saadaval jälitus osa on URL-kodeeringuga plekid ja un encoded sihtkohtade.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>LAHTI saadetakse asünkroonselt isegi juhul, kui suvand saada asünkroonne lahti on märkimata  
Kaaluge selle stsenaariumi – kui soovite, märkige ruut **saada asünkroonne lahti** , määrake URL-i saatmine asünkroonne lahti, ja seejärel tühjendage ruut **saada asünkroonne lahti** uuesti selle lahti endiselt saadetakse määratud URL-i isegi siis, kui saate saata asünkroonne hallatavate andmevõrguteenuste pole valitud.  
Lahendusena, peate tühjendage märkeruut **saada asünkroonne lahti** eemaldades enne määratud URL ja seejärel juurutada AS2 lepingu.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Lisaks kehtiv andmevahetuse tühikumärke põhjustada tühja meilisõnumi saata Peata lõpp-punkti  
Kui loendis on lisaks ka IEA lõigu tühikud, ning disassembler käsitleb see praeguse andmevahetuse lõppu ja suunatud täitmisega tühikud järgmine teade. Kuna see pole kehtiv andmevahetuse, võib jälgida, et ühe eduka sõnumi saatmist marsruutimiseks sihtkohta ja ühe tühja sõnumi saatmist Peata lõpp-punkti.  
### <a name="tracking-in-biztalk-services-portal"></a>BizTalki teenuste portaalis jälgimine  
Jälgimise sündmuste on jäädvustatud kuni EDI sõnumi töötlemine ja mis tahes korrelatsioonikordaja. Sõnumi nurjumisel väljaspool protokolli esitusala jälitus kuvatakse nii eduka. Sellisel juhul viitavad LOG jaotises **üksikasjad** veerus **jälgida** tõrke üksikasjad.
Funktsiooni X12 sätted saatmiseks ja vastuvõtmiseks ([loomine ja X12 lepingu Azure'i BizTalki teenuste](https://msdn.microsoft.com/library/azure/hh689847.aspx)) teavet protokolli esitusala.  

### <a name="update-agreement"></a>Lepingu värskendamine  
BizTalki Services portaal võimaldab teil muuta kvalifikatsioon, identiteedi lepingu konfigureerimisel. See võib põhjustada inconsistence atribuudid. Näiteks on lepingu ZZ:1234567 ja ZZ:7654321 on kvalifikatsioon abil. BizTalki Services portaal profiili sätteid, muudate ZZ:1234567 olema 01:ChangedValue. Avatud lepingu ja 01:ChangedValue kuvatakse ZZ:1234567 asemel.
Kvalifikatsioon, identiteedi muutmiseks kustutamine lepingu, **identiteedid** partneri profiili värskendamine ja seejärel uuesti lepingu.  
> AZURE'I. Hoiatus seda käitumist mõjutab X12 ja AS2.  

### <a name="as2-attachments"></a>AS2 manused  
Manuste AS2 sõnumeid ei toeta saata või vastu võtta. Täpsemalt manused vaikselt ignoreeritakse ja sõnumi kehasse töötlemise tavaline AS2 sõnumina.  
### <a name="resources-remembering-path"></a>Ressursid: Uuendatakse tee  
**Ressursside**lisamisel võib mäleta dialoogiaken varem kasutanud lisada ressursi tee. Meeles pidada tee varem kasutanud, proovige Internet Exploreri **Usaldusväärsete saitide** lisamine BizTalki Services portaal veebisaiti.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Kui üksuse nime silla ümber nimetada ja sulgeda ilma muudatusi salvestamata projekt, üksus uuesti avamist tulemuseks tõrge
Kaaluge stsenaarium järgmises järjestuses.  
* BizTalki teenuse project silla (nt mõne XML-i One-Way Bridge) lisamine  

* Pange silla, määrates selle isiku nimi atribuudi väärtus. See teie määratud nimi seotud .bridgeconfig faili ümbernimetamine  

* Sulgege fail .bcs (sulgedes menüü Visual Studios) muudatusi salvestamata.  

* Avage fail .bcs Solution Exploreris uuesti.  
Märkate, et ajal seotud .bridgeconfig fail on määratud uus nimi, isiku nimi konstruktsioonipinnalt on endiselt vana nimega. Kui proovite Bridge konfiguratsiooni avamiseks topeltklõpsake bridge komponent, kuvatakse järgmine tõrketeade:  
  "<old name>"Üksus on seotud faili"<old name>.bridgeconfig" pole olemas  
Vältida seda stsenaariumi, veenduge, et muudatuste salvestamine pärast ümbernimetamist BizTalki teenuse projektis üksused.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalki teenuse project koostab edukalt isegi siis, kui artefakt välja Visual Studio projekti
Kaaluge stsenaarium, kui lisate artefakt (nt XSD-faili) BizTalki teenuse Project, kaasata selle artefakt Bridge konfiguratsioon (nt määrates selle taotluse sõnumi tüübiks) ja seejärel välistamine Visual Studio projekti. Sellisel juhul koostamise projekti ei anna viga kui kustutatud artefakt kättesaadav samas kohas, kus on kaasatud Visual Studio projekti kettal.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>BizTalki teenuse project ei kontrolli skeemi-saadavus konfigureerimisel sillad
BizTalki teenuse Projectis, kui projekt on lisatud skeemi impordib mõni muu skeem, BizTalki teenuse project ei kontrolli, kas imporditud skeemi on lisatud projekt. Kui proovite luua sellise projekti, mida ei saa Koosta vigu.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>XML-i taotluse-vasta silda vastuse sõnum on alati UTF-8 märgistik.
Selles versioonis märgistik vastuse sõnumi XML-i taotluse-vasta sillalt seatakse alati UTF-8.
### <a name="user-defined-datatypes"></a>Kasutaja määratletud andmetüübid
Käesolev artikkel kehtib adapterit sees BizTalki adapterit teenuse funktsiooni saate kasutada kasutaja määratletud andmetüübid adapterit toimingute.
Kasutaja määratletud andmetüübid kasutamisel kopeerimine ketas: \Programmifailid\Microsoft BizTalki adapterit Service\BAServiceRuntime\bin\ või abil globaalsesse vahemällu (GAC) BizTalki adapterit teenuse majutusteenuses serveris failid (dll). Muul juhul järgmine tõrge võib ilmneda klient.  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Soovitatav on GACUtil.exe abil saate installida faili komplekti globaalsesse vahemällu. GACUtil.exe dokumendid, kuidas kasutada seda tööriista ja Visual Studio Käsurea võtmed.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Veebisaidi BizTalki adapterit teenuse taaskäivitamine
**BizTalki adapterit teenuse käitusaja** installimise* loob soovitud * *BizTalki adapterit teenuse* * veebisait IIS-i, mis sisaldab soovitud * *BAService* * rakenduste.* *BAService** rakendus ettevõttesiseselt kasutab relay sidumine kohapealne teenuse lõpp-punkti pilveteenusesse ulatust laiendada. On majutatud teenuse kohapealse, registreeritakse vastavate relay lõpp-punkti kohta teenuse siini ainult asutusesisese teenuse käivitamisel.  

Kui lõpetada ja käivitage rakendus, automaatne käivitamine rakenduse konfigureerimine on au. Nii kui **BAService** lõpetada, tuleb alati taaskäivitada **BizTalki adapterit teenuse** veebisaidi hoopis. Alustada või lõpetada **BAService** rakendus.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Erimärkide sisestamine ei tohi kasutada aadress ja üksuse nimed LOB komponendid
Ärge kasutage erimärgid aadress ja üksuse nimede LOB komponendid. Kui te seda teete, saate tõrke projekti BizTalki teenuse juurutamisel. Teatud märgid, näiteks "%", veebisaidi BizTalki adapterit teenuse võib minna peatatud olekus ja on teil selle käsitsi käivitamiseks.
### <a name="test-map-with-get-context-property"></a>Get seoses vara Map testimine
Kui teisenduse sisaldab **Saada seoses vara** kaardi toiming, **Kaardi Test** nurjub. Ajutise lahendusena, Asendage **Saada seoses vara** kaardi toimingu fiktiivne andmeid sisaldava stringi Concatenate kaardi toiminguga. See asustada target skeem ja võimaldab teil testida transformatsioon muud funktsioonid.
### <a name="test-map-property-does-not-display"></a>Testi Vastenda atribuuti ei kuvata
Visual Studio kuvada **Testi vastenduse** atribuudid. See võib juhtuda siis, kui korraga ei dokitud aknas **Atribuudid** ja **Lahenduse Exploreri** aknas. Probleemi lahendamiseks dokkida **Atribuudid** ja **Lahenduse Exploreri** aknad.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Kuupäeva ja kellaaja vormindada drop-down on Hall
Kui kuupäeva ja kellaaja vormindada kaardi toiming on lisatud kujunduspinna ja konfigureeritud, võib-olla tuhm ripploendist vorming. See võib juhtuda siis, kui arvuti ekraan on **Keskmine – 125%** või **suurem – 150%**. Probleemi lahendamiseks seadke Kuva **väiksem – 100% (vaikesäte)** , kasutades järgmist:  
1. Avage **Juhtpaneel** ja klõpsake käsku **ilme ja isikupärastamine**.
2. Klõpsake nuppu **Kuva**.
3. **Väiksem – 100% (vaikesäte)** ja klõpsake nuppu **Rakenda**.

**Vorming** ripploendist peaks nüüd õigesti töötada.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Dubleeritud lepingute BizTalki teenuste portaalis
Võtke arvesse järgmist stsenaariumi.
1. Lepingu börsipäev partnerite halduse OM API abil saate luua.
2. Avage lepingu BizTalki teenuste portaalis kaks eri vahekaartidel.
3. Lepingu nii vahekaartidel juurutamine.
4. Selle tulemusena nii lepingute kasutada tulemuseks Topeltkirjete BizTalki teenuste portaalis

**Lahendus**. Avage mõni dubleeritud lepingute BizTalki teenuste portaali ja kasutuselt eemaldada.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Sillad kasutada värskendatud sert, isegi pärast sert on värskendatud artefakt poes
Kujutage ette järgmisi olukordi.  

**Stsenaarium 1: Sõrmejälje-põhiste sertide kasutamine turvaliseks sõnumi üleviimine silla teenuse lõpp-punkti**  
Kaaluge stsenaarium, kus kasutada BizTalki teenuse projektis sõrmejälje-põhiste serdid. Saate värskendada BizTalki teenuste portaalis serdi sama nime, kuid erinevad sõrmejälje, kuid ei saa värskendada BizTalki teenuse project vastavalt sellele. Sel juhul, võivad silla jätkata töötlemine sõnumid, kuna vanemate serdi andmeid võib siiski kanali vahemälu. Pärast seda, sõnumi töötlemine nurjub.  

**Lahendus**: värskendage serdi BizTalki teenuse Projecti ja Juurutage uuesti projekt.  

**Stsenaarium 2: Nime alusel käitumist abil tuvastada sõnumi üleviimine silla teenuse lõpp-punkti turvaliseks serdid**

Kaaluge stsenaarium, kus saate kasutada nime alusel käitumist BizTalki teenuse projektis serdid tuvastamiseks. Serdi BizTalki portaalis teenuste uuendamisel, kuid ei saa värskendada BizTalki teenuse project vastavalt. Sel juhul, võivad silla jätkata töötlemine sõnumid, kuna vanemate serdi andmeid võib siiski kanali vahemälu. Pärast seda, sõnumi töötlemine nurjub.  

**Lahendus**: värskendage serdi BizTalki teenuse Projecti ja Juurutage uuesti projekt.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Sillad jätkata sõnumite isegi siis, kui SQL-andmebaasi on võrguühenduseta
BizTalki teenuste sillad jätkata sõnumite samal ajal, isegi juhul, kui on Microsoft Azure'i SQL-andmebaasi (mis talletab esitatava teabe nagu juurutatud esemeid ja torujuhtmete), pole võrguühendust. Selle põhjuseks on BizTalki Services kasutab vahemällu talletatud artefakte ja bridge konfigureerimine.
Kui te ei soovi sillad töödelda kõiki sõnumeid, kui SQL-andmebaasi on ühenduseta, saate lõpetada või peatada BizTalki teenuse BizTalki teenuste PowerShelli cmdlet-käsud. Leiate [Azure'i BizTalki teenuse haldus valimi](http://go.microsoft.com/fwlink/p/?LinkID=329019) Windows PowerShelli cmdlet-käskude toimingute tegemiseks.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>XML-i sõnumi sees on bridge kohandatud koodi osa lugemise sisaldab mõnda eest komplekti märk
Kaaluge stsenaarium, kuhu soovite lugeda XML-i sõnumit mõnda bridge kohandatud koodi. Kui kasutate .net-i API System.Text.Encoding.UTF8.GetString(bytes) sõnumi alguses väljundisse kaasatud on saadaval täiendav komplekti märgi. Jah, kui soovite kaasata eest komplekti laadi väljundi, peate kasutama ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Sõnumite saatmine silda kasutades WCF-i skaala
Sõnumite saatmiseks silda kasutades WCF-i skaala. Peaksite kasutama HttpWebRequest scalable kliendi soovi.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>Uuele VERSIOONILE: Luba pakkuja tõrge pärast üleminekut opsüsteemilt BizTalki teenuste eelvaade, et General Availability (GA)
Aktiivsete töölehtede abil on EDI või AS2 leping. Kui BizTalki teenuse versiooniuuendust eelvaate-GA, võib ilmneda järgmine:
* Tõrge: Turbeloa pakkuja ei paku turvalisuse luba. Turbeloa pakkuja tagastatud teade: serveri nime ei saanud lahendada.

* Paketi tööülesanded on tühistatud.

**Lahendus**: pärast BizTalki teenuse värskendatakse, et General Availability (GA), Juurutage uuesti lepingu.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>VERSIOONITÄIENDUS: Tööriistakasti kuvatakse pärast üleminekut BizTalki teenuste SDK vana silla ikoonid
Pärast täiendamist varasemas versioonis BizTalki teenuste Tarkvaraarenduskomplektist, mis oli vana ikoonid, mis tähistab sillad, tööriistakasti jätkuvalt vana ikoonide sildade kuvamine. Kui lisate silla BizTalki teenuse project kujundaja surface, kuvatakse pind ikooni Uus.  

**Lahendus**. Saate töötada selle probleemi lahendamiseks kustutades .tbd nuppudelt jaotise <system drive>: \Users\<kasutaja > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>Uuele VERSIOONILE: BizTalki portaali update eelvaatest ga võidakse kuvada tõrketeate, mis ütleb, et EDI võimalus pole saadaval
Kui ajal BizTalki teenuste versiooniuuendust eelvaade ga, on BizTalki teenuste portaali sisse logitud, võidakse kuvada järgmine tõrketeade portaalis.  

See funktsioon pole saadaval selles väljaandes Microsoft Azure'i BizTalki teenuste osana. Kasutage neid võimalusi aktiveerige sobiv väljaanne.  

**Eraldusvõime**: portaali, Sule ja Ava brauseris ja seejärel logige sisse portaali Logi välja.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>Uuele VERSIOONILE: Uus jälgimise andmed ei kuvata pärast täiendamist ga BizTalki teenused
Kui teil on juurutatud BizTalki teenuste eelvaade tellimuse XML-i bridge stsenaarium endale. Silla sõnumeid saata ja vastav jälgimise andmed on saadaval BizTalki Services portaal. Nüüd, kui BizTalki Services portaal ja BizTalki teenuste käitusaja bittide on uuendatud ga ja sõnumi saatmiseks sama bridge lõpp-punkti juurutatud varasemas versioonis, jälgimise andmed ei kuvata pärast uuele versioonile üleminekut saadetud sõnumid.  

### <a name="pipelines-vs-bridges"></a>Torujuhtmete v/s sillad
Selles dokumendis kasutatakse vaheldumisi Termini "torujuhtmete" ja "sillad". Mõlemad tähendab põhiosas sama, mis on, sõnumi töötlemise üksus juurutatud BizTalki Services.  

### <a name="concepts"></a>Mõisted  

[BizTalki teenused](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
