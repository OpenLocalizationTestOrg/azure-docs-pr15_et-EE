<properties
    pageTitle="Azure Active Directory ühendatud rakenduste device põhineva juurdepääsu poliitika määramine | Microsoft Azure'i"
    description="Azure'i AD-ühendus rakenduste device põhineva juurdepääsu poliitikate seadmine."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Azure Active Directory ühendatud rakenduste device põhineva juurdepääsu poliitika määramine


Azure Active Directory (Azure AD) device põhineva juurdepääsu aitavad teil kaitsta ettevõtte ressursse.

- Access püüab tundmatu või mittehallatava seadmetest.
- Seadmete turbepoliitikate teie ettevõtte ei vasta.

Juurdepääsu ülevaate leiate teemast [Azure Active Directory juurdepääsu](active-directory-conditional-access.md).

Saate määrata device põhineva juurdepääsu kaitsmiseks poliitikaid rakendada need rakendused.

- Office 365 SharePoint Online'i kaitsta teie ettevõtte saitidele ja dokumentidele
- Office 365 Exchange Online, kaitsta oma ettevõtte e-posti
- Service (SaaS) rakenduste, mis on ühendatud Azure AD autentimise tarkvara
- Kohapealse rakendusi, mis on avaldatud Azure AD Rakenduse puhverserveri teenuste abil

