<properties
 pageTitle="Sõnastikupõhise hooldustööd eelkonfigureeritud lahenduse | Microsoft Azure'i"
 description="Azure'i asjade sõnastikupõhise hooldustööd eelkonfigureeritud lahenduse kirjeldus."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Eelkonfigureeritud sõnastikupõhise hooldustööd lahenduse ülevaade

*Sõnastikupõhise hooldustööd* eelkonfigureeritud lahendus on üks [eelkonfigureeritud lahenduste] [ lnk_preconfigured_solutions] [Microsoft Azure'i asjade komplekti]osana välja[lnk_iot_suite]. See lahendus reaalajas seadme telemeetria saidikogumi integreerub [Azure seadme õ]abil loodud sõnastikupõhise mudeli[lnk_machine_learning].


Azure'i asjade Suite suurettevõtete saate kiiresti ja hõlpsalt ühenduse varade jälgimine ja reaalajas andmete analüüsiks. Eelkonfigureeritud sõnastikupõhise hoolduse lahendus võtab need andmed ja kasutab rikkaliku armatuurlaudade ja visualiseeringute pakkuda ettevõtetele uus ärianalüüsi, mida saab juhtida tõhusust ja täiustamiseks tulu voogu.

## <a name="the-scenario"></a>Stsenaarium

Fabrikam on piirkondliku lennufirma, mis keskendub hea kliendi kogemus eest. Ühe lendude hilinemist põhjuseks on hooldus küsimusi ja lennundustehnilise engine on eriti keeruline. Otsimootori tõrge lennureisi ajal tuleb vältida iga hinna nii Fabrikam uurib selle mootorite regulaarselt ja järgib programmi kavandatud hooldustööd. Siiski alati ei mootorite sama kandma. Mõned mittevajalike hooldust tehakse puhul. Veelgi olulisem probleeme tekkida, mis saab maa õhusõiduki kuni hooldust tehakse. See põhjustab kulukaid viivitusi, eriti siis, kui ei ole kohas, kus õige tehniliste töötajate või vaba osade pole saadaval.

Fabrikam's õhusõidukite mootorite on kinnitatakse kraane jälgimine engine tingimuste lennureisi ajal. Fabrikam abil Azure'i asjade komplekti andur on lennureisi ajal kogutud andmete kogumise. Pärast kogunev aastaid engine funktsionaalseid ja tõrge andmeid, on Fabrikam's andmeteadlaste modelleerida võimalus prognoosida selle ülejäänud kasulik kestus (RUL) õhusõiduki mootori. On tuvastatud on omavahel andmeid nelja mootori andurid koos engine kandma, mis viib lõpliku tõrge. Kuigi Fabrikam jätkuvalt korrapäraselt turvalisuse tagamiseks teha, selle abil saate nüüd mudelid arvutada iga mootori RUL pärast iga flight telemeetria abil kogutud mootorite soovitud lennureisi ajal. Nüüd saate fabrikam prognoosida tulevaste punktid ja haldamise kavandamine ja parandada ette.

> [AZURE.NOTE] Lahendus mudeli kasutab tegeliku mootori kanda andmeid.

Punkti prognoosida, kui hooldustööd on vaja, saate Fabrikam Optimeeri tegevuse kulude vähendamiseks. Hoolduse koordinaatorid töötada skeduloijat kavandada hooldustööd ühtib õhusõidukiga peatuvad teatud asukohta ja tagada õhusõiduki tuleb välja teenuse katkemise ajakava ilma piisavalt aega. Fabrikam saavad ajastada tehnikud tagada õhusõiduki juurde pääseb tõhus ilma ootama aega. Laoseisu juhtelemendi haldurid saavad hooldus lepingud, et nad saaksid oma tellimise protsessi optimeerimine ja säästa osade loetelu. Kõik see lubab Fabrikam minimeerimiseks õhusõiduki põhjusel aega ja samal ajal tagada ja meeskonna vähendamiseks.

Et aru saada, kuidas [Azure'i asjade komplekti] [ lnk_iot_suite] pakub võimaluste kliendid on vaja mõista sõnastikupõhise haldamise võimalusi, palun vaadake üle selle [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Kuidas sõnastikupõhise hoolduse lahendus on loodud

Lahendus mõjutab olemasoleva Azure seadme õ mudeli mallina neid võimalusi töötades seadme telemeetria kogutud asjade komplekti teenuste kuvamiseks saadaval. Microsoft on loonud [regressiooni mudel] [ lnk_regression_model] õhusõiduki mootori ja avaldatud täieliku Mall, andmete<sup>\[1\]</sup>, ja üksikasjalikud juhised, kuidas kasutada.

Azure'i asjade sõnastikupõhise hooldustööd eelkonfigureeritud lahendus kasutab regressiooni mudelit, mis on loodud selle malli; See on juurutatud Azure tellimuse sisse ja avatud automaatselt loodud API kaudu. Lahendus sisaldab 4 (Kokku 100), mis tähistab testimise andmete alamhulga mootorite ja 4 (21 kokku), andur andmevoogu, mis pakuvad õige tulemuse koolitatud mudel.

*\[1\] A. Saxena ja K. Goebel (2008). "Turboventilaatormootorite Engine degradeerumine simulatsioon andmehulga", NASA Ames Prognostics andmete talletuskoht (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center Moffett välja, CA*

## <a name="next-steps"></a>Järgmised sammud

Lisateavet selle kohta, kuidas Azure'i asjade võimaldab sõnastikupõhise hooldustööd stsenaariumid, lugege [jäädvustada väärtus asjade Interneti kaudu][lnk_capture_value].

Võtta ka [kiirtutvustus] [ lnk-predictive-walkthrough] sõnastikupõhise hoolduse eelkonfigureeritud lahendus.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Samuti saate uurida muude funktsioonide ja võimaluste kohta asjade komplekti eelkonfigureeritud lahendused:

- [Korduma kippuvad küsimused asjade komplekti][lnk-faq]
- [Asjade turvalisuse kohalt kaudu][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
