<properties
    pageTitle="Lisage oma kohandatud domeeni nimi ja välise sisselogimise Azure Active Directory häälestamine | Microsoft Azure'i"
    description="Kuidas lisada oma ettevõtte domeeninimed Azure Active Directory ja kuidas häälestada ühendatud sisselogimise Azure Active Directory ja teie asutusesisese federation lahendus vahel."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Azure Active Directory domeeni lisamine

Kohandatud domeeni nimi, näiteks "contoso.com", saate konfigureerida nii, et kasutajad contoso.com on ühendatud üks sisselogimise kogemus oma ettevõtte võrku. Kui teil juba on Active Directory Federation Services (AD FS) või muu liiduserveri, mis töötab teie ettevõtte võrguga, saate konfigureerida Azure AD kasutada oma kohandatud domeeni nime Azure'i AD-ühenduse tööriista abil. Saate kasutada Azure'i AD-ühenduse juurutamiseks uue AD FS-i keskkonnas, ja konfigureerimine mis ühendatud ühekordse sisselogimise Azure AD.

Kui ei ole ja ei kavatse juurutamine AD FS-i või muu liiduserveri, järgige neid juhiseid: [Azure Active Directory domeeni nime lisamine](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Kohandatud domeeninime lisamine kataloogi

1. Kasutajakonto, mis on Azure AD kataloogi üldadministraator [Azure klassikaline portaali](https://manage.windowsazure.com/) sisse logida.

2. **Active Directory**, avage kataloogi ja valige vahekaart **Domeenid** .

3. Käsuribal, valige **Lisa**ja seejärel sisestage oma kohandatud domeeni, näiteks "contoso.com" nimi. Kindlasti ka .com, .net-i või muu ülataseme pikendamine.

4. Märkige ruut **I leping selle domeeni jaoks ühekordse sisselogimise minu kohaliku Active Directoryga konfigureerimiseks** .

5. Valige **Lisa**.

Käivitage tööriist Azure'i AD-ühenduse saada DNS-i kirje kasutavate Azure AD domeeni kinnitamiseks. Kuvatakse viisardi juhises **Azure AD domeeni** DNS-i kirje. Saate vaadata, mis selle viisardi juhises, näeb välja nagu [neid juhiseid](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Kui teil pole tööriista Azure'i AD-ühenduse, saate [alla laadida siin](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>DNS-i kirje lisamine domeeni nimi domeeniregistraatori juures

Järgmise juhise juurde oma kohandatud domeeninime kasutada Azure AD on domeeni DNS-i tsooni faili värskendada. See võimaldab Azure AD kinnitamaks, et ettevõtte kuulub kohandatud domeeni nimi.

1. Logige sisse veebilehel domeeninimeregistraatori jaoks teie domeeni nimi. Kui teil pole juurdepääsu seda teha, paluge isiku või meeskonna ettevõttes, kellel on see juurdepääs juhise 2 ja teile teada, kui see on lõpule viidud.

2. Värskendage domeeni DNS-i tsoonifail DNS-i kirje, mis annab teie käsutusse Azure AD lisamisega. DNS-i kirje võimaldab Azure AD oma domeeni omandiõiguse kinnitamiseks. DNS-i kirje ei muuda mis tahes muud käitumised, nt Meilimarsruutimise või web hosting.

Selles etapis tuleb abi, lugege [veebisaidil populaarsete DNS-i registraatorit DNS-i kirje lisamise juhised](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Veenduge, et domeeninimi Azure AD

Kui olete lisanud DNS-i kirje, olete valmis, kinnitamaks, et domeeninimi Azure AD.

Domeeni kinnitamiseks valige **järgmise** **Azure AD domeeni** järgu Azure'i AD-ühenduse viisard. Azure AD otsib DNS-i tsooni faili domeeni DNS-i kirje. Azure AD kinnitamine ainult domeeni nimi, kui DNS-i kirjed on levitatud. Paljundamine sageli võtab ainult sekundi, kuid see võib vahel võtta tund või rohkem. Kui kinnitamine ei toimi esimest korda, proovige hiljem uuesti.

Seejärel jätkake Azure'i AD-ühenduse viisardi ülejäänud juhised. Saate sünkroonida oma Windows Server AD saajatele Azure AD. Sünkroonitud kasutajate jaoks konfigureeritud domeeni saab oma ettevõtte võrku ühendatud ühekordse sisselogimise teenuse avamiseks Azure AD.

## <a name="troubleshooting"></a>Tõrkeotsing

Kui te ei saa kinnitada domeeni nimi, proovige järgmist. Vaatame kõige levinum käivitamine ja allapoole vähem levinud töötada.

1.  **Oodake üks tund**. DNS-i kirjed tuleb enne Azure AD saate kontrollida domeeni kajastuma. See võib võtta tund või rohkem.

2.  **Kontrollige, kas DNS-i kirje on sisestanud, ja see on õige**. Seda toimingut veebisaidil domeeni nimi domeeniregistraatori jaoks. Azure AD ei saa kinnitada domeeni nimi, kui DNS-i kirje ei esine DNS-i tsooni faili, või kui see pole täpne vaste DNS-i kirje selle Azure AD andnud teile. Kui teil pole juurdepääsu nimi domeeniregistraatori juures domeeni DNS-i kirjete värskendamine, DNS-i kirje ühiskasutusse isiku või meeskonna teie asutuses, kellel on see juurdepääs, ja paluge tal DNS-i kirje lisamine.

3.  **Kustutage teise kataloogist Azure AD domeeni nimi**. Ainult ühe kataloogi saab kontrollida domeeni nimi. Kui domeeninime kandis varem teise kataloogi, see tuleb kustutada seal enne saab kontrollida, uue kataloogis. Kustutamise domeeninimede kohta lisateabe saamiseks vt [kohandatud domeeninimede haldamine](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Lisage veel kohandatud domeeni nimed

Kui teie asutus kasutab mitu kohandatud domeeni nime, nagu "contoso.com" ja "contosobank.com", saate need lisada kuni 900 domeeninimede. Kasutage samu juhiseid selle artikli lisada iga teie domeeninime.

## <a name="next-steps"></a>Järgmised sammud

-   [Kohandatud domeeninimede haldamine](active-directory-add-manage-domain-names.md)
-   [Lisateavet domeeni haldamise põhimõtet Azure AD](active-directory-add-domain-concepts.md)
-   [Kuva ettevõtte kasutaja sümboolika, kui teie kasutajad sisse logida](active-directory-add-company-branding.md)
-   [PowerShelli kasutamine Azure AD domeeninimede haldamine](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
