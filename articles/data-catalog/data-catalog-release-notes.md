<properties
   pageTitle="Azure'i andmekataloogi väljalaskemärkmed | Microsoft Azure'i"
   description="Azure'i andmekataloogi väljalaskemärkmed."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure'i andmekataloogi väljalaskemärkmed

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Märkmete November 20, 2015 vabastage Azure'i andmekataloogi.

### <a name="opening-data-sources-in-power-bi-desktop"></a>Power BI Desktopi avamine andmeallikad

Suvandi "Ava rakenduses Power BI Desktopi" portaalist **Azure'i andmekataloogi** kasutamisel kasutajad võivad ilmneda üks kahest probleeme rakenduse Power BI Desktopi:

- Kuvatakse dialoogiboks, tiitliga "Ei saa abil avatud dokumendis"
- Rakendus Power BI Desktopi avaneb, kuid fail kuvatakse tühi

Iga olukorra jaoks saate alla laadida ja installida uusima versiooni Power BI Desktopi [powerbi.com lehe kaudu](https://powerbi.com)probleemi lahendada.

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Märkmete November 13, 2015 vabastage Azure'i andmekataloogi.

### <a name="registering-and-connecting-to-teradata"></a>Registreerimine ja ühenduse loomine Teradata

Kui loote ühenduse kasutajate olete installinud õige Teradata ODBC-draiver Teradata andmeallikatega, vastavad kasutatava tarkvara bitness (32-bitine või 64-bitine).

Selle ADC Väljalaske kuupäeva seisuga viimase [Teradata ODBC-draiver Windowsi (versioon 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) ühildub Office 2013, kuid mitte Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Märkmete juuli 13, 2015 vabastage Azure'i andmekataloogi.

### <a name="registering-and-connecting-to-oracle-database"></a>Registreerimine ja ühenduse loomine Oracle'i andmebaasiga

Kui kasutajad olete installinud õige Oracle'i draiverid, mis vastavad bitness (32-bitine või 64-bitine) kasutatava tarkvara Oracle'i andmebaasiga andmeallikatega ühenduse loomine.

-   Oracle'i andmeallikate 32-bitine Windows arvutis registreerimisel kasutatakse 32-bitine Oracle'i draiverid
-   Oracle'i andmeallikate arvutisse, kus töötab 64-bitises Windowsis registreerimisel kasutatakse 64-bitine Oracle'i draiverid
-   Ühendamisel Oracle'i andmeallikate Exceli arvutisse, kus töötab Microsoft Office'i 32-bitise versiooni 64-bitises Windowsis, sh 32-bitine Oracle'i draiverid kasutatakse
-   Ühendamisel Oracle'i andmeallikate Exceli arvutisse, kus töötab 64-bitine versioon Microsoft Office'i 64-bitine Oracle'i draiverid kasutatakse

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registreerimine ja ühenduse loomine SQL serveri aruandlusteenuste

SQL serveri aruandlusteenused (SSRS) andmeallikate tugi on praegu piiratud Omarežiimis serveriga. Uuem väljaanne lisatakse tugi SSRS SharePointi režiimis.

### <a name="opening-data-assets-in-excel"></a>Avamine andmete varad Excelis

Microsoft Exceli andmete varad avamine **Azure'i andmekataloogi** portaali kasutajad võidakse teil paluda **Microsoft Excel turbeteatis** dialoogiboks. See on standardne, oodatud käitumine ja kasutajate saate valida **lubada** jätkata.

Lisateavet leiate teemast [lubamine või keelamine Turbeteatiste linke ja kahtlaste veebisaitide](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Puhverserveri ja poliitika konfigureerimine ja andmete allikas registreerimine

Kasutajate võib tekkida olukord, kus ta saab sisse logida Azure'i andmekataloogi portaali, kuid proovimisel andmete allikas registreerimise tööriist neil tekkida võivad tõrketeade, mis takistab nende sisse logite sisse logida.

On kaks võimalikud põhjused seda probleemi käitumist.

**Põhjuse 1: Active Directory Federation Services konfigureerimine** Andmete allikas registreerimise tööriist kasutab valideerimiseks kasutaja ka vastu Active Directory vormide autentimist. Eduka sisselogimise kohta, vormide autentimist peab olema lubatud globaalne autentimise poliitika Active Directory administraator.

Mõnel juhul see tõrge võib juhtuda ainult siis, kui kasutaja on ettevõtte võrgus või ainult siis, kui kasutaja on väljaspool ettevõtte võrku ühendamine. Globaalne autentimise poliitika võimaldab autentimismeetodite tuleb lubada eraldi sise-ja suhtevõrgus ühendused. Sisselogimise tõrgete võib ilmneda juhul, kui vormide autentimine on lubatud, kust kasutaja on ühendatud võrku.

Lisateabe saamiseks vt [Autentimise poliitikate konfigureerimine](https://technet.microsoft.com/library/dn486781.aspx).

**Põhjus 2: võrgu puhverserveri konfigureerimise** Kui ettevõtte võrgus kasutab puhverserverit, ei saa tööriista registreerimist ühenduse Azure Active Directory puhvri kaudu. Kasutajate saate tagada, et registreerimise tööriist tööriista konfiguratsioonifail, redigeerides faili selle jaotise lisamine:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Otsige üles fail RegistrationTool.exe.config, käivitage tööriist registreerimise ja seejärel avage Windowsi Tegumihalduris. Tegumihalduri vahekaardil üksikasjad Paremklõpsake RegistrationTool.exe ja valige hüpikmenüüst käsk Ava faili asukoht.
