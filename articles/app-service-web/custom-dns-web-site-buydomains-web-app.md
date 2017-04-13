<properties
    pageTitle="Azure'i rakenduse teenuse veebirakendustes kohandatud domeeninime ostmine"
    description="Saate teada, kuidas web Appiga teenuses Azure rakenduse kohandatud domeeninime ostmine."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Osta ja kohandatud domeeninime Azure'i rakendust Service konfigureerimine

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Kui loote web appi, määratakse Azure'i azurewebsites.net alamdomeen. Kui teie web Appis nimega **contoso**, on URL-i **contoso.azurewebsites.net**. Azure'i määrab ka virtuaalse IP-aadressi.

Tootmise web appi, soovite tõenäoliselt saajatele kuvada kohandatud domeeni nimi. Selles artiklis selgitatakse, kuidas osta ja konfigureerida kohandatud domeeni [rakenduse teenuse Web](http://go.microsoft.com/fwlink/?LinkId=529714)apps. 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Ülevaade

Kui teil pole oma veebirakenduse jaoks domeeninime, saate osta ühte [Azure'i](https://portal.azure.com/)portaalis. Ostmise käigus saate on www-d ja root domeeni DNS-i kirjed automaatselt vastendada oma veebirakenduse. Samuti saate hallata oma domeeni otse Azure portaali.


Osta domeeni nimed ja määrata oma veebirakenduse järgmiste juhiste abil.

1. Avage brauseris [Azure portaali](https://portal.azure.com/).

2. **Veebirakenduste** menüüs klõpsake oma veebirakenduse nime, klõpsake nuppu **sätted**ja valige **kohandatud domeenide**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. **Kohandatud domeenide** tera, klõpsake nuppu **osta domeeni**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. **Osta domeeni** labale abil tekstivälja tippige domeeni nimi, mida soovite osta ja vajutage sisestusklahvi. Tekstivälja all kuvatakse soovitatud saadaval domeenid. Valige, mida soovite osta domeeni. Saate valida ostmiseks korraga mitu domeeni. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Klõpsake nuppu **Kontaktteave** ja sisestage selle domeeni kontaktteabe vorm.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] See on väga oluline, täitke kõik nõutavad väljad nii palju täpsusega võimalikult, eriti e-posti aadress. Korral ilma "Privaatsuskaitset" domeeni ostmine, võidakse paluda enne domeeni aktiveerub oma e-posti kontrollida. Mõnel juhul tulemuseks vale andmete kontaktteave jätmine osta domeeni. 

6. Nüüd saate valida

    (a) "automaatne pikendamine" domeeni iga aasta
    
    (b) valida-in "Privaatsuskaitset", mis sisaldub ostuhind tasuta (välja arvatud tippdomeenid kasutaja registri ei toeta Privaatsus. For example:. co.in,. co.uk jne.)  
    
    c) "määrata vaikimisi hostinimed" www-d ja root domeeni praeguse Web Appi. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Suvand C konfigureerib DNS-i sidumiste ja Hostname sidumiste automaatselt teie eest.  Sellisel viisil oma veebirakenduse pääseb kasutades kohandatud domeeni niipea, kui ostu on lõpetatud (DNS-i paljundamine viivitust mõnel juhul paljastus). Juhuks oma veebirakenduse on taha Azure'i liikluse haldur, kuvatakse suvand määrata juurdomeeni A-kirjed ei tööta liikluse haldur. Saate alati määrata selle domeenide/all-domains ostetud ühe Web Appis teise Web App ja vastupidi. Vaadake lisateavet punkti 8. 
    
7. Klõpsake soovitud **Valige** **Osta domeeni** tera ja seejärel kuvatakse teave ostu **osta kinnituse** tera. Kui juriidiline nõustumine ja klõpsake nuppu **osta**, esitatakse tellimuse ja saate jälgida ostmise protsessi kohta **teatise**. Domeeni ostmine võib kuluda mõni minut. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Kui tellisite edukalt domeeni, saate hallata domeeni ja määrata oma veebirakenduse. Klõpsake soovitud **"..."** paremal küljel oma domeeni. Seejärel saate **osta tühistamine** või **haldamine Domeen**. Klõpsake nuppu **Halda domeeni**, siis me siduda **alamdomeen** meie web appi **haldamine domeeni** tera. Kui soovite siduda **alamdomeen** erinevate Web Appi siis seda juhist: vastav veebirakenduse raames. Siin saate määrata domeeni liikluse haldur lõpp-punkti (kui Web App ei taha TM) lihtsalt valimise liikluse haldur valimisel nimi rippmenüüst. Seda tehes määratakse domeeni/alamdomeen automaatselt Web Apps taha seda liikluse haldur lõpp-punkti. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] "Saate osta" ettemakstud 5 päeva jooksul. Pärast 5 päeva, siis ei saa "Loobu osta", selle asemel kuvatakse suvandit "Kustuta" domeeni. Domeeni kustutamise tulemuseks vabastamine tellimusest ilma toetuseta ja muutuvad kättesaadavaks domeeni. 

Kui konfigureerimine on lõppenud, loetletakse **Hostname sidumiste** osas oma veebirakenduse kohandatud domeeni nimi.

Selles etapis peaks olema võimalus sisestage brauseri kohandatud domeeni nimi ja leiate, et see edukalt suunab teid oma veebirakenduse.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Mis juhtub ostetud kohandatud domeeni

Kohandatud domeeni ostsite **kohandatud domeenid ja SSL-i** tera on seotud Azure'i tellimus. Mis on Azure ressurss on selle kohandatud domeeni rakendusest rakenduse teenuse esmalt ostsite domeeni jaoks eraldi ja sõltumatu. See tähendab, et:

- Azure portaali, saate kasutada rohkem kui ühte rakendust Service rakendust ja mitte ainult rakenduse esmalt ostetud kohandatud domeeni kaudu ostetud kohandatud domeeni. 
- Saate hallata kõik kohandatud domeenide Azure'i tellimuse minnes **kohandatud domeenid ja SSL-i** tera *mõnda* rakendust Service rakenduse tellimuse ostsite.
- Saate määrata mõne rakenduse teenuse rakenduse sama Azure tellimuse alamdomeeni kohandatud domeenis.
- Kui otsustate kustutada rakenduse teenuse rakendus, te ei soovi kohandatud domeeni, kui see on seotud, kui soovite edasi kasutada muude rakenduste kustutamine.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Kui te ei näe kohandatud domeeni ostsite

Kui ostetud kohandatud domeeni kaudu **kohandatud domeenid ja SSL-i** tera sees, kuid ei näe kohandatud domeeni jaotises **hallatavad domeenid**, kontrollige järgmisi asju:

- Kohandatud domeeni loomine on lõpule jõudnud. Märkige ruut teatis Gaussi Azure portaali ülaosas edenemist.
- Kohandatud domeeni loomine võib-olla ei ole mingil põhjusel. Märkige ruut teatis Gaussi Azure portaali ülaosas edenemist.
- Kohandatud domeeni võib olla õnnestus, kuid tera pole veel värskendada. Proovige **kohandatud domeenid ja SSL-i** tera uuesti avada.
- On kustutatud kohandatud domeeni mingil hetkel. Auditilogide kontrollimiseks klõpsake nuppu **sätted** > **Audit logid** oma rakenduse peamised tera kaudu. 
- **Kohandatud domeenid ja SSL-i** tera otsite võib kuuluvad rakendust, mis on loodud mõnda muud Azure tellimust. Aktiveerige teine rakendus erinevate tellimus ja märkige oma **kohandatud domeeni ja SSL-i** blade.  
  Portaali, ei saa vaadata või mõnda muud Azure tellimust, kui rakenduse loodud kohandatud domeenide haldamiseks. Juhul, kui klõpsate nuppu **Täpsemad halduse** domeeni **haldamine domeeni** blade, suunatakse teid domeeni pakkuja veebisaidile, kus teil tuleb   [käsitsi](web-sites-custom-domain-name.md) konfigureerida kohandatud domeeni, nagu mis tahes välise kohandatud domeeni 
   rakenduste loodud mõnda muud Azure tellimust. 


