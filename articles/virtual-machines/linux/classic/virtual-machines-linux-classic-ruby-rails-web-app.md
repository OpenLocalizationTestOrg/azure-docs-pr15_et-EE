<properties
    pageTitle="Hosti Ruby, rööpad veebisaidil Linux VM | Microsoft Azure'i"
    description="Häälestamine ja läbiviimine Ruby, rööpad vastavalt veebisaidil Azure Linux virtuaalse masina abil."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby rööpad veebirakenduse on Azure VM

Selle õpetuse näitab, kuidas majutada Ruby, rööpad veebisaidil Azure Linux virtuaalse masina abil.  

Selles õpetuses on kinnitatud, kasutades Ubuntu Server 14.04 LTS. Kui kasutate erinevat Linuxi jaotuse, peate võib muuta rööpad installimise juhiseid.

> [AZURE.IMPORTANT] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [ressursihaldur ja klassikaline](../../../resource-manager-deployment-model.md).  Selles artiklis antakse ülevaade klassikaline juurutamise mudeli abil. Microsoft soovitab, et kõige uue juurutuste ressursihaldur mudeli kasutamine.

## <a name="create-an-azure-vm"></a>Azure'i VM loomine

Alustuseks loomise on Azure VM pildiga Linux.

VM loomiseks saate kasutada Azure klassikaline portaali või Azure käsurea liides (CLI).

### <a name="azure-management-portal"></a>Azure'i haldusportaal

1. [Azure'i klassikaline portaali](http://manage.windowsazure.com) sisse logida
2. Klõpsake nuppu **Uus** > **arvutada** > **virtuaalse masina** > **kiire loomine**. Valige Linux pilt.
3. Sisestage parool.

Pärast VM on ette valmistatud, klõpsake VM nime ja valige **armatuurlaud**. Otsige SSH lõpp-punkti, loendis **SSH üksikasjad**.

### <a name="azure-cli"></a>Azure'i CLI

Järgige [loomine virtuaalse masina töötab Linux][vm-instructions].

Pärast VM on ette valmistatud, saate SSH lõpp-punkti, käivitage järgmine käsk:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installige Ruby rööbastel

1. Kasutage SSH ühenduse VM.

2. SSH seansi, saate kasutada järgmisi käske installida Ruby VM:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Installimine võib kuluda mõni minut. Kui see on lõpule jõudnud, kasutage kinnitamaks, et Ruby on installitud järgmine käsk:

        ruby -v

    See tagastab Ruby, kuhu on installitud versiooni.

3. Järgmise käsu abil saate installida rööpad:

        sudo gem install rails --no-rdoc --no-ri -V

    Kasutage--no-rdoc ja--no-ni lipud vahele installimist dokumentatsiooni kohta, mis on kiirem.
    See käsk tõenäoliselt võtab kaua aega, et käivitada, nii, et lisada -V kuvatakse installi edenemise teabe.

## <a name="create-and-run-an-app"></a>Loomine ja käivitamine rakendus

Olles endiselt sisse logitud SSH, käivitage järgmine käsk:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Käsk [Uus](http://guides.rubyonrails.org/command_line.html#rails-new) loob uue rööpad rakenduse. [Serveri](http://guides.rubyonrails.org/command_line.html#rails-server) käsk käivitatakse rööpad kaasneva WEBrick veebiserver. (Tootmisotstarbeks, soovite tõenäoliselt soovite kasutada eri server, nt ükssarvik või reisija.)

Peaksite nägema väljund sarnaneb järgmisega.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Lõpp-punkti lisamine

1. Avage [Azure'i klassikaline portaali] [ management-portal] ja valige oma VM.

    ![virtuaalse masina loend][vmlist]

2. Valige lehe ülaosas **lõpp-punktid** ja seejärel klõpsake nuppu **+ Lisa lõpp-punkti** lehe allosas.

    ![lõpp-punktid leht][endpoints]

3. **Lõpp-punkti lisamine** dialoogiboksis valige "Lisa autonoomse endpoint" ja noolenuppu **Järgmine** .

    ![uue lõpp-punkti dialoogiboks][new-endpoint1]

3. Dialoogiboksi järgmisel lehel sisestage järgmine teave:

    * **Nimi**: HTTP

    * **Protokoll**: TCP

    * **Avalik PORT**: 80

    * **Privaatne PORT**: 3000

    See loob avaliku pordi 80, mis kuvatakse marsruutida liikluse privaatne porti 3000, kus on listening rööpad server.

    ![uue lõpp-punkti dialoogiboks][new-endpoint]

4. Klõpsake märkeruutu salvestamiseks lõpp-punkti.

5. Tuleks kuvada teade, mis kinnitab **UPDATE EDENEMISEST**. Kui see teade kaob, lõpp-punkti on aktiivne. Nüüd võidakse rakenduse testimiseks liikumine virtual arvuti DNS-i nimi. Veebisaidi peaks kuvatama umbes järgmine:

    ![rööpad vaikeleheks][default-rails-cloud]

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses tegite enamik juhiseid käsitsi. Tootmiskeskkonnas, oleks kirjutada oma rakenduse arengu arvutisse ja selle juurutama Azure VM. Enamik tootmise keskkondades majutada ka rööpad taotluse koos muu serveri protsess nagu Apache või NginX, millised pidemete taotleda marsruutimine mitu eksemplari rööpad taotluse ja serveeritakse staatilise ressursid. Lisateavet leiate teemast http://rubyonrails.org/deploy/.

Ruby on Rails kohta lisateabe saamiseks külastage [Ruby on Rails juhendid][rails-guides].

Leiate Azure'i teenused foneetiline rakenduse kasutamiseks.

* [Struktureerimata andmeid plekid salvestada][blobs]

* [Poe /-väärtuse paarideks tabelite abil][tables]

* [Kõrge läbilaskevõime sisu sisu kohaletoimetamise võrguga][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
