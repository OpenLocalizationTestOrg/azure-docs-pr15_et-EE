<properties
    pageTitle="Azure'i AD domeeniteenused: Loomine AAD näiteks Põhiliselt administraatorite rühma | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused töötamise alustamine"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Azure'i AD domeeniteenused kasutamise alustamine

Selles artiklis tutvustatakse nõutav Azure AD domeeniteenused teie Azure AD rentniku konfigureerimine tööülesanded.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Ülesanne 1: 'AAD näiteks Põhiliselt administraatorite' rühma luua.
Esimese tööülesande on teie Azure Active Directory rentnikus haldusrühma loomine. See teisiti haldusrühma nimetatakse **AAD näiteks Põhiliselt administraatorid**. Selle rühma liikmetel on administraatoriõigusi, mis on Azure AD domeeniteenused hallatava domeeni domeeni ühendatud arvutites anda. Domeeni ühendatud arvutites selles rühmas on lisatud 'Administraatorite' rühma. Lisaks selle rühma liikmed saate kasutada kaugtöölaua domeeni ühendatud masinad eemalt ühendust luua.  

> [AZURE.NOTE] Teil pole Azure AD domeeniteenused abil loodud hallatavate domeenis domeeni administraatori või ettevõtte administraatori õigusi. Hallatavate domeene, need õigused on reserveeritud teenuse ja tehakse kättesaadavaks kasutajatele rentniku. Siiski saate loodud konfiguratsiooni selles ülesandes teisiti administraatorite rühma teatud õigustega toimingute tegemiseks. Need toimingud võivad olla liitumist arvutite domeeni, administraatorite rühma domeeni ühendatud arvutites konfigureerimise rühma poliitika jne.

Selles ülesandes konfiguratsiooni haldusrühma loomine ja kataloogis ühe või mitme kasutaja lisamiseks rühma. Järgmiste toimingute Azure AD domeeni teenuste haldusrühma loomiseks:

1. Liikuge **Azure klassikaline portaali** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Valige vasakpoolsel paanil sõlme **Active Directory** .

3. Valige Azure AD rentniku (directory), mille jaoks soovite lubada Azure AD domeeniteenused. Saate luua ainult ühe domeeni iga Azure AD kataloog.

    ![Valige Azure AD kataloog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klõpsake vahekaarti **rühmad** .

5. Azure AD rentniku oma rühma lisada, nuppu **Lisa rühm** tööpaanilt lehe allosas.

    ![Rühma nupp Lisa](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Nimega **AAD näiteks Põhiliselt administraatorite**rühma loomine. Määrake **jaotises Tüüp** **Turvalisus**.

    > [AZURE.WARNING] Azure'i AD domeeniteenused jooksul juurdepääsu lubamiseks õnnestus domeeni, selle täpse nimega rühma loomine.

    ![Administraator rühma loomine](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Lisage selle rühma kirjeldus, et teised aru, et selle rühma kasutatakse Azure AD domeeni teenustesse administraatoriõigusi anda.

8. Pärast rühma loomist, klõpsake selle rühma atribuutide kuvamiseks rühma nime. Selle rühma liikmed kasutajate lisamiseks nuppu **Lisa liikmeid** Alumine paneel.

    ![Rühma liikmete nupp Lisa](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. Valige dialoogiboksis **Lisa liikmeid** kasutajatele, kes peaksid olema selle rühma liikmed ja märkige ruut, kui olete valmis.

    ![Kasutajate lisamine rühma administraator](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Ülesanne 2: Looge või valige muu Azure virtuaalse kaudu
Järgmise ülesande konfigureerimine on [luua](active-directory-ds-getting-started-vnet.md)või valida muu Azure virtuaalse kaudu.
