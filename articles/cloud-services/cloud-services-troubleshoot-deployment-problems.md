<properties
 pageTitle="Pilveteenuse teenuse juurutamise tõrkeotsing | Microsoft Azure'i"
 description="On paar levinumad probleemid, mis võivad tekkida pilveteenus Azure juurutamisel. Selles artiklis on toodud mõned neist lahendusi."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Pilveteenuse teenuse juurutamise probleemide tõrkeotsing

Pilvepõhise teenuse rakendusepaketi juurutamisel Azure saate hankida teavet juurutamise Azure'i portaalis paanil **Atribuudid** . Saate üksikasjade paan pilveteenusesse probleemide tõrkeotsingu ja saate sisestada seda teavet Azure'i toetamiseks uue tugiteenuse taotluse avamisel.

Leiate paanil **Atribuudid** järgmiselt:

* Azure'i portaalis, klõpsake oma pilveteenuses juurutamise, käsku **Kõik sätted**ja seejärel klõpsake käsku **Atribuudid**.
* Azure'i klassikaline portaalis, klõpsake oma pilveteenuses juurutamise, klõpsake **ARMATUURLAUA**lehe (jaotises **kiire ülevaade**) alumises paremas nurgas asuv. Pange tähele, et selle paani ei silti pole "Atribuudid".

> [AZURE.NOTE] Klõpsake paani paremas allnurgas ikooni saate kopeerimine lõikelauale sisu paanil **Atribuudid** .

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probleem: kasutaja ei pääse minu veebisaidile, kuid minu juurutamise käivitatakse ja kõik rolli aknad on valmis

Veebisaidi URL-i link kuvatakse portaalis ei sisalda pordi. Veebilehed vaikeport on 80. Kui teie taotlus on konfigureeritud käivitamiseks teise porti, peate lisama õige pordinumber URL, veebisaidi kasutamisel.

1. Azure'i portaalis, klõpsake oma pilveteenuse kasutuselevõtt.
2. Märkige paanil **Atribuudid** Azure portaali pordid rolli eksemplaride (jaotises **Sisendit lõpp-punktid**).
3. Kui pordi pole 80, lisada õige pordi väärtus URL, kui pöördute rakendus. Mitte vaikeport määramiseks tippige URL-i, millele järgneb ka pordinumber ilma tühikuteta, millele järgneb koolon (:).

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probleem: Minu rolli aknad ümber töödelda ilma minu tee midagi

Teenuse tervendav toimub automaatselt Azure tuvastab probleemi sõlmed ja seetõttu viib rolli eksemplarid uue sõlmed. Sel juhul võidakse kuvada oma rolli aknad ringlussevõtu automaatselt. Et teada saada, kui teenus paranemine ilmnes:

1. Klõpsake Azure portaali kasutuselevõtu oma pilveteenusesse.
2. Azure'i portaalis paanil **Atribuudid** teave üle ja kindlaks teha, kas teenuse paranemine ilmnes ajal, et saate märkida ringlussevõtu rollid.

Rollide kuvatakse ka prügikastis umbes üks kord kuus host-OS ja Külastajate-OS värskendused.  
Lisateavet leiate ajaveebipostitusest [Rolli eksemplari taaskäivitamist tingitud OS uuendamine](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probleem: ma ei saa teha VIP Vaheta ja kuvatakse tõrketeade

VIP vahetamine on lubatud, kui juurutamine värskendamine on pooleli. Juurutamise värskendused võivad tekkida automaatselt kui:

* Uus Külastajate operatsioonisüsteem on saadaval ja teie arvuti on konfigureeritud automaatselt värskendusi.
* Teenus paranemine toimub.

Kui soovite teada saada, kas automaatne värskendamine ei luba teil teha VIP vahetamine:

1. Azure'i portaalis, klõpsake oma pilveteenuse kasutuselevõtt.
2. Azure'i portaalis paanil **Atribuudid** pilk väärtus **olek**. Kui see on **valmis**, siis märkige **viimase toimingu** kuvamiseks, kui üks viimati juhtus, mis võivad takistada VIP vahetamine.
3. Korrake juhiseid 1 ja 2 tootmise juurutamiseks.
4. Kui automaatvärskenduse on protsess, oodake, kuni see enne teha VIP Vaheta lõpuleviimiseks.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probleem: Rolli eksemplari hoidke alustamine, Initializing, hõivatud ja peatamiseni vahel

See tingimus võib viidata probleem rakenduse kood, paketi või konfiguratsiooni faili. Sel juhul tuleks olekut, muutes iga mõne minuti ja Azure portaali võib öelda midagi nagu **ringlussevõtt**, **hõivatud**või **Initializing**. See näitab, et on midagi valesti, et hoiab rolli eksemplari, kus töötab.

Lisateavet selle probleemi tõrkeotsingu kohta leiate ajaveebipostitusest [Azure'i PaaS arvutada diagnostika andmed](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) ja [levinud probleemid, mis põhjustavad rollid taaskasutada](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Probleem: Minu rakenduste lõpetas töötamise

1. Klõpsake Azure portaali rolli eksemplari.
2. Kaaluge Azure portaali paanil **Atribuudid** probleemi lahendamiseks järgmised tingimused.
   * Kui rolli eksemplari on hiljuti lõpetanud (märgite ruudu väärtus **arv hüljata**), juurutamise võib värskendamine. Oodata, et näha, kui rolli eksemplari uuesti oma toimimist.
   * Kui rolli eksemplari on **hõivatud**, kontrollige oma rakenduse koodi kuvamiseks, kui sündmus [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) käsitletakse. Võib juhtuda lisada või parandada mõne selle sündmuse koodi.
   * Avage Diagnostikaandmete ja tõrkeotsingu stsenaariumid ajaveebipostituse [Azure'i PaaS arvutada diagnostika andmete](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)kaudu.

>[AZURE.WARNING] Kui te prügikastis oma pilveteenuses, lähtestamist atribuutide juurutamiseks, tõhus kustutamine algse probleemi teave.

## <a name="next-steps"></a>Järgmised sammud

Saate vaadata rohkem [artikleid tõrkeotsingu](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pilveteenustega.

Pilvepõhise teenuse roll probleemide tõrkeotsingu Azure'i PaaS arvuti diagnostika andmete abil leiate teemast [Kevin Williamson ajaveebi sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
