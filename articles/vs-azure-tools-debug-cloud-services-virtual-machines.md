<properties 
    pageTitle="Azure'i pilveteenuses- või Visual Studio virtuaalse masina silumine | Microsoft Azure'i"
    description="Silumine pilveteenuses või virtuaalse masina Visual Studio"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Azure'i pilveteenuses- või Visual Studio virtuaalse masina silumine

Visual Studios annab teile silumine Azure pilveteenustega ja virtuaalmasinates erinevaid võimalusi.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Oma kohalikus arvutis pilveteenuses silumine

Saate säästa aega ja raha, kasutades funktsiooni Azure'i arvutada emulaator silumine oma kohalikus arvutis pilveteenuses. Enne juurutamist silumine kohalik teenus, saate parandada töökindlust ja jõudlust Arvuta aja maksmata. Vigu võib siiski tekkida ainult käivitamisel pilveteenus Azure ise. Kui lubate Kaug silumine kui avaldamine teenust ja seejärel selle manusena siluri rolli eksemplari, saate nende vigade silumine.

Emulaator jäljendab Azure'i arvutada teenuse ja töötab teie kohaliku keskkonnas, et saate testida ja silumine oma pilveteenuses enne juurutamist. Emulaator tegeleb oma rolli aknad elutsükli ja pääsete juurde jäljendatud ressursse, näiteks kohalikku. Kui silumine või teenust pidamine Visual Studios, käivitub automaatselt emulaator tausta rakendus ja seejärel juurutamine teenust emulaator. Emulaator abil saate vaadata oma teenuste käitamisel kohalikus keskkonnas. Saate käivitada täisversiooni või emulaator kiire versiooni. (Alates Azure'i 2.3, emulaator kiire versioon on vaikimisi.) Vt [abil emulaator kiire käivitamiseks ja silumine kohalikult pilveteenuses](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Kui soovite oma kohalikus arvutis pilveteenuses silumine

1. Valige menüüribal **silumine**, **Käivitage silumine** Azure pilveteenuse teenuse projekti käivitamiseks. Teise võimalusena võite vajutada klahvi F5. Kuvatakse teade, et arvutada emulaator hakkab. Emulaator käivitamisel ikooni süsteem teid sellest.

    ![Azure'i emulaator süsteemisalves](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Kasutajaliidese kuvamine Arvuta emulaatori, avades kiirmenüü Azure ikoon olekualal ja seejärel valige **Kuva arvutada emulaator UI**.

    UI vasakul paanil kuvatakse Arvuta emulaator ja rolli aknad, kus iga teenuse töötab praegu juurutatud teenuseid. Saate valida kasutatava teenuse või kuvamiseks elutsükli, logimine ja diagnostikateave parempoolsel paanil rollid. Kui fookus on ülaveerise kaasatud akna, laieneb see parempoolsel paanil.

1. Juhis rakenduse kaudu, valides menüü **silumine** käske ja säte katkestuspunktid koodi. Samm siluris rakenduse kaudu, värskendatakse paanid praeguse oleku kuvamiseks rakenduse abil. Kui lõpetate silumine, kustutatakse rakenduse juurutamine. Kui teie taotlus sisaldab web rolli ja määrate atribuudi käivitus toimingu alustamiseks veebibrauseri, käivitab Visual Studio oma veebirakenduse brauseris. Kui muudate teenuse konfigureerimine rolli eksemplaride arv, peate oma pilveteenuses peatada ja seejärel taaskäivitage silumine nii, et te saate need uued eksemplarid rolli silumine.

    **Märkus:** Kui lõpetate opsüsteemi või silumine teenust, kohaliku Arvuta emulaator ja salvestusruumi emulaator ei lõpetanud. Peatage neile konkreetselt olekuala kaudu.


## <a name="debug-a-cloud-service-in-azure"></a>Pilveteenus Azure silumine

Silumine pilveteenus remote arvutist, tuleb selle funktsiooniga selgesõnaliselt lubada, kui juurutate oma pilveteenuses nii, et nõutav virtuaalmasinates, mis töötavad teie rolli aknad on installitud teenused (nt msvsmon.exe). Kui te ei luba Kaug silumine teenuse avaldamisel, tuleb uuesti avaldada teenuse abil Kaug silumine lubatud.

Kui lubate Kaug silumine pilveteenus, ei kaasa tuua jõudluse langemist või täiendava tasuline. Ei tohiks saate Kaug silumine tootmise teenus, kuna klientidele, kes kasutavad teenust võib kahjustada.

>[AZURE.NOTE] Kui avaldate pilveteenus Visual Studio, saate lubada **IntelliTrace** rollide teenuses, mis on suunatud .NET Framework 4 või .NET Framework 4.5. **IntelliTrace**abil saate uurida varem toimunud rolli eksemplari sündmuste ja paljundada kaudu selle aja kontekstis. Lugege teemat [silumine avaldatud pilveteenuses IntelliTrace ja Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) ja [IntelliTrace abil](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Lubada remote silumine pilveteenus

1. Azure'i projekti kiirmenüü avamine ja seejärel valige **Avalda**.

1. Valige **lavastus** keskkonna ja **silumine** konfigureerimine.

    See on ainult üldjoontes. Saate valida, kas käivitada oma test keskkondades tootmiskeskkonnas. Samas võite halvendada kasutajate kui lubate Kaug silumine tootmiskeskkonda kohta. Soovi korral saate väljaanne konfiguratsiooni, kuid silumine konfiguratsiooni teeb silumine lihtsamaks.

    ![Valige silumine konfiguratsioon](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Tavaline juhised, kuid märkige ruut **Luba Kaug kõigi rollide** vahekaardil **Täpsemad sätted** .

    ![Silumine konfigureerimine](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Azure pilveteenusesse siluri manustamine

1. Server Explorer, laiendage oma pilveteenuses.

1. Roll või roll eksemplari, millele soovite manustada kiirmenüü avamine ja valige **Manusta Silur**.

    Kui te silumine rollid, peab iga eksemplari rolli Visual Studio siluri. Siluri on murda katkestuspunkti rolli kõigepealt, mis käivitub selle rida koodi ja kõik selle katkestuspunkt tingimuste jaoks. Kui te silumine eksemplari siluri peab ainult selle eksemplari ja leheküljepiiride katkestuspunkti ainult siis, kui selle kindla eksemplari töötab selle rida koodi ja selle katkestuspunkt tingimustele.

    ![Siluri manustamine](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Pärast siluri manustab eksemplari, silumine nagu tavaliselt. Siluri manustab automaatselt vastav host protsessi oma rolli. Sõltuvalt sellest, mis on roll, peab siluri w3wp.exe, WaWorkerHost.exe või WaIISHost.exe. Kinnitamise protsess, millele on manustatud siluri, laiendage eksemplari Server Explorer. Teemast [Azure rolli arhitektuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Azure protsesside kohta lisateavet.

    ![Valige dialoogiboksi koodi tüüp](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Protsessid, millele on manustatud siluri tuvastamiseks protsesside dialoogiboksi avamine, klõpsake menüü, valides silumine, Windowsi, protsessid. (Klaviatuuri: klahvikombinatsiooni Ctrl + Alt + Z) Konkreetse protsessi eemaldamiseks avage kiirmenüüs ja valige **Protsessi lahti**. Või, otsimiseks Exploreris serveri eksemplar, protsessi leidmise järel, selle kiirmenüü avamine ja valige **Protsessi eemaldada**.

    ![Protsesside silumine](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Vältige pikk peatub katkestuspunktid kui Kaug silumine. Azure'i käsitleb protsessi, mis on peatatud rohkem kui paar minutit, kui ei reageeri ja lõpetab liikluse saata selle eksemplari. Kui lõpetate liiga kauaks, eemaldub msvsmon.exe protsess.

Kõigi protsesside siluri eksemplari või rolli eemaldamiseks rolli või eksemplar, mida te kasutate silumine kiirmenüü avamine ja valige **Siluri eemaldada**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Remote silumine Azure piirangud

Azure'i SDK 2.3 kaudu remote silumine on järgmised piirangud.

- Koos Kaug silumine lubatud, ei saa avaldada, kus on mis tahes rolli üle 25 eksemplari pilveteenus.

- Siluri kasutab pordid 30400 abil 30424, 31400 abil 31424 ja 32400 abil 32424. Kui proovite kasutada mis tahes järgmised pordid, ei saa te teenust avaldama ja ühte järgmistest tõrketeadetest kuvatakse tegevuste Logi Azure: 

    - Tõrge .cscfg faili vastu .csdef faili valideerimine. 
    Reserveeritud pordi vahemik "vahemik" lõpp-punkti Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector roll 'rolli' kattub juba määratud või vahemik.
    - Eraldatud nurjus. Proovige hiljem uuesti, proovige vähendada VM suuruse või rolli eksemplaride arv või proovige muule alale juurutamine.


## <a name="debugging-azure-virtual-machines"></a>Azure'i virtuaalmasinates silumine

Saate silumine Azure'i virtuaalmasinates Server Explorer Visual Studio abil töötavad programmid. Kui lubate Kaug silumine Azure virtuaalne arvutis, installib Azure virtuaalse masina Kaug silumine laiend. Seejärel saate manustada protsesside virtual arvutisse ja silumine nagu tavaliselt.

>[AZURE.NOTE] Azure'i ressursi halduri virnas abil loodud virtuaalmasinates saate kaugühenduse teel silumisel Visual Studio 2015 Cloud Exploreri kaudu. Lisateabe saamiseks vt [Haldamise Azure'i ressursid Cloud Exploreriga](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Kui soovite mõne Azure virtuaalse masina silumine

1. Server Explorer, laiendage Virtuaalmasinates ja valige sõlm virtuaalse masina, mida soovite silumine.

1. Valige **Luba silumine**kontekstimenüü avamiseks järgmist. Kui teilt küsitakse, kui te pole kindel, kas soovite lubada silumine virtual arvutisse, valige **Jah**.

    Azure'i installib Kaug silumine laiend virtuaalse masina lubamiseks silumine.

    ![Virtuaalse masina silumine käsu lubamiseks](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure'i tegevuse log](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Kui installimine on lõppenud Kaug silumine laiend, virtuaalse masina kontekstimenüü avamiseks ja valige **Manusta siluri**

    Azure'i saab protsesside loendi virtual arvutisse ja kuvab need manustamine protsessi dialoogiboks.

    ![Siluri käsk Manusta](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Valige dialoogiboksis **Manusta protsessi** **valida** tulemuste loendis kuvada ainult soovitud tüüpi kood, mida soovite silumine piirata. Te saate silumine 32 - või 64-bitine hallatud koodi, kohalikke kood või mõlemad.

    ![Valige dialoogiboksi koodi tüüp](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Valige protsesside soovite silumine virtual arvutisse ja seejärel valige **Manusta**. Näiteks võite w3wp.exe protsessi, kui soovite kalendriüksusi silumine web appi virtual arvutisse. Lisateabe saamiseks vaadake [silumine ühe või mitme protsessi Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) ja [Azure rolli arhitektuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Web projekti ja virtuaalse masina silumine loomine

Azure'i projekti avaldamiseks võib olla teile kasulik seda katsetada seda suletud keskkonnas, mis toetab silumine ja testimine stsenaariumid ja kus installida katsetada ja jälgida programmid. Üks viis selleks on eemalt silumine rakenduse virtual arvutisse.

Visual Studio ASP.net-i projektid pakub võimalust luua mugav virtuaalse masina, mille abil saate rakenduse testimiseks. Virtuaalse masina sisaldab sageli vaja lõpp-punktid nagu PowerShelli, kaugtöölaua ja WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Luua web projekti ja virtuaalse masina silumine

1. Visual Studio ASP.net-i uue veebirakenduse loomine.

1. Valige dialoogiboksi uue ASP.net-i projekti Azure jaotises ripploendiboksi **virtuaalse masina** . Jätke **loomine remote ressursid** ruut märgitud. Valige jätkamiseks **OK** .

    Kuvatakse dialoogiboks **Loo virtuaalse masina Azure** .


    ![ASP.net-i veebidialoogiboksis projekti loomine](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Märkus:** Küsitakse Azure'i kontosse sisse logida, kui te pole veel loginud.

1. Valige virtuaalse masina erinevad sätted ja seejärel klõpsake **nuppu OK**. Lisateabe saamiseks vaadake [Virtuaalmasinates]( http://go.microsoft.com/fwlink/?LinkId=623033) .

    DNS-i nimi soovitud nimi on virtuaalse masina nimi. 

    ![Azure'i dialoogiboksi virtuaalse masina loomine](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure'i loob virtuaalse masina ja seejärel sätteid ja konfigureerib lõpp-punktid, nt kaugtöölaua ja Web juurutamine



1. Kui virtuaalse masina on täielikult konfigureeritud, valige virtuaalse masina sõlme Server Explorer.

1. Valige **Luba silumine**kontekstimenüü avamiseks järgmist. Kui teilt küsitakse, kui te pole kindel, kas soovite lubada silumine virtual arvutisse, valige **Jah**. 

    Azure'i installib Kaug silumine laiend virtuaalse masina lubamiseks silumine.

    ![Virtuaalse masina silumine käsu lubamiseks](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure'i tegevuse log](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Saate oma projekti avaldada, nagu on kirjeldatud [kohta: juurutamine on Web projekti abil ühe klõpsuga avaldamine Visual Studios](https://msdn.microsoft.com/library/dd465337.aspx). Kuna soovite silumine arvutis virtual **Veebis avaldamine** viisardi lehel **sätted** valige konfiguratsiooni **silumine** . See muudab kindel, et koodi sümbolid ajal silumine on saadaval.

    ![Avaldamine sätted](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. **Faili Avaldamissuvandid**, valige **täiendavate failide sihtkohta eemaldamine** kui projekt on juba juurutanud varasema ajal.

1. Kui projekt avaldab virtuaalse masina kontekstimenüü Server Explorer, valige **Manustamine Silur**

    Azure'i saab protsesside loendi virtual arvutisse ja kuvab need manustamine protsessi dialoogiboks.

    ![Siluri käsk Manusta](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Valige dialoogiboksis **Manusta protsessi** **valida** tulemuste loendis kuvada ainult soovitud tüüpi kood, mida soovite silumine piirata. Te saate silumine 32 - või 64-bitine hallatud koodi, kohalikke kood või mõlemad.

    ![Valige dialoogiboksi koodi tüüp](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Valige protsesside soovite silumine virtual arvutisse ja seejärel valige **Manusta**. Näiteks võite w3wp.exe protsessi, kui soovite kalendriüksusi silumine web appi virtual arvutisse. Lisateabe saamiseks vaadake [silumine ühe või mitme protsessi Visual Studios](https://msdn.microsoft.com/library/jj919165.aspx) .

## <a name="next-steps"></a>Järgmised sammud

- Kõned ja sündmuste logi kogumine väljaanne server **Intellitrace** abil. Vt [silumine avaldatud pilveteenuses IntelliTrace ja Visual Studio abil](http://go.microsoft.com/fwlink/?LinkID=623016).
- **Azure'i diagnostika** abil saate üksikasjalikku teavet koodi, mis töötab rollid, jooksul sisse logida, kas rollid töötab soovitud arenduskeskkond või Azure. Vt [kogumine logimine andmete Azure'i diagnostika abil](http://go.microsoft.com/fwlink/p/?LinkId=400450).
