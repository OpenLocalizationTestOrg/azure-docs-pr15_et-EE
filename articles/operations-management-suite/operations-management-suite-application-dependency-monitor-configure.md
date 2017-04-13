<properties
   pageTitle="Rakenduse sõltuvus kuvari (ADM) konfigureerimine Operations Management Suite (OMS) | Microsoft Azure'i"
   description="Sõltuvus kuvari (ADM) on toimingud halduse komplekti (OMS) lahenduse, mis automaatselt leiab operatsioonisüsteemides Windows ja Linux komponendid ja teenuste suhtlemine kaardid.  Selles artiklis on toodud üksikasjad juurutamine ADM keskkond ja kasutada erinevaid stsenaariumeid lähemalt."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Konfigureerida rakendus sõltuvus kuvari lahenduse toimingud halduse komplekti (OMS)
![Teatiseikoon haldus](media/operations-management-suite-application-dependency-monitor/icon.png) Sõltuvus kuvari (ADM) automaatselt leiab operatsioonisüsteemides Windows ja Linux komponendid ja kaardid suhtlemine teenused. See võimaldab teil vaadata oma servereid, nagu te arvate neist – ühendatud võrkudega, et pakkuda kriitiliste teenuste.  Rakenduse sõltuvus kuvari näitab seoseid serverites protsessid, ja pordid üle kõik TCP ühendatud arhitektuur pole konfigureerimine vajalik erinevalt agent installi.

