####<a name="configuring-the-ios-project-in-xamarin-studio"></a>IOS-i projekti Xamarin Studios seadistamine

1. Xamarin.Studio, avage **Info.plist**ja värskendada **Komplekt identifikaator** kogumi ID varem loodud oma uue rakenduse ID-ga.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Kerige allapoole suvandini **Taustal režiimi** ja märkige ruut **Luba taustal režiimi** ja väljal **Remote teatised** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Projekti lahenduse paneeli **Projekti suvandid**avamiseks topeltklõpsake nuppu.

4.  Valige **iOS-i komplekt sisselogimise** jaotises **koostamine**ja valige vastav **identiteedi** ja **Provisioning profiili** oli äsja seadistatud selle projekti. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    See tagab, et projekt kasutab uue profiili koodi allkirjastamiseks. Ametlik Xamarin seadme dokumentatsiooni ettevalmistamise, leiate teemast [Xamarin seadme ettevalmistamise].

####<a name="configuring-the-ios-project-in-visual-studio"></a>Visual Studio projekti iOS-i seadistamine

1. Paremklõpsake projekti Visual Studios, ja seejärel klõpsake käsku **Atribuudid**.

2. Atribuutide lehtede, klõpsake vahekaarti **iOS-i rakendus** ja värskendada **identifikaator** varem loodud ID-ga.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. **IOS-i komplekt sisselogimise** menüüs valige vastav **identiteedi** ja **Provisioning profiili** oli äsja seadistatud selle projekti jaoks. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    See tagab, et projekt kasutab uue profiili koodi allkirjastamiseks. Ametlik Xamarin seadme dokumentatsiooni ettevalmistamise, leiate teemast [Xamarin seadme ettevalmistamise].

4. Topeltklõpsake Info.plist, avage see ja seejärel lubada **RemoteNotifications** jaotises Taust režiimi. 



[Xamarin seadme ettevalmistamine]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/