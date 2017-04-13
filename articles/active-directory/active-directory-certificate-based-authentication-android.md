<properties 
    pageTitle="Alustamine vastavalt serdi autentimist Android | Microsoft Azure'i" 
    description="Saate teada, kuidas Androidi seadmete Solutions serti autentimise konfigureerimine" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>Alustamine vastavalt serdi autentimist Android - avaliku eelvaade  

> [AZURE.SELECTOR]
- [iOS-i](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


Selles teemas näidatakse, kuidas kasutada Office 365 Enterprise, Business ja Educationi lepingutes rentnikud serti autentimine (tulude) kasutajate jaoks Androidi seadmes ja konfigureerimiseks. 

TULUDE võimaldab teil autentida Azure Active Directory kliendi sertifikaadiga Android- või iOS-seadmes oma Exchange Online'i konto ühenduse loomisel. 

- Office Mobile'i rakendusi nagu Microsoft Outlooki ja Microsoft Word   
- Exchange ActiveSynci (EAS) kliendid 

Selle funktsiooni konfigureerimise kohta leiate kõrvaldab tuleb sisestada kasutajanime ja parooli kombinatsiooni teatud e-posti ja Microsoft Office'i rakenduste mobiilsideseadmes. 
 

## <a name="supported-scenarios-and-requirements"></a>Toetatud stsenaariumid ja nõuded  



### <a name="general-requirements"></a>Üldised nõuded 


Selles teemas kõik stsenaariumid, on vaja järgmisi toiminguid.  

- Juurdepääs serdi ametiasutus(ed) väljaandmisel kliendi serdid.  

- Sertide ametiasutus(ed) tuleb konfigureerida Azure Active Directory. Siit leiate üksikasjalikud juhised selle kohta, kuidas [Alustamine](#getting-started) jaotises konfigureerimise lõpuleviimiseks.  

- Serdi juurorgan ja mis tahes vahe sertimiskeskuste peab olema konfigureeritud Azure Active Directory.  

- Iga sertimiskeskus peab olema sertide tühistusloendid (CRL), millele viidatakse vastastikuste URL-i Interneti kaudu.  

- Kliendi sert antakse välja kliendi autentimiseks.  


- Ainult Exchange ActiveSynci klientide kliendi sert peab olema kasutaja marsruuditavaid meiliaadress Exchange Online'i põhilise nime või teema alternatiivne nimi välja RFC822 nimi väärtus. Azure Active Directory kaardid RFC822 väärtus puhverserveri aadressi atribuut kataloogis.  



### <a name="office-mobile-applications-support"></a>Office Mobile'i rakenduste tugi 

| Rakendused                      | Tugi      |
| ---                       | ---          |
| Wordi / Excel ja PowerPoint | ![Märkige ruut][1]  |
| OneNote'i                   | Tulen varsti  |
| OneDrive'i                  | ![Märkige ruut][1]  |
| Outlooki                   | ![Märkige ruut][1]  |
| Yammeri                    | ![Märkige ruut][1]  |
| Skype'i ärirakenduses        | ![Märkige ruut][1]  |


### <a name="requirements"></a>Nõuded  

Seadme OS versioon peab olema Android 5.0 (Lollipop) ja uuemad. 

Liiduserveri peab olema konfigureeritud.  


Azure Active Directory kliendi sertifikaadi tühistada, ADFS-i luba peab olema järgmised nõuded:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Järjenumbri kliendi sert) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Stringi kliendi serdi väljaandja) 

Azure Active Directory lisab need nõuded värskendamise luba, kui need on saadaval ADFS-i luba (või SAML luba). Kui värskendamine luba peab olema kinnitatud, seda teavet kasutatakse tühistamise kontrollimine. 

Hea tava, peate värskendama ADFS-i tõrkelehtede juhised, kuidas kasutaja sert. 

