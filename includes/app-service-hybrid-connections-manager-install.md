
1. **Hübriidjuurutuse ühendused** labale äsja loodud ühenduse hübriid, seejärel käsku **Kuulajale häälestus**.
    
    ![Klõpsake nuppu kuulajale häälestamine](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. Avaneb tera **hübriid ühenduse atribuudid** . Jaotises **Kohapealse hübriid haldur**valige **alla laadida ja käsitsi konfigureerida**, allalaaditud HybridConnectionManager.msi paketi salvestamine ja kopeerige ühendusstring lüüsi.
    
    ![Klõpsake siin, et installida](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Administraatori Käsuviip, tippige järgmine käsk installiprogrammi käivitamiseks:

        start HybridConnectionManager.msi
 
7. Pärast Installiprogramm käivitub, **nüüd pole**, klõpsake nuppu ja seejärel otsige sirvides üles kaust %ProgramFiles%\Microsoft\HybridConnectionManager, käivitage HCMConfigWizard.exe ja dialoogiboksis **Kasutajakonto kontroll** , klõpsake nuppu **Jah** .
        
7. Kleepige varem kopeeritud hübriid ühendusstring ja klõpsake nuppu **OK**. 
    
    ![Installimine](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Kui installimine on lõpule jõudnud, klõpsake nuppu **Sule**.
    
    ![Klõpsake nuppu Sule](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    Enne **hübriid ühendusi** , kuvatakse veerus **olek** nüüd **ühendatud**. 
    
    ![Ühendatud olek](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)