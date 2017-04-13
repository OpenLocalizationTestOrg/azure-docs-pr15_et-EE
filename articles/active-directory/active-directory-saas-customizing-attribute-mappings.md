<properties
    pageTitle="Atribuudi vastendused kohandamise | Microsoft Azure'i"
    description="Siit saate teada, milliseid atribuudi vastendused SaaS rakenduste Azure Active Directory on, kuidas saab muuta neid teie ettevõtte vajadustele."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>Atribuudi vastendused kohandamine


Microsoft Azure AD toetab kasutaja ettevalmistamise kolmanda osapoole SaaS rakendustele nagu Salesforce, Google Appsi ja teised. Kui teil on kasutaja ettevalmistamise jaoks kolmandale osapoolele SaaS rakenduse lubatud, Azure'i haldusportaal määrab selle atribuudi väärtuste vormi nimega "atribuudi vastendamine" konfiguratsiooni.

On eelkonfigureeritud atribuut Azure AD kasutaja objektid ja iga SaaS rakenduse kasutaja objektide vaheliste vastenduste kogum. Mõni muud tüüpi objektid, nt rühmade või Kontaktide haldamine <br> 
Saate kohandada vaike atribuudi vastendused vastavalt teie ettevõtte vajadustele. See tähendab, et saate muuta või kustutada olemasoleva atribuudi vastendused või luua uue atribuudi vastendused.

Azure AD portaalis, pääsete juurde selle funktsiooni, klõpsates nuppu Atribuudid SaaS rakenduse tööriistaribal.

> [AZURE.NOTE] **Atribuutide** link on ainult saadaval juhul, kui kasutaja ettevalmistamise SaaS rakenduse jaoks lubatud. 


![Salesforce'i][1] 


Kui klõpsate tööriistaribal praeguse vastendused, mis on konfigureeritud SaaS rakenduse loendit atribuute.

Järgmine pilt kuvatakse näiteks nii:



![Salesforce'i][2]  


Ülaltoodud näites näete, et mõne hallatava objekti Salesforce'i **eesnimi** atribuut lisatakse **givenName** väärtus on lingitud Azure AD objekti.

Kui soovite kohandada oma atribuudi vastendused või kui soovite taastada kohandatud sätteid tagasi vaikimisi konfiguratsiooni selleks allservas rakenduse tööriistaribal nuppu seotud.


![Salesforce'i][3]  


On atribuudi vastendused, mis on nõutud SaaS rakenduse töötaks õigesti. Tabelis atribuutide vastendused seotud atribuut on **Jah** atribuudi **nõutav** väärtuseks. Kui mõni atribuudi vastendamine on vajalik, ei saa kustutada. Sel juhul **kustutamine** funktsioon pole saadaval.

Mõne olemasoleva atribuudi vastenduse muutmiseks valige lihtsalt vastendust ja seejärel klõpsake nuppu **Redigeeri**. Avab dialoogiboksi lehe, mis võimaldab teil muuta valitud atribuudi vastendamine.


![Atribuut vastenduse redigeerimine][4]  



## <a name="understanding-attribute-mapping-types"></a>Atribuudi vastendamine tüüpi mõistmine


Atribuut vastendustega, juhtelemendi kuidas atribuute täidetakse kolmanda osapoole SaaS rakenduse. On neli vastenduse eri tüüpi toetatud.

- **Otsest** – target atribuut lisatakse lingitud objekti atribuudi väärtus Azure AD.


- **Konstandi** – target atribuut lisatakse teie määratud teatud string.


- **Avaldise** - target atribuudi sisustatakse skripti nagu avaldise tulemi põhjal. Täpsema teabe saamiseks vt [Avaldiste kirjutamist atribuudi vastendused Azure Active Directory jaoks](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Ükski** - target atribuut on vasakule mille pole muudetud. Aga kui target atribuut pole kunagi tühi, see on olema täidetud järgmised teie määratud vaikeväärtus.



Lisaks nelja lihtsa atribuudi vastendamine järgmist tüüpi, kohandatud atribuudi vastendused toetavad **vaikimisi** väärtuse ülesande mõiste. Vaikimisi väärtuse ülesande tagab target atribuudi täidetud väärtusega, kui on määratud pole kumbagi väärtus Azure AD ega target objekti.

Microsoft Azure AD pakub väga tõhus rakendamine sünkroonimise protsess. Käivitub keskkonnas töödeldakse domeenikirjeid ainult neid objekte, mis nõuavad värskendused. Atribuudi vastendused värskendamine mõjutab domeenikirjeid jõudlus. See on atribuudi vastendamine konfiguratsiooni värskendus nõuab kõigi hallatavate objektide olema reevaluated. Seetõttu on soovitatav parimaks hoida oma atribuudi vastendused vähemalt järjestikust tehtud muudatuste arv.


##<a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Kasutaja ettevalmistamise/Deprovisioning SaaS rakenduste automatiseerimine](active-directory-saas-app-provisioning.md)
- [Avaldiste atribuudi vastendused kirjutamine](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Ulatuse määramine filtrid kasutajate ettevalmistamine](active-directory-saas-scoping-filters.md)
- [Luba automaatne ettevalmistamise kasutajatele ja rühmadele Azure Active Directory rakendustele SCIM abil](active-directory-scim-provisioning.md)
- [Konto ettevalmistamise teatised](active-directory-saas-account-provisioning-notifications.md)
- [Õpetused kuidas integreerida SaaS rakenduste loend](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
