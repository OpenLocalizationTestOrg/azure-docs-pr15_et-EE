<properties 
    pageTitle="Rakenduse Discovery registrisätete puhverserveri teenuste Cloud | Microsoft Azure'i" 
    description="Selles teemas eesmärk on teile juhiseid tuleb teha vajaliku pordi määramiseks kasutavad Cloud rakenduse Discovery agent." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Pilveteenuse rakenduse Discovery registrisätete puhverserveri teenuste jaoks

Vaikimisi on pilveteenuse rakenduse Discovery agendi konfigureeritud kasutama ainult pordid 80 ja 443. Kui te plaanite installimine pilveteenuse rakenduse Discovery keskkonnas puhverserverit, mis kasutab kohandatud pordi (80 ega 443), peate oma agentide kasutada selle pordi konfigureerimiseks. Konfiguratsiooni põhjal registrivõtme.


Selles teemas eesmärk on teile juhiseid tuleb teha vajaliku pordi määramiseks kasutavad Cloud rakenduse Discovery agent.



**Pilveteenuse rakenduse Discovery agent töötava arvuti kasutatav port muutmiseks tehke järgmist.**


1. Käivitage registriredaktor. <br> ![Käivita](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Avage või looge järgmisele registrivõtmele: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud rakenduse Discovery\Endpoint** 

3. Saate luua uue nimega **pordid** **stringist** väärtuse. ![Uue](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. **Muutmine** dialoogiboksi avamiseks topeltklõpsake väärtust pordid.


5. Tekstiväljale väärtus sisestage järgmised väärtused ja lisage kõik kohandatud pordid, mida kasutatakse teie asutuses. <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Mitme stringi redigeerimine](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Muutmine** .



**Lisaressursid**


* [Kuidas saab oma ettevõttes töötavaid Ülikooli pilve rakendused, mida kasutatakse leida](active-directory-cloudappdiscovery-whatis.md) 


