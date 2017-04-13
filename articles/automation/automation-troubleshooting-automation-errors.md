<properties
   pageTitle="Azure'i automaatika vigade käsitlemise | Microsoft Azure'i"
   description="Sellest artiklist leiate põhilised tõrge käsitsemise juhiseid Azure automatiseerimine levinud vigade parandamine ja tõrkeotsing."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="automatiseerimise tõrge, tõrge töötlemine"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Tõrketöötluse Azure automatiseerimine levinud vigade näpunäited

See artikkel selgitab levinud Azure automatiseerimine vead võib tunnete ja soovitab võimalike tõrketöötluse juhiseid.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Azure'i automaatika tegevusraamatud töötamisel autentimise tõrkeotsing  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Stsenaarium: Kontole sisse logida Azure'i nurjus.

**Tõrge:** Kuvatakse tõrketeade "Unknown_user_type: Tundmatu kasutaja tüüp" töötamisel lisa-AzureAccount või Logi sisse – AzureRmAccount cmdlet-käsud.

**Tõrke põhjus:** See tõrge ilmneb juhul, kui mandaati varade nimi ei sobi või kui kasutajanimi ja parool, mida kasutasite setup automatiseerimise mandaati varade ei sobi.

**Tõrkeotsingu näpunäited:** Selleks, et otsustada, mis on vale, tehke järgmist.  

1. Veenduge, et teil pole erimärke, sh selle **@** automatiseerimine mandaati varade nimi Azure ühenduse loomiseks kasutatava märk.  

