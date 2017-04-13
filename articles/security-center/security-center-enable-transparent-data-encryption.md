<properties
   pageTitle="Läbipaistva andmete krüptimine Azure'i Security Center | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **Lubada läbipaistvaid andmete krüptimist**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Läbipaistva andmete krüptimine Azure'i Security Center

Azure'i turbekeskus soovitada lubamine läbipaistev krüptimise (TDE) SQL andmebaase kui TDE pole veel lubatud. TDE kaitseb teie andmeid ja aitab teil oma andmebaasi, seotud varukoopiate ja tehingulogi failide veebisaidil ülejäänud, krüptides nõudmata rakenduse muudatuste nõuetele vastavuse. Lisateavet leiate [Läbipaistvaid andmete krüptimine koos Azure'i SQL-andmebaasi](https://msdn.microsoft.com/library/dn948096).

See soovitus kehtib Azure SQL-i teenuse ainult; SQL-i töötab teie virtuaalmasinates ei sisalda.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused** tera, valige **Luba läbipaistev andmete krüptimist**.
![Läbipaistva andmete krüptimine][1]

2. Avatakse tera **Lubada läbipaistvaid andmete krüptimine SQL andmebaase** . Valige SQL-andmebaasi TDE lubamiseks.
![Valige SQL DB TDE lubamiseks][2]
3. Enne **läbipaistvaks andmete krüptimist** , **ON** jaotises andmete krüptimist ja valige **Salvesta** ülemise lindil tera.
![TDE sisselülitamine][3]

  TDE lubamisel valitud SQL-andmebaasi **krüptimine olek** muutub **krüptitud**.    

  ![Krüptimise olek][4]

## <a name="see-also"></a>Vt ka

Selles artiklis ilmnes saate kuidas rakendada turbekeskus soovitust "Luba läbipaistev andmete krüptimine." TDE SQL-i kohta lisateabe saamiseks vaadake järgmist:

- [Läbipaistva andmete krüptimine SQL Azure'i andmebaas](https://msdn.microsoft.com/library/dn948096)
- [Töö alustamine läbipaistev andmete krüptimise (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
