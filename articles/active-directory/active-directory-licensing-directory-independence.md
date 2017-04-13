<properties
   pageTitle="Lisada ja hallata mitut Azure Active Directory kataloogide | Microsoft Azure'i"
   description="Juhised ja lisamise ja Azure Active Directory kataloogid, mis selgitab kataloogide sõltumatu ressurssidena haldamise head tavad"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Lisada ja hallata mitut Azure Active Directory kataloogide

Azure Active Directory (Azure AD), on iga directory sõltumatu ressurss: omavahelistes, täielikult Objekte ja muud kataloogid, et haldate loogiliselt sõltumatud. Kataloogide vahel ei ole ema-tütre seose. See sõltumatus vahel kataloogide sisaldab ressursi sõltumatus, haldus sõltumatus ja sünkroonimise sõltumatus.

##<a name="resource-independence"></a>Ressursi sõltumatus

Kui loote või kustutada ressurss ühes kataloogis, on mõni muu kataloogi osaline, välja arvatud allpool kirjeldatud väliste kasutajate ressursi. Kui kasutate kohandatud domeeni "contoso.com" ühe Directory, ei saa kasutada muude Directoryga.

##<a name="administrative-independence"></a>Sõltumatus haldus

Kui haldus – kasutaja kataloogi "Contoso" loob siis testi kausta "Katse":
- Vaikimisi kasutaja, kes loob kausta on lisatud väline kasutaja selle uue kausta ja selle kausta üldadministraatori rolli määratud.
- Administraatorid directory "Contoso" on kataloogi "Katse" direct administraatoriõigusi, kui "Katse" administraator spetsiaalselt annab neile need õigused. Administraatorid "Contoso" saate hallata juurdepääsu directory "Katse", kui nad kontrollida kasutajakonto, mis on loodud "Katse."
- Kui soovite muuta (lisada või eemaldada) ühes kataloogis kasutaja administraatori rolli muutmine ei mõjuta administraatorirolli, et kasutajal oleks teise kataloogi.

##<a name="synchronization-independence"></a>Sünkroonimise sõltumatus

Saate konfigureerida iga Azure AD directory sõltumatult ühekordsest ühte sünkroonitud andmete toomine.
  - Kataloogisünkroonimise (DirSync) tööriista andmete sünkroonimiseks ühe AD mets.
  - Funktsiooni Azure Active Directory Connector jaoks Forefront Identity Manager, üks või mitu kohapealse metsade ja/või -Azure AD andmeallikate andmed sünkroonida.

##<a name="add-an-azure-ad-directory"></a>Lisage mõni Azure AD kataloog

Lisamiseks on Azure AD directory Azure klassikaline portaalis, valige vasakul Azure Active Directory pikendamise ja koputage nuppu **Lisa**.

> [AZURE.NOTE]   Erinevalt muud Azure ressursid ei ole oma kataloogide lapse ressursid Azure tellimuse. Kui tühistamine või lubada Azure tellimuse aegumist, pääsete endiselt andmevaadete directory Azure PowerShelli, Azure'i Graph API või muude liideste näiteks Office 365 halduskeskus. Mõne muu tellimuse saate seostada ka kataloogi.

Azure AD hulgilitsentsimise probleemide ja heade tavade laialdane ülevaate leiate teemast [mis on Azure Active Directory litsentsimise?](active-directory-licensing-what-is.md).
