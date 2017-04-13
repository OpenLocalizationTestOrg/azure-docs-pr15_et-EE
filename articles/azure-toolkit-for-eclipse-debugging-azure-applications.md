<properties
    pageTitle="Azure'i rakenduste Eclipse silumine"
    description="Siit saate teada, silumine Azure'i rakenduste Eclipse Azure'i tööriistakomplekt kasutamise kohta."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Azure'i rakenduste Eclipse silumine #

Azure'i tööriistakomplekt Eclipse kasutamisel saate silumine rakenduste kas need töötavad Azure või Arvuta emulaator kui kasutate opsüsteemi Windows. Järgmisel pildil on kujutatud **silumine** atribuutide dialoogiboks kasutada remote silumine:

![][ic719504]

Selle õpetuse eeldab, et teil on juba rakendus, mis on loodud ja on tuttav Arvuta emulaator ja juurutamisel Azure.

Kasutame rakenduse [abil Azure'i teenus käitusaja teek JSP][] õpetuse alguspunktiks selle teema. Enne jätkamist luua selle rakenduse, kui te pole seda juba teinud.

## <a name="to-debug-your-application-while-running-in-azure"></a>Kui soovite rakenduse Azure käitamise ajal silumine ##

>[AZURE.WARNING] Praegune olevat tööriistakomplekti tugi remote Java silumine on mõeldud eelkõige juurutuste Azure Arvuta emulaator töötab. Kuna silumine ühendus pole turvaline, mitte lubage Kaug silumine tootmise juurutuste. Kui teil on vaja silumine rakendus töötab Azure'i pilves, kasutada lavastus juurutus, kuid mõistan, et isegi juhul, kui see on lavastus juurutamise on võimalik, kui keegi teab pilve juurutamise IP-aadress seansi silumine volitamata juurdepääsu.

1. Projekti testimiseks emulaator koostamine: rakenduses Eclipse Project Explorer, paremklõpsake **MyAzureProject**, klõpsake käsku **Atribuudid**, klõpsake **Azure**ja seatud **koostamine** **juurutamise Cloud**.
1. Taastada oma projekti: Eclipse klõpsake menüü **Projekt**, klõpsake käsku **Koostada kõik**.
1. Rakenduse *lavastus* Azure juurutamine.
    >[AZURE.IMPORTANT] Nagu eespool, see on soovitatav Arvuta emulaator enamasti silumine ja seejärel silumine lavastus keskkonnas ainult siis, kui täiendavad silumine on vajalik. Soovitatav on mitte silumine tootmiskeskkonda.
1. Kui teie juurutusega on valmis Azure, hangib [Azure'i haldusportaal][]juurutamise DNS-i nimi. Lavastus juurutamise on DNS-i nimi kujul http://*&lt;guid&gt;*. cloudapp.net, kus * &lt;guid&gt; * on määratud Azure'i GUID väärtus.
1. Eclipse's Project Explorer, paremklõpsake **WorkerRole1**, klõpsake **Azure**ja klõpsake **silumine**.
1. **WorkerRole1 silumine atribuutide** dialoogiboksis:
    1. Märkige ruut **Luba sellel rollil Kaug silumine.**
    1. **Sisestusmeetodi lõpp-punkti kasutada**, kasutage **silumine (avalik: 8090, Privaatne: 8090)**.
    1. Veenduge, et **Alustada töötab peatatud režiimis, siluri ühenduse ootamine** on märkimata.
        >[AZURE.IMPORTANT] Suvandi **Siluri ühenduse ootamine peatatud režiimis töötab käivitamine** on mõeldud Täpsemad silumine Arvuta emulaator ainult stsenaariumid (pole puhul pilveteenuste juurutuste). **Alustada töötab peatatud režiimis, siluri ühenduse ootamine** suvand kasutamisel peatab serveri käivitamisel protsessi kuni Eclipse Silur on ühendatud selle töötab. Kuigi võite selle suvandi abil Arvuta emulaator silumine seansi jaoks, kasutage seda silumine seansi cloud juurutamine. Mõne serveri lähtestamine toimub toimingu Azure käivitus ja Azure pilveteenuse ei võimalda avaliku lõpp-punktid kuni käivitus tööülesanne on lõpule viidud. Seega käivitus protsess on lõpule, kui see on lubatud pilvepõhise juurutuse, kuna see ei saa ühenduse välise Eclipse kliendilt vastu.
1. Klõpsake nuppu **Loo silumine konfiguratsioone**.
1. **Azure'i silumine konfiguratsiooni** dialoogi:
    1. **Java projekti silumine**, valige **MyHelloWorld** projekt.
    1. **Konfigureerimine silumine**, märkige ruut **Azure pilveteenuse (lavastus)**.
    1. Veenduge, et **arvutada Azure emulaator** on märkimata.
    1. **Host**, sisestage etapiviisilise juurutamise, kuid ilma eelneva **http://**DNS-i nimi. Näiteks (kasutada oma GUID asemel joonisel GUID): **4e616d65-6f6e-6 d 65-6973-526f62657274.cloudapp.net**
1. Klõpsake nuppu **OK** , et sulgeda **Azure'i silumine konfiguratsiooni** dialoogi.
1. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Atribuudid WorkerRole1 silumine** .
1. Kui teil ei ole juba seadmine index.jsp katkestuspunkti, määrake see:
    1. Eclipse's Project Exploreri, laiendada **MyHelloWorld**, laiendage **WebContent**ja topeltklõpsake **index.jsp**.
    1. Jooksul index.jsp, paremklõpsake sinise riba vasakule Java-koodi ja nuppu **Lülita katkestuspunktid**, nagu on näidatud järgmist: ![][ic551537]
1. Eclipse's menüü, klõpsake nuppu **Käivita** ja seejärel käsku **Silumine konfiguratsioone**.
1. Dialoogiboksis **Silumine konfiguratsioone** laiendage vasakpoolsel paanil **Java-rakendus** , valige **Azure pilveteenuse (WorkerRole1)**, ja klõpsake **silumine**.
1. Käivitage oma brauseris etapiviisilise rakenduse, **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, asendades GUID kaudu oma DNS-i nimi * &lt;guid&gt;*. **Kinnitage perspektiivi vahetamise** dialoogiboksi küsimisel nuppu **Jah**. Seansi silumine peaks nüüd käivitada kood, kus katkestuspunkt on seatud rida.

>[AZURE.NOTE] Kui proovite alustada Interneti silumine ühenduse juurutus, mis sisaldab rolli mitmes eksemplaris töötab, ei saa praegu kontrollida milline siluri algselt ühendatakse, mida Azure laadi koormusetasakaalustusteenuse valida näiteks juhusliku. Kui olete seotud selle eksemplari, kuigi, saate jätkata, silumine samas eksemplaris. Märkme, kui seal on rohkem kui 4 minutit (näiteks siis, kui olete lõpetanud, veebisaidil katkestuspunkti liiga kauaks) tegevusetuse perioodi, Azure'i võib sulgeda ka ühendus.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Silumine kindla rolliga eksemplari mitme eksemplari juurutamine ##

Kui juurutamise töötab pilves, siis tõenäoliselt töötab see mitme Arvuta, või roll, eksemplarid. See võimaldab teil ära Azure'i 99,95%-saadavus garantii ja välja rakenduse skaala.

Sellisel juhul peate kaugühenduse teel silumine Java rakenduse kindla rolliga eksemplari. Siiski ainult tavaline Sisestuskeel endpoint silumine lubamisel Azure laadi koormusetasakaalustusteenuse teeb peaaegu ei võimalda ühenduse siluri kindla rolliga eksemplari. Selle asemel see loob ühenduse rolli eksemplari, mis seda juhusliku.

See on stsenaarium, kus ära eksemplari Sisestuskeel lõpp-punktid hõlbustab teil silumine kindla rolliga eksemplari tüüp.

Oletame, et kavatsete kasutada kuni 5 rolli eksemplari juurutamise. Kasutamisel rolli atribuutide dialoogiboksis **lõpp-punktid** atribuudileht eksemplari Sisestuskeel lõpp-punkti loomine ja selle määramine mitmesuguseid avaliku pordid, mitte ühe pordi number. Näiteks Määrake väljal **avaliku pordi** Sisestuskeel **81-85**.

Pärast juurutamist rakenduse abil see eksemplar endpoint, määrata Azure'i iga rolli aknad selles vahemikus unikaalne pordinumber. Seejärel, et teada saada, milline sai määratud mis pordi number, saate *InstanceEndpointName***_PUBLICPORT** keskkonna muutuja (kus *InstanceEndpointName* on teile määratud, kui olete loonud eksemplari lõpp-punkti nimi) automaatselt konfigureeritud tööriistakomplekti (nt tagastades väärtusega veebilehel jalus nii, et saaks seda lugeda, selle sirvimisel) juurutamise teel.

Kui teate, millised avaliku pordi number selle eksemplari on määratud, viitamiseks seda oma silumine konfiguratsiooni Eclipse, järgi lisamine oma teenuse hosti nimi. See võimaldab Eclipse siluri ühenduse selle kindla eksemplari, ja mitte mõnda muud eksemplarid.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Ainult Windows: silumine Arvuta emulaator käitamise ajal rakenduse abil ##

>[AZURE.NOTE] Azure'i emulaator on saadaval ainult Windows. Kui kasutate operatsioonisüsteemi Windows peale selle jaotise vahele jätta. 

1. Projekti testimiseks emulaator koostamine: rakenduses Eclipse Project Exploreri, paremklõpsake **MyAzureProject**, klõpsake käsku **Atribuudid**, klõpsake **Azure**ja seatud **testimine emulaator** **koostamine** .
1. Taastada oma projekti: Eclipse klõpsake menüü **Projekt**, klõpsake käsku **Luua kõik**.
1. Eclipse's Project Explorer, paremklõpsake **WorkerRole1**, klõpsake **Azure**ja klõpsake **silumine**.
1. **WorkerRole1 silumine atribuutide** dialoogiboksis:
    1. Märkige ruut **Luba sellel rollil Kaug silumine.**
    1. **Sisestusmeetodi lõpp-punkti kasutada**, kasutage vaikimisi lõpp-punkti automaatselt genereeritud tööriistakomplekt, mis on loetletud **silumine (avalik: 8090, Privaatne: 8090)**.
    1. Veenduge, et **Alustada töötab peatatud režiimis, siluri ühenduse ootamine** suvand on märkimata.
        >[AZURE.IMPORTANT] Suvandi **Siluri ühenduse ootamine peatatud režiimis töötab käivitamine** on mõeldud Täpsemad silumine Arvuta emulaator ainult stsenaariumid (pole puhul pilveteenuste juurutuste). **Alustada töötab peatatud režiimis, siluri ühenduse ootamine** suvand kasutamisel peatab serveri käivitamisel protsessi kuni Eclipse Silur on ühendatud selle töötab. Kuigi võite selle suvandi abil Arvuta emulaator silumine seansi jaoks, kasutage seda silumine seansi cloud juurutamine. Mõne serveri lähtestamine toimub toimingu Azure käivitus ja Azure pilveteenuse ei võimalda avaliku lõpp-punktid kuni käivitus tööülesanne on lõpule viidud. Seega käivitus protsess on lõpule, kui see on lubatud pilvepõhise juurutuse, kuna see ei saa ühenduse välise Eclipse kliendilt vastu.
1. Klõpsake nuppu **Loo silumine konfiguratsioone**.
1. **Azure'i silumine konfiguratsiooni** dialoogi:
    1. **Java projekti silumine**, valige **MyHelloWorld** projekt.
    1. **Konfigureerimine silumine**, märkige ruut **Arvuta Azure emulaator**.
1. Klõpsake nuppu **OK** , et sulgeda **Azure'i silumine konfiguratsiooni** dialoogi.
1. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Atribuudid WorkerRole1 silumine** .
1. Seadke katkestuspunkti index.jsp:
    1. Eclipse's Project Exploreri, laiendada **MyHelloWorld**, laiendage **WebContent**ja topeltklõpsake **index.jsp**.
    1. Jooksul index.jsp, paremklõpsake sinise riba vasakule Java-koodi ja nuppu **Lülita katkestuspunktid**, nagu on näidatud järgmist: ![][ic551537]

       Katkestuspunkti määratakse, kui näete oma Java kood vasakul katkestuspunkt ikooni maksimaalselt sinisel ribal.
1. Käivitage rakendus Arvuta emulaator Azure tööriistaribal nuppu **Käivita Azure emulaator** .
1. Eclipse's menüü, klõpsake nuppu **Käivita** ja seejärel käsku **Silumine konfiguratsioone**.
1. Dialoogiboksis **Silumine konfiguratsioone** laiendage vasakpoolsel paanil **Java-rakendus** , valige **Azure emulaator (WorkerRole1)**, ja klõpsake **silumine**.
1. Pärast Arvuta emulaator näitab, et teie rakendus töötab brauseri, käivitage **http://localhost:8080/MyHelloWorld**.
    **Kinnitage perspektiivi vahetamise** dialoogiboksi küsimisel nuppu **Jah**.
    Seansi silumine peaks nüüd käivitada kood, kus katkestuspunkt on seatud rida.

See ilmnes saate kohta silumine Arvuta emulaator; Järgmises jaotises kirjeldatakse, kuidas silumine rakenduse Azure'i juurutatud.

## <a name="debugging-notes"></a>Märkmete silumine ##

* Pärast silumine, mida saab vahetada perspektiivi kaudu **silumine** **Java** kaudu, klõpsates menüü Eclipse's klõpsake **aknas** **Avatud**, ja valige perspektiivi, mida soovite kasutada.
* Remote silumine GlassFish lubamiseks Ärge kasutage Kaug silumine konfigureerimise funktsiooni Azure'i tööriistakomplekt Eclipse. GlassFish hoopis käsitsi konfigureerida. GlassFish käsitleb Java suvandid eelnevalt keskkonna muutujate sellepärast olevat tööriistakomplekti Kaug silumine konfigureerimise funktsiooni ei tööta õigesti GlassFish. Kui soovitud tööriistakomplekt Kaug silumine konfigureerimise funktsiooni jaoks on lubatud, see võib takistada GlassFish.

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i haldusportaal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546
[Azure'i teenus käitusaja teegi JSP abil]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
