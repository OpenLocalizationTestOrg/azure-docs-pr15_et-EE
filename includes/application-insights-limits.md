On mõned piirangud arv mõõdikute ja sündmuste kohta (st instrumentation võtme kohta). 

[Esimese taseme hinnad](https://azure.microsoft.com/pricing/details/application-insights/) teie valitud sõltuvad piirangud.

**Ressurss** | **Vaikimisi piiri** | **Lubatud**
-------- | ------------- | -------------
Seansi andmepunktid<sup>1, 2</sup> kuus | piiramatu | 
Kokku andmepunktide kuus koosolekukutsete, sündmus, sõltuvus, Jälita, erand ja lehe vaatamine | 5 miljonit | 50 miljonit<sup>3</sup>
Andmete [jälgimine ja logige](../articles/application-insights/app-insights-search-diagnostic-logs.md) määr | 200 dp/s | 500 dp/s
[Erandi](../articles/application-insights/app-insights-asp-net-exceptions.md) andmete määr | 50 dp/s | 50 dp/s
Koosolekukutse, sündmus, sõltuvus ja lehe vaate telemeetria kokku andmete määr | 200 dp/s | 500 dp/s
[Otsingu](../articles/application-insights/app-insights-diagnostic-search.md) -ja [Analytics](../articles/application-insights/app-insights-analytics.md) toorandmetega säilitamine | 7 päeva
Kokkuvõtlike andmete säilitamise [mõõdikute](../articles/application-insights/app-insights-metrics-explorer.md) Exploreri | 90 päeva
[Atribuudi](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) nimi loendamine | 100 |
Atribuudi nime pikkus | 150 | 
Atribuudi väärtus pikkus | 8192 | 
Jälita ja erandi teade pikkus | 10000 |
[Meetermõõdustik](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) nime arv | 100 |
Argumendil nime pikkus |  150 | 
[Kättesaadavus kontrollib](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> andmepunkti on üksikud argumendil väärtuse või sündmus, manustatud atribuudid ja mõõdud.

<sup>2</sup> A seansi andmepunkti logib algus- või seansi lõppu ja logib kasutaja identiteet.

<sup>3</sup> saate osta täiendav läbilaskevõime üle 50 miljonit.
 
[Hinnad ja rakenduse ülevaated kvootide kohta](../articles/application-insights/app-insights-pricing.md)
