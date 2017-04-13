<properties
   pageTitle="Täiendamine on Azure teenuse struktuuri kobar | Microsoft Azure'i"
   description="Versioonitäienduse teenuse struktuuri kood ja/või konfiguratsiooni, mis töötab teenuse struktuuri kobar, sh kobar värskendamise režiim, üleminekut serdid, lisades rakenduse pordid, tehes OS plaastrid, seada jne. Mis võib juhtuda, kui uuendamine teostamise?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Uuele versioonile on Azure teenuse struktuuri kobar

> [AZURE.SELECTOR]
- [Azure'i kobar](service-fabric-cluster-upgrade.md)
- [Autonoomse kobar](service-fabric-cluster-upgrade-windows-server.md)

Mis tahes tänapäevane süsteemi laiendatavus kavandamine on oluline toote pikaajalise edu saavutamisel. Mõne Azure teenuse struktuuri klaster on ressurss, mida te oma, kuid on osaliselt haldab Microsoft. Selles artiklis kirjeldatakse, mida hallatakse automaatselt ja millised saab ise konfigureerida.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Struktuuri versioon, mis töötab klaster juhtimine

Saate seada klaster struktuuri automaatne uuendamine, vastu võtta, kui Microsoft välja uus versioon või valige soovitud klaster olevat toetatud struktuuri versiooni valimine.

Saate seda teha, "upgradeMode" kobar konfiguratsiooni säte portaalis või ressursihaldur kasutamise ajal loomise või uuem versioon reaalajas kobar 

>[AZURE.NOTE] Veenduge, et hoida klaster versiooniga toetatud struktuuri alati. Kui me teatada vabastamist uue versiooni teenuse struktuuri, eelmine versioon on märgitud tugi lõppu vähemalt 60 päeva pärast. kuvatakse uus väljalasete on teada [teenuse struktuuri meeskonna ajaveebist](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Uus versioon on saadaval, siis valida. 

14 päeva jooksul enne klaster töötab, väljaandmist seisund sündmuse luuakse, mis viib klaster olekusse hoiatus seisund. Klaster jääb hoiatus olekus kuni toetatud struktuuri versiooni.


### <a name="setting-the-upgrade-mode-via-portal"></a>Versioonitäienduse režiimi portaali kaudu seadmine 

Saate seada klaster automaatselt või käsitsi loomisel klaster.

![Create_Manualmode][Create_Manualmode]

Saate seada klaster automaatselt või käsitsi kui kohta reaalajas kobar, kasutades kogemusi haldamine. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Klaster, mis on seatud käsitsi režiimi portaali kaudu uuele versioonile üleminekut.
 
Uue versiooni täiendamiseks peate tegema on saadaval versioon valige rippmenüüst ja salvestamine. Struktuuri versiooniuuenduse saab käivitati automaatselt. Kobar seisund poliitikate (sõlm seisundi ja seisundi kombinatsiooni kõik Helistamine ja klaster) järgimine versiooni täiendamise käigus.

Kui kobar seisund poliitikate pole täidetud, on uuele versioonile tagasi pöörata. Kerige lugeda Lisateavet selles dokumendis kohta, kuidas kohandatud seisund poliitika määramine. 

Kui on fikseeritud tulemuseks tagasipööramine probleeme, peate kontaktiga täiendamise uuesti, enne kui samu juhiseid järgides.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Versioonitäienduse režiimi kaudu ressursihaldur malli seadmine 

"UpgradeMode" konfiguratsiooni lisamine Microsoft.ServiceFabric/clusters ressursside määratlemine ja "clusterCodeVersion" määramine toetatud struktuuri versioone, nagu allpool näidatud ja seejärel malli. Sobivad väärtused "upgradeMode" on "Käsitsi" või "Automaatne"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Klaster, mis on seatud käsitsi režiim kaudu ressursihaldur Mall uuele versioonile üleminekut.
 
Kui klaster on käsitsi režiim, uue versiooni, muuta "clusterCodeVersion" toetatud versiooni ja juurutada. Juurutamise malli, saab algab struktuuri versiooniuuenduse automaatselt käivitati. Kobar seisund poliitikate (sõlm seisundi ja seisundi kombinatsiooni kõik Helistamine ja klaster) järgimine versiooni täiendamise käigus.

Kui kobar seisund poliitikate pole täidetud, on uuele versioonile tagasi pöörata. Kerige lugeda Lisateavet selles dokumendis kohta, kuidas kohandatud seisund poliitika määramine. 

Kui on fikseeritud tulemuseks tagasipööramine probleeme, peate algatada täiendamise uuesti, enne kui samu juhiseid järgides.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Kõik saadaolevat loendi saamiseks kõik keskkonnas antud tellimusele

Käivitage järgmine käsk ja saate väljund umbes järgmine.

"supportExpiryUtc" ütleb oma kui antud väljaanne aegub või on aegunud. Uusim versioon pole kehtiv kuupäev – see on väärtus "9999-12-31T23:59:59.9999999", mis tähendab lihtsalt aegumise kuupäeva veel pole määratud.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Struktuuri versioonitäienduse käitumine klaster täiendamine režiim on automaatne

Microsoftil struktuuri kood ja konfiguratsiooni, mis on Azure kobar töötab. Me sooritada jälgida Sirvimisajaloo kustutamine tarkvara on vastavalt vajadusele. Nende uuendamine võib olla kood, konfigureerimine või mõlemad. Veenduge, et rakenduse kannatab ei mõjuta või minimaalne mõju tõttu nende uuendamine, saame tehke järgmised etapid uuendamine.

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Etapp 1: Versiooniuuenduse sooritatakse abil kõik kobar rakendamist

Selles etapis uuendamine jätkake korraga üks versiooniuuenduse domeeni ja klaster töötavate rakenduste jätkatakse ilma mis tahes. Kobar seisund poliitikate (sõlm seisundi ja seisundi kombinatsiooni kõik Helistamine ja klaster) järgimine versiooni täiendamise käigus.

Kui kobar seisund poliitikate pole täidetud, on uuele versioonile tagasi pöörata. E-posti saadetakse siis tellimuse omanik. E-posti sisaldab järgmist teavet:

- Teatis tuli kobar versiooniuuenduse tagasi pöörata.
- Soovitatud parandusmeetmete, kui see on olemas.
- (N) seni, kuni me käivitada etapp 2 päevade arv.

Me proovige käivitada sama uuele versioonile paar korda juhuks, kui mõni uuendamine nurjus taristu põhjustel. Pärast n päeva kuupäevast meilisõnum saadeti, jätkake me etapp 2.

Kui kobar seisund poliitika on täidetud, uuele versioonile ei õnnestunud ja lõpetatuks märgitud. See võib juhtuda ajal algsete versiooniuuendus või mis tahes versioonitäienduse kordused selles etapis. Ei ole e-posti, eduka Käivita. See on vältimiseks saatmist saate liiga palju e-kirju--saavad e-posti tuleks käsitleda tavaline erand. Loodame kõige kobar uuendamine õnnestub oma rakenduste-saadavus mõjutamata.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Etapp 2: Versiooniuuenduse sooritatakse seisund vaikepoliitikaid ainult abil

Seisund poliitikate selles etapis määratakse nii, et rakendused, mis olid terve versioonitäienduse alguses arv ei muutu versiooniuuenduse kestel. Etapp 1, nagu etapp 2 täienduste jätkake korraga üks versiooniuuenduse domeeni ja klaster töötavate rakenduste jätkatakse ilma mis tahes. Kobar seisund poliitikate (sõlm seisundi ja seisundi kombinatsiooni kõik Helistamine ja klaster) järgimine versioonitäienduse kestel.

Kui kobar seisund poliitikate tegelikult on täidetud, on uuele versioonile tagasi pöörata. E-posti saadetakse siis tellimuse omanik. E-posti sisaldab järgmist teavet:

- Teatis tuli kobar versiooniuuenduse tagasi pöörata.
- Soovitatud parandusmeetmete, kui see on olemas.
- (N) seni, kuni me käivitada etapp 3 päevade arv.

Me proovige käivitada sama uuele versioonile paar korda juhuks, kui mõni uuendamine nurjus taristu põhjustel. Meeldetuletuse meilisõnumi saatmise paar päeva enne n päeva. Meilisõnum saadeti kuupäevast n päeva pärast me jätkata etapp 3. E-kirju, mida meie teile saadame etapp 2 tuleb oluliselt ja parandusmeetmete tuleb teha.

Kui kobar seisund poliitika on täidetud, uuele versioonile ei õnnestunud ja lõpetatuks märgitud. See võib juhtuda ajal algsete versiooniuuendus või mis tahes versioonitäienduse kordused selles etapis. Ei ole e-posti, eduka Käivita.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Etapp 3: Versiooniuuenduse sooritatakse agressiivne rakendamist abil

Nende poliitikat ning see etapp on suunatud seisundi rakenduste asemel versiooniuuenduse lõpuleviimiseks. Selles etapis lõpuks väga vähe kobar uuendamine. Kui klaster saab selle etapi, on hea võimalus, et rakenduse muutub vigane ja/või kättesaadavus kaotsi minna.

Sarnaselt kahest etapist, etapp 3 täienduste nüüd üks versiooniuuenduse domeeni korraga.

Kui kobar seisund poliitikate pole täidetud, on uuele versioonile tagasi pöörata. Me proovige käivitada sama uuele versioonile paar korda juhuks, kui mõni uuendamine nurjus taristu põhjustel. Pärast seda, klaster on kinnitatud, nii et enam saab tugi ja/või uuendamine.

E-posti, kus see teave saadetakse tellimuse omanik, koos parandusmeetmed. Me ei oota mis tahes kogumite olekusse, kus on nurjunud etapp 3 saada.

Kui kobar seisund poliitika on täidetud, uuele versioonile ei õnnestunud ja lõpetatuks märgitud. See võib juhtuda ajal algsete versiooniuuendus või mis tahes versioonitäienduse kordused selles etapis. Ei ole e-posti, eduka Käivita.

## <a name="cluster-configurations-that-you-control"></a>Saate määrata kuvatakse kobar

Lisaks määrata klaster täiendamine režiimi, järgnevalt kuvatakse reaalajas klaster saate muuta.

### <a name="certificates"></a>Serdid

Saate lisada uusi või kustutada hõlpsasti kobar ja kliendi portaali kaudu serdid. [Üksikasjalikud juhised selle dokumendi](service-fabric-cluster-security-update-certs-azure.md) viidata

![Kuvatõmmis, mis näitab serdi thumbprints Azure'i portaalis.][CertificateUpgrade]


### <a name="application-ports"></a>Rakenduse pordid

Saate muuta rakenduse pordid, muutes laadi koormusetasakaalustusteenuse ressursi atribuudid, mis on seotud sõlm tüüp. Saate kasutada portaali või saate ressursihaldur PowerShelli otse.

Avage uus porti kõik VMs sõlm tüüp, tehke järgmist.

1. Lisage uue juures vastav laadi koormusetasakaalustusteenuse.

    Kui olete juurutanud klaster portaali kaudu, koormus soolise on nimega "LB-nimi, rühma-NodeTypename ressursi" sõlm jaoks eraldi. Kuna laadi koormusetasakaalustusteenuse nimed on kordumatud üksnes ressursirühma, on parim kui otsite neid jaotises kindla ressursirühma.

    ![Kuvatõmmis, mis näitab lisamine laadi koormusetasakaalustusteenuse portaalis on juures.][AddingProbes]

2. Laadi koormusetasakaalustusteenuse lisada uus reegel.

    Uue reegli lisamine sama laadi koormusetasakaalustusteenuse, kasutades eelmises etapis loodud juures.

    ![Uue reegli lisamine laadi koormusetasakaalustusteenuse portaalis.][AddingLBRules]


### <a name="placement-properties"></a>Paigutuse atribuudid

Iga tüüpi sõlm, saate lisada kohandatud paigutuse atribuudid, mida soovite kasutada oma rakendustes. NodeType on vaikimisi atribuut kasutatavad konkreetselt lisamata.

>[AZURE.NOTE] Paigutuse piiranguid, sõlm atribuudid ja nende määratlemise kohta täpsema teabe saamiseks vaadake jaotist "Paigutuse piiranguid ja sõlm atribuudid" teenuse struktuuri kobar ressursihaldur dokumendis kohta, [Mis kirjeldab teie kobar](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="capacity-metrics"></a>Võimsuse mõõdikud

Iga tüüpi sõlm, saate lisada kohandatud võimsus mõõdikute, mida soovite kasutada aruande laadi rakendusi. Aruandele võimsus mõõdikute kasutamise kohta lisateavet laadimine, vaadake teenuse struktuuri kobar ressursihaldur dokumentide [Kirjeldab teie kobar](service-fabric-cluster-resource-manager-cluster-description.md) ja [mõõdikute ja laadi](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Struktuuri täiendamise sätted – seisund poliitikat

Saate määrata kohandatud seisund poliitikat struktuuri üleminek versioonile. Kui olete seadistanud klaster struktuuri automaatne uuendamine, siis need poliitikad saada rakendatud struktuuri automaatne uuendamine 1 etapi.
Kui seate klaster jaoks käsitsi struktuuri uuendamine, siis need poliitikad saada rakendatud iga kord, kui valite käivitamise süsteemi võrgukoosolekuga struktuuri versioonitäienduse klaster uus versioon. Kui teil alistada poliitikatega, kasutatakse vaikesätteid.

Saate määrata kohandatud seisund poliitikate või vaadake üle kehtivad sätted jaotises "struktuuri uuendamine" tera, valides täpsemalt täiendamise sätted. Järgmisel pildil kohta, kuidas vaadata. 

![Kohandatud seisund poliitikate haldamine][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Klaster struktuuri sätete kohandamine

Vaadake [struktuuri kobar struktuuri Teenusesätted](service-fabric-cluster-fabric-settings.md) kohta, mida ja kuidas saate neid kohandada.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>OS laigud VMs, mis moodustavad klaster

See funktsioon on plaanitud tulevikus automatiseeritud funktsioonina. Kuid praegu teie vastutate teie VMs paik. Tehke see ühe VM korraga nii, et te ei võta rohkem kui ühe korraga.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>VMs, mis moodustavad klaster OS uuendamine

Kui peate täiendama virtuaalmasinates klaster ning OS pilt, peate tegema selle ühe VM korraga. Teie vastutate seda versioonitäiendust--on praegu pole see automatiseerimine.

## <a name="next-steps"></a>Järgmised sammud
- Siit saate teada, kuidas kohandada teatud [struktuuri kobar struktuuri Teenusesätted](service-fabric-cluster-fabric-settings.md)
- Siit saate teada, kuidas [mastaapimiseks klaster ja vähendamine](service-fabric-cluster-scale-up-down.md)
- Lisateavet [rakenduse uuendused](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG