<properties
   pageTitle="Üksikasjaliku selgituse Azure Active Directory B2B koostöö preview abil | Microsoft Azure'i"
   description="Azure Active Directory B2B koostöö toetab oma rist – ettevõtte seoseid, võimaldades äripartneritega valikuliselt juurdepääsu oma ettevõtte rakendused"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure'i AD B2B koostöö eelvaade: üksikasjaliku selgituse

Juhendis kirjeldatakse, kuidas kasutada Azure AD B2B koostöö. Contoso IT-administraator, kui soovime kolme partneri tootjate rakenduste töötajatele. Ükski partneri ettevõte peab olema Azure AD.

- Alice: lihtsa partneri organisatsiooniskeemi
- Bob alla keskmise partneri organisatsiooniskeemi, peab Accessi rakenduste komplekt
- Carol kaudu keerukate partneri organisatsiooniskeemi, vajab juurdepääsu rakendused ja rühmaliikmeid contoso

Pärast kutsed saadetakse partneri kasutajad, meil saate konfigureerida neid Azure AD anda juurdepääsu rakendused ja liikmelisuse Azure portaali kaudu. Alustame Alice lisamisega.

## <a name="adding-alice-to-the-contoso-directory"></a>Contoso kataloogi Alice lisamine
1. Luua CSV-faili abil päised, nagu on näidatud, ilma vaevata luua ainult Alice'i **e-posti**, **DisplayName**ja **InviteContactUsUrl**. **Kuvatav** nimi ei kutsu kuvatav nimi ja Contoso Azure AD directory kuvatav nimi. **InviteContactUsUrl** on viis Alice Contoso ühendust võtta. Järgmises näites määrab InviteContactUsUrl Contoso LinkedIn profiili. See on oluline õigekirja sildid sellisena, nagu [CSV-faili vorming viite](active-directory-b2b-references-csv-file-format.md)määratud CSV-faili esimene rida.  
![Alice CSV-näidisfail](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Azure'i portaalis kasutaja lisamine Contoso kataloogi (Active Directory > Contoso > Kasutajad > Lisa kasutaja). "Kasutaja tüüp" rippmenüü, valige "Kasutajate partneri ettevõtetes". CSV-faili üleslaadimine Veenduge, et enne üleslaadimist on suletud CSV-faili.  
![CSV-faili üleslaadimine Alice](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice on nüüd esindatud Contoso Azure AD kataloogis väliskasutajana.  
![Alice on loetletud Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice saab järgmisel e-posti.  
![Alice kutse e-posti](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice klõpsab ja ta küsitakse kutse vastu ja logige sisse oma töö mandaadi abil. Kui Alice pole kataloogis Azure AD, Alice palutakse sisse logida.  
![Pärast kutse Alice registreerumine](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice on ümber rakenduse Access paani, tühja seni, kuni ta on antud juurdepääs rakendused.  
![Accessi Alice paneel](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

See toiming võimaldab B2B koostöö lihtsaim vorm. Contoso Azure AD kataloogis kasutajana Alice saate anda Accessi rakenduste ja rühmadele Azure portaali kaudu. Nüüd lisame Bob, kellel on vaja juurdepääsu rakendused Moodle ja Salesforce'i.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Contoso kataloogi Bob lisamine ja rakenduste juurdepääsu andmine
1. Azure AD moodul Windows PowerShelli kasutamine installitud rakenduste ID-d Moodle ja Salesforce'i leidmiseks. ID-d saab tuua cmdlet-käsu abil: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` kuvatakse tabeliveergude loend kõik saadaolevad rakendused Contoso ja nende AppPrincialIds.  
![Toomine Bob ID-d](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Looge CSV-faili sisaldava Bob e-posti ja DisplayName, **InviteAppID**, **InviteAppResources**ja InviteContactUsUrl. Asustamiseks **InviteAppResources** koos AppPrincipalIds Moodle ja Salesforce'i leitud PowerShelli, eraldades need tühikuga. Asustada **InviteAppId** koos sama AppPrincipalId Moodle kujundamine e-posti ja lehtede Moodle logo sisse logida.  
![Bob CSV-näidisfail](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. CSV-faili Azure portaali kaudu üles laadida, nagu seda tehti Alice. Nüüd on Bob Väliskasutaja Contoso Azure AD kataloogis.

4. Bob saab järgmisel e-posti.  
![Bob kutse e-posti](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Bob klõpsab ja küsitakse kutse vastu võtta. Pärast seda, kui ta on sisse logitud, ta suunatakse Accessi paani ja saate kasutada juba Moodle ja Salesforce'i.  
![Accessi Bob paneel](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Lisame Carol edasi, kes vajab rakendustele juurdepääsemiseks ning liikmelisuse kataloogis Contoso rühmadesse.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Contoso kataloogi Carol lisamine, rakenduste juurdepääsu andmine ja andes rühmakuuluvus

1. Installitud rakenduste ID-sid ja Contoso rühma ID-d leidmiseks Azure AD moodul Windows PowerShelli kasutamine.
 - AppPrincipalId cmdlet-käsu abil saate tuua `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, nagu Bob sama
 - Rühmade cmdlet-käsu abil saate tuua ObjectId väärtuse `Get-MsolGroup | fl DisplayName, ObjectId`. Kuvatakse tabeliveergude Contoso ja nende ObjectIds kõigi rühmade loendi. Rühma ID-d saab laadida ka objekti ID atribuudid vahekaardil rühma Azure'i portaalis.  
![ID-d ja rühmade Carol toomine](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Looge CSV-faili, ilma vaevata luua Caroli e-posti, DisplayName, InviteAppID, InviteAppResources, **InviteGroupResources**ja InviteContactUsUrl. **InviteGroupResources** on täidetud, ObjectIds MyGroup1 ja vastavalt sellele, eraldades need tühikuga rühma.  
![Carol CSV-näidisfail](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Laadige CSV-faili Azure portaali kaudu.

4. Carol on Contoso kataloogis kasutaja ja on ka liige rühmade MyGroup1 ja vastavalt sellele, nagu näha Azure portaali.  
![Carol on loetletud rühma Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol saab kutse vastu linki sisaldava meilisõnumi. Pärast, suunatakse ta rakenduse Access paneeli Moodle ja Salesforce'i juurde.  

See on kõik on kasutajate lisamise Azure AD B2B koostöös partneriga ettevõtetelt. Juhendis ilmnes Alice, Bob ja Carol kasutajate lisamine Contoso kataloogi kolm eraldi CSV-faili abil. Selle protsessi võib olla lihtsam kondenseeruv eraldi CSV-failid ühte faili.  
![Alice, Bob ja Carol CSV-näidisfail](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Seotud artiklid
Sirvige meie teised artiklid Azure AD B2B koostöö.

- [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kuidas see toimib](active-directory-b2b-how-it-works.md)
- [CSV-faili vorming viide](active-directory-b2b-references-csv-file-format.md)
- [Väliste kasutajate Turbeloa vorming](active-directory-b2b-references-external-user-token-format.md)
- [Väliste kasutajate objekti atribuutide muutmine](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Praegune eelvaade piirangud](active-directory-b2b-current-preview-limitations.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
