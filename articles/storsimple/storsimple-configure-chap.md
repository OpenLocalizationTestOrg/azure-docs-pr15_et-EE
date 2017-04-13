<properties 
   pageTitle="Seadme StorSimple CHAP konfigureerimine | Microsoft Azure'i"
   description="Kirjeldab, kuidas konfigureerida StorSimple seadme ülesanne käepigistus autentimise Protocol (CHAP)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Seadme StorSimple CHAP konfigureerimine

Selle õpetuse selgitatakse, kuidas konfigureerida CHAP StorSimple seadme. Selles artiklis kirjeldatud juhiseid kehtib StorSimple 8000 sarja samuti StorSimple 1200 seadmed.

Peatükk tähistab ülesanne käepigistus autentimine protokolli. See on autentimise skeem, mis kasutavad serverid remote klientide identiteedi kontrollimine. Kinnitamine põhineb ühiskasutusega parooli või salajane. CHAP võib olla ühesuunalise (Ühesuunalised) või vastastikune (kahesuunaline). Ühesuunalise CHAP on kui sihtkohas autendib algataja. Vastastikune või tagant CHAP, nõuab teiselt, sihtkohas autentida algataja ja seejärel algataja autentida sihtkohas. Algataja autentimist saab rakendada target autentimist. Siiski target autentimist saab rakendada ainult juhul, kui rakendatakse algataja autentimist. 

Hea tava, soovitame kasutada CHAP iSCSI turvalisuse täiustamiseks.

>[AZURE.NOTE] Pidage meeles, et IPSEC ei toeta praegu StorSimple seadmetes.

StorSimple seadme CHAP sätteid saab konfigureerida järgmisel viisil:

- Ühesuunalised või ühesuunalise autentimine

- Kahesuunaline või vastastikune või tagant autentimine

Iga juhul, portaali seadme ja iSCSI algataja serveritarkvara peab olema konfigureeritud. Üksikasjalikud juhised selle konfiguratsiooni puhul on kirjeldatud järgmises õpetuse.

## <a name="unidirectional-or-one-way-authentication"></a>Ühesuunalised või ühesuunalise autentimine

Klõpsake Ühesuunalised autentimine, sihtkohas autendib algataja. See autentimine nõuab CHAP algataja sätteid konfigureerida StorSimple seade ja iSCSI algataja tarkvara Host. Üksikasjalik kord StorSimple seadme ja Windows host on kirjeldatud edasi.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Seadme ühesuunalise autentimise konfigureerimine

1. Azure'i klassikaline portaalis lehel **seadmed** vahekaarti **konfigureerimine** .

    ![Algataja Peatükk](./media/storsimple-configure-chap/IC740943.png)

2. Liikuge kerides allapoole ja selle lehe jaotise **CHAP algataja** :
                                                    
    1. Sisestage kasutaja nimi oma CHAP algataja.

    2. Lisage oma CHAP algataja parooli.

         > [AZURE.IMPORTANT] CHAP kasutajanimi peab sisaldama vähem kui 233 märki. CHAP parooli peab olema 12 – 16 märki. Hosti Windowsi autentimist tõrke tulemuseks enam kasutajanime ja parooli.
    
    3. Kinnitage parool.

4. Klõpsake nuppu **Salvesta**. Kuvatakse kinnitusteade. Klõpsake muudatuste salvestamiseks nuppu **OK** .

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Windows Server hosti ühesuunalise autentimise konfigureerimine

1. Käivitage Windows Server hosti iSCSI algataja.

2. **ISCSI algataja atribuutide** aknas, tehke järgmist.
                                                    
    1. Klõpsake vahekaarti **Discovery** .

        ![iSCSI algataja atribuudid](./media/storsimple-configure-chap/IC740944.png)

    2. Klõpsake **portaali leida**.

3. **Tutvumine Target portaali** dialoogiboksis järgmist.
                                                    
    1. Saate määrata oma seadme IP-aadress.

    3. Klõpsake nuppu **Täpsemalt**.

        ![Sihtrakenduse portaali avastamine](./media/storsimple-configure-chap/IC740945.png)

4. Dialoogiboksis **Täpsemad sätted** järgmist.
                                                    
    1. Märkige ruut **Luba Peatükk sisse logida** .

    2. Lisage väljale **nimi** kasutaja nimi, mida teie määratud CHAP algataja klassikaline portaalis.

    3. Välja **Target salajane** pakkumise CHAP algataja klassikaline portaalis määratud parool.

    4. Klõpsake nuppu **OK**.

        ![Täpsemad sätted üldine](./media/storsimple-configure-chap/IC740946.png)

