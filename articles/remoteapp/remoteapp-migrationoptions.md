
<properties 
    pageTitle="Suvandite migreerimine välja Azure RemoteApp | Microsoft Azure'i" 
    description="Siit saate teada, migreerimine välja Azure RemoteApp võimaluste kohta." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Meiliteabe Azure RemoteApp välja suvandid

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kui olete lõpetanud, kasutades Azure RemoteApp tõttu [pensionile jäämise teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) või kuna olete oma hindamise lõpule jõudnud, peate rakenduse mõnes muus teenuses Azure RemoteApp off migreerimine. On kaks erinevat lähenemisviisi migreerimiseks: omas hallatavate (sageli nimetatakse taristu teenust [IaaS]) juurutamise või täielikult hallatud (sageli nimetatakse platvormi teenus) või tarkvara [PaaS/SaaS] teenust pakub. 

Iseteeninduslik IaaS on iseseisev juurutamine, mis on hallatavad, mida käitab ja olete otse juurutatud virtuaalmasinates (VMs) või füüsilise süsteemid. Teises otsas täielikult hallatud PaaS/SaaS pakub on rohkem nagu Azure RemoteApp - partneri pakub teenuse kiht peal remoting lahenduse, mis tegeleb funktsionaalseid ja hooldus, ajal, kui kliendi, tehke mõnda pilti ja rakenduse haldus.

Lugeda Lisateavet näiteid majutusteenuse erinevaid võimalusi.    

## <a name="self-managed-iaas-solutions"></a>Omas hallatavad (IaaS) lahendused

### <a name="rds-on-iaas"></a>**IaaS RDS** 
Kohalikke seansi (opsüsteemis Windows Server) kaugtöölaua teenuste juurutamine RemoteApp või lauaarvutite asutusesisese abil saate juurutada või majutatud keskkonnas (nagu klõpsake Azure VMs). RDS IaaS juurutuste sobivad eelkõige klientidele, kes juba tuttav ja mis on olemasoleva tehnilise teadmisi RDS kasutuselevõttu. 

> [AZURE.NOTE]
> Hulgilitsentsimise koos tarkvaratagatise (SA) RDS kliendi juurdepääsu litsentside juurutamise selle suvandi kasutamiseks peate.

