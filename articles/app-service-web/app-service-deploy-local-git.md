<properties
    pageTitle="Azure'i rakendust Service kohaliku Git juurutamine"
    description="Saate teada, kuidas lubada kohaliku Git juurutamise Azure'i rakenduse teenusega."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Kohaliku Git juurutamise Azure rakenduse teenusega

Selle õpetuse näidatakse, kuidas juurutamiseks [Azure'i rakendust Service] rakenduse Git hoidla kohalikus arvutis. Teenuse rakendus toetab seda moodust suvandiga **Kohaliku Git** juurutamise [Azure portaali].  
Paljude Git käskude selles artiklis kirjeldatud tehakse automaatselt loomisel rakenduse teenuse rakendus, kasutades [Azure käsurea liides] kirjeldatud [allpool](app-service-web-get-started.md).

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks vajate:

- Git. Saate alla laadida soovitud installi kahendarvu [siin](http://www.git-scm.com/downloads).  
- Põhilised teadmised Git.
- Microsoft Azure'i konto. Kui teil pole kontot, saate [tasuta prooviversiooni kasutajaks](https://azure.microsoft.com/pricing/free-trial) või [aktiveerida oma Visual Studio abonendi eelised](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter rakendus App teenuses. Nõutav; krediitkaardid kohustusi.  

## <a name="Step1"></a>Samm 1: Luua kohaliku hoidla

Tehke uue Git hoidla loomiseks järgmist.

1. Käivitage käsurea tööriista, nt **GitBash** (Windows) või **Bash** (Unix Shell). Operatsioonisüsteemides OS X pääsete käsurea **terminalis** rakenduse kaudu.

2. Liikuge kausta, kus juurutada sisu asuks.

3. Kasutage lähtestamine uue Git hoidla järgmine käsk:

        git init

## <a name="Step2"></a>Samm 2: Kinnita sisu

Teenuse rakendus toetab rakenduste loonud erinevaid programmeerimise keeled. 

1. Kui juba on oma andmebaasi sisu Jäta see käsk ja teisaldamine alltoodud punkti 2. Kui teie hoidla juba mittesisaldav sisu lihtsalt asustada staatilise .html faili järgmiselt: 

    - Kasutades tekstiredaktoris, nimega **index.html** Git hoidla juurtasemel uue faili loomine
    - Sisu jaoks index.html fail ja salvestage see lisada järgmine tekst: *Tere Git!*
        
2. Käsurea, veenduge, et teil on oma Git hoidla juurkaustas. Failide lisamiseks oma andmebaasi tehke järgmine käsk:

        git add -A 

4. Järgmiseks Kinnitage muudatused hoidla abil järgmine käsk:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Samm 3: Luba rakenduse teenuse rakenduse hoidla

Järgmiste toimingute lubamiseks Git hoidla oma rakenduse teenuse rakenduse.

1. [Azure'i portaali]sisse logida.

2. Rakenduse teenuse rakenduse tera, klõpsake **Sätted > juurutamise andmeallika**. Klõpsake **Andmeallika valimine**ja seejärel klõpsake **Kohaliku Git hoidla**ja seejärel klõpsake nuppu **OK**.  

    ![Kohaliku Git hoidla](./media/app-service-deploy-local-git/local_git_selection.png)

3. Kui see on teie esimest korda häälestamise hoidla Azure, peate selle jaoks looma sisselogimisteave. Nende abil saab Azure hoidla ja tõuketeatised muudatusi teie kohaliku Git hoidla sisse logida. Oma rakenduse tera, klõpsake nuppu **Sätted > juurutamise identimisteabe**, siis konfigureerimine oma juurutamise kasutajanime ja parooliga. Kui olete lõpetanud, klõpsake nuppu **Salvesta**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Samm 4: Oma projekti juurutamine

Järgmiste juhiste abil saate avaldada oma rakenduse App teenuse abil kohaliku Git.

1. Oma rakenduse tera Azure'i portaalis, klõpsake **Sätted > Atribuudid** **Git URL-i**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Git URL** on teie kohaliku hoidla juurutada remote viide. Saate kasutada seda URL-i toimige järgmiselt.

2. Käsurea kaudu, kontrollige, kas teie kohaliku Git hoidla juurkaustas.

3. Kasutage `git remote` lisada remote viide loetletud **Git URL-i** etappi 1. Teie käsk näeb välja umbes järgmine:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] Käsk **remote** lisab serveri hoidla nimega viide. Selles näites luuakse viide nimega 'azure' web appi hoidla.

4. Vajutage rakenduse teenuse abil soovitud uue **azure** remote äsja loodud sisu.

        git push azure master

    Teil palutakse varem loodud Azure'i portaalis mandaadi juurutamise lähtestamisel parool. Sisestage parool (Pange tähele, et Gitbash kaja tärnid konsooli parooli tippimise ajal). 
       
5. Minge tagasi oma rakenduse Azure'i portaalis. **Juurutuste** tera peaks olema kuvatud teie Viimati tehtud PTT logikirjet. 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Klõpsake rakenduse tera sisu kasutusele võetud kinnitamiseks ülaosas nuppu **Sirvi** . 
    
## <a name="Step5"></a>Tõrkeotsing

Järgmiste tõrgete või probleemid sagedamini Git avaldamiseks Azure rakendus App teenuse kasutamisel.

****

**Sümptom**: ei saa Accessi [siteURL]: [scmAddress] ühenduse loomine nurjus

**Põhjus**: See tõrge võib ilmneda juhul, kui rakendus pole tööks.

**Eraldusvõime**: Azure'i portaalis käivitage rakendus. Git juurutamise ei tööta, kui rakendus töötab. 


****

**Sümptom**: ei saanud lahendada host 'hostname"

**Põhjus**: See tõrge võib ilmneda juhul, kui sisestatud loomisel 'azure' remote aadressiteave on vale.

**Eraldusvõime**: kasutage funktsiooni `git remote -v` käsk kõik seadmed, koos seostatud URL-i. Veenduge, et 'Azure' serveri URL on õige. Kui vaja, eemaldage ja looge selle remote õige URL-i.

****

**Sümptom**: pole refs levinud ja pole määratud; mitte midagi. Võib-olla määrake soovitud haru, näiteks "meister".

**Põhjus**: See tõrge võib ilmneda juhul, kui te ei määra on haru kui läbimiseks git push toiming ja on seatud kasutatavaid Git push.default väärtus.

**Eraldusvõime**: tõuketeatised toimingut uuesti määrata juhtslaidi haru teha. Näiteks:

    git push azure master

****

**Sümptom**: src refspec [branchname] ei vasta.

**Põhjus**: See tõrge võib ilmneda juhul, kui proovite push mõne haru peale juhtslaidi 'Azure' remote.

**Eraldusvõime**: tõuketeatised toimingut uuesti määrata juhtslaidi haru teha. Näiteks:

    git push azure master

****

**Sümptom**: tõrge - muutusi pühendunud serveri hoidla, kuid teie web Appis ei värskendata.

**Põhjus**: See tõrge võib ilmneda juhul, kui juurutate Node.js rakendus, mis sisaldavad package.json faili, mis määrab täiendavad moodulid.

**Eraldusvõime**: täiendavad sõnumid, mis sisaldavad "npm ERR!" logitud enne selle tõrke tuleb ning annab selle tõrke kohta täiendavat konteksti. Järgmine on teada põhjuste ja see viga kuvatakse vastava 'npm ERR!" teade:

* **Vigane package.json fail**: npm ERR! Sõltuvused ei saanud lugeda.

* **Kohalikke moodul, mis ei ole kahendarvu jaotuse Windowsi jaoks**:

    * NPM ERR! \`cmd "/ c" "sõlm-gyp taastada"\` 1 nurjus

        VÕI

    * NPM ERR! [modulename@version]eelinstallida: \`teha || gmake\`


## <a name="additional-resources"></a>Lisaressursid

* [Git dokumentatsioon](http://git-scm.com/documentation)
* [Projekti Kudu dokumentatsioon](https://github.com/projectkudu/kudu/wiki)
* [Azure'i rakendust Service pidev juurutamine](app-service-continuous-deployment.md)
* [Azure PowerShelli kasutamine](../powershell-install-configure.md)
* [Kuidas kasutada Azure käsurea liides](../xplat-cli-install.md)

[Azure'i rakendust Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure'i portaal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure'i käsurea liides]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
