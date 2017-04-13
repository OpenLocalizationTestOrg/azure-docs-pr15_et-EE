<properties
   pageTitle="Windows Serveri või Windowsi kliendi andmete taastamine Azure'i ressursihaldur juurutamise näidise | Microsoft Azure'i"
   description="Saate teada, kuidas taastada Windows Serveri või Windowsi kliendi."
   services="backup"
   documentationCenter=""
   authors="saurabhsensharma"
   manager="shivamg"
   editor=""/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
     ms.date="08/02/2016"
     ms.author="trinadhk; jimpark; markgal;"/>

# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Failide taastamiseks Windows Serveri või Windowsi kliendi seadme ressursihaldur juurutamise mudeli kasutamine

> [AZURE.SELECTOR]
- [Azure'i portaal](backup-azure-restore-windows-server.md)
- [Klassikaline portaal](backup-azure-restore-windows-server-classic.md)

Selles artiklis käsitletakse toiminguid, täitma taastamine kaht tüüpi:

- Andmete taastamine, mis pärinevad varukoopiaid samasse arvutisse.
- Mis tahes muu seadme andmeid taastada.

Mõlemal juhul tuuakse andmed hoidlast Azure taastamise teenused.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

## <a name="recover-data-to-the-same-machine"></a>Kui soovite samasse arvutisse andmete taastamine
Kui teid kogemata kustutatud faili taastamiseks (kust varukoopia võetakse) samasse arvutisse, järgmised juhised aitavad teil andmeid taastada.

1. Avage **Microsoft Azure varukoopia** snap.
2. Klõpsake töövoo käivitamiseks **Andmete taastamine** .

    ![Andmete taastamine](./media/backup-azure-restore-windows-server/recover.png)

3. Valige soovitud * *see server (*yourmachinename*) ** võimaluse taastada varundatud samasse arvutisse fail üles.

    ![Samasse arvutisse](./media/backup-azure-restore-windows-server/samemachine.png)

4. Valige **Otsi faile** või **failide otsimine**.

    Kui plaanite taastada üks või mitu faili, mille tee on teada, jätke vaikesuvand. Kui te ei ole kindel kausta ülesehitust, kuid soovite otsida faili, valige suvand **failide otsimine** . Käesolevas jaotises jätkame koos vaikesuvand.

    ![Failide sirvimine](./media/backup-azure-restore-windows-server/browseandsearch.png)

5. Valige, kust soovite taastada faili maht.

    Saate taastada mis tahes ajal. Kuupäevad, mis **paks** kalender kontrolli näitavad taastamine punkti olemasolu. Kui valitud on kuupäev, põhineb varunduse ajakava (ja varukoopia toimingu õnnestumise), saate valida punkti **kellaaeg** rippmenüüst aeg alla.

    ![Helitugevuse ja kuupäev](./media/backup-azure-restore-windows-server/volanddate.png)

6. Valige üksusi taastada. Saate hulgivaliku kaustad/failid, mida soovite taastada.

    ![Failide valimine](./media/backup-azure-restore-windows-server/selectfiles.png)

7. Määrake taastamine parameetrid.

    ![Taastesuvandeid](./media/backup-azure-restore-windows-server/recoveroptions.png)

  - Teil on võimalus taastamiseks selle algsesse asukohta (kus faili/kausta tuleks üle kirjutada) või mõnda muusse asukohta samasse arvutisse.
  - Kui sihtkoht on olemas faili/kausta, mida soovite taastada, saate luua eksemplaride (kaks versiooni sama faili), kirjutada sihtkoht failid või faile, mis Sihtvaluuta taastamise vahele jätta.
  - See on soovitatav jätta vaikesuvand ACL-ID, mis on on taastatud faile taastada.

8. Pärast nende sisendeid on olemas, klõpsake nuppu **edasi**. Taastamine töövoo, mis taastab faile selles arvutis, hakkavad.

## <a name="recover-to-an-alternate-machine"></a>Mõne alternatiivse arvutis taastamine
Kui teie kogu server läheb kaotsi, saate siiski taastada andmete Azure varukoopia soovite mõnda teise arvutisse. Järgmised sammud kirjeldavad töövoo.  

Kasutatakse järgmist terminid sisaldab järgmist:

- *Andmeallika kohapeal* – algse arvutisse, mis võeti varundamine ja mis on praegu saadaval.
- *Sihtarvutis* -arvuti, kuhu andmete taastamine.
- *Valimi vault* – taastamise teenused vault, mis on registreeritud *Allika arvuti* ja *sihtarvutis* . <br/>

> [AZURE.NOTE] Varukoopiate tehtud seadmes ei saa taastada arvutisse, kus töötab operatsioonisüsteem varasemas versioonis. Näiteks kui varukoopiate võetakse Windows 7 arvutist, taastamist opsüsteemis Windows 8 või seadme kohale. Aga ka vastupidi ei kehti tõene.

1. Avage **Microsoft Azure varukoopia** snap *sihtarvutis*.
2. Veenduge, et *sihtarvutis* ja *Allika kohapeal* registreeritud sama taastamise teenused vault.
3. Klõpsake töövoo käivitamiseks **Andmete taastamine** .

    ![Andmete taastamine](./media/backup-azure-restore-windows-server/recover.png)

4. Valige **muu server**

    ![Mõne muu Server](./media/backup-azure-restore-windows-server/anotherserver.png)

5. *Valimi vault*vault mandaati faili, mis vastab pakkumiseks. Kui vault mandaati fail on sobimatu (või aegunud) faili alla laadida uue vault mandaati *valimi vault* Azure'i portaalis. Kui vault mandaati fail on saadaval, kuvatakse taastamise teenused vault vault mandaati faili suhtes.

6. Valige loendis kuvatud masinad *Allika kohapeal* .

    ![Loendi masinad](./media/backup-azure-restore-windows-server/machinelist.png)

7. Valige suvand **failide otsimine** või **failide sirvimine** . Käesolevas jaotises kasutame otsingut **failide jaoks** .

    ![Otsing](./media/backup-azure-restore-windows-server/search.png)

8. Valige järgmisel kuval helitugevust ja kuupäeva. Otsige soovite taastada kausta/faili nime.

    ![Üksuste otsimine](./media/backup-azure-restore-windows-server/searchitems.png)

9. Valige asukoht, kus failid tuleb taastada.

    ![Asukoha taastamine](./media/backup-azure-restore-windows-server/restorelocation.png)

10. Sisestage *Allika seadme* registreerimise käigus antud *valimi vault*krüptimise parool.

    ![Krüptimine](./media/backup-azure-restore-windows-server/encryption.png)

11. Kui sisendi on esitatud, klõpsake **taastada**, mis käivitab taastada varundatud failide esitatud sihtkohta.

## <a name="next-steps"></a>Järgmised sammud
- Nüüd, kui te olete taastatud faile ja kaustu, saate [hallata oma varukoopiad](backup-azure-manage-windows-server.md).
