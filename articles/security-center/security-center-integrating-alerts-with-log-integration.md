<properties
   pageTitle="Azure'i turbekeskus teatiste integreerimine integreerimine Azure log (eelvaade) | Microsoft Azure'i"
   description="See artikkel aitab teil alustamine turbekeskus teatiste integreerimine Azure log integreerimine."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Turbekeskus teatiste integreerimine integreerimine Azure log (eelvaade)

Palju turvalisus toimingute ja langeva rühmi toetuvad Turbeteave ja sündmuse Management (SIEM) lahenduse vigade sõelumine ja uurimiseks Turbeteatiste lähtepunktina. Azure'i log integreerimise, kliendid saate sünkroonida Azure turbekeskus teatised, virtuaalse masina turvalisus üritused kogutud Azure'i diagnostika- ja Azure auditilogid, nende log analytics või SIEM lahenduse peaaegu reaalajas koos.

Azure'i log integreerimine töötab HP ArcSight, Splunk, IBM QRadari ja teised.

## <a name="what-logs-can-i-integrate"></a>Millised logid saate integreerida?

Azure'i toodab olulisel logimine iga teenuse jaoks. Need logid on liigitada järgmiselt:

- **Järelevalve logid** , mis aitavad muuta paremini nähtavaks tegevusega Azure'i ressursihaldur loomine, värskendamine ja kustutamine.
- **Andmete lennuk logid** annavad nähtavus sündmuste astmes kasutamisel on Azure ressurss. Näide on Windowsi sündmuselogi – turvalisus ja rakenduse virtuaalse masina sisse logib.

Azure'i log integreerimine toetab praegu integreerimine.

- Azure'i VM logid
- Azure'i auditilogid
- Azure'i turbekeskus teatised

## <a name="install-azure-log-integration"></a>Installige Azure'i log integreerimine

Laadige [Azure'i logige integreerimine](https://www.microsoft.com/download/details.aspx?id=53324).

Integreerimine Azure Logi teenuse kogub telemeetria andmeid arvutist, kuhu on installitud.  Kogutud telemeetria andmed on:

- Erandid, mis ilmnevad integreerimine Azure log täitmine
- Mõõdikute kohta päringute ja töödeldud sündmuste arv
- Statistika kohta, milliseid Azlog.exe kasutatakse Käsurea võtmed

> [AZURE.NOTE] Välja lülitada telemeetria andmete kogumine eemaldades see suvand.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Sujuv Azure'i auditilogid ja turbekeskuse teatised

1. Avage käsuviip ja **CD-le** üheks **c:\Program Files\Microsoft Azure'i Log integreerimine**.

2. Käivitage käsk **azlog createazureid** loomine [Azure Active Directory teenuse põhilise](../active-directory/active-directory-application-objects.md) Azure Active Directory (AD) rentnikud majutavad Azure'i tellimused.

    Teil palutakse Azure sisselogimiseks.

    > [AZURE.NOTE] Peate olema tellimuse omanik või koostöö administraator tellimuse.

    Azure'i autentimise tehakse Azure AD.  Teenuse põhilise Azure log integreerimiseks loomise loob Azure AD identiteedi, mis antakse juurdepääs lugemise Azure'i tellimused.

3. Käivitage selle **azlog lubada <SubscriptionID> ** käsk lugeja Accessi tellimust määramiseks teenuse põhisumma loodud etappi 2. Kui te ei määra on **SubscriptionID**, siis teenuse põhilise määratakse lugeja roll kõigi tellimuste, millele teil on juurdepääs.

    > [AZURE.NOTE] Hoiatused võidakse kuvada, kui käivitate käsu **lubada** kohe pärast **createazureid** käsu sellepärast, et mõned latentsus Azure AD konto loomise ja kui konto on kasutamiseks saadaval. Kui te oodake umbes 10 sekundit pärast **createazureid** käsu käivitada käsu **autoriseerida** , siis pole näed need hoiatused.

4. Märkige ruut kinnitamiseks auditilogi logifailid JSON on järgmised kaustad:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Kontrollige kinnitada, et need olemas turbekeskus teatiste järgmistest kaustadest.

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Osutage standard SIEM faili ekspediitor konnektor kausta toru SIEM eksemplari andmed. Vaadake [SIEM konfiguratsioone](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) teie SIEM konfiguratsiooni.

Kui teil on küsimusi integreerimine Azure Log, saatke meile e-posti [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Järgmised sammud

Azure'i auditilogid ja atribuudi määratluste kohta leiate lisateavet järgmistest teemadest.

- [Auditi toimingud koos ressursihaldur](../resource-group-audit.md)
- [Tellimuse haldus sündmuste loendi](https://msdn.microsoft.com/library/azure/dn931934.aspx) - toomiseks auditilogi sündmused.

Lisateavet turbekeskus, leiate järgmistest:

- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Azure turvalisus uudiseid ja teavet saada.
