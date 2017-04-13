<properties
   pageTitle="Azure virtuaalse masina juurutus, nii et Chef | Microsoft Azure'i"
   description="Saate teada, kuidas peakokk abil teha automaatne virtual juurutus- ja Microsoft Azure konfigureerimine"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Azure virtuaalse masina juurutus, nii et Chef automatiseerimine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

On suurepärane vahend pakkuda automatiseerimine ja soovitud riik konfiguratsioone.

Cloud-api meie uusim versioon Chef pakub sujuvalt integreerimine Azure, võimaldades ettevalmistamine ja juurutamine konfiguratsiooni riigid ühe käsu kaudu.

Selles artiklis ma näitan, kuidas ette Azure'i virtuaalmasinates ja teid läbi loomise poliitika või "Kokaraamat" ja seejärel soovitud Azure virtuaalse masina kasutusele võtavad see kokaraamat Chef keskkonna häälestamine.

Alustagem!

## <a name="chef-basics"></a>Chef põhialused

Enne alustamist, ma soovitada teil läbi vaadata kokk põhimõtted. On suur materjali <a href="http://www.chef.io/chef" target="_blank">siin</a> ja ma soovitame teil on kiire lugeda, enne kui proovite juhendis. Ma juhul sulgege põhitõed, enne kui me.

Järgmisel joonisel on kujutatud üksikasjalik Chef arhitektuur.

![][2]

Peakokk on kolm peamist arhitektuuri komponendid: Chef Server, Chef kliendile (sõlme) ja Chef Workstation.

Chef Server on meie haldamise osas ja on kaks võimalust Chef server: majutatud lahenduse või kohapealse lahenduse. Me kasutame majutatud lahenduse.

Kliendi Chef (sõlme) on agent, mis asub teie hallatava serverites.

Töökoha Chef on meie administraator töökoha, kus me meie poliitikate loomine ja käivitada meie haldamise käske. Võtame käsk **nuga** Chef töökoha meie taristu haldamise kaudu.

Olemas on ka mõiste "Kokaraamatud" ja "Retseptid". Need on tõhus poliitikate me määratleda ja rakendada meie serverites.

## <a name="preparing-the-workstation"></a>Töökoha ettevalmistamine

Esmalt võimaldab töökoha ettevalmistamine. Ma kasutan standard Windows töökoha. Meil on vaja luua meie config failid ja kokaraamatud talletamiseks.

Esmalt looge kaust nimega C:\chef.

Looge teine kaust nimega c:\chef\cookbooks.

Nüüd tuleb meie Azure sätted faili alla laadida, et Chef saaksid suhelda meie Azure'i tellimus.

