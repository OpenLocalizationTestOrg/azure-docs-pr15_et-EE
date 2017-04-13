<properties
    pageTitle="Log Analytics Configuration Manager ühenduse | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse juhiseid Log Analytics Configuration Manager ühendamiseks ja käivitage andmete analüüsimine."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Log Analytics Configuration Manager ühendamine

Saate luua ühenduse System Center Configuration Manager Logi Analytics rakenduses OMS sünkroonimine seadme saidikogumi andmed. See muudab andmete juurutamise Configuration Manager OMS saadaval.

On arvu samme ühendada Configuration Manager OMS, et siin on kogu protsessi kiirülevaade.

1. Azure'i haldusportaal registreerida Configuration Manager veebirakenduse ja/või Web API app ja veenduge, et on kliendi ID ja kliendi salajane võti: registreerimise Azure Active Directory kaudu. Leiate üksikasjalikku teavet selle kohta, kuidas [kasutada portaali loomine Active Directory rakenduste ja teenuste kohta põhisumma, millele pääsete juurde ressursid](../resource-group-create-service-principal-portal.md) selle toimingu.
2. Azure'i haldusportaal, [anda juurdepääs OMS Configuration Manager (registreeritud web app)](#provide-configuration-manager-with-permissions-to-oms).
3. Configuration Manager, [Lisage OMS ühenduse viisardi abil ühenduse lisamine](#add-an-oms-connection-to-configuration-manager).
4. Configuration Manager saate [värskendada ühenduse atribuudid,](#update-oms-connection-properties) kui parooli või kliendi salajane võti kunagi aegub või läheb kaotsi.
5. [Laadige alla ja installige Microsoft Agenti jälgimine](#download-and-install-the-agent) Configuration Manager teenuse ühenduse arvutil OMS portaalist leiate punkti saidi süsteemi rolli. Agent saadab OMS Configuration Manager andmed.
6. Klõpsake OMS [importimine saidikogumid Configuration Manager](#import-collections) arvuti rühmad.
7. OMS, kuvada andmed Configuration Manager nimega [arvuti rühmad](log-analytics-computer-groups.md).

Lugege lisateavet OMS [sünkroonimine](https://technet.microsoft.com/library/mt757374.aspx)andmed: Configuration Manager Microsoft Operations Management Suite Configuration Manager ühenduse loomise kohta.



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Configuration Manager õigustega pakkumiseks OMS

Järgmistes jaotistes on Azure'i haldusportaal õigustega OMS juurdepääsu. Täpsemalt, tuleb tagada kasutajate ressursirühma *kaasautori roll* . Omakorda, mis võimaldab Azure'i haldusportaal OMS Configuration Manager ühenduse.

>[AZURE.NOTE] Määrake õigused OMS Configuration Manager. Muul juhul saate tõrketeate konfiguratsiooniviisardi Configuration Manager kasutamisel.


1. Avage [Azure portaali](https://portal.azure.com/) ja klõpsake nuppu **Sirvi** > **Log Analytics (OMS)** avamiseks tera Log Analytics (OMS).  
2. Enne **Log Analytics (OMS)** , avamiseks klõpsake nuppu **Lisa** **OMS tööruumi** tera.  
  ![OMS blade](./media/log-analytics-sccm/sccm-azure01.png)
3. Enne **OMS tööruumi** , sisestage järgmine teave ja klõpsake nuppu **OK**.
  - **OMS tööruumi**
  - **Tellimuse**
  - **Ressursirühm**
  - **Asukoht**
  - **Esimese taseme hinnad**  
    ![OMS blade](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Ülaltoodud näites loob uue ressursirühma. Pakkuda Configuration Manager õiguste OMS tööruumi selles näites kasutatakse ainult ressursirühma.

4. Klõpsake nuppu **Sirvi** > **Ressursi rühmad** avamiseks **Ressursirühma** tera.
5. **Ressursi rühmade** tera, klõpsake ressursirühma loodud kohal avamiseks soovitud &lt;ressursirühma nimi&gt; sätted tera.  
  ![ressursi rühma sätted blade](./media/log-analytics-sccm/sccm-azure03.png)
6. Rakenduses on &lt;ressursirühma nimi&gt; sätted tera, juurdepääsu reguleerimine (IAM) avamiseks klõpsake selle &lt;ressursirühma nimi&gt; kasutajate tera.  
  ![ressursi rühma kasutajad blade](./media/log-analytics-sccm/sccm-azure04.png)  
7. Rakenduses on &lt;ressursirühma nimi&gt; kasutajate labale avamiseks klõpsake nuppu **Lisa** **lisamine Accessi** tera.
8. **Valige roll,**klõpsake **lisamine Accessi** tera, ja valige **kaasautori** roll.  
  ![Valige roll](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klõpsake **kasutajate lisamine**, valige kasutaja Configuration Manager, klõpsake nuppu **Vali**ja seejärel klõpsake nuppu **OK**.  
  ![kasutajate lisamine](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Lisada mõne OMS ühenduse Configuration Manager

Lisanud OMS-ühenduse, Configuration Manager keskkonna peab olema konfigureeritud ühendusega režiimi [teenuse ühendust](https://technet.microsoft.com/library/mt627781.aspx) .

1. Valige **haldus** tööruumi Configuration Manager **OMS konnektor**. Avaneb **Lisada OMS andmeühenduse viisardisse**. Valige **edasi**.

2. **Üldine** ekraanil, veenduge, et olete teinud järgmised toimingud ja, et iga üksuse üksikasjad, ja seejärel valige **edasi**.
  1. Azure'i haldusportaal, olete registreerunud Configuration Manager veebirakenduse ja/või Web API app ja teil on [Kliendi ID registreerimine kaudu](../active-directory/active-directory-integrating-applications.md).
  2. Azure'i haldusportaal loodud rakenduse salajane võti registreeritud rakenduse Azure Active Directory.  
  3. Azure'i haldusportaal andsite registreeritud web appi OMS juurdepääsuks õigus.  
  ![Leht üldine OMS viisardi ühendamiseks](./media/log-analytics-sccm/sccm-console-general01.png)

3. Kuval **Azure Active Directory** konfigureerimine oma **rentniku** , **Kliendi ID** ja **Kliendi salajane võti** OMS ühenduse sätted ja seejärel valige **edasi**.  
  ![OMS viisardi Azure Active Directory lehe ühendamiseks](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Kui jõudsite kõik juhised edukalt, siis teave **OMS ühenduse konfiguratsiooni** ekraanil kuvatakse automaatselt selle lehe. Teave ühenduse sätted peaks kuvatama **Azure'i tellimus** , **Azure ressursirühm** ja **Toimingute haldus komplekti tööruumi**.  
  ![Lehel OMS viisardi OMS ühenduse ühendamiseks](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. Viisard loob ühenduse OMS teenuse kasutamise andmed, mida olete sisend. Valige saidikogumite seade, mida soovite sünkroonida OMS ja seejärel klõpsake nuppu **Lisa**.  
  ![Valige saidikogumid](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Kontrollige ühenduse sätteid ekraanil **Kokkuvõte** ja seejärel valige **edasi**. Kuva **edenemine** näitab ühenduse oleku, siis tuleks **lõpuleviimine**.

>[AZURE.NOTE] Tuleb ühendada OMS ülemise taseme saiti oma hierarhias. Kui OMS ühenduse autonoomse esmane saidi ja seejärel lisage administreerimiskeskuse saidi teie keskkonnas, siis on teil kustutamine ja uuesti jooksul uue hierarhiaga OMS ühenduse luua.

Pärast Configuration Manager on seotud OMS, saate lisada või eemaldada saidikogumite ja vaadata OMS ühenduse atribuudid.

## <a name="update-oms-connection-properties"></a>Värskendage OMS ühenduse atribuudid

Kui parooli või kliendi salajane võti kunagi aegub või on kadunud, peate käsitsi värskendada OMS ühenduse atribuudid.

1. Configuration Manager, liikuge **Pilveteenustega** ja seejärel valige **OMS konnektor** **OMS ühenduse atribuudid** lehe avamiseks.
2. Selle lehe vahekaarti **Azure Active Directory** vaadata oma **rentniku**, **Kliendiid** **Kliendi salajane tootenumbri aegumise**. **Kinnita** oma **Kliendi salajane võti** kui see on aegunud.


## <a name="download-and-install-the-agent"></a>Laadige alla ja installige agent

1. OMS portaalis [kaudu OMS agent setup faili alla laadida](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Kasutage üht järgmistest meetoditest installida ja konfigureerida agent arvutis installitud Configuration Manager teenuse ühenduse punkti saidi süsteemi rolli.
  - [Installige agent häälestuse abil](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Installige käsureal agent](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Installige Azure'i automaatika DSC kasutamine agent](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Saidikogumite importimine

Kui olete lisanud mõne OMS ühenduse Configuration Manager ja installitud agent arvutis installitud Configuration Manager teenuse ühendus, valige käsk saidi süsteemi rolli, järgmise sammuna tuleb importida saidikogumid kaudu Configuration Manager OMS nimega arvuti rühmad.

Pärast importimist on lubatud, tuuakse saidikogumi liikmelisusteabe iga 3 tunni saidikogumi liikmestaatuste ajakohastage. Saate importida igal ajal keelata.

1. OMS portaalis, klõpsake nuppu **sätted**.
2. Klõpsake vahekaarti **Arvuti rühmad** ja seejärel klõpsake vahekaarti **SCCM** .
3. Valige **impordi Configuration Manager saidikogumi liikmelisused** ja klõpsake siis nuppu **Salvesta**.  
  ![Arvuti rühmad - SCCM menüü](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Andmete kuvamine Configuration Manager

Kui olete lisanud mõne OMS ühenduse Configuration Manager ja agent arvutisse installitud ei tööta teenus Configuration Manager ühenduse punkti saidi süsteemi rolli, andmete agendi OMS saadetakse. OMS, kuvatakse teie Configuration Manager saidikogumid nimega [arvuti rühmad](log-analytics-computer-groups.md). Saate vaadata jaotises **Arvuti rühmad** lehelt **Configuration Manager** rühmade **sätted**.

Pärast importimist kogumite, saate vaadata mitmes arvutis ja saidikogumi liikmelisused on avastatud. Näete imporditud saidikogumite arvu.

![Arvuti rühmad - SCCM menüü](./media/log-analytics-sccm/sccm-computer-groups02.png)

Kas üks klõpsamisel avaneb otsing, kas kõik imporditud rühmade või iga rühma kuuluvad kõik arvutid kuvamiseks. [Log otsingu](log-analytics-log-searches.md)abil saate alustada põhjalikumat Configuration Manager andmete analüüsiks.

## <a name="next-steps"></a>Järgmised sammud

- [Log otsingu](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku teavet andmete Configuration Manager.
