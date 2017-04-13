<properties
    pageTitle="Näide taristu kiirtutvustus | Microsoft Azure'i"
    description="Teavet ja rakendamist suuniseid näide infrastruktuur Azure kasutamise kohta."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Näide Azure'i infrastruktuuri kiirtutvustus

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Selles artiklis tutvustatakse building näide rakenduse infrastruktuur välja. Me üksikasjalikult infrastruktuur lihtsa Interneti poe, mis ühendab kõik juhised ja otsuseid ümber nimereeglid, kättesaadavus komplekti, virtuaalne võrkude ja koormus soolise kujundamise ja tegelikult juurutamine virtuaalmasinates (VMs).


## <a name="example-workload"></a>Näide töökoormus

Adventure Worksi tsüklit soovib koostada online-poest rakenduse Azure, mis koosneb järgmistest elementidest:

- Kaks IIS-i serverid web astme ees kliendi käivitamist
- Kaks IIS-i serverid andmete töötlemise ja tellimused on rakenduse astme
- Kahe Microsoft SQL Server eksemplarid Kättesaadavusrühmad (kahe SQL serveri ja enamik sõlm tunnistaja) talletamiseks andmebaasi taseme toote põhiandmed ja tellimused
- Kaks Active Directory domeenikontrollerid kliendi- ja tarnijate mõne autentimise astme
- Kahe alamvõrku asuvad kõikides serverites:
    - ees alamvõrgu web serverite jaoks 
    - tagaandmebaas alamvõrgu rakenduste serverid, SQL-i kobar ja domeenikontrollerid

![Rakenduse infrastruktuuri erinevatele tasemetele skeem](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Sissetuleva meili turvaline web liiklus peab olema koormusetasakaalustusega web serverid nagu kliendid online poodi sirvida. Tellige töötlemine liikluse HTTP päringuid veebist serverid peavad olema tasakaalus rakenduse serverid kujul. Lisaks peavad infrastruktuuri mõeldud kõrge kättesaadavus.

Selle tulemusena tekkivad kujundus peab sisaldama:

- Azure'i tellimuse- ja kontoteavet
- Ühe ressursirühm
- Salvestusruumi kontod
- Kahe alamvõrku virtuaalse võrgu
- Kättesaadavus määrab vms sarnase rolliga
- Virtuaalmasinates

Kõik ülaltoodud järgige järgmisi nimereeglid:

- Adventure Worksi tsüklit kasutab **[IT töökoormus]-[asukoht]-[Azure ressursi]** eesliide
    - Selle näite puhul "**azos**" (Azure'i ühendusega Store) IT töökoormus nimi ja asukoht "**kasutamine**" (Ida-USA 2) on
- Salvestusruumi kontod kasutada adventureazosusesa**[Kirjeldus]**
    - "adventure" on lisatud eesliite esitada unikaalsuse ja salvestusruumi kontonimed ei toeta sidekriipsu kasutamine.
- Virtuaalne võrkude AZOS-kasutamine-VN**[arv]** kasutamine
- Kättesaadavus komplekti kasutada azos-kasutada-kui-**[roll]**
- Virtuaalse masina nimede kasutamine azos – kasutage-vm -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure'i tellimused ja kontod

Adventure Worksi tsüklit abil oma ettevõtte tellimust, nimega Adventure Works Enterprise tellimusele, sisestage see nii arveldus töökoormus.


## <a name="storage-accounts"></a>Salvestusruumi kontod

Adventure Worksi tsüklit kindlaks määrata, et neil on vaja kahte salvestusruumi kontod:

- **adventureazosusesawebapp** standard talletamist web-server, rakenduse serverite ja domeenikontrollerid ja nende andmete ketast.
- **adventureazosusesasql** Premium Storage SQL serveri VMs ja nende andmete ketast.


## <a name="virtual-network-and-subnets"></a>Virtuaalse võrgu ja alamvõrku

Kuna virtuaalse võrgu ei pea poolelioleva ühenduvuse Adventure töö tsüklit kohapealse võrgu, nad otsustanud vaid virtuaalse võrgus.

Need loodud vaid virtuaalse võrgu Azure'i portaalis järgmised sätted:

- Nimi: AZOS-kasutamine – VN01
- Asukoht: Ida-USA 2
- Virtuaalse võrgu aadressiruumi: 10.0.0.0/8
- Esimese alamvõrgu:
    - Nimi: FrontEnd
    - Aadress: 10.0.1.0/24
- Teine alamvõrgu:
    - Nimi: Taustväärtus
    - Aadress: 10.0.2.0/24


## <a name="availability-sets"></a>Kättesaadavus komplektid

Kõik neli taset online-poest kättesaadavuse säilitamiseks Adventure Works tsüklit otsustanud nelja kättesaadavus komplekti:

- **azos-kasutamine-kui-web** web serverite jaoks
- rakenduste serverid **azos--kui-appi kasutamine**
- **azos-kasutamine-kui-sql** SQL-i serverite jaoks
- **azos – kasutage-kui-AV** domeenikontrollerite


## <a name="virtual-machines"></a>Virtuaalmasinates

Adventure Worksi tsüklit otsustanud oma Azure'i vms järgmised nimed:

- **azos kasutamine – vm web01** esimene veebiserver
- **azos kasutamine – vm web02** teise veebiserver
- **azos kasutamine – vm app01** esimese rakenduse server
- **azos kasutamine – vm app02** teise rakenduse server
- **azos kasutamine – vm sql01** esimese SQL serveri serveri klaster
- **azos kasutamine – vm sql02** teine SQL serveri serveri klaster
- **azos kasutamine – vm dc01** esimese domeenikontrolleri
- **azos kasutamine – vm dc02** teise domeenikontrolleri

Siin on tulemuseks konfiguratsiooni.

![Lõplik taotlus taristu juurutatud Azure](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Selle konfiguratsiooni sisaldab:

- Vaid virtuaalse võrgu kahe alamvõrku (FrontEnd ja kirjutamata)
- Kahe salvestusruumi kontod
- Nelja kättesaadavus komplekti, üks iga taseme online pood.
- Virtuaalmasinates neli taset jaoks
- Mõne välise koormusetasakaalustusega HTTPS-põhise veebi-liikluse Interneti kaudu web serverite jaoks
- On sisemine koormusetasakaalustusega krüptimata web-liikluse rakenduste serverid web serverite jaoks
- Ühe ressursirühm


## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 