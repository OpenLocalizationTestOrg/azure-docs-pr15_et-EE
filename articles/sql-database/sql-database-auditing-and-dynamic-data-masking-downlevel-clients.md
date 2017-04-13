<properties
    pageTitle="SQL-andmebaasi alltaseme kliendid toetavad ja auditeerimine muudab IP lõpp-punkti | Microsoft Azure'i"
    description="Teave SQL-andmebaasi alltaseme klientide tugi ja IP-lõpp-punkti muudatuste auditeerimine."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-andmebaasi - toega alltaseme kliendid ja auditeerimine muudab IP lõpp-punkti


[Valemiauditi](sql-database-auditing-get-started.md) töötab automaatselt SQL-i kliendid, mida toetavad TDS ümbersuunamist.


##<a id="subheading-1"></a>Alltaseme klientide tugi

Mis tahes klient, mis rakendab TDS 7,4 toetama ka ümbersuunamist. Erandid sellest kaasata JDBC 4.0 ümbersuunamise funktsioon ei toeta täielikult ja Tedious jaoks Node.JS rakenduses millist ümbersuunamist ei rakendatud.

"Alltaseme kliendid", tuleks muudetud st millised tugi TDS versioon 7.3 ja all - serveri FQDN ühenduse string.

Algne serveri FQDN ühendusstringi: <*Serveri nimi*>. database.windows.net

Muudetud serveri FQDN ühendusstringi: <*Serveri nimi*> .database. **turvaline**. windows.net

"Alltaseme kliendid" osaline loend sisaldab.

- .NET 4.0 ja alla,
- ODBC 10.0 ja alla.
- JDBC (vaatamata JDBC TDS 7,4, TDS ümbersuunamise funktsioon ei toeta täielikult)
- Tüütu (Node.JS) jaoks

**Märkus:** Ülaltoodud server FDQN muudatus võib olla kasulik ka SQL serveri taseme auditeerimine poliitikat ilma vajaduseta konfiguratsiooni toimingut iga andmebaasi (ajutine vähendamiseks).

##<a id="subheading-2"></a>IP lõpp-punkti muudatusi, kui Valemiauditi lubamine

Pange tähele, et kui auditeerimine, muuta IP lõpp-punkti andmebaasi. Kui teil on range tulemüürisätteid, värskendage need tulemüüri sätted vastavalt sellele.

Uue andmebaasi IP lõpp-punkti sõltub andmebaasi piirkond:

| Andmebaasi piirkond | Võimalikud IP lõpp-punktid |
|----------|---------------|
| Hiina Põhja  | 139.217.29.176, 139.217.28.254 |
| Hiina Ida  | 42.159.245.65, 42.159.246.245 |
| Austraalia Ida  | 104.210.91.32 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Austraalia kodutee | 191.239.184.223 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brasiilia Lõuna  | 104.41.44.161 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Kesk-USA  | 104.43.255.70 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Ida-Aasia   | 23.99.125.133 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Ida-USA 2 | 104.209.141.31 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Ida-USA   | 23.96.107.223 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Keskse India  | 104.211.98.219, 104.211.103.71 |
| Lõuna India   | 104.211.227.102, 104.211.225.157 |
| Lääne India  | 104.211.161.152, 104.211.162.21 |
| Jaapan Ida   | 104.41.179.1 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Jaapan Lääne    | 104.214.140.140 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Põhja Kesk-USA  | 191.236.155.178 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Põhja-Euroopa  | 104.41.209.221 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Lõuna-, Kesk-USA  | 191.238.184.128 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Kagu-Aasia  | 104.215.198.156 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Lääne Euroopa  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Lääne USA.  | 191.236.123.146 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Kanada Central  | 13.88.248.106 |
| Kanada Ida  |  40.86.227.82 |
