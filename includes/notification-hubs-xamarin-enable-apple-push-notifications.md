

Aby zarejestrować aplikację na potrzeby powiadomień wypychanych za pośrednictwem usługi Apple Push Notification Service (APNS), musisz utworzyć nowy certyfikat wypychania, identyfikator aplikacji oraz profil aprowizowania dla projektu w portalu dla deweloperów firmy Apple. Identyfikator aplikacji będzie zawierać ustawienia konfiguracyjne, które umożliwią aplikacji wysyłanie i odbieranie powiadomień wypychanych. Te ustawienia będą obejmować certyfikat powiadomień wypychanych potrzebny do uwierzytelniania przy użyciu usługi Apple Push Notification Service (APNS) podczas wysyłania i odbierania powiadomień wypychanych. Więcej informacji dotyczących tych pojęć można znaleźć w oficjalnej dokumentacji usługi [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584).

#### <a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Generowanie pliku żądania podpisania certyfikatu na potrzeby certyfikatu wypychania
W tych krokach omówiono tworzenie żądania podpisania certyfikatu. Za jego pomocą zostanie wygenerowany certyfikat wypychania do użycia z usługą APNS.

1. Na komputerze Mac uruchom narzędzie Keychain Access. Można je otworzyć z folderu **Narzędzia** lub folderu **Inne** na konsoli uruchamiania.
2. Kliknij opcję **Keychain Access**, rozwiń węzeł **Asystent certyfikatu**, a następnie kliknij opcję **Żądaj certyfikatu od urzędu certyfikacji...**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Wybierz opcje **Adres e-mail użytkownika** i **Nazwa pospolita**, upewnij się, że została zaznaczona opcja **Zapisano na dysku**, a następnie kliknij przycisk **Kontynuuj**. Pozostaw pole **Adres e-mail urzędu certyfikacji** puste, ponieważ nie jest wymagane.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Wpisz nazwę pliku żądania podpisania certyfikatu (CSR) w polu **Zapisz jako**, wybierz lokalizację na liście **Gdzie**, następnie kliknij przycisk **Zapisz**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Powoduje to zapisanie pliku CSR w wybranej lokalizacji; lokalizacja domyślna znajduje się na pulpicie. Zapamiętaj lokalizację wybraną dla tego pliku.

#### <a name="register-your-app-for-push-notifications"></a>Rejestrowanie aplikacji dla usługi powiadomień wypychanych
Utwórz nowy jawny identyfikator aplikacji dla swojej aplikacji za pośrednictwem firmy Apple i skonfiguruj go na potrzeby powiadomień wypychanych.  