Lisateavet leiate teemast [AD FS sisselogimine lehtede kohandamine](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Exchange ActiveSynci klientide tugi 


Teatud Exchange ActiveSynci rakendused Android 5.0 (Lollipop) või uuem versioon on toetatud. Kindlaks teha, kui e-posti rakenduses ei toeta seda funktsiooni, pöörduge oma rakenduse arendaja. 



## <a name="getting-started"></a>Alustamine 


Alustada, peate selle sertimiskeskuste konfigureerimine Azure Active Directory. Iga sertimiskeskus üleslaadimiseks järgmist: 

- Avalik osa sert, *CER* -vormingus 

- Interneti-ühendus vastastikuste URL-id, kus asuvad sertide Tühistusloendid (CRL-ID)
 

Allpool on sertimisorgan skeemi: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Üleslaadimiseks, teabe, saate kasutada Azure AD moodul Windows PowerShelli kaudu.  
Allpool on näited lisamine, eemaldamine või muutmine sertimisorgan. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Oma Azure AD rentniku serdi konfigureerimine vastavalt autentimine 

1. Alustage Windows PowerShelli administraatoriõigused. 

2. Installige Azure'i AD moodul. Peate installima versioon [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) või uuem versioon.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Ühendada oma sihtrentnikusse. 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Uue sertimiskeskus lisamine

1. Määrata erinevaid atribuudid serte ja lisada Azure Active Directory. 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Saamiseks serdi asutused: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Sertimiskeskuste loendi toomine

Saate tuua sertimiskeskuste, praegu talletatud Azure Active Directory oma rentniku jaoks: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Sertimisorgan eemaldamine

1.  Laadi alla serdi asutused: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Sertimisorgan serdi eemaldamiseks tehke järgmist. 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying serdiga 

1.  Laadi alla serdi asutused: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Sertimisorgan atribuutide muutmiseks tehke järgmist. 

        $c[0].AuthorityType=1 

3. **Sertimiskeskus**seadmiseks tehke järgmist. 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Office Mobile'i rakenduste testimine  

Serdi autentimine Office mobiilirakenduse testimiseks tehke järgmist. 

1.  Seadme test, installige Office Mobile'i rakenduste (nt OneDrive'i) Google Play poest.

2.  Veenduge, et kasutaja sert on ette valmistatud seadmesse test. 

3.  Käivitage rakendus. 

4.  Sisestage oma kasutajanimi ja seejärel valige Kasutaja sert, mida soovite kasutada. 

Mida peaks olema edukalt sisse logitud. 





## <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSynci klientrakendustes testimine

Exchange ActiveSynci vastavalt serdi autentimist kaudu juurdepääsemiseks EAS profiili, mis sisaldavad kliendi sert peab olema saadaval rakenduse. EAS profiil peab sisaldama järgmist teavet:

- Kasutaja serdi kasutatakse autentimine 

- Eas-i lõpp-punkti peab olema outlook.office365.com (kui see funktsioon on praegu toetatud ainult Exchange Online'i mitme rentniku keskkonna)

EAS-profiil on saate konfigureeritud ja kasutamise on MDM, nt Intune või paigutades serdi käsitsi EAS profiili seadme kaudu seadmele paigutada.  


### <a name="testing-eas-client-applications-on-android"></a>EAS klientrakendustes Androidi testimine 

Testimiseks serdi autentimist rakendusega Android 5.0 (Lollipop) või uuem versioon, tehke järgmist.  

1. Konfigureerida EAS profiili rakenduses, mis vastab ülaltoodud nõuetele.  


2. Kui profiili on õigesti konfigureeritud, avage rakendus ja veenduge, et meil ei sünkrooni. 




## <a name="revocation"></a>Tühistamine

Kliendi sertifikaadi tühistada Azure Active Directory toob sertide tühistusloendid (CRL) üles laaditud serdi asutuse teabe idest ja salvestab see. Viimane avaldada CRL ajatempliga (**Kuupäev** atribuut) abil saate tagada, et CRL on kehtiv. CRL perioodiliselt viitab serdid, mis on osa loendi juurdepääsu tühistada.

Veel kiirsõnumite tühistamise on nõutav (nt kasutaja seadme katkeb), saate Kasutaja autoriseerimine luba kehtetuks. Seadmele autoriseerimine luba, määrake selle kindla kasutaja Windows PowerShelli kaudu **StsRefreshTokenValidFrom** välja. Peate värskendama iga kasutaja kohta, mida soovite tühistada juurdepääsu **StsRefreshTokenValidFrom** välja.
 
Veenduge, et tühistamise ei lahene, peate määrama CRL **Kuupäev** pärast väärtus määratud **StsRefreshTokenValidFrom** ja veenduge, et serdi kuupäevale kõnealuse on CRL.
 
Järgmised toimingud liigendamine värskendamine ja lukustage autoriseerimine luba, seades **StsRefreshTokenValidFrom** välja. 

1. Ühenda administraatori identimisteave MSOL teenusega: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Laadi alla kasutaja StsRefreshTokensValidFrom praegune väärtus: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Kasutaja on võrdne praeguse ajatempli konfigureerida uue StsRefreshTokensValidFrom väärtuse. 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Seatud kuupäev peab olema tulevikus. Kui kuupäev pole tulevikus, **StsRefreshTokensValidFrom** atribuut on seatud. Kui tulevikus on kuupäev, seatakse **StsRefreshTokensValidFrom** praeguse kellaaja (mitte tähistatud käsu Set-MsolUser kuupäev). 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png