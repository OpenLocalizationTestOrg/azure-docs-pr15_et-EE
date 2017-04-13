<properties
   pageTitle="Azure'i AD-ühendus sünkroonimine: Directory laiendid | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse funktsiooni directory laiendid Azure'i AD-ühenduse."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure'i AD-ühendus sünkroonimine: Directory laiendid
Directory laiendid võimaldab teil laiendada skeemiga Azure AD oma atribuutidega kohapealse Active Directory kaudu. See funktsioon võimaldab teil tarbimine atribuute, saate ka edaspidi haldamine kohapealse LOB rakenduste koostamine. Neid atribuute saate tarbitud [Azure AD Graphi directory laiendid](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) või [Microsoft Graphi](https://graph.microsoft.io/)kaudu. Saate vaadata saadaval [Azure AD Graphi Exploreri](https://graphexplorer.cloudapp.net) ja [Microsoft Graphi Exploreri](https://graphexplorer2.azurewebsites.net/) abil vastavalt atribuute.

Praegu ei ole Office 365 töökoormus tarbib neid atribuute.

Saate konfigureerida, millised täiendavaid atribuute, mida soovite sünkroonida installiviisardis tee kohandatud sätteid.
![Skeemi laiend viisard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) installimise näitab järgmisi atribuute, mis ei sobi leviloendite:

- Kasutaja ja rühma objekti tüübid
- Ühe väärtusega atribuute: stringi, kahendmuutuja, täisarv, Binary
- Mitme väärtusega atribuute: stringi, Binary

Atribuutide loendis on kirjutuskaitstud vahemälust installimisel Azure'i AD-ühenduse loonud. Kui teil on laiendada Active Directory skeem täiendavad atribuutidega, on [skeemi tuleb värskendada](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) enne need uue atribuudid on nähtavad.

Objekti võivad olla kuni 100 directory laiendid atribuudid. Max pikkus on 250 märki. Kui atribuudi väärtus on pikem, kärbitakse seda mootori sünkroonimine.

Installimisel Azure'i AD-ühenduse, registreeritakse rakenduse, kui need on saadaval. Saate vaadata selle rakenduse Azure'i portaalis.  
![Skeemi laiend rakenduse](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Neid atribuute on nüüd saadaval Graphi kaudu:  
![Graafik](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Atribuute, mis on tähistatud laiendiga\_{AppClientId}\_. Funktsiooni AppClientId on sama väärtus Kõik atribuudid kataloogis Azure AD.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