1. Przejdź do [portalu aprowizacji systemu iOS](http://go.microsoft.com/fwlink/p/?LinkId=272456) w Centrum deweloperów firmy Apple, zaloguj się za pomocą identyfikatora firmy Apple, kliknij opcję **Identyfikatory**, następnie opcję **Identyfikatory aplikacji**, a na koniec kliknij znak **+**, aby zarejestrować nową aplikację.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)
2. Zaktualizuj następujące trzy pola dla nowej aplikacji, a następnie kliknij przycisk **Kontynuuj**:
   
   * **Nazwa**: wpisz nazwę opisową aplikacji w polu **Nazwa** w sekcji **Opis identyfikatora aplikacji**.
   * **Identyfikator pakietu**: w sekcji **Jawny identyfikator aplikacji** wprowadź **Identyfikator pakietu** w formie `<Organization Identifier>.<Product Name>` zgodnie z opisem w [Podręczniku dystrybucji aplikacji](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Musi być on taki sam jak w projekcie XCode, Xamarin lub Cordova Twojej aplikacji.
   * **Powiadomienia wypychane**: zaznacz opcję **Powiadomienia wypychane** w sekcji **App Services**.
     
     ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
3. Na ekranie potwierdzania identyfikatora aplikacji sprawdź ustawienie, po czym kliknij przycisk **Prześlij**
4. Po przesłaniu nowego identyfikatora aplikacji zostanie wyświetlony ekran **Rejestracja ukończona**. Kliknij przycisk **Gotowe**.
5. W Centrum deweloperów w obszarze Identyfikatory aplikacji zlokalizuj nowo utworzony identyfikator aplikacji i kliknij jego wiersz. Kliknięcie wiersza z identyfikatorem aplikacji spowoduje wyświetlenie jej szczegółów. Kliknij przycisk **Edytuj** widoczny u dołu.
6. Przewiń w dół ekranu, a następnie kliknij przycisk **Utwórz certyfikat...** w sekcji **Certyfikat SSL deweloperskich powiadomień wypychanych**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
       This will display the "Add iOS Certificate" assistant.
   
   > [!NOTE]
   > Instrukcje w tym samouczku obejmują użycie certyfikatu deweloperskiego. Ten sam proces jest używany podczas rejestrowania certyfikatu produkcyjnego. Należy po prostu pamiętać, aby używać tego samego typu certyfikatu podczas wysyłania powiadomień.
   > 
   > 
7. Kliknij opcję **Wybierz plik** i przejdź do lokalizacji, w której został zapisany plik CSR dla certyfikatu wypychania. Następnie kliknij przycisk **Generuj**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
8. Po utworzeniu certyfikatu przez portal kliknij przycisk **Pobierz**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
       This downloads the signing certificate and saves it to your computer in your Downloads folder.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Domyślnie pobrany plik certyfikatu deweloperskiego nosi nazwę **aps_development.cer**.
   > 
   > 
9. Kliknij dwukrotnie pobrany certyfikat powiadomień wypychanych **aps_development.cer**. Spowoduje to zainstalowanie nowego certyfikatu w narzędziu Keychain, jak przedstawiono poniżej:
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > Nazwa w certyfikacie może być inna, ale będzie ona poprzedzona ciągiem **Apple Development iOS Push Services:**.
   > 
   > 
10. W narzędziu Keychain Access, przytrzymując naciśnięty klawisz Control, kliknij nowy certyfikat wypychania utworzony w kategorii **Certyfikaty**. Kliknij opcję **Eksportuj**, nazwij plik, wybierz format **.p12**, a następnie kliknij przycisk **Zapisz**.
    
    Zapamiętaj nazwę pliku i lokalizację wyeksportowanego certyfikatu wypychania o rozszerzeniu p12. Zostanie on przekazany do klasycznej witryny Azure Portal, co umożliwi włączenie uwierzytelniania za pomocą usługi APNS.

#### <a name="create-a-provisioning-profile-for-the-app"></a>Tworzenie profilu inicjowania obsługi dla aplikacji
1. Po powrocie do <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">portalu inicjowania obsługi w systemie iOS</a> wybierz opcję **Profile inicjowania obsługi**, wybierz **Wszystkie**, a następnie kliknij przycisk **+**, aby utworzyć nowy profil. Spowoduje to uruchomienie Kreatora **dodawania profilu inicjowania obsługi w systemie iOS**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Wybierz opcję **Tworzenie aplikacji dla systemu iOS** w obszarze **Programowanie** jako typ profilu inicjowania obsługi i kliknij przycisk **Kontynuuj**.
3. Następnie wybierz nowo utworzony identyfikator aplikacji z listy rozwijanej **Identyfikator aplikacji**, po czym kliknij przycisk **Kontynuuj**
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. Na ekranie **Wybierz certyfikaty** wybierz certyfikat deweloperski używany do podpisywania kodu, a następnie kliknij przycisk **Kontynuuj**. To jest certyfikat podpisywania, a nie nowo utworzony certyfikat wypychania.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Następnie wybierz pozycję **Urządzenia**, aby użyć jej do testowania, po czym kliknij przycisk **Kontynuuj**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Na koniec wybierz nazwę profilu na karcie **Nazwa profilu** i kliknij przycisk **Generuj**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)



<!--HONumber=Feb17_HO1-->


