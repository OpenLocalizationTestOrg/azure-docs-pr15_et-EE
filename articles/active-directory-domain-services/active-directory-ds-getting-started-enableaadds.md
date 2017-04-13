<properties
    pageTitle="Azure'i AD domeeniteenused: Lubada Azure AD domeeniteenused | Microsoft Azure'i"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Azure'i AD domeeniteenused lubamine

## <a name="task-3-enable-azure-ad-domain-services"></a>Ülesanne 3: Luba Azure AD domeeniteenused
Selles ülesandes lubate Azure AD domeeni teenuste kataloogi. Järgmiste toimingute konfiguratsiooni lubamiseks Azure AD domeeni teenuste kataloogi.

1. Liikuge **Azure klassikaline portaali** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Valige vasakpoolsel paanil sõlme **Active Directory** .

3. Valige Azure AD rentniku (directory), mille jaoks soovite lubada Azure AD domeeniteenused.

    ![Valige Azure AD kataloog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klõpsake vahekaarti **konfigureerimine** .

    ![Vahekaardil directory konfigureerimine](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Liikuge kerides jaotiseni, nimega **domeeniteenused**.

    ![Jaotises Domain Services konfigureerimine](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Tavalise suvandi nimega **domeeniteenused selle kausta jaoks lubada** **Jah**. Märkate, et mõned konfiguratsiooni lisavõimaluste Azure AD domeeniteenused kuvatakse lehe.

    ![Domeeni teenuste lubamine](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Kui lubate Azure AD domeeniteenused teie rentniku, Azure AD genereeritud ning see talletab Kerberose ja NTLM mandaati hashes vajalike kasutajate autentimiseks.

7. Määrake **domeeni teenuste DNS-i domeeni nimi**.

   - Kataloogi vaikedomeeninime (st, mis lõpeb soovitud **. onmicrosoft.com** domeeni järelliide) on vaikimisi valitud.

   - Loend sisaldab kõiki domeenid, mis on konfigureeritud Azure AD kataloogi – jaoks, sh kinnitatud kui kinnitamata domeene, mida saate konfigureerida vahekaarti "Domains".

   - Lisaks võite sisestada ka kohandatud domeeni nimi. Selles näites me tippinud kohandatud domeeninime "contoso100.com".

     > [AZURE.WARNING] Veenduge, et domeeni nimi (nt "contoso100", "contoso100.com" domeeni nimi) määratud domeeni eesliide on vähem kui 15 märki. Ei saa luua mõne Azure AD domeeniteenuste domeeni domeeni eesliitega pikem kui 15 märki.

8. Veenduge, et teie valitud hallatavate domeeni DNS-i domeeni nimi pole juba olemas virtuaalse võrgu. Täpsemalt, märkige ruut, kui:

   - teil on juba sama nimega DNS-i domeeni virtuaalse võrgus domeeni.

   - valitud virtuaalse võrgu on kohapealse võrgu VPN-ühendus ja teil on kohapealse võrgu domeeni DNS-i domeeni nimi.

   - teil on mõne olemasoleva pilveteenuses nimega virtuaalse võrgus.

9. Järgmiseks on virtuaalse võrku, kus soovite Azure AD domeeniteenused saadaval valida. Valige virtuaalse võrgu- ja sihtotstarbeline alamvõrgu loodud pealkirjaga **selle virtuaalse võrguga ühenduse loomine domeeniteenused**rippmenüüst.

   - Veenduge, et olete määranud virtuaalse võrgu kuulub Azure AD domeeniteenused ei toeta Azure alale. [Azure'i teenuste regiooniti](https://azure.microsoft.com/regions/#services/) lehe leida Azure regioonid, mis on saadaval Azure AD domeeniteenused viidata.

   - Virtuaalne võrkude kuuluvad alale, kuhu Azure AD domeeniteenused ei toetata ei kuvata ripploendis loendis.
   
   - Kasutage sihtotstarbeline alamvõrgu virtuaalse võrgustikus Azure AD domeeni teenuste jaoks. Veenduge, et valite lüüsi alamvõrgu. Lugege teemat [võrgu peaksite arvesse võtma](active-directory-ds-networking.md). 

   - Samuti virtuaalse võrke, mis on loodud, kasutades Azure ressursihaldur rippmenüü loendis ei kuvata. Azure'i AD domeeniteenused ressursihaldur vastavalt virtuaalse võrgu praegu ei toeta.

10. Azure AD domeeniteenused lubamiseks klõpsake tööpaani lehe allosas nuppu **Salvesta** .

11. Lehel kuvatakse soovitud ' pooleli... " riik, kui Azure AD domeeniteenused on kataloogi jaoks on lubatud.

    ![Domeeni teenuste - ootel olekus lubamine](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure'i AD domeeniteenused pakub kõrge kättesaadavus hallatava domeeni jaoks. Kui olete lubanud Azure AD domeeniteenused, Pange tähele IP-aadressid, kus on saadaval virtuaalse võrgu kuva üles ükshaaval domeeniteenused. Teine IP-aadress kuvatakse varsti, varsti teenus võimaldab kõrge kättesaadavus domeeni jaoks. Kõrge kättesaadavus on konfigureeritud ja aktiivne oma domeeni, peaksite nägema **domeeniteenused** jaotises **konfigureerimine** menüü kaks IP-aadressid.

12. Pärast umbes 20 – 30 minutit, näete esimese IP-aadress mille domeeniteenused on saadaval virtuaalse võrgu **IP-aadress** väljale lehe **konfigureerimine** .

    ![Lubatud – domeeniteenused esimese IP ette valmistatud](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Kui kõrge-saadavus töötab teie domeeni, kuvatakse lehel kuvatud kaks IP-aadressid. Hallatava domeeni on saadaval valitud virtuaalse võrgu need kaks IP-aadressid. Pange tähele, IP-aadresside alla nii, et saate värskendada DNS-i sätted virtuaalse võrgu jaoks. See toiming võimaldab virtuaalmasinates virtuaalse võrgu ühenduse loomiseks domeeni jaoks nagu domeeni Liitu.

    ![Domeeni teenuste lubatud – nii IP-d ette valmistatud.](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Sõltuvalt teie Azure AD rentniku suurusest (arv kasutajad, pakuvad jne), hallatava domeeni sünkroonimine võtab aega. See sünkroonimine toimub taustal. Jaoks suur rentnikud kümneid tuhandete objektid, võib kuluda päev või kaks kõik kasutajad, rühmaliikmeid ja mandaadi sünkroonimist.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Tööülesande 4 - Värskenda DNS-i sätted Azure virtuaalse võrgu jaoks
Järgmise ülesande konfigureerimine on [Azure virtuaalse võrgu jaoks DNS-i sätete värskendamine](active-directory-ds-getting-started-dns.md).
