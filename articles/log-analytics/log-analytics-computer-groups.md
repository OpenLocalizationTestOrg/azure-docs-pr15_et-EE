<properties
    pageTitle="Arvuti rühmad Log Analytics logige otsingud | Microsoft Azure'i"
    description="Arvuti rühmad Log Analytics võimaldavad ulatus log otsinguid kindla arvutite kogum.  Selles artiklis kirjeldatakse neid erinevaid viise, saate luua arvutis rühmad ja log otsingu kasutamine."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Arvuti rühmad Log Analytics logige otsingud
Arvuti rühmad Log Analytics võimaldavad ulatus [otsingud logige](log-analytics-log-searches.md) arvutisse teatud kogumi.  Iga rühma sisustatakse kasutades päring, mis määratlete arvutite või rühmade importimine erinevatest allikatest.  Kui rühm on kaasatud log otsingu, tulemused on piiratud arvutites jaotises vastavad kirjed.

## <a name="creating-a-computer-group"></a>Arvuti rühma loomine
Saate luua arvutis rühma Log Analytics kasutamise viise järgmises tabelis.  Iga meetodi andmed on esitatud allpool. 

| Meetod | Kirjeldus |
|:---|:---|
| Logi otsing       | Logi otsing, mis tagastab arvutite loendi loomine ja salvestamine arvuti rühma tulemused. |
| Otsingu API-ga sisse logida   | Log otsingu API abil programmiliselt log otsingu tulemuste arvuti rühma loomine. |
| Active Directory | Kontrolli automaatselt rühmakuuluvust agent arvutites, kus on liikmed Active Directory domeeni ja iga turberühma Log Analytics rühma loomine.
| WSUS              | Automaatselt WSUS server või kliendid otsida suunamise rühmad ja iga Log Analytics rühma loomine. |


### <a name="log-search"></a>Logi otsing

Arvuti rühmad loodud logifailide otsing sisaldab tagastatud määratlete otsingupäringu arvutite.  See päring käivitatakse iga kord, kui arvuti rühma kasutatakse nii, et rühm on loodud muudatusi, kajastuvad.

Järgmiste toimingute abil saate otsingu Logi arvutis rühma loomine.

1. [Loo log otsingu](log-analytics-log-searches.md) , mis tagastab arvutite loendi.  Otsingu peab andma arvutites erinevate kogumi midagi nagu **Erinevate arvutit** või **arvuti mõõt count()** päringu abil.  
2. Klõpsake ekraani ülaosas nuppu **Salvesta** .
3. Valige **Jah** **päringu salvestada arvuti rühmana:**.
4. Tippige **nimi** ja **kategooria** rühma.  Kui otsingu sama nime ja kategooria on juba olemas, siis teil palutakse olemas.  Saate määrata mitu sama nimega otsinguid erinevate kategooriate järgi. 

Järgmine on näide otsingupäringutest, millega saate rühma arvutisse salvestada.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Logige otsingu API

Loodud logifailide otsingu API-ga arvutis rühmad on samad, mis loodud logifailide otsingu otsingud.

