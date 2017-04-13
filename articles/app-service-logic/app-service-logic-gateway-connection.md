<properties
   pageTitle="Loogika rakenduste kohapealse lüüsi andmeühenduse | Microsoft Azure'i"
   description="Teavet selle kohta, kuidas luua ühendus lüüsiga kohapealse andmete loogika rakenduse kaudu."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Ühenduse loomiseks kohapealse andmete lüüsi loogika rakendused

Toetatud loogika rakenduste konnektorid võimaldavad teil konfigureerida ühenduse loomiseks Accessi kohapealsed andmed kohapealse andmete lüüsi kaudu.  Järgmised toimingud juhendab teid kuidas installida ja konfigureerida kohapealse andmete lüüsi loogika rakenduse töötamiseks.

## <a name="prerequisites"></a>Eeltingimused

* Peate abil töö või kooli meiliaadress Azure kohapealse andmete lüüsi seostada oma konto (Azure Active Directory põhine)
    * Kui kasutate Microsofti Account (nt @outlook.com, @live.com) Azure'i konto abil saate luua töö või kooli meiliaadress, järgides [juhiseid siin](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)

> [AZURE.WARNING] Pole praegu piirangud, mida kohapealse lüüsi installi ainult täielik kui kontoga, millel on registreerunud Power BI.  Registreeruge vahepeal mis tahes konto "Power BI tasuta" installimine lõpule viia.

* Peab olema asutusesisese andmete lüüsi [kohalikku arvutisse installitud](app-service-logic-gateway-install.md).
* Lüüsi peab ei nõuda ([taotlemine juhtub loomise etappi 2 all](#2-create-an-azure-on-premises-data-gateway-resource)) teise Azure kohapealse andmete lüüsi - installi saab ainult ühe lüüsi ressursi seostada.

## <a name="installing-and-configuring-the-connection"></a>Kui installite ja ühenduse konfigureerimine

### <a name="1-install-the-on-premises-data-gateway"></a>1. kohapealse andmete lüüsi installimine

Teavet kohapealse andmete lüüsi installimise kohta leiate [selle artikli](app-service-logic-gateway-install.md).  Lüüsi peab olema installitud mõni kohapealse arvutisse enne jätkamist ülejäänud juhised.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Azure kohapealse andmete lüüsi ressursside loomine

Pärast installimist tuleb seostada kohapealse andmete lüüsi Azure tellimuse.

1. Azure'i sama töö või kooli meiliaadress, mida kasutati lüüsi installimise ajal abil sisse logida
1. Klõpsake nuppu **Uus** ressurss
1. Leidke ja valige **kohapealse andmete lüüsi**
1. Sisestage lüüsi seostada oma konto – sh, valides sobiva **Installi nimi** teave

    ![Kohapealse andmeühenduse lüüsi][1]
1. Ressursi loomiseks nuppu **Loo**

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. designer loogika rakenduse ühenduse loomine

Nüüd, kui Azure tellimuse on seotud kohapealse andmete lüüsi eksemplari, saate luua ühenduse selle loogika rakenduses.

1. Avage loogika rakendus ja valige konnektor, mis toetab kohapealse Ühenduvus (kirjutamise, SQL Server)
1. Märkige ruut ühenduse loomiseks **kohapealse andmete lüüsi kaudu**

    ![Loogika rakenduse Designer lüüsi loomine][2]
1. Valige **lüüsi** ühenduse ja muude ühenduseteavet nõutav lõpuleviimine
1. Klõpsake nuppu **Loo** ühenduse loomine

Ühendus peaks nüüd edukalt loogika rakenduse jaoks konfigureeritud.  

## <a name="next-steps"></a>Järgmised sammud
- [Levinud näited ja stsenaariumid loogika rakendused](app-service-logic-examples-and-scenarios.md)
- [Ettevõtte Kliendiintegratsiooni funktsioonide](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG