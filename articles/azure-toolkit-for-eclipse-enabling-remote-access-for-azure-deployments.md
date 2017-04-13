<properties
    pageTitle="Azure'i juurutuste on Eclipse Kaugpöördusteenuse lubamine"
    description="Saate teada, kuidas lubada Azure juurutuste Azure'i tööriistakomplekt kasutamise Eclipse Kaugpöördusteenuse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Azure'i juurutuste on Eclipse Kaugpöördusteenuse lubamine

Saate tõrkeotsinguks võib lubamine ja Kaugpöördusteenuse ühenduse virtuaalse masina majutusteenuse juurutamise abil. Funktsiooni Kaugpöördusteenuse tugineb klõpsake Kaugtöölaua protokolli (RDP). Saate konfigureerida Kaugpöördusteenuse oma juurutamiseks pärast selle avaldamist Azure või kui kasutate operatsioonisüsteemi Windows Eclipse, saate konfigureerida Kaugpöördusteenuse enne Azure avaldamist. Pange tähele, et peate remote töölauakliendi, mis ühildub operatsioonisüsteem ühenduse loomiseks oma juurutuse virtuaalse masina Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Kuidas lubada Kaugpöördusteenuse enne juurutamist Azure

> [AZURE.NOTE] Kaugpöördusteenuse enne juurutamist Azure rakenduse lubamiseks peate olema Eclipse opsüsteemi Windows.

Järgmisel pildil on kujutatud **Kaugpöördusteenuse** atribuutide dialoogiboks kasutada Kaugpöördusteenuse.

![][ic719494]

On kaks võimalust **Kaugpöördusteenuse** atribuutide dialoogiboksi kuvamiseks.

* **Azure'i avalda** dialoogiboksi jaotises **Kaugpöördusteenuse** linki **Täpsemalt** .
* Avage Azure'i projekti dialoogiboks **Atribuudid** .

Kui loote uue Azure juurutamise projekti, ei ole projekti Kaugpöördusteenuse vaikimisi sisse lülitatud. Siiski saate hõlpsalt lubada Kaugpöördusteenuse määramisega kasutajanimi ja parool dialoogiboksis **Azure'i avalda** . Kaugpöördusteenuse parool on krüptitud X.509 serdid. Kui te ei kasuta pakkuda oma serdi krüptimise tugineb iseallkirjastatud serdi tarniti Eclipse Azure'i lisandmooduli abil. See iseallkirjastatud sert on **cert** kausta Azure'i projekti, talletatud nii nagu avaliku serdifail (SampleRemoteAccessPublic.cer) ja isikliku teabe Exchange (PFX) serdifail (SampleRemoteAccessPrivate.pfx). See sisaldab serti privaatvõti ja see on vaikimisi parooli, **parool1**. Siiski, kuna see parool on üldiselt teada, vaikimisi serdi võib kasutada ainult õppe eesmärgil, mitte tootmise juurutamiseks. Nii kui õppimisega, kui soovite lubada teie juurutuste kaugseansi teil peaks linki **Täpsemalt** **Avalda Azure** dialoogiboksis Määrake oma sert. Pange tähele, et peate üleslaadimiseks PFX versiooni serdi majutatud teenust jooksul Azure'i haldusportaal, saate selle Azure'i dekrüptida kasutaja parooli.

Õpetuse ülejäänud näitab, kuidas lubada Kaugpöördusteenuse Azure juurutamise projekti, mis loodi algselt Kaugpöördusteenuse keelatud. Selles õpetuses huvides loome uue iseallkirjastatud serti ja pfx-fail on teie valitud parool. Teil on ka võimalus kasutada väljastatud serti sert.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Kuidas lubada Kaugpöördusteenuse pärast seda, kui olete juurutanud Azure

Pärast seda, kui olete juurutanud Azure Kaugpöördusteenuse lubamiseks tehke järgmist:

1. Azure'i konto kaudu Azure'i haldusportaal sisse logida
1. Valige oma loendis **pilveteenuste**oma juurutatud pilveteenuses
1. Pilveteenuse web lehel linki **konfigureerimine**
1. Konfiguratsiooni lehe allosas linki **Remote**
1. Kui kuvatakse hüpikakende dialoogiboks:
    * Saate, mille jaoks soovite lubada Kaugpöördusteenuse rolli määramine
    * Märkige ruut **Luba kaugtöölaud**
    * Määrake kasutajanimi ja parool, mida soovite kasutada Kaugpöördusteenuse
    * Valige sert kasutamine
1. Klõpsake nuppu **OK** 

