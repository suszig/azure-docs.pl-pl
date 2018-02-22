

1. Zaloguj się do [konsoli Firebase](https://firebase.google.com/console/). Utwórz nowy projekt Firebase, jeśli jeszcze go nie masz.
2. Po utworzeniu projektu, zaznacz **Firebase dodać do aplikacji systemu Android**. Następnie postępuj zgodnie z instrukcjami, które zostały opublikowane.

    ![Dodaj Firebase do aplikacji systemu Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. W konsoli Firebase wybierz koło zębate dla projektu. Następnie wybierz **ustawienia projektu**.

    ![Wybierz ustawienia projektu](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Wybierz **ogólne** kartę w ustawieniach projektu. Następnie pobierz **google services.json** pliku, który zawiera klucz interfejsu API serwera i identyfikator klienta.
5. Wybierz **Cloud Messaging** karcie w ustawieniach projektu, a następnie skopiuj wartość **klucz serwera starszych**. Ta wartość służy do konfigurowania zasad dostępu Centrum powiadomień.
