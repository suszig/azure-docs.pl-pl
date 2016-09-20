
1. Przejdź do konsoli [Google Cloud Console](https://console.developers.google.com/project) i zaloguj się przy użyciu poświadczeń konta Google. 
 
2. Kliknij opcję **Create Project** (Utwórz projekt), wpisz nazwę projektu, a następnie kliknij przycisk **Create** (Utwórz). Jeśli jest to wymagane, przeprowadź weryfikację SMS i ponownie kliknij przycisk **Create** (Utwórz).

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   

     Wpisz nową nazwę w polu **Project name** (Nazwa projektu) i kliknij pozycję **Create project** (Utwórz projekt).

3. Kliknij pozycję **Utilities and More** (Narzędzia i inne), a następnie kliknij opcję **Project Information** (Informacje o projekcie). Zanotuj numer widoczny w polu **Project Number** (Numer projektu). Tę wartość należy skonfigurować jako zmienną `SenderId` w aplikacji klienta.

    ![](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)


4. Na pulpicie nawigacyjnym projektu w obszarze **Mobile APIs** (Mobilne interfejsy API) kliknij pozycję **Google Cloud Messaging**, po czym na następnej stronie kliknij pozycję **Enable API** (Włącz interfejs API) i zaakceptuj warunki świadczenia usługi. 

    ![Włączanie usługi GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)

    ![Włączanie usługi GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 

5. Na pulpicie nawigacyjnym projektu kliknij kolejno opcje **Credentials** (Poświadczenia) > **Create Credential** (Utwórz poświadczenia) > **API Key** (Klucz interfejsu API). 

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)

6. W obszarze **Create a new key** (Utwórz nowy klucz) kliknij opcję **Server key** (Klucz serwera), wpisz nazwę klucza, a następnie kliknij przycisk **Create** (Utwórz).

7. Zanotuj wartość w polu **API KEY** (KLUCZ INTERFEJSU API).

    Ta wartość klucza interfejsu API zostanie użyta do włączenia na platformie Azure funkcji uwierzytelniania za pomocą usługi GCM i wysyłania powiadomień wypychanych w imieniu aplikacji.




<!--HONumber=sep16_HO1-->


