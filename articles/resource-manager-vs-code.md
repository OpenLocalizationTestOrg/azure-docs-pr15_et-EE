<properties
   pageTitle="Ressursihaldur mallide kasutamine VS kood | Microsoft Azure'i"
   description="Näitab, kuidas häälestada Visual Studio kood Azure'i ressursihaldur mallide loomiseks."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Töötamine Visual Studio kood Azure ressursihaldur Mallid

Azure'i ressursihaldur Mallid on JSON faile, mis kirjeldavad ressursi ja seotud sõltuvused. Need failid võib mõnikord olla suur ja keeruline nii instrumentaarium tugi on oluline. Visual Studio kood on uus, kerge, avatud lähtekoodi, mitu platvormi koodi redaktor. See toetab loomise ja redigeerimise ressursihaldur Mallid on [uus laiend](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)kaudu. VS kood töötab igal pool ja ei nõua Interneti-ühendus, v.a juhul, kui soovite oma ressursihaldur Mallid juurutamine.

Kui teil on juba VS koodi, saate selle installida [https://code.visualstudio.com/](https://code.visualstudio.com/)juures.

## <a name="install-the-resource-manager-extension"></a>Installige ressursihaldur laiend

JSON Mallid VS koodi töötamiseks on vaja installida laiendamine. Järgmised toimingud allalaadimine ja installimine keele tuge ressursihaldur JSON Mallid:

1. Käivitage VS kood 
2. Avatud kiirülevaate Ava (Ctrl + P) 
3. Käivitage järgmine käsk: 

        ext install azurerm-vscode-tools

4. Taaskäivitage VS kood, kui teil palutakse lubada laiendamine. 

 Töö valmis!

## <a name="set-up-resource-manager-snippets"></a>Ressursihaldur Koodilõigud häälestamine

Eespool kirjeldatud installitud instrumentaarium tugi, kuid nüüd tuleb konfigureerida VS koodi kasutada JSON malli Koodilõigud.

1. Faili sisu kopeerimine [Azure'i xplat-arm instrumentaarium](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) hoidla teie lõikelauale.
2. Käivitage VS kood 
3. VS koodi, saate avada JSON Koodilõigud faili kas **faili**liikumine -> **eelistused** -> **Kasutaja Koodilõigud** -> **JSON**, või, valides **F1** ja tippima **eelistused** , kuni saate valida **eelistused: Koodilõigud**.

    ![eelistused Koodilõigud](./media/resource-manager-vs-code/preferences-snippets.png)

    Valige soovitud suvandid, **JSON**.

    ![Valige json](./media/resource-manager-vs-code/select-json.png)

4. Samm 1 teie kasutaja Koodilõigud faili lõplik enne faili sisu kleepimine "}" 
5. Veenduge, et selle JSON kõik on korras ja seal pole kritseldused suvalist kohta. 
6. Salvestage ja sulgege kasutaja Koodilõigud fail.

Mis on kõik, mida on vaja hakata kasutama ressursihaldur pikad. Järgmiseks tuleb Panime selle setup test.

## <a name="work-with-template-in-vs-code"></a>Malli VS koodi töötamine

Lihtsaim viis malli töötamise alustamiseks on Haarake ühest saadaval [github](https://github.com/Azure/azure-quickstart-templates) kiiresti alustada malli või kasutage mõnda oma. Saate hõlpsasti [eksportida malli](resource-manager-export-template.md) mis tahes ressursi rühm portaali kaudu. 

1. Kui malli eksporditaval ressursirühma VS koodi ekstraktitud failide avamine

    ![failide kuvamine](./media/resource-manager-vs-code/show-files.png)

2. Avage fail template.json, nii et saate seda redigeerida ja lisada täiendavaid materjale. Pärast selle **"ressursid": [** uue rea alustamiseks vajutage enter. Kui tipite **arm**, saate esitada loendi suvandite abil. Need suvandid on installitud malli pikad. See peaks välja nägema umbes järgmine: 

    ![Kuva Koodilõigud](./media/resource-manager-vs-code/type-snippets.png)

3. Valige konfigureeritav väljavõte. See artikkel ma valides **arm-ip** luua uue avaliku IP-aadressi. Sellele koma lõpusulu järele "}" vastloodud ressursi veendumaks, et teie mall on lubatud.

     ![koma lisamine](./media/resource-manager-vs-code/add-comma.png)

4. VS kood on sisseehitatud IntelliSense'i. Mallide redigeerimise ajal näitab VS koodi saadaval väärtused. Näiteks muutujate jaotise lisamiseks malli lisamine **""** (kaks jutumärke) ning valige vahel need jutumärkidega **Klahvikombinatsiooni Ctrl + tühikuklahv** . Ilmub võimalusi, sealhulgas **muutujate**abil.

    ![muutujate lisamine](./media/resource-manager-vs-code/add-variables.png)

5. IntelliSense'i osutada saadaval väärtuste või funktsioonid. Parameetri väärtus atribuudi seadmiseks **"[]"** ja **Klahvikombinatsiooni Ctrl + tühikuklahv**avaldise loomine. Saate funktsiooni nime tippimist alustada. Valige **vahekaart** , kui olete leidnud soovitud funktsiooni.

    ![parameetri lisamine](./media/resource-manager-vs-code/select-parameters.png)

6. Funktsioonis Saadaolevate parameetrite sees malli loendi kuvamiseks valige uuesti **Klahvikombinatsiooni Ctrl + tühikuklahv** .

    ![parameetri lisamine](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Kui teil on küsimusi skeemi valideerimine malli, kuvatakse teile tuttavad kritseldused redaktoris. Tõrgete ja hoiatuste loendit saate vaadata, tippides **Klahvikombinatsiooni Ctrl + Shift + M** või valida saab otsitava sõna vasakus allnurgas olekuribal.

    ![tõrked](./media/resource-manager-vs-code/errors.png)

    Valideerimine malli abil saate tuvastada süntaks probleeme; Siiski võidakse kuvada ka vigu, mida võite ignoreerida. Mõnel juhul on redaktori võrdlus malli skeemi suhtes, mis ei ole ajakohane ja seetõttu aruanded viga, isegi kui teate, et see on õige. Oletame näiteks, et funktsiooni on hiljuti lisatud ressursihaldur abil, kuid pole skeemiga värskendatud. Redaktori aruanded, vaatamata sellele, funktsioon töötab korralikult juurutamisel viga.

    ![tõrketeade](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Teie uus ressursid juurutamine

Kui teie mall on valmis, saate juurutada uue ressursside alltoodud juhiseid: 

### <a name="windows"></a>Windows

1. Avage käsuviip PowerShelli 
2. Sisselogimise tüüp: 

        Login-AzureRmAccount 

3. Kui teil on mitu tellimust, kuvada loendi tellimustest abil.

        Get-AzureRmSubscription

    Valige tellimus kasutada.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Värskendage oma parameters.json faili parameetrid
5. Malli Azure juurutamiseks Deploy.ps1 käivitamine

### <a name="osxlinux"></a>OSX/Linux

1. Avage terminal aknas 
2. Sisselogimise tüüp:

        azure login 

3. Kui teil on mitu tellimust, valige õige tellimuse abil.

        azure account set <subscriptionNameOrId> 

4. Värskendage parameters.json faili parameetrid.
5. Malli, käivitage.

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Järgmised sammud

- Mallide kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md).
- Malli funktsioonide kohta leiate teemast [Azure ressursihaldur malli funktsioonid](resource-group-template-functions.md).
- Töötamine Visual Studio kood veel näiteid, näha [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 ühendamine [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [koostamine pilveteenuste rakendusi, mille Visual Studio kood](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) . Lisateavet quickstarts HealthClinic.biz demo, leiate [Azure'i Arendaja tööriistad Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
