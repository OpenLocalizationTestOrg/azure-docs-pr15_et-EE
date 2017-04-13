<properties
    pageTitle="Seansi osaleja Eclipse Azure'i tööriistakomplekt kasutamise lubamine"
    description="Saate teada, kuidas seansi osaleja Eclipse Azure'i tööriistakomplekt kasutamise lubamine."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Seansi osaleja lubamine #

Sees Azure'i tööriistakomplekt Eclipse, saate lubada HTTP seansi osaleja või "sticky seansid" oma rollid. Järgmisel pildil on kujutatud **Koormusetasakaalustuseks** atribuutide dialoogiboks kasutatud seansi osaleja funktsiooni:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Seansi osaleja oma rolli lubamiseks ##

1. Paremklõpsake Eclipse's Project Exploreri roll, klõpsake **Azure**ja klõpsake **Koormusetasakaalustuseks**.
1. Dialoogiboksis **Atribuudid WorkerRole1 Koormusetasakaalustuseks** :
    1. Märkige ruut **lubamiseks HTTP seansi osaleja (sticky seanssi) selle rolli.**
    1. **Sisestusmeetodi lõpp-punkti kasutada**, valige Sisestuskeel lõpp-punkti kasutada näiteks **http (avalik: 80, Privaatne: 8080)**. Rakenduse peate kasutama oma HTTP lõpp-selle lõpp-punkti. Saate oma rolli lubada mitme lõpp-punktid, kuid saate valida ainult üks neist toetamiseks sticky seansid.
    1. Rakenduse taastada.

Kui lubatud, kui teil on rohkem kui üks roll näiteks HTTP päringuid pärit konkreetse kliendi jätkub sama rolli eksemplari, mida kasutavad.

Eclipse tööriistakomplekt lubab seda, installides teisiti IIS-i mooduli kahtluse iga rolli eksemplaride rakenduse taotleda marsruutimine (ARR). ARR reroutes HTTP päringuid sobiv roll eksemplari. Tööriistakomplekt ümber valitud lõpp-punkti automaatselt konfigureeritud, nii, et sissetulevad HTTP-liikluse suunatakse esmalt ARR tarkvara. Tööriistakomplekt loob ka uue sisemise lõpp-punkti, mis teie Java server on konfigureeritud kuulata. Mis on kasutada ARR marsruudi HTTP liikluse sobiv roll eksemplari lõpp-punkti. Sellisel viisil iga rolli eksemplari mitme eksemplari juurutamise toimib tagant puhverserveri kõikidel muudel juhtudel, võimaldades sticky seansid.

## <a name="notes-about-session-affinity"></a>Märkmete seansi osaleja kohta ##

* Seansi osaleja Arvuta emulaator ei tööta. Sätteid saab rakendada Arvuta emulaator võrreldes oma Koosta protsess või emulaator täitmise arvutada, kuid funktsioon ise ei toimi Arvuta emulaator jooksul.
* Seansi osaleja lubamine tulemuseks suurendamise kettaruumi, juurutamise Azure, nagu täiendavat tarkvara laaditakse alla ja installitakse teie teenuse käivitamist Azure'i pilves oma rolli aknad.
* Lähtestada iga rolli aega võtab rohkem aega.
* Sisemise lõpp-liikluse rerouter eespool nimetatud toimida saab added.ss

Näide sellest, kuidas seansi andmeid säilitada, kui seansiga osaleja on lubatud, vaadake [teemat säilitada seansi andmete seansi osaleja][].

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

[Kuidas säilitada seansi andmete seansi osaleja][]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Kuidas säilitada seansi andmete seansi osaleja]: http://go.microsoft.com/fwlink/?LinkID=699539
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
