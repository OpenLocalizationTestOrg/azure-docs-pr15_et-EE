<properties
    pageTitle="Azure Active Directory domeeniteenused: Administreerimiskeskuse juhend | Microsoft Azure'i"
    description="Azure AD domeeniteenused hallatavate domeenide ettevõtte üksus (OU) loomine"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Looge organisatsiooni üksus (OU) on Azure AD domeeniteenused hallatava domeeni
Azure'i AD domeeniteenused hallatavate domeenide kaasata kaks sisseehitatud ümbriste, mida nimetatakse "AADDC arvutid" ja "AADDC kasutajad". Container "AADDC arvutid" on arvuti objektide kõik arvutid, mis on ühendatud hallatava domeeni. Container 'AADDC kasutajad' sisaldab Azure AD rentniku kasutajad ja rühmad. Aeg-ajalt, võib olla vaja luua kontod hallatavate domeenis juurutada töökoormus. Selleks saate luua kohandatud ettevõtte üksus (OU) hallatava domeeni ja luua selle OU sees. Selles artiklis kirjeldatakse, kuidas luua Organisatsiooniüksusega hallatava domeeni.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Domeeni ühendatud virtuaalse masina kaughalduse AD haldustööriistade installimine
Azure'i AD domeeniteenused hallatavate domeenide saab hallata kaugkustutada tuttavad Active Directory Haldusriistad, nt Active Directory haldus Center (ADAC) või AD PowerShelli. Rentniku administraatorite domeenikontrollerid hallatava domeeni kaudu kaugtöölaua ühenduse õigusi pole. Hallatava domeeni manustamiseks AD halduse tööriistad funktsiooni installida virtuaalse masina ühendatud hallatava domeeni. Lugege artiklit pealkirjaga [on Azure AD domeeniteenused hallatavate domeeni haldamise](active-directory-ds-admin-guide-administer-domain.md) juhised.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Looge Organisatsiooniüksus mõne hallatava domeeni
Nüüd, kui on installitud AD Haldusriistad domeeni liitunud virtuaalse masina, saame luua Organisatsiooniüksus hallatava domeeni kasutada nende tööriistade. Tehke järgmist.

> [AZURE.NOTE] Ainult 'AAD näiteks Põhiliselt administraatorid' rühma liikmetel on nõutavad õigused luua kohandatud OU. Veenduge, et kasutaja, kes selle rühma kuulub tehke järgmist.

1. Klõpsake avakuval **Haldustööriistad**. Peaksite nägema virtual arvutisse installitud AD Haldustööriistad.

    ![Serverisse installitud Haldusriistad](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klõpsake nuppu **Active Directory halduskeskus**.

    ![Active Directory halduskeskus](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Domeeni vaatamiseks klõpsake domeeni nimi (nt contoso100.com) vasakul paanil.

    ![ADAC - vaade domeeni](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. Klõpsake paanil **toimingud** paremal pool sõlme domeeni nime all nuppu **Uus** . Selles näites me nuppu **Uus** paremal pool **tööülesannete** paanil sõlme 'contoso100(local)' all.

    ![ADAC - uue OU](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Peaksite nägema võimalus luua mõne Organisatsiooniüksus. Klõpsake nuppu **Organisatsiooniüksus** **Loomine Organisatsiooniüksus** dialoogiboksi käivitada.

6. Määrake dialoogiboksis **Loomine Organisatsiooniüksus** uue OU **nimi** . Sisestage OU lühikirjeldus. Lisaks saate määrata välja **Hallatavate** OU. Luua kohandatud OU, klõpsake nuppu **OK**.

    ![ADAC – OU dialoogiboksi loomine](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. Äsja loodud OU peaks nüüd kuvatama AD haldus Center (ADAC).

    ![ADAC – OU, mis on loodud](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Õiguste ja turbe vastloodud organisatsiooniüksused
Vaikimisi on loonud kohandatud OU kasutaja ('AAD näiteks Põhiliselt administraatorid' rühma liige) administraatoriõigusi (Täielik kasutusõigus) OU üle anda. Kasutaja saab minna ning õigusi anda teistele kasutajatele või 'AAD näiteks Põhiliselt administraatorite' rühma vastavalt soovile. Nagu näha järgmine pilt kasutaja 'bob@domainservicespreview.onmicrosoft.com' koostajale 'MyCustomOU' Organisatsiooniüksus antakse täielik kontroll.

 ![ADAC - uue OU turvalisus](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Märkmete kohandatud organisatsiooniüksused haldamise kohta
Nüüd, kui olete loonud kohandatud OU, saate jätkata ja selle OU kasutajad, rühmad, arvutite ja teenuse kontode loomine. Te ei saa teisaldada kasutajate või rühmade 'AAD näiteks Põhiliselt kasutajad' OU kohandatud organisatsiooniüksused.

> [AZURE.WARNING] Kasutajakontode, rühmad, kontod ja arvuti objekte, mis teil luua kohandatud organisatsiooniüksused jaotises ei ole saadaval oma Azure AD rentniku. Teisisõnu, neid objekte ei kuvata Azure AD Graph API abil või Azure AD UI. Need objektid on saadaval ainult Azure AD domeeniteenused hallatava domeeni.


## <a name="related-content"></a>Seotud teemad

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory halduskeskus: Alustamine](https://technet.microsoft.com/library/dd560651.aspx)

- [Teenuse kontod üksikasjalik juhend](https://technet.microsoft.com/library/dd548356.aspx)