2. Vaadake, mida saate kasutada kasutajanimi ja parool, mis on talletatud Azure automatiseerimine mandaati teie kohaliku PowerShell ISE redaktoris. Saate seda teha, kui PowerShell ISE töötavad järgmised cmdlet-käsud:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Kui teie autentimine nurjub, tähendab see, et te pole häälestanud Azure Active Directory mandaat õigesti. Vaadake [Azure'i Azure Active Directory abil autentimine](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) ajaveebipostituse saada Azure Active Directory konto on õigesti häälestatud.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Stsenaarium: Ei leia Azure tellimuse

**Tõrge:** Kuvatakse tõrketeade "nimega tellimuse ``<subscription name>`` ei leitud" töötades valige-AzureSubscription või valige-AzureRmSubscription cmdlet-käsud.

**Tõrke põhjus:** See tõrge ilmneb juhul, kui tellimuse nime ei sobi või kui tellimuse üksikasjad üritavad Azure Active Directory kasutaja pole konfigureeritud tellimuse administraatoriks.

**Tõrkeotsingu näpunäited:** Selleks, et otsustada, kui on õigesti autenditud Azure ja juurdepääs tellimus, mida soovite valida, tehke järgmist.  

1. Veenduge, et käivitada enne käivitamist **Valige-AzureSubscription** cmdlet-käsk **Lisa-AzureAccount** .  

2. Kui endiselt kuvatakse see tõrketeade, koodi muuta, lisades jälgimise **Lisamine-AzureAccount** cmdlet-käsk **Get-AzureSubscription** cmdlet-käsk ja seejärel käivitada koodi.  Nüüd veenduge, et kui Get-AzureSubscription väljund sisaldab teie tellimuse üksikasjad.  
    * Kui väljund mis tahes Tellimuse üksikasjad ei kuvata, tähendab see, et tellimus pole veel lähtestatud.  
    * Kui näete väljund Tellimuse üksikasjad, veenduge, et te kasutate õige tellimuse nime või ID abil **Valige-AzureSubscription** cmdlet-käsk.   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Stsenaarium: Azure'i autentimine nurjus, kuna mitmikautentimise on lubatud

**Tõrge:** Kuvatakse tõrketeade "lisa-AzureAccount: AADSTS50079: nõutav on tugev autentimine registreerimine (tõendada üles)" kui autentimine Azure'i oma Azure kasutajanime ja parooliga.

**Tõrke põhjus:** Kui teil on mitmikautentimise Azure'i konto, ei saa kasutada rakendust Azure Active Directory Azure autentida.  Selle asemel, peate kasutama sert või teenuse põhisumma Azure autentida.

**Tõrkeotsingu näpunäited:** Azure'i teenuse halduse cmdletid serdi kasutamiseks viidata [loomine ja lisamine Azure'i teenuste haldamiseks sert.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Teenuse põhilise Azure'i ressursihaldur cmdlet-käskude kasutamiseks viidata [loomine teenuse põhilise Azure'i portaalis](./resource-group-create-service-principal-portal.md) ning [autentimisel teenuse põhilise Azure'i ressursihaldur.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Levinud probleemide tõrkeotsingu tegevusraamatud töötamisel

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Stsenaarium: Käitusjuhendi nurjub deserialized objekt

**Tõrge:** Teie käitusjuhendi nurjus tõrke "ei saa siduda parameeter ``<ParameterName>``. Ei saa teisendada soovitud ``<ParameterType>`` väärtust, mille tüüp Deserialized ``<ParameterType>`` tippida ``<ParameterType>``".

**Tõrke põhjus:** Kui teie käitusjuhendi on PowerShelli töövoo, see talletab keerukate objektide deserialized vormingus selleks, et püsida käitusjuhendi oleku, kui töövoog on peatatud.  

**Tõrkeotsingu näpunäited:**  
Mis tahes järgmised kolm lahendusi probleemi lahendada:

1. Kui teil on toru keerukate ühe cmdlet teise objektid, murdmiseks on InlineScript need cmdlet-käsud.  
2. Andke nimi või väärtust, mille peate keerukate objekti mitte saata kogu objekti.  

3. PowerShelli töövoo käitusjuhendi asemel kasutada PowerShelli käitusjuhendi.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Stsenaarium: Käitusjuhendi töö nurjus, kuna määratud kvoot ületab

**Tõrge:** Oma käitusjuhendi töö nurjub tõrkega "kuu kokku töö Käitusaeg kvoodi saavutamiseni selle tellimuse".

**Tõrke põhjus:** See tõrge ilmneb juhul, kui töö täitmise ületab 500-minutilise tasuta kvoodi teie konto jaoks. Selle piirmäära rakendatakse kõigi töö täitmise ülesanded, näiteks testimine töö töö alates portaali, käivitamisel tööd kasutades webhooks ja ajastamise töö, mida soovite käivitada Azure portaali kaudu või oma andmekeskuses. Lisateavet teemast kohta rohkem teavet automaatika [hinnad automatiseerimine](https://azure.microsoft.com/pricing/details/automation/).

**Tõrkeotsingu näpunäited:** Kui soovite kasutada rohkem kui 500 minutit töötlemise kuus peate tellimuse muutmine on tasuta kiht tavaline astme. Saate värskendada tavaline astme järgi järgmiselt:  

1. Azure'i tellimuse sisse logida  
2. Valige automatiseerimise konto, mida soovite uuendada  
3. Klõpsake **sätete** > **hinnakirjad taseme- ja kasutus** > **hinnakirjad taseme**  
4. **Valige oma hinnakirjad taseme** enne, valige **tavaline**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Stsenaarium: Cmdlet-käsu lisamine käitusjuhendi täitmisel ei tuvastata

**Tõrge:** Oma käitusjuhendi töö nurjub tõrkega "``<cmdlet name>``: termin ``<cmdlet name>`` seda cmdlet-käsk funktsioon skriptifail või programmi nime ei tuvastata."

**Tõrke põhjus:** Selle tõrke põhjustavad kui PowerShelli mootor ei leia te kasutate oma käitusjuhendi cmdlet.  Võimalik, et moodul, mis sisaldab cmdlet puudub konto on nimekonflikt käitusjuhendi nimega või cmdlet esineb ka teise mooduli ja automatiseerimise ei saa nime lahendada.

**Tõrkeotsingu näpunäited:** Ühte järgmistest lahendustest on probleemi lahendamiseks:  

- Kontrollige, kas sisestasite cmdlet-käsu nimi õigesti.  

- Veenduge, et cmdlet automatiseerimine konto on olemas ja konfliktid puuduvad. Kontrollimaks, kas cmdlet on olemas, avage soovitud käitusjuhendi redigeerimisrežiimis ja otsige soovitud teegis või Käivita cmdlet **Get-käsk ``<CommandName>`` **.  Kui teil on kinnitatud saadaolev cmdlet-i kontole ja mis nimi konfliktid teiste cmdlet-käskude või tegevusraamatud puuduvad, lisage see lõuend ja veenduge, et kasutate kehtiv parameetri seadmine oma käitusjuhendi.  

- Kui teil on nimekonflikt cmdlet on saadaval kaks erinevat moodulit, saate selle lahendada, kasutades cmdlet täielik nimi. Näiteks saate kasutada **ModuleName\CmdletName**.  

- Kui teil on käivitamisel käitusjuhendi asutusesisese hübriid töötaja rühma, siis veenduge, et mooduli/cmdlet-käsk on installitud hübriid töötaja hostiva masina.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Stsenaarium: Kaua töötab käitusjuhendi süsteemne jätab erandiga: "töö ei saa jätkata töötab, kuna see on korduvalt eemaldada sama kontrollpunkt:".

**Tõrke põhjus:** See on kujundus käitumine tõttu "Osa" jälgimine protsesside Azure'i automaatika, mis peatab on käitusjuhendi automaatselt, kui see käivitab enam kui 3 tundi. Siiski ei paku tagastatud tõrketeade "mida edasi teha" suvandid. Soovitud käitusjuhendi saate peatada mitmel põhjusel. Peatab juhtub enamasti tõrgete tõttu. Näiteks tabamatu erandi lisamine käitusjuhendi, võrgu tõrke või krahhi Käitusjuhendi töötaja töötab käitusjuhendi kõigi põhjustab käitusjuhendi peatada ja alustada oma viimase kontrollpunkt kui jätkata.

**Tõrkeotsingu näpunäited:** Selle probleemi vältimiseks dokumenteeritud lahendus on kasutada postkastid töövoo.  Lisateavet rohkem viidata [Õ PowerShelli töövood](automation-powershell-workflow.md#Checkpoints).  Selle artikli teemad ajaveebi [Abil postkastid tegevusraamatud rakenduses](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/)leiate põhjalikumat selgitus "Osa" ja kontrollpunkt.


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Levinud probleemide tõrkeotsingu moodulid importimisel

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Stsenaarium: Mooduli nurjub importimine või cmdlet-käske ei saa käivitada pärast importimist

**Tõrge:** Mooduli nurjub importimine või impordib edukalt, kuid pole cmdlettide ekstraktitakse.

**Tõrke põhjus:** On mõned levinud põhjused, miks mooduli võib pole edukalt importida Azure automatiseerimine.  

- Struktuuri ei vasta struktuur, mis automatiseerimise peab see olema.  

- Mooduli sõltub teise moodul, mis on juurutatud kontole automatiseerimine.  

- Mooduli puudub sõltuvustega kausta.  

- Cmdlet-käsu **New-AzureRmAutomationModule** kasutatakse mooduli üles laadida ning teil on antud salvestusruumi täielik tee või ei laadita mooduli avalikult juurdepääsetava URL-i abil.  

**Tõrkeotsingu näpunäited:**  
Ühte järgmistest lahendustest on probleemi lahendamiseks:  

- Veenduge, et mooduli järgib järgmises vormingus:  
ModuleName.Zip **->** ModuleName või versiooninumber **->** (ModuleName.psm1, ModuleName.psd1)

- Avage fail .psd1 ja kas mooduli on sõltuvusi.  Kui seda ei, laadige need moodulid automatiseerimise konto.  

- Veenduge, et kõik viidatud .dlls esinevad mooduli kausta.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Levinud probleemide tõrkeotsingu abil soovitud oleku konfiguratsioon (DSC) töötamisel  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Stsenaarium: Sõlm on nurjunud oleku "Ei leitud" tõrge

**Tõrge:** Sõlme on **nurjunud** oleku ja kus asub tõrkega aruande "proovi toimingu toomine serveri https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction nurjus, kuna kehtiv konfiguratsiooni ``<guid>`` ei leita."

**Tõrke põhjus:** See tõrge ilmneb tavaliselt sõlme on määratud konfiguratsiooni nimi (nt ABC) asemel sõlm konfiguratsiooni nimi (nt ABC. Veebiserverile).  

**Tõrkeotsingu näpunäited:**  

- Veenduge, et määrate sõlm "sõlm konfiguratsiooni nimi" ja ei "konfiguratsiooni nimi".  

- Saate määrata sõlm konfiguratsioon Azure'i portaalis sõlme või PowerShelli cmdlet-käsu.
    - Azure'i portaalis sõlm sõlm konfiguratsiooni määramiseks, et avada **DSC sõlmed** tera, valige sõlm ja **määrata sõlm konfiguratsioon** nuppu.  
    - Selleks, et PowerShelli cmdlet-käsu abil sõlm sõlm konfiguratsiooni määramiseks kasutada cmdlet-käsu **Set-AzureRmAutomationDscNode**


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Stsenaarium: Toodeti pole sõlm konfiguratsioone (RM-failid), kui koostatakse konfiguratsioon

**Tõrge:** DSC koostamine tööpäevad peatab tõrkega: "koostamine lõpule viidud, kuid pole sõlm konfiguratsiooni .mofs loodi".

**Tõrke põhjus:** Kui **sõlm** märksõnale DSC konfiguratsiooni avaldise väärtuseks $null siis pole sõlm konfiguratsioone valmistatakse.    

**Tõrkeotsingu näpunäited:**  
Ühte järgmistest lahendustest on probleemi lahendamiseks:  

- Veenduge, et **sõlm** märksõna konfiguratsiooni määratlus kõrval avaldist hinnatakse ei $null.  
- Kui ConfigurationData on möödudes, koostamisel konfiguratsiooni, veenduge, et, et teil on läbides oodatud väärtusega, mis nõuab konfiguratsiooni [ConfigurationData](automation-dsc-compile.md#configurationdata)kaudu.


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Stsenaarium: DSC sõlm aruanne muutub ummikus "pooleli" olek

**Tõrge:** DSC Agent väljundid "Ei leitud antud kinnisvara eksemplari."

**Tõrke põhjus:** Teie WMF versioon on täiendatud ja on rikutud WMI.  

**Tõrkeotsingu näpunäited:** Järgige ajaveebipostitus [DSC teadaolevad probleemid ja piirangud](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) probleemi lahendamiseks.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Stsenaarium: Ei saa kasutada mandaati DSC konfigureerimine

**Tõrge:** Tööpäevad DSC Kompileerimine on peatatud tõrkega: "System.InvalidOperationException tõrge, atribuudi 'Mandaati' tüüpi töötlemine ``<some resource name>``: teisendamine ja talletamine on krüptitud parool, kui Lihttekst on lubatud ainult kui PSDscAllowPlainTextPassword on seatud väärtusele tõene".

**Tõrke põhjus:** Konfiguratsiooni mandaati kasutanud, kuid ei paku proper **ConfigurationData** **PSDscAllowPlainTextPassword** tõene iga sõlm konfiguratsiooni määramiseks.  

**Tõrkeotsingu näpunäited:**  
- Veenduge, et läbida proper **ConfigurationData** **PSDscAllowPlainTextPassword** väärtuseks true iga sõlm konfiguratsiooni mainitud konfiguratsiooni. Lisateabe saamiseks vaadake [varasid Azure automatiseerimine DSC](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Järgmised sammud

Kui eeltoodud tõrkeotsingu juhiste ja vajate lisaabi, mis tahes hetkel selles artiklis, saate teha järgmist.

- Pöörduge abi saamiseks Azure'i eksperdid. Esitage oma küsimus on [MSDN-i Azure või Virnlintdiagrammil ületäitumise foorumeid.](https://azure.microsoft.com/support/forums/)

- Failide Azure'i tugiteenuse taotluse. [Azure tugiteenuste sait](https://azure.microsoft.com/support/options/) ja klõpsake nuppu **tootetugi** jaotises **tehniline ja arvelduse tugiteenuste**.

- Postitada kui otsite lahenduse Azure automatiseerimine käitusjuhendi või mõne integreerimine mooduli skripti taotlemine [Skripti keskele](https://azure.microsoft.com/documentation/scripts/) .

- Azure'i automaatika [Kasutaja](https://feedback.azure.com/forums/34192--general-feedback)hääl postitada tagasiside või funktsiooni taotlused.
