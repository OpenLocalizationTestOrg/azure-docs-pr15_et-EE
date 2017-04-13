<properties
   pageTitle="Amazon veebiteenuste autentimise konfigureerimine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas luua ja kinnitage AWS mandaadi tegevusraamatud Azure'i automaatika AWS ressursside haldamine."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="AWS autentimise konfigureerimine aws"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Autentida tegevusraamatud Amazon veebiteenuste
Levinud ressurssidega Amazon Web Services (AWS) ülesannete automatiseerimine on võimalik teha koos automaatika tegevusraamatud Azure.  Paljude tööülesannete AWS automaatika tegevusraamatud täpselt samamoodi nagu saate seda teha ressursid Azure abil automatiseerida.  Vaja on kaks asja.

* Tellimuse AWS ja mandaadi kogumist.  Konkreetselt oma AWS juurdepääsu võti ja salajane võti.  Lisateabe saamiseks vaadake artiklit [AWS mandaadi abil](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Azure'i tellimus ja automatiseerimise konto.  Azure automatiseerimine konto häälestamise kohta lisateabe saamiseks vaadake artiklis [Konfigureerimine Azure'i käivitada nagu konto](../automation/automation-sec-configure-azure-runas-account.md).  

Autentimiseks AWS, peate määrama AWS mandaadikomplekt autentida oma tegevusraamatud alates Azure automatiseerimine. Kui teil on juba loodud automatiseerimise konto ja te soovite kasutada autentimiseks AWS, saate selle juhiste kohta järgmisest jaotisest.  Kui soovite spetsiaalne konto tegevusraamatud sihtimine AWS ressursid, peaksite kõigepealt looma uue [automatiseerimise käivitada nagu konto](../automation/automation-sec-configure-azure-runas-account.md) (teenuse põhilise loomine Jäta vahele) ja seejärel järgige alltoodud juhiseid.

## <a name="configure-automation-account"></a>Automaatika konto konfigureerimine
Azure'i automaatika AWS suhelda, peate esmalt tuua AWS mandaat ja salvestaks need saadaolevad Azure automatiseerimine.  Järgmiste toimingute dokumenteerida AWS dokumendi [Haldamise kiirklahvide AWS konto](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) loomine kiirklahvide ja kopeerige **kiirklahv ID** ja **Salajane võti** (soovi korral faili alla laadida oma tootenumbri talletamiseks seda kuskil ohutu).

Pärast seda, kui olete loonud ja oma AWS turvalisus klahvid kopeeritud, peate konto Azure automatiseerimine turvaliselt talletada ja neid oma tegevusraamatud viidata mandaati varade loomiseks.  Tehke jaotises **luua uue mandaadi** [mandaat varasid Azure automatiseerimine](../automation/automation-certificates.md/###To create a new credential with the Azure portal) artiklis toodud juhised ja sisestage järgmine teave:

1. Sisestage väljale **nimi** **AWScred** või pärast oma nime standardid sobivat väärtust.  
2. Tippige väljale **kasutajanimi** oma **Accessi ID** "ja" **Salajane kiirklahv** väljale **parool** ja **parooli kinnitus** .   

## <a name="next-steps"></a>Järgmised sammud

- Reivew lahenduse artiklist saate teada, kuidas luua tegevusraamatud ülesannete automatiseerimiseks programmis AWS [automatiseerimine kasutuselevõtu Amazon veebiteenuste VM](../automation/automation-scenario-aws-deployment.md) .
