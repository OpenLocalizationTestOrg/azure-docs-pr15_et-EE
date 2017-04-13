
<properties
    pageTitle="Dünaamiliste rühmade liikmelisuse tõrkeotsing | Microsoft Azure'i"
    description="Dünaamiliste Azure AD rühmade liikmelisuse tõrkeotsingu näpunäiteid."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Dünaamiliste liikmelisused rühmade tõrkeotsing

**Ma konfigureeritud reegli rühma, kuid pole liikmelisus õle jaotises**<br/>Veenduge, et sätte **Luba delegeeritud haldus** on seatud **Jah** **konfigureerimine** vahekaardil. Selle sätte kuvatakse ainult siis, kui olete sisse logitud kasutajana, kellele on määratud on Azure Active Directory Premium litsents. Veenduge, et väärtusi kasutaja atribuutide reegli: ei vasta kasutajatelt?

**Mul on konfigureeritud reegli, kuid nüüd olemasolevate liikmete reegel on eemaldatud**<br/>See on oodatud käitumine. Olemasoleva rühma liikmetel on eemaldatud, kui reegel on lubatud või muudetud. Tagastatud hindamisest reegli kasutajad lisatakse isikuid rühma.     

**Ma ei näe liikmelisus muutub kohe, kui lisada või muuta reegli, miks mitte?**<br/>Sihtotstarbeline liikmelisuse hindamiseks tehakse perioodiliselt käigus asünkroonne tausta. Palju aega kulub on määratud kataloogi kasutajate arv ja jaotises reegli loodud suurust. Tavaliselt näete kataloogide väikese arvu kasutajate rühma Liikmestaatuse muudatused vähem kui paar minutit. Suure hulga kasutajate kataloogid võib kuluda 30 minutit või pikemaks asustamiseks.

Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)
* [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
* [Mis on Azure Active Directory?](active-directory-whatis.md)
* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
