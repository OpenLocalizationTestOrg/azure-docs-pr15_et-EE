<properties
   pageTitle="Pliiatsi testimine | Microsoft Azure'i"
   description="Artikkel annab ülevaate kättesaadavuse, testimine (pentest) protsessi ja kuidas teha pentest oma rakenduste töötab Azure'i infrastruktuuri eest."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Pliiatsi testimine

Üks suuri asju kasutades Microsoft Azure'i rakenduse testimine ja juurutamise jaoks on mittevajalikud kohapealse infrastruktuur arendamise, katsetamine ja juurutamine rakenduste kokku panna. Microsoft Azure'i platvormi teenused on tehtud eest kõik taristu. Te ei pea muretsema requisitioning, hankimine, "villimiste ja virnastamine" oma kohapealse riistvara.

See on suur-, kuid peate veenduge, et teete tavaline turvalisuse nõuetele vastavuse. Asjad, mida peate tegema on kättesaadavuse testimiseks juurutamist rakenduste Azure.

Te juba teate, et Microsoft teeb [kättesaadavuse kontrollimine meie Azure keskkonnas](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). See aitab meie platvormi täiustada ja juhendab meie toimingud parandamine turbemeetmed, uue turbemeetmed tutvustus ja parandada meie turvalisus protsessid.

Me ei pliiatsi testige saate rakendust, kuid me mõista, et te soovite ja peate täitma pliiats katsetamine oma rakendustes. Mis on hea, kuna kui saate suurendada rakenduste turvalisust, mida turvalisemaks kogu Azure ökosüsteemi.

Kui te pliiatsi rakenduste testimiseks tunduda see meile rünnaku. Me [pidevalt jälgida](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) rünnata mustrite ja alustab käigus langeva vastust, kui peame. See ei aita, ja see ei aidake meil, kui me käivitamine langeva vastuse tõttu oma tähtaja hoolsuse pliiatsi testimine.

Mida teha?

Kui olete valmis pliiatsi Azure'i majutatud rakenduste testimiseks, peate andke meile teada. Kui me teada, et te ei kavatse täita teatud kontrollib, me ei tahtmatult saate sulgeda (nt blokeerimine sa testida, IP-aadress), kui teie vasta on Azure pliiatsi testimise tingimustega.
Standardsed katsed saate teha järgmised.

- Lõpp-punktide avastada [avatud Web rakenduse turvalisus Project (OWASP) ülemine 10 nõrkade kohtade](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) testide
- Lõpp-punktide [fuzz testimine](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
- Lõpp-punktide [port skannimine](https://en.wikipedia.org/wiki/Port_scanner)

Test, mida te ei saa teha ühte tüüpi on mis tahes tüüpi rünnak [Eitamine Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) . See hõlmab alustamist DoS rünnaku ise või läbimiseks seotud testide, mis võib kindlaks teha, näidata, või mis tahes tüüpi DoS rünnakut simuleerida.

On teil algust pliiatsiga testimine majutatud Microsoft Azure rakenduste? Kui jah, siis pea kohta üle lehe [Kättesaadavuse Test ülevaade](https://security-forms.azure.com/penetration-testing/terms) (ja valige testimine taotleda lehe allservas nuppu Loo. Leiate lisateavet pliiatsi testimine tingimused ja kohta, kuidas saate teatada turvalisuse vigu Azure ja Microsofti muude teenustega seotud kasulikud lingid.
