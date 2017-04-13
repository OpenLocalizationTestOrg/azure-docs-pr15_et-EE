<properties
    pageTitle="Mis on Azure MFA rakenduse paroolid?"
    description="Selle lehe aitab kasutajatel mõista, mis on rakenduse paroolid ja mida nad on kasutatakse koos arvesse Azure'i MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Mis on rakenduse paroolid Azure'i Mitmikautentimise?

Teatud-brauseri rakendusi, nt Apple algset e-posti klient, mis kasutab Exchange Active Synci, ei toeta praegu mitmikautentimise. Mitmikautentimise on lubatud kasutaja kohta. See tähendab, et kui kasutaja on lubatud mitmikautentimise ja nad proovite kasutada-brauseri rakendused, neid ei saa seda teha. Rakenduse parooli võimaldab see, et.

>[AZURE.NOTE] Kaasaegne autentimine Office 2013 klientidele
>
> Office 2013 kliendid (sh Outlook) nüüd toetavad uus autentimise protokollid ja lubanud Mitmikautentimise tugi.  See tähendab, et kui lubatud, rakenduse paroolid ei ole vaja Office 2013 kliendi jaoks.  Lisateabe saamiseks lugege teemat [Office 2013 kaasaegne autentimise avaliku eelvaate teatamine](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Rakenduse paroolide kasutamise kohta

Järgnevalt mõned asjad meeles pidada, kuidas kasutada rakenduse paroolid.

- Tegelik parooli luuakse automaatselt ja kasutaja ei esitata. See on, kuna automaatselt loodud parool raskem mõelda leidmise ja on turvalisem.
- Praegu on piiratud 40 paroolid kasutaja kohta. Kui proovite luua, kui olete jõudnud limiit, palutakse teil ühe oma rakenduse parooliga selleks, et luua uue kustutamine.
- Soovitatav on luua rakenduse paroolid seade ja pole rakenduste kohta. Näiteks saate luua ühe rakenduse parooli sülearvuti ja selle rakenduse parooli kasutada kõigi rakenduste selle sülearvutis.
- Teile rakenduse parooli esmakordsel sisselogimisel.  Kui vajate täiendavaid neist, saate neid luua.

![Häälestamine](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Kui olete rakenduse parooli, selle asemel kasutada algset parooli neid-brauseri rakendustega.  Nii näiteks, kui kasutate mitmikautentimise ja Apple'i algset e-posti kliendi oma telefonis.  Kasutage rakenduse parooli, et see saab mööduda mitmikautentimise ja tööd jätkata.

## <a name="creating-and-deleting-app-passwords"></a>Loomine ja kustutamine rakenduse paroolid
Teie algne sisselogimine ajal teile rakenduse parool, mida saate kasutada.  Lisaks saate luua ja kustutada rakenduse paroolid hiljem.  Kuidas seda teha, sõltub sellest, kuidas kasutada mitmikautentimise.  Valige üks, et enamik tingimustest on täidetud.

Kuidas kasutada mitut tegurit authentiation|Kirjeldus
:------------- | :------------- |
[Seda kasutada koos teenusekomplektiga Office 365](#creating-and-deleting-app-passwords-with-office-365)|  See tähendab, et kas soovite luua rakenduse paroolid Office 365 portaali kaudu.
[Ma ei tea](#creating-and-deleting-app-passwords-with-myapps-portal)|See tähendab, et soovite luua rakenduse parooli [https://myapps.microsoft.com](https://myapps.microsoft.com) kaudu
[Selle abil Microsoft Azure'i](#create-app-passwords-in-the-azure-portal)| See tähendab, et soovite luua rakenduse paroolid Azure portaali kaudu.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Loomine ja kustutamine Office 365 rakenduse paroolid

Kui kasutate teenusekomplekti Office 365 mitmikautentimise mida soovite loomine ja kustutamine Office 365 portaali kaudu rakenduse paroolid.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Office 365 portaalis rakenduse paroolide loomine
--------------------------------------------------------------------------------

1. [Office 365 portaali](https://login.microsoftonline.com/)sisse logima.
2. Valige vidin paremas ülanurgas ja valige Office 365 sätted.
3. Klõpsake täiendavate turvalisus kinnitamine.
4. Klõpsake parempoolsel paanil linki, mis ütleb, et **värskendada minu telefoninumbrid kasutatakse konto turvalisus.** 
 ![Häälestamine](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. See viib teid leht, mis võimaldab teil muuta oma sätteid.
![Pilveteenuse](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Ülaosas kõrval suurema turvalisuse tagamiseks kinnitamine, klõpsake **Rakenduse paroolid.**
7. Klõpsake nuppu **Loo**.
![Pilveteenuse](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Sisestage parool rakenduse nimi ja klõpsake nuppu **edasi**.
![Rakenduse parooli loomine](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Rakenduse parool kopeerimine lõikelauale ja kleepige see oma rakenduse.
![Rakenduse parooli loomine](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Office 365 portaalis rakenduse paroolid kustutamine
--------------------------------------------------------------------------------


1. [Office 365 portaali](https://login.microsoftonline.com/)sisse logima.
2. Valige vidin paremas ülanurgas ja valige Office 365 sätted.
3. Klõpsake täiendavate turvalisus kinnitamine.
4. Klõpsake parempoolsel paanil linki, mis ütleb, et **värskendada minu telefoninumbrid kasutatakse konto turvalisus.** 
 ![Häälestamine](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. See viib teid leht, mis võimaldab teil muuta oma sätteid.
![Pilveteenuse](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Ülaosas kõrval suurema turvalisuse tagamiseks kinnitamine, klõpsake **Rakenduse paroolid.**
7. Rakenduse parool kõrval, mida soovite kustutada, klõpsake nuppu **Kustuta**.
![Rakenduse parooli kustutamine](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Kustutamise kinnitamiseks nuppu **Jah**.
![Selle kustutamise kinnitamine](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Pärast kustutatakse rakenduse parooli saate klõpsata nuppu **Sule**.
![Sulgege](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Loomine ja kustutamine rakenduse paroolid portaalis minu rakendused.
Kui te pole kindel, kuidas kasutada mitmikautentimise, siis saate alati loomine ja kustutamine rakenduse paroolid portaalis minu rakendused.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Minu rakendused portaalis rakenduse parooli loomine

1. [Https://myapps.microsoft.com](https://myapps.microsoft.com) sisselogimine
2. Valige kohal profiil.
3. Valige täiendavad turvalisus kinnitamine.
![Pilveteenuse](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. See viib teid leht, mis võimaldab teil muuta oma sätteid.
![Häälestamine](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Ülaosas kõrval suurema turvalisuse tagamiseks kinnitamine, klõpsake **Rakenduse paroolid.**
6. Klõpsake nuppu **Loo**.
![Rakenduse parooli loomine](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Sisestage parool rakenduse nimi ja klõpsake nuppu **edasi**.
![Rakenduse parooli loomine](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Rakenduse parool kopeerimine lõikelauale ja kleepige see oma rakenduse.
![Rakenduse parooli loomine](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Minu rakendused portaalis rakenduse parooli kustutamine

1. [Https://myapps.microsoft.com](https://myapps.microsoft.com) sisselogimine
2. Valige kohal profiil.
3. Valige täiendavad turvalisus kinnitamine.
![Pilveteenuse](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. See viib teid leht, mis võimaldab teil muuta oma sätteid.
![Häälestamine](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Ülaosas kõrval suurema turvalisuse tagamiseks kinnitamine, klõpsake **Rakenduse paroolid.**
6. Rakenduse parool kõrval, mida soovite kustutada, klõpsake nuppu **Kustuta**.
![Rakenduse parooli kustutamine](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Kustutamise kinnitamiseks nuppu **Jah**.
![Selle kustutamise kinnitamine](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Pärast kustutatakse rakenduse parooli saate klõpsata nuppu **Sule**.
![Sulgege](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Azure'i portaalis rakenduse paroolide loomine

Kui kasutate Azure mitmikautentimise soovite luua rakenduse paroolid Azure portaali kaudu.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Rakenduse paroolid Azure portaali loomine

1. Logige sisse Azure haldusportaali.
2. Ülaservas, paremklõpsake oma kasutajanimi ja valige täiendavad turvalisus kinnitamine.
3. Valige lehel proofup kohal rakenduse paroolid
4. Klõpsake nuppu **Loo**.
5. Sisestage parool rakenduse nimi ja klõpsake nuppu **edasi**
6. Rakenduse parool kopeerimine lõikelauale ja kleepige see oma rakenduse.
![Pilveteenuse](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Rakenduse paroolid Azure'i portaalis kustutamine

1. Logige sisse Azure haldusportaali.
2. Ülaservas, paremklõpsake oma kasutajanimi ja valige täiendavad turvalisus kinnitamine.
3. Ülaosas kõrval suurema turvalisuse tagamiseks kinnitamine, klõpsake **Rakenduse paroolid.**
4. Rakenduse parool kõrval, mida soovite kustutada, klõpsake nuppu **Kustuta**.
5. Kustutamise kinnitamiseks nuppu **Jah**.
6. Pärast kustutatakse rakenduse parooli saate klõpsata nuppu **Sule**.
![Sulgege](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
