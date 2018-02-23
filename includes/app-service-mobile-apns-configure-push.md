

1. Na komputerze Mac Uruchom **dostęp łańcucha kluczy**. Na pasku nawigacyjnym po lewej stronie w obszarze **kategorii**, otwórz **moje certyfikaty**. Znajdź certyfikat, który został pobrany w poprzedniej sekcji, a następnie ujawnić jego zawartość. Wybierz tylko certyfikat (nie wybieraj klucz prywatny). Następnie [wyeksportować go](https://support.apple.com/kb/PH20122?locale=en_US).
2. W [portalu Azure](https://portal.azure.com/), wybierz pozycję **Przeglądaj wszystko** > **usługi aplikacji**. Następnie wybierz aplikacji mobilnej zaplecza. 
3. W obszarze **ustawienia**, wybierz pozycję **aplikacji usługi wypychania**. Następnie wybierz nazwę Centrum powiadomień. 
3. Przejdź do **usługi powiadomień wypychanych firmy Apple** > **Przekaż certyfikat**. Przekaż plik .p12, wybierając poprawny **tryb** (w zależności od tego, czy certyfikat SSL klienta od wcześniej to środowisko produkcyjne lub piaskownicy). Zapisz zmiany.

Usługa jest teraz skonfigurowane do pracy z powiadomień wypychanych w systemie iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
