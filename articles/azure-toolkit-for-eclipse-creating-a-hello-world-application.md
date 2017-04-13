<properties
    pageTitle="Tere tulemast maailma pilveteenuses Azure'i Eclipse loomine"
    description="Saate teada, kuidas luua lihtsa Tere, maailm rakenduse kasutamise Eclipse Azure'i tööriistakomplekt."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Tere tulemast maailma pilveteenuses Azure'i Eclipse loomine #

Järgmised toimingud näitavad, kuidas luua ja juurutada JSP taotluse Azure'i Azure tööriistakomplekt kasutamise Eclipse. JSP näide on esitatud lihtne, kuid väga sarnane juhiseid oleks sobilikud Java servlet, kuna Azure juurutamise on.

Rakenduse näeb välja umbes järgmine:

![][ic600360]

## <a name="prerequisites"></a>Eeltingimused ##

* Mõne Java arendaja Kit (JDK), v 1.7 või uuem versioon.
* Eclipse IDE Java EE arendajatele Indigo või uuem versioon. See saab alla laadida <http://www.eclipse.org/downloads/>.
* Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat, GlassFish, JBoss Application Server, Jetty või IBM® WebSphere® rakenduse Server Liberty Core jaotuse.
* Azure'i tellimusega alates <http://azure.microsoft.com/pricing/purchase-options/>omandanud.
* Azure'i tööriistakomplekt Eclipse. Lisateabe saamiseks leiate [Azure'i tööriistakomplekt Eclipse installimist][].

## <a name="to-create-a-hello-world-application"></a>Tere, maailm rakenduse loomine ##

Kõigepealt tuleb Alustame Java projekti loomine.

*  Käivitage Eclipse, ja klõpsake menüü **fail**, nuppu **Uus**ja klõpsake **Dünaamiline Web projekti**. (Kui te ei näe **Dünaamiline Web projekti** loendis Saadaolevad projekti pärast menüü **fail**käsku **Uus**, siis tehke järgmist: klõpsake menüüd **fail**, nuppu **Uus**, valige **Project...**, laiendage **Web**, valige **Dünaamiline Web Project**ja klõpsake nuppu **edasi**.)
*  Selle õpetuse huvides nime projekti **MyHelloWorld**. (Tagada selle nime kasutamiseks, edaspidised juhised selles õpetuses oodata sõda faili oma nime MyHelloWorld). Ekraanil kuvatakse järgmine: ![][ic589576]
* Klõpsake nuppu **valmis**.
* Eclipse's Project Exploreri vaates, laiendage **MyHelloWorld**. Paremklõpsake **WebContent**, nuppu **Uus**ja seejärel valige **JSP fail**.
* Dialoogiboksis **Uue JSP faili** nimi faili **index.jsp**. Säilita emakausta nimega **MyHelloWorld/WebContent**, nagu on näidatud järgmist:  ![][ic659262]
* Dialoogiboksis **Valige JSP malli** selles õpetuses eesmärgil Vali **Uus JSP fail (html)** ja klõpsake nuppu **valmis**.
* Index.jsp faili avamisel Eclipse lisamine dünaamiliselt kuvatav tekst **Tere, maailm!** jäävad `<body>` element. Teie värskendatud `<body>` sisu peaks olema järgmine:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Salvestage index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Kiire ja lihtne viis rakenduse Azure, juurutamiseks ##

Niipea, kui olete valmis, et testida Java veebirakenduse, saate järgmist otseteed proovima otse sisse Azure pilve.

1. Klõpsake Eclipse's Project Explorer **MyHelloWorld**.
1. Eclipse tööriistaribal klõpsake nuppu **Avalda** rippmenüü ja klõpsake **Avaldamine nimega Azure'i pilveteenuses**
    ![][publishDropdownButton]
1. Kui avaldate selle rakenduse Azure esimest korda ja te pole loonud on Azure juurutamise projekt enne selle rakenduse, Azure juurutamise projekt, mis luuakse teie jaoks automaatselt. Peaksite nägema järgmine viip, mis sisaldab ka JDK paketi ja rakenduse server, mis kuvatakse automaatselt juurutatud rakenduse käivitada.
    ![][ic789598]

    Seda moodust otsetee võimaldab kiire ja lihtne viis rakenduse testi Azure ilma kindla serveri või JDK, mis erineb vaikesätete konfigureerimine. Kui olete rahul vaikeväärtused, võite klõpsata **OK** jätkamiseks tehke järgmist.
    Juhul, kui soovite muuta selle JDK või rakendus serveri kasutada rakenduse, saate seda teha hiljem, redigeerides Azure juurutamise projekti teie jaoks automaatselt loodud või nuppu **Loobu** kohe ja lugege selles õpetuses **kohta Azure juurutamise projektid jaotises** .
