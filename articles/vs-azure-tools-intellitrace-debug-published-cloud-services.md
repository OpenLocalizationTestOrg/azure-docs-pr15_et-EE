<properties 
   pageTitle="Avaldatud pilveteenuses IntelliTrace ja Visual Studio silumine | Microsoft Azure'i"
   description="Avaldatud pilveteenuses IntelliTrace ja Visual Studio silumine"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Avaldatud pilveteenuses IntelliTrace ja Visual Studio silumine

##<a name="overview"></a>Ülevaade

IntelliTrace, kus saate sisse logida olulisel silumine teavet rolli eksemplari Azure käitamisel. Kui teil on vaja probleemi põhjuse, saate juhis oma koodi kaudu Visual Studio, kui see ei tööta Azure IntelliTrace logid. IntelliTrace kirjete võti koodi täitmine ja keskkonna andmete kui Azure rakenduse töötab pilveteenus Azure ja võimaldab teil kordus Visual Studio salvestatud andmeid. Teise võimalusena saate Kaug silumine pilveteenuses, kus töötab Azure manustamiseks. Vt [silumine pilveteenused](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace on mõeldud ainult silumine stsenaariumid ja tootmise juurutamiseks ei tohi kasutada.

>[AZURE.NOTE] Saate kasutada, kui teil on installitud Visual Studio Enterprise IntelliTrace ja oma Azure rakenduse sihtkohtade .NET Framework 4 või uuem versioon. IntelliTrace kogub teavet oma Azure rollid. Virtuaalmasinates administraatorirollid alati käivitada 64-bitiste opsüsteemide jaoks.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Konfigureerida Azure rakenduse IntelliTrace

Azure'i rakenduse IntelliTrace lubamiseks peate looma ja rakenduse Visual Studio Azure'i projekti avaldada. IntelliTrace tuleb konfigureerida Azure rakenduse enne selle avaldamist Azure. Kui avaldamine IntelliTrace konfigureerimata rakenduse, kuid seejärel Otsustage, kas soovite seda teha, on teil rakenduse Visual Studio uuesti avaldada. Lisateavet leiate teemast [avaldamise pilveteenus Azure tööriistade abil](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Kui olete valmis kasutama Azure'i rakenduse, veenduge, et oma projekti koostamine eesmärgid on seatud **silumine**.

1. Azure'i projekti kiirmenüü avamine Solution Exploreris ja valige käsk **Avalda**.
 
    Kuvatakse avaldada Azure taotluse viisard.

1. Koguda IntelliTrace logid rakenduse, kui see on avaldatud pilves, märkige ruut **Luba IntelliTrace** .

    >[AZURE.NOTE] Saate lubada IntelliTrace või profiilide Azure rakenduse avaldamisel. Te ei saa kasutada mõlemat.

1. Tavaline IntelliTrace konfiguratsiooni kohandamiseks valige **sätted** hüperlink.

    Kuvatakse dialoogiboks IntelliTrace sätted, nagu on näidatud järgmisel joonisel. Saate määrata, millised sündmuste logi, kas kõne teabe kogumiseks, millised moodulid ja protsesside kogumiseks logib jaoks ja kui palju ruumi eraldada salvestise. IntelliTrace kohta leiate lisateavet teemast [silumine IntelliTrace abil](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

IntelliTrace Logi on ringikujulise logifaili (vaikimisi suurus on 250 MB) IntelliTrace sätetes määratud maksimaalse suurusega. IntelliTrace logid kogutakse faili virtuaalse masina failisüsteemis. Kui taotluse logid, hetktõmmise sel hetkel aega võtta ja teie kohalikku arvutisse alla laadida.

Pärast Azure rakendus on avaldatud Azure, saate teada, kui IntelliTrace on lubatud Azure Arvuta sõlme Server Explorer, nagu on näidatud järgmisel pildil:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>IntelliTrace logid rolli eksemplari allalaadimine

IntelliTrace logid rolli eksemplari jaoks saate alla laadida **Pilveteenustega** sõlm **Server**Explorer. Laiendage **Pilveteenused** seni, kuni olete huvitatud eksemplari leidmist, töövooeksemplari kiirmenüü avamine ja valige **Vaate IntelliTrace logid**. Logide IntelliTrace laaditakse faili kataloogis kohalikus arvutis. Iga kord, kui selle IntelliTrace juhised.taotluse esitamisel logib, luuakse uus hetktõmmis.

Kui logid laaditakse, kuvatakse Visual Studio toimingu edenemist aknas Azure'i tegevuste Logi. Nagu on näidatud järgmisel joonisel, saate laiendada reaüksuse täpsemaks vaatamiseks toimingu jaoks.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Saate jätkata töötamine Visual Studios, kui laadite IntelliTrace logid. Kui allalaadimine on lõppenud Logi, siis avatakse automaatselt Visual Studio.

>[AZURE.NOTE] IntelliTrace logid võivad sisaldada erandid, mis loob ja seejärel tegeleb raames. Sisemise framework kood tekitab erandite käivitamine rollid, nii, et neid võib turvaline ignoreerida tavaline osana.

## <a name="see-also"></a>Vt ka

[Pilveteenustega silumine](https://msdn.microsoft.com/library/ee405479.aspx)

