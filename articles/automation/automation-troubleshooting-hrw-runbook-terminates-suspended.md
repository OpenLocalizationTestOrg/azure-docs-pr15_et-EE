<properties
   pageTitle="Hü Käitusjuhendi töötaja: Käitusjuhendi töö lõpeb olekut Suspended | Microsoft Azure'i"
   description="Sümptomid põhjuseid ja lahendusi hübriid Käitusjuhendi töötaja töö lõpetamise tõrge."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hübriid Käitusjuhendi töötaja: Käitusjuhendi töö lõpeb Suspended olek

## <a name="summary"></a>Kokkuvõte

Teie käitusjuhendi on peatatud vahetult pärast seda, kui proovite täita kolm korda. On tingimusi, mis võib katkestada käitusjuhendi lõpulejõudmist ja seotud tõrketeade ei sisalda täiendavat teavet, mis näitab, miks. Sellest artiklist leiate tõrkeotsingujuhiseid hübriidjuurutuse Käitusjuhendi töötaja käitusjuhendi täitmise tõrkeid seotud probleemid.

Kui käesolevas artiklis ei käsitleta Azure probleemi, külastage [MSDN-i ja virnas ületäitumise](https://azure.microsoft.com/support/forums/)Azure foorumeid. Saate postitada oma probleemi foorumites või juurde [ @AzureSupport Twitter](https://twitter.com/AzureSupport). Samuti saate faili Azure'i tugiteenuse taotluse, valides **tootetugi** [Azure tugiteenuste](https://azure.microsoft.com/support/options/) sait.

## <a name="symptom"></a>Sümptom

Käitusjuhendi käivitamine nurjub ja tõrke tagastada, "'Aktiveeri' ei saa käivitada, kuna protsess on peatatud ootamatult töö toiming. Töö toiming on proovitakse 3 korda."


## <a name="cause"></a>Põhjus

On mitu võimalikku põhjust seda tõrke. 

  1. Hübriidjuurutuse töötaja on taga tulemüüri või puhverserveri
  2. Hübriid töötaja töötab arvutil on väiksem kui riistvara [nõuded](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. Funktsiooni tegevusraamatud ei saa autentida kohaliku ressurssidega


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Põhjus 1: Hübriidjuurutuse Käitusjuhendi töötaja ei taha puhverserveri või tulemüüri

Hübriidjuurutuse Käitusjuhendi töötaja töötab arvuti taga tulemüüri või puhverserveri ja Väljamineva meili võrgupääs võib olla lubatud või õigesti konfigureeritud.

### <a name="solution"></a>Lahendus

Veenduge, et arvutil on väljaminev juurdepääsu *. cloudapp.net pordid 443, 9354 ja 30000-30199 kohta. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Põhjus 2: Arvutil on väiksem kui minimaalsed riistvaranõuded

Arvutid, kus töötab hübriid Käitusjuhendi töötaja peaksid vastama riistvara miinimumnõuetele enne määrata seda, et see funktsioon majutada. Muul juhul ressursi kasutamise muud taust protsessid ja väide tingitud tegevusraamatud käitamise ajal, olenevalt arvuti on muutunud üle kasutatud ja põhjustada käitusjuhendi töö viivitused või ajalõpud. 

### <a name="solution"></a>Lahendus 

Esmalt veenduge, et määratud hü Käitusjuhendi töötaja funktsiooni käivitamiseks arvuti vastab riistvara miinimumnõuetele.  Kui seda ei, jälgida mis tahes korrelatsiooni hübriid Käitusjuhendi töötaja protsesside jõudlus ja Windowsi määratlemiseks CPU ja mälu kasutamine.  Kui on mälu või CPU rõhk, võib see viidata vajadust täiendamine lisada täiendavad protsessorite või suurendada mälu ressursside kitsaskohaks ja tõrke kõrvaldamine. Teise võimalusena valige muu Arvuta ressurss, mis toetab miinimumnõuded ja skaala kui töökoormus nõudmistele on vaja suurendada.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Põhjus 3: Tegevusraamatud ei saa autentida kohalikud ressursid

### <a name="solution"></a>Lahendus

Märkige ruut **Microsoft-SMA** sündmuste logi vastava sündmuse koos kirjeldusega *Win32 joonis väljunud koodiga [4294967295]*.  Selle tõrke põhjuseks on teil pole konfigureeritud autentimise oma tegevusraamatud või määratud Käivita kasutajana mandaati hübriid töötaja rühma.  Palun vaadake üle [Käitusjuhendi õigused](automation-hybrid-runbook-worker.md#runbook-permissions) on õigesti konfigureeritud autentimise jaoks oma tegevusraamatud kinnitamiseks.  


 

