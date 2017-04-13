<properties
   pageTitle="Head tavad tarkvaravärskendusi Microsoft Azure'i IaaS kohta | Microsoft Azure'i"
   description="Artikkel sisaldab head tavad tarkvaravärskendusi Microsoft Azure'i IaaS keskkonnas.  See on mõeldud IT-spetsialistidele ja turvalisus ärianalüütikud, kes lahendavad muuta juhtelemendi, tarkvara värskendamise ja varade haldamine iga päev, sh oma ettevõtte Turve ja nõuetele vastavus püüete eest."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Tarkvaravärskendusi Microsoft Azure'i IaaS kohta head tavad

Enne kui mis tahes tüüpi arutelu heade tavade kohta Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) keskkonnas, on oluline mõista, mis stsenaariumid on, mis on teil haldamise tarkvaravärskendused ja ülesanded. Alloleval joonisel tuleks aidata teil mõista nende piirmäärad:

![Pilveteenuse mudelite ja ülesannete](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Vasakpoolse veerus kuvatakse seitse kohustused (määratletud jaotistest) ettevõtted peaks kaaluma, mis moodustavad turvalisuse ja privaatsuse IT-keskkond.
 
Andmete liigitamine ja vastutuse ja kliendi & lõpp-punkti kaitse on kohustused, mis on üksnes domeeni klientide ja füüsilise, Host ja võrgu kohustused on domeeni pilveteenuse pakkujate PaaS ja SaaS mudelid. 

Ülejäänud kohustused jagavad kliendid ja Pilveteenusest teenusepakkujatele. Mõned kohustused vaja autori nimega ja klientide haldamine ja hallata ülesanne koos, sh oma domeenide audit. Näiteks kaaluge identiteedi ja juurdepääsu juhtimine kasutades Azure Active Directory Services; kliendi konfiguratsiooni, nagu mitmikautentimise on, kuid tagada tõhus funktsioonid on Microsoft Azure'i.

> [AZURE.NOTE] Pilveteenuses ühiskasutusse antud kohustused kohta lisateabe saamiseks lugege [Ühiskasutusega vastutusalad Cloud arvutuste](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Neid samu põhimõtteid rakendada hübriidjuurutuse stsenaariumi, kui teie ettevõte kasutab Azure IaaS VMs, mis suhelda kohapealse ressurssidega, nagu on näidatud alloleval joonisel.

![Microsoft Azure'i tüüpilised hübriidjuurutuse stsenaarium](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Esmane hindamine

Isegi juhul, kui teie ettevõttel on juba update halduse süsteemi abil ja teil on juba tarkvara update poliitikate kohas, on oluline sageli eelmise poliitika hinnangute vaadata ja värskendada oma praeguse vajaduste järgi. See tähendab, et peate olema tuttav ressursside ettevõtte hetkeseisu. See olek saavutamiseks peate teadma:

-   Füüsilise ja virtuaalse arvutites teie ettevõttes.

-   Opsüsteemid ja töötab iga füüsilise ja virtuaalse arvutitesse versioonid.

-   Tarkvaravärskendusi (service pack versioonid, tarkvaravärskendused ja muud muudatused) iga arvutisse installitud.

-   Funktsiooni igasse arvutisse, teeb teie ettevõttes.

-   Rakenduste ja igas arvutis töötavad programmid.

-   Omaniku- ja kontakti teavet iga arvutis.

-   Varade esitamine keskkonna ja nende suhteline väärtus määratlemiseks valdkonnad vajavad kõige tähelepanu ja kaitse.

-   Turvalisus teadaolevate probleemide ja protsesside teie ettevõttel on koht isikupärase uue turvalisus või muutuste turbetaseme.

-   Keskkonna tagamiseks juurutatud vastumeetmete.

Peate värskendama seda teavet regulaarselt ja see peaks olema käepärast teie tarkvara värskendamise halduse protsessi osalejatele.

## <a name="establish-a-baseline"></a>Kindlaks

Tarkvara värskendamise halduse protsessi oluline osa on luua algse standard installide operatsioonisüsteemi versiooni, rakenduste ja riistvara arvutites teie ettevõttes; neid nimetatakse võrdlusandmeid?. Võrdlusalus on toote või loodud teatud punktis ajas süsteemi konfiguratsioon. Rakenduse või operatsioonisüsteemi võrdlusalus, näiteks pakub võimalust arvutis või teenuse teatud oleku taastada.

Võrdlusandmeid alus otsimine ja võimalikud probleemid ja lihtsustada tarkvara värskendamise halduse protsessi, peab teie ettevõttes juurutada tarkvaravärskendusi arvu ja jälgida, kuidas teie võimalus suurendades.

Pärast oma ettevõtte algne auditilogi, kasutage teavet, mis on saadud auditilogi määratleda mõne funktsionaalseid võrdlusalus IT komponentide tootmiskeskkonnast sees. Arvu võrdlusandmeid võib olla nõutav, olenevalt erinevat tüüpi riist- ja tarkvara tootmisse juurutatud.

Näiteks mõned serverid nõuda tarkvara update takistada nende hangumise sulgemisel toomise, kui töötab Windows Server 2012. Järgmistes serverites jaoks peaks sisaldama tarkvara värskendust.

Suurtes ettevõtetes on sageli kasulik jagada arvutites teie ettevõttes varade kategooriad ja hoida igas kategoorias standardne võrdlusalus, kasutades sama tarkvara ja tarkvaravärskendusi versioonid. Seejärel saate nende varade kategooriate seadmise tarkvara update jaotuse.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Vastav tarkvara teatis teenuste tellimine

Pärast tarkvara algse auditit kasutab ettevõtte, peaksite määratlema teatiste uue tarkvara värskenduste iga tarkvara toote ja versiooni jaoks parim meetod. Tarkvara erinev, parim meetod teatis võib olla meiliteatisi, veebisaitide või arvuti publikatsioonides.

Näiteks Microsoft turvalisus vastuse Center (MSRC) vastab kõik turvalisusega seotud probleemid Microsofti toodete kohta ning pakub Microsoft Security Bulletin Service tasuta e-posti teate loomine kohtade ja tarkvaravärskendusi, mis on välja antud tegeleda nende nõrkade. Saate tellida see teenus http://www.microsoft.com/technet/security/bulletin/notify.mspx juures.

## <a name="software-update-considerations"></a>Tarkvara värskenduse peaksite arvesse võtma

Pärast algse auditit tarkvara kasutab ettevõtte, peaksite määratlema häälestamine teie tarkvara update halduse süsteemi, mis sõltub tarkvara update halduse süsteemi kasutate nõuetele. Lugege WSUS, lugege [Heade tavade Windows Server Update teenustega](https://technet.microsoft.com/library/Cc708536), System Center [Configuration Manager värskenduste kavandamisel](https://technet.microsoft.com/library/gg712696).

Kuid on mõned üldised kaalutlused ja põhitõed, mida saate rakendada sõltumata lahenduse, mis on loodud, nagu on näidatud jaotiste järgneva.

### <a name="setting-up-the-environment"></a>Keskkonna häälestamine

Kaaluge järgmistest toimingutest tarkvara häälestamine kavandamisel värskendada halduse keskkonnas.

-   **Loo tootmise tarkvara update saidikogumid ühed kriteeriumide alusel**: üldiselt ühed kriteeriumide kasutamise tarkvara update varude ja jaotuse Saidikogumite loomine aitab lihtsustada kõigil tarkvara värskendamise halduse protsessi. Ühed kriteeriumid võib sisaldada kliendi installitud operatsioonisüsteemi versiooni ja hoolduspaketi tase, süsteemi rolli või target ettevõttes.

-   **Loo tootmiseelse saidikogumid, mis sisaldavad viide arvutisse**: tootmiseelse saidikogumi peaks sisaldama esindaja konfiguratsioone operatsioonisüsteemi versiooni, joone business tarkvara ja muu tarkvara töötab teie ettevõttes.

Võiksite kaaluda ka, kuhu tarkvara uuendamine paigutatakse, kui see on Azure IaaS taristu pilves, või kui oleks kohapealse. See on oluline otsus, sest teil on vaja hinnata liikluse asutusesiseste ressursside ja Azure taristu vahel. Lugege oma kohapealse taristu Azure ühenduse loomise kohta lisateabe saamiseks [ühenduse loomine mõne kohapealse võrgu loomine Microsoft Azure virtuaalse](https://technet.microsoft.com/library/Dn786406.aspx) .

Suvandi kujundus, mis määravad, kuhu paigutatakse uuendamine sõltub ka oma praeguse taristu ja tarkvara update süsteemi, mida parajasti kasutate. Lugege WSUS [Juurutada Windows Server Update Services teie asutuse](https://technet.microsoft.com/library/hh852340.aspx) ja System Center Configuration Manager lugeda [kavandamisel saitide ja hierarhiad Configuration Manager](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Varundus

Korrapäraste varukoopiate plaanimine on oluline mitte ainult tarkvara update halduse platvorm ise ka värskendatakse serverite jaoks. Ettevõtted, mis on [protsessi muuta](https://technet.microsoft.com/library/cc543216.aspx) , eeldab see, et põhjused, miks server on vaja värskendada, hinnanguline tööseisakute ning mõju. Veenduge, et on tagasipööramine konfiguratsiooni kohas juhuks, kui värskendus nurjub, veenduge, et varundada süsteemi regulaarselt.

Mõned Azure'i IaaS varukoopia suvandid on järgmised.

-   [Azure'i IaaS töökoormus kaitse abil andmete kaitse Manager](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Azure'i virtuaalmasinates varundamine](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Jälgimine

Peaks töötama aruannetel jälgimine arvu puudu või installitud värskendusi või värskendusi lõpetamata olekuga iga tarkvara värskendus, mis on lubatud. Samuti aruannete jaoks tarkvaravärskendusi, mis pole veel lubatud võib hõlbustada lihtsam juurutamise otsuseid.

Peaksite kaaluma järgmisi toiminguid:

-   Kohaldatavad ja installitud turbevärskendusi kõik arvutid oma ettevõtte auditit läbiviimine.

-   Lubada ja juurutamine vajalikud arvutid värskendusi.

-   Jälgida varude ja värskendage installimise oleku ja edenemise kuvamiseks kõik arvutid oma ettevõtte jaoks.

Lisaks üldist asjaolu selgitati käesolevas artiklis, võiksite kaaluda ka iga toote on parim tava, näiteks: kui teil on VM Azure SQL serveriga, veenduge, et jälgite tarkvara värskenduste soovitus selle toote jaoks.

## <a name="next-steps"></a>Järgmised sammud

Kasutage aidata teil määrata parimad võimalused virtuaalmasinates sees Azure'i IaaS tarkvaravärskendusi selles artiklis kirjeldatud juhiseid. Tarkvara värskendus head tavad traditsiooniline andmekeskuses ja Azure IaaS vahel on palju sarnasusi, seetõttu on soovitatav hinnata oma praeguse tarkvara update poliitika kaasata Azure VMs ja oluline head tavad: selles artiklis kaasamine oma üldine tarkvara värskenduse toimingu.
