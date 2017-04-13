<properties
 pageTitle="Asjade jaoturi seadme juhtimise ülevaade | Microsoft Azure'i"
 description="Selles artiklis antakse ülevaade sellest, seadmehalduse kasutamist teenuses Azure asjade jaoturi: enterprise seadme elutsükli, taaskäivitage arvuti, factory reset, püsivara värskendus, konfiguratsiooni, seadme kaksikud, päringute, tööde haldamine"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Ülevaade: Azure'i asjade jaoturi mobiilsideseadmete halduse (eelvaade)

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi pakub funktsioone ja laiendatavuse mudelit, mis seadmest ja tagaandmebaas arendajatel luua töökindlaid asjade seadme lahenduste lubamine. Asjade seadmete ulatuvad piiratud andurid ja ühe eesmärgi mikrokontrollerid, võimsaid lüüsid, mis marsruutimine suhtlus rühmade seadmete jaoks.  Lisaks kasutamine juhtudel ja asjade tehtemärgid nõuded oluliselt erineda tööstusharudes.  Hoolimata järgmist varianti pakub Azure'i asjade jaoturi mobiilsideseadmete halduse võimaluste, mustrite ja koodi teekide abil erinevaid komplekti seadmed ja lõppkasutajad sobi.

Oluline osa loomise eduka ettevõtte asjade lahenduse strateegia ette kuidas tehtemärgid toime jooksvate haldustoimingute oma kogumise seadmed. Asjade operaatorid nõuavad lihtne ja usaldusväärne tööriistade ja rakenduste, mis võimaldavad keskenduda rohkem strategic aspektide oma töö. Selles artiklis on toodud:

- Azure'i asjade jaoturi lähenemisviisi mobiilsideseadmete halduse lühiülevaade.
- Levinud seadme põhimõtted kirjeldus.
- Seadme elutsükli kirjeldus.
- Levinud seadme halduse mustrite ülevaade.

## <a name="iot-device-management-principles"></a>Asjade seadme põhimõtted

Asjade toob kaasa ainulaadse seadmes juhtimise väljakutsed ja iga äriklassi lahendus peab käsitlema järgmisi põhimõtteid.

