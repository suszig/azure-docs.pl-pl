

1. Na komputerze Mac Uruchom **dostęp łańcucha kluczy**. Na pasku nawigacyjnym po lewej stronie w obszarze **kategorii**, otwórz **moje certyfikaty**. Znajdź certyfikat SSL, który został pobrany w poprzedniej sekcji, a ujawnić jego zawartość. Wybierz tylko certyfikat (nie zaznaczaj klucz prywatny), a [wyeksportować go](https://support.apple.com/kb/PH20122?locale=en_US).
2. W [portalu Azure](https://portal.azure.com/), kliknij przycisk **Przeglądaj wszystko** > **usługi aplikacji**i kliknij przycisk z zaplecza aplikacji mobilnej. W obszarze **ustawienia**, kliknij przycisk **aplikacji usługi wypychania**, a następnie kliknij nazwę Centrum powiadomień. Przejdź do **usługi powiadomień wypychanych firmy Apple** > **Przekaż certyfikat**. Przekaż plik .p12, wybierając poprawny **tryb** (w zależności od tego, czy certyfikat SSL klienta od wcześniej to środowisko produkcyjne lub piaskownicy). Zapisz zmiany.

Usługa jest teraz skonfigurowane do pracy z powiadomień wypychanych w systemie iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