Selles artiklis kirjeldatakse konfigureerida rakendus sõltuvus jälgimine ja kasutuselevõtt agentide üksikasju.  ADM kasutamise kohta leiate teemast [rakenduse abil kuvari sõltuvus lahenduse toimingud halduse komplekti (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Rakenduse sõltuvus kuvari praegu privaatne eelvaade.  Saate taotleda juurdepääsu ADM privaatne eelvaade [https://aka.ms/getadm](https://aka.ms/getadm)juures.
>
>Privaatne eelvaate, on kõik OMS kontod ADM. piiramatu juurdepääs  ADM sõlmed on tasuta, kuid Log Analytics andmete AdmComputer_CL ja AdmProcess_CL tüüpi on Mahupõhised nagu mis tahes muu lahendus.
>
>Pärast ADM sisestab avaliku eelvaade, oleks saadaval vaid tasulisi klientidele ülevaate ja analüüsi OMS hinnad kavas.  Tasuta taseme kontod piirduvad 5 ADM sõlmed.  Kui osalevad privaatne eelvaade ja on registreeritud OMS hinnad kavas ADM viimisel avaliku eelvaate, keelatakse ADM sel ajal. 



## <a name="connected-sources"></a>Ühendatud allikad
Järgmises tabelis kirjeldatakse ühendatud allikad, mida toetavad ADM lahendus.

| Ühendatud allikas | Toetatud | Kirjeldus |
|:--|:--|:--|
| [Windowsi agentide](../log-analytics/log-analytics-windows-agents.md) | Jah | ADM analüüsib ja kogub andmeid Windows agent arvutitest.  <br><br>Pange tähele, et OMS agent, Lisaks nõuavad Windows agentide sõltuvus Microsoft Agenti.  [Toetatud operatsioonisüsteemid](#supported-operating-systems) operatsioonisüsteemi versiooni täieliku loendi leiate. |
| [Linux agentide](../log-analytics/log-analytics-linux-agents.md) | Jah | ADM analüüsib ja kogutud andmete Linux agent arvutitest.  <br><br>Pange tähele, et OMS agent, lisaks Linux agentide nõua sõltuvus Microsoft Agenti.  [Toetatud operatsioonisüsteemid](#supported-operating-systems) operatsioonisüsteemi versiooni täieliku loendi leiate. |
| [SCOM rühma](../log-analytics/log-analytics-om-agents.md) | Jah | ADM analüüsib ja kogub andmeid Windows ja Linux agentide ühendatud SCOM haldus jaotises. <br><br>OMS SCOM agent arvutist otse ühendus on nõutav. Andmeid ei saadeta otse OMS hoidla haldus rühmast edastada.|
| [Azure storage konto](../log-analytics/log-analytics-azure-storage.md) | Ei | ADM kogub andmete agent arvutid, seega ei ole see Azure storage koguda andmeid. |

Pange tähele, et ADM toetab ainult 64-bitiste platvormide.

Windows Microsoft jälgimise Agent (MMA) kasutavad nii SCOM ja OMS koguda ja saata jälgida andmeid.  (See agent nimetatakse SCOM Agent, OMS Agent, MMA või otsene Agent, olenevalt konteksti.)  SCOM ja OMS pakkuda muu välja ruut versioonide MMA, kuid neid versioone, saate iga aruande SCOM, OMS või mõlemad.  Linux, OMS Agent Linux kogub ja saadab OMS andmete jälgimine.  Saate ADM serverite OMS otsese agentide või serverites, mis on lisatud OMS SCOM halduse rühmade kaudu.  Selleks, et need dokumendid me suunab kõik need agentide – klõpsake Linux või Windows, kas ühendatud SCOM MG või otse OMS – "OMS agendina", kui konkreetse juurutamise agendi nimi on vaja kontekstis.

ADM agent Edasta kõik andmed, ise ja tulemüürid või pordid muudatused ei ole vaja.  ADM on alati andmete OMS agent OMS, et otse või OMS lüüsi kaudu.

![ADM agentide](media/operations-management-suite-application-dependency-monitor/agents.png)

Kui olete ühendatud OMS SCOM klientide rühmaga haldus:

- Kui teie SCOM agentide saab Interneti ühenduse OMS, ei konfiguratsiooni on vaja.  
- Kui teie SCOM agentide ei pääse Interneti kaudu OMS, peate konfigureerimine OMS lüüsi SCOM töötamiseks.
  
Kui kasutate OMS otsese Agent, peate konfigureerida ühenduse OMS või OMS lüüsi ise OMS Agent.  Lüüsi OMS saab alla laadida [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Duplikaatandmete vältimine

Kui olete SCOM klient, te peaksite konfigureerima teie SCOM agentide edastada OMS või andmete esitatakse kaks korda.  ADM, tulemuseks arvutit kuvata loendis seadme kaks korda.

Konfiguratsiooni OMS tulebki ainult ühes järgmistest kohtadest:

- SCOM konsooli Operations Management Suite paani hallatavate arvutites
- Azure'i töö ülevaated MMA atribuutide konfigureerimine

Nii konfiguratsioone kasutades sama tööruumi iga põhjustab kattuvat andmed. Vastuolulised konfiguratsiooni (üks ADM lahenduse lubatud ja teine ilma), mis võivad vältida andmete voolamist ADM täielikult võib põhjustada abil nii erinevatesse tööruumidesse.  

Pange tähele, et ka juhul, kui enese pole SCOM konsooli OMS konfiguratsiooni, kui ka eksemplari rühma, näiteks "Windows Serveri eksemplari rühma" on aktiivne, endiselt tekkida arvuti vastuvõtmise OMS konfiguratsiooni SCOM kaudu.



## <a name="management-packs"></a>Juhtimise tuvastamine
ADM aktiveerimisel kuvatakse OMS tööruumis 300KB halduspaketi saadetakse kõigi selle Microsofti jälgimise agentide mõnes kindlas tööruumis.  Kui kasutate SCOM agentide [ühendatud rühma](../log-analytics/log-analytics-om-agents.md), juurutatakse ADM halduspaketi SCOM kaudu.  Kui agentide on otsene Interneti-ühendus, MP esitatud OMS.

MP nimeks Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  See on kirjutatud *%Programfiles%\Microsoft jälgimise Agent\Agent\Health State\Management hoolduspaketi\*.  Kasutatavad halduspakett toiminguhalduri andmeallikas on *% programmi files%\Microsoft jälgimis Agent\Agent\Health teenuse State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfigureerimine
Lisaks Windows ja Linux arvutisse on installitud ja ühendatud OMS agent, sõltuvus Agent Installeri peavad olema alla laadida ADM lahendus ja seejärel juurkausta või administraator iga hallatava serverisse installitud.  Kui ADM agent on installitud aruandlus OMS serveris, kuvatakse ADM sõltuvus kaartide 10 minuti jooksul.  Kui teil on küsimusi, saatke [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migreerimine versioonist BlueStripe FactFinder versiooni
Rakenduse sõltuvus kuvari toob BlueStripe tehnoloogia üheks OMS etapist. FactFinder olemasolevad kliendid on endiselt toetatud, kuid pole enam osta.  Sõltuvus Agent seda eelvaateversiooni saate edastada ainult OMS.  Kui olete praeguse FactFinder klient, Loetlege testi serverite ADM, mida haldab FactFinder kogum. 

### <a name="download-the-dependency-agent"></a>Sõltuvus Agent allalaadimine
Lisaks Microsoft Agent (MMA) ja OMS Linux Agent, mis võimaldab arvuti ja OMS vahelise ühenduse, analüüsitud rakenduse sõltuvus kuvari kõigisse arvutitesse peab olema installitud sõltuvus Agent.  Linux, peab enne sõltuvus Agent installitud OMS Agent Linuxi jaoks. 

![Rakenduse sõltuvus kuvari paani](media/operations-management-suite-application-dependency-monitor/tile.png)

Selleks, et laadida sõltuvus Agent, klõpsake **Lahenduse konfigureerimine** **Rakenduse sõltuvus kuvari** paani avamiseks **Sõltuvus Agent** tera.  Sõltuvus Agent tera on lingid aknad ja agentide Linuxi jaoks. Klõpsake vastavat linki iga agent alla laadida. Järgmistest jaotistest üksikasjalikku installimise agent süsteemid on erinevad.

### <a name="install-the-dependency-agent"></a>Installige sõltuvus Agent

#### <a name="microsoft-windows"></a>Microsoft Windows
Installimine või desinstallimine agent peavad administraatoriõigused.

Sõltuvus Agent on installitud Windowsi arvutite ADM – Agent-Windows.exe. Kui käivitate selles käivitatava ilma suvandid, seejärel hakkavad viisard, mida saate jälgida installi interaktiivseks.  

Järgmiste juhiste abil saate iga Windowsi arvuti sõltuvus Agent installimine.

1.  Veenduge, et OMS agent on installitud, veebisaidil ühendamine arvutite otse OMS juhiste abil.
2.  Laadige alla Windows agent ja käivitage see järgmise käsu abil.<br>*ADM – Agent-Windows.exe*
3.  Järgige viisardi installida agent.
4.  Kui sõltuvus Agent ei käivitu, kontrollige logid tõrke üksikasjalikku teavet. Logige kataloogi on Windowsi agentide *C:\Program Files\BlueStripe\Collector\logs*. 

Juhtpaneeli kaudu administraator saab desinstallida sõltuvus Agent for Windows.


#### <a name="linux"></a>Linux
Installima ja konfigureerima agent on vaja root ühendust.

Sõltuvus Agent on installitud Linux arvutite ADM – Agent-Linux64.bin shell script koos omas ekstraktimine kahendarvuks. Saate käivitada faili sh või lisada käivitada fail ise õigused.
 
Järgmiste juhiste abil saate sõltuvus Agent iga Linux arvutisse installida.

1.  Veenduge, et OMS agent on installitud, kasutades juhiseid [koguda ja Linux arvutitest andmete haldamine.  See peab olema installitud enne Linux sõltuvus Agent](https://technet.microsoft.com/library/mt622052.aspx).
2.  Installige Linux sõltuvus agent root järgmise käsu abil.<br>*sh ADM – Agent-Linux64.bin*.
3.  Kui sõltuvus Agent ei käivitu, kontrollige logid tõrke üksikasjalikku teavet. Logige kataloogi on Linux agentide */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Sõltuvus Agent Linux desinstallimine
Täielikult eemaldada Linux sõltuvus Agent, peate eemaldama agent ise ja puhverserveri, mis installitakse automaatselt agent.  Nii ühe järgmise käsu abil saate desinstallida.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Käsurea installimine
Installimise sõltuvus kuvari agent vaikevalikute abil annab eelmise jaotise juhiseid.  Järgmistes jaotistes anda suuniseid installimist agent käsureal, kasutades kohandatud suvandeid.

#### <a name="windows"></a>Windows
Järgmises tabelis suvandite abil saate installi mõne käsurea kaudu. Loendi installi lipud käivitage installer ja selle /? lipu järgmiselt.

    ADM-Agent-Windows.exe /?

| Lipp | Kirjeldus |
|:--|:--|
| /S | Vaikne install koos pole kasutaja viipasid. |

Failid Windows sõltuvus Agent paigutatakse *C:\Program Files\BlueStripe\Collector* vaikimisi.


#### <a name="linux"></a>Linux
Järgmises tabelis suvandite abil saate installida. Lipud käivitada installimise loendi kuvamiseks installimise programm – abil lipp järgmiselt.

    ADM-Agent-Linux64.bin -help

| Lipu kirjeldus
|:--|:--|
| -s | Vaikne install koos pole kasutaja viipasid. |
| --kontrollimine | Kontrollib, õiguste ja operatsioonisüsteemi, kuid ei saa installida agent. |

Sõltuvus Agent failid paigutatakse järgmised kataloogid.

| Failide | Asukoht |
|:--|:--|
| Core failid | /usr/lib/bluestripe-Collector |
| Logifailide | /var/opt/Microsoft/Dependency-agent/log |
| Config failid | /etc/opt/Microsoft/Dependency-agent/config |
| Teenuse täitmisfailid | /sbin/bluestripe-Collector<br>/sbin/bluestripe-Collector-Manager |
| Binaarsed salvestusruumi failid | /var/opt/Microsoft/Dependency-agent/Storage |



## <a name="troubleshooting"></a>Tõrkeotsing
Kui teil tekib probleeme rakenduse sõltuvus monitoris, saate tõrkeotsingu teabe kogumine mitme komponendid, kasutades järgmist teavet.

### <a name="windows-agents"></a>Windowsi agentide

#### <a name="microsoft-dependency-agent"></a>Microsoft sõltuvus Agent
Sõltuvus agendi tõrkeotsingu andmete loomiseks avage administraatorina käsuviiba ja käivitage järgmine käsk CollectBluestripeData.vbs skript.  Saate lisada--abi lipu, et kuvada veel suvandeid.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Tugiteenuste andmed paketti salvestatakse praeguse kasutaja kataloogis % KASUTAJAPROFIILI %.  Saate kasutada - faili <filename> võimalus salvestage see mõnda muusse asukohta.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Microsoft sõltuvus Agent halduspaketi MMA
Sõltuvus Agent halduspaketi töötab Microsoft Management Agent sees.  See saab sõltuvus Agent ja edastab selle ADM pilveteenusesse.
  
Veenduge, et halduspakett toiminguhalduri laaditakse, toimige järgmiselt.

1.  Otsige faili nimega Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp C:\Program Files\Microsoft jälgimise Agent\Agent\Health State\Management hoolduspaketi.  
2.  Kui fail pole ja agent on ühendatud SCOM halduse rühma, seejärel veenduge, et see on imporditud SCOM, märkides ruudu halduse paketid haldamine tööruumis toimingud konsooli.

ADM MP kirjutab sündmuste toimingute halduri Windowsi sündmuselogi.  Logi saab [otsida OMS](../log-analytics/log-analytics-log-searches.md) kaudu süsteemi Logi lahenduse, kus saate konfigureerida milliseid logifailid üles laadida.  Kui silumine sündmused on lubatud, kirjutada need rakenduse sündmuste logi Sündmuse allikas *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Agenti jälgimine
Koguda diagnostika jälgi, avage administraatorina käsuviiba ja käivitage järgmine käsk: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

C:\Windows\Logs\OpsMgrTrace kirjutada jälgi.  Soovi korral saate peatada jälgimine StopTracing.cmd abil.


### <a name="linux-agents"></a>Linux agentide

#### <a name="microsoft-dependency-agent"></a>Microsoft sõltuvus Agent
Sõltuvus agendi, logige sisse kontoga, millel on samad õigused, sudo või root tõrkeotsingu andmete loomiseks ja käivitage järgmine käsk.  Saate lisada--abi lipu, et kuvada veel suvandeid.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Tugiteenuste andmed paketti salvestatakse /var/opt/microsoft/dependency-agent/log (kui root) jaotises esindaja installikaust või töö script (kui mitte-root) kasutaja home kataloogi.  Saate kasutada--faili <filename> võimalus salvestage see mõnda muusse asukohta.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Microsoft sõltuvus Agent Fluentd Linuxi lisandmoodul
Sõltuvus Agent Fluentd lisandmooduli käivitatakse OMS Linux Agent sees.  See saab sõltuvus Agent ja edastab selle ADM pilveteenusesse.  

Logide kirjutada järgmised kaks failid.

- /var/opt/Microsoft/omsagent/log/omsagent.log
- /var/log/Messages

#### <a name="oms-agent-for-linux"></a>OMS Agent Linuxi
OMS Linux serveritega ühenduse tõrkeotsingu ressursi leiate siit: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

Logide OMS agent Linux asuvad */var/valida/microsoft/omsagent/log/*.  

Logide jaoks omsconfig (agent konfiguratsioon) asuvad */var/valida/microsoft/omsconfig/log/*.
 
Logi OMI ja SCX komponendid, mis pakuvad mõõdikute jõudlusandmeid asuvad */var/valida/omi/log/* ja */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Andmete kogumine
Võib juhtuda iga agent umbes 25MB päevas, olenevalt sellest, kuidas keerukas teie süsteemi sõltuvused on edastada.  Iga 15 sekundi järel saadetakse iga agent ADM sõltuvus andmed.  

ADM Agent tavaliselt tarbib muutmälu 0,1% ja 0,1% süsteemi CPU.


## <a name="supported-operating-systems"></a>Toetatud opsüsteemid
Järgmistes jaotistes on loetletud toetatud operatsioonisüsteemide sõltuvus agent.   32-bitine arhitektuur pole toetatud opsüsteemide jaoks.

### <a name="windows-server"></a>Windows Server
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windowsi töölaua
- Märkus: Windows 10 veel ei toetata
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Punane rolli Enterprise Linux, CentOS Linux ja Oracle'i Linux (RHEL tuum)
- Ainult vaike- ja SMP Linuxi tuum väljalasete on toetatud.
- Mittestandardsed tuum annab, näiteks PAE ja Xen, mis tahes Linux jaotuse pole toetatud. Näiteks väljaanne string "2.6.16.21-0.8-xen" süsteemi ei toetata.
- Kohandatud tuumad, sh recompiles standard tuumad, ei toetata
- CentOS pluss tuum ei toetata.
- Oracle'i purunematu tuum (UEK) käsitletakse erinevaid jaotist allpool.


#### <a name="red-hat-linux-7"></a>Punane rolli Linux 7
| Opsüsteemi versioon | Tuum versioon |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Punane rolli Linux 6
| Opsüsteemi versioon | Tuum versioon |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6,7 | 2.6.32-573 |
| 6,8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Punane rolli Linux 5
| Opsüsteemi versioon | Tuum versioon |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5,9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle'i Enterprise Linux w / purunematu tuum (UEK)

#### <a name="oracle-linux-6"></a>Oracle'i Linux 6
| Opsüsteemi versioon | Tuum versioon
|:--|:--|
| 6.2 | Oracle'i 2.6.32-300 (UEK R1) |
| 6.3 | Oracle'i 2.6.39-200 (UEK R2) |
| 6.4 | Oracle'i 2.6.39-400 (UEK R2) |
| 6.5 | Oracle'i 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle'i 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle'i Linux 5

| Opsüsteemi versioon | Tuum versioon
|:--|:--|
| 5.8 | Oracle'i 2.6.32-300 (UEK R1) |
| 5,9 | Oracle'i 2.6.39-300 (UEK R2) |
| 5.10 | Oracle'i 2.6.39-400 (UEK R2) |
| 5.11 | Oracle'i 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Opsüsteemi versioon | Tuum versioon
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Opsüsteemi versioon | Tuum versioon
|:--|:--|
| 10 SP4. | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnostika-ja kasutamine
Microsoft kogub automaatselt kasutus- ja jõudluse andmeid rakenduse sõltuvus kuvari teenuse kasutamise kaudu. Microsoft kasutab neid andmeid sisestada ja parandada kvaliteeti, turvalisus ja terviklus rakenduse sõltuvus kuvari teenuse. Andmete sisaldab teavet teie tarkvara operatsioonisüsteemi ja versioon nagu konfiguratsiooni ja sisaldab ka IP-aadress, DNS-i nimi ja töökoha nimi probleemide tõrkeotsinguks ja lahendamiseks täpne ja tõhus võimaluste pakkumiseks. Me ei kogu nimesid, aadresse või muud kontaktteavet.

Andmete kogumise ja kasutamise kohta leiate lisateavet teemast [Microsoft Online Services privaatsusavaldus](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Järgmised sammud
- Siit saate teada, kuidas [kasutada rakenduse sõltuvus kuvari](operations-management-suite-application-dependency-monitor.md) üks kord on juurutada ja konfigureerida.
