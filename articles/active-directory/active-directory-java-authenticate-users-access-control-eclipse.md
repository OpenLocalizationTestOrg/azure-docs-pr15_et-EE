<properties
    pageTitle="Kuidas kasutada juurdepääsu reguleerimine (Java) | Microsoft Azure'i"
    description="Saate teada, kuidas arendamise ja kasutada Java Azure juurdepääsu reguleerimine."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Kuidas abil Eclipse teenusega Azure Access Web kasutajaid autentida

Sellest juhendist näitab teile, kuidas kasutada on Azure Access Control teenus (ACS) jooksul Azure'i tööriistakomplekt Eclipse. ACS kohta leiate lisateavet jaotisest [järgmised toimingud](#next_steps) .

> [AZURE.NOTE]
> Azure'i Access Services juhtelemendi filtri on ühenduse tehnoloogia eelvaade. Väljalaske-eelse tarkvara, kui see on ametlikult Microsoft ei toeta.

## <a name="what-is-acs"></a>Mis on ACS?

Enamik arendajad ei ole identiteedi ja üldiselt soovite veedavad aega arendamise autentimise ja luba menetlustele nende rakenduste ja teenuste jaoks. ACS on Azure'i teenus, mis on lihtne viis kinnitamise kasutajad, kellel on vaja juurdepääsu oma web rakenduste ja teenuste ilma vajaduseta tegur keerukate autentimise loogika lahkumata.

Järgmised funktsioonid on saadaval ACS:

-   Integreerimine Windows Identity Foundation (WIF).
-   Web populaarse identiteedipakkujad (IP-d) koos Windows Live ID-ga, Google, Yahoo! ja Facebooki tugi.
-   Tugi Active Directory Federation Services (ADFS) 2.0.
-   Open Data protokolli (OData)-põhiste halduse teenus, mis sisaldab Programmeerimisjuurdepääs ACS sätted.
-   Haldusportaali, mis võimaldab juurdepääs ACS sätted.

ACS kohta leiate lisateavet teemast [Accessi juhtelemendi teenuse 2.0][].

## <a name="concepts"></a>Mõisted

Azure ACS põhineb põhisumma taotlusepõhise identiteedi - ühtse lähenemisviisi loomiseks autentimist menetlustele rakenduste töötamine kohapealse või pilveteenuses. Taotlusepõhise identiteedi pakub rakenduste ja teenuste hankimine identiteedi teavet nad peavad kasutajad oma asutuses töötavate, teiste asutuste ja Internetis levinud viisi.

Sellest juhendist toimingu lõpuleviimiseks tuleks mõista järgmist põhimõtet:

**Kliendi** - kontekstis õpetused juhend, see on brauserit, mis proovib juurde pääseda oma veebirakenduse.

**Relying tootja (RP) rakenduse** - An RP rakendus on veebisaidi või teenus, mis tellib ühe välise asutusele autentimist. Identiteedi žargoon, öelda, et RP loodab asutuse. Sellest juhendist selgitatakse, kuidas konfigureerida rakenduse usalda ACS.

**Turbeloa** - märgiks on saidikogumi turvalisus andmeid, mis on tavaliselt välja alusel eduka kasutaja autentimine. See sisaldab *taotluste*atribuudid autenditud kasutaja. Nõude võib tähistada kasutaja nimi, identifikaatori rolli kasutaja kuulub, kasutaja vanus jne. Märgiks on tavaliselt digitaalselt allkirjastatud, mis tähendab, et see saab hangitud alati tagasi, et selle väljaandja, ja selle sisu ei saa rikutud. Kasutaja saab juurdepääsu RP rakenduse esitades kehtiv luba, mis RP rakenduse loodab asutuse antud.

**Identiteedi pakkuja IP** - An IP on asutuse autendib kasutaja identiteedi ja probleemid märkide turvalisus. Tegelik töö väljasta lube rakendatakse kuigi teisiti teenust nimega turvalisuse Turbeloa teenus (STS). Tüüpilised IP-aadressi kaasata Windows Live ID, Facebooki, business kasutaja hoidlate (nt Active Directory) jne.
ACS konfigureerimisel usaldusväärsuse IP süsteemi vastu ja kinnitage sõned, et IP välja. ACS usaldusväärsed mitu IP-d korraga, mis tähendab, et kui rakenduse loodab ACS, saate kohe pakkuda rakenduse kõik autenditud kasutajad kõik IP-d, et ACS loodab teie nimel.

