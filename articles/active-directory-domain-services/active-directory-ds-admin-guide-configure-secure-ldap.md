<properties
    pageTitle="Azure'i AD domeeni teenuste konfigureerimine turvaline LDAP (LDAPS) | Microsoft Azure'i"
    description="Azure'i AD domeeniteenused hallatava domeeni Secure LDAP (LDAPS) konfigureerimine"
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

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Azure'i AD domeeniteenused hallatava domeeni Secure LDAP (LDAPS) konfigureerimine
Selles artiklis kirjeldatakse, kuidas saate lubada Secure Lightweight Directory Access Protocol (LDAPS) Azure AD domeeniteenused hallatava domeeni jaoks. Turvaline LDAP on ka ' Lightweight Directory Access Protocol (LDAP) üle Secure Sockets Layer (SSL) / transpordikihi Turve (TLS) ".

## <a name="before-you-begin"></a>Enne alustamist
Selles artiklis toodud ülesannete täitmiseks peate.

1. Kehtiv **Azure'i tellimus**.

2. Mõne **directory Azure'i AD** - ühte sünkroonida mõne kohapealse kataloogi või vaid kataloogist.

3. Azure AD directory **domeeniteenused Azure AD** peab olema lubatud. Kui te pole seda veel teinud, täitke ülesandeid, mis on kirjeldatud [kasutusjuhend](./active-directory-ds-getting-started.md).

