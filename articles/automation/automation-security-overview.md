<properties
   pageTitle="Azure'i automaatika turvalisus | Microsoft Azure'i"
   description="Selles artiklis antakse ülevaade automatiseerimise turbe- ja saadaval eri autentimismeetodite automatiseerimise kontode Azure automatiseerimine."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automaatika turvalisus, turvalist automatiseerimine" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure'i automaatika turvalisus
Azure'i automaatika võimaldab teil esitada ressursse Azure asutusesiseselt, ja teiste pakkujate kontodelt cloud näiteks Amazon Web Services (AWS) automatiseerimine.  Selleks, et käitusjuhendi, selle nõutud toimingute teostamiseks, peab olema õigused, saate turvaliseks juurdepääsuks ressursside nõutavad tellimuse minimaalne õigustega.  
Selles artiklis hõlmab erinevate autentimise stsenaariumid ei toeta Azure automatiseerimine ja näitab teile, kuidas alustada keskkonna või haldamiseks peate keskkonnas.  

## <a name="automation-account-overview"></a>Automaatika konto ülevaade
Kui käivitate Azure automatiseerimine esimest korda, peate looma vähemalt üks automatiseerimise konto. Automatiseerimise raamatupidamine võimaldab teil oma automatiseerimise ressursse (tegevusraamatud, varad, konfiguratsioone) eristamiseks ressursside sisalduvate muudelt kontodelt automatiseerimine. Saate automatiseerimise kontod eraldi ressursid eraldi loogilise keskkonnas. Näiteks võite kasutada ühe konto arengu, teise tootmiseks ja teise kohapealse keskkonna jaoks.  Azure'i automaatika konto erineb teie Microsofti konto või kontod Azure tellimuse jaoks luua.

Automaatika ressursside iga automatiseerimise konto on seotud üks Azure piirkond, kuid automatiseerimise kontod saate hallata ressursse, mis tahes ala. Peamine põhjus eri piirkondade automatiseerimise kontode loomine oleks, kui teil poliitikad, mis nõuavad andmete ja ressursse, mis olema eraldatud teatud alale.

>[AZURE.NOTE]Automatiseerimise kontode ja ressursse, mis neis on loodud Azure'i portaalis, ei pääse Azure klassikaline portaalis. Kui soovite hallata nende kontodega või nende ressursside Windows PowerShelli abil, peate kasutama Azure'i ressursihaldur moodulid.

Kõik tööülesanded, mida esitada ressursse Azure'i ressursihaldur ja Azure cmdlet-käskude kasutamine Azure automatiseerimine tuleb autentida Azure'i Azure Active Directory organisatsiooni identiteedi mandaati autentimise abil.  Serdi autentimise oli algse autentimise meetodit Azure'i Teenusehaldus režiimiga, kuid see on keeruline häälestamine.  Azure'i Azure AD kasutaja autentimine on kasutusele tagasi 2014 mitte ainult lihtsustada konto autentimise konfigureerimine, kuid toetavad ka mitte interaktiivseks autentimiseks Azure töötanud nii Azure'i ressursihaldur ja klassikalises ressursside ühe kasutajakonto.   

Praegu siis, kui loote uue konto automatiseerimise Azure'i portaalis, loob automaatselt:

-  Käivitage nimega konto, mis loob uue teenuse põhilise Azure Active Directory sert, ja määrab kaasautor Rollipõhine juurdepääsu reguleerimine (RBAC) ressursihaldur ressursid abil tegevusraamatud haldamiseks kasutatavad.
-  Klassikaline käivitada nagu konto üleslaadimisel halduse tunnistus, mis kasutab Azure Teenusehaldus või klassikalises ressursside tegevusraamatud abil hallata.  

Rollipõhine juurdepääsu reguleerimine on saadaval Azure ressursihaldur andmiseks lubatud tegevused Azure AD kasutajakonto ja käivitage nimega konto ja selle teenuse põhilise autentida.  Lugege [Rollipõhine juurdepääsu reguleerimine Azure automatiseerimine artiklis](../automation/automation-role-based-access-control.md) Lisateavet aidata arendada oma mudel automatiseerimise õiguste haldamine.  

Hübriid Käitusjuhendi töötaja oma andmekeskuses või arvutuste AWS teenused töötavad tegevusraamatud ei saa kasutada sama meetodit, mida kasutatakse tavaliselt tegevusraamatud autentimine Azure ressursse.  Seda sellepärast, et nende ressursside töötavad väljaspool Azure ja seetõttu on vaja oma turvalisus identimisteabe määratletud autentimiseks ressursid, millest juurdepääs kohalikult automatiseerimine.  

## <a name="authentication-methods"></a>Kasutajaautentimise meetodite

Järgmises tabelis on toodud erinevad autentimismeetodite iga keskkond, mis ei toeta Azure automatiseerimine ja see artikkel kirjeldab, kuidas häälestada oma tegevusraamatud autentimise.

Meetod  |  Keskkonnas  | Artikkel
----------|----------|----------
Azure'i AD kasutajakonto. | Azure'i ressursihaldur ja Azure teenuste haldus | [Autentimiseks tegevusraamatud Azure AD kasutajakonto](../automation/automation-sec-configure-aduser-account.md)
Azure'i Käivita konto | Azure'i ressursihaldur | [Autentida tegevusraamatud Azure'i käivitada nimega kontoga](../automation/automation-sec-configure-azure-runas-account.md)
Azure'i klassikaline Käivita konto | Azure'i Teenusehaldus | [Autentida tegevusraamatud Azure'i käivitada nimega kontoga](../automation/automation-sec-configure-azure-runas-account.md)
Windowsi autentimine | Kohapealse andmekeskusega | [Autentida tegevusraamatud hübriid Käitusjuhendi töötajatele](../automation/automation-hybrid-runbook-worker.md)
AWS identimisteave | Amazon veebiteenused | [Autentida tegevusraamatud Amazon veebiteenuste (AWS)](../automation/automation-sec-configure-aws-account.md)



