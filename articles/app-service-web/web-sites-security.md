<properties
    pageTitle="Rakendus App Azure'i teenus Secure"
    description="Saate teada, kuidas secure web appi, mobiilirakenduse kirjutamata või API rakenduse Azure'i rakendust Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Rakendus App Azure'i teenus Secure

See artikkel aitab teil alustada oma veebirakenduse, mobiilirakenduse kirjutamata või API rakenduse Azure'i rakendust Service turvaliseks. 

Azure'i rakendust Service Turve on kahel tasemel. 

- **Taristu- ja platvormi turbe** - usaldate Azure'i peab olema peate käivitama tegelikult asju turvaliselt pilve.
- **Rakenduse turbe** - peate rakenduse ise kujundada turvaliselt. See hõlmab, kuidas saate integreerida Azure Active Directory, kuidas hallata serdid ja kuidas teil tagada, et saate turvaliselt rääkida erinevad teenused. 

#### <a name="infrastructure-and-platform-security"></a>Taristu- ja platvormi turvalisus
Kuna rakendus teenus säilitab Azure VMs, salvestusruumi, võrguühenduste, web raamistik, haldus ja Kliendiintegratsiooni funktsioonide ja palju muud on turvatud ja aktiivselt kõva kontrollimisel tugev nõuetele vastavus ja kontrollib pidevalt tagada, et:

- Rakenduste rakenduse teenus on eraldatud nii Interneti kaudu ja teiste klientide Azure ressursse.
- Suhtlus saladusi (nt ühendusstringi) rakenduse App teenuse ja muud Azure ressursid (nt SQL-andmebaasi) ressursirühma Azure'is jääb ja ei rist mis tahes võrk piirid. Saladusi on alati krüptitud.
- Kogu suhtlus rakenduse teenuse rakendus ja välised ressursid, nt PowerShelli haldus, käsurea liides, Azure'i SDK-d, REST API-d ja hübriid ühendused vahel on õigesti krüptitud.
- 24-tunnise ohtu halduse kaitseb rakenduse teenuse ressursside kaudu ründevara, jaotatud eitamine teenuse (DDoS) mees-sisse--Lähis (MITM) ja muud ohud. 

Azure'i taristu- ja platvormi turvalisuse kohta leiate lisateavet teemast [Azure Usalduskeskus](/support/trust-center/security/).

#### <a name="application-security"></a>Rakenduse turvalisus

Kuigi Azure'i vastutab turvaliseks taristu- ja platvorm, mis teie rakendus töötab, on tagada rakenduse enda vastutusel. Teisisõnu, peate arendada, juurutada ja hallata oma rakenduse koodi ja sisu turvaliselt. Ilma selleta oma rakenduse koodi või sisu võib olla tundlik ohtude nagu.

- SQL süst
- Seansi ärandamine
- Rist-saidi skriptimine
- Rakendusetaseme MITM
- Rakendusetaseme DDoS