4. **Serdi kasutada turvalist LDAP**.
    - **Soovitatav** - serdi hankima oma ettevõtte CA või avaliku sertimiskeskus. Selle konfiguratsiooni valik on turvalisem.
    - Vaheldumisi, võib valida ka [iseallkirjastatud serdi loomise](#task-1---obtain-a-certificate-for-secure-ldap) kohta vt käesoleva artikli.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Turvaline LDAP serdi nõuded
Hankida kehtiv kohta järgmisi juhiseid, enne kui lubate turvaline LDAP. Kui proovite lubada turvaline LDAP hallatava domeeni vigane/vale serdiga ilmneb tõrkeid.

1. **Usaldusväärne väljaandja** - sertifikaadi peab välja asutuse, mis on usaldusväärsed arvutid, mida on vaja luua ühendus turvaline LDAP abil. See asutus võib olla teie ettevõtte ettevõtte sertimiskeskus või avaliku arvutitesse usaldusväärne sertimiskeskus.

2. **Eluiga** - peab sert olema lubatud vähemalt järgmise 3 – 6 kuud. Turvaline juurdepääs LDAP hallatava domeeni katkestati serdi aegumisel.

3. **Teema nimes** - serdi teema nimi peab olema metamärkide hallatava domeeni jaoks. Näiteks kui teie domeeni nimi "contoso100.com", on serdi subjekti nimi peab olema "*. contoso100.com". DNS-i nimi (teema alternatiivne nimi) selle metamärkide nime määramine

3. **Võtme kasutamine** – sert peab olema konfigureeritud järgmist kasutab - digitaalallkirjad ja võtme šifreerimine.

4. **Serdi otstarve** - peab sert olema lubatud SSL-i serveri autentimine.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Ülesanne 1 – turvaline LDAP saamiseks serdi
Esimese tööülesande hõlmab saamiseks serdi turvaline juurdepääs LDAP hallatava domeeni kasutada. Teil on kaks võimalust.

- Serdi hankima sertimiskeskus. Asutuse võib teie ettevõtte enterprise CA või avaliku sertimiskeskus.

- Iseallkirjastatud serdi loomine.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Võimalus (soovitatav) – turvaline LDAP serdi hankima sertimiskeskus
Kui teie asutus kasutab ettevõtte avaliku võtme infrastruktuur (PKI), peate serdi hankima ettevõtte ettevõtte sertimiskeskus (CA). Kui teie asutus saab oma serdid avaliku sertimiskeskuselt, peate selle avaliku sertimisorgan turvaline LDAP serdi hangib.

Serdi taotlemisel veenduge, et järgite [nõue turvaline LDAP serti](#requirements-for-the-secure-ldap-certificate)kirjeldatud nõuetele.

> [AZURE.NOTE] Peate ühenduse hallatava domeeni kasutamine Lightweight turvaline klientarvutid peab usalda LDAPS serdi väljaandja.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Võimalus B – turvaline LDAP iseallkirjastatud serdi loomine
Võite valida iseallkirjastatud serdi loomine turvaline LDAP, kui:

- ettevõtte serdid ei ole välja andnud ettevõtte sertide või
- teie oodatav kasutada avaliku sertimiskeskuselt sert.

**PowerShelli kasutamine iseallkirjastatud serdi loomine**

Windowsi arvutisse, avage uus PowerShelli aken **administraatorina** ja tippige järgmised käsud, uue iseallkirjastatud serdi loomine.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Eelnev näide, asendage "contoso100.com" Azure AD domeeniteenused hallatava domeeni DNS-i domeeni nimi.

![Valige Azure AD kataloog](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Äsja loodud iseallkirjastatud serdi paigutatakse kohaliku arvuti serdi poest.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Ülesanne 2: ekspordi turvaline LDAP sertifikaadi lisamine. PFX-fail
Enne selle toimingu alustamist veenduge, et teil on saanud turvaline LDAP oma ettevõtte sertimiskeskus või avaliku sertimisorgan või pole loonud iseallkirjastatud sert.

Tehke järgmist, eksportida LDAPS serdi lisamine. PFX-fail.

1. Klõpsake nuppu **Start** ja tippige **R**. Dialoogiboksis **Käivita** , tippige **mmc** ja klõpsake nuppu **OK**.

    ![Käivitage MMC konsool](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Kui küsitakse **Kasutajakonto kontroll** , klõpsake nuppu **Jah** käivitamiseks administraatorina MMC (Microsoft Management Console).

3. Klõpsake menüü **fail** käsku **lisamine või eemaldamine Snap-in...**.

    ![Lisandmooduli lisamine MMC konsooli](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. Valige dialoogiboksis **Lisa või Eemalda lisandmoodulid** lisandmooduli **serdid** ja klõpsake soovitud **Lisa >** nupp.

    ![Serdid lisandmooduli lisamine MMC konsooli](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. **Lisandmooduli serdid** viisardi, valige **arvuti konto** ja klõpsake nuppu **edasi**.

    ![Lisandmooduli serdid arvuti konto lisamine](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Valige lehel **Valige arvuti** **kohalikus arvutis: (arvuti see konsool töötab)** ja klõpsake nuppu **valmis**.

    ![Lisandmooduli serdid – valige arvuti lisamine](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. **Lisage või eemaldage lisandmoodulid** dialoogiboksis nuppu **OK** , et serdid lisandmooduli lisamine MMC-s.

    ![Serdid lisandmooduli lisamine MMC - valmis](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. **Konsooli juur**laiendamiseks klõpsake MMC aknas. Peaksite nägema serdid lisandmooduli laaditud. Klõpsake nuppu **serdid (kohaliku arvuti)** laiendamiseks. Klõpsake **isikliku** laiendage järgneb sõlme **serdid** .

    ![Avage Isiklikud serdid pood](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Peaksite nägema lõime iseallkirjastatud sert. Saate uurida atribuutide serdi tagamiseks on sõrmejälje vastab mis teatatud Windows PowerShelli serdi loomisel.

10. Valige iseallkirjastatud serti ja **paremklõpsake**. Paremklõpsake käsku **Kõik toimingud** ja valige **ekspordi …**.

    ![Serdi eksportimine](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. **Serdi ekspordiviisardi**, klõpsake nuppu **edasi**.

    ![Serdi ekspordiviisardi](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Klõpsake lehel **Ekspordi privaatvõti** valige **Jah, ekspordi privaatvõti**ja klõpsake nuppu **edasi**.

    ![Serdi ekspordi privaatvõti](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] Privaatvõti koos serdi tuleb eksportida. Kui annate PFX, mis ei sisalda privaatvõti serti, lubada turvaline LDAP hallatava domeeni nurjub.

13. Valige lehel **Ekspordi failivorming** **isikuandmete vahetus – PKCS #12 (. PFX)** failivormingus eksporditud serti.

    ![Ekspordi serdi failivorming](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Ainult selle. PFX failivorming on toetatud. Ekspordi sertifikaat on. CER-i failivorming.

14. Valige **lehel,** parooliga kaitsmiseks **parooli** suvand ja tippige soovitud. PFX-fail. Pidage meeles seda parooli, kuna see on vajalik järgmise ülesande. Klõpsake nuppu **edasi** jätkamiseks.

    ![Parooli serdi eksportimine ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Märkige see parool. Peate võimaldades turvalist LDAP selle hallatava domeeni [ülesanne](#task-3---enable-secure-ldap-for-the-managed-domain) 3 – turvaline LDAP hallatava domeeni lubamine

15. Määrake lehel **faili eksportimine** faili nimi ja asukoht, kuhu soovite eksportida sert.

    ![Serdi eksportimine tee](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Järgmisel lehel nuppu **valmis** serdiga PFX-faili eksportida. Kuvatakse kinnituse dialoogiboks eksportimisel sert.

    ![Eksportige valmis sert](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Ülesanne 3 – luba turvaline LDAP hallatava domeeni
Turvaline LDAP lubamiseks tehke järgmist konfiguratsiooni:

1. Liikuge **[Azure klassikaline portaalis](https://manage.windowsazure.com)**.

2. Valige vasakpoolsel paanil sõlme **Active Directory** .

3. Valige Azure AD kataloogi (nimetatakse ka "rentniku"), mille jaoks teil on lubatud Azure AD domeeniteenused.

    ![Valige Azure AD kataloog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klõpsake vahekaarti **konfigureerimine** .

    ![Vahekaardil directory konfigureerimine](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Liikuge kerides jaotiseni **domeeniteenused**pealkirjaga. Peaksite nägema suvandi nimega **Secure LDAP (LDAPS)** , nagu on näidatud järgmine pilt:

    ![Jaotises Domain Services konfigureerimine](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Avab dialoogiboksi **Secure LDAP serdi konfigureerimine** nuppu **Konfigureeri... sert** .

    ![Turvaline LDAP serdi konfigureerimine](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Klõpsake kaustaikooni all **PFX faili SERDIGA** määramiseks PFX fail, mis sisaldab serti soovite turvaline juurdepääs LDAP hallatava domeeni jaoks kasutada. Serdi eksportimisel PFX-fail määratud parool sisestada. Klõpsake allosas nuppu valmis.

    ![Määrake turvaline LDAP PFX-fail ja parool](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. Jaotise **domeeni teenuste** **konfigureerimine** menüü peaks saada Hall ja on selle **ootel...** maakond paar minutit. Selle aja jooksul LDAPS sert on kinnitatud täpsuse ja turvalise LDAP on konfigureeritud lubama hallatava domeeni.

    ![Turvaline LDAP - ootele](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Kulub lubada turvaline LDAP hallatava domeeni umbes 10 – 15 minutit. Kui esitatud turvaline LDAP serti ei vasta nõutud kriteeriumid, turvalist LDAP kataloogi pole lubatud ja kuvatakse tõrge. Näiteks domeeninimi on vale, sert on aegunud või aegub varsti jne.

9. Turvaline LDAP on edukalt hallatava domeeni puhul lubatud, on **pooleli...** sõnumi kaduma. Peaksite nägema kuvatakse serdi sõrmejälje.

    ![Turvaline LDAP lubatud](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Tööülesande 4 - luba turvaline LDAP juurdepääs Interneti kaudu
**Valikuline tööülesande** – kui te ei kavatse hallatava domeeni LDAPS Interneti kaudu juurde, jätke selle ülesande konfigureerimine.

Enne selle toimingu alustamist veenduge, et olete teinud [tööülesande 3](#task-3---enable-secure-ldap-for-the-managed-domain)kirjeldatud juhised.

1. Peaksite nägema suvand **Luba SECURE LDAP juurdepääsu Interneti üle** **domeeniteenused** jaotises lehe **konfigureerimine** . See suvand on määratud **ei** vaikimisi kuna üle turvaline LDAP hallatava domeeni Interneti-ühendus on vaikimisi välja lülitatud.

    ![Turvaline LDAP lubatud](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Lülita **Interneti kaudu turvaline LDAP juurdepääsu lubamine** **Jah**. Klõpsake nuppu **Salvesta** Alumine paneel.
    ![Turvaline LDAP lubatud](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. Jaotise **domeeni teenuste** **konfigureerimine** menüü peaks saada Hall ja on selle **ootel...** maakond paar minutit. Mõne aja pärast üle turvaline LDAP hallatava domeeni Interneti-ühendus on lubatud.

    ![Turvaline LDAP - ootele](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] 10-minutilist kulub üle turvaline LDAP hallatava domeeni Interneti juurdepääsu lubamine.

4. Kui turvaline juurdepääs LDAP hallatava domeeni Interneti kaudu on edukalt lubatud, on **ootel...** sõnumi kaduma. Peaksite nägema saab kasutada juurdepääsu kataloogi üle LDAPS väljale **IP ADDRESS jaoks LDAPS VÄLISJUURDEPÄÄSU**välise IP-aadress.

    ![Turvaline LDAP lubatud](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Tööülesande 5 – DNS-i hallatava domeeni Interneti kaudu juurdepääsu konfigureerimine
**Valikuline tööülesande** – kui te ei kavatse hallatava domeeni LDAPS Interneti kaudu juurde, jätke selle ülesande konfigureerimine.

Enne selle toimingu alustamist veenduge, et olete teinud [tööülesande 4](#task-4---enable-secure-ldap-access-over-the-internet)kirjeldatud juhised.

Kui hallatava domeeni jaoks on lubatud Interneti kaudu LDAP turvaline juurdepääs, peate värskendama DNS-i, et klientarvutite leiate selle hallatava domeeni. Tööülesande 4 lõpus kuvatakse **välise IP ADDRESS jaoks LDAPS**Accessi vahekaardil **konfigureerimine** välise IP-aadress.

Konfigureerimine välise DNS-i pakkuja nii, et hallatava domeeni (nt contoso100.com) DNS-i nimi, mis osutab selle välise IP-aadress. Selles näites on vaja järgmisi DNS-i kirje loomine:

    contoso100.com  -> 52.165.38.113

See on õige – nüüd olete valmis ühenduse hallatava domeeni turvaline LDAP Interneti kaudu.

> [AZURE.WARNING] Pidage meeles, et klientarvutite peab usaldada LDAPS serdi väljaandja saama edukalt ühenduse hallatava domeeni LDAPS abil. Kui kasutate ettevõtte sertide või avalikult usaldusväärne sertimiskeskus, pole vaja midagi teha, kuna klientarvutite usaldada nende sert väljaandjad. Kui kasutate iseallkirjastatud serdi, peate installima avaliku osa iseallkirjastatud sert usaldusväärsete serdi salvestuskohta klientarvutis.

<br>

## <a name="related-content"></a>Seotud teemad

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](active-directory-ds-admin-guide-administer-domain.md)
