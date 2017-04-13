<properties
   pageTitle="Konnektor versiooniajalugu väljaanne | Microsoft Azure'i"
   description="Selles teemas on loetletud kõik versioonides konnektorid Forefront Identity Manager (FIM) ja Microsofti Identity Manager (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Konnektor väljaanne versiooniajalugu
Konnektorite Forefront Identity Manager (FIM) ja Microsofti Identity Manager (MIM) muudetakse sageli.

>[AZURE.NOTE]
See teema on ainult FIM ja MIM. Azure'i AD-ühenduse ei toetata järgmisi konnektorid.

Selles teemas loetletud ühendused, mis on välja kõik versioonid.

Seotud lingid:

- [Laadige uusimad konnektorid](http://go.microsoft.com/fwlink/?LinkId=717495)
- [Üldise LDAP konnektor](active-directory-aadconnectsync-connector-genericldap.md) dokumentides
- [Üldine SQL-i konnektor](active-directory-aadconnectsync-connector-genericsql.md) dokumentides
- [Web Services konnektor](http://go.microsoft.com/fwlink/?LinkID=226245) dokumentides
- [PowerShelli konnektor](active-directory-aadconnectsync-connector-powershell.md) dokumentides
- [Lotus Domino Connector](active-directory-aadconnectsync-connector-domino.md) dokumentides

## <a name="111170"></a>1.1.117.0
Välja: märts 2016

**Uue konnektor**  
Algse versiooni [Üldise SQL-i konnektor](active-directory-aadconnectsync-connector-genericsql.md).

**Uued funktsioonid:**

- Üldise LDAP konnektor:
    - Lisatud tugi delta import Isode.
- Web Services konnektor:
    - Värskendatud csEntryChangeResult tegevuste ja setImportErrorCode tegevuse lubamiseks objekti tagastatakse tagasi sync engine tõrked.
    - Värskendatud SAP6 ja SAP6User mallide kasutamine objekti taseme tõrge uued funktsioonid.
- Lotus Domino konnektor:
    - Ekspordi, tuleb teil ühe sertifitseerija aadressiraamatu kohta. Nüüd saate haldamise hõlbustamiseks kõik sertifitseerijate sama parool.

**Fikseeritud küsimustes:**

- Üldise LDAP konnektor:
    - IBM Tivoli DS, mõned viide atribuute ei tuvastatud õigesti.
    - Avatud LDAP delta importimise ajal, olid tühikud alguses ja lõpus stringide kärbitakse.
    - Novell ja NetIQ, teisaldatud objekti vahel organisatsiooniüksused/ümbriste ja samal ajal eksport ümber objekti nurjus.
- Web Services konnektor:
    - Kui veebiteenuse mitme näitajate osas jaoks sama sidumine, siis konnektor ei õigesti avastanud nende näitajate osas.
- Lotus Domino konnektor:
    - Ekspordi e-posti andmebaasi atribuudi nimi ei tööta.
    - Ekspordi mis nii lisada ja eemaldada rühma liige eksporditakse ainult lisatud liikmed.
    - Kui märkmete dokument on kehtetu (atribuudi isValid väärtuseks false) ja seejärel konnektor nurjub.

## <a name="older-releases"></a>Vanemate väljalasete
Enne veebruar 2016 konnektorid olid välja tugiteemad.

**Üldise LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597 mai 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, märts 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, jaanuar 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 mai
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, märts 2014

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 mai

**PowerShelli**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 mai

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597 mai 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, märts 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 August
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003 veebruaris 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, oktoobris 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534 2013 August

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