Täieliku arutelu turvakaalutlused veebipõhised rakendused on selle dokumendi väljapoole. Alguspunktina täiendavad juhised turvaliseks rakenduse jaoks, lugege teemat [Avatud Web rakenduse turvalisus Project (OWASP)](https://www.owasp.org/index.php/Main_Page), spetsiaalselt selle [projekti ülemine 10.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), mis on loetletud praeguse ülemine 10 kriitiline web rakenduse turvalisuse vigu, määratud OWASP liikmed.

## <a name="perform-penetration-testing-on-your-app"></a>Sooritada oma rakenduse testimine kättesaadavuse

Üks lihtsamaid võimalusi alustamiseks veel nõrkade kohtade oma rakenduse teenuse rakenduse testimine on [integreerimine hõbepaber turvalisus](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) sooritada ühe klõpsuga haavatavuse skannimine rakenduse abil. Saate vaadata kontrolltöö tulemuste lihtne mõista aruandes, ja saate teada, kuidas lahendada kõrvaldamiseks üksikasjalikke juhiseid.

Kui eelistate teha oma kättesaadavuse kontrollib või soovite kasutada mõne muu skanneri komplekti või pakkuja, peate kirjeldatud [Azure kättesaadavuse testimise kinnitamise](https://security-forms.azure.com/penetration-testing/terms) ja saada eelneva soovitud kättesaadavuse testide sooritamiseks.

##<a name="https"></a>Turvaline side klientidega

Kui kasutate funktsiooni ** \*. azurewebsites.net** loodud rakendust Service rakenduse domeeninime saate kohe kasutada HTTPS, nagu SSL-sert on esitatud kõigi ** \*. azurewebsites.net** domeeninimede. Kui teie sait kasutab [kohandatud domeeninime](web-sites-custom-domain-name.md), saate üles laadida SSL-serdi [HTTPS-i lubamine](web-sites-configure-ssl-certificate.md) kohandatud domeeni.

[HTTPS-i](https://en.wikipedia.org/wiki/HTTPS) lubamine aitab kaitsta MITM eest teatise vahel rakenduse ja selle kasutajad.

## <a name="secure-data-tier"></a>Turvaline andmete taseme

Teenuse rakenduse väga integreerub SQL-andmebaasi nii, et kõik ühendusstringi krüptitud kõikjal ja dekrüptitakse VM, et rakendus töötab *ja* ainult siis, kui rakendus töötab ainult. Lisaks Azure SQL-andmebaas sisaldab palju turbefunktsioonid aitavad secure küberohtude, sh [veebisaidil ülejäänud krüptimist](https://msdn.microsoft.com/library/dn948096.aspx), [Alati krüptitud](https://msdn.microsoft.com/library/mt163865.aspx), [Dünaamiline andmete Masking](../sql-database/sql-database-dynamic-data-masking-get-started.md)ja [Ohtude tuvastamise](../sql-database/sql-database-threat-detection-get-started.md)pärit andmete rakenduse. Kui teil on nõuetele või tundliku sisuga andmeid, vaadake lisateavet selle kohta, kuidas kaitsta teie andmeid [turvaliseks SQL-andmebaasi](../sql-database/sql-database-security.md) .

Kui kasutate muu andmebaasi pakkuja juures, nt ClearDB, konsulteerige selle pakkuja dokumendid otse turvalisus heade tavade kohta.  

##<a name="develop"></a>Turvaliseks ja juurutamine

### <a name="publishing-profiles-and-publish-settings"></a>Avaldamise profiilid ja avaldada sätted

Rakenduste haldamise ülesandeid täitvate või abil Utiliidid **Visual Studios**, **Web maatriksi**, **Azure PowerShelli** või **Azure käsurea liides (Azure'i CLI)**, nagu ülesannete automatiseerimine saate faili *sätete avaldamine* või *avaldamise profiili*. Nii failitüüpide autenditakse te Azure ja peaks olema kinnitatud volitamata juurdepääsu vältimiseks.

* **Avaldamine sätete** fail sisaldab

    * Oma Azure tellimuse ID

    * Juhtimise sert, mis võimaldab teil teha haldustoimingute oma tellimuse *ilma konto nimi või parool*.

* **Avaldamise profiili** fail sisaldab

    * Lisateavet rakenduse avaldamine

Kui kasutate kasuliku, mis kasutab avalda sätete fail või avalda profiili faili, importida fail, mis sisaldab avalda sätted või profiili kasuliku ja seejärel **kustutage** fail. Kui hoiate faili jagada teistega, näiteks projekti kallal talletada turvalises asukohas on *krüptitud* directory piiratud õigustega nt.

Lisaks veenduge, et imporditud identimisteave on kinnitatud. Näiteks **Azure PowerShelli** ja **Azure käsurea liides (Azure'i CLI)** talletamiseks imporditud teave **home directory** (*~* Linux või OS X süsteemide ja */users/yourusername* operatsioonisüsteemides Windows.) Turvalisuse suurendamiseks võib-olla soovite **krüptida** järgmistesse kohtadesse krüptimise saadaolevate tööriistade abil oma operatsioonisüsteemi.

### <a name="configuration-settings-and-connection-strings"></a>Otsingukonfiguratsiooni sätetest ja ühendusstringi
See on tavaks ühendusstringi, autentimiseks mandaadi ja muu tundliku teabe failid salvestada. Kahjuks need failid võib olla avatud veebisaidi või märgitud avaliku andmebaasi, asetades selle teabe. Lihtne otsing [GitHub](https://github.com), näiteks saate avastada exposed saladusi avaliku hoidlate lugematuid failid.

Parim on see teave hoida oma rakenduse failid. Rakenduse teenus võimaldab salvestada konfiguratsiooniteavet käitusaja keskkonna osana **rakenduse sätete** ja **ühendusstringi**. Rakenduse kaudu *keskkonna muutujate* enamik programmeerimise keelte käitusajal puutuvad väärtused. .NET rakenduste, sisestatakse neid väärtusi sisse oma .net-i konfigureerimine käitusajal. Teistel juhtudel nende sätete konfigureerimine jääb krüptitud juhul, kui saate vaadata või seadistada [Azure portaali](https://portal.azure.com) või Utiliidid nagu PowerShelli või Azure CLI abil. 

Rakenduse teenuses konfiguratsiooni teabe talletamise võimaldab rakenduse administraator lukustamiseks tundliku teabe tootmise rakendused alla. Arendajad abil konfiguratsioonisätted eraldi kogum rakenduste arendamiseni ning sätteid saate automaatselt asendatud sätteid konfigureerida rakendus teenuses. Isegi arendajad teadma saladusi tootmise rakenduse jaoks konfigureeritud. Rakenduse teenuses rakenduse sätete ja ühendusstringi konfigureerimise kohta leiate lisateavet teemast [konfigureerimine web apps](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Azure'i rakenduse teenus pakub turvaline FTP juurdepääs failisüsteemi kaudu **FTPS**oma rakenduse. Nii saate turvaliseks juurdepääsuks rakenduse koodi web Appi kui ka diagnostika logid. On soovitatav alati kasutada FTPS asemel FTP. 

FTPS lingi oma rakenduse leiate tehke järgmist:

1. Avage [Azure'i portaalis](https://portal.azure.com).
2. Valige **Sirvi kõiki**.
3. Keelest **Sirvi** , valige **Rakenduse teenused**.
4. **Rakenduse teenuste** keelest, valige soovitud rakendus.
5. Rakenduse tera, valige **Kõik sätted**.
6. Keelest **sätted** nuppu **Atribuudid**.
7. FTP ja FTPS linke on esitatud enne **sätted** . 

FTPS kohta leiate lisateavet teemast [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Järgmised sammud

Lisateavet turvalisuse Azure'i platvormi, aruandlus **Turvalisus juhtum väärkasutus**, või Microsoft teavitada, et sooritate **kättesaadavuse testimine** saidi, [Microsoft Azure'i usalduskeskuses](https://azure.microsoft.com/support/trust-center/security/)jaotisest turvalisus.

Rakenduse rakendused **web.config** või **applicationhost.config** failide kohta leiate lisateavet teemast [konfiguratsiooni suvandid lukustamata Azure'i rakendust Service veebirakendustes](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Rakenduse teenuse rakendusi, mis võib olla kasulik avastada eest, kohta logiteabe kohta teavet teemast [diagnostikalogimise lubamiseks](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud

* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)
