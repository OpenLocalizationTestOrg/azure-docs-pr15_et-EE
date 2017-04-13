<properties
   pageTitle="Uue veebiteenuse juurutamine"
   description="Töövoo juurutamine ARM vastavalt veebiteenuse"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Uue veebiteenuse juurutamine

Microsoft Azure'i masina Õppekeskuse nüüd pakub veebiteenuseid, mis põhinevad [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) , mis võimaldab uue lepingu suvandid arveldamine ja juurutamine oma veebiteenuse mitme piirkonna jaoks.

Microsoft Azure'i masina õ veebiteenuste abil veebiteenuse juurutamiseks üldine töövoog on:

* Sõnastikupõhise katse loomine
* See juurutamine
* selle nime konfigureerimine
* arvelduse kavandamine
* Katsetage seda
* See peab olema.

Järgmisel joonisel on näidatud töövoog.

![Web teenuse juurutamise töövoo][1]
 
## <a name="deploy-web-service-from-studio"></a>Veebiteenuse Studio juurutamine 

Juurutamiseks katse uus veebiteenus. Logige arvutisse õ Studio ja loomine veebiteenuse sõnastikupõhise. 

**Märkus**: kui olete juba juurutanud katse nimega klassikaline veebiteenuse ei saa juurutamist uus veebiteenus nimega.
 
**Käivitage** katse lõuend allosas nuppu ja seejärel nuppu **Juurutamine veebiteenuse** ja **Juurutamine veebiteenuse [uus]**. Seadme õ veebiteenuse halduri juurutamise leht avatakse.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Seadme õ Web Service Manager juurutamine katse leht
Sisestage lehel juurutamine katse veebiteenuse nimi.
Valige hinnakirjad leping. Kui teil on, saate selle valida olemasoleva hinnakirjad plaani, vastasel juhul peate looma uue lepingu hind teenuse. 

1.  **Hind lepingu** rippmenüüd, valige olemasoleva plaani või suvand **uue lepingu valimine** .
2.  **Lepingu nime**, tippige nimi, mis tuvastab teie arvele leping.
3.  Valige üks **Kuu leping astme**. Pange tähele, et lepingute oma vaikimisi piirkond ja oma veebiteenuse lepingu astme vaikimisi on juurutatud piirkonnas.

Klõpsake käsku **Deploy** ja kiirjuhend oma web teenuse avaneb leht.

## <a name="quickstart-page"></a>Lehe Kiirjuhend
Teenuse Kiirjuhend veebileht annab teile juurdepääsu ja juhiseid kõige esmaste toimingute sooritamist uus veebiteenus loomise järel. Siit pääsete hõlpsalt **testleht** nii **tarbida** lehe.

## <a name="testing-your-web-service"></a>Oma veebiteenuse testimine

Kiirjuhend lehel klõpsake testi veebiteenuse jaotises tavalised toimingud.   

Testimiseks veebiteenuse nimega mõne päringu vastuse-teenuse (RRS) tehke järgmist.

* Klõpsake menüüribal nuppu **testi** .
* Klõpsake **Päringu vastuse**.
* Sisestage oma katse Sisestuskeel veergude jaoks vastavad väärtused.
* Klõpsake nuppu testi **Päringu vastuse**.

Saate tulemid kuvatakse lehe paremal serval.

Paketi täitmise teenuse (BES) veebiteenuse testimiseks saate CSV-faili.

* Klõpsake menüüribal nuppu **testi** .
* Klõpsake **paketi**.
* Jaotises sisendit, klõpsake nuppu Sirvi ja liikuge oma Näidisandmete fail.
* Klõpsake nuppu **testi**.

Oma testi olek kuvatakse jaotises **Pakett-tööde testida**.

## <a name="consuming-your-web-service"></a>Oma veebiteenuse tarbimine

Web teenuse juurutamisel anda Azure seadme õ katsete REST API-ga, saate tarbitud mitmed seadmed ja platvormide. Selle põhjuseks on lihtne REST API aktsepteerib ja vastab koos JSON-vormingus sõnumite. Azure'i masina õ portaalis on kood, mida saab kasutada helistamiseks veebiteenuse R, C# ja Python.
 
Klõpsake lehel Consuming leiate:

* API võti ja URI jaoks tarbimine veebiteenuse rakendustes.
* Exceli ja web appi mallide alustada oma tarbimine alustamiseks.
* Proovi kood C#, python ja R võite alustada.

Tarbimine veebiteenuste kohta leiate lisateavet teemast [Azure seadme õ veebiteenus, mis on kasutusele võetud Õppekeskuse katse seadmes peab olema](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Järgmised sammud

Tarbimine veebiteenuste kohta leiate lisateavet teemast:

[Kuidas Azure seadme õ Õppekeskuse katse seadmes juurutatud veebiteenuse kasutamine](machine-learning-consume-web-services.md)

[Azure'i masina õ Web Services: Juurutus- ja tarbimine](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
