<properties 
    pageTitle="Azure'i API teabehalduspoliitikate atribuutide kasutamine" 
    description="Saate teada, kuidas kasutada Azure API teabehalduspoliitikate atribuudid." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Azure'i API teabehalduspoliitikate atribuutide kasutamine

API teabehalduspoliitikate on võimas võimalus süsteemi, mis võimaldavad avaldaja, kelle soovite muuta serveri käitumist API konfigureerimise kaudu. Poliitika on kogumi laused, mis täidetakse järjest taotluse või vastuse API. Poliitika laused saate ehitatud sõnasõnaline tekst väärtused, poliitika avaldiste ja atribuudid. 

Iga API halduse eksemplari on /-väärtuse paarideks globaalse eksemplari atribuutide kogum. Neid atribuute saab kasutada konstandi stringiväärtust kogu API konfigureerimine ja poliitikate haldamine. Iga on järgmised atribuudid.


| Atribuut | Tüüp            | Kirjeldus                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Nimi      | string          | Atribuudi nimi. See võib sisaldada ainult tähti, numbrit, periood, mõttekriipsu ja allkriipsumärkideks. |
| Väärtus     | string          | Atribuudi väärtus. See võib olla tühi või koosnevad ainult tühiku.                           |
| Salajane    | kahendmuutuja         | Määrab, kas väärtus on salajane ja peaksid olema krüptitud või mitte.                                |
| Sildid      | massiivi string. | Valikuline siltidega kui saab kasutada atribuudi loendit filtreerida.                               |

Publisheri portaalis vahekaardil **Atribuudid** on konfigureeritud atribuudid. Järgmises näites on konfigureeritud kolme atribuudid.

![Atribuudid][api-management-properties]

