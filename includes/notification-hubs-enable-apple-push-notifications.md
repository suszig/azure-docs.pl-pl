

## Generowanie pliku żądania podpisania certyfikatu
Usługa Apple Push Notification Service (APNS) używa certyfikatów do uwierzytelniania powiadomień wypychanych. Wykonaj instrukcje, aby utworzyć niezbędny certyfikat powiadomień wypychanych umożliwiający ich wysyłanie i odbieranie. Więcej informacji dotyczących tych pojęć można znaleźć w oficjalnej dokumentacji usługi [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584).

Wygeneruj plik żądania podpisania certyfikatu (CSR), który jest używany przez firmę Apple do wygenerowania podpisanego certyfikatu powiadomień wypychanych.

1. Na komputerze Mac uruchom narzędzie Keychain Access. Można je otworzyć z folderu **Narzędzia** lub folderu **Inne** na konsoli uruchamiania.
2. Kliknij opcję **Keychain Access**, rozwiń węzeł **Asystent certyfikatu**, a następnie kliknij opcję **Żądaj certyfikatu od urzędu certyfikacji...**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Wybierz opcje **Adres e-mail użytkownika** i **Nazwa pospolita**, upewnij się, że została zaznaczona opcja **Zapisano na dysku**, a następnie kliknij przycisk **Kontynuuj**. Pozostaw pole **Adres e-mail urzędu certyfikacji** puste, ponieważ nie jest wymagane.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Wpisz nazwę pliku żądania podpisania certyfikatu (CSR) w polu **Zapisz jako**, wybierz lokalizację na liście **Gdzie**, następnie kliknij przycisk **Zapisz**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
    Powoduje to zapisanie pliku CSR w wybranej lokalizacji; lokalizacja domyślna znajduje się na pulpicie. Zapamiętaj lokalizację wybraną dla tego pliku.

Następnie zarejestrujesz aplikację w firmie Apple, włączysz powiadomienia wypychane i przekażesz ten wyeksportowany plik CSR, aby utworzyć certyfikat powiadomień wypychanych.

## Rejestrowanie aplikacji dla usługi powiadomień wypychanych
Aby móc wysyłać powiadomienia wypychane do aplikacji dla systemu iOS, należy zarejestrować aplikację w firmie Apple oraz zarejestrować się w usłudze powiadomień wypychanych.  

1. Jeśli aplikacja nie została jeszcze zarejestrowana, przejdź do <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">portalu inicjowania obsługi systemu iOS</a> w Centrum deweloperów firmy Apple, zaloguj się za pomocą identyfikatora firmy Apple, kliknij opcję **Identyfikatory**, następnie kliknij opcję **Identyfikatory aplikacji**, a na koniec kliknij znak **+**, aby zarejestrować nową aplikację.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
2. Zaktualizuj następujące trzy pola dla nowej aplikacji, a następnie kliknij przycisk **Kontynuuj**:
   
   * **Nazwa**: wpisz nazwę opisową aplikacji w polu **Nazwa** w sekcji **Opis identyfikatora aplikacji**.
   * **Identyfikator pakietu**: w sekcji **Jawny identyfikator aplikacji** wprowadź **Identyfikator pakietu** w formie `<Organization Identifier>.<Product Name>` zgodnie z opisem w [Podręczniku dystrybucji aplikacji](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). *Identyfikator organizacji* i *Nazwa produktu*, których używasz, muszą być zgodne z identyfikatorem organizacji i nazwą produktu, których będziesz używać podczas tworzenia projektu w programie XCode. Na zrzucie ekranu poniżej pozycja *NotificationHubs* stanowi identyfikator organizacji, a *GetStarted* nazwę produktu. Zagwarantowanie, że pozycje te są zgodne z wartościami, które zostaną użyte w projekcie w programie XCode, pozwoli na użycie poprawnego profilu publikowania w programie XCode. 
   * **Powiadomienia wypychane**: zaznacz opcję **Powiadomienia wypychane** w sekcji **App Services**.
     
     ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
     Spowoduje to wygenerowanie identyfikatora aplikacji, a użytkownik zostanie poproszony o potwierdzenie podanych informacji. Kliknij przycisk **Zarejestruj**, aby potwierdzić nowy identyfikator aplikacji.
     
     Po kliknięciu przycisku **Zarejestruj** zostanie wyświetlony ekran **Rejestracja ukończona**, jak przedstawiono poniżej. Kliknij przycisk **Gotowe**.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. W Centrum deweloperów w obszarze Identyfikatory aplikacji zlokalizuj nowo utworzony identyfikator aplikacji i kliknij jego wiersz.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
    Kliknięcie identyfikatora aplikacji spowoduje wyświetlenie jej szczegółów. Kliknij przycisk **Edytuj** widoczny u dołu.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
