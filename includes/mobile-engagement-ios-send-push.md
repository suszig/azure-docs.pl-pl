### <a name="grant-access-to-your-push-certificate-to-mobile-engagement"></a>Przyznawanie dostępu do certyfikatu wypychania do usługi Mobile Engagement
Aby zezwolić usłudze Mobile Engagement na wysyłanie powiadomień wypychanych w swoim imieniu, musisz przyznać jej dostęp do swojego certyfikatu. W tym celu należy skonfigurować i wprowadzić certyfikat w portalu usługi Mobile Engagement. Upewnij się, że dysponujesz certyfikatem p12, zgodnie z objaśnieniem w [dokumentacji firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).

1. Przejdź do portalu Mobile Engagement. Upewnij się, że znajdujesz się na poziomie właściwej aplikacji, a następnie kliknij przycisk **Engage** (Włącz) widoczny u dołu:
   
    ![](./media/mobile-engagement-create-app-in-portal-new/engage-button.png)
2. Kliknij stronę **Settings** (Ustawienia) w portalu usługi Engagement. Następnie kliknij pozycję **Native Push** (Natywne powiadomienia wypychane), aby przesłać certyfikat p12:
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. Wybierz certyfikat p12, prześlij go i wpisz hasło:
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <a id="send"></a>Wysyłanie powiadomienia do aplikacji
Teraz utworzymy prostą kampanię z użyciem powiadomień wypychanych, która wyśle powiadomienie wypychane do aplikacji:

1. Przejdź do karty **Reach** (Zasięg) w portalu usługi Mobile Engagement.
2. Kliknij przycisk **New Announcement** (Nowy anons), aby utworzyć kampanię powiadomień wypychanych.
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. Skonfiguruj pierwsze pola kampanii:
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * Podaj nazwę kampanii w polu **Name** (Nazwa). 
   * W obszarze **Delivery time** (Godzina dostarczenia) wybierz opcję **Out of app only** (Tylko poza aplikacją) — jest to prosty typ powiadomienia wypychanego firmy Apple, które zawiera krótki tekst.
   * W tekście powiadomienia w polu **Title** (Tytuł) wpisz tytuł, który będzie stanowić pierwszy wiersz powiadomienia wypychanego.
   * Następnie w polu **Message** (Komunikat) wpisz komunikat, który będzie stanowić drugi wiersz.
4. Przewiń w dół i w sekcji zawartości wybierz opcję **Notification only** (Tylko powiadomienie).
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. Najbardziej podstawowa kompania została skonfigurowana. Teraz przewiń w dół i kliknij przycisk **Create** (Utwórz), aby zapisać kampanię z użyciem powiadomień wypychanych. 
6. Na koniec kliknij opcję **Activate** (Aktywuj), aby wysłać powiadomienie wypychane. 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. Powiadomienie będzie można odebrać na urządzeniu z systemem iOS w centrum powiadomień w następujący sposób:
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. Jeśli masz urządzenie Apple Watch sparowane z danym urządzeniem z systemem iOS, powiadomienie zostanie wyświetlone na urządzeniu Apple Watch:
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

