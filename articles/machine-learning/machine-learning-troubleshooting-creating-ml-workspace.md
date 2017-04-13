<properties
    pageTitle="Tõrkeotsing: Loomine ja luua ühenduse arvuti õ tööruumi | Microsoft Azure'i"
    description="Loomine ja Azure seadme õ tööruumi ühenduse levinumate probleemide lahendustega"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Tõrkeotsingu juhend: loomine ja luua ühenduse mõne seadme õ tööruumi

Sellest juhendist leiate lahenduste jaoks sageli esinevate toimetulekuks Kui häälestate Azure seadme õ tööruumid.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Tööruumi omanik

Kui loote uue arvuti õ tööruumi, ID väljale TÖÖRUUMI omanik peab olema lubatud Microsofti kontoga (varem Windows Live ID), näiteks john-contoso@live.com või john-contoso@hotmail.com. Mitte-Microsofti konto, näiteks oma ettevõtte e-posti konto ei saa see olla. Tasuta Microsofti konto loomiseks avage [www.live.com](http://www.live.com).

Pange tähele, et Azure portaali sisse logida tööruumi loomiseks kasutatud konto pole automaatselt õigus, et *avada* selle tööruumi juhul, kui määrate selle konto omanik. Tööruumi avamiseks seadme õ Studios te peate olema sisse logitud Microsofti Account, mis on määratletud tööruumi omanik või peate kutse liituda tööruumi omanik. Azure'i klassikaline portaalist, aga saate *hallata* tööruum, mis sisaldab omaniku muutmine ja konfigureerimine Accessi.

Tööruumi haldamise kohta leiate lisateavet teemast [mõne Azure seadme õ tööruumi].

[Mõne Azure seadme õ tööruumi haldamine]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Lubatud piirkonnad

Seadme õ on praegu saadaval teatud piirkondadele. Kui teie tellimus ei sisalda ühte regioonides, võidakse kuvada tõrketeade, "Teil pole tellimuste lubatud piirkondades."

Taotleda piirkonnas teie tellmusele lisada, valige **Pöörduge Microsofti tootetoe poole** Azure'i klassikaline portaalis, valige **arveldus** probleemi tüüp ja järgige viipasid esitada teie taotlus.

![Pöörduge Microsofti klienditoe poole][screen1]

## <a name="storage-account"></a>Salvestusruumi konto

Seadme õ teenuse peab storage konto andmete talletamiseks. Saate olemasoleva salvestusruumi konto või uue konto salvestusruumi saate luua uue arvuti õ tööruumi loomisel (kui teil on kvoodi salvestusruumi uue konto loomiseks).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Tööruumi loomine][screen2]

Pärast uue arvuti õ tööruum on loodud, saate logige sisse arvutisse õ Studio omanik ja tööruumi määratud Microsofti konto abil. Kui teil tekib kasutamisel kuvatakse tõrketeade "Tööruumi ei leitud" (sarnane järgmine pilt), kasutage brauseri küpsiste kustutamiseks toimige järgmiselt.

![Tööruumi ei leitud][screen3]

**Brauseri küpsiste kustutamine**

Kui kasutate Internet Explorerit, klõpsake paremas ülanurgas nuppu **Tööriistad** ja valige **Interneti-suvandid**.  

![Interneti-suvandid][screen4]

Klõpsake vahekaardi **üldist** jaotises **kustutamine...**

![Vahekaardi Üldist][screen5]

Dialoogiboksis **Kustuta sirvimisajalugu** veenduge, et **küpsised ja veebisaidi andmed** on valitud, ja klõpsake nuppu **Kustuta**.

![Küpsiste kustutamine][screen6]

Pärast küpsised on kustutatud, taaskäivitage brauser ja seejärel avage leht [Microsoft Azure'i masina õ](https://studio.azureml.net) . Kui teilt küsitakse kasutajanime ja parooliga, sisestage sama Microsofti konto omanik ja tööruumi määratud.

Meie eesmärk on teha seadme õppimise kogemus sama sujuv kui võimalik. Saatke [Azure seadme õppe Foorum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) hõlbustavate teid parem kõik kommentaarid ja probleemid.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
