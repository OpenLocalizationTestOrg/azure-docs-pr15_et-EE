<properties
   pageTitle="Andmeallikatega ühenduse loomise | Microsoft Azure'i"
   description="Esiletõstmine avastanud, Azure'i andmekataloogi andmeallikatega ühenduse loomise juhise."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Kuidas andmeallikatega ühenduse loomine

## <a name="introduction"></a>Sissejuhatus
**Microsoft Azure'i andmekataloogi** on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteem. Teisisõnu on **Azure andmekataloogi** teave aitab tuvastada, mõista ja andmeallikate ja aidata ettevõtted abil saate oma olemasolevad andmed rohkem väärtust. Oluline osa sel juhul kasutab andmed – kui kasutaja leiab andmeallika ja mõistab selle eesmärk, järgmise sammuna tuleb luua ühendus andmeallikaga panna oma andmeid, mida kasutada.

## <a name="data-source-locations"></a>Andmete allikas asukohad
Andmete allikas registreerimisel **Azure'i andmekataloogi** saab metaandmete andmeallika kohta. Selle metaandmete sisaldab andmeallika asukoha üksikasju. Asukoha üksikasjad erinevad andmeallikast andmeallikas, kuid see alati sisaldab ühenduse vajalik teave. Näiteks asukoha jaoks SQL serveri tabel sisaldab serverinime, andmebaasinime, skeemi nimi ja tabeli nimi, SQL serveri aruandlusteenuste aruande asukoht sisaldab serveri nime ja teed aruanne. Muud andmeallikatüüpide on struktuuri ja allika süsteemi võimaluste asukohad.

## <a name="integrated-client-tools"></a>Integreeritud kliendi tööriistad
Lihtsaim viis andmeallikaga ühenduse on kasutada on "Ava. …" menüü **Azure'i andmekataloogi** portaalis. See menüü kuvatakse valitud andmete varade ühenduse suvandite loend.
Paani vaikevaade kasutamisel see menüü on saadaval iga paani.

 ![SQL serveri tabeli avamise Exceli andmete varade paan](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Loendivaate kasutamisel menüü on saadaval otsinguriba portaali akna ülaservas.

 ![SQL serveri aruandlusteenuste aruande avamine Report Manageri otsinguriba kaudu](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Toetatud klientrakendused
Kui kasutate funktsiooni "Ava. …" menüü Azure'i andmekataloogi portaalis õige klientrakendusega andmeallikate jaoks peab olema installitud klientarvutis.

| Avatud rakenduses | Faililaiend / protokoll | Toetatud rakenduste versioonid |
| --- | --- | --- |
| Exceli | odc | Excel 2010 või uuemas versioonis |
| Excel (ülal 1000) | odc | Excel 2010 või uuemas versioonis |
| Power Query | .xlsx | Excel 2016 või Excel 2010 või Excel 2013 Power Query for Exceli lisandmoodul on installitud
| Power BI Desktopi | .pbix | Power BI Desktopi juuli 2016 või uuem versioon |
| SQL Server Data Tools | vsweb: / / | Visual Studio 2013 värskenduse 4 või uuem versioon ja SQL serveri instrumentaarium installitud |
| Aruandehalduri | http:// | [Brauserinõuded SQL serveri aruandlusteenuste](https://technet.microsoft.com/en-us/library/ms156511.aspx) näha |

## <a name="your-data-your-tools"></a>Andmete oma tööriistad
Praegu valitud andmete varade tüüpi sõltub saadaval menüüs Suvandid. Muidugi pole kõigi võimalike tööriistade kaasatakse funktsiooni "avatud. …" menüü, kuid see on endiselt hõlpsalt ühenduse andmeallikaga, mis tahes kliendi tööriista abil. **Azure'i andmekataloogi** portaalis andmete varade valimisel täieliku asukoht kuvatakse paanil atribuudid.

 ![SQL Serveri tabeliga ühenduse teave](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Ühenduse teave üksikasjad on erinevad andmeallika tüüp andmeallika tüüp, kuid portaalis sisalduv teave annab teile kõike, mida on vaja luua ühendus andmeallikaga, mis tahes kliendi tööriistas. Kasutajaid saate kopeerida ühenduse üksikasju andmeallikaid, mida nad on avastanud, kasutades **Azure andmekataloogi**, nad saavad valitud tööriistas andmetega töötamiseks.

## <a name="connecting-and-data-source-permissions"></a>Ühenduse loomine ja andmete allikas õigused
Kuigi **Azure'i andmekataloogi** teeb andmeallikate leitavaks, jääb ise andmetele juurdepääsu andmete allikas omanik või administraator kontrolli all. **Azure'i andmekataloogi** andmeallikas avastamine ei anna kasutaja ise andmeallikale juurdepääsu mis tahes õigused.

Lihtsam kasutajatele, kes andmeallika avastamine, kuid teil pole õigust pääsevad oma andmetele juurde, et kasutajad saavad sisestada teavet atribuudi juurdepääsutaotluse kui marginaalid lisanud andmeallika. Teabe – sealhulgas linke protsessi või kontaktisiku andmete allikas võimaldamine – siin on esitatud portaalis andmete allikas asukohateavet kõrval.

 ![Ühenduseteavet koos taotluse juurdepääsu juhised](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Kokkuvõte
Registreerimise andmeallika **Azure'i andmekataloogi** teeb andmete leitavaks kopeerides strukturaalset ja kirjeldav metaandmeid andmeallikast kataloogi teenus. Pärast andmeallika registreeritud ja avastanud, kasutajatel luua andmeallika **Azure'i andmekataloogi** portaalist "Ava. …" " menüü või nende andmete tööriistade valiku abil.

## <a name="see-also"></a>Vt ka
- [Alustamine Azure'i andmekataloogi](data-catalog-get-started.md) õpetuse üksikasjalike üksikasju andmeallikatega ühenduse loomise kohta.
