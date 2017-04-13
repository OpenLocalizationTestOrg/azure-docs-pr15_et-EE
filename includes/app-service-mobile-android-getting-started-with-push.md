1. **Rakenduse** projektis avage fail `AndroidManifest.xml`. Asendage kood järgmised kaks toimingut, _`**my_app_package**`_ projekti rakendusepaketi nimi, mis on väärtus on `package` atribuut on `manifest` silt.

2. Järgmised uued õigused lisamine pärast olemasoleva `uses-permission` elementi:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Lisage järgmine kood pärast selle `application` avamine silt:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Avage fail *ToDoActivity.java*ja lisage järgmine impordi-lause:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Lisada järgmised privaatne muutuja klassi: Asendage _`<PROJECT_NUMBER>`_ määratud Google'i ülalkirjeldatud toimingu rakenduse Project number:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Muuda *MobileServiceClient* määratluse **era** **avalikud staatilised**, nii, et see nüüd näeb välja umbes järgmine:

        public static MobileServiceClient mClient;

7. Edasi tuleb lisada uutel käsitlema teatised. Avage rakenduses Project Explorer **src** => **põhi** => **java** sõlmed ja Paremklõpsake sõlme paketi nimi: klõpsake nuppu **Uus**ja seejärel klõpsake nuppu **Java klassi**.

8. Tippige **nimi** `MyHandler`, seejärel klõpsake nuppu **OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. Failis MyHandler asendada klassi deklaratsiooni abil

        public class MyHandler extends NotificationsHandler {


10. Lisada impordi järgmistest jaoks soovitud `MyHandler` klassi:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Järgmisena lisage selle liikme funktsiooni `MyHandler` klassi:

        public static final int NOTIFICATION_ID = 1;


12. Klõpsake soovitud `MyHandler` klassi, lisage järgmine kood **onRegistered** meetod, mis registreerib seadme mobiilsideteenuse teate jaoturi alistada.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. Klõpsake soovitud `MyHandler` klassi, lisage järgmine kood alistada **onReceive** meetod, mis põhjustab teate kuvamiseks, kui see on saanud.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Tagasi TodoActivity.java failis, värskendage *ToDoActivity* ainekursuse registreerida teatis sündmuseohjuri ainekursuse **onCreate** meetod. Veenduge, et lisada see koodi pärast *MobileServiceClient* on käivitatud.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Rakenduse on värskendatud Tõuketeatiste toetavad.