5. Akna **iSCSI algataja atribuudid** vahekaardil **sihtkohtade** seadme olek peaks kuvatama **ühendatud**. Kui kasutate StorSimple 1200 seadet, siis iga maht olema paigaldatud nimega sihtseadmega nagu allpool näidatud. Seega peate korratakse iga helitugevuse juhiseid 3 ja 4.

    ![Paigaldatud eraldi sihtkohtade mahud](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Kui muudate iSCSI nime, kasutatakse uue iSCSI seansid uus nimi. Uued sätted ei kasutata olemasoleva seansid enne, kui logite välja ja logige uuesti.

Windows Server hosti CHAP konfigureerimise kohta lisateabe saamiseks minge [täiendavate peaksite arvesse võtma](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Kahesuunaline või vastastikune autentimine

Kahesuunaline autentimisel sihtkohas autendib algataja ja seejärel algataja autendib sihtkohas. Selleks, et kasutaja seade ja iSCSI algataja tarkvara hosti CHAP algataja sätted, kui ka vastupidise CHAP sätete konfigureerimiseks. Järgmised toimingud on kirjeldatud juhiseid vastastikune autentimise konfigureerimine seadme ja Windows Server.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Seadme vastastikune autentimise konfigureerimine

1. Azure'i klassikaline portaalis lehel **seadmed** vahekaarti **konfigureerimine** .

    ![CHAP Target (sihtkoht)](./media/storsimple-configure-chap/IC740948.png)

2. Liikuge kerides allapoole sellel lehel, ja klõpsake jaotises **CHAP Target (sihtkoht)** :
                                                    
    1. Sisestage oma seadme **Peatükk tühistada kasutaja nimi** .

    2. Lisage oma seadme **tühistada Peatükk parooli** .

    3. Kinnitage parool.

3. Jaotis **CHAP algataja** .
                                                
    1. Sisestage **kasutajanimi** oma seade.

    1. Sisestage **parool** oma seadme.

    3. Kinnitage parool.

4. Klõpsake nuppu **Salvesta**. Kuvatakse kinnitusteade. Klõpsake muudatuste salvestamiseks nuppu **OK** .

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Windows Server hosti kahesuunaline autentimise konfigureerimine

1. Käivitage Windows Server hosti iSCSI algataja.

2. Klõpsake aknas **iSCSI algataja atribuudid** vahekaarti **konfigureerimine** .

3. Klõpsake **Peatükk**.

4. **ISCSI algataja vastastikune Peatükk salajane** dialoogiboksis järgmist.
                                                    
    1. Tippige **parool tühistada Peatükk** konfigureeritud Azure klassikaline portaalis.

    2. Klõpsake nuppu **OK**.

        ![iSCSI algataja vastastikune CHAP salajane](./media/storsimple-configure-chap/IC740949.png)

5. Klõpsake vahekaarti **sihtkohtade** .

6. Klõpsake nuppu **Loo ühendus** . 

7. Klõpsake dialoogiboksis **Ühenduse Target** **Täpsemalt**.

8. Dialoogiboksis **Täpsemad atribuudid** :
                                                    
    1. Märkige ruut **Luba Peatükk sisse logida** .

    2. Lisage väljale **nimi** kasutaja nimi, mida teie määratud CHAP algataja klassikaline portaalis.

    3. Välja **Target salajane** pakkumise CHAP algataja klassikaline portaalis määratud parool.

    4. Märkige ruut **Rakenda vastastikune autentimine** .

        ![Täpsemad sätted vastastikune autentimine](./media/storsimple-configure-chap/IC740950.png)

    5. Klõpsake nuppu **OK** , et CHAP konfigureerimise lõpuleviimiseks
     
Windows Server hosti CHAP konfigureerimise kohta lisateabe saamiseks minge [täiendavate peaksite arvesse võtma](#additional-considerations).

## <a name="additional-considerations"></a>Täiendavad kaalutlused

Funktsiooni **Kiirühendamine** ei toeta ühendusi, millel on lubatud CHAP. Kui CHAP on lubatud, veenduge, et kasutada **sihtkohtade** vahekaardil ühenduse eesmärgiks on saadaval nuppu **Ühenda** .

![Target (sihtkoht) ühenduse loomine](./media/storsimple-configure-chap/IC740947.png)

Mis on esitatud dialoogiboksis **ühenduse Target** ruut **Lisa selle ühenduse lemmik sihtkohad loendit** . See tagab, et iga kord, kui arvuti taaskäivitamist, on katse iSCSI lemmik eesmärgid ühenduse taastamine.

## <a name="errors-during-configuration"></a>Tõrgete konfigureerimise käigus

Kui teie CHAP konfiguratsiooni on vale, siis tõenäoliselt **autentimise nurjumise** tõrketeate kuvamiseks.

## <a name="verification-of-chap-configuration"></a>CHAP konfiguratsiooni kontrollimine

Saate kontrollida, et kasutataks CHAP, tehes järgmist.

#### <a name="to-verify-your-chap-configuration"></a>Kinnitamaks, et teie CHAP konfigureerimine

1. Klõpsake käsku **lemmik sihtkohad**.

2. Valige siht, mille märkisite autentimist.

3. Klõpsake nuppu **üksikasjad**.

    ![iSCSI algataja atribuudid lemmik sihtkohad](./media/storsimple-configure-chap/IC740951.png)

4. Dialoogiboksis **Lemmik Target üksikasjad** Märkus kirje väljale **autentimine** . Kui konfiguratsiooni õnnestus, peaks olema kirjas **Peatükk**.

    ![Lemmik target üksikasjad](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet [StorSimple turvalisus](storsimple-security.md).

- Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
