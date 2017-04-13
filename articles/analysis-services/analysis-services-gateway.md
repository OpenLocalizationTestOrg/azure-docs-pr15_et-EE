<properties
   pageTitle="Kohapealse andmete lüüsi | Microsoft Azure'i"
   description="Mõne asutusesisese lüüsi on vajalik, kui analüüsiteenuste serveri Azure on kohapealse andmeallikatega ühenduse loomine."
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

# <a name="on-premises-data-gateway"></a>Kohapealse andmete lüüsi

Kohapealse andmete lüüsi toimib bridge, pakkudes turvaline andmeedastus kohapealse andmeallikate ja Azure analüüsiteenuste serveri pilveteenuses vahel.

Lüüs on installitud arvuti võrku. Ühe lüüsi peab olema installitud iga Azure'i analüüsiteenuste serveri, mis on Azure tellimuse jaoks. Näiteks, kui teil on kaks serverid Azure tellimuse, mis kohapealse andmeallikatega ühenduse loomine, lüüsi peab olema installitud võrgu kahes eraldi arvutis.

## <a name="requirements"></a>Nõuded

**Miinimumnõuded:**

- .NET 4.5 framework
- 64-bitine Windows 7 ja Windows Server 2008 R2 (või uuem versioon)

**Soovitatav:**

- 8 core CPU
- 8 GB mälu
- 64-bitine versioon Windows 2012 R2 (või uuem versioon)

**Oluline teave:**

- Lüüsi ei saa installida domeenikontrolleri.

- Ühes arvutis saab installida ainult üks lüüsi.

- Installige arvutisse, mis jääb ja unerežiimile lüüsi. Kui arvuti on välja lülitatud, ei saa teie andmete värskendamine kohapealse andmeallikatega ühenduse Azure'i analüüsiteenuste serveri.

- Lüüsi ei traadita võrku ühendatud arvutisse installida. Jõudluse võib olla vähendatav.

- Soovite muuta serveri nimi lüüsi, mis on juba konfigureeritud, peate uuesti ja uue lüüsi konfigureerimine.

