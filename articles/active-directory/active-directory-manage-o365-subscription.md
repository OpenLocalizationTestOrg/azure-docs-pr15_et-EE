<properties
   pageTitle="Office 365 tellimuse Azure directory haldamine | Microsoft Azure'i"
   description="Office 365 tellimuse directory abil Azure Active Directory ja Azure klassikaline portaali haldamine"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Office 365 tellimuse Azure directory haldamine

Selles artiklis kirjeldatakse, kuidas hallata Office 365 tellimuse kasutajaks, Azure klassikaline portaalis loodud kataloogis. Peate olema kas teenuse administraator või koostöö administraator Azure tellimuse Azure klassikaline portaali sisse logima. Kui teil pole veel Azure tellimuse, saate registreeruda [tasuta 30 - päevane](https://azure.microsoft.com/trial/get-started-active-directory/) täna ja juurutada esimese pilve lahendus all 5 minutit, kasutades seda linki. Ärge unustage kasutada töökoha või kooli kontoga, mida kasutate Office 365 sisse logida.

Pärast lõpetamist Azure'i tellimus, saate Azure klassikaline portaali sisse logida ja Azure teenustele juurdepääs puudub. Klõpsake nuppu Active Directory laiendamine samas kaustas, mis kontrollib Office 365 kasutajate haldamine.

Kui teil on juba Azure'i tellimus, on täiendavad directory haldamine on ka lihtne. Näiteks, Michael Smith on Office 365 tellimuse jaoks Contoso.com. Ta on ka Azure tellimuse, kasutades oma Microsofti konto kasutajaks allkirjastatud msmith@hotmail.com. Sel juhul ta haldab kahe kataloogi.

  Tellimuse |  Office 365  |  Azure'i
  -------------- | ------------- | -------------------------------
  Kuvatav nimi |  Contoso  |     Vaikekataloogi Azure Active Directory (Azure AD)
  Domeeninimi  |  contoso.com  | msmithhotmail.onmicrosoft.com

Ta soovib hallata kasutajate identiteete kataloogis Contoso samal ajal, kui ta on sisse logitud oma Microsofti kontoga Azure'i nii, et ta saab lubada Azure AD funktsioone nagu multifactor autentimist. Järgmine diagramm võib aidata kirjeldavad protsess.

![Diagrammi kahe sõltumatu kataloogide haldamine](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

Sel juhul on kaks kataloogide üksteisest sõltumatult.

## <a name="to-manage-two-independent-directories"></a>Kahe sõltumatu kataloogide haldamine
Selleks, et hallata nii kataloogide samal ajal, kui ta on sisse logitud Azure'i nimega Michael Smith msmith@hotmail.com, ta peate tegema järgmised toimingud:

> [AZURE.NOTE]
> Neid juhiseid lõpule viia ainult siis, kui kasutaja on sisse loginud Microsofti kontoga. Kui kasutaja on sisse loginud töö või kooli kontot, võimalus **kasutada olemasoleva kausta** pole saadaval. Töökoha või kooli kontoga on kinnitatud, vaid oma kodune kataloogi (st kataloogi, kus talletatakse töökoha või kooli kontoga ja ettevõtte või kooli).

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com) sisse logida msmith@hotmail.com.
2.  Klõpsake nuppu **Uus** > **rakenduse teenuste** > **Active Directory** > **Directory** > **Kohandatud loomine**.
3.  Klõpsake nuppu Kasuta olemasoleva kausta ja märkige ruut **olen valmis nüüd välja logitud** .
4.  Azure'i klassikaline portaali sisse logima nimega globaalne administraator, Contoso.onmicrosoft.com (nt msmith@contoso.com).
5.  Kui teil palutakse **Contoso kataloogi kasutamine Azure?**, klõpsake nuppu **Jätka**.
6.  Klõpsake nuppu **Logi välja kohe**.
7.  Azure'i klassikaline portaali sisse logida msmith@hotmail.com. Contoso kataloogi ja vaikekataloogi kuvatakse Active Directory laiendamine.

Pärast nende juhiste msmith@hotmail.com on kataloogis Contoso üldadministraator.

## <a name="to-administer-resources-as-the-global-admin"></a>Kui globaalne administraator ressursid haldamiseks
Nüüd Oletame, et Aile mägi vajab haldamine veebisaitide ja andmebaasi ressursse, mis on seotud Azure'i abonementide msmith@hotmail.com. Enne kui ta saab teha, Michael Smith peab lõpule viima täiendavad järgmist:

1.  Logige sisse [Azure klassikaline portaalis](https://manage.windowsazure.com) administraator kontot kasutades Azure tellimuse (selles näites msmith@hotmail.com).
2.  Tellimus üle kanda Contoso kataloogi: klõpsake nuppu **sätted** > **tellimuste** > valige tellimus, > **Redigeeri Directory** > valige **Contoso (Contoso.com)**. Osana edastamise, mis tahes töö või kooli kontot, mis on kaasadministraatorite tellimuse eemaldatakse.
3.  Koostöö administraator tellimuse Aile mägi kuuluva: klõpsake nuppu **sätted** > **Administraatorid** > valige tellimus, > **Lisa** > tüüp **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Järgmised sammud
Tellimused ja kataloogide vaheline seose kohta lisateabe saamiseks näha, [Kuidas tellimus on seostatud kataloog](active-directory-how-subscriptions-associated-directory.md).
