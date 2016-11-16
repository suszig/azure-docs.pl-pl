> [!IMPORTANT]
> Aby odbierać powiadomienia wypychane z usługi Mobile Engagement, należy włączyć w aplikacji funkcję `Silent Remote Notifications`. Trzeba dodać wartość remote-notification do tablicy UIBackgroundModes w pliku Info.plist.
> 
> 

1. Otwórz plik `info.plist` w projekcie.
2. Kliknij prawym przyciskiem myszy pierwszy element na liście (`Information Property List`) i dodaj nowy wiersz.
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. W nowym wierszu wprowadź wartość `Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Kliknij strzałkę w lewo, aby rozwinąć wiersz.
5. Dodaj następującą wartość do elementu 0: `App downloads content in response to push notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. Po wprowadzeniu zmiany plik XML info.plist powinien zawierać następujący klucz i wartość:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. Jeśli używasz programu **Xcode 7 +** i systemu **iOS 9 +**:
   
   * Włącz funkcję **Push Notifications** (Powiadomienia wypychane), wybierając kolejno opcje Targets (Obiekty docelowe ) > Your Target Name (Nazwa obiektu docelowego) > Capabilities (Funkcje).



<!--HONumber=Nov16_HO2-->


