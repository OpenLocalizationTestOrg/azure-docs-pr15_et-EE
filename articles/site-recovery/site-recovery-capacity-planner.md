<properties
    pageTitle="Leping võimsus virtuaalmasinates ja Azure saidi taastamise füüsilise serverites | Microsoft Azure'i"
    description="Azure'i saidi taastamine koordinaadid dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja kohapealse Azure'i või sekundaarne kohapealse saidi asuv füüsilise serveri." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Lepingu võimsus virtuaalmasinates ja füüsilise serverites Azure saidi taastamine

Azure'i saidi taastamise võimsus Plaanur tööriista abil saate aru saada oma Hyper-V VMs, VMware VMs ja Azure'i saidi taastamine Windows/Linux füüsilise serveri nõuetele.


## <a name="overview"></a>Ülevaade

Kasutage saidi taastamise võimsus Plaanur analüüsida teie andmeallika keskkonna ja teenustest ja läbilaskevõime vajadustele, peate andmeallika asukoha serveri ressursid ja ressursid (virtuaalmasinates ja salvestusruumi jne), peate oma sihtkoht välja selgitada. 

Käivitage tööriist režiimide paari:

- **Kiirülevaate kavandamine**: Käivita tööriist selles režiimis saada võrgu ja serveri prognooside põhjal Keskmine arv VMs, ketast, salvestusruumi ja Muuda määr.
- **Üksikasjalik kavandamine**: selles režiimis tööriista ja iga töökoormus VM tasemel üksikasjad. Analüüsi VM ühilduvuse ja saada võrgu ja serveri prognooside.

## <a name="before-you-start"></a>Enne alustamist

Enne tööriista:

