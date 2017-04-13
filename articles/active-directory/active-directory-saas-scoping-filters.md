<properties
    pageTitle="Atribuut põhinev rakenduse ettevalmistamise ulatuse määramine filtrite abil | Microsoft Azure'i"
    description="Saate teada, kuidas vältida objektide rakendused, mis toetavad automatiseeritud kasutaja ettevalmistamise kaudu tegelikult ette valmistatud kui objekti ei vasta teie ettevõtte nõuetele ulatuse filtrite abil."
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


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Atribuut põhinev rakenduse ettevalmistamise ulatuse määramine filtrite abil

Selles jaotises eesmärk selgitatakse, kuidas kasutada ulatuse filtrite määratlemine atribuut põhinevad reeglid, mis määravad, millistel kasutajatel on ette valmistatud rakendusse.





## <a name="clauses-and-scope-groups"></a>Osalaused ja ulatus rühmad


![Ulatuse määramine Filter][1] 




Ulatuse filtrid on määratletud ühe või mitme **ulatus rühmad**, millest igaüks hoidke ühe või mitme **klauslitest**. Kindla ulatus rühma klauslitest vaatamiseks laiendage vasakul rühma nime noolt klõpsates.

**Klausel** määratleb, millised kasutajad tohivad läbivad ulatuse filtri hindamisel iga kasutaja atribuutide alusel. Näiteks võib teil ühe klausel, mis eeldab, et kasutaja "riik" atribuut New York, mis tähendab, et kuvatakse ainult New York kasutajate ette valmistatud rakendusse võrdne.

![Ulatuse määramine rühma nimi][2] 



Iga **rühma** algab ühe kohustuslik **klauslis**, nagu on näidatud pildil. See klausel lihtsalt täpsustab, et kasutaja tuleb esmalt määrata rakenduse enne seda hinnatakse teie ulatuse filtrid. See klausel ei saa kustutada ega muuta.

Saate lisada uue klauslitest või uue ulatus rühmad, vajutades vastavat nuppu. Saate anda iga rühma nimi, redigeerides atribuudi **Ulatus rühma nime** .





## <a name="how-scoping-filters-are-evaluated"></a>Kuidas hinnatakse filtrid ulatuse määramine

Me katse ajal ettevalmistamise, iga määratud kasutajale ulatuse filtrite suhtes kindlaks teha, kui kasutaja väärib rakenduse juurdepääsu. Mõelge iga klausel on test, mis tuleb selleks, et vältida saada filtreeritud kasutaja edasi. 

Kui teil on määratletud mitu ulatus rühma, iga kasutaja peate läbima vähemalt üks neist rakenduse juurdepääsuks. Igas rühmas ulatus siiski kasutaja peate läbima iga ühe klausel läbimiseks selle teatud rühma. 

Teisisõnu, mõelge ulatus rühmad on või oleks koos, ja Mõelge klauslitest neis on ja soovite koos. Näiteks vaatame allpool ulatuse filter.


![Ulatuse määramine rühma nimi][2]  


Vastavalt selle ulatuse filtri kasutajad peavad vastama järgmistele kriteeriumitele, peab olema ette valmistatud:

1. Ta peab olema määratud rakendus.

2. Ta peab töötama osakonnas

3. Need tuleb töö Kanada või San Francisco.


##<a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Kasutajate ettevalmistamine ja SaaS rakendustele Deprovisioning automatiseerimine](active-directory-saas-app-provisioning.md)
- [Kasutajate ettevalmistamine atribuudi vastendused kohandamine](active-directory-saas-customizing-attribute-mappings.md)
- [Avaldiste atribuudi vastendused kirjutamine](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Konto ettevalmistamise teatised](active-directory-saas-account-provisioning-notifications.md)
- [Luba automaatne ettevalmistamise kasutajatele ja rühmadele Azure Active Directory rakendustele SCIM abil](active-directory-scim-provisioning.md)
- [Õpetused kuidas integreerida SaaS rakenduste loend](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