2. Przewiń w dół ekranu, a następnie kliknij przycisk **Utwórz certyfikat...** w sekcji **Certyfikat SSL deweloperskich powiadomień wypychanych**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
    Spowoduje to wyświetlenie Asystenta usługi „Dodaj certyfikat iOS”.
   
   > [!NOTE]
   > Instrukcje w tym samouczku obejmują użycie certyfikatu deweloperskiego. Ten sam proces jest używany podczas rejestrowania certyfikatu produkcyjnego. Należy po prostu pamiętać, aby używać tego samego typu certyfikatu podczas wysyłania powiadomień.
   > 
   > 
3. Kliknij opcję **Wybierz plik**, przejdź do lokalizacji, w której został zapisany plik CSR utworzony w pierwszym zadaniu, a następnie kliknij przycisk **Generuj**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. Po utworzeniu certyfikatu przez portal kliknij przycisk **Pobierz** a następnie kliknij przycisk **Gotowe**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
    Spowoduje to pobranie certyfikatu i zapisanie go na komputerze w folderze Pobrane.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Domyślnie pobrany plik certyfikatu deweloperskiego nosi nazwę **aps_development.cer**.
   > 
   > 
5. Kliknij dwukrotnie pobrany certyfikat powiadomień wypychanych **aps_development.cer**.
   
    Spowoduje to zainstalowanie nowego certyfikatu w narzędziu Keychain, jak przedstawiono poniżej:
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > Nazwa w certyfikacie może być inna, ale będzie ona poprzedzona ciągiem **Apple Development iOS Push Services:**.
   > 
   > 
6. W narzędziu Keychain Access kliknij prawym przyciskiem myszy nowy certyfikat powiadomień wypychanych utworzony w kategorii **Certyfikaty**. Kliknij opcję **Eksportuj**, nazwij plik, wybierz format **.p12**, a następnie kliknij przycisk **Zapisz**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    Zanotuj nazwę pliku i lokalizację wyeksportowanego certyfikatu .p12. Zostanie on użyty do włączenia uwierzytelniania za pomocą usługi APNS.
   
   > [!NOTE]
   > W tym samouczku opisano proces tworzenia pliku QuickStart.p12. Nazwa i lokalizacja pliku mogą się różnić.
   > 
   > 

## Tworzenie profilu inicjowania obsługi dla aplikacji
1. Po powrocie do <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">portalu inicjowania obsługi w systemie iOS</a> wybierz opcję **Profile inicjowania obsługi**, wybierz **Wszystkie**, a następnie kliknij przycisk **+**, aby utworzyć nowy profil. Spowoduje to uruchomienie Kreatora **dodawania profilu inicjowania obsługi w systemie iOS**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Wybierz opcję **Tworzenie aplikacji dla systemu iOS** w obszarze **Programowanie** jako typ profilu inicjowania obsługi i kliknij przycisk **Kontynuuj**. 
3. Następnie wybierz nowo utworzony identyfikator aplikacji z listy rozwijanej **Identyfikator aplikacji**, po czym kliknij przycisk **Kontynuuj**
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. Na ekranie **Wybierz certyfikaty** wybierz certyfikat programowania zwykle używany do podpisywania kodu, a następnie kliknij przycisk **Kontynuuj**. To nie jest nowo utworzony certyfikat powiadomień wypychanych.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Następnie wybierz pozycję **Urządzenia**, aby użyć jej do testowania, po czym kliknij przycisk **Kontynuuj**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Na koniec wybierz nazwę profilu na karcie **Nazwa profilu** i kliknij przycisk **Generuj**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. Podczas tworzenia nowego profilu inicjowania obsługi kliknij go, aby go pobrać i zainstalować na komputerze deweloperskim z zainstalowanym programem Xcode. Następnie kliknij przycisk **Gotowe**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

<!--HONumber=sep16_HO1-->


