<properties
    pageTitle="Installige Windows VM MongoDB | Microsoft Azure'i"
    description="Saate teada, kuidas installida Azure VM, mis töötab Windows Server 2012 R2 loodud mudeliga ressursihaldur juurutamise MongoDB."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installige ja konfigureerige MongoDB Windows Azure VM
[MongoDB](http://www.mongodb.org) on populaarsed avatud lähtekoodi, suure jõudlusega NoSQL andmebaas. Selles artiklis juhatab teid läbi installimine ja konfigureerimine MongoDB Azure'i Windows Server 2012 R2 virtuaalse masina (VM). Samuti saate [installida Linux VM Azure MongoDB](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Eeltingimused

Enne installimist ja konfigureerimist MongoDB, peate luua VM ja ideaalvariandis mitte lisada andmed kettale. Lugege järgmisi artikleid luua VM ja lisada andmed kettale:

- [Loo Windows Server VM abil Azure portaali](virtual-machines-windows-hero-tutorial.md) või [Looge Windows Server VM Azure PowerShelli abil](virtual-machines-windows-ps-create.md)
- [Manusta andmed kettale Windows Server VM kasutades Azure portaali](virtual-machines-windows-attach-disk-portal.md) või [manustamine andmed kettale Windows Server VM Azure PowerShelli kaudu](https://msdn.microsoft.com/library/mt603673.aspx)
    
Installimine ja konfigureerimine MongoDB, [logige sisse oma Windows Server VM](virtual-machines-windows-connect-logon.md) kaugtöölaua alustada.


## <a name="install-mongodb"></a>Installige MongoDB

> [AZURE.IMPORTANT] Autentimis- ja IP-aadressi sidumine, nt MongoDB turbefunktsioonid on vaikimisi lubatud. Enne juurutamist MongoDB tootmiskeskkonda peaks olema lubatud turbefunktsioonid. Lisateavet leiate teemast [MongoDB turvalisus ja autentimine](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Pärast ühendatud teie VM kaugtöölaua kasutamise, avage Internet Explorer VM menüü **Start** kaudu.

2. Valige **Turvalisus, privaatsus ja ühilduvus sätted soovitatav kasutada** Internet Exploreri esmakordsel avamisel ja klõpsake nuppu **OK**.

3. Internet Exploreri täiustatud turvalisuse konfiguratsioon on vaikimisi lubatud. Lubatud saitide loendi MongoDB veebisaidi lisamiseks tehke järgmist.

    - Valige paremas ülanurgas ikooni **Tööriistad** .
    - **Interneti-suvandid**, klõpsake vahekaarti **Turvalisus** ja valige ikooni **Usaldusväärsed kohad** .
    - Klõpsake nuppu **saidid** . Lisage _https://\*. mongodb.org_ loendisse usaldusväärsed saidid ja seejärel sulgege dialoogiboks.

    ![Internet Exploreri turvalisus sätete konfigureerimine](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Liikuge lehele [MongoDB – allalaadimine](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Vaikimisi märkida **Ühenduse Server** edition ja praeguse ühed uusimaks jaoks Windows Server 2008 R2 64-bitine ja uuemad versioonid. Installiprogrammi allalaadimiseks klõpsake nuppu **Laadi alla (msi)**.

    ![MongoDB Installeri allalaadimine](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Kui allalaadimine on lõpule jõudnud, käivitage installer.

6. Lugege läbi ja nõustuge litsentsilepinguga. Kui teilt küsitakse, valige **Complete** installi.

7. Lõplik kuval klõpsake nuppu **Installi**.


## <a name="configure-the-vm-and-mongodb"></a>VM ja MongoDB konfigureerimine

1. Tee muutujate MongoDB installer ei värskendata. Funktsiooni MongoDB ilma `bin` oma tee muutuja asukohta, peate määrama täielik tee iga kord, kui kasutate MongoDB käivitatava. Teie tee muutuja asukoha lisamiseks järgmist.

    - Paremklõpsake menüü **Start** ja valige **süsteem**.
    - Klõpsake nuppu **Täpsemad sätted süsteem**ja klõpsake **Keskkonna muutujate**.
    - Jaotises **System muutujate**, valige **tee**ja seejärel klõpsake nuppu **Redigeeri**.

    ![TEE muutujate konfigureerimine](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Tee lisamine oma MongoDB `bin` kausta. MongoDB on tavaliselt installitud `C:\Program Files\MongoDB`. Kontrollige oma VM Installitee. Järgmises näites liidetakse vaikimisi MongoDB installida asukoht, kuhu soovite selle `PATH` muutuja:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Veenduge, et lisada ees semikooloniga (`;`) näitamaks, et lisate asukoht, kuhu soovite oma `PATH` muutujana.

2. Luua oma andmete asuvate MongoDB andmete ja logide kataloogide. Menüü **Start** , valige **Käsuviip**. Järgmised näited looge kataloogid f:

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Järgmine käsk, reguleerides andmete tee MongoDB seansi käivitamine ja logige kataloogide vastavalt sellele.

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Võib kuluda mitu minutit MongoDB eraldada töölehe failid ja alustada listening ühenduste jaoks. Kõik Logi sõnumid suunatakse *F:\MongoLogs\mongolog.log* fail nimega `mongod.exe` server käivitub ja eraldab töölehe failid.

    > [AZURE.NOTE] Käsuviip jääb aktiivse selle tööülesande oma MongoDB eksemplari töötamise ajal. Jätke käsuviiba aken avatuks, MongoDB jätkata. Või installida MongoDB teenus, nagu on järgmise juhise juurde.

4. Installige senisest käepärasemad MongoDB kogemuse saamiseks selle `mongod.exe` teenust. Loomine teenuse tähendab, ei pea te jätke töötab iga kord, kui soovite kasutada MongoDB. Looge teenuse järgmiselt, muuta oma andmete ja logide kataloogide tee vastavalt vajadusele.

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Eelmise käsu loob teenuse MongoDB, nimega "Mongo DB" kirjeldus. Järgmiste parameetrite abil, on ka määratud:

    - Funktsiooni `--dbpath` suvand määrab andmete kataloogi asukoht.
    - Funktsiooni `--logpath` suvandit tuleb kasutada määramiseks logifaili, kuna töötab teenus pole väljundisse kuvamiseks käsuviiba aken.
    - Funktsiooni `--logappend` suvand määrab, et teenuse taaskäivitamist põhjustab väljundi lisamiseks olemasoleva logifaili.

  MongoDB teenuse käivitamine, käivitage järgmine käsk:

    ```
    net start MongoDB
    ```

    MongoDB teenuse loomise kohta leiate lisateavet teemast [konfigureerimine MongoDB teenust Windows](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testige MongoDB eksemplari

Abil MongoDB ühekordsest töötab või kui teenus on installitud, saate kohe alustada loomise ja kasutamise oma andmebaasi. MongoDB haldus shell alustamiseks teise käsuviiba aken avamine menüü **Start** ja sisestage järgmine käsk:

```
mongo  
```

Saate loetleda andmebaase, kus on `db` käsk. Lisage mõned andmed järgmiselt:

```
db.foo.insert( { a : 1 } )
```

Andmeid otsida järgmiselt:

```
db.foo.find()
```

Väljund sarnaneb järgmises näites:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Väljumine on `mongo` konsooli järgmiselt:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfigureerige tulemüür ja võrgu turberühma reeglid
Nüüd, kui MongoDB on installitud ja käivitatud, avage port Windowsi tulemüüri nii, et kaugühenduse teel saate luua ühenduse MongoDB. TCP port 27017 lubamiseks uue sissetuleva reegli loomiseks avage haldus PowerShelli Käsuviip ja sisestage järgmine käsk:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

**Täiustatud turvalisusega Windowsi tulemüür** graafiline halduse tööriista abil saate luua ka reegel. TCP port 27017 lubamiseks uue sissetuleva reegli loomiseks.

Vajadusel väljaspool olemasoleva Azure virtuaalse alamvõrku MongoDB kaudu juurdepääsu võrgu turberühma reegli loomine. [Azure portaali](virtual-machines-windows-nsg-quickstart-portal.md) või [Azure PowerShelli](virtual-machines-windows-nsg-quickstart-powershell.md)abil saate luua reeglid võrgu turberühma. Windowsi tulemüüri reegleid, sarnaselt luba TCP port 27017 oma MongoDB VM virtuaalse võrgu liides.

> [AZURE.NOTE] TCP port 27017 on kasutatud MongoDB vaikeport. Saate muuta selle pordi abil soovitud `--port` käivitamisel parameetrite `mongod.exe` käsitsi või teenust. Kui muudate pordi, veenduge, et värskendada Windowsi tulemüür ja võrgu turberühma reeglid eelmist toimingut.


## <a name="next-steps"></a>Järgmised sammud
Selles õpetuses saate teada, kuidas installida ja konfigureerida MongoDB oma Windows VM. Nüüd pääsete MongoDB oma Windows VM, järgides täpsemalt teemade [MongoDB dokumentatsiooni](https://docs.mongodb.com/manual/).
