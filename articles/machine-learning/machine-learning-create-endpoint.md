<properties
    pageTitle="Loomine veebis teenuse lõpp-punktid masina õ | Microsoft Azure'i"
    description="Azure'i masina õ Web teenuse lõpp-punktid loomine"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Lõpp-punktid loomine

>[AZURE.NOTE] Selles teemas kirjeldatakse meetodite kehtivad klassikaline veebiteenuse abil.

Kui loote veebiteenused, mida müüte edasi oma klientidele, peate andma koolitatud mudelite iga katse, mis loodi veebiteenuse endiselt lingitud kliendile. Lisaks katse värskendusi rakendatakse valikuliselt lõpp kohanduste kirjutamata.

Selleks Azure seadme õ võimaldab teil luua juurutatud veebiteenuse mitme lõpp-punktid. Iga lõpp-punkti veebiteenuse sõltumatult on adresseeritud, rakendus ja hallata. Iga lõpp-punkti on kordumatu URL-i ja luba klahvi, mida saate levitada oma klientidele.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Lõpp-punktid lisamine veebiteenusele

On kolm võimalust veebiteenuse lõpp-punkti lisamiseks.

* Programmiliselt
* Azure'i masina õ veebiteenuste portaali kaudu
* Kuigi Azure klassikaline portaal

Kui lõpp-punkti on loodud, saate kasutada seda sünkroonse API-de, paketi API-d, kaudu ja Exceli töölehed. Lisaks lõpp-punktid selle Kasutajaliidese kaudu lisamist saate kasutada ka lõpp-punkti haldus API-de programmiliselt lisada lõpp-punktid.

 >[AZURE.NOTE] Kui olete lisanud veebiteenuse täiendavad lõpp-punktid, ei saa kustutada vaikimisi lõpp-punkti.

## <a name="adding-an-endpoint-programmatically"></a>Lõpp-punkti programmiliselt lisamine

Saate lisada oma veebiteenuse abil programmiliselt [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) proovi kood lõpp.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Azure'i masina õ veebiteenuste portaalis lõpp lisamine

1. Klõpsake seadme õ Studio, klõpsake vasakpoolsel navigeerimispaanil veerus, Web Services.
2. Web teenuse armatuurlaua allosas nuppu **Halda lõpp-punktid**. Azure'i masina õ veebiteenuste portaali avab veebiteenuse lehele lõpp-punktid.
3. Klõpsake nuppu **Uus**.
4. Tippige nimi ja kirjeldus uue lõpp-punkti. Lõpp-punkti nimed peavad olema 24 märk või vähem pikkus ja tuleb numbrid või väiketähed tähestikus. Valige soovitud Logimistase ja kas näidisandmed on lubatud. Logimise kohta leiate lisateavet teemast [logimise masina õ veebiteenuste jaoks](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Azure'i klassikaline portaalis lõpp lisamine


1. [Azure'i klassikaline portaali](http://manage.windowsazure.com)sisse logida, klõpsake nuppu **Seadme õ** vasakus veerus. Klõpsake käsku tööruum, mis sisaldab veebiteenus, mis teile huvi.

    ![Liikuge tööruumi](./media/machine-learning-create-endpoint/figure-1.png)

2. Klõpsake **veebiteenused**.

    ![Liikuge veebiteenused](./media/machine-learning-create-endpoint/figure-2.png)

3. Klõpsake teid huvitava saadaval lõpp-punktid loendi vaatamiseks veebiteenuse.

    ![Liikuge lõpp-punkti](./media/machine-learning-create-endpoint/figure-3.png)

4. Klõpsake lehe allosas **Lisa lõpp-punkti**. Tippige nimi ja kirjeldus, veenduge, et ei ole muud lõpp selle veebiteenuse sama nimega. Enne, kui olete nõuete jätke vaikeväärtus ahendamise tase. Pidurdamise kohta leiate lisateavet teemast [Skaleerimist API lõpp-punktid](machine-learning-scaling-webservice.md).

    ![Lõpp-punkti loomine](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Järgmised sammud

[Kuidas peab olema avaldatud Azure seadme õ veebiteenusest](machine-learning-consume-web-services.md).