Juurutamine RDS Azure VMs on hõlpsam kui kunagi varem, kui kasutate juurutamise ja lappimine mallid (vt [Ülevaade](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) ja seejärel [Valige saada neid](https://aka.ms/rdautomation)). Saate avada sama elastne skaleerimise võimaluste Azure klassikaline juurutamise mudeli vahendeid (mitte Azure'i ressursi mudeli ressursid) Azure RemoteApp kuigi on veel kohandused ja konfiguratsioone [muutmine skripti](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76)abil. RDS Azure VMs juurutamisel toetatakse kaudu [Azure'i tugi](https://azure.microsoft.com/support/plans/), saate toetatud Azure'i RemoteAppi sama tugitöötajad. Saate hankida maksumuse hinnangute põhjal oma olemasoleva kasutus, [Azure'i](https://azure.microsoft.com/support/plans/)klienditoega või saab teha arvutusi ise abil soovitud varsti vabastatakse kulude Kalkulaator.  Lisaks N-seeria VMs (praegu privaatne eelvaade) saate lisada vGPU - kuulata veel vGPU lisamise kohta ja selle kohta, kuidas rakendada [RDS täiustused Windows Server 2016](https://myignite.microsoft.com/videos/2794) meie Ignite seanss.   

Meil on samm-sammult juurutamise juhised [Windows Server 2012 R2](http://aka.ms/rdsonazure) ja [Windows Server 2016](http://aka.ms/rdsonazure2016) juurutamise abistamine. Tutvuge [Kaugtöölaua ajaveebi](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) värskeimate uudiste.
 
### <a name="citrix-on-iaas"></a>**Klõpsake IaaS Citrix** 
Kohalikke Citrix, võib olla seansi XenApp või XenDesktop juurutatud asutusesisese või majutatud keskkonnas (nt klõpsake Azure VMs). 

Juurutamise samm-sammult juhendi leiate [Citrix XA 7,6 Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), lisateabe saamiseks vaadake. Lugege lisateavet [Citrix Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), sh hinna Kalkulaator. Leiate ka [Citrix, võtke ühendust](http://citrix.com/English/contact/index.asp) oma võimalusi arutada.

## <a name="fully-managed-paassaas-offerings"></a>Täielikult hallatud (PaaS/SaaS) pakkumisi

### <a name="citrix-cloud"></a>**Citrix pilveteenuses** 
[Citrix olemasoleva pilve lahendus](https://www.citrix.com/products/citrix-cloud/)arhitektuuriliselt identne Citrix XenApp kiire. Citrix olemasoleva Azure RemoteApp klientidele, kes pakub [50% allahindlus edendamine](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) . 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp väljendada (tech preview)**
[Registreeruge oma tehnilise eelvaate](http://now.citrix.com/remoteapp), ja vaadake oma [Ignite seanss](https://myignite.microsoft.com/videos/2792) (alates 20:30 minuti). XenApp Express on identne arhitektuuriliselt Citrix Cloud, kuid see sisaldab lihtsustatud haldamise UI ja muude funktsioonide ja võimaluste kohta, mis on sarnane Azure RemoteApp. 

Lisateave [Citrix XenApp kiire](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix teenuse pakkuja programm** 
Citrix teenuse pakkuja programmi lihtsustab teenusepakkujatele esitamisel lihtsad virtuaalse pilve arvutuste SMBs, et nende teenuste soovivad lihtne, pensionitingimustega mudelis pakkumine. Citrix teenusepakkujatele oma Microsofti SPLA äri edendamine ja laiendamine RDS platvormi investeeringute mis tahes seadme abil, igal pool juurdepääs laiemas rakenduse tugi, rikkalikke kogemusi, turvalisust ja suurema skaleeritavus. Omakorda Citrix teenusepakkujatele meelitada rohkem kasutajaid, klientide rahulolu ja oma tegevuse kulusid vähendada. [Lisateavet leiate teemast](http://www.citrix.com/products/service-providers.html) või [partneri otsimine](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsofti majutatud teenusepakkuja** 
Majutusteenuse partnerite pakuvad tavaliselt täielikult hallatud majutatud Windowsi töölaua ja rakenduste teenus, mis võib sisaldada Azure ressursse, operatsioonisüsteemide ja rakenduste kasutajatugi, kasutades partneri haldamine kasutaja litsentsid lepinguid Microsofti ja muude koos on teenuse pakkuja litsentsileping nuus abonendi juurdepääs litsentsi (SAL) lubama pakkujad. Järgmine teave pakub üksikasjad ja mõned hosters, et kliendid saavad Azure RemoteApp migreerimise abistamine spetsialiseerunud kontaktteave. Vaadake üle lõpetanud, klõpsake IaaS Õppekeskuse tee ja hindamise RDS [Praegune loetelu majutatud teenusepakkujatele](http://aka.ms/rdsonazurecertified) .  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) on tarkvaratoode üleminekul pilveteenusesse ja ISV "Kas soovite oma praeguse cloud seadistuse optimeerimine. ASPEX pakub laia valikut haldab, devops ja arvamust taotlev teenuste.  

Esmane asukoht: Suurbritannia

Toiming piirkond: Lääne-Euroopa

Partneri olek: [hõbe](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsofti pilveteenuse pakkuja: Jah

Seansi RemoteAppi ja lahenduste pakkumine: Jah, mõlemad

Azure RemoteApp migreerimise lahenduste: Jah, [Lugege lisateavet](https://www.aspex.be/en/azure-remote-apps)

**Kontaktisik:**

- Telefon: +3232202198
- E-posti:[info@aspex.be](mailto:info@aspex.be)
- Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (platvormi nimi: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) on automatiseerimine platvorm seda ettevõtete lihtsustada, optimeerimine ja mastaapimiseks migreerimine ja remote lauaarvutid, remote rakenduste ja taristu Microsoft Azure'i pilves. 

MyCloudIT platvormi vähendab juurutamise aja 95%, Azure'i maksumus 30%, ja viib oma kliendi kogu IT taristu mõned peamised tinditõmmete paari pilve. Partnerite nüüd saate hallata klientide ühe globaalne armatuurlaualt, teenuse lõppkasutajad kogu maailmas nagu kunagi varem ja arendada tulu täiendavate pea kohal või ulatusliku Azure koolituse lisamata.  

Esmane asukoht: Dallase, TX, USA

Toiming piirkond: kogu maailmas

Partneri olek: [kuldmedali](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsofti pilveteenuse pakkuja: Jah

Seansi RemoteAppi ja lahenduste pakkumine: Jah, mõlemad

Azure RemoteApp migreerimise lahenduste: Jah, [Lugege lisateavet](https://mycloudit.com/remote-app-microsoft/)

**Kontaktisik:**
- Brian Garoutte VP Business arenduse

   Telefon: 972-218-0741

   E-post:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) on majutatud töölaua lahendusi, esitada kogu töölaua ja rakenduste ISV kogemusi ehitatakse Microsoft Technology base globaalne kliendi Azure'i ja oma andmekeskuste.

Esmane asukoht: Londoni aja järgi, Suurbritannia; Singapur; Houstoni

Toiming piirkond: kogu maailmas

Partneri olek: kuldmedali

Microsofti pilveteenuse pakkuja: Jah

Seansi RemoteAppi ja lahenduste pakkumine: Jah, mõlemad

Azure RemoteApp migreerimise lahenduste: Jah, [Lugege lisateavet](http://www.acuutech.com/ara-migration/)

**Kontaktisik:**

- Ühendkuningriik:

  5/6 York maja, Langston tee,

  Shrewsbury, Essex IG10 3TQ
  
  Telefon: 44 (0) 20 8502 2155
 
- Singapur:

  100 Cecil tänav, #09-02, 
  
  Maakera, Singapur 069532
  
  Telefon: + 65 6709 4933
 
- Põhja-Ameerika: 

  3601 S. Sandman St.
  
  Komplekti 200, Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>Kas vajate rohkem abi?
Veel on vaja aidata valimine või teil on küsimusi? Abi saamiseks kasutage üht järgmistest meetoditest. 

1.  Saada meile aadressil [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Pöörduge [Azure'i tugi](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Alustuseks on [Azure tugiteenuste juhul](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)avamine.
3.  Helista. [Otsi kohalikku numbrit müük](https://azure.microsoft.com/overview/sales-number/).
