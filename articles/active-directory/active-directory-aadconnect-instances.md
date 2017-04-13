<properties
    pageTitle="Azure'i AD-ühendus: Sünkroonida teenuse eksemplari | Microsoft Azure'i"
    description="Selle lehe dokumentidega Azure AD eksemplarid silmas pidada."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure'i AD-ühendus: Silmas pidada eksemplari
Azure'i AD-ühendus on kõige sagedamini kasutatav koos Azure AD kogu maailmas eksemplari ja Office 365. Kuid on ka muid juhtumeid ja need on URL-ide ja muude silmas pidada jaoks erinevad nõuded.

## <a name="microsoft-cloud-germany"></a>Microsofti pilveteenuse Saksamaa
[Microsofti pilveteenuse Saksamaa](http://www.microsoft.de/cloud-deutschland) on sõltumatud pilve käitatava Saksa andmete haldur.

See pilv on praegu eelvaade. Stsenaariumid, mis on tavaliselt saate teha ise, näiteks paljude kinnitamine domeenide, peab tegema tehtemärk. Osalemine eelvaate kohta lisateabe saamiseks pöörduge oma kohaliku Microsofti esindaja poole.

URL-ide avamine puhverserver |
--- |
\*. microsoftonline.de |
\*. windows.net |
Sertide Tühistusloendid |

Kui logite Azure AD kataloogi kasutage konto onmicrosoft.de Domeen.

Funktsioonid, mis on praegu ei esine Microsoft Cloud Saksamaa.

- Azure'i AD ühenduse seisund pole saadaval.
- Automaatvärskenduste pole saadaval.
- Parooli tagasikirjutusega pole saadaval.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Governmenti pilveteenuses
[Microsoft Azure'i Government cloud](https://azure.microsoft.com/features/gov/) on USA valitsuse pilve.

See pilv on DirSync varasemates versioonides toetatud. Azure'i AD-ühenduse ehitada 1.1.180 järgmise genereerimine pilveteenuste on toetatud. See genereerimine on kasutusel ainult U.S. vastavalt lõpp-punktid ja teil URL-ide avamine puhverserverisse erinevate loendit.

URL-ide avamine puhverserver |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
Sertide Tühistusloendid |

Azure'i AD-ühendus ei saa automaatselt tuvastada, et kataloogi Azure AD asub Government pilveteenuses. Selle asemel peate tegema järgmised toimingud Azure'i AD-ühenduse installimisel.

1. Azure'i AD-ühenduse installi alustada.
2. Kohe, kui näete esimesel lehel, kus te peaksid EULA aktsepteerimiseks, jätkata, kuid jätta installimise viisard töötab.
3. Käivitage käsk regedit ja registrivõtme muutmine `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` väärtusele `2`.
4. Minge tagasi, et Azure'i AD-ühenduse installimise viisard aktsepteerima EULA ja jätkata. Installimise ajal veenduge, et kasutada **kohandatud Otsingukonfiguratsiooni** Installitee (ja mitte Expressi installimise). Seejärel jätkake installimise nagu tavaliselt.

Funktsioonid, mis on praegu ei esine Microsoft Azure Governmenti pilveteenuses.

- Azure'i AD ühenduse seisund pole saadaval.
- Automaatvärskenduste pole saadaval.
- Parooli tagasikirjutusega pole saadaval.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