Kuvatakse teade selle kohta, et oma konfiguratsiooni muuta on pooleli, mis võib kuluda mõni minut lõpuleviimiseks. Pärast konfiguratsiooni muutmine on lõppenud, järgige juhiseid jaotises **kaugühenduse teel sisse logida** selle artikli.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Kuidas lubada Kaugpöördusteenuse teie pakett

1. Eclipse's Project Exploreri paanil, paremklõpsake Azure'i projekti ja klõpsake käsku **Atribuudid**.

1. Laiendage vasakpoolsel paanil **Azure'i** dialoogiboks **Atribuudid** ja klõpsake nuppu **Kaugpöördusteenuse**.

1. Veenduge dialoogiboksis **Kaugpöördusteenuse** **lubamiseks kõikide rollide aktsepteerimiseks Remote'i töölaua ühendusi nende sisselogimisteave** oleks märgitud.

1. Määrake kasutajanimi kaugtöölaua ühendus.

1. Määrake ja kinnitage parool kasutajale. Kui teete kaugtöölaua ühendus kasutab seada selles dialoogiboksis väärtusi kasutaja nimi ja parool. (Pange tähele, et see on eraldi PFX parool parool).

1. Saate määrata aegumiskuupäeva kasutajakonto.

1. Klõpsake nuppu **Uus** , et luua uut iseallkirjastatud serti. (Teise võimalusena saate valida sert süsteemist tööruumi või fail nuppe **tööruumi** või **failisüsteemi** kaudu vastavalt, kuid selles õpetuses eesmärgil loome uut serti.)

    * Dialoogiboksis **Uut serti** Määrake ja kinnitage parool, mida kasutate oma PFX-faili.

    * Aktsepteerige **Nimi (CN)**sätestatud väärtuse või kasutada kohandatud nime.

    * Määrake tee ja faili nimi, kuhu salvestatakse uut serti, CER kujul. Selles etapis tuleb ja järgmise juhise juurde, võite kasutada Azure projekti **cert** kausta, kuid olete õigus, valige mõni muu asukoht. Selle õpetuse huvides kasutame **c:\mycert\mycert.cer**. ( **C:\mycert** kausta enne jätkamist loomine või olemasoleva kausta kasutamiseks, kui soovitud.)

    * Määrake tee ja faili nimi, kus uut serti ja privaatvõti pfx vorm, salvestatakse. Selle õpetuse huvides kasutame **c:\mycert\mycert.pfx**. Teie **Uus serdi** dialoogiboks peaks sarnanema (kui te ei kasuta **c:\mycert**värskendada kausta tee) järgmist:

        ![][ic712275]

    * Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Uus sert** .

1. Oma **Kaugpöördusteenuse** dialoogiboksi peaks välja nägema umbes järgmine:</p>

    ![][ic719495]

1. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Kaugpöördusteenuse** .
    
Taastada oma rakenduse abil määrata juurutamiseks Cloud koostamine.

## <a name="to-log-in-remotely"></a>Logige kaugühenduse teel

Kui teie roll eksemplari on valmis, saate Eemalt logite virtuaalse masina, mis majutab rakenduse.

* Kui kasutate Eclipse Windows ja teie valitud suvand **Start kaugtöölaua juurutamine** juurutamise Azure, ilmub kaugtöölaua ühendus sisselogimise ekraan juurutamise käivitamisel. Kui teilt küsitakse kasutajanime ja parooli, sisestage serveri kasutaja määratud väärtused ja saab sisse logida.

* Teine võimalus teises arvutis sisse loginud on <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure haldusportaali</a>kaudu:

    * Vaadetes **Pilveteenustega** Azure haldusportaali, klõpsake oma pilveteenuses, klõpsake **eksemplarid**, klõpsake eksemplariga ja seejärel klõpsake nuppu **Loo ühendus** . Nupp **Ühenda** kuvatakse käsuriba järgmiselt:

        ![][ic659273]

    * Pärast nupu **Loo ühendus** , palutakse teil RDP-faili avada. Avage fail ja järgige viipasid. (Teil võib ka salvestada see fail kohalikus arvutis ja käivitage fail, topeltklõpsates remote Logi virtuaalne arvuti ilma esmalt minge portaali haldus.)

    * Kui teilt küsitakse kasutajanime ja parooli, sisestage serveri kasutaja määratud väärtused ja saab sisse logida.

> [AZURE.NOTE] Kui kasutate-Windowsi operatsioonisüsteemi, peate kaugtöölaua klient, mis ühildub operatsioonisüsteem ja järgige juhiseid kliendi ja allalaaditud faili RDP sätete konfigureerimine.

## <a name="see-also"></a>Vt ka

[Azure'i tööriistakomplekt Eclipse][]

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
