

1. Zaloguj się do [konsoli Firebase](https://firebase.google.com/console/). Utwórz nowy projekt Firebase, jeśli jeszcze go nie masz.
2. Po utworzeniu projektu kliknij pozycję **Add Firebase to your Android app** (Dodaj usługę Firebase do aplikacji systemu Android) i postępuj zgodnie z instrukcjami.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. W konsoli Firebase kliknij koło zębate dla projektu, a następnie kliknij pozycję **Project Settings** (Ustawienia projektu).

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Kliknij przycisk **ogólne** karcie w ustawieniach projektu, a następnie pobrać **google services.json** pliku, który zawiera klucz interfejsu API serwera i identyfikator klienta.
5. Kliknij przycisk **Cloud Messaging** w ustawieniach projektu, a następnie skopiować wartość **klucz serwera starszych**. Ta wartość będzie służyć do konfigurowania zasad dostępu do Centrum powiadomień.
