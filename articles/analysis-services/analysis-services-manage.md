<properties
   pageTitle="Azure'i analüüsiteenuste haldamine | Microsoft Azure'i"
   description="Siit saate teada, kuidas hallata ettevõtte analüüsiteenuste serveris Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Analüüsiteenuste haldamine

Kui olete loonud mõne analüüsiteenuste serveri Azure, võib olla tuleb teha kohe või millalgi loendiseoste haldamine ja juhtimine põhitoiminguid. Näiteks käivitage töötlemise andmete värskendamine reguleerimine juurdepääs mudelite serveris või teie serveri seisundi jälgimine. Mõned haldamisega seotud toiminguid saab ainult Azure'i portaalis, teised rakenduses SQL Server Management Studio (SSMS), ja mõningaid ülesandeid saab teha ühte.

## <a name="azure-portal"></a>Azure'i portaal
[Azure portaali](http://portal.azure.com/) on, kus saate luua ja kustutada serverid, jälgida serveri ressursid, suuruse muutmine, ja hallata, kellel on juurdepääs teie serverid.  Kui teil on probleeme, saate ka esitada tugiteenuse taotluse.

![Serveri nimi Azure hankimine](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Azure'i oma serveriga on nagu ühenduse serveri eksemplar oma asutuses. SSMS, saate teha paljusid samu toiminguid nagu protsess andmete või luua skripti töötlemine, rollide haldamine ja PowerShelli kasutamine.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Üks suurem erinevus on autentimise abil saate luua ühendus serveriga. Azure'i analüüsiteenuste serveri loomiseks peate valima **Active Directory Paroolautentimine**.

### <a name="to-connect-with-ssms"></a>SSMS suhtlemiseks
1. Enne, kui ühendate, vajate serveri nime. **Azure'i** portaalis > Serveri > **Ülevaade** > **Serveri nimi**, kopeerige serveri nime.

    ![Serveri nimi Azure hankimine](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. SSMS > **Objekti Exploreri**, klõpsake nuppu **Ühenda** > **Analysis Services**.

3. Dialoogiboksis **ühenduse Server** kleepige väljale Serveri nimi ja seejärel sisse **autentimine**, valige üks järgmistest:

    **Active Directory integreeritud autentimise** Active Directory ühekordse sisselogimise Azure Active Directory federation abil.

    **Active Directory Paroolautentimine** organisatsioonikonto kasutama. Näiteks mitte domeenist ühenduse liitunud arvuti.

    Märkus: Kui te ei näe Active Directory autentimine, peate [Azure Active Directory autentimise](#enable-azure-active-directory-authentication) SSMS.

    ![Ühenduse loomine SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Hallata oma server Azure'i SSMS abil on sama palju haldamine on kohapealses serveris, me ei kavatse minna andmed siin. Kõiki aidata teil vaja leiate [Analysis Services eksemplar halduse](https://msdn.microsoft.com/library/hh230806.aspx) MSDN-is.

## <a name="server-administrators"></a>Serveri administraatorid
Saate **Analysis Services administraatorid** juhtelemendi tera oma server Azure'i portaalis või SSMS serveri administraatorite haldamine. Analysis Services administraatorid on andmebaasi serveri administraatorid, õigused levinud andmebaasi haldustoiminguid, nt lisamise ja eemaldamise andmebaasid ja kasutajate haldamine. Vaikimisi lisatakse kasutaja, mis loob server Azure'i portaalis automaatselt Analysis Services administraatorina

Lisaks peaksite teadma:

-   Windows Live ID-d ei ole toetatud identiteedi tüüp Azure'i analüüsiteenuste.  
-   Analysis Services administraatoritel peavad olema lubatud Azure Active Directory kasutajad.
-   Kui mõni Azure analüüsiteenuste serveri kaudu Azure'i ressursihaldur mallide loomise, suunab Analysis Services administraatorid JSON massiiv kasutajate, mis tuleks lisada administraatorid.

Analysis Services administraatorid võib erineda Azure ressursi administraatorid, kus saate hallata ressursid Azure'i tellimused. See säilitab ühilduvus olemasolevate XMLA ja TSML haldamine käitumist analüüsiteenuste ja teenuseid teie eraldada vahel Azure ressursside haldamine ja analüüsi andmebaasi haldamine.

Kõikide rollide kuvamine ja juurdepääs teie Azure'i analüüsiteenuste ressursi tüübid, kasutage kontrolli enne juurdepääsu reguleerimine (IAM).

## <a name="database-users"></a>Andmebaasi kasutajad
Azure'i analüüsiteenuste mudeli andmebaasi kasutajad peavad olema oma Azure Active Directory. Ettevõtte e-posti aadress või UPN-i peab olema määratud mudel andmebaasi kasutajanimed. See erineb kohapealse mudeli andmebaase, mis toetavad Windowsi domeeni kasutajanimed kasutajad.

Saate lisada kasutajaid [rollimääranguid Azure Active Directory](../active-directory/role-based-access-control-configure.md) abil või SQL Server Management Studio [Tabelina mudeli skriptimise keele](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) abil.

**Skripti näide TMSL**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Azure Active Directory autentimine
SSMS registris Azure Active Directory autentimine funktsiooni lubamiseks luua tekstifaili nimega EnableAAD.reg, ja seejärel kopeerige ja kleepige järgmine:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Salvestage ja seejärel käivitage fail.



## <a name="next-steps"></a>Järgmised sammud
Kui te pole juba juurutanud tabelmudeli uue serverisse, siis nüüd on hea aeg. Lisateavet leiate teemast [Deploy Azure'i Analysis Services](analysis-services-deploy.md).

Kui mudeli serverisse juurutamist olete valmis ühenduse kliendi või veebibrauseri abil. Lisateabe saamiseks lugege teemat [Azure analüüsiteenuste serveri andmeid](analysis-services-connect.md).
