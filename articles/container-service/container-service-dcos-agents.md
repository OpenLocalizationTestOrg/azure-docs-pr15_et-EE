<properties
   pageTitle="Avaliku ning privaatne AV/OS Agent ACS | Microsoft Azure'i"
   description="Kuidas töötada mõne Azure'i teenus kobar avalike ja privaatvõ agent kaustu."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Keskmise suurusega, ümbriste, mikro-teenuste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Näiteks Põhiliselt/OS Agent kaustu jaoks Azure'i teenus

Näiteks Põhiliselt/OS Azure'i teenus jagab agentide avalik või privaatne kaustadesse. Juurutamine saab teha mõlemal pool, mõjutamata masinad container teenust vahelist ühendust. Masinad saab Internetis (avaliku) või säilitatakse sisemise (privaatne). See artikkel annab lühiülevaade, miks on avalike ja privaatvõ pool.

### <a name="private-agents"></a>Privaatne agentide

Privaatne agent sõlmed käivitada-marsruuditavaid võrgu kaudu. Sellest võrgust on juurdepääsetav ainult administraator tsooni või avalik ala serva ruuteri kaudu. Vaikimisi käivitab AV/OS privaatne agent sõlmed rakendused. Lugege lisateavet võrgu turvalisuse [AV/OS dokumentatsiooni](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

### <a name="public-agents"></a>Avalike

Avalik agent sõlmed käivitada näiteks Põhiliselt/OS rakenduste ja teenuste avalikult juurdepääsetava võrgu kaudu. Lugege lisateavet võrgu turvalisuse [AV/OS dokumentatsiooni](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

## <a name="using-agent-pools"></a>Agent kaustu abil

Vaikimisi kasutab **maraton** *Privaatne* agent sõlmi mis tahes uus rakendus. Teil on rakenduse *avaliku* sõlm rakenduse loomise ajal konkreetselt juurutamine. Valige vahekaart **Valikuline** ja sisestage **slave_public** **Aktsepteeritud ressursi rollid** väärtus. See toiming on dokumenteerida [siin](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ja [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentatsiooni.

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [oma AV/OS ümbriste haldamine](container-service-mesos-marathon-ui.md).

Siit saate teada, kuidas [avada tulemüüri](container-service-enable-public-access.md) Azure'i soovite lubada juurdepääsu oma AV/OS container järgi.