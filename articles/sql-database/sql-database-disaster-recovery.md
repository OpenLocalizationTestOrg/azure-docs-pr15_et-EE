<properties
   pageTitle="SQL-andmebaasi avariitaastet | Microsoft Azure'i"
   description="Saate teada, kuidas andmebaasi taastamine piirkondliku andmekeskuse katkestuste või tõrge Azure SQL-i andmebaasi aktiivse Geo-kopeerimine ja Geo-taaste võimalusi."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Teisese Tõrkesiirde või Azure'i SQL-andmebaasi taastamine

SQL Azure'i andmebaas pakub taastamise on katkestuste järgmisi funktsioone:

- [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md)
- [Geo-taastamine](sql-database-recovery-using-backups.md#point-in-time-restore)

Äriprotsessid järjepidevuse ja toetavad järgmisi olukordi funktsioonide kohta leiate teemast [järjepidevuse](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Mõne katkestuste sündmuse ettevalmistamine

Edu taastamine aktiivse Geo-kopeerimine või geograafilise liigne varukoopiate teise andmete alale, peate koostama server teise andmekeskuse katkestuste saada uus esmane server peaks vaja tekkida nagu on määratletud juhiseid dokumenteerida ja testitud, et tagada sujuv taastamine. Järgmised ettevalmistavaid toiminguid.

- Leidke loogiline server teises regioonis saada uus esmane server. Aktiivse Geo koos – kopeerimine, see on vähemalt üks ja võib-olla iga teisene serverid. Geo taastamiseks, see üldiselt olla serveri [andmepunktipaaride piirkond](../best-practices-availability-paired-regions.md) , kus asub andmebaasi regiooni jaoks.
- Tuvastamine ja soovi korral määratleda serveritaseme tulemüüri reeglid kasutajatele juurdepääs esmane uue andmebaasi jaoks vaja.
- Määratlege, kuidas kavatsete ümber suunata kasutajate uus esmane server, näiteks muutes ühendusstringi või, muutes DNS-i kirjed.
- Tuvastamine ja soovi korral luua sisselogimise, mis peab juhtslaidi andmebaasis uus esmane server ja veenduge, et need sisselogimise on vajalikud õigused juhtslaidi andmebaasi, kui mõni. Lisateabe saamiseks vt [SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md)
- Leidke teatiste reeglid, mis on vaja värskendada vastendamiseks esmane uue andmebaasi.
- Dokumendi praeguse andmebaasi esmane Auditeerimispoliitika konfiguratsiooni
- Tehke [avariitaastet süvitsi](sql-database-disaster-recovery-drills.md). Jäljendamiseks on katkestuste Geo-taastamine, saate kustutada või põhjustada rakenduse ühenduvuse nurjumine lähteandmebaasi ümber nimetada. Simuleerida on katkestuste aktiivse Geo-kopeerimine, saate keelata veebirakenduse või virtuaalne seade on ühendatud andmebaasi või Tõrkesiirde andmebaasi rakenduse connectity tõrkeid põhjustada.

## <a name="when-to-initiate-recovery"></a>Millal algatada taastamine

Taastamise toiming mõjutab rakendus. See nõuab ühendusstringi SQL-i või ümbersuunamist DNS-i abil muutmist ja võib kaasa tuua püsivate andmekao. Seetõttu tuleks teha ainult siis, kui selle katkestuste tõenäoliselt kestab kauem kui rakenduse taastamise aja eesmärk. Rakenduse juurutamisel tootmise tuleks teha rakenduse seisundi kontrollimaks ja kasutada järgmist andmepunktide kinnitada, et taastamis on õigustatud.

1.  Püsivate ühenduvuse tõrge rakenduse taseme andmebaas.
2.  Azure portaali kuvatakse laialdane mõju piirkonna juhtum kohta teatise.
3.  Azure'i SQL-andmebaasi server on märgitud halvenenud.

Olenevalt teie rakenduse hälbe tööseisakute ning võimalik business vastutuse võite kaaluda järgmist taastesuvandeid.

[Too Taastatavad andmebaas](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) abil saate taastada Geo kopeeritud Viimane punkt.

## <a name="wait-for-service-recovery"></a>Oodake, kuni teenuse taastamine

Azure'i meeskondadel töö, et taastada teenuse kättesaadavus nii kiiresti kui võimalik, kuid sõltuvalt juurkausta põhjustada võib selleks kuluda tundides või päevades.  Kui saate rakenduse luba märgatavat tööseisakute võite lihtsalt oodata taastamise lõpuleviimiseks. Sel juhul enam teie pole vaja midagi. Praeguse teenuse olek saate vaadata meie [Azure'i teenuste seisundi armatuurlaud](https://azure.microsoft.com/status/). Pärast taastamis piirkonna taastatakse rakenduse kättesaadavus.

## <a name="failover-to-geo-replicated-secondary-database"></a>Tõrkesiirde geo kopeeritud teisene andmebaasiga

Kui teie taotlus tööseisakute põhjustada business vastutuse siis tuleks kasutada geo tiražeeritud andmebaasi oma rakenduse. See võimaldab rakendus kiiresti taastamiseks teises regioonis korral on katkestuste kättesaadavus. Siit saate teada, kuidas [konfigureerida Geo-Dispersioonanalüüs](sql-database-geo-replication-portal.md).

Kättesaadavus andmebaasi taastamine peate toetatud meetoditega geo kopeeritud teisese rikketeenus algatada.

Kasutage ühte järgmist juhendite Tõrkesiirde geo kopeeritud teisene andmebaasi.

- [Azure'i portaalis geo kopeeritud teisese Tõrkesiirde](sql-database-geo-replication-portal.md)
- [Tõrkesiirde geo kopeeritud teisese PowerShelli abil](sql-database-geo-replication-powershell.md)
- [Tõrkesiirde geo kopeeritud teisese T-SQL-i abil](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Taastada, kasutades Geo-taastamine

Kui teie taotlus tööseisakute ei põhjusta business vastutuse saate kasutada Geo-taastamine meetodi oma rakenduse andmebaasi taastada. See loob andmebaasi koopia uusima geograafilise liigne varukoopia.

Kasutage ühte järgmistest aitab geo taastatava andmebaasi uue piirkonda:

- [Azure'i portaalis uus alale andmebaasi Geo-taastamine](sql-database-geo-restore-portal.md)
- [Geo-taastamine andmebaasi uus regioon PowerShelli abil](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Andmebaasi taastamine pärast konfigureerimine

Kui kasutate kas geo-dispersioonanalüüs Tõrkesiirde või geo-taastamine on katkestuste taastada, peate veenduma, uute andmebaaside ühenduvust õigesti konfigureeritud nii, et tavalise rakenduse funktsiooni võib jätkata. See on kontroll-loendi ülesannete oma taastatud andmebaasi tootmise ettevalmistamine.

### <a name="update-connection-strings"></a>Ühendusstringi värskendamine

Kuna taastatud andmebaasi hakkavad asuma serveris, peate värskendama oma rakenduse ühendusstringi osutamiseks selles serveris.

Ühendusstringi muutmise kohta leiate lisateavet teemast [ühendusteegi](sql-database-libraries.md)sobiva arengu keel.

### <a name="configure-firewall-rules"></a>Konfigureerige tulemüür reeglid

Peate veenduge, et tulemüüri reeglid, mis on konfigureeritud server ja andmebaas kattuvad esmane server ja andmebaas esmane konfigureeritud. Lisateavet leiate teemast [kohta: tulemüüri sätete konfigureerimine (Azure'i SQL-andmebaasi)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Sisselogimise konfigureerimine ja andmebaasi kasutajad

Peate veenduge, et kõik on teie rakendus kasutab sisselogimise olemas serveris, mis majutab taastatud andmebaasi. Lisateavet leiate teemast [Turvalisuse konfiguratsiooni Geo - kopeerimise](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Peaksite konfigureerimine ja testige oma serveri tulemüüri reeglid ja sisselogimise (ja nende õiguste) ajal katastroofi taastamine süvitsi. Need serveritaseme objektid ja nende konfiguratsiooni ei pruugi olla saadaval on katkestuste ajal.

### <a name="setup-telemetry-alerts"></a>Telemeetria teatiste häälestamine

Peate veenduge, et teie olemasoleva reegli sätted on värskendatud vastendamiseks taastatud andmebaas ja muu server.

Andmebaasi teatiste reeglite kohta leiate lisateavet teemast [Saadetakse teatis teatised](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) ja [Jälitus teenuse seisund](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Luba auditi

Kui andmebaasi juurdepääsuks on vaja auditeerimine, peate luba auditeerimine pärast andmebaasi taastamine. Hea näitaja auditeerimine on nõutav on klientrakendused kasutada turvalist ühendust stringide muster *. database.secure.windows.net. Lisateabe saamiseks lugege teemat [Alustamine SQL-andmebaasi audit](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Järgmised sammud

- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Järjepidevuse kujundus ja taastamise äriprotsessid kohta leiate teemast [järjepidevuse stsenaariumid](sql-database-business-continuity.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
