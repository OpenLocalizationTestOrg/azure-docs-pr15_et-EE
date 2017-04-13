<properties
    pageTitle="Active Directory Federation Services Azure | Microsoft Azure'i"
    description="Selles dokumendis saate teada, kuidas kasutada Azure AD FS-i kõrge ligipääsetavusele."
    keywords="juurutada Azure AD FS-i, juurutada azure ADFS-i, azure ADFS-i, azure ad FS-i, juurutada ADFS-i, juurutada ad FS-i ADFS-i Azure, juurutada ADFS-i Azure, juurutada azure, ADFS-i azure AD FS-i, Azure AD FS-i Azure'i iaas, ADFS-i, Sissejuhatus AD FS-i ADFS-i Azure teisaldamine"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>Azure'i AD FS-i juurutamine 

AD FS-i pakub lihtsustatud ja turvalise identiteedi ühendamine ja Web ühekordse sisselogimise (SSO) võimalusi. Azure AD domeeniliitu või O365 võimaldab kasutajatel autentida kohapealse mandaadiga ja juurdepääs kõik ressursid pilveteenuses. Selle tulemusena muutub oluline on väga kättesaadav AD FS-i taristu ressursid kättesaadavuse tagamiseks asutusesiseselt ja pilves. Azure AD FS-i juurutamine aitab saavutada püüete minimaalne vajalik kõrge kättesaadavus.
Rakendades Azure AD FS-i annab mitmeid eeliseid, mõned neist on loetletud allpool.

* **Kõrge-saadavus** - Azure'i kättesaadavus komplektid power on tagate väga kättesaadav taristu.
* **Lihtne skaala** -jõudlust on vaja? Hõlpsalt migreerimine võimsam masinad vaid paari klõpsuga Azure
* **Rist-Geo koondamise** – koos Azure Geo koondamise võite olla kindel, et teie taristu on väga saadaval kogu maailmas
* **Lihtne haldamine** -väga lihtsustatud haldamise võimalusi Azure'i portaalis oma taristu haldamise on väga lihtne ja probleemideta 

## <a name="design-principles"></a>Kavandamise põhimõtted