Azure'i portaalis device põhineva juurdepääsu poliitika, seadmiseks avage konkreetse rakenduse kataloogis.


  ![Azure'i portaali kataloogis rakenduste loendist] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Rakenduste")


Valige rakendus ja seejärel klõpsake vahekaarti **konfigureerimine** seadmiseks juurdepääsu poliitika.  


  ![Rakenduse konfigureerimine] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Vastavalt seadme juurdepääsureeglid")




Device põhineva juurdepääsu poliitika, **vastavalt seadme juurdepääsureeglid** jaotises **Luba juurdepääsureeglid**, valige **kohta**.

Device põhineva juurdepääsu poliitika on kolm osa.

- **Kui soovite rakendada**. Kasutajate poliitika kehtib ulatust.

- **Seadme reeglid**. Tingimuste seade peab vastama enne selle pääsete juurde rakenduse.

- **Taotluse täitmine**. Klientrakendused (või brauseri emakeel) poliitika kehtib.

  ![Kolm osa device põhineva juurdepääsu poliitika] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Vastavalt seadme juurdepääsureeglid")


## <a name="select-the-users-the-policy-applies-to"></a>Valige kasutajad, kehtib poliitika

Jaotises **Rakenduskoht** saate valida kasutajad selle poliitika kehtib ulatust.

Kui loote mõnda Accessi poliitika ulatus kasutajate jaoks on teil kaks võimalust:

- **Kõik kasutajad**. Kõik kasutajad, kellel on juurdepääs rakenduse poliitika rakendada.

- **Rühmad**. Piirake poliitika kasutajatele, kes on teatud rühma liige.

![Rakenda poliitika kõigile kasutajatele või rühmale] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Rakendamine")


 Kasutaja välistamine poliitika, märkige ruut **peale** . See on kasulik, kui soovite mõne kindla kasutaja rakenduse avamiseks ajutiselt õiguste andmine. Valige see suvand, näiteks, kui mõni kasutaja on seadmed, mis pole juurdepääsu valmis. Seadmete pole registreeritud veel või need on välja nõuetele vastavus.


## <a name="select-the-conditions-that-devices-must-meet"></a>Valige tingimused, millele peavad vastama

**Seadme reeglite** abil saate määrata tingimused seade peab vastama rakenduse avamiseks.

Saate määrata device põhineva juurdepääsu järgmist tüüpi seadme jaoks.

- Windows 10 tähtpäeva Update, Windows 8.1 ja Windows 7
- Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ja Windows Server 2008 R2
- iOS-seadmetes (iPadis, iPhone'is)
- Androidi seadmed

Maci tugi on tulekul.

  ![Rakenda poliitika ja seadmed] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Rakenduste")

 >[AZURE.NOTE] Domeeni ühendatud ja Azure'i AD-ühendatud seadmete erinevuste kohta leiate teavet teemast [kasutamine Windows 10 seadmete oma töökoha](active-directory-azureadjoin-windows10-devices.md).

Teil on kaks võimalust seadme reeglid.

- **Kõigis seadmetes peab olema nõuetele**. Kõik seadme platvormid, kuhu juurdepääs rakenduse peab olema nõuetele. Käivitage platvormide, mis ei toeta device põhineva juurdepääsu seadmete on juurdepääs.

- **Ainult valitud seadmed peavad olema nõuetele**. Ainult kindla seadme platvormid peab olema nõuetele. Muude platvormide jaoks loodud või muude platvormide jaoks loodud, millele pääsete juurde rakenduse, on juurdepääs.

  ![Seadme reeglite ulatuse määramine] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Rakenduste")

Azure'i AD-ühendatud seadmete on nõuetele, kui need on märgitud **nõuetele vastavuse** kataloogis Intune või muu mobiilsideseadme halduse süsteemi, mis ühendab Azure AD.

Domeeni ühendatud seade vastab kui:

- See on registreeritud Azure AD. Paljud organisatsioonid loeb domeeni ühendatud seadmete usaldusväärsed seadmed.
- See on märgitud **nõuetele vastavuse** Azure AD System Center Configuration Manager 2016.

 ![Mis on nõuetele vastavuse domeeni ühendatud seadmete] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Seadme reeglid")

Windowsi isikliku seadmed on nõuetele, kui need on märgitud **nõuetele vastavuse** kataloogis Intune või muu mobiilsideseadme halduse süsteemi, mis ühendab Azure AD.

Mitte-Windowsi seadmed on nõuetele, kui need on märgitud **nõuetele vastavuse** kataloogis Intune.

 >[AZURE.NOTE] Erinevaid spikritest Azure AD seadme nõuetele vastavuse seadistamise kohta leiate lisateavet teemast [Azure Active Directory juurdepääsu](active-directory-conditional-access.md).


Saate valida ühe või mitme seadme platvormid device põhineva juurdepääsu poliitika. See hõlmab Androidi, iOS-i, Windows Mobile (Windows 8.1 telefonid ja tahvelarvutid) ja Windows (kõik muud Windowsi seadmetes, sh kõik Windows 10 seadmed).
Poliitika hindamise ilmneb ainult valitud platvormid. Kui seade, mis avaldab access ei tööta üks valitud platvormide, seadme pääsete juurde rakenduse, kui kasutajal on juurdepääs. Seadme poliitika hinnatakse.

![Valige seadme reeglite platvormid] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Seadme reeglid")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Poliitika hindamise tüüpi seadmine

**Taotluse täitmine** jaotises määrata poliitika hindab kasutaja või seadme Accessi rakenduste tüübi.

Teil on kaks võimalust kaasata rakenduse tüübi jaoks.

- Brauseri- ja algsete rakenduste
- Algsete rakenduste ainult

![Valige brauseris või algsete rakenduste] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Rakenduste")

Accessi rakenduste põhimõtted märkige **brauseri ja algsete rakenduste puhul**. Seejärel saate kaasata.

- (Nt Microsoft Edge Windows 10) või iOS-i Safari brauserites.
- Mida kasutada Active Directory autentimise Raamatukogu (ADAL) mis tahes platvormi või mis WebAccountManager (WAM) API Windows 10 rakendused.

>[AZURE.NOTE] Lisateavet brauserite ja muude kaalutluste kohta kasutaja, kellel on juurdepääs seadme põhinev, serdi asutuse kaitstud rakenduse teemast [Azure Active Directory juurdepääsu](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>E-postile juurdepääsu kaitsmiseks Exchange ActiveSynci põhiseid rakendusi

Office 365 Exchange Online'i rakendustes saate blokeerida e-postile juurdepääsu Exchange ActiveSynci-põhise e-posti rakenduste Exchange ActiveSynci.

![Exchange ActiveSynci vastavuse suvandid] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Rakenduste")

![Nõua ühilduv seadme juurdepääsu e-posti] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Rakenduste")


## <a name="next-steps"></a>Järgmised sammud

- [Azure Active Directory juurdepääsu](active-directory-conditional-access.md)