**Federation pakkuja (Esiettekanne)** - IP-d on otsese teadmised kasutajad, ja nende mandaadi abil autentimiseks ja nõuete kohta, mida nad teada neid. Federation pakkuja (Esiettekanne) on teistsugune asutuse: selle asemel, et kasutajad otse autentimine, toimib vahendaja ja maaklerid autentimise vahel ühe RP ja üks või mitu IP-d. IP-d nii sekundis probleemi turvalisus sõned, seega mõlemad kasutada turvalisuse Turbeloa Services (STS). ACS on üks Esiettekanne.

**ACS reegli Engine** - sissetulevate usaldusväärsete IP-d abil märkide kaupa RP mõeldud märkide teisendamiseks kasutatava loogika on kodeeritud kujul lihtsa taotluste teisendus reeglid. ACS funktsioonid reegli engine, mis hoolitseb rakendamise mis tahes teisendus loogika oma RP määratud.

**ACS Namespace** - nimeruumi on ülataseme sektsioon ACS kasutatav korraldamine sätete kohta. Nimeruumi hoiab loendi IP-aadressi usaldate, RP rakendused, mida soovite kasutada, reegleid, et ootate reeglit töödelda sissetuleva sõned koos mootori jne. Nimeruumi seab erinevad lõpp-punktid, mis kasutab rakendus ja arendaja saada ACS tähistamise ülesannet.

Järgneval joonisel on kujutatud ACS autentimise toimimine veebirakendus:

![Vooskeemi ACS][acs_flow]

1.  RP taotleb kliendi (brauseris juhul) lehele.
2.  Kuna taotluse ei ole veel kinnitatud, RP suunab kasutaja asutusele, et ta loodab, mis on ACS. Nõuandekomisjone kujutab kasutaja valik selle RP määratud IP-aadressi. Kui kasutaja valib sobiva IP.
3.  Kliendi IP-aadressi autentimise lehele sirvib ja palub kasutajal sisse logida.
4.  Pärast kliendi autenditakse (nt mandaat on sisestatud ID), probleemid IP turvalisuse luba.
5.  Turvalisuse luba väljastamiseks IP suunab kliendi ACS ja klient saadab välja IP ACS Turbeloa.
6.  ACS kinnitatakse Turbeloa välja IP, sisendina ID väidab selle märgiks ACS reeglid mootori, arvutab väljundi identiteedi taotluste ja uue Turbeloa, mis sisaldab need väljundi taotluste probleemid.
7.  ACS suunab kliendi RP. Kliendi saadab uue Turbeloa RP ACS välja. RP kinnitatakse välja ACS Turbeloa allkirja, kinnitatakse selle märgiks nõuded ja tagastab leht, mis on algselt nõutud.

## <a name="prerequisites"></a>Eeltingimused

Sellest juhendist ülesannete tegemiseks on vaja järgmist:

