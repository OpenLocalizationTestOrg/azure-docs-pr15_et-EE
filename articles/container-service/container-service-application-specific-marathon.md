<properties
   pageTitle="Rakenduse või teenuse kindla kasutaja maraton | Microsoft Azure'i"
   description="Rakenduse või teenuse kindla kasutaja maraton loomine"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Ümbriste maraton, mikro-teenuseid, näiteks Põhiliselt/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Rakenduse või teenuse kindla kasutaja maraton loomine

Azure'i teenus pakub juhtslaidi serverid, millel preconfigure Apache Mesos ja maraton komplekti. Neid saab kasutada rakenduste klaster korraldab, kuid on kõige parem kasutada selleks juhtslaidi servereid. Näiteks tutistamine maraton konfiguratsiooni nõuab juhtslaidi serverite ise sisse logida ja teha muudatused – see soodustab kordumatu juhtslaidi serverid, mis on pisut erinev standardit ja hooldanud ja hallatavate sõltumatult. Lisaks ei pruugi nõutud ühe meeskonnatöö konfiguratsiooni optimaalse konfiguratsiooni teise meeskonna jaoks.

Selles artiklis selgitame lisamine rakenduse või teenuse kindla kasutaja maraton.

Kuna see teenus kuulub ühe kasutaja või meeskonnale, nad võivad seda mis tahes viisil, et nad soovivad konfigureerimine. Azure'i teenus tagab ka, et jätkuvalt teenuse käivitamine. Kui teenus ei, Azure'i teenus selle uuesti teie eest. Enamikul juhtudel ei isegi märkate tal tööseisakute.

## <a name="prerequisites"></a>Eeltingimused

[Azure'i teenus eksemplari Deploy](container-service-deployment.md) orchestrator tüüpi AV/OS ja [veenduge, et teie klient saab ühendada klaster](container-service-connect.md). Lisaks, tehke järgmist.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Rakenduse või teenuse kindla kasutaja maraton loomine

Alustage loomisega JSON konfiguratsioonifail, mis määratleb nimi rakenduse teenus, mida soovite luua. Siin kasutame `marathon-alice` nimeks raames. Salvestage fail umbes `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Järgmiseks kasutada näiteks Põhiliselt/OS CLI installida maraton eksemplari võimalused, mis on Otsingukonfiguratsiooni failis.

```bash
dcos package install --options=marathon-alice.json marathon
```

Nüüd peaks nähtaval olema oma `marathon-alice` oma AV/OS UI vahekaart teenused töötav teenus. UI saab `http://<hostname>/service/marathon-alice/` kui soovite otse juurde pääseda.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Näiteks Põhiliselt/OS CLI teenuse määramine

Soovi korral saate konfigureerida oma AV/OS CLI juurde pääseda, seades selle uue teenuse soovitud `marathon.url` atribuut Osutage soovitud `marathon-alice` näiteks järgmiselt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Saate kontrollida, millist eksemplari maraton, mis töötavad koos vastu teie CLI on `dcos config show` käsk. Juhtslaidi maraton teenust käsu abil saate taastada `dcos config unset marathon.url`.
