<properties
    pageTitle="Azure Active Directory domeeniteenused: Liituda RHEL VM hallatava domeeni | Microsoft Azure'i"
    description="Azure'i AD domeeniteenused punane rolli Enterprise Linux virtuaalse masina liituda"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Punane rolli Enterprise Linux 7 virtuaalse masina hallatava domeeni liitumine
Selles artiklis kirjeldatakse, kuidas liituda Red rolli Enterprise Linux (RHEL) 7 virtuaalse masina Azure AD domeeniteenused hallatava domeeni.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Punane rolli Enterprise Linux virtuaalse masina ettevalmistamine
Järgmiste toimingute ettevalmistamise RHEL 7 virtuaalse masina Azure'i portaalis.

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

    ![Azure'i portaali armatuurlaud](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Klõpsake vasakpoolsel paanil nuppu **Uus** ja **Punane rolli** tipite otsinguriba, nagu on näidatud järgmises kuvatõmmis. Otsingutulemites kuvatakse kirjed punane rolli Enterprise Linuxi jaoks. Klõpsake nuppu **punane rolli Enterprise Linux 7.2**.

    ![Valige RHEL tulemuste](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Otsingutulemite paanil **Kõik** loetletakse punane rolli Enterprise Linux 7.2 pilt. **Punane rolli Enterprise Linux 7.2** virtual arvuti pildi kohta lisateabe vaatamiseks klõpsake.

    ![Valige RHEL tulemuste](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. **Punane rolli Enterprise Linux 7.2** paanil, peaksite nägema virtual arvuti pildi kohta lisateavet. **Valige juurutamise mudeli** rippmenüü, valige **klassikaline**. Klõpsake nuppu **Loo** .

    ![Pildi üksikasjade kuvamine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Sisestage paanil **VM luua** uue virtuaalse masina **Hosti nimi** . Määrata ka kohaliku administraatori kasutajanimi **kasutajanimi** ja **parool**. Samuti võite kasutada ka SSH võti kohaliku administraatori autentida. Valida ka **Hinnad taseme** virtuaalse masina jaoks.

    ![Looge VM - põhilised üksikasjad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Klõpsake nuppu **Valikuline konfigureerimine**. Klõpsake paanil **Valikuline config** **võrku**.

    ![Luua VM - virtuaalse võrgu konfigureerimine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Kuvatakse tabeliveergude pealkirjaga **võrgu**paani. Klõpsake paanil **võrgu** **Virtuaalse võrgu** virtuaalse võrgu, millele tuleks Linuxi juurutatud. Avatakse paan **Virtuaalse võrgu** . Klõpsake paanil **Virtuaalse võrgu** valige suvand **Kasuta olemasolevat virtuaalse võrku** . Valige virtuaalse võrgu Azure AD domeeniteenused on saadaval. Selles näites me valige "MyPreviewVNet" virtuaalse võrgu.

    ![Luua VM - virtuaalse võrgu valimine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. **Valikuline config** paanil, klõpsake nuppu **OK** .

    ![VM - valitud virtuaalse võrgu loomine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Nüüd olete valmis looma virtuaalse masina. Klõpsake paani **VM loomine** nuppu **Loo** .

    ![VM - valmis loomine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Juurutamise uue virtuaalse masina põhjal RHEL 7.2 pilt peab algama.

  ![Looge VM - juurutamise alustamine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Pärast paar minutit, virtuaalse masina tuleb kasutada edukalt ja valmis kasutamiseks.

  ![VM - juurutatud loomine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Äsja ettevalmistatud Linuxi virtuaalse masina kaugühenduse teel ühenduse
Azure on ette valmistatud RHEL 7.2 virtuaalse masina. Järgmise ülesande on virtuaalse masina eemalt ühendust luua.

**Ühenduse loomine RHEL 7.2 virtuaalse masina** Järgige juhiseid teemas [Kuidas töötab Linux sisse logida](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Ülejäänud juhiste täitmiseks endale PuTTY SSH kliendi abil saate ühendada RHEL virtuaalse masina. Lisateabe saamiseks vaadake [lehe PuTTY alla laadida](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Avage kitt programm.

2. Sisestage vastloodud RHEL virtuaalse masina **Hosti nimi** . Selles näites on meie virtuaalse masina hosti nimi "contoso-rhel.cloudapp .net". Kui te pole kindel, et teie VM hostinimi, vaadake VM armatuurlaua Azure'i portaalis.

    ![Kitt ühenduse loomine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Logige sisse virtuaalse masina teie määratud virtuaalse masina loomise kohaliku administraatori identimisteave. Selles näites kasutatakse kohaliku administraatorikonto "mahesh".

    ![Kitt sisselogimine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Nõutav paketid Linux virtual arvutisse installida
Pärast ühenduse, virtuaalse masina, on järgmise ülesande pakettide nõutav domeeni ühenduse, virtuaalse arvutisse installida. Tehke järgmist.

1. **Installida realmd:** Realmd paketi kasutatakse domeeni Liitu. Teie kitt terminal, tippige järgmine käsk:

    sudo yum installi realmd

    ![Installige realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Mõne minuti pärast peaks realmd paketi virtual arvutisse installitud.

    ![realmd installitud](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Installida sssd:** Realmd paketi sõltub sssd domeeni Liitu toimingute tegemiseks. Teie kitt terminal, tippige järgmine käsk:

    sudo yum installi sssd

    ![Installige sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Mõne minuti pärast peaks sssd paketi virtual arvutisse installitud.

    ![realmd installitud](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Installida kerberos:** Teie kitt terminal, tippige järgmine käsk:

    sudo yum installida krb5-töökoha krb5-kena tegevust jälle

    ![Kerberose installimine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Mõne minuti pärast peaks realmd paketi virtual arvutisse installitud.

    ![Kerberose installitud](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Liitumine Linux virtuaalse masina hallatava domeeni
Nüüd, kui nõutav paketid on Linux virtual arvutisse installitud, on järgmise ülesande liituda virtuaalse masina hallatava domeeni.

1. Vaadake AAD domeeniteenused hallatava domeeni. Teie kitt terminal, tippige järgmine käsk:

    sudo Domeen avastamine CONTOSO100.COM

    ![Domeen avastamine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Kui **Domeen avastamine** ei leia hallatava domeeni, veenduge, et domeen on kättesaadav virtual arvutist (proovige ping). Samuti veenduge, et virtuaalse masina tõepoolest kasutusele võetud sama virtuaalse võrgu hallatavate domeen on saadaval.

2. Kerberose lähtestada. Teie kitt terminal, tippige järgmine käsk. Veenduge, et kasutaja, kes kuulub 'AAD näiteks Põhiliselt administraatorite' rühmale määratud. Ainult need kasutajad saavad liituda arvutite hallatava domeeni.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Tagama, et teie määratud domeeninime suurtähtedega veel kinit nurjub.

3. Liituda domeeniks masinat. Teie kitt terminal, tippige järgmine käsk. Määrake sama kasutaja määratud eelmises etapis (kinit").

    sudo Domeen Liitu--Paljusõnaline CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Domeen liitumine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Saate sõnumile ("edukalt sisselogimise seadme Domeen") Kui seade on ühendatud edukalt hallatava domeeni.


## <a name="verify-domain-join"></a>Veenduge, et domeeni liitumine
Saate kiiresti kontrollida, kas seade on edukalt ühinenud hallatava domeeni. Ühenduse loomine selle äsja domeeni RHEL VM SSH ja domeeni kasutajakonto ja märkige seejärel abil vaadata, kui kasutaja on lahendatud õigesti ühendatud.

1. Teie kitt terminalis, tippige järgmine käsk ühenduse loomiseks soovitud äsja domeeni ühendatud RHEL virtuaalse masina SSH. Domeeni kontoga, mis kuulub hallatava domeeni (nt 'bob@CONTOSO100.COM' sel juhul.)

    SSH -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net

2. Teie kitt terminalis, tippige järgmine käsk kuvamiseks, kui home directory lähtestatud õigesti.

    PWD

3. Teie kitt terminalis, tippige järgmine käsk kuvamiseks, kui selle rühma liikmestaatusi lahendatakse õigesti.

    ID

Valimi väljundi neid käske järgmiselt:

![Veenduge, et domeeni liitumine](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Domeeni ühenduse tõrkeotsing
Lugege artiklit [tõrkeotsing domeeni Liitu](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Seotud teemad
- [Domeeniteenused Azure'i AD - kasutusjuhend](./active-directory-ds-getting-started.md)

- [Liitumine virtuaalse masina Windows Server Azure'i AD domeeniteenused hallatava domeeni](active-directory-ds-admin-guide-join-windows-vm.md)

- [Kuidas töötab Linux sisse logida](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Kerberose installimine](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Punane rolli Enterprise Linux 7 - Windows integreerimise juhend](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
