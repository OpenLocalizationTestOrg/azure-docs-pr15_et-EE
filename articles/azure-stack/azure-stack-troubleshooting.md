<properties
    pageTitle="Microsoft Azure'i virnas tõrkeotsing | Microsoft Azure'i"
    description="Azure'i virnas tõrkeotsing."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure'i Virnlintdiagrammil tõrkeotsing

Selles dokumendis on toodud Azure'i virnas seotud levinud tõrkeotsingu teavet.

Parima kasutuskogemuse saamiseks veenduge, et juurutamise keskkonna vastab kõik [nõuded](azure-stack-deploy.md) ja [ettevalmistused](azure-stack-run-powershell-script.md) enne installimist. 

Soovitused tõrkeotsingu, mida on kirjeldatud selles jaotises on tuletatud mitmest allikast ja võib või ei pruugi teie kindla probleemi lahendada. Kui teil esineb probleeme pole dokumenteerida, veenduge, et kontrollida täiendava abi ja lisateabe [Azure'i virnas MSDN-i Foorum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) .

Koodi näited on olemas, nagu on ja ei saa tagada oodatud tulemusi. See jaotis kehtib tootega täiustamine rakendatavate sagedased muudatused ja värskendused.

  

## <a name="known-issues"></a>Teadaolevad probleemid

 - Võidakse kuvada järgmised-lõpetatakse tõrked juurutamisel, mis mõjutavad juurutamise edu.
     - "Tundmatu termin"C:\WinRM\Start-Logging.ps1""
     - "Autonoomsest EceAction: ei saa indekseerida null massiivi sisse" 
     - "InvokeEceAction: ei saa siduda argumendi parameeter 'Teade', kuna see on tühi string."
 - Teile võidakse kuvada juurutamise nurjuda samm 60.61.93 viga "Rakendus identifikaator"URI"ei leitud." Kuidas rakenduste registreeritud Azure Active Directory on selline käitumine.  Kui kuvatakse järgmine tõrketeade, jätkake uuesti [installimine skripti](azure-stack-rerun-deploy.md) juhise juurest 60.61.93 kuni juurutamise on lõpule viidud.
 - Näete, et **VirtualMachine RÜHMAGA** kategoorias kuvatakse kuvatakse Marketplace'ist ressursi **Kättesaadavus** – see on ainult kosmeetiline probleem.
 - Kui loote uue virtuaalse masina portaalis juhises **põhitõed** vaikimisi SSD salvestusruumi suvand.  Seda sätet muuta HDD või **suurus** järgu VM juurutus, ei kuvata saadaval, valige ja juurutamise jätkamiseks VM suurused. 
 - Kuvatakse vaikimisi MAS-CON01 VM enam installitud AzureRM PowerShelli moodulid (TP1 see oli nimega ClientVM). Selline käitumine on teadlikult, kuna on mõnel muul viisil [installida need moodulid](azure-stack-connect-powershell.md)ja ühendamine.  
 - Näete, et **Microsoft.Insights** ressursi pakkuja ei ole automaatselt registreeritud rentniku tellimused. Kui soovite näha jälgida andmeid juurutatud rentniku VM, käivitage järgmine käsk: PowerShelli (pärast saate [installida ja ühendage](azure-stack-connect-powershell.md) rentniku): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Kuvatakse funktsionaalsus portaalis eksportimiseks ressursi rühmad, aga teksti on kuvatud ja saadaval ekspordi.      
 - Saate alustada salvestusruumi ressursse, mis on suurem kui saadaval piirmäära juurutamise.  Selle rakendamine ebaõnnestub ja konto ressursside peatatakse.  Saadaval on kaks parandamise võimalust.
     - Teenuse administraator saab suurendada kvoodi, kuigi muudatused kuvatakse jõustuvad kohe ja tihti kuluda kuni tund levitada.
     - Teenuse administraator saate luua mõne lisandmooduli lepinguga täiendavad kvoodi, mis rentniku seejärel saate lisada tellimuse.
 - Luua VMs Azure'i virnas keskkonnad identiteedi "Azure - Hiina" portaali kasutamisel ei kuvata VM suurused saadaval, valige **suurus** etapil VM juurutamise ja ei saa jätkata juurutamise.
 - Juurutamise tõrge portaalis, võidakse kuvada, kui VM tegelikult on edukalt juurutatud.
 - Kui kustutate pakkumine, leping või tellimus, VMs ei saa kustutada.
 - Kuvatakse VM laiendid rakenduses kuvatakse Marketplace'ist.
 - Ei saa juurutada salvestatud VM pildilt VM.
 - Rentnikud võidakse kuvada teenuseid, mis ei kuulu oma tellimust.  Kui rentnikud proovivad juurutada need ressursid, nad tõrketeate.  Näide: Rentniku tellimus sisaldab ainult salvestusruumi ressursid.  Rentniku kuvatakse võimalus luua muud ressursid, nt VMs.  Selle stsenaariumi korral, kui rentniku proovib juurutamine VM, nad saavad teade VM ei saa luua. 
 - Installimisel TP2, peaksite ei aktiveeri selle hosti OS VHD, kui käivitate Azure virnas häälestamise skripti või võidakse kuvada sõnumid, milles Windowsi tõrke aegub varsti.


## <a name="deployment"></a>Juurutamine

### <a name="deployment-failure"></a>Juurutamine nurjus
Kui installimisel tekib tõrge, Azure'i virnas installer võimaldab teil nurjunud installimise jätkamiseks [uuesti juurutamise juhiseid](azure-stack-rerun-deploy.md)järgides.

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Juurutamise lõpus PowerShelli seanss on endiselt avatud ja ei kuvata mõnda väljund

Selline käitumine on tõenäoliselt lihtsalt PowerShelli käsuaknas vaikekäitumise tulemus, kui see on valitud. POC juurutamise on tegelikult õnnestus, kuid script on peatatud akna valimisel. Saate kontrollida, see on nii otsides sõna "valige" tiitliribal käsuviiba aken.  Selle valiku tühistamiseks vajutage paoklahvi (ESC) ja selle järele peaks kuvatama lõpetamise sõnum.

## <a name="templates"></a>Mallid

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure'i malli ei juurutada Azure virnas

Veenduge, et:

- Malli peate kasutama Microsoft Azure'i teenus, mis on juba olemas või eelvaates Azure'i virnas.
- API-d kasutatakse teatud ressursi ei toeta Azure virnas kohaliku eksemplari ja, et olete suunatud sobiv asukoht ("Kohalik" sisse Azure virnas Technical Preview (TP) 2, vs "Ida-USA" või "Lõuna India" Azure).
- [Selles artiklis](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) Test-AzureRmResourceGroupDeployment cmdlet-käsud, mis jaole juba väike erinevused Azure'i ressursihaldur süntaksi kohta vaadake üle.

Azure'i virnas Mallid, mis on juba antud [GitHub hoidla](http://aka.ms/AzureStackGitHub/) abil aitavad teil alustada.

## <a name="virtual-machines"></a>Virtuaalmasinates

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Pärast käivitamist minu Microsoft Azure'i virnas POC host, kõik minu rentnikud VMs Hyper-V halduri läinud ja naaske automaatselt pärast veidi ootab?

Kui süsteemi tagasi tuleb salvestusruumi alasüsteemi ja RPs vajate järjepidevuse määratlemiseks. Vajalik aeg sõltub riistvara ja andmed kasutusel, kuid võib olla mõne aja pärast taaskäivitage host rentniku VMs ta tagasi tuleb ja tuvastada.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Mõned virtuaalmasinates kustutanud, kuid näha siiski VHD faili kettal. Selline käitumine peaks?

Jah, see on oodatud käitumine. Kuna sellisel viisil loodud:

- VM kustutamisel ei kustutata VHDs. Ketast on eraldi ressursside ressursirühma.
- Kui salvestusruumi konto kustutamine, kustutamise on nähtav kohe kaudu Azure'i ressursihaldur (portaalis PowerShelli), kuid ketast, võib sisaldada veel hoida salvestusruumi kuni Prügikoristus töötab.

Kui näete VHDs "harva", on oluline teada, kui need on osa kausta Kustutatud salvestusruumi konto. Kui salvestusruumi kontot ei kustutatud, on tavaline, et need on ikka olemas.

Lugege lisateavet konfigureerimine säilituspoliitika lävi [Halda salvestusruumi kontode](azure-stack-manage-storage-accounts.md)kohta.

## <a name="installation-steps"></a>Installimise juhiseid.
Azure'i virnas installimisjuhiseid järgmine teave võib olla kasulik tõrkeotsinguks:  

| Index | Nimi | Kirjeldus|
| ----- | ----- | -----|
|0.11 | (DEP) Kinnitage füüsilise masinad | Riistvara ja OS konfiguratsiooni füüsilise sõlmed kontrollimine. |
| 0,12 | (DEP) Füüsilise masinad networking POC konfigureerimine | Konfigureerida virtuaalse võrgu parameetrid ja liideste. |
| 0,14 | (DEP) Domeeni juurutamine | Juurutada Active Directory Virtual arvutisse. |
| 0,15 | (DEP) Domeeni server konfigureerimine | Seadistada domeeni serveri turberühmad jne. |
| arvu 0,16 | (DEP) Füüsilise seadme konfigureerimine | Võrgunduse ja Liitu domeeni häälestamise kohalike administraatorite konfigureerimine. |
| 0,18 | (LÕPETA) Salvestusruumi kobar konfigureerimine | Saate luua salvestusruumi kobar, luua salvestusruumi kausta ja faili server. |
| 0,19 | (CPI) Häälestamise struktuuri taristu | Saate häälestada eeltingimused struktuuri juurutamiseks. |
| 0,21 | (NETO) BGP ja NAT häälestamine | Installib BGP ja NAT - vaja ainult ühe sõlme. |
| 0,22 | (NETO) NAT ja kellaaja serveri konfigureerimine | Sünkroonib aeg server ja konfigureerib NAT kirjed. |
| 40.41 | (CPI) Külalisena VMs loomine | Saate luua VMs haldus. |
| 40,42 | (FBI) PowerShelli JEA häälestamine | Kõikide rollide PowerShelli JEA seadistamine |
| 40.43 | (FBI) Azure'i virnas sertimiskeskus häälestamine | Installib Azure virnas sertimiskeskus. |
| 40.44 | (FBI) Azure'i virnas sertimiskeskus konfigureerimine | Konfigureerib Azure virnas sertimiskeskus. |
| 40,45 | (NETO) Klõpsake VMs NC häälestamine | Installib NC külaline VMs |
| 40.46 | (NETO) Konfigureerige NC VMs | Konfigureerige NC külaline VMs |
| 40,47 | (NETO) Külalisena VMs konfigureerimine | Halduse VMs NC ACL-ide konfigureerimine. |
| 60.61.81 | (FBI) FabricRing nõutav Azure virnas struktuuri Helisemine teenuste - juurutamine | Loob VIP FabricRing jaoks |
| 60.61.82 | (FBI) Azure'i virnas struktuuri Helisemine teenuste juurutamine - struktuuri Ring kobar juurutamine | Saate installida ja konfigureerida Azure virnas struktuuri Ring kobar. |
| 60.61.83 | (FBI) Ressursi pakkujate administraator laiendid juurutamine | Ressursi pakkujate administraator laiendid installimine |
| 60.61.84 | (ACS) Seadistada Azure ühtsete salvestusruumi sõlm tase. | Installida ja konfigureerida Azure ühtsete salvestusruumi sõlm tasemes. |
| 60.61.85 | (ACS) Seadistada Azure ühtsete salvestusruumi kobar tase. | Installida ja konfigureerida Azure ühtsete salvestusruumi kobar tasemes. |
| 60.61.86 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | Eeltingimuste InfraServiceController |
| 60.61.87 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | CPI eeltingimused |
| 60.61.88 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | Eeltingimuste ASAppGateway |
| 60.61.89 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | Salvestusruumi kontrolleril eeltingimused |
| 60.61.90 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | Eeltingimuste HealthMonitoring |
| 60.61.91 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | Euroopa eeltingimused |
| 60.61.92 | (FBI) Nõutav Azure virnas struktuuri Ring kontrolleril teenuste - juurutamine | PMM eeltingimused |
| 60.61.93 | (Katal) AzureStack teenuse põhisumma loomine | Luua Azure Graphi rakenduste ja teenuste põhisumma AAD. |
| 60.61.94 | (NETO) GW VMs häälestamine | Installib GW külaline VMs. |
| 60.61.95 | (NETO) GW VMs konfigureerimine | Konfigureerib GW külaline VMs. |
| 60.61.96 | (NETO) IDNS hosts juurutamine | IDNS taristu hosts juurutamine |
| 60.61.97 | (NETO) IDNS konfigureerimine | IDNS rolli konfigureerimine |
| 60.61.98 | (FBI) WSUS VMs häälestamine | Installib WSUS server külaline VMs. |
| 60.61.99 | (FBI) WSUS VMs konfigureerimine | Konfigureerib WSUS server külaline VMs. |
| 60.61.100 | (FBI) SQL Azure'i VMs häälestamine | Installib Azure SQL serveri külaline VMs |
| 60.61.101 | (Katal) Häälestamise eeltingimuste oli VMs. | Häälestab eeltingimused Microsoft Azure'i virnas külaline VMs kohta. |
| 60.61.102 | (Katal) Installiprogramm ei VMs | Installib Microsoft Azure'i virnas külaline VMs. |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.121 | (FBI) Ressursi pakkujad ja kontrollerid juurutamine | Installib ressursi pakkujad ja kontrollerid |
| 60.120.122 | (FBI) Selle domeenikontrolleri konfigureerimine | Konfigureerib kontrollerid |
| 60.120.123 | (Katal) Konfigureerimine on VMs | Microsoft Azure'i Virnlintdiagrammil konfigureeritakse külaline VMs. |
| 60.120.124 | (Katal) Azure'i virnas AAD konfigureerimine. | Konfigureerib Azure virnas Azure AD. |
| 60.120.125 | (Katal) ADFS-i installimine | Installib Active Directory Federation Services (ADFS) |
| 60.120.126 | (Katal) ADFS-i/Graphi installimine | Installib Azure virnas graafik |
| 60.120.127 | (Katal) ADFS-i konfigureerimine | Konfigureerib Active Directory Federation Services (ADFS) |
| 60.140.141 | (FBI) SRP konfigureerimine | Konfigureerib salvestusruumi ressursi pakkuja |
| 60.140.142 | (ACS) Azure'i ühtsete salvestusruumi konfigureerimine. | Konfigureerib Azure'i ühtsete salvestusruumi. |
| 60.140.143 | (FBI) Salvestusruumi kontode loomine | Looge kõik salvestusruumi kontod pakkujate kasutamiseks. |
| 60.140.144 | (FBI) Registreeruge SRP kasutus | Registreeruge salvestusruumi pakkuja kasutuse. |
| 60.140.145 | (CPI) Migreerimine CPI loodud VMs, Hosts ja kobar | Objektide loodud VMs, Hosts ja kobar migreerib CPI abil |
| 60.140.146 | (FBI) Windows Defenderi konfigureerimine | Konfigureerib Windows Defender |
| 60.160.161 | (E) Jälgimine Agent konfigureerimine | Konfigureerib Agent jälgimine |
| 60.160.162 | (FBI) NRP nõutav | Installib NRP eeltingimused |
| 60.160.163 | (FBI) NRP juurutamine | Installib NRP  |
| 60.160.164 | (FBI) NRP konfigureerimine | Konfigureerib NRP |
| 60.160.165 | (FBI) CRP nõutav | Installib CRP eeltingimused |
| 60.160.166 | (FBI) CRP juurutamine | Installib CRP |
| 60.160.167 | (FBI) CRP konfigureerimine | Konfigureerib CRP |
| 60.160.168 | (FBI) FRP nõutav | Installib FRP eeltingimused |
| 60.160.169 | (FBI) FRP juurutamine | Installib FRP  |
| 60.160.170 | (FBI) FRP konfigureerimine | Konfigureerib FRP |
| 60.160.174 | (FBI) Eelnevalt nõutud URP | Installib URP eeltingimused |
| 60.160.175 | (FBI) URP juurutamine | Installib URP  |
| 60.160.176 | (FBI) URP konfigureerimine | Konfigureerib URP |
| 60.160.171 | (FBI) MRP nõutav | Installib MRP eeltingimused |
| 60.160.172 | (FBI) MRP juurutamine | Installib MRP  |
| 60.160.173 | (FBI) MRP konfigureerimine | Konfigureerib MRP |
| 60.160.177 | (KV) KeyVault nõutav | Installib KeyVault eeltingimused |
| 60.160.178 | (KV) KeyVault juurutamine | Installib KeyVault  |
| 60.160.179 | (KV) KeyVault konfigureerimine | Konfigureerib KeyVault | 
| 60.190.191 | (FBI) Galerii konfigureerimine | Galerii konfigureerimine |
| 60.190.192 | (FBI) Struktuuri Helisemine teenuste konfigureerimine | Struktuuri Helisemine teenuste konfigureerimine |
| 60.221 | (FBI) Konsooli VMs häälestamine | Installib konsooli server külaline VMs. |
| 60.222 | (FBI) Konsooli VMs häälestamine | DVM sisu teisaldamiseks konsooli VM. |
| 251 | Valmistuda tulevaste host taaskäivitamisega | Poliitika määramine taaskäivitage arvuti |


## <a name="next-steps"></a>Järgmised sammud

[Korduma kippuvad küsimused](azure-stack-FAQ.md)
