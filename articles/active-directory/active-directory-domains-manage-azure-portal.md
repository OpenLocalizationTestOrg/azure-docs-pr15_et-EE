<properties
    pageTitle="Kohandatud domeeninimesid eelvaate Azure Active Directory haldamine | Microsoft Azure'i"
    description="Juhtimise põhimõtet ja õpetused haldamise Azure Active Directory domeeni nimi"
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
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Kohandatud domeeninimesid eelvaate Azure Active Directory haldamine

Domeeninimi on oluline osa tunnuse palju directory ressursse: see on osa kasutaja nimi või meiliaadress kasutaja, rühma, aadressi ja saab rakenduse ID URI rakenduse osa. Ressursi Azure Active Directory (Azure AD) preview's saate lisada domeeni nimi, mis on juba kinnitatud kuuluma ressursi sisaldava kausta. [Mis on eelvaates?](active-directory-preview-explainer.md) Ainult globaalne administraator saab teha domeenihalduse tööülesandeid Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Määrake esmane domeeninimi Azure AD kataloogi jaoks

Kataloogi loomisel algne domeeninimi, näiteks "contoso.onmicrosoft.com", on ka esmane domeeninimi. Põhidomeen on uus kasutaja domeeni nimi, kui loote uue kasutaja. See muudab protsessi jaoks luua uusi kasutajaid portaalis administraator. Esmane domeeninimi muutmiseks tehke järgmist.

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **Azure Active Directory** ja seejärel valige **Sisesta**.

    ![Avamine kasutajate haldamine](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Enne ***kataloogi nimi*** , valige **domeeninimede**.

4. Enne ** *kaustanimi* - domeeninimede** , valige domeeni nimi, mida soovite muuta esmane domeeninimi.

5.  ***Domainname*** enne (ehk teisisõnu öeldes tera avatavas sisaldava uue domeeninime pealkiri), valige käsk **Muuda esmaseks** . Kinnitage oma valik, kui seda küsitakse.

    ![Domeeninime esmane tegemine](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Saate muuta olema kinnitatud kohandatud domeen, mis on ühendatud kataloogi jaoks esmane domeeninimi. Kataloogi jaoks põhidomeen muutmine ei muuda kasutajanimed mis tahes olemasolevatele kasutajatele.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Kohandatud domeeni nime lisamine oma Azure AD

Saate lisada kuni 900 kohandatud domeeninimesid iga Azure AD kataloogi. [Täiendavad kohandatud domeeninime](active-directory-domains-add-azure-portal.md) lisamine protsess on sama esimese kohandatud domeeni nime.

## <a name="add-subdomains-of-a-custom-domain"></a>Lisada kohandatud domeeni alamdomeene

Kui soovite lisada kolmanda taseme Domeen nimi, näiteks "europe.contoso.com" kataloogi, tuleks esmalt lisada ja kinnitada? teise taseme domeen, nt contoso.com. Alamdomeen on Azure AD automaatselt kontrollida. Alamdomeen, mille just lisasite vastavuse kontrollimist vaatamiseks värskendage lehte brauseris, kus on loetletud domeenid.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Mida teha, kui soovite muuta domeeniregistraatori DNS-i jaoks kohandatud domeeninime

Kui soovite muuta domeeniregistraatori DNS-i jaoks kohandatud domeeninime, saate jätkata Azure AD, ise katkestusteta ja Täiendavad konfigureerimistoimingud ilma oma kohandatud domeeninime kasutamine. Kui kasutate teenusekomplekti Office 365 oma domeeni nimi, Intune või muid teenuseid, mis sõltuvad kohandatud domeeninimesid Azure AD, viidata nende teenuste dokumentatsioonist.

## <a name="delete-a-custom-domain-name"></a>Kohandatud domeeni nime kustutamine

Kohandatud domeeninime saate kustutada oma Azure AD, kui teie asutus kasutab enam selle domeeni nimi, või kui teil on vaja seda domeeninime kasutamine teise Azure AD.

Kohandatud domeeninime kustutamiseks tuleb kõigepealt veenduda, et ressursse kataloogis toetuvad domeeni nimi. Domeeninime ei saa kustutada oma kataloogist, kui:

-   Igal kasutajal on kasutaja nimi, meiliaadress või puhverserveri aadressi, mis sisaldab domeeni nimi.

-   Mis tahes rühma on e-posti aadress või puhverserveri aadressi, mis sisaldab domeeni nimi.

-   Mis tahes taotlus oma Azure AD on rakenduse ID URI, mis sisaldab domeeni nimi.

Peab muudate või kustutate selline ressurss Azure AD kataloogis, enne kui saate kustutada kohandatud domeeni nimi.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>PowerShelli kasutamine või Graph API domeeninimede haldamine

Enamik haldustoimingute domeeninimed Azure Active Directory's saate täita ka Microsoft PowerShelli kaudu või programmiliselt kasutamine Azure AD Graph API (avaliku eelvaade).

-   [PowerShelli kasutamine Azure AD domeeninimede haldamine](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Domeeninimede haldamine Azure AD Graph API abil](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Järgmised sammud

-   [Kohandatud domeeninime lisamine](active-directory-domains-add-azure-portal.md)