1. Koguge teavet keskkonna, sh VMs ketast VM ketta salvestusruumi kohta.
2. Leidke oma igapäevast muutmine (piimapütt) määr kopeeritud andmete jaoks. Soovitud toiming

    - Kui olete imitatsiooniga Hyper-V VMs siis [Hyper-V võimsus kavandamise tööriista](https://www.microsoft.com/download/details.aspx?id=39057) saada muutmine määr alla. [Lisateavet](site-recovery-capacity-planning-for-hyper-v-replication.md) selle tööriista kohta. Soovitame seda tööriista käivitamist üle nädala keskmiste jäädvustada.
    - Kui olete imitatsiooniga VMware virtuaalmasinates, [vSphere võimsus kavandamise seadme](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) abil piimapütt määr välja selgitada.
    - Kui olete imitatsiooniga füüsilise serveri peate prognoosimiseks käsitsi.

## <a name="run-the-quick-planner"></a>Käivitage kiirülevaate Plaanur
1.  Laadige alla ja avage [Azure'i saidi taastamise võimsus Plaanur](http://aka.ms/asr-capacity-planner-excel) tööriist. Peate käivitada makrosid nii valige Luba redigeerimine ja sisu, kui seda küsitakse. 
2.  **Valige Plaanur tüüp** valige loendiboksist **Kiirülevaate Plaanur** .

    ![Alustamine](./media/site-recovery-capacity-planner/getting-started.png)

3.  **Võimsus Plaanur** töölehel, sisestage nõutav teave. Peate täitma kõik väljad circled punaselt pildil.

    - **Valige stsenaariumist** valige **Hyper-V Azure'i** või **VMware/füüsilise Azure**.
    - **Keskmine igapäevane andmeid muuta (%)** teave sellele koguda saate [Hyper-V võimsus kavandamise tööriista](site-recovery-capacity-planning-for-hyper-v-replication.md) või [vSphere võimsus kavandamise seadme](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance)abil.  
    - Kui imitatsiooniga VMware VMs või füüsilise serveri Azure tihendamise kehtib ainult **tihendamise** . Meie hinnangul 30% või rohkem, kuid te saate seda sätet vastavalt vajadusele muuta. Imitatsiooniga Hyper-V VMs Azure'i tihendamise saate kasutada mõne muu seadme nagu jõesäng. 
    -  Määrake **Säilituspoliitika sisendina** kaua tuleks säilitada koopiad. Kui te olete imitatsiooniga VMware või füüsilise serveri sisestage väärtus päeva jooksul. Kui te olete imitatsiooniga Hyper-V Määrake mõne tunni pärast.
    -  **Arv, mis algse dispersioonanalüüs virtuaalmasinates paketi tunnid tuleb täita** ja **arvu virtuaalmasinates algse dispersioonanalüüs partii** sisestatud sätted, mida kasutatakse arvutada algse dispersioonanalüüs nõuded.  Saidi taastamine juurutamisel kogu algse andmehulga üles laadida. 

    ![Sisendina](./media/site-recovery-capacity-planner/inputs.png)

2.  Pärast seda, kui te olete sellele allika keskkonnas väärtused, kuvatakse väljund sisaldab:

    - **Läbilaskevõime vaja delta dispersioonanalüüs** (MB sekundis). Võrgu läbilaskevõime delta kopeerimise arvutatakse Keskmine andmete muutmine hind.
    - **Läbilaskevõime vaja algse dispersioonanalüüs** (MB sekundis). Võrgu läbilaskevõime algse kopeerimise arvutatakse algse dispersioonanalüüs väärtused saate panna. 
    - **Salvestusruumi nõutav (GBs)** on nõutav Azure kogu talletusmaht.
    - **Kokku IOPS kindlad kontodel** on arvutatud 8 K IOPS üksuse suurus standard saadaoleva salvestus-kontodega.  Kiirülevaate Plaanur arv arvutatakse allika VMs ketast ja igapäevane andmete põhjal muuta määr. Üksikasjalik Plaanur arv arvutatakse koguarvu VMs, mis on vastendatud standard Azure VMs ja andmete põhjal muuta need intressimäär. 
    - **Kindlad kontode arv** pakub kindlad kontod vaja kaitsta VMs arv. Pange tähele, et kindlad konto võib sisaldada kuni 20 000 IOPS üle kõik VM standard salvestusruumi ja kuni 500 IOPS toetatud ketta kohta. 
    - **Nõutav arv bloobimälu ketast** esitab ketast, Azure Storage loodud arvu.
    - **Nõutav arv premium salvestusruumi kontod** pakub premium salvestusruumi kontod vaja kaitsta VMs arv. Pange tähele, et allika VM kõrge IOPS (suurem kui 20000) koos premium salvestusruumi konto. Salvestusruumi premium konto võib olla kuni 80000 IOPS.
    - **Kokku IOPS premium Storage** on arvutatud 256 K IOPS ühiku suurus kokku premium salvestusruumi kontodega.  Kiirülevaate Plaanur arv arvutatakse allika VMs ketast ja igapäevane andmete põhjal muuta määr. Üksikasjalik Plaanur arv arvutatakse koguarvu VMs, mis on vastendatud premium Azure VM (DS ja GS seeria) ja andmete põhjal muuta need intressimäära. 
    - **Nõutav konfiguratsioon serverid arv** näitab, mitu konfiguratsiooni serverid on vaja juurutamise (1)
    - **Nõutav täiendavad protsessi serverid arv** näitab, kas täiendavad protsessi serverid on vaja lisaks protsessi server on konfigureeritud konfigureerimine serveris vaikimisi.
    - **100% täiendav salvestusruum allikas** näitab, kas allikas asukoht on vaja täiendavat salvestusruumi.
            
    ![Väljund](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>Käivitage üksikasjalik Plaanur


1.  Laadige alla ja avage [Azure'i saidi taastamise võimsus Plaanur](http://aka.ms/asr-capacity-planner-excel) tööriist. Peate käivitada makrosid nii valige Luba redigeerimine ja sisu, kui seda küsitakse. 
2.  **Valige Plaanur tüüp** valige loendiboksist **Üksikasjalik Plaanur** .

    ![Alustamine](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  **Töökoormus kvalifikatsioon** töölehel, sisestage nõutav teave. Peate täitma tähistatud väljad.

    - Määrake **protsessori** allikas server valdkond koguarv.
    - Määrake **mälu eraldamine MB** RAM-i serveris suurust. 
    - **Arv, NICs** Määrake allikas server võrguadapteri arv. 
    -  Määrake **saadaoleva salvestus (GB) jaotises** suuruse VM salvestusruumi. Näiteks kui lähteserveriga on 500 GB 3 ketast, siis kokku mälumaht on 1500 GB.
    -  Määrake **arv ketast, millele on manustatud** ketast allikas serveri koguarv.
    -  Määrake **ketta võimsus kasutamine** keskmise kasutamine.
    -  **Iga päev muuta määr (%)** Määrake iga päev andmeid muuta serveris.
    -  **Azure'i vastendamise suurus** ühte sisestage Azure VM suurus, mida soovite vastendada. Kui te ei soovi seda teha käsitsi nuppu**Arvuta IaaS VMs**. Pange tähele, et kui sisestusteade käsitsi määramine ja seejärel klõpsake nuppu Arvuta IaaS VMs oma käsitsi määramine võib üle kirjutada, kuna Arvuta protsessi automaatselt tuvastab Parim vaste Azure VM suuruse.

    ![Töökoormus tingimustele vastamine](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Kui klõpsate nuppu **Arvuta IaaS VMs** siin on toiming:

    - Kohustusliku sisendina kinnitatakse.
    - Arvutab IOPS ja soovitab parima Azure VM aize iga VMs vastet, mis on Azure kopeerimise. Kui sobiva suurusega Azure'i VM ei saa tuvastada, antakse tõrke. Näiteks kui arvu kettad kinnitada 65 viga on probleeme alates kõrgeim suurus Azure VM on 64.
    - Soovitab salvestusruumi konto, mida saab kasutada ka Azure VM.
    - Arvutab kindlad kontod ja premium salvestusruumi kontod nõutavaid töökoormus koguarv. Liikuge kerides allapoole paremal salvestusruumi Azure type ja salvestusruumi konto, mida saab kasutada serveris vaatamine
    - On lõpule jõudnud ja sordib ülejäänud nõutavad salvestusruumi tüüp (standard- või lisatasu) VM ja ketast, millele on manustatud arvu määratud tabelis. Kuvatakse kõik VMs, mis vastavad varukoopiat Azure, veerg A (on VM sobivad?) Jah. Kui VM ei saa tagatud kuni kuvatakse Azure'i viga.

AA veerud au on väljundi ja teavet iga VM.

![Töökoormus tingimustele vastamine](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Näide
Näitena kuus vms väärtustega tabelis, tööriist arvutab ja määrab parima Azure VM match ja Azure storage nõuetele.

![Töökoormus tingimustele vastamine](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- Näiteks väljund võtke arvesse järgmist.
    
    - Esimene veerg on VMs, ketast ja piimapütt veeru valideerimine.
    - Kahe kindlad kontod ja ühe premium salvestusruumi konto on vaja viit vms. 
    -  VM3 ei kvalifitseeru kaitse, kuna üks või enam ketast on rohkem kui 1 TB.
    -  VM1 ja VM2 saate kasutada esimese kindlad konto
    -  VM4 saate kasutada kindlad teisele kontole.
    -  VM5 ja VM6 premium salvestusruumi konto ja saab kasutada nii ühe konto.

    >[AZURE.NOTE]  Standard- ja premium Storage IOPS arvutatakse VM tasemel ja pole ketta tasemel. Standardse virtuaalse masina saab hakkama kuni 500 IOPS ketta kohta. Kui ketta jaoks IOPS on suurem kui 500 peate premium mälu. Aga kui IOPS jaoks on rohkem kui 500, kuid IOPS VM ketast jaoks on piires tugi standard Azure VM (VM suurus, arv ketast, adapterit, CPU, mälu arv), siis selle Plaanur huvitavat standard VM ei DS või GS sarja. Peate käsitsi värskendamiseks vastenduse Azure suurus lahtri vastav DS või GS sarja VM.

5. Pärast kõik andmed on olemas, klõpsake nuppu **Edasta andmed Plaanur tööriista** **Võimsus Plaanur** avamiseks töökoormus on esile tõstetud kuvamiseks, kas need on kaitse või mitte.


### <a name="submit-data-in-the-capacity-planner"></a>Esitage andmete võimsus Plaanur

1.  Kui avate **Võimsus Plaanur** töölehe sisustatakse vastavalt teie määratud sätted. Sõna "Töökoormus" kuvatakse näitamaks, et Sisestuskeel **Töökoormus kvalifikatsioon** töölehe lahtris **Infra sisendina allikas** . 
2.  Kui soovite muuta, peate muuta **Töökoormus kvalifikatsioon** tööleht ja klõpsake uuesti nuppu Esita andmed Plaanur tööriist.  

    ![Võimsuse Plaanur](./media/site-recovery-capacity-planner/capacity-planner.png)


