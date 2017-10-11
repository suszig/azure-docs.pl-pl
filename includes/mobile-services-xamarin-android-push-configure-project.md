
1. W widoku Solution (lub **Eksploratora rozwiązań** w programie Visual Studio), kliknij prawym przyciskiem myszy **składniki** folderu, kliknij przycisk **Uzyskaj więcej składników...** , wyszukaj **Google Cloud Messaging Client** składników i dodaj go do projektu.
2. Otwórz plik projektu o nazwie ToDoActivity.cs i dodaj następującą instrukcję do klasy using:
   
        using Gcm.Client;
3. W **ToDoActivity** klasy, Dodaj następujący kod: 
   
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
   
    Dzięki temu można uzyskać dostępu do wystąpienia klientów urządzeń przenośnych z procesu wypychanej obsługi usługi.
4. Dodaj następujący kod do **OnCreate** metody, po **MobileServiceClient** utworzeniu:
   
       // Set the current instance of TodoActivity.
       instance = this;
   
       // Make sure the GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register the app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Twoje **ToDoActivity** jest teraz gotowy do Dodawanie powiadomień wypychanych.

