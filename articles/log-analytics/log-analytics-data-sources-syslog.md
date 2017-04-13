<properties 
   pageTitle="Logi sõnumite Logi Analytics | Microsoft Azure'i"
   description="Logi on sündmuste logimine protokolli, mis on ühine Linux.   Selles artiklis kirjeldatakse, kuidas konfigureerida Logi sõnumite kogumit Log analüütika ja nende loodud OMS hoidlas kirjete üksikasjad."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Log Analytics Logi andmeallikad

Logi on sündmuste logimine protokolli, mis on ühine Linux.  Rakenduste saadab sõnumid, mis on talletatud kohalikus arvutis või Logi Collector kohale toimetatud.  OMS Agent Linuxi installimisel seda konfigureerib kohaliku Logi daemon agent sõnumite edastamiseks.  Agent saadab sõnumi Log Analytics, kus vastav kirje on loodud OMS hoidla.  

> [AZURE.NOTE]Log Analytics toetab rsyslog või Logi-ng saadetud sõnumite kogumit. Logi sündmus saidikogumi vaikimisi Logi daemon punane rolli Enterprise Linux, CentOS ja Oracle'i Linuxi versioon (sysklog) 5 versiooni ei toetata. Selle versiooni nende jaotuse Logi andmeid koguda, peaks [rsyslog daemon](http://rsyslog.com) installitud ja asendamiseks sysklog konfigureeritud.

![Logi saidikogumi](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Logi konfigureerimine
OMS Agent Linuxi kogub ainult võimaluste ja selle konfiguratsiooni määratud raskusastme sündmused.  Saate konfigureerida Logi OMS portaali kaudu või oma Linux agentide konfigureerimine failide haldamine.


### <a name="configure-syslog-in-the-oms-portal"></a>Logi OMS portaalis konfigureerimine

Logi [menüü andmed Log Kasutusanalüüsi sätteid](log-analytics-data-sources.md#configuring-data-sources)konfigureerida.  Selle konfiguratsiooni toimetatakse iga Linux agent konfigureerimise faili.

Saate lisada uusi poole, tippides selle nime ja klõpsates käsku **+**.  Iga objekti kogutakse ainult valitud raskusastme sõnumeid.  Märkige ruut raskusastme kindla poole, mida soovite koguda.  Ei saa anda täiendavate kriteeriumide filtri sõnumeid.

![Logi konfigureerimine](media/log-analytics-data-sources-syslog/configure.png)


Vaikimisi lükata kõik konfiguratsioonimuudatuste automaatselt kõigile.  Kui soovite iga Linux Agent Logi käsitsi konfigureerida, siis tühjendage ruut *Rakenda all minu Linux masinad konfigureerimine*.


### <a name="configure-syslog-on-linux-agent"></a>Logi Linux agent konfigureerimine

Kui ta [OMS agent installitud Linux klient](log-analytics-linux-agents.md)installib vaikimisi Logi konfiguratsioonifail, mis määratleb poole ja tõsidust sõnumid, mis on kogunud.  Saate muuta selle faili muuta konfiguratsiooni.  Konfiguratsioonifail erineb sõltuvalt Logi daemon kliendi on installitud.

> [AZURE.NOTE] Kui redigeerite Logi konfiguratsiooni, peate Logi daemon muudatuste jõustumiseks.

#### <a name="rsyslog"></a>rsyslog

Konfiguratsioonifail rsyslog asub **/etc/rsyslog.d/95-omsagent.conf**.  Vaikesisu on näidatud allpool.  See kogub Logi saadetud kohaliku agendi kõik hoiatuse või kõrgema taseme vahendid.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Üksuse eemaldamiseks, eemaldades selle osa konfiguratsiooni faili.  Saate piirata raskusastme, mis on kogunud konkreetsele objektile, muutes selle objekti kirje.  Näiteks piirata kasutaja võimalus sõnumite koos raskusaste, tõrge või uuemas versioonis saate muuta sellise konfiguratsiooni faili järgmine:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Logi-ng

Konfiguratsioonifail rsyslog on **/etc/syslog-ng/syslog-ng.conf**kohas.  Vaikesisu on näidatud allpool.  See kogub Logi saadetud kohaliku agendi kõik võimalused ja kõik raskusastme.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Üksuse eemaldamiseks, eemaldades selle osa konfiguratsiooni faili.  Saate piirata raskusastme, mis on kogutud konkreetsele objektile eemaldamine loendist.  Näiteks piirata kasutaja poole ainult teatis ja kriitilised sõnumitele, saate muuta see osa konfiguratsiooni faili järgmine:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Muutmine Logi pordi

OMS agent kuulab Logi sõnumite kohaliku kliendi Port 25224.  Saate muuta selle pordi, lisades OMS agent konfiguratsioonifail asub **/etc/opt/microsoft/omsagent/conf/omsagent.conf**järgmisest jaotisest.  Pordi number, mida soovite asendada 25224 **pordi** kirje.  Pange tähele, et ka peate muutmine Logi daemon sõnumeid saata selle pordi konfiguratsioonifail.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Andmete kogumine

OMS agent kuulab Logi sõnumite kohaliku kliendi Port 25224. Logi daemon konfiguratsioonifail Logi selle pordiga, kui need on kogutud Log Analytics rakendusest saadetud sõnumite edasisaatmine


## <a name="syslog-record-properties"></a>Logi kirje atribuudid

Logi kirjed peavad tüüpi **Logi** ja atribuutide järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| Arvuti | Arvuti, mis on kogutud sündmus. |
| Võimalus | Määratleb sõnumi loonud süsteemi osa. |
| HostIP | Sõnumi saatmise süsteemi IP-aadress.  |
| Hostname (hostinimi) | Sõnumi saatmise süsteemi nimi. |
| SeverityLevel | Raskusaste taseme sündmus. |
| SyslogMessage | Sõnumi teksti. |
| ProcessID | Protsessi loonud sõnumi ID-d. |
| EventTime | Kuupäev ja kellaaeg, mis on loodud sündmus.



## <a name="log-queries-with-syslog-records"></a>Log päringuid Logi kirjed

Järgmises tabelis antakse eri ülevaade log päringuid, mis tuua Logi kirjed.

| Päringu | Kirjeldus |
|:--|:--|
| Tüüp = Logi | Kõik Syslogs. |
| Tüüp = Logi SeverityLevel = tõrge | Kõik Logi kirjetega tõsidust tõrge. |
| Tüüp = Logi & #124; Mõõtke count() arvuti | Count of Logi kirjed arvuti. |
| Tüüp = Logi & #124; Mõõtke count() mis ettevõte | Count of Logi kirjed, mis ettevõte. |

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks. 
- Üksikute väljade Logi kirje andmete sõeluda [Kohandatud väljade](log-analytics-custom-fields.md) abil.
- Muud tüüpi andmete kogumiseks [konfigureerimine Linux agentide](log-analytics-linux-agents.md) . 
