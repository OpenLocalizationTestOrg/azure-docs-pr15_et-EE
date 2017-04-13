<properties
   pageTitle="Azure Active Directory haldus üksuste haldus"
   description="Täpsema delegeerimine Azure Active Directory õiguste haldus üksuste abil"
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

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Azure'i AD - avaliku eelvaate haldus üksuste haldus

Selles artiklis kirjeldatakse haldus mõõtühikutes – uus Azure Active Directory ümbris ressursside, mida saate kasutada delegeerivad administraatoriõigusi üle kasutajate ja kasutajate alamhulga rakendamine poliitika kohta. Azure Active Directorys haldus üksused lubada keskse administraatorite volitatud esindaja õigused piirkondliku administraatoritele või Varundustöö taseme poliitika määramine.

See on kasulik ettevõtted, kus sõltumatu osad, näiteks suurte university, mis koosneb paljud autonoomne koolid (Business kooli matemaatika kooli ja jne) mis on üksteisest sõltumatult. Sellised on oma IT-administraatorid, kes juurdepääsu kontrollimiseks, kasutajate haldamine ja juurutada poliitikad nende jagamiseks. Keskse administraatorid on võimalik soovite anda need jagatud administraatoritele õiguste üle nende kindla osakondades kasutajate. Täpsemalt selles näites keskne administraator saab, näiteks haldus üksuse jaoks teatud kooli (Business kool) loomine ja asustada ainult kooli ärikasutajatele. Seejärel keskne administraator saate lisada selle personali Business kooli jäävates roll, teisisõnu, anda IT töötajad Business kooli administraatoriõigusi ainult üle Business kooli haldus üksus.

> [AZURE.IMPORTANT]
> Saate luua ja kasutada ainult siis, kui lubate Azure Active Directory Premium haldus üksused. Lisateavet leiate teemast [Azure AD Premium töötamise alustamine](active-directory-get-started-premium.md).

Keskne administraator seisukohast haldus üksus on directory objekti, mis saab luua ja eeltäidetakse ressursid. **Selles väljaandes need ressursid võib olla ainult need kasutajad.** Kui loodud ja täidetud, saab haldus üksus on antud õiguste piiramiseks ainult üle ressursid sisalduva haldus.

## <a name="managing-administrative-units"></a>Haldus üksuste haldamine

Preview väljaandes saate luua ja hallata haldus üksuste Azure Active Directory moodul Windows PowerShelli jaoks cmdlettide kasutamise.

Tarkvaranõuded ja Azure AD moodul installimise kohta lisateabe saamiseks ja haldamise haldus üksused, sh süntaks, parameetri kirjeldused ja näiteid, Azure AD moodul cmdlet-käskude kohta teavet teemast [haldamine Windows PowerShelli kaudu Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Järgmised sammud
[Azure Active Directory väljaanded](active-directory-editions.md)
