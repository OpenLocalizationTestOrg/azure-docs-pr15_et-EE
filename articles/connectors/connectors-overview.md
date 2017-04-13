<properties
    pageTitle="Loogika rakenduste konnektorid ülevaade | Microsoft Azure'i"
    description="Konnektorid, mida saate kasutada loogika rakenduse ülevaade"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Loogika rakenduse konnektorite abil

Konnektorid pakuvad üle teenused ja protokollid platvormide jaoks loodud kiireks juurdepääsuks sündmusi, andmed ja toimingud.  Täielik ühendused, mis toetab loogika rakenduste loendit saate [olla, leiate siit](apis-list.md).  Konnektorid saab kasutada käivitamiseks või toimingu loogika rakendus ja võib olla vaja kasutada konfigureeritud *ühendus* (nt: lubatakse Twitteri kontole juurde pääseda või postitavad teie nimel).

## <a name="basics"></a>Põhitõed

Konnektorid on majutatud teenused pääsete osana loogika app integreerimiseks Dynamics, Azure, Salesforce, [ja rohkem](apis-list.md)nagu muude teenustega.  Need on juurutatud ja haldab Microsoft, seega saate koostada oma integreerimine töövoogude skaala, jõudlus ja turvalisus tehtud eest.  Konnektori lisamiseks loogika rakenduse otsimine ja konnektori toimingu või käivitada, klõpsake jaotises **Kuva Microsoft hallatav API-de**valimine.

![Menüü Action valimise päästik][1]

Iga toimingu konnektor või päästik on selle atribuutide komplekti, konfigureerida.  Klõpsake toimingu kohta nuppude teave või viide selle dokumentatsiooni kohta [Lisateavet](apis-list.md).

Kui soovite integreerida teenuse või API, mis pole veel konnektor, saate ka laiendamine loogika rakenduste [kohandatud konnektor](../app-service-logic/app-service-logic-create-api-app.md) kaudu või helistage otse teenusesse on nagu HTTP-protokolli.

## <a name="triggers"></a>Päästikute

Mõned on päästik, mis tähendab, et sündmuse selle konnektor ei tule loogika rakenduse ja edastama mis tahes andmete päästiku osana.  Käivitamiseks on alati esimene samm loogika rakendus.  Populaarsed päästikute järgmised toimingud nagu.
 
 * Korduvus - iga tunni käivitamine
 * Kui HTTP-päring on vastu võetud
 * Kui üksuse järjekorda lisada
 * Kui meilisõnum on vastu võetud
 
Mõned päästikute tule vahetu sündmuse juhtub teavitus loogika rakenduse kaudu ja teised peavad Korduvus intervall, kui tihti loogika rakenduse kontrollib sündmuse (kuni iga 15 sekundi järel) teenus konfigureeritud.  

Kui sündmus on saanud, käivitage rakendus loogika tule ja töövoo toimingutes hakkavad.  Saate ka saab kasutada mis tahes andmete päästik kogu töövoo (nt "klõpsake uue säutsu" päästik läheb soovitud säutsu jooksma).

## <a name="actions"></a>Toimingud

Enamik konnektorid on ühe või mitu toimingud, mida saab kasutada töövoo käigus.  Mis tahes juhiseid, mis juhtub pärast Käivita on käivitatud kaudu käivitamiseks on toimingud.  Lisamiseks klõpsake toimingu **Uuele etapile** nuppu ja otsige konnektor, mida soovite kasutada.  Kui valitud (ja konfigureerinud kõik [ühendused](#connections) , mis võib olla vajalik) kuvatakse toimingu kaardi saate konfigureerida.  Saate andmeid valida eelmisi toiminguid, mis tahes märkide jaoks väljundeid klõpsates või sisestage mis tahes muud konfiguratsioon vastavalt vajadusele.

![Konnektor toimingu konfigureerimine][2]

## <a name="connections"></a>Ühendused

Enamik konnektorite tuleb konfigureerida *ühenduse* , enne kui saate kasutada konnektor.  *Ühendus* on Logi sisse- või ühenduse konfiguratsiooni, juurdepääsu konnektor.  Ühendused, mis kasutavad OAuthi, saate luua ühenduse tähendab teenusesse (nt Office 365, Salesforce või GitHub) kus saate oma juurdepääsu luba krüptitud ja Azure salajane poes säilitatakse.  Muude konnektorid (nt FTP ja SQL-i) jaoks on vaja ühendus, mis sisaldab konfiguratsiooni serveri aadress, kasutajanimi ja parool.  Need ühenduse konfiguratsiooni üksikasjad on ka krüptitud ja turvaliselt.  Ühenduste saab juurdepääsu teenus, kui teenus võimaldab.  Azure Active Directory OAuthi ühenduste (nt Office 365 ja Dynamics) saate jätkata värskendamine juurdepääsu luba lõpmatuseni.  Muude teenuste võib võtta kaua kasutame märgiks ilma selleta värskendamise piirangud.  Üldiselt teatud toiminguid nagu parooli muutmise kaotavad kõik Accessi sõned.  

Ühenduste saab vaadata ja hallata Azure, klõpsates nuppu **Sirvi** ja valides **API ühendused**.  API ühendused ressursside kaudu saate vaadata, redigeerida, värskendada või uuesti lubada mis tahes teie loodud ühendused.

## <a name="next-steps"></a>Järgmised sammud

- [Oma esimese loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Siit saate teada, levinud kasutab ja loogika rakenduste näited](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Ettevõtte integreerimine päästikute ja toimingute kasutamise alustamine](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png