Kinnisvara võib sisaldada literaalstringide kordamine ja [poliitika avaldised](https://msdn.microsoft.com/library/azure/dn910913.aspx). Järgmises tabelis eelmisele kolme valimi atribuudid ja nende atribuutide. Väärtus `ExpressionProperty` poliitika avaldis, mis tagastab stringi, mis sisaldab praeguse kuupäeva ja kellaaja. Atribuut `ContosoHeaderValue` on märgitud salajase, nii, et selle väärtus ei kuvata.

| Nimi               | Väärtus                      | Salajane | Sildid    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | FALSE  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | True   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | FALSE  |         |

## <a name="to-use-a-property"></a>Atribuudi kasutamine

Atribuut poliitika kasutamiseks paigutada atribuudi nimi sees looksulud nagu kahekordsete jutumärkide paar ilma `{{ContosoHeader}}`, nagu on näidatud järgmises näites.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

Selles näites `ContosoHeader` kasutatakse päise nimi on `set-header` poliitika, ja `ContosoHeaderValue` kasutatakse päise väärtus. Kui see poliitika hinnatakse taotluse või vastuse API andmehalduslüüsi ajal `{{ContosoHeader}}` ja `{{ContosoHeaderValue}}` on asendatud nende vastavate atribuutide väärtused.

Atribuudid saab kasutada täielikku atribuuti või elemendi väärtused, nagu on näidatud eelmises näites, kuid nad saavad ka lisada või koos osa sõnasõnaline tekst avaldis, nagu on näidatud järgmises näites:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Poliitika avaldiste võib sisaldada ka atribuudid. Järgmises näites on `ExpressionProperty` kasutatakse.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Kui see poliitika hinnatakse, `{{ExpressionProperty}}` on asendatud väärtusega: `@(DateTime.Now.ToString())`. Kuna väärtus on poliitika avaldis, avaldist hinnatakse ja poliitika tulu selle täitmist.

Toiming, mis on poliitikale atribuudid ulatus helistades saate testida arendaja portaalis see välja. Järgmises näites on kaks eelmises näites nimega toimingu `set-header` poliitika atribuudid. Pange tähele, et vastus ei sisalda kaks kohandatud päised, konfigureeritud poliitikate abil atribuudid.

![Portaali arendaja][api-management-send-results]

Kui vaatate [API inspektor Jälita](api-management-howto-api-inspector.md) kutse, mis sisaldab eelmise kahe valimi poliitikate atribuutidega, saate vaadata kahe `set-header` poliitikaid ning poliitika avaldise hindamise atribuudi, kus asunud poliitika avaldise lisatud atribuudi väärtustega.

![API inspektor jälgimine][api-management-api-inspector-trace]

Pange tähele, et ajal kinnisvara võib sisaldada poliitika avaldist, kinnisvara ei tohi sisaldada muid atribuute. Kui tekst, mis sisaldab viite atribuuti kasutatakse atribuudi väärtust, `Property value text {{MyProperty}}`, selle atribuudi viide ei saa asendada ja selle atribuudi väärtust hulka.

## <a name="to-create-a-property"></a>Atribuudi loomiseks

Atribuudi loomiseks klõpsake nuppu **Lisa atribuut** vahekaardil **Atribuudid** .

![Atribuudi lisamine][api-management-properties-add-property-menu]

**Nimi** ja **väärtus** on nõutav väärtused. Kui selle atribuudi väärtuseks on salajane, märkige ruut **see on salajane** . Sisestage üks või mitu valikuline silte, et aidata korraldada oma atribuudid ja klõpsake nuppu **Salvesta**.

![Atribuudi lisamine][api-management-properties-add-property]

Uue atribuudi salvestamisel **Otsingu atribuudi** tekstivälja lisatakse uus atribuudi nimi ja kuvatakse uus atribuut. Kuva kõik atribuudid, tühjendage **atribuut otsingu** tekstiväli ja vajutage enter.

![Atribuudid][api-management-properties-property-saved]

Atribuut REST API abil loomise kohta leiate teemast [REST API abil atribuusi loomine](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Kui soovite atribuudi redigeerimine

Atribuudi redigeerimine atribuudi redigeerimiseks kõrval nuppu **Redigeeri** .

![Atribuudi redigeerimine][api-management-properties-edit]

Tehke soovitud muudatused ja klõpsake nuppu **Salvesta**. Kui muudate atribuudi nimi, mis tahes poliitika, mis viitavad selle atribuudi värskendatakse automaatselt uue nime kasutamiseks.

![Atribuudi redigeerimine][api-management-properties-edit-property]

Redigeerimise atribuut REST API abil leiate teemast [REST API abil atribuudi redigeerimine](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Atribuudi kustutamiseks

Atribuudi kustutamiseks klõpsake atribuudi kustutamiseks kõrval nuppu **Kustuta** .

![Atribuudi kustutamine][api-management-properties-delete]

Klõpsake nuppu **Jah, kustutage see** kinnitamiseks.

![Kustutamise kinnitamine][api-management-delete-confirm]

>[AZURE.IMPORTANT] Kui atribuut on viidatud, mis tahes poliitikaid, ei saa edukalt kustutada, kuni kõik poliitikad, mida kasutada selle atribuudi eemaldamine.

Atribuut REST API abil kustutamise kohta leiate teemast [REST API abil atribuudi kustutamine](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Otsing ja filtri atribuudid

Vahekaart **Atribuudid** sisaldab otsingu ja filtreerimise võimalusi, mis aitavad teil hallata oma atribuudid. Atribuut loendi atribuudi nime järgi filtreerimiseks Sisestage otsingutermin väljale **Otsi atribuut** . Kuva kõik atribuudid, tühjendage **atribuut otsingu** tekstiväli ja vajutage enter.

![Otsing][api-management-properties-search]

Atribuut loendi sildi väärtuste järgi filtreerimiseks sisestamine üks või mitu tekstivälja **Filtreeri sildid** . Kuva kõik atribuudid, tühjendage ruut **siltide alusel filtreerimiseks** tekstiväli ja vajutage sisestage.

![Filtreerimine][api-management-properties-filter]

## <a name="next-steps"></a>Järgmised sammud

-   Lisateavet poliitikate töötamise kohta
    -   [Poliitikate API haldus](api-management-howto-policies.md)
    -   [Poliitika viide](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Poliitika avaldised](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Vaadake videot ülevaade

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

