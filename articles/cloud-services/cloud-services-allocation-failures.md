<properties
    pageTitle="Pilveteenuses eraldatud tõrge tõrkeotsing | Microsoft Azure'i"
    description="Kui juurutate pilveteenustega Azure tõrkeotsingu jaotamine nurjus"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Kui juurutate pilveteenustega Azure tõrkeotsingu jaotamine nurjus

## <a name="summary"></a>Kokkuvõte
Kui juurutada eksemplarid pilveteenusesse või lisada uue veebis või töötaja rolli aknad, Microsoft Azure'i eraldab Arvuta ressursid. Aeg-ajalt kuvatakse tõrgete täites need toimingud juba enne seda, kui jõuate Azure tellimuse piirangud. Selles artiklis selgitatakse mõningaid levinud jaotamise ebaõnnestumisi põhjused ja soovitab võimalike parandamise. Teave ka võib olla kasulik, kui plaanite oma teenuste.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Tausta – eraldatud tööpõhimõtted
Azure'i andmekeskuste serverid on liigendatud kogumite sisse. Uue cloud eraldatud teenusetaotlus on proovitakse mitme rühmades. Kui esimene eksemplar on juurutatud pilveteenusesse (jaotises kas lavastus või tootmise), mis teenus cloud saab arvutikobaras kinnitatud. Mis tahes täiendavaid juurutuste jaoks pilveteenusesse ilmneb sama kobar. Selles artiklis me kuvatakse viidata see nagu "kinnitatud arvutikobaras". Diagramm 1 all näitab puhul tavaline eraldatud, mis on proovitakse mitme rühmades; 2 diagramm näitab jaotamine täheregistri mis on kinnitatud kobar 2, kuna see on, kus olemasolevad pilvepõhise teenuse CS_1 majutatakse.

![Eraldatud skeem](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Põhjus jaotamine nurjus?
Kui arvutikobaras on kinnitatud taotluse eraldatud, on suurem võimalus ei tuvastanud tasuta ressursid on saadaval ressursivaru arvutikobaras piiratud. Lisaks kui arvutikobaras on kinnitatud eraldatud kutse, kuid ressursi teie soovitud tüüpi ei toeta seda kobar, teie taotlus nurjub, isegi siis, kui klaster on tasuta ressurss. Diagramm 3 all näitab juhul, kui kinnitatud jaotust nurjub, kuna ainult testversiooni kobar ei ole tasuta ressursse. 4 diagramm näitab juhul, kui kinnitatud jaotust nurjub, kuna ainult testversiooni kobar ei toeta nõutud VM suurus, isegi kui klaster on tasuta ressursid.

![Kinnitatud jaotamine nurjus](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Eraldatud nurjumisest pilveteenustega tõrkeotsing
### <a name="error-message"></a>Tõrketeade
Teile võidakse kuvada järgmine tõrketeade:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Levinud probleemid
Siin on eraldatud levinud stsenaariumi, mis on eraldatud taotlus olema kinnitatud ühe kobar põhjustada.

- Juhul, kui mõnda pilveteenusesse on juurutamine kummagi pesa lavastus pesa - kasutusele võtavad, siis kogu pilveteenuses on kinnitatud teatud kobar.  See tähendab, et kui tootmise pesa on juurutamine juba olemas, siis uus lavastus juurutamise ainult saab eraldada sama nimega tootmise pesa klaster. Kui klaster on peaaegu võimsus, ei pruugi kutse.

- Skaleerimist – uued eksemplarid lisamine olemasoleva pilvepõhise teenuse eraldavad sama kobar.  Väike skaleerimist taotlusi saab tavaliselt jaotada, kuid mitte alati. Kui klaster on lähenemas võimsus, ei pruugi kutse.

- Osaleja rühma - uue juurutamise mõnda tühja pilveteenusesse saab jaotada, mis tahes klaster selle piirkonna struktuuri kui pilveteenusesse kinnitatakse osaleja rühma. Sama klaster proovitakse juurutuste samade osaleja rühma. Kui klaster on peaaegu võimsus, ei pruugi kutse.

- Osaleja rühma vNet - vanemate virtuaalse võrgu seotud osaleja piirkondade asemel rühmadele ja pilveteenustega nende virtuaalse võrkudes peaks olema kinnitatud osaleja rühma kobar. Seda tüüpi virtuaalse võrgu juurutuste proovitakse kinnitatud klaster. Kui klaster on peaaegu võimsus, ei pruugi kutse.

## <a name="solutions"></a>Lahendused

1. Uue pilveteenusesse ümberkorraldamine – see lahendus on tõenäoliselt kõige edukamad, kuna see võimaldab valida selle piirkonna kõik kogumite platvormi.

    - Uue pilveteenusesse töökoormus juurutamine  

    - Värskendage CNAME-i või kirje liikluse osutamiseks uue pilveteenuses

    - Kui null liikluse saab vana saidi, saate kustutada vana pilveteenuses. See lahendus peaks tekkida null tööseisakute.

2. Tootmise ja lavastus slots kustutamine – see lahendus säilitab teie olemasoleva DNS-i nimi, kuid see võib põhjustada tööseisakute rakenduse.

    - Kustuta tootmise ja lavastus olemasoleva pilvepõhise teenuse Ava, et pilveteenusesse on tühi, ja seejärel

    - Luua uue juurutamise olemasoleva pilveteenusesse. See proovitakse uuesti eraldatud kõik kogumite piirkonna kohta. Veenduge, et pilveteenusesse pole tunniga seotud osaleja rühma.

3. Reserveeritud IP - see lahendus säilitab teie olemasoleva IP aadress, kuid põhjustab tööseisakute rakenduse.  

    - Looge ReservedIP, teie olemasoleva juurutamiseks PowerShelli abil

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Järgige #2 ülevalt, veendudes, et määrata uue ReservedIP teenuse CSCFG.

4. Osaleja rühma uue juurutuste - eemaldamine osaleja rühmad on soovitatav enam. Järgige juhiseid # 1 ülaltoodud juurutada uue pilveteenuses. Veenduge, et pilveteenuses pole osaleja nimel.

5. Teisendage piirkondliku virtuaalse võrgu - lugege teemat [kuidas migreerida osaleja rühmade piirkondliku virtuaalse võrku (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
