<properties
   pageTitle="Klõpsake virtuaalmasinates kogumite MATLAB | Microsoft Azure'i"
   description="Microsoft Azure'i virtuaalmasinates abil saate luua oma Arvuta mahukat paralleelselt MATLAB töökoormus käivitamiseks MATLAB jaotatud Computing serveri kogumite"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Azure VMs MATLAB jaotatud arvutuste serveri kogumite loomine 

Microsoft Azure'i virtuaalmasinates abil saate luua ühe või mitme MATLAB jaotatud Computing serveri kogumite käivitamiseks oma Arvuta mahukat paralleelselt MATLAB töökoormus. MATLAB jaotatud arvutuste serveri tarkvara installimine base pildina ja kasutada Azure Kiirjuhend malli või Azure PowerShelli skripti (saadaval [github](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) juurutada ja hallata klaster VM. Pärast juurutuse ühenduse kobar käivitamiseks oma töökoormus. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>MATLAB ja MATLAB jaotatud arvutuste serveri kohta 

[MATLAB](http://www.mathworks.com/products/matlab/) platvormi on optimeeritud engineering ja teaduslik probleemide lahendamiseks. Suuremahuliste simulatsioonid ja andmetöötlus tööülesannete MATLAB kasutajad saavad kasutada arvutuste toodete MathWorks samal ajal kiirendada oma Arvuta mahukat töökoormus ära Arvuta kogumite ja ruudustiku teenused. [Paralleelne arvutuste tööriistakasti](http://www.mathworks.com/products/parallel-computing/) abil MATLAB kasutaja parallelize rakenduste ja ära mitme protsessorite GPU, ja arvutada kogumite. [MATLAB jaotatud arvutuste Server](http://www.mathworks.com/products/distriben/) lubab MATLAB kasutada paljude arvutite arvutikobaraga. 


Azure'i virtuaalmasinates abil saate luua MATLAB jaotatud arvutuste serveri kogumite, mis sisaldavad kõiki samu menetlustele esitada paralleelselt töö nimega kohapealse kogumite, nt interaktiivsed tööde haldamine, pakett-tööde, sõltumatu tööülesanded ja suhtlemine tööülesanded saadaval. Kasutades Azure'i platvormi MATLAB koos on palju kasu võrreldes ettevalmistamise ja kasutamise traditsiooniline asutusesisese riistvara: virtuaalse masina erinevaid mahud kogumite nõudmisel nii, et maksate ainult Arvuta ressursid saate kasutamine ja võimalus testida mudelite tasandil loomine.  

## <a name="prerequisites"></a>Eeltingimused

* **Klientarvuti** - peate Windowsi-põhise klientarvuti suhelda Azure ja MATLAB jaotatud arvutuste serveri kobar pärast juurutamist. 

* **Azure'i PowerShelli** – vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) oma kliendi arvutisse installida. 

* **Azure'i tellimuse** – kui olete tellinud, saate luua [tasuta konto](https://azure.microsoft.com/free/) vaid paar minutit. Suuremate kogumite, on soovitatav pensionitingimustega tellimuse või muid ostmise võimalusi. 

* **Tuuma kvoodi** - suurendada core kvoote suur kobar või rohkem kui üks MATLAB jaotatud arvutuste serveri kobar juurutamiseks peate. Suurendamiseks kvoodi, [avage mõni Online'i kliendi tugiteenuse taotluse](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) tasuta. 

* **MATLAB, paralleelselt arvutuste tööriistakasti ja MATLAB jaotatud arvutuste serveri litsentside** - skriptide Oletame, et kõik litsentside kasutatakse [MathWorks majutatud litsentsi haldur](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) .  

* **MATLAB jaotatud arvutuste serveritarkvara** - installitakse VM, mida kasutatakse kobar VMs nimega VM pilti. 


## <a name="high-level-steps"></a>Kõrge taseme juhised

Teie kogumite MATLAB jaotatud arvutuste Server Azure'i virtuaalmasinates kasutamiseks on vaja üksikasjalik järgmist. Üksikasjalikud juhised on lisatud Kiirjuhend Mall ja skriptide [github](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)dokumentatsiooni.

1. **Looge base VM pilt**  
    * Allalaadimine ja installimine MATLAB jaotatud arvutuste serveri tarkvara peale selle VM. 

    >[AZURE.NOTE]Selleks võib kuluda paar tundi, kuid tuleb seda teha, kui iga MATLAB versiooni te kasutate.   
    
2. **Ühe või mitme kogumite loomine**  
    * Kasutage kaasasolevat PowerShelli skripti või Kiirjuhend malli abil saate luua klaster VM pilti.   
    * Hallata rühmad abil esitatud PowerShelli skripti, mis võimaldab teil loendis, peatamiseks, jätkamiseks ja kustutada kogumite. 
 
## <a name="cluster-configurations"></a>Kobar konfiguratsioone 

Praegu kobar loomine script ja malli võimaldavad teil luua ühe MATLAB jaotatud arvutuste serveri topoloogia. Soovi korral võite luua ühe või mitme täiendava kogumite, iga töötaja VMs, kasutades erineva suurusega VM, on muu number kobar jne. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB klient ja Azure kobar 

MATLAB kliendi sõlm, MATLAB projektiplaanur sõlm ja MATLAB jaotatud arvutuste serveri "töötaja" sõlmed on kõik konfigureeritud Azure VMs virtuaalse võrku, nagu on näidatud järgmisel joonisel. 

![Kobar topoloogia](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Klaster kasutamiseks ühenduse kaugtöölaua kliendi sõlm. Kliendi sõlm käivitatakse MATLAB kliendi. 

* Kliendi sõlm on kõik töötajad pääsevad juurde failikettal.

* MathWorks majutatud litsentsi Manager kasutatakse kõik MATLAB tarkvara litsentsi kontroll. 

* Vaikimisi luuakse üks MATLAB jaotatud arvutuste serveri töötaja kohta core töötaja VMs, kuid saate määrata mõni muu arv. 


## <a name="use-an-azure-based-cluster"></a>Kasutage mõnda Azure'i kobar 

Muud tüüpi MATLAB jaotatud arvutuste serveri kogumite, peate kasutama kobar profiili Manager (klientarvutis VM) klient MATLAB MATLAB projektiplaanur kobar profiili loomine.

![Profiili halduri](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Järgmised sammud

* Juurutada ja hallata MATLAB jaotatud arvutuste serveri kogumite Azure üksikasjalikud juhised leiate teemast [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) hoidla, mis sisaldab malle ja skriptide. 

* Minge [MathWorks saidi](http://www.mathworks.com/) jaoks üksikasjalikud dokumendid MATLAB ja MATLAB jaotatud arvutuste serveri jaoks.
