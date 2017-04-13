<properties
   pageTitle="Kuidas seadistada Turbeteatiste | Microsoft Azure'i"
   description="Saate teada, kuidas konfigureerida Azure õigustega identiteetide haldamisviis laiend Turbeteatiste."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Kuidas seadistada Turbeteatiste Azure AD õigustega identiteetide haldus

## <a name="security-alerts"></a>Turbeteatiste
Azure'i õigustega identiteedi haldamine (PIM) loob teatisi, kui kahtlase või ebaturvalise tegevuse on teie keskkonnas. Kui reegli, kuvatakse see PIM armatuurlaual. Valige aruanne, kus on loetletud selle kasutaja või kasutajate rollid, teatise käivitanud kuvamiseks teatise.

![Turbeteatiste PIM armatuurlaua - kuvatõmmis][1]



| Teavita | Päästik | Soovitused |
| ----- | ------- | -------------- |
| **Rollid määratakse väljaspool PIM** | Administraator on jäädavalt määratud roll, PIM kasutajaliidese väljaspool. | Vaadake üle uue rolli määramine. Kuna muude teenustega saate määrata ainult püsivate administraatorid, õigus ülesandele vajadusel muuta. |
| **Sageli on aktiveeritud rollid** | Sätteid aja jooksul oli liiga palju reactivations sama roll. | Võtke ühendust kasutaja ei näe, miks nad on aktiveeritud roll nii mitu korda. Võib-olla tähtaeg on liiga lühend neid nende toimingute tegemiseks või võib-olla need kasutate skriptide automaatselt aktiveerida rollid. |
| **Rollide ei nõua mitmikautentimise aktiveerimine** | Administraatorirolle on ilma MFA lubatud sätteid. | Nõua MFA jaoks kõige suuremate õigustega rollid, kuid me soovitame MFA lubamine kõikide rollide aktiveerimine. |
| **Administraatorid ei kasuta nende õigustega rollid** | On õigus administraatorid, mis ei ole aktiveeritud viimati nende rollid. | Alustage kasutajate, mida enam ei vaja juurdepääsu määratlemiseks ülevaate access. |
| **On liiga palju globaalse administraatorid** | Pole veel globaalne administraatorit, kui soovitatav. | Kui teil on suur hulk globaalse administraatorid, on tõenäoline, et kasutajad saavad rohkem õigusi, kui nad vajavad. Kasutajate teisaldamine vähem õigustega rollidele või teha mõned neist õigus rolli asemel jäädavalt määratud. |

## <a name="configure-security-alert-settings"></a>Turvalisus Teatiste sätete konfigureerimine

Saate kohandada mõnda soovitud turbeteatised PIM töötamiseks oma keskkonna ja turbe-eesmärke. Järgmiste juhiste sätted tera saavutamiseks.

1. Valige paani **Azure AD õigustega identiteetide haldamisviis** armatuurlaualt [Azure portaali](https://portal.azure.com/) sisse logima.
2. Valige **hallatavad õigustega rollid** > **sätted** > **teatiste sätted**.

    ![Liikuge turbesätted teatised][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Teatise "Rollid on aktiveeritud sageli liiga"

See teatis käivitab, kui kasutaja aktiveerib sama õigustega rolli mitu korda määratud aja jooksul. Saate konfigureerida nii aja jooksul ja arv kordi.

- **Aktiveerimise pikendamise ajavahemiku**: tundi, minutit määrata, ja teine aja jooksul, mida soovite kasutada jälgida kahtlaste pikendamine.

- **Aktiveerimise pikendamine arv**: määratud arv kordi 2 kuni 100, mis teie arvates väärt teatise valitud aja jooksul. Saate muuta see säte liigutamine või tippides mitu tekstivälja.


### <a name="there-are-too-many-global-administrators-alert"></a>Teatise "On liiga palju globaalse administraatorid"

PIM käivitab teemasse, kui kaks erinevat kriteeriumid ja saate konfigureerida mõlemad. Esmalt peate saavutamiseks globaalne administraatorite teatava piiri. Teiseks protsendimäära oma kokku rollimääranguid peavad olema üld administraatorid. Kui vastate ainult ühte mõõtmise, teatise ei kuvata.  

- **Globaalne administraatorite miinimum arv**: määrake globaalse administraatorid, 2 kuni 100, kaaluge ebaturvaliste summa arv.

- **Globaalne administraatorite protsent**: määrake protsent administraatoritele, kes on globaalne administraatorid, 0% 100%, mis on teie keskkonnas ebaturvaliste.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Teatise "Administraatorid ei kasuta nende õigustega rollid"

See teatis käivitab, kui kasutaja läheb teatud aja jooksul aktiveerimata rollid.

- **Päevade arv**: määrake päevade vahemikus 0 – 100, et kasutaja võib minna rolli aktiveerimata arv.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
