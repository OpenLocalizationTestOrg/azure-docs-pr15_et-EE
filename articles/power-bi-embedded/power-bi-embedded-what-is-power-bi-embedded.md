<properties
   pageTitle="Mis on Microsoft Power BI manustatud?"
   description="Power BI manustatud võimaldab integreerida Power BI aruanded teie veebis või mobiilirakenduste seetõttu ei pea te luua kohandatud lahendusi kasutajate andmete visualiseerimiseks"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Mis on Microsoft Power BI manustatud?

**Power BI manustatud**, kus saate integreerida otse oma veebis või mobiilirakenduste Power BI aruanded.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI manustatud on **Azure'i teenus** , mis võimaldab tarkvaratoode ja Surface'i Power BI andmete kogemusi nende rakenduste rakenduste arendajatele. Arendaja, kui olete varem rakendusi ja need rakendused on oma kasutajate ja erinevate funktsioonide komplekt. Nendes rakendustes võib ilmneda ka sisseehitatud andmete jooni, näiteks diagramme ja aruandeid, mis on nüüd võib olla tootja Microsoft Power BI manustatud olema. Kasutajad ei pea Power BI konto oma rakenduse kasutamine. Jätkavad saate sisse logida nii nagu varem, rakenduse ja vaatamine ja kasutamine Power BI aruandluse kasutuskogemuse nõudmata täiendavad litsentsid.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Microsoft Power BI litsentsimine manustatud

**Microsoft Power BI manustatud** kasutus mudelis litsentsimine Power BI ei ole lõppkasutaja kohustusi.  Selle asemel **renderdab** rakendus, mis on visuaalsete tarbib arendaja on ostetud ja ei lisandu tellimusele, mis kuulub nende ressursside.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI manustatud kontseptuaalne mudel

![](media\powerbi-embedded-whats-is\model.png)

Nagu muude teenustega Azure Power BI manustatud ressursid on ette valmistatud [Azure'i ARM API-de](https://msdn.microsoft.com/library/mt712306.aspx)kaudu. Sel juhul on ressurss, mille olete ette **Power BI tööruumi saidikogumi**.

## <a name="workspace-collection"></a>Tööruumi saidikogumi

**Tööruumi saidikogumi** on ülataseme Azure container ressursid 0 või rohkem **tööruumide**sisaldava.  **Tööruumi** **saidikogumi** on kõik Azure standardatribuudid, samuti järgmist:

-   **Kiirklahvide** – Power BI API (kirjeldatud edaspidi) turvaliselt helistades kasutatud võtmed.
-   **Kasutajate** – Azure Active Directory (AAD) kasutajad, millel on administraatoriõigused haldamine Power BI tööruumi saidikogumi Azure portaali kaudu või ARM API.
-   **Piirkonna** – osana ettevalmistamise **Tööruumi saidikogumi**, saate valida piirkonnas olema ette valmistatud sisse. Lisateabe saamiseks lugege teemat [Azure regioonid](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Tööruumi

**Tööruum** on Power BI sisu, mis võivad hõlmata andmekomplektide, aruandeid ja armatuurlaudu. **Tööruum** on tühi, esmakordsel loomisel. Eelvaate, mida kuvatakse Autor kogu sisu, mis on Power BI Desktopi kasutamine ja üks tööruumide kasutamine [Power BI REST API-de](http://docs.powerbi.apiary.io/reference)laaditakse.

## <a name="using-workspace-collections-and-workspaces"></a>Tööruumi saidikogumite ja tööruumide kasutamine
**Tööruumi saidikogumite** ja **tööruumid** on ümbriste sisu, mis on kasutada ja korraldada sellest, millist paremini sobib kujunduse rakendus on loomine. Seal on saanud korraldate sisu neis mitmel erineval viisil. Kui valite panna ühte tööruumi sees kogu sisu ja seejärel kasutage hiljem rakenduse sõned täpsemaks jagada oma klientidele seas sisu. Samuti võite panna kõigile oma klientidele eraldi tööruumides, nii et on mõned nende vahe. Või võite valida piirkond, mitte klientide kasutajad. See paindlik kujundus võimaldab teil valida parim viis sisu korraldamiseks.

## <a name="cached-datasets"></a>Vahemällu salvestatud andmekomplektide

Vahemällu salvestatud andmekomplektide saab eelvaates.  Siiski ei saa värskendada vahemällu talletatud andmetega, kui see on koormatud **Microsoft Power BI manustatud**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Autentimise ja luba koos rakenduse märkide

**Microsoft Power BI manustatud** asukohta rakenduse sooritamiseks vajalikud kasutaja autentimise ja luba. Ei ole konkreetsete nõutav, et teie lõppkasutajad olema klientide Azure Active Directory (Azure AD).  Selle asemel väljendab rakenduse **Microsoft Power BI manustatud** autoriseerimine renderdamiseks aruande Power BI **Rakenduse autentimine märkide (rakenduse sõned)**abil.  Nende **Rakenduste sõned** luuakse vastavalt vajadusele, kui teie rakendus soovib aruande.  Vt [rakenduse märgid](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Microsoft Power BI manustatud**autentimiseks kasutatakse **Rakenduse autentimine märkide (rakenduse sõned)** .  On kolme tüüpi **Rakenduse sõned**.

1.  Märkide - kasutada, kui ettevalmistamise uue **tööruumi** **Tööruumi saidikogumi** ettevalmistamine
2.  Arengu sõned - kasutatud tegemisel kõned otse **Power BI REST API-d**
3.  Märkide - kasutada, kui helistamist aruande IFRAME'i manustatud manustamine

Nende tähiste kasutatakse koos **Microsoft Power BI manustatud**suhtluse etappide.  Märkide on koostatud nii, et saate Delegaadi õigused rakenduse Power BI. Lisateavet leiate teemast [Rakenduse Turbeloa Flow](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Vt ka
- [Microsoft Power BI manustatud tavastsenaariumid](power-bi-embedded-scenarios.md)
- [Microsoft Power BI manustatud alustamine](power-bi-embedded-get-started.md)
