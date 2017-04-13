<properties
   pageTitle="Avaliku juurdepääsu lubamine ACS rakendusse | Microsoft Azure'i"
   description="Kuidas lubada juurdepääsu teenuse Azure ümbrises."
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
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Azure'i Container teenuserakenduse juurdepääsu lubamine

Mis tahes AV/OS container ACS [avalik agent rakenduskausta](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) on automaatselt avatud Interneti-ühendus. Vaikimisi, pordid **80**, **443**, **8080** on avanud ja mis tahes (avaliku) container kuulamise need pordid on kättesaadavad. Selles artiklis kirjeldatakse, kuidas Azure'i teenus veel portide oma rakenduste avamine.

## <a name="open-a-port-portal"></a>Avage port (portaal) 

Kõigepealt tuleb soovime port avada.

1. Portaali sisse logida.
2. Otsige juurutatud Azure'i Container teenuse ressursirühma.
3. Valige agent laadi koormusetasakaalustusteenuse (mis sarnaneb **XXXX-agent-lb-XXXX**nimega).

    ![Azure'i container teenuse laadi koormusetasakaalustusteenuse](media/container-service-dcos-agents/agent-load-balancer.png)

4. Klõpsake **sondid** ja seejärel **lisada**.

    ![Azure'i container teenuse laadi koormusetasakaalustusteenuse sondid](media/container-service-dcos-agents/add-probe.png)

5. Juures vormi täitmine ja klõpsake nuppu **OK**.

  	| Väli | Kirjeldus |
  	| ----- | ----------- |
  	| Nimi  | Funktsiooni juures kirjeldav nimi. |
  	| Port  | Pordi ümbrisest testida. |
  	| Tee  | (Kui HTTP režiim) Suhteline veebisaidi tee sond. HTTPS-i ei toetata. |
  	| Intervall | Aja vahel juures püüab sekundites. |
  	| Vigane lävi | Enne ümbris vigane püüab arvu järjestikust juures. | 
    

6. Tagasi agent laadi koormusetasakaalustusteenuse atribuute, klõpsake **tasakaalustavad reeglite laadimine** ja seejärel **Lisa**.

    ![Azure'i container teenuse laadi koormusetasakaalustusteenuse reeglid](media/container-service-dcos-agents/add-balancer-rule.png)

7. Laadi koormusetasakaalustusteenuse vormi täitmine ja klõpsake nuppu **OK**.

  	| Väli | Kirjeldus |
  	| ----- | ----------- |
  	| Nimi  | Laadi koormusetasakaalustusteenuse kirjeldav nimi. |
  	| Port  | Avaliku sissetuleva pordi. |
  	| Kirjutamata port | Sisemise avaliku pordi abil saate marsruutida liiklust-ümbrisest. |
  	| Taustväärtus pool | Ümbriste selles kaustas saab seda laadi koormusetasakaalustusteenuse eesmärk. |
  	| Juures | Juures, kui **kirjutamata rakenduskausta** eesmärgiks on terve. |
  	| Seansi püsimine | Määratleb liikluse klientrakenduses käsitlemist seansi kestel.<br><br>**Pole**: järjestikust taotlused sama kliendi võib käsitleda mis tahes container järgi.<br>**Kliendi IP**: järjestikuste taotlusi samalt kliendi IP käsitletakse samasse container järgi.<br>**Kliendi IP- ja Protocol (protokoll)**: järjestikuste päringutele sama kliendi IP- ja Protocol (protokoll) kombinatsiooni käsitletakse samasse container järgi. |
  	| Jõudeaja ajalõpu | (Ainult TCP) Minutiga, avage aeg hoida TCP/http kaudu kliendi ilma tuginedes *ülalhoidmise* sõnumeid. |

## <a name="add-a-security-rule-portal"></a>Turvalisus (portaal) reegli lisamine

Järgmiseks tuleb lisada turvalisus reegli, mis marsruudib liikluse meie avatud pordi läbi tulemüüri kaudu.

1. Portaali sisse logida.
2. Otsige juurutatud Azure'i Container teenuse ressursirühma.
3. Valige **avaliku** agent võrgu turberühma (mis sarnaneb **XXXX-agent-avalik-nsg-XXXX**nimega).

    ![Azure'i container teenuse võrgu turberühm](media/container-service-dcos-agents/agent-nsg.png)

4. Valige **sissetulevad reeglid turvalisus** ja seejärel **Lisa**.

    ![Azure'i container teenuse võrgu turvalisuse rühma reeglid](media/container-service-dcos-agents/add-firewall-rule.png)

5. Täitke tulemüüri reegel oma avaliku port lubada, ja klõpsake nuppu **OK**.

  	| Väli | Kirjeldus |
  	| ----- | ----------- |
  	| Nimi  | Tulemüüri reegel kirjeldav nimi. |
  	| Priority (prioriteet) | Priority (prioriteet) reiting reegli. Madalam arv, mida suurem prioriteet. |
  	| Allikas | Sissetuleva IP aadresse lubatud või keelatud selle reegli alusel piirata. Kasutada **mis tahes** ei määra piirang. |
  	| Teenus | Valige see turvalisus reegel on eelmääratletud teenuseid. Kui soovite luua oma kasutada **kohandatud** . |
  	| Protocol (protokoll) | Liikluse **TCP** või **UDP**alusel piirata. Kasutada **mis tahes** ei määra piirang. |
  	| Port vahemikus | Kui **teenus** on **kohandatud**, saate määrata pordid mõjutab selle reegli vahemikus. Saate kasutada ühte porti, nt **80**või vahemiku nagu **1024-1500**. |
  	| Toiming | Lubada või keelata liikluse määratud kriteeriumidele. |

## <a name="next-steps"></a>Järgmised sammud

Lisateavet [avalike ja privaatvõ AV/OS agentide](container-service-dcos-agents.md)vahe.

Lugege lisateavet [oma AV/OS ümbriste haldamine](container-service-mesos-marathon-ui.md).