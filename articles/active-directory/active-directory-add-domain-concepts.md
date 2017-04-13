<properties
    pageTitle="Kohandatud domeeni nime Azure Active Directory kontseptuaalne ülevaade | Microsoft Azure'i"
    description="Selgitab kontseptuaalne raamistik Azure Active directory, sh federation ühekordse sisselogimise jaoks kohandatud domeeninime kasutamine"
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
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Kohandatud domeeni nime Azure Active Directory kontseptuaalne ülevaade

Domeeninimi on oluline osa tunnuse palju directory ressursse: see on osa kasutaja nimi või meiliaadress kasutaja, rühma, aadressi ja saab rakenduse ID URI rakenduse osa. Ressursi Azure Active Directory (Azure AD) saate lisada domeeni nimi, mis on juba kinnitatud kuuluma ressursi sisaldava kausta. Ainult globaalne administraator saab teha domeenihalduse tööülesandeid Azure AD.

Domeeninimed Azure AD on globaalselt kordumatu. Domeeninime saate kasutada ühe Azure AD. Kui üks Azure AD kataloog on domeeninimi, saate muude Azure AD directory kinnitamine või kasutada sama domeeninime.

## <a name="initial-and-custom-domain-names"></a>Algne ja kohandatud domeeni nimed

Iga domeeni nimi Azure AD on algne domeeninimi või kohandatud domeeni nimi.

Iga Azure AD kaasas algset domeeninime contoso.onmicrosoft.com vorm. Selle kolmanda taseme domeeninimi, selles näites "contoso.onmicrosoft.com," ei tuvastatud kataloogi on loodud, tavaliselt kataloogis loonud administraator. Algne domeeninimi kausta ei saa muuta ega kustutada. Algne domeeninimi ajal täielikult toimiv, on mõeldud eelkõige bootstrapping süsteem seni, kuni on kinnitatud kohandatud domeeninime kasutada.

Enamik Tootmiskeskkondades kataloog on vähemalt üks kinnitatud kohandatud domeen, nt "contoso.com," ja see on selle domeeni, mis on nähtav lõppkasutajatele. Kohandatud domeeninime on domeeninimi, mis on omanik ja kasutada seda asutust, nt "contoso.com," kasutab näiteks majutada oma veebisaiti. Selle domeeni nimi on töötajate tuttav, kuna see on kasutaja nimi, mida nad kasutavad ettevõtte võrku sisse logida või saatmist ja vastuvõtmist e-posti osa.

Enne seda saab kasutada Azure AD, kohandatud domeeni nimi peab olema lisatud kataloogi ja kinnitatud.

## <a name="verified-and-unverified-domain-names"></a>Kinnitatud ja kinnitamata domeeninimed

Kausta algne domeeninimi hinnatakse peidetult, nagu on kinnitatud Azure AD. Kui administraator lisab kohandatud domeeninime Azure AD, on esialgu kinnitamata olekus. Azure AD ei võimalda mis tahes directory ressursse, mis on kinnitamata domeeninime kasutada. See tagab, et ainult üks directory abil saate eriti domeeninime, ja et organisatsiooni kasutab domeeninime tegelikult kuulub selle domeeni nimi.

Azure AD kinnitab domeeni omandiõiguse otsides konkreetse kirje domeeni nimi (DNS) teenuse tsoonifaili domeeni nimi. Domeeni omandiõiguse kinnitamiseks administraator saab DNS-i kirje Azure AD, et Azure AD otsib ja lisab selle kirje DNS-i tsooni faili domeeni nime. Domeeni domeeninimeregistraatori haldab DNS-i tsooni faili. Artikli kohta, kuidas [lisada kohandatud domeeni, et kataloogi Azure AD](active-directory-add-domain.md)kuvatakse juhiseid domeeni kinnitamiseks.

DNS-i kirje lisamine domeeni nime zone file ei mõjuta näiteks e-posti või veebi hostiteenuse muude teenustega.

## <a name="federated-and-managed-domain-names"></a>Ühendatud ja hallatavate domeeninimed

Kohandatud domeeninime Azure AD saab konfigureerida anda kasutajatele ühendatud Logi oma kohapealse Active Directory ja Azure AD vahel. Värskendused õigustega ressursid Azure AD ja oma Windows Server Active Directory domeeni ühinemise jaoks konfigureerida nõuab. Ühendatud domeenis tuleb täita: Azure'i AD-ühenduse konfigureerimise või PowerShelli kaudu. Ühendamine kohandatud domeeni ei saa algatatud Azure klassikaline portaalist. [Sellest videost saate lisateavet AD FS-i kasutaja sisselogimine Azure'i AD-ühenduse konfigureerimise kohta](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domeenid, mis on ühendatud, on mõnikord nimetatakse hallatava domeenid. Algseks domeeniks on Azure AD kataloogi hinnatakse peidetult hallatava domeeni nimega.

## <a name="primary-domain-names"></a>Põhidomeen nimed

Kausta esmane domeeninimi on domeeni nimi, mis on eelnevalt valitud vaikeväärtus "Domeen" osa kasutajanimi, kui administraator loob uue kasutaja [Azure klassikaline portaali](https://manage.windowsazure.com/) või mõne muu portaali, nt Office 365 halduskeskuse kaudu. Kaust võib olla ainult üks esmane domeeninimi. Administraator saab muuta esmane domeeninimi olla mis tahes kinnitatud kohandatud domeeni, mis on ühendatud, või algseks domeeniks.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domeeninimedega Azure AD ja muude Microsoft Online Services

Domeeninimi tuleb kontrollida Azure AD, enne kui saate seda kasutada mõnes muus Microsoft Online'i teenuses Exchange Online, SharePoint Online ja Intune. Tavaliselt nõuavad nende teenuste lisada ühe või mitme DNS-kirjed, mis on kindla teenuse administraator.

Azure'i web app kasutab oma oma domeeni omandiõiguse kinnitamiseks. Domeen peab kontrollima kasutamiseks koos Azure AD isegi juhul, kui see on varem kontrollitud kasutamiseks tellimus, mis sõltub selle Azure AD Azure web app. Azure'i web app saate kasutada domeeni nimi, mis on kinnitatud erinevat kataloogi kataloogist, mis tagab web appi.

## <a name="managing-domain-names"></a>Domeeninimede haldamine

Domeeni haldamise toiminguid saate täita Azure klassikaline portaalist ja PowerShelli kaudu. Paljude ülesannete lõpule viia kasutamine Azure AD Graph API (avaliku eelvaade).

-   [Lisamine ja kinnitatava domeeni nimi](active-directory-add-domain.md)

-   [Domeenihalduse Azure klassikaline portaalis](active-directory-add-manage-domain-names.md)

-   [PowerShelli kasutamine Azure AD domeeninimede haldamine](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Azure'i AD Graph API abil Azure AD domeeninimede haldamine](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