![Azure'i asjade jaoturi seadme halduse põhimõtted pilt][img-dm_principles]

- **Ulatuse ja automatiseerimise**: asjade lahendused nõuavad lihtsa tööriistu, mida saate tavalised automatiseerida ja lubada suhteliselt toimingute töötajate miljoneid seadmete haldamine. Igapäevane, tehtemärkide oodata toime seadme toimingute kaugühenduse teel, mitmekaupa ja ainult hoiatada, kui probleeme tekkida, mille jaoks on vaja oma otsest tähelepanu.

- **Avatud ja ühilduvus**: The asjade seadme ökosüsteem on eriti mitmesuguse. Haldusriistad peab kohandatud mahutamiseks hulgaliselt seadme tunnid, platvormid ja protokollid. Tehtemärkide peate saama toetab mitut tüüpi seadmete kaudu on kõige piiratud ühe-protsess kiipide, võimas ja töökorras arvutisse.

- **Konteksti teadlikkuse**: asjade keskkonnas on dünaamiline ja -muutuvad. Usaldusväärne teenus on äärmiselt oluline. Seadme toimingute peab tegur SLA hooldustööd windows, olekus võrgu ja power, kasutusel tingimused ja seadme geograafiline asukoht tagada selle hooldustööd tööseisakute ei mõjuta kriitilised äritegevuse ega luua ohtliku tingimused.

- **Teenuse palju rolle**: kordumatu Töövood ja protsesside asjade toimingute rollidest tugi on oluline. Toimingute töötajad tuleb töötada harmooniliselt antud piiranguid sisemise selle osakondade.  Need tuleb leida ka säästvalt Surface'i reaalajas seadme toimingute teabe järelevalve ja muud äri juhtimis rollid.

## <a name="iot-device-lifecycle"></a>Asjade seadme elutsükkel

On üldine seadme etapi ühised ettevõtte kõigi asjade projektide kogum. Azure'i asjade, on viis etappi asjade seadme elutsükli jooksul.

![Viis Azure'i asjade seadme elutsükli etapist: kavandamine, ettevalmistamine, konfigureerida, jälgimine, pensionile][img-device_lifecycle]

Igas viis etapil on mitu seadet tehtemärk nõuetele, mis peavad olema täidetud täielik lahendus.

- **Kavandamine**: ettevõtjatele seadme metaandmete värviskeemi, mis võimaldab neid hõlpsasti ja täpselt päringu ja suunata seadmete hulgi toimingute rühma loomiseks. Seadme Double abil saate talletada selle seadme metaandmeid sildid ja atribuudid.

    *Välislingid*: [Alustamine seadme kaksikud][lnk-twins-getstarted], [mõistmine seadme kaksikud][lnk-twins-devguide], [Kuidas kasutada Double atribuudid][lnk-twin-properties]

- **Sätte**: turvaliselt ettevalmistamise uute seadmete asjade jaoturi ja ettevõtjatele kohe avastada seadme võimalustest.  Asjade jaoturi seadme registri abil saate luua paindlik seadme identiteedid ja mandaadi ja selle toimingu mitmekaupa abil tööd. Koostada seadmete teatada nende võimaluste ja seadme atribuutide Double seadme kaudu.

    *Välislingid*: [haldamine seadme identiteedid][lnk-identity-registry], [hulgi seadme identiteetide haldus][lnk-bulk-identity], [Kuidas kasutada Double atribuudid][lnk-twin-properties]

- **Konfigureerimine**: hõlbustada hulgi konfiguratsioonimuudatuste ja püsivara värskendused ja seadmed, säilitades nii seisundi ja turvalisus. Toiminguid nende seadme halduse mitmekaupa abil soovitud atribuudid või otsene meetodite ja leviedastuse tööde haldamine.

    *Välislingid*: [kasutada otsest meetodit][lnk-c2d-methods], [Invoke otsene meetod seadmes][lnk-methods-devguide], [kasutamine Double atribuudid][lnk-twin-properties], [ajakava ja leviedastuse töökohtade][lnk-jobs], [ajakava tööde haldamine mitmes seadmes][lnk-jobs-devguide]

- **Kuvari**: Üldine seadme saidikogumi seisundi, poolelioleva toimingute oleku jälgimine ja Teavita probleemidele tähelepanu nõudvate tehtemärke.  Rakendage seadme Double seadmed aruande reaalajas tingimusi ja oleku värskendus toimingute lubamiseks. Luua võimsaid armatuurlaua aruandeid, mis pinna kõige otsesem probleeme seadme Double päringute abil.

    *Välislingid*: [kasutamine Double atribuudid][lnk-twin-properties], [päringukeele kaksikud ja-tööde haldamine][lnk-query-language]

- **Retire**: asendamine või eemaldada seadmete pärast tõrke, uuendada tsükli, või teenuse eluiga lõpus.  Kasutage seadme Double säilitamiseks seadme teave, kui seadme on asendatud või arhiivitud, kui on kasutuselt kõrvaldatud. Kasutage asjade jaoturi seadme registri turvaliselt tühistamiseks seadme identiteedid ja mandaadi.

    *Välislingid*: [Kuidas kasutada Double atribuudid][lnk-twin-properties], [haldamine seadme identiteedid][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>Asjade jaoturi seadme halduse mustrite

Asjade jaoturi võimaldab seadme halduse mustrite järgmised kogum.  [Seadme halduse õpetused] [ lnk-get-started] näete täpsemalt, kuidas neid mustreid täpse stsenaariumist mahutamiseks laiendada ja kuidas kujundada uue core need mallide põhjal mustrite.

- **Taaskäivitage** - tagaandmebaas rakenduse teavitab seadme kaudu otsene algatatud uuesti.  Kasutab seade seadme Double teatatud värskendamiseks taaskäivitage arvuti oleku seadme atribuudid.

    ![Azure'i asjade jaoturi mobiilsideseadmete halduse taaskäivitage graafiline muster][img-reboot_pattern]

- **Factory lähtestamine** - tagaandmebaas rakenduse teavitab seadme kaudu otsene algatatud factory lähtestamine.  Kasutab seade seadme Double teatatud atribuutide värskendamine on factory Lähtesta olek seade.

    ![Azure'i asjade jaoturi seadme halduse factory lähtestamine graafiline muster][img-facreset_pattern]

- **Konfiguratsiooni** - tagaandmebaas rakendus kasutab konfigureerida tarkvara töötab seade seadme Double soovitud atribuudid.  Kasutab seade seadme Double teatatud atribuutide värskendamine konfiguratsiooni seadme olek.

    ![Azure'i asjade jaoturi seadme halduse konfigureerimine mustri pilt][img-config_pattern]

- **Püsivara värskendus** - tagaandmebaas rakenduse teavitab seadme kaudu otsene algatatud püsivara värskendus.  Seadme alustab multistep protsessi laadige püsivara, püsivara paketi ja lõpuks uuesti asjade jaoturi teenusega.  Kogu mult-sammult kasutab seade seadme Double teatatud atribuutide värskendamine ja seadme olekut.

    ![Azure'i asjade jaoturi seadme halduse püsivara värskendada graafiline muster][img-fwupdate_pattern]

- Seadme Double päringud, **aruandlus ja olekut** - rakenduse tagaandmebaas käivitatakse üle seadmete teatada oleku ja edenemise töötavate seadmete toimingute kogum.

    ![Azure'i asjade jaoturi mobiilsideseadmete halduse aruandlus ja olekut mustri pilt][img-report_progress_pattern]

## <a name="next-steps"></a>Järgmised sammud

Saate kasutada võimalusi, mustrite ja koodi teekide, leiate Azure'i asjade jaoturi mobiilsideseadmete halduse, luua asjade rakendusi, mis vastavad igas seadmes elutsükli enterprise asjade tehtemärk nõuetele.

Rohkem õppematerjale Azure'i asjade jaoturi seadme halduse funktsioonide kohta, leiate [Azure'i asjade jaoturi mobiilsideseadmete halduse alustamine] [ lnk-get-started] õpetuse.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md