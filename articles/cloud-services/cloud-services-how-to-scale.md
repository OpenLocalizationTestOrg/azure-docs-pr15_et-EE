<properties
    pageTitle="Automaatne mastaapimiseks pilveteenus portaalis | Microsoft Azure'i"
    description="(klassikaline) Saate teada, kuidas kasutada klassikaline portaali konfigureerida Azure automaatne skaala reeglid pilvepõhise teenuse web rolli või töötaja roll."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Kuidas automaatne skaala pilveteenus

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-scale-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-scale.md)

Azure'i klassikaline portaalis lehel skaala saate käsitsi muudate oma web rolli või töötaja rolli või saate lubada automaatset mastaapimist protsessorit või sõnumi järjekorda.

>[AZURE.NOTE] See artikkel keskendub pilveteenuses veebi ja töötaja rollid. Kui loote virtuaalse masina (klassikaline) otse, see on majutatud mõnes pilveteenuses. Osa sellest teabest kehtib järgmist tüüpi virtuaalmasinates. Skaleerimist kättesaadavus kogumi virtuaalmasinates on tõesti ainult sulgumist neid sisse ja välja põhjal saate konfigureerida skaala reegleid. Virtuaalmasinates ja -saadavus komplektid kohta leiate lisateavet teemast [haldamine Virtuaalmasinates kättesaadavus](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Enne konfigureerida rakenduse skaleerimist arvestage järgmist teavet.

- Skaleerimist ei mõjuta core kasutus. Suurema rolliga eksemplari kasutada rohkem tuuma. Muudate ainult piires valdkond rakenduse tellimuse. Näiteks kui teie tellimus on kuni kakskümmend ja -vormid saate käivitada rakenduse koos kahe keskmise suurusega cloud services (kokku neli valdkond), saate ainult skaalal muude pilvepõhise teenuse juurutuste teie tellimus 16 puursüdamikproovide. Vt [Pilvepõhise teenuse suurused](cloud-services-sizes-specs.md) suurused kohta lisateavet.

- Peate looma järjekorda ja seostada selle rolli enne, kui muudate rakenduse aluseks on sõnumi. Lisateabe saamiseks vaadake, [Kuidas kasutada salvestusteenus järjekorda](../storage/storage-dotnet-how-to-use-queues.md).

- Saate skaala ressursse, mis on lingitud teie pilveteenusesse. Ressursside linkimise kohta leiate lisateavet teemast [kohta: Link ressursi pilveteenusesse](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Rakenduse kättesaadavuse lubamiseks veenduge, et see on juurutatud kahe või enama rolli aknad. Lisateavet leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Ajakava mastaapimine

Vaikimisi kõikide rollide järgi teatud ajakava. Järelikult ühtegi sätet muuta rakendada kogu aeg ja kõik päevad kogu aasta. Kui soovite, saate häälestada käsitsi või automaatse skaleerimist jaoks:

- Nädalapäevade
- Nädalavahetustel
- Nädala ööks
- Nädala hommikul
- Kindlad kuupäevad
- Konkreetseid kuupäevavahemikke

See on conigured [Azure klassikaline portaalis](https://manage.windowsazure.com/) on  
**Cloud Services** > **\[oma pilveteenuses\]** > **skaala** > **\[tootmise või lavastus\] ** lehe.

Klõpsake nuppu **häälestamine ajakava korda** iga rolli, mida soovite muuta.

![Pilveteenuse teenuse automaatne skaleerimist põhjal ajakava][scale_schedules]



## <a name="manual-scale"></a>Käsitsi skaala

Klõpsake lehel **skaala** saate käsitsi suurendada või vähendada arvu töötavad eksemplarid mõnes pilveteenuses. See on konfigureeritud iga ajakava on loodud või kogu aeg, kui te pole loonud ajakava.

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com/), klõpsake **Pilveteenustega**, ja klõpsake pilveteenuses avamiseks armatuurlaua nime.

    > [AZURE.TIP] Kui te ei näe oma pilveteenuses, peate muuta **tootmise** **lavastus** või vastupidi.

2. **Klõpsake käsku.**

3. Valige ajakava, mida soovite skaleerimise suvandite muutmine. Vaikimisi *ei ajastatud korda* kui teil pole määratletud ajakava.

4. Leidke jaotis **skaala meetermõõdustik,** valige **NONE**. See on kõikide rollide vaikesätteks.

5. Iga rolli pilveteenusesse, on liugur muutmise kasutada eksemplaride arv.

    ![Käsitsi mastaapimiseks pilvepõhise teenuse roll][manual_scale]

    Kui teil on vaja mitu eksemplari, peate [pilvepõhise teenuse virtuaalse masina suuruse](cloud-services-sizes-specs.md)muutmiseks.

6. Klõpsake nuppu **Salvesta**.  
Rolli aknad saab lisada või eemaldada põhjal soovitud valikud.

>[AZURE.TIP] Kui näete ![][tip_icon] Viige hiirekursor sellele ja saate abi kohta, milliseid kindlat sätet ei.


## <a name="automatic-scale---cpu"></a>Automaatse skaalal - CPU

See muudab suurust, kui keskmine protsent CPU hõivatus läheb üles või alla määratud lävede; rolli aknad on loodud või kustutatud.

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com/), klõpsake **Pilveteenustega**, ja klõpsake pilveteenuses avamiseks armatuurlaua nime.

    > [AZURE.TIP] Kui te ei näe oma pilveteenusesse, peate muuta **tootmise** **lavastus** või vastupidi.

2. **Klõpsake käsku.**

3. Valige ajakava, mida soovite skaleerimise suvandite muutmine. Vaikimisi *ei ajastatud korda* kui teil pole määratletud ajakava.

4. Leidke jaotis **skaala meetermõõdustik,** valige **CPU**.

5. Nüüd saate konfigureerida rollid eksemplari, target CPU hõivatus (käivitamiseks skaala üles) ja mitu eksemplari skaala üles ja alla miinimum- ja lahtrivahemik.

![Mastaapimiseks pilvepõhise teenuse rolli alusel protsessorit][cpu_scale]

>[AZURE.TIP] Kui näete ![][tip_icon] Viige hiirekursor sellele ja saate abi mõne kindla säte ei kohta.





## <a name="automatic-scale---queue"></a>Automaatse skaalal - järjekord

See automaatselt skaala kui sõnumite järjekorras läheb üles või alla määratud; rolli aknad on loodud või kustutatud.

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com/), klõpsake **Pilveteenustega**, ja klõpsake pilveteenuses avamiseks armatuurlaua nime.

    > [AZURE.TIP] Kui te ei näe oma pilveteenuses, peate muuta **tootmise** **lavastus** või vastupidi.

