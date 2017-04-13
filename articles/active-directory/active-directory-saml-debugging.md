<properties 
    pageTitle="Kuidas SAML-põhiste ühekordse sisselogimise rakendustele Azure Active Directory silumine | Microsoft Azure'i" 
    description="Saate teada, kuidas silumine SAML-põhiste ühekordse sisselogimise rakendustele Azure Active Directory 's " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Kuidas silumine SAML-põhiste ühekordse sisselogimise rakendustele Azure Active Directory 's

Kui silumine SAML-põhiste rakenduste integreerimise, on sageli kasulik tööriista nagu [viiuldaja](http://www.telerik.com/fiddler) leiate SAML kutse, SAML vastuse ja tegeliku SAML luba, mis on rakenduse abil. Uurides SAML luba, saate tagada, et kõik nõutavad atribuudid, kasutajanime SAML teema ja väljaandja URI tulevad ootuspäraselt.

![][1]

Vastus: Azure'i AD, mis sisaldab SAML luba on tavaliselt ühe, mis ilmneb pärast mõne HTTP 302 suunata https://login.windows.net ja saadetakse konfigureeritud **Vastus URL-i** rakendus. 
 
Saate vaadata SAML luba see rida ja seejärel klõpsake soovitud **inspektorid > WebForms** menüü paremal. Sealt, paremklõpsake **SAMLResponse** väärtust ja valige **saada TextWizard**. Valige luba dekodeerida ja selle sisu kuvamiseks **muuta** menüü **Kaudu Base64** .
 
**Märkus**: HTTP-päring sisu kuvamiseks viiuldaja võib teilt konfigureerimine dekrüptimine HTTPS-liikluse, mida peate tegema.

## <a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Ühekordse sisselogimise rakendustele, mis ei ole Azure Active Directory rakenduse Galerii konfigureerimine](active-directory-saas-custom-apps.md)
- [Kuidas kohandada taotluste väljastatud SAML luba eelnevalt integreeritud rakendused](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