Log otsingu API abil arvuti rühma loomise kohta teavet artiklist [arvuti rühmad Log Analytics logige search REST API -ga](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory

Log Analytics importimiseks Active Directory rühma liikmestaatusi konfigureerimisel analüüsib mis tahes ühendatud domeeni arvutite OMS agent rühmakuuluvust.  Arvuti on loonud Log Analytics iga turberühma Active Directory ja arvuti rühmadesse kuulumise alusel turberühmade vastav lisatakse iga arvuti.  Selle liikmelisuse värskendatakse pidevalt iga neli tundi.  

Saate konfigureerida Log Analytics importimiseks Active Directory turberühmad Log Analytics **sätete**menüüst **Arvuti rühmad** .  Valige **automatiseerimine** ja seejärel **impordi Active Directory rühma liikmestaatusi arvutitest**.  On pole veel konfigureerimine vajalik.

![Arvuti rühmad Active Directory kaudu](media/log-analytics-computer-groups/configure-activedirectory.png)

Kui rühmad on imporditud, loetletakse menüü tuvastatud rühmakuuluvus arvutite arv ja rühmade imporditud arv.  Võite klõpsata ühte järgmistest linkidest **ComputerGroup** kirjed selle teabe tagastamiseks.

### <a name="windows-server-update-service"></a>Windows Serveri värskendusteenus

Log Analytics importimiseks WSUS rühmaliikmeid konfigureerimisel analüüsib kõik arvutid OMS agendi suunatud rühmakuuluvust.  Kui kasutate kliendipoolne suunamise, kui arvutis on ühendatud OMS ja on osa, mis tahes WSUS suunamise rühmad on selle rühma liikmelisusel Log Analytics importida. Kui kasutate serveripoolne suunamise, kuvatakse OMS agent peaks olema installitud WSUS server võimaldamaks rühma liikmeks saamiseks OMS importida.  Selle liikmelisuse värskendatakse pidevalt iga neli tundi. 

Saate konfigureerida Log Analytics importimiseks Active Directory turberühmad Log Analytics **sätete**menüüst **Arvuti rühmad** .  Valige **Active Directory** ja seejärel **impordi Active Directory rühma liikmestaatusi arvutitest**.  On pole veel konfigureerimine vajalik.

![Arvuti rühmad Active Directory kaudu](media/log-analytics-computer-groups/configure-wsus.png)

Kui rühmad on imporditud, loetletakse menüü tuvastatud rühmakuuluvus arvutite arv ja rühmade imporditud arv.  Võite klõpsata ühte järgmistest linkidest **ComputerGroup** kirjed selle teabe tagastamiseks.

## <a name="managing-computer-groups"></a>Arvuti rühmade haldamine

Saate vaadata arvuti rühmad, loodud logifailide otsingu või Log otsingu API Log Analytics **sätete**menüüst **Arvuti rühmad** .  Klõpsake nuppu **x** **eemaldamine** veeru kustutamiseks jaotises arvuti.  Klõpsake käivitamiseks rühma Logi otsing, mis tagastab liikmete rühma **liikmete vaatamine** ikooni. 

![Arvutisse salvestatud rühmad](media/log-analytics-computer-groups/configure-saved.png)

Rühma muutmiseks sama **kategooria** ja **nime** ülekirjutamiseks algsele uue rühma loomine.

## <a name="using-a-computer-group-in-a-log-search"></a>Arvuti rühma log otsingu kasutamine
Järgmise süntaksi abil viidata log otsingu arvuti rühma.  **Kategooria** on valikuline ja seda määrab nõutav, kui teil on arvuti rühmad sama nimega erinevate kategooriate järgi. 

    $ComputerGroups[Category: Name]

Otsingu käitamisel lahendatakse esmalt mõni otsing hõlmab arvuti rühma liikmete.  Kui rühm on Logi otsing, siis selle otsingu käitatakse ülemise taseme log otsingu enne rühma liikmete tagastamiseks.

Tavaliselt kasutatakse arvuti rühmad nagu järgmises näites otsingus log **IN** -klauslit.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Rühma andmed

Kirje on loodud on iga loodud Active Directory või WSUS arvuti rühmakuuluvus OMS hoidla.  Need kirjed peavad tüüpi **ComputerGroup** ja atribuutide järgmises tabelis.  Arvuti rühmade põhjal log otsingute jaoks ei looda kirjed.

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Arvuti            | Arvuti liikme nimi. |
| Rühma               | Rühma nime. |
| GroupFullName       | Täielik tee rühma, sh lähte-kui ka andmeallika nimi.
| GroupSource         | Andmeallika selle rühma oli kogutud. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Rühmad on kogutud allika nimi.  Active Directory, on selle domeeni nimi. |
| ManagementGroupName | SCOM agentide haldus rühma nimi.  Muude töötajatega, see on AOI -\<tööruumi ID\> |
| TimeGenerated       | Kuupäev ja kellaaeg, mis on loodud või värskendatud jaotises arvuti. |



## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks.  