2. **Klõpsake käsku.**

3. Leidke jaotis **skaala meetermõõdustik,** valige **CPU**.

4. Nüüd saate konfigureerida rollid eksemplari, järjekord ja suurus järjekorda sõnumid töödelda iga eksemplari ja mitu eksemplari skaala üles ja alla miinimum- ja mitmesuguseid.

![Mastaapimiseks pilvepõhise teenuse rolli alusel sõnumite järjekord][queue_scale]

>[AZURE.TIP] Kui näete ![][tip_icon] Viige hiirekursor sellele ja saate abi mõne kindla säte ei kohta.


## <a name="scale-linked-resources"></a>Mastaapimiseks lingitud ressursid

Sageli siis, kui muudate rollile, on kasulik mastaapimiseks andmebaas, mida rakendus kasutab ka. Kui lingite andmebaasi pilveteenusesse, pääsete juurde selle ressursi skaleerimise sätted, klõpsates vastavat linki.

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com/)klõpsake **Pilveteenustega**ning klõpsake pilveteenusesse avamiseks armatuurlaua nime.

    > [AZURE.TIP] Kui te ei näe oma pilveteenuses, peate muuta **tootmise** **lavastus** või vastupidi.

2. **Klõpsake käsku.**

3. Leidke **lingitud ressursid** jaotis ja klõpsanud **selle andmebaasi haldamine skaala**.

    > [AZURE.NOTE] Kui **lingitud ressursid** jaotist ei kuvata, pole teil ilmselt lingitud ressursse.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
