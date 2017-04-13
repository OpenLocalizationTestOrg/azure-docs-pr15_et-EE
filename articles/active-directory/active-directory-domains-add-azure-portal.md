<properties
    pageTitle="Kohandatud domeeninime lisamine Azure Active Directory eelvaade | Microsoft Azure'i"
    description="Kuidas lisada oma ettevõtte domeeninimed Azure Active Directory ja kuidas kontrollida domeeni nimi."
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
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Kohandatud domeeninime lisamine Azure Active Directory eelvaade

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-domains-add-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-add-domain.md)

Teil on ühe või mitme domeeninimede, mida teie asutus kasutab äri ja kasutajate logige sisse oma ettevõtte võrguga, kasutades teie ettevõtte domeeni nimi. Azure Active Directory (Azure AD) eelvaate kasutamisel saate lisada oma ettevõtte domeeninime kasutada ka Azure AD. [Mis on eelvaates?](active-directory-preview-explainer.md) See võimaldab teil määrata kataloogis kasutajanimed, mis on tuttav kasutajaid, näiteks ‘alice@contoso.com.’ on lihtne:

1. Kohandatud domeeninime lisamine kataloogi
2. Lisage DNS-i kirje domeeninime nimi domeeniregistraatori juures
3. Kohandatud domeeninime Azure AD kinnitamine

## <a name="how-do-i-add-a-domain-name"></a>Kuidas lisada domeeninime?

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **Azure Active Directory** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Enne ***kataloogi nimi*** , valige **domeeninimede**.

4. Enne ** *kaustanimi* - domeeninimede** , valige käsk **Lisa** .

  ![Valige käsk Lisa](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Enne **domeeni nimi** , oma kohandatud domeeni nimi, sisestage väljale, nt "contoso.com", ja seejärel valige **Lisa domeen**. Kindlasti ka .com, .net-i või muu ülataseme pikendamine.

6. ***Domeeninimi*** enne (ehk teisisõnu öeldes tera avatavas sisaldava uue domeeninime pealkiri) saada kasutavate Azure AD kinnitamaks, et teie asutus kuulub kohandatud domeeninime DNS-i kirje teave.

  ![DNS-i kirje teabe saamine](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Nüüd, kui olete lisanud domeeni nimi, Azure AD veenduge, et ettevõtte kuulub domeeni nimi. Enne Azure AD saab teha kontrolli, peate lisama DNS-i tsooni faili domeeninime DNS-i kirje. Selle toimingu jaoks domeeninime domeeninimeregistraatori veebisaidil.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>DNS-i kirje lisamine domeeni nimi domeeniregistraatori juures

Järgmise juhise juurde oma kohandatud domeeninime kasutada Azure AD on domeeni DNS-i tsooni faili värskendada. See võimaldab Azure AD kinnitamaks, et ettevõtte kuulub kohandatud domeeni nimi.

1.  Logige sisse domeeni domeeninimeregistraatorilt. Kui teil pole juurdepääsu värskendada DNS-i kirje, küsige isiku või meeskonna, kellel on see juurdepääs juhise 2 ja teile teada, kui see on lõpule viidud.

2.  Värskendage domeeni DNS-i tsoonifail DNS-i kirje, mis annab teie käsutusse Azure AD lisamisega. DNS-i kirje võimaldab Azure AD oma domeeni omandiõiguse kinnitamiseks. DNS-i kirje ei muuda mis tahes muud käitumised, nt Meilimarsruutimise või web hosting.

DNS-i kirje lisamist abi, lugege [veebisaidil populaarsete DNS-i registraatorit DNS-i kirje lisamise juhised](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Veenduge, et domeeninimi Azure AD

Kui olete lisanud DNS-i kirje, olete valmis, kinnitamaks, et domeeninimi Azure AD.

Domeeninime saab kontrollida ainult siis, kui DNS-i kirjed on levitatud. Selle paljundamine sageli võtab ainult sekundi, kuid see võib vahel võtta tund või rohkem. Kui kinnitamine ei toimi esimest korda, proovige hiljem uuesti.

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **Sirvi**, sisestage kasutaja halduse tekstivälja ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Enne **kasutajahalduse - domeeninimede** , valige kinnitamata domeeni nimi, mida soovite kontrollida.

4. Enne ***domainname*** (ehk teisisõnu öeldes tera avatavas sisaldava uue domeeninime pealkiri), valige **Kinnita** kontrollimise lõpetamiseks.

Nüüd saate [määrata kasutajate nimed, mis sisaldavad teie kohandatud domeeni nimi](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Tõrkeotsing

Kui te ei saa kinnitada domeeni nimi, proovige järgmist. Vaatame kõige levinum käivitamine ja allapoole vähem levinud töötada.

1.  **Oodake üks tund**. DNS-i kirjed tuleb enne Azure AD saate kontrollida domeeni kajastuma. See võib võtta tund või rohkem.

2.  **Kontrollige, kas DNS-i kirje on sisestanud, ja see on õige**. Seda toimingut veebisaidil domeeni nimi domeeniregistraatori jaoks. Azure AD ei saa kinnitada domeeni nimi, kui DNS-i kirje ei esine DNS-i tsooni faili, või kui see pole täpne vaste DNS-i kirje selle Azure AD andnud teile. Kui teil pole juurdepääsu nimi domeeniregistraatori juures domeeni DNS-i kirjete värskendamine, DNS-i kirje ühiskasutusse isiku või meeskonna teie asutuses, kellel on see juurdepääs, ja paluge tal DNS-i kirje lisamine.

3.  **Kustutage teise kataloogist Azure AD domeeni nimi**. Ainult ühe kataloogi saab kontrollida domeeni nimi. Kui domeeninime kandis varem teise kataloogi, see tuleb kustutada seal enne saab kontrollida, uue kataloogis. Kustutamise domeeninimede kohta lisateabe saamiseks vt [kohandatud domeeninimede haldamine](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Lisage veel kohandatud domeeni nimed

Kui teie asutus kasutab mitu kohandatud domeeni nime, nagu "contoso.com" ja "contosobank.com", saate need lisada kuni 900 domeeninimede. Kasutage samu juhiseid selle artikli lisada iga teie domeeninime.

## <a name="next-steps"></a>Järgmised sammud

[Kohandatud domeeninimede haldamine](active-directory-domains-manage-azure-portal.md)
