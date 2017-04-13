<properties
   pageTitle="Andmete toomine analüüsiteenustest Azure'i | Microsoft Azure'i"
   description="Saate teada, kuidas ühenduse loomiseks ja andmete toomine analüüsiteenustest serverist Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Andmete toomine Azure'i Analysis Services
Kui olete loonud server Azure'i ja juurutatud tabelmudel seda, on teie asutuse kasutajatele ühenduse loomiseks ja andmete analüüsimisega alustamiseks valmis.

Azure'i Analysis Services toetab klientrakenduse kaudu, kasutades [värskendada objekti mudelite](#client-libraries); TOM, AMO, Adomd.Net või MSOLAP, xmla serveriga ühendust luua. Näiteks Power BI, Power BI Desktopi, Exceli või mis tahes muu kliendi rakendus, mis toetab objekti mudelid.

## <a name="server-name"></a>Serveri nimi
Azure'i ettevõtte analüüsiteenuste serveris loomisel saate määrata kordumatu nimi ja regioon, kus server on luua. Kui ühendus, mis määrab serveri nime, serveri nime kava on:
```
<protocol>://<region>/<servername>
```
 Kui protokoll stringi **asazure**, piirkond on piirkond, kus on loodud server Uri (näiteks Lääne USA, westus.asazure.windows.net) ja serveri nimi on teie piirkonnas kordumatu serveri nimi.

## <a name="get-the-server-name"></a>Serveri nime hankimine
Enne, kui ühendate, vajate serveri nime. **Azure'i** portaalis > Serveri > **Ülevaade** > **Serveri nimi**, kopeerige kogu serveri nime. Kui ettevõtte teised kasutajad on ühenduse see server liiga, soovite nendega jagada selle serveri nime. Kui soovite määrata serverinime, tuleb kasutada kogu tee.

![Serveri nimi Azure hankimine](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Ühenduse loomine rakenduses Power BI Desktopi

> [AZURE.NOTE] See funktsioon on eelvaade.

1. [Power BI Desktopi](https://powerbi.microsoft.com/desktop/), klõpsake nuppu **Too andmed** > **andmebaaside** > **Azure Analysis Services**.

2. **Server**, kleepimine lõikelaualt serveri nime.

3. **Andmebaasi**, kui te ei tea nime tabelmudeli andmebaasi või perspektiivi, mida soovite ühendada, kleepige see siin. Muul juhul saate saate jätke see väli tühjaks. Saate valida andmebaasi või perspektiivi järgmisel kuval.

4. Lahku **ühendamine live** vaikesuvand valitud, seejärel vajutage **ühenduse loomine**. Kui teil palutakse sisestada konto, sisestage oma ettevõtte konto.

5. **Navigaator**, laiendage server, siis valige mudel või perspektiivi, mida soovite ühendada, siis klõpsake nuppu **Ühenda**. Ühe klõpsuga mudel või perspektiivi kuvatakse kõigi objektide vaates.


## <a name="connect-in-power-bi"></a>Ühenduse loomine Power BI
1. Luua Power BI Desktopi faili, mis on reaalajas ühenduse mudelisse serverisse.

2. [Power BI](https://powerbi.microsoft.com), klõpsake nuppu **Too andmed** > **failid**. Leidke ja valige fail.


## <a name="connect-in-excel"></a>Ühenduse loomine Excelis
Azure'i analüüsiteenuste serveri Exceli ühenduse toetatakse rakenduses Excel 2016 andmete hankimine või varasemates versioonides Power Query abil. Nõutav on [MSOLAP.7 pakkuja](https://aka.ms/msolap) . Ühenduse loomine Power Pivoti tabeli impordiviisardi abil ei toetata.

1. Rakenduses Excel 2016, klõpsake **andmete** lindil nuppu **Too välisandmed** > **Muust allikast** > **Teenusest Analysis Services**.

2. Andmeühendusviisardi, **serverinime**, kleepimine lõikelaualt serverinime. Seejärel **sisselogimismandaate**, valige **kasutada järgmist kasutajanime ja parooli**, ja seejärel tippige ettevõtte kasutaja nimi, näiteks nancy@adventureworks.com, ja parool.

    ![Ühenduse loomine rakenduses Excel sisselogimine](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. **Valige andmebaas ja tabel**, valige andmebaas ja mudel või perspektiivi ja klõpsake siis nuppu **valmis**.

    ![Ühenduse loomine Exceli valige mudel](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Ühendusstring
Azure'i analüüsiteenuste tabelina esitatud objektimudeli abil ühendamisel kasutada ühenduse stringi failivormingud:

###### <a name="integrated-azure-active-directory-authentication"></a>Integreeritud Azure Active Directory autentimine
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Integreeritud autentimine on kättesaamine Azure Active Directory mandaat vahemälu, kui see on saadaval. Kui ei, siis kuvatakse Azure sisselogimisaknas.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory autentimine kasutajanime ja parooliga
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Kliendi teegid
Kui ühendate Azure'i analüüsiteenuste Excelist või muude liideste näiteks TOM AsCmd, ADOMD.NET, peate installige uusim pakkuja kliendi teegid. Saada uusimad:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Järgmised sammud
[Oma serveri haldamine](analysis-services-manage.md)
