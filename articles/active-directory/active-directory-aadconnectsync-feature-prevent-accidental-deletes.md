<properties
   pageTitle="Azure'i AD-ühendus sünkroonimine: vältida juhuslikku kustutab | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse funktsiooni vältida juhuslikku kustutab (vältida juhuslikku kustutamist) Azure'i AD-ühenduse."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure'i AD-ühendus sünkroonimine: vältida juhuslikku kustutab
Selles teemas kirjeldatakse funktsiooni vältida juhuslikku kustutab (vältida juhuslikku kustutamist) Azure'i AD-ühenduse.

Kui installimisel Azure'i AD-ühenduse, juhuslike kustutab vaikimisi lubatud ja konfigureeritud lubama mitte rohkem kui 500 kustutab koos ekspordi. See funktsioon on mõeldud teie juhusliku konfiguratsioonimuudatuste ja muudatuste eest kaitsta oma kohapealse kataloogi, mis võivad mõjutada paljud kasutajad ja muude objektide.

Levinumad stsenaariumid, kui palju kustutab kaasata:

- Muudatuste [filtreerimine](active-directory-aadconnectsync-configure-filtering.md) , kus kogu [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) või [Domeen](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) on valimata.
- Kõigil objektidel on OU kustutatakse.
- Organisatsiooniüksusega ümber nii, et kõigil objektidel peetakse sünkroonimiseks väljas.

Vaikeväärtust, milleks on 500 objektide saab muuta PowerShelli abil `Enable-ADSyncExportDeletionThreshold`. Peaksite konfigureerima seda väärtust vastavalt ettevõtte suurust. Alates sünkroonimine ajasti töötab iga 30 minuti järel, väärtus on arv, kustutab näinud 30 minuti jooksul.

Kui loendis on liiga palju kustutab etapiviisilise eksportimisel Azure AD, siis ekspordi lakkab töötamast ja saate meili umbes järgmine:

![Juhusliku kustutab meilisõnumite vältimine](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Tere (tehniline kontakt). (Ajal) identiteedi sünkroonimise teenuse avastada, et kustutamised arv ületab läve konfigureeritud kustutamise (ettevõtte nimi). Kokku (arv) objektide saadeti kustutamiseks klõpsake selle identiteedi sünkroonimine. See täidetud või ületanud konfigureeritud kustutamise lävi väärtus (arv) objektid. Teil tuleb kinnitust, et need kustutamised peaks olema töödeldud enne jätkame. Lugege artiklit vältida juhuslikku kustutamist loendis selle meilisõnumi tõrke kohta lisateavet.*

Saate vaadata ka oleku `stopped-deletion-threshold-exceeded` kui vaatate **Sünkroonimise Service Manager** UI ekspordi profiili.
![Vältida juhuslikku kustutab sünkroonimine Service Manager UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Kui see oli ootamatu, uurida ja tegevuste. Millised objektid on kustutatakse peagi vaatamiseks tehke järgmist.

1. Menüü Start kaudu **sünkroonimise teenuse** käivitamine.
2. Minge **konnektorid**.
3. Valige tüüp **Azure Active Directory**konnektor.
4. Valige jaotises **toimingud** paremal **Otsingu konnektori ruumi**.
5. Klõpsake jaotises **ulatus**hüpikaknas valige **Katkestatud, kuna** ja valige kunagi varem. Klõpsake nuppu **Otsi**. Sellelt lehelt leiate ülevaate kõigi objektide kustutatakse peagi. Iga üksuse klõpsates saate lisateavet objekti. Võite klõpsata ka lisada täiendavad atribuudid on nähtav koordinaatvõrgu **Veeru säte** .

![Otsingu konnektori ruumi](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Kui kõik kustutab on soovitud, siis tehke järgmist.

1. Ajutiselt keelata kaitse ja lasta need kustutab läbida, käivitage PowerShelli cmdlet-käsk: `Disable-ADSyncExportDeletionThreshold`. Pakuvad Azure AD üldadministraatori kontot ja parooli.
![Identimisteave](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Azure Active Directory konnektor valituks, valige **Käivita** toiming ja valige **ekspordi**.
3. Kaitse uuesti lubamiseks käivitage PowerShelli cmdlet-käsk: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Järgmised sammud

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
