
1. Lahenduse vaade (või **Solution Exploreris** Visual Studios), paremklõpsake kausta **komponendid** nuppu **Saada lisateavet komponendid...**, **Google Cloud sõnumside kliendi** komponent otsimine ja lisamine projekti.

2. Avage fail ToDoActivity.cs projekti ja lisage järgmine lause klassi abil:

        using Gcm.Client;

3. **ToDoActivity** ainekursust, lisage uus järgmine kood: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    See lubab teil juurde pääseda tõuketeatised sündmuseohjuri teenuse protsessi mobiilikliendi eksemplari.

4.  Lisage järgmine kood **OnCreate** meetod **MobileServiceClient** loomise järel.

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Teie **ToDoActivity** on nüüd valmis Tõuketeatiste lisamine.