1. **Azure'i avalda** dialoogiboksis:
    1. Kui on pole tellimuste korral valige loendis **tellimuse** veel, järgige neid juhiseid oma tellimuse teabe importimine.
        1. Klõpsake nuppu **Impordi failist AVALDA-sätted**.
        1. Klõpsake dialoogiboksis **Impordi tellimuse teave** **AVALDA-sätted faili alla laadida**. Kui te pole veel Azure'i kontosse sisse logitud, palutakse teil sisse logida. Seejärel palutakse Salvesta on Azure avaldada sätete fail. Salvestage see oma arvutisse.
        1. Endiselt **Tellimuse teabe** dialoogis, klõpsake nuppu **Sirvi** , valige avalda sätete fail, mis on salvestatud kohalikult eelmises etapis ja seejärel klõpsake nuppu **Ava**. Kuva peaks välja nägema umbes järgmine: ![][ic644267]
        1. Klõpsake nuppu **OK**.
    1. **Tellimuse**korral valige tellimus, mida soovite kasutada oma juurutamiseks.
    1. **Salvestusruumi konto**, valige salvestusruumi konto, mida soovite kasutada, või nuppu **Uus** salvestusruumi uue konto loomiseks.
    1. **Teenuse nimi**, valige pilveteenuses, mida soovite kasutada või klõpsake nuppu **Uus** , et luua uue pilveteenuses.
    1. **Target OS**, valige operatsioonisüsteem, mida soovite kasutada oma juurutamiseks versiooni.
    1. **Target keskkonnas**, valige selles õpetuses eesmärgil **lavastus**. (Kui olete valmis oma tootmiskohast juurutada, saate seda muuta **tootmisele**.)
    1. Valikuline: Veenduge, et **kirjutada eelmise juurutamise** on märgitud, kui soovite oma uue juurutuse ülekirjutamiseks automaatselt eelmise juurutamise. Kui selle suvandi lubamiseks saate küll ei kogemus "409 konflikt" probleemid avaldamise samasse asukohta.
        Pange tähele, et dialoogiboksi **Avalda Azure** sisaldab jaotise **Kaugpöördusteenuse**. Vaikimisi Kaugpöördusteenuse pole lubatud ja me ei luba see selle näite puhul. Remote juurdepääsu lubamine oleks sisestage kasutajanimi ja parool kaugühenduse teel logimine. Kaugpöördusteenuse kohta leiate lisateavet teemast [Lubamine Kaugpöördusteenuse Azure'i juurutuste Eclipse sisse][].
        Teie **Avalda Azure** dialoogiboks kuvatakse järgmine: ![][ic719488]
1. Klõpsake nuppu **Avalda** avaldamiseks lavastus keskkonnas.
    Kui teil palutakse seda teha täielik koostamine, klõpsake nuppu **Jah**. See võib kuluda mitu minutit esimese koostamine.
    On **Azure tegevuse Log** kuvatakse teie Eclipse vahekaartidega jaotist vaated.
    ![][ic719489]
   Saate selle logi ning **konsooli** vaade, juurutamise edenemise kuvamiseks. Teine võimalus on [Azure'i haldusportaal][]sisse logida ja **Pilveteenustega** jaotise abil saate jälgida olekut.
1. Juurutamise on edukalt juurutatud, jäänud **Azure'i tegevuse Log** olek on **avaldatud**. Klõpsake nuppu **avaldatud**, nagu on näidatud järgmisel pildil ja brauseri avatakse juurutamise eksemplari.
    ![][ic719490]

Kuna tegemist on juurutamine lavastus keskkonnas, saab DNS-i nimi kujul http://&lt;*guid*&gt;. cloudapp.net ning siis URL-i DNS-i nimi ning järelliide rakenduse sisaldavad. Näiteks http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. ( **MyHelloWorld** osa pole tõstutundlik). DNS-i nimi näete ka siis, kui klõpsate nuppu juurutamise nime Azure'i platvormi haldusportaali (jooksul pilveteenustega osa portaalis haldus).

Kuigi see walk-through juurutamiseks lavastus keskkonnas, tootmise juurutamine järgib samu juhiseid, välja arvatud **Avalda Azure** dialoogiboksi sees, valige **tootmise** asemel **lavastus** **Target keskkonnas**. DNS-i nime järgi teie valitud asemel kasutatud lavastus GUID URL-is tootmistulemusi juurutamine.

>[AZURE.WARNING] Selles etapis on juurutatud Azure rakenduse pilve. Siiski enne jätkamist mõistan, et juurutatud rakendusega, isegi siis, kui see ei tööta, jätkub tasustatav aeg tellimuse lisada. Seetõttu on väga oluline soovimatute juurutuste kustutamine Azure tellimuse.

## <a name="about-azure-deployment-projects"></a>Azure'i juurutamise projektide kohta ##

Juurutage üks või mitu rakendust Java Azure, Azure juurutamise projekt, mis on vaja. "Pakette" üheks peab olema avaldatud Azure mähkimiseks vajavate rakenduste roll.

Lisaks rakenduste kohta leiate Azure'i juurutamise projekt, mis sisaldab ka teavet teiste oluliste komponentide juurutamise, kõige olulisem: rakenduse serveri ümbris web app käivitamiseks ning Java runtime käitada. Azure'i toetab laia valikut Java runtimes ja Java rakenduste serverid saate valida.

Kuigi siin kasutatud näide on oluliselt lihtsustatud õppe-eesmärgil, Azure juurutamise projekt, mis võib sisaldada ka muud olulised konfiguratsiooniteavet, mis võimaldab teil luua peaaegu meelevaldselt keerukate scalable väga kättesaadav, mitmekihilise pilveteenustega oma rakendustega. Saate lubada **seansi osaleja ("sticky seanssi")**, **kiire vahemällu**, **Kaug silumine**, **SSL-i mahalaadimine**, **tulemüüri/port marsruutimine**, **Kaugpöördusteenuse**ja mitmesuguseid muid võimsaid võimalusi.

Kui olete lõpetanud eelmises jaotises selle õpetuse ("juurutada rakendust Azure, Kiire ja lihtne viis"), kuvatakse nüüd uue Azure juurutamise projekti Project Explorer loob teie jaoks automaatselt ja nimega "**MyHelloWorld_onAzure**".

Võib ka alustamist selles õpetuses esimene luua tühja Azure juurutamise projekti ise ja lisada selle oma rakendustes. See on pikem protsess, kuid annab teile rohkem kontrolli esialgne konfiguratsioon algusest.

Uue Azure juurutamise projekti loomine algusest peale ise luua, klõpsake nuppu **Uus Azure juurutamise projekt** ![][ic710876].

Sõltumata sellest, kas on juba olemasoleva Azure juurutamise projekti töötamine või loomine algusest peale ise luua, teil on võimalik muuta oma konfiguratsioonisätted ja osad on JDK või rakenduse server, nagu soovite igal ajal hõlpsalt.

JDK, või rakenduste serveri või rakenduste loendis Azure juurutamise olemasoleva projekti muutmiseks tehke järgmist.

1. Laiendage projekti (nt **MyHelloWorld_onAzure**) Project Explorer
2. Paremklõpsake **WorkerRole1**
3. **Azure'i** alammenüü kontekstimenüüs laiendamine
4. Klõpsake **serveri konfigureerimine**

Sõltumata sellest, kas hakkasite järgmist serveri konfiguratsiooni, redigeerides Azure juurutamise projekti, nagu eespool näidatud või luua uue eksemplari algusest peale ise luua, kuvatakse sama tüüpi dialoogid, mis võimaldab teil konfigureerida oma JDK, server ja rakenduse komponendid. Lisateavet nende dialoogid JDK, rakenduste serveri muuta ja lisada või eemaldada rakendusi juurutamine, nt sätete muutmise kohta artiklist [serveri konfiguratsiooni atribuudid][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Ainult Windows: rakenduse Arvuta emulaator juurutamiseks ##

>[AZURE.NOTE] Azure'i emulaator on saadaval ainult Windows. Kui kasutate operatsioonisüsteemi Windows peale selle jaotise vahele jätta.

Kui olete loonud uue Azure juurutamise projekti kirjeldatule varasemas versioonis, st peidetult, avaldades rakenduse Azure, JDK ja rakenduse serverid olla konfigureeritud pilves, kuid mitte kohaliku imiteerimist. Projekti koostada kohaliku emulaator testimiseks tehke järgmist.

1. Klõpsake Eclipse's Project Explorer **MyHelloWorld_onAzure**.
1. Paremklõpsake **WorkerRole1**.
1. Laiendage **Azure'i** alammenüü kontekstimenüüs.
1. Klõpsake **serveri konfigureerimine**.
1. Märkige vahekaardil **JDK** , kui tööriistakomplekt on eelnevalt konfigureeritud vaikimisi kohaliku JDK teie eest. Kui pole, või kui soovite eeldatava vaikesätete muutmiseks, veenduge, et märkeruut **kasutada JDK testimiseks kohalikult selle faili tee** on märgitud ja JDK installimise kohta, mida soovite kasutada määratud. Kui soovite seda muuta, klõpsake nuppu **Sirvi** ja valige kausta asukoht JDK kasutada Sirvi juhtelemendi abil.
1. Klõpsake vahekaarti **Server** .
1. **Kohaliku serveri tee** tekstivälja dialoogiboksi allservas, sisestage kohalikult installitud server, mis vastab tüüp ja põhiversioon arv valitud ülaosas dialoogiboksis **Deploy seda tüüpi server** ruut jaotises serveri tee. Kui soovite kasutada muud tüüpi või serveri põhiversiooni, muuta selle ruut jaotises valiku esmalt.
1. Klõpsake nuppu **OK**.
1. Eclipse tööriistaribal, klõpsake nuppu **Käivita Azure emulaator** ![][ic710879]. Kui nupp **Käivita Azure emulaator** pole lubatud, veenduge, et **MyHelloWorld_onAzure** on valitud Eclipse's Project Exploreri ja tagada, et Eclipse's Project Exploreri fookuse praeguse aken. See esmalt täielik Koosta projekti alustamine ja seejärel käivitage oma Java veebirakenduse Arvuta emulaator. (Sõltuvalt teie arvuti parameetritele, esimese koostamine võib kuluda mõni minut mõne hetke, et edaspidised koostab Märkus saab kiiremini.) Pärast kõigepealt koostamine on lõpule viidud, küsitakse, Windowsi kasutajakonto kontroll (UAC) Kui soovite lubada selle käsu muudatuste tegemiseks oma arvutisse. Klõpsake nuppu **Jah**.

>[AZURE.IMPORTANT] Kui te ei näe UAC viip, märkige ruut Windowsi tegumiriba ikooni UAC ja klõpsake seda esmalt. Mõnikord UAC viip ei kuvata nimega Pealmine aken, kuid on nähtav ainult nimega tegumiriba ikooni.

1. Uurige Arvuta emulaator UI kindlaks teha, kui seal on probleeme oma projekti väljund. Sõltuvalt sisu juurutamise, võib kuluda paar minutit oma rakenduse Arvuta emulaator täielikult alustada.
1. Alustage brauseri ja URL-i `http://localhost:8080/MyHelloWorld` aadressina (selle `MyHelloWorld` osa URL-i pole tõstutundlik). Rakenduse MyHelloWorld (väljund index.jsp), peaksite nägema umbes järgmist pilti: ![][ic589579]

Kui olete valmis rakenduse käivitumist Arvuta emulaator, Eclipse tööriistaribal lõpetamiseks klõpsake nuppu **Lähtesta Azure emulaator** ![][ic710880].

## <a name="to-delete-your-deployment"></a>Juurutamise kustutamine ##

Juurutamise jooksul Eclipse Azure'i tööriistakomplekt kustutatava veenduge, et **MyHelloWorld_onAzure** on valitud Eclipse's Project Exploreri, veenduge, et Eclipse Project Exploreri on aktiivne aken tähelepanu ja seejärel klõpsake nuppu **Tühista avaldamine** ![][ic710883], Eclipse tööriistaribal. (Võib tehke sama toimingut, klõpsates **MyHelloWorld_onAzure** Eclipse's Project Exploreri, klõpsates **Azure** ja seejärel käsku **Undeploy: Azure'i pilve**.) Kuvatakse dialoogiboks **Avaldamise Azure'i projekti** .

![][ic719491]

Valige tellimus ja pilvepõhise teenus, mis sisaldab teie juurutamise, valige juurutamise, mida soovite kustutada ja seejärel klõpsake nuppu **Tühista avaldamine**.

(Asemel tööriistakomplekt kustutatava juurutamise on kasutada Azure'i haldusportaal **Pilveteenustega** osa: liikuge juurutamise, valige see ja seejärel klõpsake nuppu **Kustuta** . See lõpetada, ja seejärel kustutage juurutamise. Kui ainult soovite lõpetada juurutamise ja kustutage see, asemel nuppu **Kustuta** nuppu **Lõpeta** , kuid eespool nimetatud kui kustutate juurutamise arveldatavate kulude kasutab ka edaspidi oma juurutamiseks lisada ka siis, kui see on peatatud).

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

[Mis on uut rakenduses Azure tööriistakomplekt Eclipse][]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i haldusportaal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure'i juurutuste on Eclipse Kaugpöördusteenuse lubamine]: http://go.microsoft.com/fwlink/?LinkID=699538
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546
[Serveri konfiguratsiooni atribuudid]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Mis on uut rakenduses Azure tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
