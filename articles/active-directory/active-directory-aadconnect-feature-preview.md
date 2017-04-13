<properties
   pageTitle="Azure'i AD-ühendus: Funktsioonid eelvaates | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse üksikasjad rohkem funktsioone, mis on eelvaade Azure'i AD-ühenduse."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Täpsemat teavet funktsioonide eelvaade
Selles teemas kirjeldatakse, kuidas kasutada funktsioone praegu eelvaade.

## <a name="group-writeback"></a>Rühma tagasikirjutusega
Rühma tagasikirjutusega valikulised funktsioonid suvandi võimaldab teil tagasikirjutusega **Office 365 rühmade** abil mets koos Exchange'iga installitud. See on rühm, mis on alati õppinud pilveteenuses. Kui teil on asutusesisese Exchange'i, siis te saate kirjutamise tagasi nende rühmade kohapealse nii asutusesisese Exchange'i postkasti kasutajate saab saata ja vastu võtta e-kirju nende rühmade.

Lisateabe saamiseks Office 365 rühmade ja kuidas neid kasutada leiate [siit](http://aka.ms/O365g).

Selles rühmas on esindatud levirühmana asutusesiseses AD DS. Teie asutusesisese Exchange'i serveriga peab olema Exchange 2013 koondvärskenduse 8 (avaldatud märts 2015) või Exchange 2016 äratuntavaks selle uue rühma tüüp.

**Märkmete eelvaate ajal**

- Atribuut aadress aadressiraamatu praegu eelvaates pole täidetud. Ilma see atribuut pole nähtavad rühma globaalses Aadressiloendis. Atribuudi asustamiseks on kõige lihtsam viis on kasutada Exchange PowerShelli cmdleti `update-recipient`.
- Ainult metsad koos skeemi Exchange on kehtiv sihtkohtade rühmade jaoks. Kui tuvastati pole Exchange'i, siis rühma tagasikirjutusega ei ole võimalik lubada.
- Ainult ühe – mets Exchange'i organisatsiooni juurutuste on praegu toetatud. Kui teil on rohkem kui üks Exchange'i organisatsiooni kohapealse, siis peate lahenduse kohapealse GALSync nende rühmade kuvada oma metsas.
- Funktsiooni rühma tagasikirjutusega ei oska praegu turberühmad või levirühmad.

>[AZURE.NOTE] Azure'i AD Premiumi tellimuse jaoks on vaja rühma tagasikirjutusega.

## <a name="user-writeback"></a>Kasutaja tagasikirjutusega
> [AZURE.IMPORTANT] Kasutaja tagasikirjutusega Eelvaatefunktsioon eemaldatud August 2015 Värskenda Azure'i AD-ühenduse. Kui olete selle lubanud, siis tuleks selle funktsiooni keelata.

## <a name="next-steps"></a>Järgmised sammud
Jätkake oma [kohandatud installi Azure'i AD-ühenduse](./connect/active-directory-aadconnect-get-started-custom.md).

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