Laadige oma sätted [siin](https://manage.windowsazure.com/publishsettings/)avaldada.

C:\chef avalda sätete fail salvestada.

##<a name="creating-a-managed-chef-account"></a>Hallatavate Chef konto loomine

Majutatud Chef konto [siin](https://manage.chef.io/signup).

Registreerumise käigus, palutakse teil luua uue organisatsiooni.

![][3]

Kui teie asutus on loodud, laadige starter kit.

![][4]

> [AZURE.NOTE] Kui kuvatakse hoiatus, saate oma klahvid lähtestatakse viip, on kui meil pole veel konfigureeritud infrastruktuuri jätkamiseks ok.

See starter kit zip-fail sisaldab teie ettevõtte config failid ja võtmed.

##<a name="configuring-the-chef-workstation"></a>Töökoha Chef konfigureerimine

Chef starter.zip, et C:\chef sisu ekstraktida.

Kopeerige kõik failid jaotises chef-starter\chef-repo\.koka c:\chef kataloogi.

Kataloogi peaks nüüd välja nägema umbes järgmine näide.

![][5]

Nüüd saate neli faile, sealhulgas Azure avaldamise faili c:\chef juurkaustas.

PEM failid sisaldavad teie ettevõttes ja administraator privaatvõtmete suhtlemiseks knife.rb fail sisaldab teie nuga konfiguratsiooni. Vajame knife.rb faili redigeerimiseks.

Avage fail oma redaktoris ja muuta "cookbook_path", jättes selle /. tabeliks või nii, et see kuvataks, nagu on näidatud järgmisel tee.

    cookbook_path  ["#{current_dir}/cookbooks"]

Lisada ka järgmisi joon, mis kajastab nimi oma Azure avalda sätete fail.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Failis knife.rb peaks sarnanema nüüd järgmises näites.

![][6]

Nende ridade tagab nuga viitab kokaraamatud kataloogi jaotises c:\chef\cookbooks, ja kasutab ka meie Azure'i avalda sätete fail ajal Azure toimingud.

## <a name="installing-the-chef-development-kit"></a>Chef arenduskomplekt installimine

Järgmise [alla laadida ja installida](http://downloads.getchef.com/chef-dk/windows) ChefDK (Chef arenduskomplekt) häälestada oma töökoha Chef.

![][7]

Installige c:\opscode vaikeasukohta. Selle installimiseks kulub umbes 10 minutit.

Kinnitage oma tee muutuja sisaldab kirjete C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Kui nad pole olemas, veenduge, et lisada need teed!

*PANGE TÄHELE, JÄRJESTUST TEE ON OLULINE!* Kui teil on probleeme õiges järjestuses ei ole oma opscode teed.

Taaskäivitage oma töökoha enne jätkamist.

Järgmiseks me installida Azure nuga laiend. See pakub nuga "Azure lisandmooduli".

Käivitage järgmine käsk.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Argument – eelnevalt tagab saabunud nuga Azure'i lisandmoodul, mis annab juurdepääsu uusima API-de määramine RC uusim versioon.

On tõenäoline, et arv sõltuvused installitakse ka samal ajal.

![][8]


Veenduge, et kõik on õigesti konfigureeritud, käivitage järgmine käsk.

    knife azure image list

Kui kõik on õigesti konfigureeritud, kuvatakse saadaval Azure piltide Sirvige loendit.

Palju õnne. Töökoha on häälestatud!

##<a name="creating-a-cookbook"></a>Loomise kokaraamat

Kokaraamat kasutatakse peakokk määratlevad käsud, mida soovite käivitada oma hallatavate klienti. Kokaraamat loomine on lihtne ja kasutame **chef luua kokaraamat** käsk meie kokaraamat malli loomiseks. Ma kutsudes minu kokaraamat veebiserverisse nagu ma poliitika, mis kasutab automaatselt IIS-i.

Klõpsake jaotises C:\Chef kataloogi käivitage järgmine käsk.

    chef generate cookbook webserver

See loob failikogumi Directory C:\Chef\cookbooks\webserver. Nüüd tuleb määratleda käskude soovime meie hallatava virtuaalse masina käivitada kliendi Chef.

Käsud on salvestatud fail default.rb. Selle faili ma kuvatakse määratlemine käskude komplekt, mis installitakse IIS-i, käivitab IIS-i ja kopeerib mallifail wwwroot kausta.

C:\chef\cookbooks\webserver\recipes\default.rb faili muuta ja lisada järgmised read.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Kui olete lõpetanud, salvestage fail.

## <a name="creating-a-template"></a>Malli loomine

Nagu eelnevalt mainitud, läheb vaja luua malli fail, mida kasutatakse default.html lehelt.

Käivitage järgmine käsk luua malli.

    chef generate template webserver Default.htm

Nüüd avage C:\chef\cookbooks\webserver\templates\default\Default.htm.erb fail. Faili redigeerimiseks, lisades mõned lihtsad "Tere, maailm" HTML-i kood ja seejärel salvestage fail.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Chef serverisse üles kokaraamat

Selles etapis tuleb oleme me enda loodud meie kohalikus arvutis kokaraamat koopia tegemine ja üleslaadimine Chef majutatud serveris. Kui laadisite, kuvatakse kokaraamat vahekaardil **poliitika** .

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Virtuaalse masina nuga Azure juurutamine

Nüüd on Azure virtuaalse masina juurutada ja rakendada "Veebiserverile" kokaraamat, mis installitakse meie IIS-i teenuse ja vaikimisi web veebilehele.

Selleks käsu **nuga azure serveri loomine** .

Olen, kuvatakse järgmine käsk näide.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parameetrid on iseenesestmõistetavad. Asendada oma kindla muutujate ja käivitamine.

> [AZURE.NOTE] Kuni selle käsurea, ma olen ka automatiseerimine lõpp-punkti võrgu filtri reeglite kasutades parameetrit – tcp-lõpp-punktid. Veebilehena avatud pordid 80 ja anda juurdepääsu oma veebilehe ja RDP-seansi 3389.

Kui käivitate selle käsu, Azure'i portaalis ja te näete arvuti sätte hakata.

![][13]

Käsuviip, kuvatakse järgmine.

![][10]

Kui juurutamise on lõpule jõudnud, peaks olema me ühenduse veebiteenuse port 80 me oli avada pordi, kui me ette valmistatud virtuaalse masina nuga Azure'i käsk. Selle virtuaalse masina on ainult minu pilveteenuses virtuaalse masina, ma seda ühendust pilvepõhise teenuse URL-i.

![][11]

Nagu näete, mul loominguline minu HTML-koodi.

Ärge unustage ka ühendada kaudu Azure'i klassikaline portaalis pordi 3389 RDP-seanss.

Ma loodame, et see on kasulik! Avage ja alustada oma taristu kood reisi Azure juba täna!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
