<properties
   pageTitle="Azure'i andmekataloogi eeltingimused | Microsoft Azure'i"
   description="Azure'i andmekataloogi eeltingimused – mida on vaja Azure andmekataloogi kasutamise alustamine."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure'i andmekataloogi eeltingimused

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Mida on vaja Azure andmekataloogi kasutamist alustada?

On mõned asjad, mida peate enne häälestamist **Azure'i andmekataloogi**eest. Ärge muretsege – ei võta kaua!

## <a name="azure-subscription"></a>Azure'i tellimus
Azure'i andmekataloogi häälestada, peate olema omanik või kaasomaniku Azure tellimuse.

Azure'i tellimused aitavad teil korraldada pilvepõhise teenuse ressursid nagu Azure'i andmekataloogi juurdepääs. Nad saate määrata, kuidas esitatakse ressursikasutus, aitavad eest tuleb tasuda ja makstud. Iga tellimuse võib olla erinevad arveldus- ja makse häälestamise, siis võite olla erinevad tellimused ja eri lepingud, osakond, project, piirkondlike office ja jne. Iga pilveteenuses kuulub tellimus ja peab teil olema tellimus enne häälestamist Azure'i andmekataloogi. Lisateabe saamiseks vt [kontode haldamine, tellimused ja administraatorirollid](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Azure'i andmekataloogi häälestada, peate olema logitud Azure Active Directory kasutaja konto abil sisse.

Azure Active Directory (Azure AD) abil on lihtne hallata identiteedi ja juurdepääsu nii pilveteenuste ja kohapealse oma ettevõtte jaoks. Kasutajad saavad kasutada ühe töö või kooli konto jaoks ühekordse sisselogimise mis tahes pilveteenusesse ja kohapealse veebirakendus. Azure'i andmekataloogi kasutab Azure AD sisselogimise autentida. Lisateavet leiate teemast [mis on Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] [Azure'i portaal](http://portal.azure.com/) võimaldab kasutajatel sisse logida isikliku Microsofti Account või mõne Azure Active Directory töö või kooli kontoga. Azure'i andmekataloogi Azure'i portaalis või [andmekataloogi portaali](http://www.azuredatacatalog.com) häälestamiseks peate olema logitud Azure Active Directory kontot, mitte isikliku konto abil sisse.

## <a name="active-directory-policy-configuration"></a>Active Directory poliitika konfigureerimine

Mõnel juhul kasutajad võib tekkida olukord, kus ta saab sisse logida Azure'i andmekataloogi portaali, kuid proovimisel andmete allikas registreerimise tööriist neil tekkida võivad tõrketeade, mis takistab nende sisse logite sisse logida. See probleem võib juhtuda ainult siis, kui kasutaja on ettevõtte võrgus või võib ilmneda ainult siis, kui kasutaja on väljaspool ettevõtte võrku ühendamine.

Andmete allikas registreerimise tööriist kasutab valideerimiseks kasutaja ka vastu Active Directory vormide autentimist. Eduka sisselogimise kohta, vormide autentimist peab olema lubatud globaalne autentimise poliitika Active Directory administraator.

Globaalne autentimise poliitika võimaldab autentimismeetodite tuleb lubada eraldi sise-ja suhtevõrgus ühendused, nagu on näidatud allpool. Sisselogimise tõrgete võib ilmneda juhul, kui vormide autentimine on lubatud, kust kasutaja on ühendatud võrku.

 ![Active Directory globaalne autentimise poliitika](./media/data-catalog-prerequisites/global-auth-policy.png)

Lisateabe saamiseks vt [Autentimise poliitikate konfigureerimine](https://technet.microsoft.com/library/dn486781.aspx).