- Mõne Java arendaja Kit (JDK), v 1,6 või uuem versioon.
- Eclipse IDE Java EE arendajatele Indigo või uuem versioon. See saab alla laadida <http://www.eclipse.org/downloads/>. 
- Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat, GlassFish, JBoss Application Server või Jetty jaotuse.
- Azure'i tellimusega alates <http://www.microsoft.com/windowsazure/offers/>omandanud.
- Azure'i tööriistakomplekt Eclipse, aprillis 2014 versioon või uuem versioon. Lisateabe saamiseks leiate [Azure'i tööriistakomplekt Eclipse installimist](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
- X.509 vastav sert oma rakendusega kasutada. Peate selle serdi nii avaliku sert (CER) ja isikliku teabe Exchange (. Vorming PFX). (Selle serdi loomise suvandid on kirjeldatud hiljem selles õpetuses).
- Tundmine on Azure Arvutage emulaator ja arutada [loomise Tere maailma taotluse Azure Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx)juurutamise meetodite abil.

## <a name="create-an-acs-namespace"></a>Mõne ACS Namespace loomine

Azure Access Control teenus (ACS) kasutamise alustamiseks peate looma ka ACS nimeruum. Nimeruumi pakub kordumatu ulatus ACS ressursse rakenduse tegelemiseks.

1. Logige [Azure haldusportaali][].
2. Klõpsake nuppu **Active Directory**. 
3. Luua uue juurdepääsu reguleerimine nimeruumi, nuppu **Uus**, klõpsake **Rakenduse teenused**, klõpsake **Juurdepääsu reguleerimine**, ja klõpsake **Kiiresti luua**. 
4. Sisestage nimeruumi nimi. Azure'i kinnitab, et nimi on kordumatu.
5. Valige soovitud piirkond, kus kasutatakse nimeruumi. Parima jõudluse tagamiseks kasutada piirkond, kus juurutate rakenduse.
6. Kui teil on mitu tellimust, valige tellimus, mida soovite kasutada ACS nimeruumi.
7. Klõpsake nuppu **Loo**.

Azure'i loob ja aktiveerib nimeruumi. Oodake, kuni uus nimeruumi olek on **aktiivne** enne jätkamist. 

## <a name="add-identity-providers"></a>Lisage identiteedipakkujad

Selles ülesandes lisamist IP-d autentimise oma RP rakendusega kasutada. Tutvustamise otstarbeks selle ülesande näitab, kuidas lisada Windows Live'i IP, kuid võite kasutada mis tahes loetletud ACS haldusportaal IP-aadressi.


1.  [Azure'i haldusportaal][]klõpsake **Active Directory**, valige soovitud juurdepääsu reguleerimine nimeruum ja seejärel klõpsake käsku **Halda**. Avaneb ACS haldusportaali.
2.  Klõpsake vasakpoolsel navigeerimispaanil ACS haldusportaali, **identiteedipakkujad**.
3.  Windows Live ID on vaikimisi sisse lülitatud ja ei saa kustutada. Selle õpetuse huvides kasutatakse ainult Windows Live ID-ga. Kuva, aga kui võite lisada muud IP-d, klõpsates nuppu **Lisa** .

Windows Live ID on nüüd lubatud IP oma ACS nimeruumi. Järgmiseks saate määrata oma Java veebirakenduse (hiljem loodud) RP nimega.

## <a name="add-a-relying-party-application"></a>Tuginedes tootja rakenduse lisamine

Selles ülesandes Konfigureerige ACS oma Java veebirakenduses kinnitamist RP äratuntavaks.

1.  Klõpsake haldusportaal ACS **Relying tootja rakenduses**.
2.  **Tuginedes rakenduste** lehel klõpsake nuppu **Lisa**.
3.  Lehe **Lisamine tuginedes tootja rakenduse** , tehke järgmist.
    1.  Tippige väljale **nimi**nimi RP. Tippige selle õpetuse huvides **Azure Web App**.
    2.  **Režiimis**, valige **sätete sisestamiseks käsitsi**.
    3.  Tippige **Domeen**, millele kehtib välja ACS Turbeloa URI. Tippige selle ülesande jaoks **http://localhost:8080 /**.
        ![Tuginedes tootja Domeen Arvuta emulaator kasutamiseks][relying_party_realm_emulator]
    4.  **Tagastab URL-i,** tippige URL-i, kus käib ACS Turbeloa. Tippige selle ülesande jaoks **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying tootja tagastada URL-i Arvuta emulaator kasutamiseks][relying_party_return_url_emulator]
    5.  Aktsepteerige vaikeväärtused ülejäänud väljad.

4.  Klõpsake nuppu **Salvesta**.

Nüüd konfigureeritud Java veebirakenduses Azure Arvuta emulaator käitamisel (veebisaidil http://localhost:8080 /) olevat oma ACS nimeruumi RP. Järgmisena looge reeglid, mis ACS kasutab RP nõuded.

## <a name="create-rules"></a>Reeglite loomine

Selles ülesandes määratleda reegleid, et juhtida, kuidas taotluste oma RP kaudu IP-d edasi. Käesolevas juhendis me lihtsalt konfigureerida ACS kopeerimiseks Sisestuskeel taotluste tüübid ja väärtused otse väljundi luba, filtreerimine ja nende muutmine ilma.

1.  Klõpsake haldusportaali ACS põhilehe **rühmad**.
2.  Klõpsake lehel **Rühmad** **Vaikimisi reegli rühma Azure Web App**.
3.  Klõpsake lehel **Reegel jaotises redigeerimine** nuppu **Loo**.
4.  Klõpsake soovitud **luua reeglid: vaikimisi reegli rühma Azure Web Appi** lehel, veenduge, et Windows Live ID on märgitud ja seejärel klõpsake nuppu **Loo**.    
5.  Lehel **Reegli rühma redigeerimine** , klõpsake nuppu **Salvesta**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Serdi üleslaadimine oma ACS nimeruum

Selles ülesandes üleslaadimist lisamine. PFX-sert, mida kasutatakse logige loodud oma ACS nimeruum Turbeloa taotlused.

1.  Klõpsake haldusportaali ACS põhilehe **serdid ja võtmed**.
2.  Klõpsake lehel **serdid ja võtmed** **Turbeloa sisselogimise**kohal **Lisa** .
3.  **Lisage Turbeloa allkirjasert või klahvi** lehel järgmist.
    1. Jaotises **kasutatud** nuppu **Tuginedes tootja rakendus** ja valige **Azure Web App** (mis varem seada oma tuginedes tootja rakenduse nimi).
    2. Valige jaotises **Tüüp** **x.509 vastav sert**.
    3. Jaotises **sert** , klõpsake nuppu Sirvi ja liikuge x.509 vastav sert fail, mida soovite kasutada. See on. PFX-fail. Valige fail, klõpsake nuppu **Ava**ja seejärel sisestage tekstiväljale **parool** serdi parool. Pange tähele, et katsetamiseks, võite kasutada enda-signed-sert. Iseallkirjastatud serdi loomine kasutada nupp **Uus** **ACS Filter teegi** dialoogiboksis (allpool kirjeldatud) või kasutage **encutil.exe** kasuliku [projekti veebisaidi][] Azure'i Starter komplekt Java.
    4. Veenduge, et **Teha esmane** on märgitud. Oma **lisada Turbeloa allkirjasert või klahvi** lehe peaks sarnanema järgmisega.
        ![Loaallkirjastamissert lisamine][add_token_signing_cert]
    5. Klõpsake sätete salvestamiseks ja sulgege **lisamine Turbeloa allkirjasert või klahvi** lehe **salvestamine** .

Järgmiseks rakenduste integreerimise lehel teave üle ja kopeerige, mida on vaja konfigureerida Java veebirakenduse kasutama ACS URI.

## <a name="review-the-application-integration-page"></a>Vaadake lehel rakenduste integreerimine

Leiate teavet ja koodi Java veebirakenduse (RP rakendus) töötamiseks ACS ACS haldusportaali lehel rakenduste integreerimise konfigureerimiseks vajalikku. See teave on vaja oma väline autentimist Java veebirakenduse konfigureerimisel.

1.  Klõpsake haldusportaal ACS **rakenduste integreerimine**.  
2.  Klõpsake lehel **Rakenduste integreerimise** **Login lehed**.
3.  **Sisselogimise lehe integreerimine** lehel klõpsake **Azure Web App**.

Rakenduses on **sisselogimise lehe integreerimine: Azure'i Web Appi** lehel, URL-i loetletud **suvand 1: ACS majutatud sisselogimislehe Link** kasutatakse teie Java veebirakenduse. Azure'i juurdepääsu juhtimine teenuste Filter teeki lisamisel Java rakenduse, peate seda väärtust.

## <a name="create-a-java-web-application"></a>Java veebirakenduse loomine
1. Jooksul Eclipse, menüüs klõpsake menüüd **fail**, nuppu **Uus**ja klõpsake **Dünaamiline Web projekti**. (Kui te ei näe **Dünaamiline Web projekti** loendis Saadaolevad projekti pärast menüü **fail**käsku **Uus**, siis tehke järgmist: klõpsake menüüd **fail**, nuppu **Uus**, valige **Project**, laiendage **Web**, valige **Dünaamiline Web Project**ja klõpsake nuppu **edasi**.) Selle õpetuse huvides nime projekti **MyACSHelloWorld**. (Tagada selle nime kasutamiseks, edaspidised juhised selles õpetuses oodata sõda faili oma nime MyACSHelloWorld). Ekraanil kuvatakse järgmine:

    ![Tere, maailm projekti jaoks ACS exampple loomine][create_acs_hello_world]

    Klõpsake nuppu **valmis**.
2. Eclipse's Project Exploreri vaates, laiendage **MyACSHelloWorld**. Paremklõpsake **WebContent**, nuppu **Uus**ja seejärel valige **JSP fail**.
3. Dialoogiboksis **Uue JSP faili** nimi faili **index.jsp**. Säilita emakausta nimega MyACSHelloWorld/WebContent, nagu on näidatud järgmist:

    ![Näiteks ACS JSP faili lisamine][add_jsp_file_acs]

    Klõpsake nuppu **edasi**.

4. **Valige JSP malli** dialoogiboksis Vali **Uus JSP fail (html)** ja klõpsake nuppu **valmis**.
5. Index.jsp faili avamisel Eclipse lisamine kuvatav tekst **Tere ACS maailm!** jäävad `<body>` element. Teie värskendatud `<body>` sisu peaks olema järgmine:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Salvestage index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Rakenduse ACS Filter teegi lisamine

1. Eclipse's Project Explorer, paremklõpsake **MyACSHelloWorld**, nuppu **Tee koostamine**ja klõpsake **Konfigureerimine koostada tee**.
2. **Java koostada tee** dialoogiboksis vahekaarti **teekide** .
3. Klõpsake **teeki lisada**.
4. Klõpsake **Azure juurdepääsu juhtimine teenuste Filtreeri (MS avatud tehnoloogia)** ja seejärel klõpsake nuppu **edasi**. Kuvatakse dialoogiboks **Azure juurdepääsu juhtimine teenuste Filtreeri** .  (Väli **asukoht** võib olla mõni muu tee, sõltuvalt sellest, kuhu installisite Eclipse, ja versiooninumber võib olla erinev, olenevalt tarkvaravärskendusi.)

    ![ACS Filter teegi lisamine][add_acs_filter_lib]

5. Brauseris avatud lehele **Sisselogimise lehe integreerimine** haldusportaali, kopeerige URL loendis soovitud **suvand 1: ACS majutatud sisselogimislehe Link** välja ja kleepige see dialoogiboksi Eclipse **ACS autentimise lõpp-punkti** välja.
6. Brauseris avatud halduse portaali lehele **Redigeerimine tuginedes tootja rakendus** , kopeerige URL-i **Domeen** väljal loetletud ja kleepige see Eclipse dialoogiboksi väli **Tuginedes tootja Domeen** .
7. **Turvalisus** jaotises Eclipse dialoog, kui soovite kasutada lüüsiarvutis olemasolev sert, klõpsake nuppu **Sirvi**, liikuge sert, mida soovite kasutada, valige see ja klõpsake nuppu **Ava**. Või kui soovite luua uut serti, klõpsake nuppu **Uus** , et kuvada dialoogiboks **Uue sert** , siis määrake parool, CER-faili nimi ja uut serti pfx-faili nimi.
8. Märkige ruut **Manusta serdi sõda faili**. Manustatud serdi sel viisil kaasanud selle juurutamise nõudmata, saate selle käsitsi lisada osana. (Kui selle asemel peab salvestate oma serdi väliselt sõda failist, võite serdi lisamine rolli osana ning tühjendage **serdi sõda faili manustamine**.)
9. [Valikulised] Säilita **nõua HTTPS ühendusi** märgitud. Kui seate selle suvandi, peate teie taotlus HTTPS-protokolli abil. Kui te ei soovi nõudma HTTPS ühendusi, tühjendage see suvand.
10. Arvuta emulaator juurutamiseks **Azure ACS** filtrisätted näeb umbes järgmist.

    ![Azure ACS filtrisätted Arvuta emulaator juurutamine][add_acs_filter_lib_emulator]

11. Klõpsake nuppu **valmis**.
12. Klõpsake nuppu **Jah** on dialoogiboksis ruut märgitud web.xml faili luuakse esitamisel.
13. Klõpsake nuppu **OK** , et sulgeda dialoogiboksi **Java koostada tee** .

## <a name="deploy-to-the-compute-emulator"></a>Arvuta emulaator juurutamine

1. Eclipse's Project Explorer, paremklõpsake **MyACSHelloWorld**, klõpsake **Azure**ja klõpsake **paketi Azure**.
2. **Projekti nime**, tippige **MyAzureACSProject** ja klõpsake nuppu **edasi**.
3. Valige soovitud JDK ja rakenduse serveris. (Järgmist asuvad üksikasjalikult [loomise Tere maailma taotluse Azure Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) õpetuses).
4. Klõpsake nuppu **valmis**.
5. Klõpsake nuppu **Käivita Azure emulaator** .
6. Kui teie Java veebirakenduse hakkavad Arvuta emulaator, sulgege kõik eksemplarid brauseri (nii, et kõik praeguse brauseri seansid ei häiriks teie ACS sisselogimise test).
7. Rakenduse käivitada, avades <> http://localhost:8080/MyACSHelloWorld/oma brauserit ( <> **või/https://localhost:8080/MyACSHelloWorld/nõua HTTPS ühendusi**märkimisel). Peaks teilt sisselogimise Windows Live'i ID ja seejärel tuleb saatja määratud tuginedes tootja rakenduse URL.
99.  Kui olete rakenduse vaatamise lõpetanud, klõpsake nuppu **Lähtestamine Azure emulaator** .

## <a name="deploy-to-azure"></a>Azure'i juurutamine

Azure'i juurutamiseks peate muuta tuginedes tootja domeen ja URL-i oma ACS nimeruumi naasmiseks.

1. Azure'i haldusportaal lehel **Redigeeri tuginedes tootja rakenduse** sees muuta olema juurutatud saidi URL-i **Domeen** . Asendage **näide** teie määratud juurutamiseks oma DNS-i nimi.

    ![Tuginedes tootja Domeen kasutamiseks töökeskkonnas][relying_party_realm_production]

2. **Tagastab URL-i** olevat URL-i rakenduse muuta. Asendage **näide** teie määratud juurutamiseks oma DNS-i nimi.

    ![Tuginedes tootja saatja URL-i kasutamiseks töökeskkonnas][relying_party_return_url_production]

3. Klõpsake nuppu **Salvesta** salvestada oma tootja värskendatud vastamine domeen ja URL-i muudatused tagasi.
4. Hoidke **Sisselogimise lehe integreerimine** leht, mis on brauseris avatud, peate kopeerige varsti.
5. Eclipse's Project Explorer, paremklõpsake **MyACSHelloWorld**, nuppu **Tee koostamine**ja klõpsake **Konfigureerimine koostada tee**.
6. Klõpsake vahekaarti **teekide** , klõpsake **Azure juurdepääsu juhtimine teenuste Filter**ja seejärel klõpsake nuppu **Redigeeri**.
7. Brauseris avatud lehele **Sisselogimise lehe integreerimine** haldusportaali, kopeerige URL loendis soovitud **suvand 1: ACS majutatud sisselogimislehe Link** välja ja kleepige see dialoogiboksi Eclipse **ACS autentimise lõpp-punkti** välja.
8. Brauseris avatud halduse portaali lehele **Redigeerimine tuginedes tootja rakendus** , kopeerige URL-i **Domeen** väljal loetletud ja kleepige see Eclipse dialoogiboksi väli **Tuginedes tootja Domeen** .
9. **Turvalisus** jaotises Eclipse dialoog, kui soovite kasutada lüüsiarvutis olemasolev sert, klõpsake nuppu **Sirvi**, liikuge sert, mida soovite kasutada, valige see ja klõpsake nuppu **Ava**. Või kui soovite luua uut serti, klõpsake nuppu **Uus** , et kuvada dialoogiboks **Uue sert** , siis määrake parool, CER-faili nimi ja uut serti pfx-faili nimi.
10. Säilita **serdi sõda faili manustamine** märgitud, eeldades, et soovite serdi sõda faili manustada.
11. [Valikulised] Säilita **nõua HTTPS ühendusi** märgitud. Kui seate selle suvandi, peate teie taotlus HTTPS-protokolli abil. Kui te ei soovi nõudma HTTPS ühendusi, tühjendage see suvand.
12. Azure'i juurutamiseks Azure ACS filtrisätted näeb umbes järgmist.

    ![Azure ACS filtrisätted tootmise juurutamine][add_acs_filter_lib_production]

13. Klõpsake nuppu **valmis** **Redigeerimine teegis** dialoogiboksi sulgemiseks.
14. Klõpsake nuppu **OK** , et sulgeda **MyACSHelloWorld atribuutide** dialoogiboksis.
15. Eclipse, klõpsake nuppu **Avalda Azure'i pilveteenusesse** . Vastata viipasid, sarnaselt [loomise Tere maailma taotluse Azure Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) teema jaotises **rakenduse Azure juurutamiseks** lõpuleviiduks. 

Pärast kasutusele võetud oma veebirakenduse, sulgege kõik avatud brauseri seansid, käivitage veebirakendus ja teil palutakse logige Windows Live'i ID mandaat, saadetakse tuginedes tootja rakenduse saatja URL, millele järgneb.

Kui olete valmis oma ACS Tere, maailm rakendust pidage meeles, et kustutada juurutamise (saate teada, kuidas kustutada [loomise Tere maailma taotluse Azure Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) teemas juurutamine).


## <a name="next_steps"></a>Järgmised sammud

Läbivaatusele, on turvalisuse väide Markup Language (SAML) tagastatud ACS rakenduse, vaadake, [Kuidas vaadata SAML tagastatud Azure'i juurdepääsu juhtimine teenuse][]. Veelgi uurimine ACS's funktsioonid ja eksperimenteerida keerukamaid stsenaariumid, lugege teemat [Access juhtelemendi teenuse 2.0][].

Selles näites kasutatakse ka **serdi sõda faili** suvandi. See suvand on lihtne juurutada sert. Kui soovite selle asemel hoida oma Allkirjaserdi eraldi sõda faili, saate järgmine meetod:

1. Tippige dialoogiboksi **Azure'i juurdepääsu juhtimine filtrisse** jaotis **Turve** sees **${keskkonna. JAVA_HOME}/MyCERT.CER** ja tühjendage ruut **serdi sõda faili manustamine**. (Reguleerida mycert.cer, kui teie serdi faili nimi erineb.) Klõpsake dialoogiboksi sulgemiseks **valmis** .
2. Kopeerige serdi juurutamise osa: rakenduses Eclipse Project Explorer laiendamine **MyAzureACSProject**, paremklõpsake **WorkerRole1**, klõpsake käsku **Atribuudid**, laiendage **Azure'i rolli**, ja klõpsake **komponendid**.
3. Klõpsake nuppu **Lisa**.
4. Jooksul **Komponent lisamine** dialoog:
    1. Jaotises **impordi** :
        1. **Faili** nupu abil saate liikuda sert, mida soovite kasutada. 
        2. Valige **Kopeeri** **meetod**.
    2. **Kui nime**, klõpsake tekstivälja ja jätta vaikenime.
    3. **Deploy** jaotis.
        1. Valige **Kopeeri** **meetod**.
        2. **Kataloogi**, tippige **JAVA_HOME %**.
    4. Teie **Komponendi lisamine** dialoogiboksi peaks sarnanema järgmisega.

        ![Lisage serdi komponent][add_cert_component]

    5. Klõpsake nuppu **OK**.

Selles etapis sisaldaks juurutamise teie sert. Pange tähele, et sõltumata sellest, kas serdi sõda faili manustada või lisada selle oma juurutuse komponent, peate oma nimeruumi, nagu on kirjeldatud jaotises [laadige üles sert oma ACS nimeruumi][] serdi üleslaadimine.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Serdi üleslaadimine oma ACS nimeruum]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[Projekti veebileht]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Kuidas vaadata SAML tagastatud Azure'i juurdepääsu juhtimine teenus]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Juurdepääsu juhtimine teenuse 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure'i haldusportaal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
