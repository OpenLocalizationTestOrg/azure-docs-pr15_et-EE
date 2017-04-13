Ressurss|Tasuta|Ühiskasutusega (eelvaade)|Tavaline|Standard|Premium (eelvaade)</th>
---|---|---|---|---|---
[Veebi mobiiltelefon või API rakenduste](https://azure.microsoft.com/services/app-service/) per [rakenduse teenuse leping](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Piiramatu<sup>2</sup>|Piiramatu<sup>2</sup>|Piiramatu<sup>2</sup>
[Loogika rakenduste](https://azure.microsoft.com/services/app-service/logic/) kohta [rakenduse teenusleping](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 core kohta|20 core kohta
[Rakenduse teenusleping](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 iga piirkonna kohta|10 kaarti ressursirühm|100 ressursirühm|100 ressursirühm|100 ressursirühm
Arvutage eksemplari tüüp|Ühiskasutusse antud|Ühiskasutusse antud|Sihtotstarbeline<sup>3</sup>|Sihtotstarbeline<sup>3</sup>|Sihtotstarbeline<sup>3</sup></p>
[Skaala-Out](../articles/app-service-web/web-sites-scale.md) (max eksemplare)|ühiskasutusse antud 1|ühiskasutusse antud 1|3 sihtotstarbeline<sup>3</sup>|10 sihtotstarbeline<sup>3</sup>|20 spetsiaalne (50 ASE)<sup>3,4</sup>
Salvestusruumi<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU aeg (lühendatud)<sup>6</sup>|3 minutit|3 minutit|Piiramatu maksma standard [määr](https://azure.microsoft.com/pricing/details/app-service/)</a>|Piiramatu maksma standard määrade|Piiramatu maksma standard määrade
CPU aeg (päev)<sup>6</sup>|60 minutit|240 minutit|Piiramatu maksma standard [määr](https://azure.microsoft.com/pricing/details/app-service/)</a>|Piiramatu maksma standard määrade|Piiramatu maksma standard määrade
Mälu (1 tund)|1024 MB rakenduse teenusleping kohta.|1024 MB rakenduse kohta|N/A|N/A|N/A
Läbilaskevõime|165 MB|Piiramatu, [edastada andmeid](https://azure.microsoft.com/pricing/details/data-transfers/) rakendamine|Piiramatu andmeedastus määrade rakendamine|Piiramatu andmeedastus määrade rakendamine|Piiramatu andmeedastus määrade rakendamine
Rakenduse arhitektuur|32-bitine versioon|32-bitine versioon|32-bitine/64-bitine|32-bitine/64-bitine|32-bitine/64-bitine
Web Sockets eksemplari<sup>7</sup> euro kohta.|5|35|350|Piiramatu|Piiramatu
Samaaegseid [siluri ühendused](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) taotluse kohta|1|1|1|5|5
[azurewebsites.net alamdomeeni FTP/S ja SSL-i abil](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[Kohandatud domeeni](../articles/app-service-web/web-sites-custom-domain-name.md) tugi||X|X|X|X
Kohandatud domeeni [SSL-i tugi](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Piiramatu|Piiramatu, 5 SNI SSL-i ja 1 IP SSL-i ühendused sisalduv|Piiramatu, 5 SNI SSL-i ja 1 IP SSL-i ühendused sisalduv
Integreeritud laadi koormusetasakaalustusteenuse||X|X|X|X
[Alati](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Ajastatud varukoopiaid](../articles/app-service-web/web-sites-backup.md)||||Üks kord päevas|Üks kord igale 5 minutit<sup>8</sup>
[Mastaabi automaatselt](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
[Azure'i Scheduler](https://azure.microsoft.com/services/scheduler/) tugi||X|X|X|X
[Lõpp-punkti jälgimine](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Lavastus Slots (eelvaade)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Kohandatud domeenide rakenduse kohta</a>||500|500|500|500
SLA||<p>|99,9%|99,95%<sup>10</sup>|99,95%<sup>10</sup>

<sup>1</sup> Rakenduste ja salvestusmahu on rakenduse teenusleping kohta, kui teisiti.  
<sup>2</sup> Rakendused, mida saate hostida need masinad tegelikku arvu sõltub rakendused tegevuse, suurus ja seadme eksemplari vastava ressursi kasutamine.  
<sup>3</sup> Spetsiaalne eksemplaride võib olla erineva suurusega. Üksikasjalikumat teavet teemast [Rakenduse teenuse hinnad](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup> Premium taseme võimaldab kuni 50 arvutab eksemplarid (saadavuse) ja 500 GB vaba kettaruumi rakenduse teenuse keskkonnas kasutamisel ja 20 arvutada eksemplarid ja 250 GB salvestusruumi teisiti.  
<sup>5</sup> Talletuslimiidi on kogu sisu maht üle kõik rakendused sama rakenduse teenusleping. Rohkem mäluruumi suvandid on saadaval [rakenduse teenuse](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage) keskkonnas  
<sup>6</sup> Järgmiste ressursside piiravad füüsilised ressursid sihtotstarbeline aknad (eksemplari suurus ja eksemplaride arv).  
<sup>7</sup> Kui muudate tavaline astme kaks eksemplarides rakendus, on teil 350 samaaegseid ühendused iga kaks korda.  
<sup>8</sup> Premium taseme võimaldab varukoopia intervallide allapoole kuni 5 minutit iga rakenduse teenuse keskkonnas kasutamisel ja 50 korda päevas muul viisil.  
<sup>9</sup> Käivitage kohandatud täitmisfailid ja/või skriptide nõudmisel ajakava või pidevalt tausta tööülesandeks sees oma rakenduse eksemplari. Alati on pidev WebJobs täitmiseks vaja. Azure'i Scheduler tasuta või Standard on vaja ajastatud WebJobs. Pole eelmääratletud piiratud arvu WebJobs, mida saate kasutada ka rakenduse eksemplari, kuid on praktiline piirangud, mis sõltuvad sellest, mis rakenduse koodi üritab teha.   
<sup>10</sup> SLA sätestatud juurutuste, mida kasutada mitmes eksemplaris Azure'i liikluse haldur 99,95% konfigureeritud Tõrkesiirde.  
