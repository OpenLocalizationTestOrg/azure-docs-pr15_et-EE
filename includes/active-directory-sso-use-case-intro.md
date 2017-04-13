Ettevõtted on kasutada rohkem [kui teenus (SaaS) tarkvara](https://azure.microsoft.com/overview/what-is-saas/) rakenduste kontorirakenduste, sest muutuvad kergemini kättesaadavaks pilve tehnoloogia ja tööriistu. SaaS rakenduste arv kasvab, muutub keeruline administraatorite haldamine kontod ja kasutusõigused ja kasutajate paroolide erinevate meeles pidada. Nende rakenduste haldamine ükshaaval loob lisatööd ja on ebaturvalisem.


- Töötajad, kes on silma peal palju paroole pigem kasutada vähem turvaline meetodite meeles pidada, kirjutamine paroolide alla või sama paroolide kasutamise üle mitme kontod.

- Uue töötaja saabub või üks korral peab kõiki oma kontosid eraldi ette valmistatud või tühistage ette valmistatud.

- Lisaks töötajad võivad kasutuselevõtt SaaS rakendused oma töö minemata, mis tähendab, et need on arvutites IT-administraatorid pole kinnitatud ja ei kontrolli oma kontode loomine.  

Kõiki neid probleeme lahenduseks on ühekordse sisselogimise (SSO). See on lihtsaim viis mitme rakenduste haldamine ja anda kasutajatele ühtse sisselogimise kogemus. Azure Active Directory (Azure AD) pakub töökindlad SSO lahenduse ja on palju saadaolevad eelnevalt integreeritud rakendused, õpetused administraatoritele kiiresti häälestada uus rakendus ja alustage kasutajate ettevalmistamine.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Kuidas Azure Active Directory integreerida rakendusi?  

Azure AD võimaldab teil integreerida oma rakendused ja ettevalmistatud kontod. Seda saab teha ühte kahest võimalustest kaudu.

- Kui rakendus pole eelnevalt integreeritud rakenduse Galerii, saate läbida selle portaali rakenduste häälestamine ja lubama SSO sätete konfigureerimine. Mis tahes Galerii rakenduse, saate alustada, järgige lihtsa üksikasjalikke juhiseid rakenduse Galerii ja Azure'i portaalis esitatud ühekordse sisselogimise lubamiseks.

- Kui rakendus pole galeriis, saate enamiku rakenduste häälestamine Azure AD kohandatud rakendusena. Selleks on vaja veidi tehnilise teadmiste konfigureerida. Saate lisada mis tahes rakendus, mis toetab SAML 2.0 ühendatud rakendusena või mis tahes rakendus, mis on ka HTML-i-põhine sisselogimislehel rakendusena parooli SSO-d.

Kui kasutajad on loodud oma SaaS rakendusi, mis pole hallatavate kontode puhul [Pilveteenuste rakenduse Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) tööriist pakub lahenduse. See tööriist jälgib web liiklus kindlaks teha, millised rakendused on kasutusel terves asutuses ja inimestega, kes kasutavad iga arvu. Selle teabe abil saate seda saate teada, millised rakendused eelistate ja otsustada, milline integreerida Azure AD SSO kasutajad.  

Kui rakendus integreerida Azure AD, saate vastendada kasutajate loodud rakenduse identiteedid vastavaid Azure AD identiteedid.  