![Juurutamise kujundus](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Ülaltoodud joonisel on soovitatav lihtsa topoloogia alustada oma AD FS-i taristu Azure juurutamine. Erinevate osade topoloogia põhimõtted on järgmised:

* **AV / ADFS-i serverid**: kui teil on vähem kui 1000 kasutajat saate lihtsalt installida AD FS-i rolli teie domeenikontrollerid. Kui te ei soovi domeenikontrollerid mõjutaks jõudlust või kui teil on üle 1000 kasutaja, siis juurutada AD FS-i eraldi serverites.
* **WAP-serveri** – nii, et kasutajad saavad saavutamiseks AD FS-i kui nad pole ettevõtte võrgus ka on vaja Web teenuserakenduse puhverserverite juurutamine.
* **DMZ**: puhverserver serverid paigutatakse DMZ ja ainult TCP/443 juurdepääsu on lubatud DMZ ja sisemise alamvõrgu vahel.
* **Laadi soolise**: AD FS-i ja Web rakenduse puhverserveri serverid kättesaadavuse tagamiseks soovitame kasutada ka sisemise koormuse koormusetasakaalustusteenuse AD FS server ja Azure laadimine koormusetasakaalustusteenuse Web teenuserakenduse puhverserverite.
* **Kättesaadavus komplekti**: esitada koondamise juurutamise AD FS-i abil, on soovitatav, et te rühmitada kahe või enama virtuaalmasinates sarnaseid töökoormus Set saadavus. Selle konfiguratsiooni tagab, et jooksul planeerimata ja kavandatud hooldustööde sündmuse, vähemalt üks virtuaalse masina saab
* **Salvestusruumi kontod**: soovitatav on kaks salvestusruumi kontod. Ühe salvestusruumi konto võib põhjustada tõrke ühtne loomine ja võib põhjustada juurutamiseks muutuvad kättesaamatuks tõenäoline stsenaariumis, kus salvestusruumi konto läheb alla. Kahe salvestusruumi kontod aitab siduda ühe salvestusruumi konto viga iga rea kohta.
* **Võrgu eraldamine**: Web teenuserakenduse puhverserverite tuleb kasutada eraldi DMZ võrku. Saate jagada ühe virtuaalse võrgu kahe alamvõrku ja seejärel klõpsake mõnda neist eraldatud alamvõrgu Web rakenduse puhverserver(ID) juurutamine. Saate lihtsalt iga alamvõrgu jaoks võrgu rühma sätete konfigureerimine ja lubada ainult nõutav suhtlemine kaks alamvõrku. Lisateavet üksikasjad juurutamise stsenaarium allpool kohta

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Juhised Azure AD FS-i juurutamiseks

Selles jaotises kirjeldatud juhiseid liigendamine juurutamiseks juhend on kujutatud AD FS-i taristu Azure all.

### <a name="1-deploying-the-network"></a>1. võrgu juurutamine

Eespool kirjeldatud te saate kas loomine kahe alamvõrku ühe virtuaalse võrgu või muu loomine kahe hoopis virtuaalse võrgu (VNet). See artikkel keskendub juurutamine ühe virtuaalse võrgu ning jagada kahe alamvõrku. See on praegu lihtsam lähenemisviisi nagu kahte eraldi VNets nõuaks on VNet VNet Gateway suhtlus.

**1.1 loomine virtuaalse võrgu**

![Virtuaalse võrgu loomine](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
Azure'i portaalis valige virtuaalse võrgu- ja te saate juurutada virtuaalse võrgu ja ühe alamvõrgu kohe ühe hiireklõpsuga. INT alamvõrgu on määratletud ka ja on nüüd valmis vms lisada.
Järgmiseks on lisada teise alamvõrgu võrku, st DMZ alamvõrgu. DMZ alamvõrku, lihtsalt loomine

* Valige äsja loodud võrgu
* Valige atribuutide alamvõrgu
* Alamvõrgu paneel klõpsake nuppu Lisa
* Luua alamvõrgu alamvõrgu nime ja aadressi ruumi teave

![Alamvõrgu](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Alamvõrgu DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. loomine võrgu turberühmad**

Võrgu turberühma (NSG) sisaldab juurdepääsu juhtimine loend (ACL) reeglid, mis lubada või keelata võrguliikluse virtuaalse võrgu VM eksemplaride loendi. NSGs võib olla seotud alamvõrku või üksikud VM eksemplarid selle alamvõrgu sees. Kui ka NSG on seostatud mõne alamvõrku, ACL reeglite rakendamine VM juhtudel selle alamvõrgu.
Juhised, et loome kaks NSGs: üks sisevõrk ja soovitud DMZ. Need on nimi NSG_INT ja NSG_DMZ vastavalt.

![NSG loomine](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Pärast soovitud NSG on loodud, on 0 sissetulevate ja 0 väljamineva reegleid. Kui rollid vastavates serverites installitakse ja funktsionaalne, siis sissetulevate ja väljaminevate reegleid saab teha vastavalt turvalisuse soovitud taset.

![NSG lähtestamine](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Pärast selle NSGs on loodud, saate seostada alamvõrgu NSG_INT INT ja NSG_DMZ alamvõrgu DMZ abil. Mõni näide kuvatõmmis on esitatud allpool:

![NSG konfigureerimine](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klõpsake paani jaoks alamvõrku avamiseks alamvõrku
* Valige soovitud NSG seostada alamvõrgu 

Pärast konfiguratsioon alamvõrku paneel peaks välja nägema all.

![Pärast NSG alamvõrku](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. ühenduse loomiseks kohapealse**

Kohapealse ühenduse peame juurutamiseks Azure domeenikontrolleri (näiteks Põhiliselt). Azure'i pakub oma kohapealse taristu ühenduse loomiseks oma Azure'i infrastruktuuri ühenduvuse Valikud.

* Punkti – saidile
* Virtuaalse võrgu saidilt
* ExpressRoute

Soovitatav on kasutada ExpressRoute. ExpressRoute võimaldab teil luua privaatne seoseid Azure andmekeskuste ja infrastruktuur, mis on oma ettevõttes või kaasautorluse kohta keskkonnas. Avaliku Interneti kaudu ei lähe ExpressRoute ühendused. Nad pakuvad rohkem töökindlust, kiirem kiirust, lower latentsused ja suurem turvalisus, kui tüüpilised ühendused Interneti kaudu.
Kuigi on soovitatav kasutada ExpressRoute, võite kõik teie asutuse jaoks kõige paremini ühendusviisi. Lisateavet ExpressRoute ja abil ExpressRoute ühenduvuse erinevate suvandite kohta teabe saamiseks lugege [ExpressRoute tehniline ülevaade](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2 salvestusruumi kontode loomine

Kõrge-saadavus säilitamiseks ja vältimine ühe salvestusruumi konto, saate luua kaks salvestusruumi kontod. Jagage masinad kättesaadavus igas kogumis kahte rühma ja seejärel määrake iga rühma eraldi salvestusruumi konto.

![Salvestusruumi kontode loomine](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. kättesaadavus komplekti loomine

Iga rolli (näiteks Põhiliselt/AD FS-i ja WAP), luua kättesaadavus komplektid sisaldavad 2 masinad vähemalt. See aitab saavutada suurema saadavus iga rolli. Kättesaadavus loomine seab, on oluline otsustada järgmist:
* **Viga domeenide**: virtuaalmasinates sama viga domeeni jagada sama power lähte-kui ka füüsilise võrgu aktiveerimine. Vähemalt 2 viga domeenid on soovitatav. Vaikeväärtus on 3 ja jätta välja nagu on selleks, et see juurutamine
* **Värskendage domeeni**: masinad kuuluvad sama update domain taaskäivitada koos ajal värskendust. Soovite vähemalt 2 update domeenid. Vaikeväärtus on 5 ja jätta välja nagu on selleks, et see juurutamine

![Kättesaadavus komplektid](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Järgmised kättesaadavus komplekti loomine

| Kättesaadavus määramine | Roll | Viga domeenid | Värskendage domeeni |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | NÄITEKS PÕHILISELT/ADFS-I | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. virtuaalmasinates juurutamine
Järgmiseks on erinevad rollid oma infrastruktuuri majutavad virtuaalmasinates juurutamine. Kättesaadavus igas kogumis on soovitatav vähemalt kaks masinat. Saate luua lihtsa juurutamiseks kuus virtuaalmasinates.

| Seadme | Roll | Alamvõrgu | Kättesaadavus määramine | Salvestusruumi konto | IP-aadress |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|NÄITEKS PÕHILISELT/ADFS-I|INT|contosodcset|contososac1|Staatiline|
|contosodc2|NÄITEKS PÕHILISELT/ADFS-I|INT|contosodcset|contososac2|Staatiline|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Staatiline|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Staatiline|

Kui te olete märganud, pole NSG on määratud. See on azure võimaldab kasutada NSG alamvõrgu tasemel. Seejärel saate määrata, kasutades üksiku NSG, kas alamvõrgu või muu objekti NIC seotud arvuti võrguliiklust. Lugeda Lisateavet sees [mis on võrgu turvalisus jaotises (NSG)](https://aka.ms/Azure/NSG).
Staatiline IP-aadress on soovitatav, kui haldate DNS-i. Saate kasutada Azure DNS-i ja selle asemel teie domeeni DNS-i kirjeid, viidata uued masinad oma Azure'i FQDN-i abil.
Oma virtuaalse masina paani peaks välja nägema all pärast juurutamise on lõpule viidud.

![Juurutatud Virtuaalmasinates](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. selle domeenikontrolleri seadistamine / AD FS server
 Selleks, et sissetulevad taotlused autentida, tuleb pöörduda selle domeenikontrolleri AD FS-i. Azure'i kulukas reisi salvestamiseks kohapealse AV autentimine on soovitatav juurutada Azure domeenikontrolleri koopia. Kõrge-saadavus saavutamiseks on soovitatav luua ka kättesaadavus hulka kuuluvad at-vähemalt 2 domeeni.

|Selle domeenikontrolleri|Roll|Salvestusruumi konto|
|:-----:|:-----:|:-----:|
|contosodc1|Koopia|contososac1|
|contosodc2|Koopia|contososac2|

* Saab reklaamida kahe koopia domeenikontrolleritena DNS-serverid
* Konfigureerimine AD FS-i serverid, installides AD FS server halduriga roll.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. rakendades sisemise koormuse koormusetasakaalustusteenuse (ILB)

**6.1. selle ILB loomine**

Mõne ILB juurutada, valige koormus soolise Azure portaali ja klõpsake lisamisnuppu (+).
>[AZURE.NOTE] kui te ei näe menüü **Koormus soolise** , klõpsake nuppu **Sirvi** portaali ja kerige vasakus allnurgas seni, kuni näete **Koormus soolise**.  Klõpsake selle lisamiseks oma menüü Kollane täht. Nüüd valige uue laadi koormusetasakaalustusteenuse ikoon laadi koormusetasakaalustusteenuse konfigureerimine alustamiseks paani avamiseks.

![Laadi koormusetasakaalustusteenuse sirvimine](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Nimi**: pange nimi, mis tahes sobiva laadi koormusetasakaalustusteenuse
* **Värviskeemi**: Kuna seda laadi koormusetasakaalustusteenuse paigutatakse AD FS-i serverid ette ja on mõeldud ainult sisemise võrguühenduste saate valida "Internal"
* **Virtuaalse võrgu**: kui juurutate AD FS-i virtuaalse võrgu valimine
* **Alamvõrgu**: valige sisemise alamvõrgu siin
* **IP-aadress ülesande**: dünaamiline

![Sisemise koormuse koormusetasakaalustusteenuse](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Pärast nupu Loo ja selle ILB juurutatakse, peaks kuvatakse loendis koormus soolise:

![Pärast ILB koormus soolise](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Järgmise sammuna tuleb konfigureerida taustväärtus pool ja taustväärtus juures.

**6.2. ILB kirjutamata rakenduskausta konfigureerimine**

Valige äsja loodud ILB koormus soolise paanil. Rühmasätete paneel avaneb. 
1.  Valige taustväärtus kaustu Rühmasätete paneel
2.  Klõpsake lisa kirjutamata rakenduskausta paanil lisa virtual arvutisse
3.  Ilmub koos kuvatakse paanil, kus saate valida kättesaadavus määramine
4.  Määrake AD FS-i kättesaadavus

![ILB kirjutamata rakenduskausta konfigureerimine](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. juures konfigureerimine**

Valige paneelil ILB sondid.
1.  Klõpsake lisamine
2.  Üksikasjade pakkumine juures on. **Nimi**: Probe nime b. **Protokoll**: TCP c. **Port**: d 443 (HTTPS). **Intervall**: 5 (vaikeväärtus) – see on intervall, mille ILB sondi masinad kirjutamata rakenduskausta e. **Vigane limiit**: 2 (vaikimisi val ue) – see on lävi järjestikust juures tõrkeid, pärast mida ILB kuvatakse deklareerida masina taustväärtus pool – reageeri ja peatage liikluse saata.

![ILB juures konfigureerimine](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. Laadi tasakaalustamiseks reeglite loomine**

Liiklus tõhus tasakaalustamiseks peaks olema konfigureeritud selle ILB koormusega tasakaalustamiseks reeglid. Selleks, et luua reegli, tasakaalustamiseks koormus 
1.  Valige laadi tasakaalustamiseks reegel on ILB paanilt sätted
2.  Klõpsake nuppu Lisa reegel paani tasakaalustamiseks koormus
3.  Lisa koormus tasakaalustamiseks reegli paneel lisamine. **Nimi**: b reegli nimi. **Protocol (protokoll)**: valige TCP c. **Port**: 443 d. **Taustväärtus port**: 443 e. **Taustväärtus pool**: valige jaoks AD FS-i klaster varasemates f loodud kausta. **Juures**: valige juures, mis on varem loodud AD FS server

![Reeglite tasakaalustamiseks ILB konfigureerimine](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. värskendada DNS-i ILB**

Minge oma DNS-serveri ja luua ka CNAME soovitud ILB. CNAME-i peaks olema federation teenuse osutab selle ILB IP-aadress IP-aadressiga. Näiteks kui ILB DIP aadress on 10.3.0.8 ja installitud federation teenus on fs.contoso.com, siis luua ka CNAME osutab 10.3.0.8 fs.contoso.com jaoks.
See tagab, et kõik fs.contoso.com lõpetamise teatise juures olevat ILB häälestamine ja marsruuditakse õigesti.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. Web rakenduse puhverserveri konfigureerimine

**7.1. Web teenuserakenduse puhverserverite AD FS-i serverid jõuda konfigureerimine**

Web teenuserakenduse puhverserverite jõudnud soovitud ILB taha AD FS-i serverid tagamiseks looge kirje on %systemroot%\system32\drivers\etc\hosts jaoks soovitud ILB. Pange tähele, et Eraldusnimi (DN) peaks olema federation teenuse nimi, näiteks fs.contoso.com. Ja IP-kirje tuleks selle ILB IP-aadress (nagu näites 10.3.0.8).

**7.2. installimisel Web rakenduse puhverserveri roll**

Kui teil tagada, et Web teenuserakenduse puhverserverite on jõudnud AD FS-i serverid ILB taga, saate installida järgmine Web teenuserakenduse puhverserverite. Web teenuserakenduse puhverserverite ühendatud domeeniks. Web rakenduse puhverserveri rollide installimine kahe Web teenuserakenduse puhverserverite, valides Kaugpöördusteenuse roll. Server manager juhendab teid WAP installimise lõpuleviimiseks.
WAP juurutamise kohta lisateabe saamiseks lugege [installima ja konfigureerima Web rakenduse puhverserver](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. Interneti-ühendus (avalik) laadi koormusetasakaalustusteenuse vastastikuste juurutamine

**8.1. Interneti vastastikuste (avalik) laadi koormusetasakaalustusteenuse loomine**
 
Azure'i portaalis valige koormus soolise ja seejärel klõpsake Lisa. Sisestage paanil loomine laadi koormusetasakaalustusteenuse järgmine teave
1. **Nimi**: Laadi koormusetasakaalustusteenuse nimi
2. **Värviskeemi**: avaliku – see suvand Azure'i ütleb, et peate selle laadi koormusetasakaalustusteenuse avaliku aadress.
3. **IP-aadress**: looge uus IP-aadress (dünaamiline)

![Internet suunatud laadi koormusetasakaalustusteenuse](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Pärast kasutuselevõttu, kuvatakse loendis laadi soolise laadi koormusetasakaalustusteenuse.

![Laadi koormusetasakaalustusteenuse loend](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. määrata DNS-i sildi avaliku IP**

Klõpsake vastloodud laadi koormusetasakaalustusteenuse kirje laadi soolise paneeli avab paani konfigureerimise kohta. Järgige allpool juhised avaliku IP sildi DNS-i konfigureerimine.
1.  Klõpsake avaliku IP-aadressi. See avatakse paneeli avaliku IP-ja selle sätteid
2.  Klõpsake konfigureerimine
3.  Sisestage DNS-i silt. See muutub avaliku DNS-i silt, millele pääsete juurde igalt poolt, nt contosofs.westus.cloudapp.azure.com. Saate lisada federation teenuse (nt fs.contoso.com), mille lahenduseks DNS-i silt välise koormuse koormusetasakaalustusteenuse (contosofs.westus.cloudapp.azure.com) välise DNS-i kirje.

![Internet vastastikuste laadi koormusetasakaalustusteenuse konfigureerimine](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Vastastikuste laadi koormusetasakaalustusteenuse (DNS) Interneti konfigureerimine](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Interneti vastastikuste (avaliku) laadi koormusetasakaalustusteenuse konfigureerimine kirjutamata pool** 

Järgige samu juhiseid nagu luua sisemise koormuse koormusetasakaalustusteenuse, konfigureerima kirjutamata rakenduskausta jaoks Internet vastastikuste (avaliku) laadimine koormusetasakaalustusteenuse seadmine WAP serverid kättesaadavus. Näiteks contosowapset.

![Taustväärtus kogumi Internet vastastikuste laadi koormusetasakaalustusteenuse konfigureerimine](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8.4. juures konfigureerimine**

Järgige samu juhiseid nagu juures WAP serverid taustväärtus kogumi jaoks konfigureerida sisemise koormuse koormusetasakaalustusteenuse konfigureerimine.

![Juures, Interneti vastastikuste laadi koormusetasakaalustusteenuse konfigureerimine](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. Laadi tasakaalustamiseks reeglite loomine**

Järgige samu juhiseid nagu ILB laadi tasakaalustamiseks reegli TCP 443 jaoks konfigureerida.

![Tasakaalustavad reeglid Internet vastastikuste laadi koormusetasakaalustusteenuse konfigureerimine](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. võrgu turvamine

**9.1. sisemise alamvõrgu tagamine**

Üldiselt on vaja järgmisi reegleid tõhus tagada teie sisemise alamvõrku (selles järjestuses, nagu allpool)

|Reegli|Kirjeldus|Voog|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| Lubada HTTPS-i side DMZ | Sissetulev |
|DenyInternetOutbound| Puudub juurdepääs internet | Väljamineva meili |

![INT juurdepääsureeglid (Sissetuleva)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[kommentaar]: <> (![INT juurdepääsureeglid (Sissetuleva)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [kommentaar]: <> (![INT juurdepääsureeglid (väljaminev)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. DMZ alamvõrgu turvamine**

|Reegli|Kirjeldus|Voog|
|:----|:----|:------:|
|AllowHTTPSFromInternet| Lubage Internet HTTPS DMZ | Sissetulev|
|DenyInternetOutbound|  Midagi peale HTTPS Interneti on blokeeritud | Väljamineva meili |

![EXT juurdepääsureeglid (Sissetuleva)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[kommentaar]: <> (![EXT juurdepääsureeglid (Sissetuleva)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [kommentaar]: <> (![EXT juurdepääsureeglid (väljaminev)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Kui kliendi kasutaja serdi autentimine (clientTLS autentimist kasutades X509 kasutaja serdid) on nõutav, siis AD FS-i jaoks on vaja TCP pordi 49443 sissetuleva juurdepääs lubatud.

###<a name="10--test-the-ad-fs-sign-in"></a>10. test AD FS-i sisselogimine

Lihtsaim viis on IdpInitiatedSignon.aspx lehel on testimiseks AD FS-i. Selleks, et teha, et see on nõutav IdpInitiatedSignOn AD FS-i atribuutide kohta. Järgige allpool AD FS-i häälestuse kontrollimine
1.  Käivitage selle all cmdlet AD FS server, määrake selleks lubatud PowerShelli abil.
    Set-AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  Mis tahes seadme välise juurdepääsu https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx kaudu  
3.  Peaksite nägema AD FS-i lehe nagu allpool:

![Sisselogimislehe test](./media/active-directory-aadconnect-azure-adfs/test1.png)

Eduka sisselogimine, klõpsake seda annab teile edu sõnum nagu allpool näidatud:

![Katse õnnestumise](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Malli Azure AD FS-i kasutamise kohta

Mall kasutab 6 seadme häälestamise, 2 domeenikontrollerid, AD FS-i ja WAP.

[Azure'i juurutamise malli AD FS-i](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Saate kasutada olemasolevat virtuaalse võrku või luua uue VNET juurutamisel selle malli. Erinevate parameetrite kohandamise juurutamise saadaval on loetletud allpool juurutamise käigus parameetri kasutamist kirjeldust. 

| Parameetri | Kirjeldus |
|:--------|:-----|
|Asukoht| Piirkonna juurutamiseks ressursside sisse, nt Ida US. |
|StorageAccountType| Loodud salvestusruumi konto tüüp|
|VirtualNetworkUsage| Näitab, et kui uue virtuaalse võrgu luuakse või olemasoleva kasutamiseks|
|VirtualNetworkName| Virtuaalse võrgu loomine nii olemasolevad või uued virtuaalse võrgu kasutamine kohustuslik nimi|
|VirtualNetworkResourceGroupName| Saate määrata ressursirühm, kus asub olemasoleva virtuaalse võrgu nimi. Mõne olemasoleva virtuaalse võrgu kasutamisel see muutub kohustuslik parameeter, nii et juurutamise saate olemasoleva virtuaalse võrgu ID leidmine|
|VirtualNetworkAddressRange| Uue VNET, kui virtuaalse uue võrgu loomine kohustuslik aadress vahemikus|
|InternalSubnetName| Sisemise alamvõrku, kohustuslik nii virtuaalse võrgu kasutus suvandid (uude või olemasolevasse) nimi|
|InternalSubnetAddressRange| Aadress vahemik sisemise alamvõrgu, mis sisaldab domeenikontrollerid ja ADFS-i serverid, kohustuslik, kui virtuaalse uue võrgu loomine.|
|DMZSubnetAddressRange| Aadress vahemik dmz alamvõrgu, mis sisaldab Windowsi rakenduse puhverserveri serverid, kohustuslik, kui virtuaalse uue võrgu loomine.|
|DMZSubnetName| Sisemise alamvõrku, kohustuslik nii virtuaalse võrgu kasutus suvandid (uude või olemasolevasse) nimi. |
|ADDC01NICIPAddress| Sisemise IP-aadressi esimese domeenikontrolleri, IP-aadress määratakse staatiliselt näiteks Põhiliselt ja peab olema lubatud ip-aadress, sisemine alamvõrgu sees|
|ADDC02NICIPAddress| Sisemise IP-aadressi teise domeenikontrolleri, IP-aadress määratakse staatiliselt näiteks Põhiliselt ja peab olema lubatud ip-aadress, sisemine alamvõrgu sees|
|ADFS01NICIPAddress| Sisemise IP-aadress esimene ADFS-i server, IP-aadress määratakse staatiliselt ADFS-i serverisse ja peab olema lubatud ip-aadress, sisemine alamvõrgu sees|
|ADFS02NICIPAddress| Sisemise IP-aadress teine ADFS-i server, IP-aadress määratakse staatiliselt ADFS-i serverisse ja peab olema lubatud ip-aadress, sisemine alamvõrgu sees|
|WAP01NICIPAddress| Sisemise IP-aadress esimene WAP server, IP-aadress määratakse staatiliselt WAP-serveri ja peab olema lubatud ip-aadress, alamvõrgu DMZ sees|
|WAP02NICIPAddress| Sisemise IP-aadress teine WAP server, IP-aadress määratakse staatiliselt WAP-serveri ja peab olema lubatud ip-aadress, alamvõrgu DMZ sees|
|ADFSLoadBalancerPrivateIPAddress| ADFS-i laadi koormusetasakaalustusteenuse sise IP IP-aadress määratakse staatiliselt koormusetasakaalustusteenuse laadimine ja peab olema lubatud ip-aadress, sisemine alamvõrgu sees|
|ADDCVMNamePrefix| Virtuaalse masina nimi eesliite domeenikontrollerid|
|ADFSVMNamePrefix| Virtuaalse masina nimi eesliite ADFS-i serverite jaoks|
|WAPVMNamePrefix| Virtuaalse masina nimi eesliite WAP serverite jaoks|
|ADDCVMSize| Selle hulka kuuluvad domeeni vm suurus|
|ADFSVMSize| ADFS-i serverid vm suurus|
|WAPVMSize| WAP serverid vm suurus|
|AdminUserName| Kohalik administraator on virtuaalmasinates nimi|
|AdminPassword| Virtuaalne masinad kohaliku administraatorikonto parooli|

## <a name="additional-resources"></a>Lisaressursid
* [Kättesaadavus komplektid](https://aka.ms/Azure/Availability ) 
* [Azure'i laadi koormusetasakaalustusteenuse](https://aka.ms/Azure/ILB)
* [Sisemise koormuse koormusetasakaalustusteenuse](https://aka.ms/Azure/ILB/Internal)
* [Interneti-ühendusega laadi koormusetasakaalustusteenuse](https://aka.ms/Azure/ILB/Internet)
* [Salvestusruumi kontod](https://aka.ms/Azure/Storage )
* [Azure'i](https://aka.ms/Azure/VNet)
* [AD FS-i ja Web rakenduse puhverserveri lingid](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Järgmised sammud

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
* [Konfigureerimise ja haldamise oma AD FS-i Azure'i AD-ühenduse kaudu](active-directory-aadconnectfed-whatis.md)
* [Kõrge-saadavus-geograafiliste AD FS-i juurutamist Azure Azure'i liikluse haldur](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