- Mõnel juhul ühenduse andmeallikate kohalikke pakkujad, nt SQL serveri Omakliendi (SQLNCLI11) tabelmudelites võib ilmneda tõrge. Lisateavet leiate teemast [andmeallika ühendusi](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Kohapealsete toetatud andmeallikad
Eelvaate jaoks lüüsi toetab seoseid Azure'i analüüsiteenuste serveri ja kohapealse järgmiste andmeallikatega:

- SQL serveri
- SQL-i andmebaas
- APPS
- Oracle'i
- Teradata


## <a name="download"></a>Laadi alla
 [Lüüsi allalaadimine](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Installimine ja konfigureerimine

1. Käivita häälestus.

2. Valige soovitud installi asukoht ja litsentsitingimustega.

3. Azure'i sisse logida.

4. Azure'i analüüsi serverinime määramiseks. Saate määrata ainult ühe serveri lüüsi kohta. Klõpsake nuppu **Konfigureeri** ja teil on korras.

    ![Azure'i sisse logida](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Kuidas see toimib
Lüüsi töötab Windows teenus, **kohapealse andmete lüüsi**, arvutisse sisse oma asutuse võrku. Lüüsi jaoks Azure'i analüüsiteenuste installimist põhineb samas lüüsis kasutatakse muude teenustega, nagu Power BI, kuid mõned erinevused, kuidas see on konfigureeritud.

![Kuidas see toimib](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Päringute ja andmete meilivoo töö umbes järgmine:

1.  Päringu on loodud pilveteenuses krüptitud identimisteabega kohapealse andmeallika jaoks. Seejärel saadetakse töötlemiseks lüüsi järjekorda.

2.  Lüüsi pilveteenuses analüüsib päringu ja sunnib taotluse [Azure'i teenus siini](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Kohapealse andmete lüüsi küsitlused Azure'i teenus siini jaoks ootel kutsed.

4.  Lüüsi saab päringu dekrüptib mandaadi ja loob ühenduse mandaadi andmeallikad.

5.  Lüüsi saadab päringu täitmise andmeallikas.

6.  Tulemused saadetakse andmeallikast, tagasi, et lüüsi, ja seejärel peale pilveteenusesse.

## <a name="windows-service-account"></a>Windowsi teenusekonto

Kohapealse andmete lüüsi on konfigureeritud kasutama *NT SERVICE\PBIEgwService* Windowsi teenuse sisselogimise identimisteabega. Vaikimisi on sisselogimise õigus teenust; arvuti, mida te installite lüüsi kontekstis. Selle mandaati pole sama kontoga ühenduse loomiseks kohapealse andmeallikate või Azure'i kontosse.  

Kui teil tekib probleeme koos oma puhverserveri autentimine tõttu, mida soovite Windowsi teenusekonto kasutaja domeeni muutmine või hallatavate teenuse konto.

## <a name="ports"></a>Pordid

Lüüsi loob väljamineva ühenduse Azure'i teenus siini. See edastab Väljaminev pordid kohta: TCP 443 (vaikeväärtus) 5671, 5672, 9350 9354 kaudu.  Lüüsi ei nõua sissetuleva meili pordid.

See on soovitatav nimekiri IP-aadressid oma andmepiirkond tulemüüri. Saate alla laadida [Microsoft Azure'i andmekeskuse IP-loend](https://www.microsoft.com/download/details.aspx?id=41653). See loendisse värskendatakse iga nädal.

> [AZURE.NOTE]  Azure'i andmekeskuse IP loendis IP-aadresse pole CIDR märke. Näiteks ei tähenda 10.0.0.0/24 10.0.0.0 10.0.0.24 kaudu. Lugege lisateavet [CIDR märke](http://whatismyipaddress.com/cidr).

Täielikult kvalifitseeritud domeeninimede kasutatavaid lüüsi on järgmised.

|Domeeninimed|Väljaminev pordid|Kirjeldus|
|---|---|---|
|*. powerbi.com lehe kaudu|80|HTTP installer alla laadida.|
|*. powerbi.com lehe kaudu|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671-5672|Täiustatud sõnumi, järjekorda paneku protokolli (AMQP)|
|*. servicebus.windows.net|443, 9350-9354|Klõpsake teenuse siini Relay (nõuab 443 juurdepääsu reguleerimine Turbeloa omandamise) TCP kuulajatele|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. core.windows.net|443|HTTPS|
|login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Kasutada kontrollimiseks Interneti-ühenduse lüüs on Power BI teenus kättesaamatu.|
|*.microsoftonline-p.com|443|Kasutada autentimiseks konfiguratsioonist.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Sunnib HTTPS Azure'i teenus siini suhtlemine

Saate jõustada lüüsi suhelda Azure'i teenus siini HTTPS asemel otse TCP; kuid see võib oluliselt vähendada jõudluse. Peate *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* faili muutmiseks. Välja väärtust muuta `AutoDetect` et `Https`. See fail asub vaikimisi *C:\Program Files\On kohapealsete andmete*Gateway.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Tõrkeotsing
Kaitstud, kasutatakse teie asutusesisese andmeallikatega ühenduse loomine Azure Analysis Services kohapealse andmete lüüs on kasutada Power BI samas lüüsis.

Kui teil on probleeme installimisel ja konfigureerimisel lüüsi, kindlasti leiate [Power BI lüüsi tõrkeotsing](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Kui arvate, et teil on probleeme oma tulemüüri, leiate järgmistest jaotistest tulemüüri või puhverserveri.

Kui arvate, et olete tekib probleeme puhverserveri, lüüsi, kuvada [Power BI lüüside puhverserveri sätete konfigureerimine](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Järgmised sammud
- [Analüüsiteenuste haldamine](analysis-services-manage.md)
- [Andmete toomine Azure'i Analysis Services](analysis-services-connect.md)
