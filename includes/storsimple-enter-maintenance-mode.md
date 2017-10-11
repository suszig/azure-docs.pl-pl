<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Aby przejść do trybu konserwacji
1. W menu konsoli szeregowej wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.
2. Wpisz hasło. Domyślne hasło jest **Password1**.
3. W wierszu polecenia wpisz
   
     `Enter-HcsMaintenanceMode`
4. Zobaczysz komunikat ostrzegawczy informujący o trybu konserwacji będzie zakłócać wszystkich żądań We/Wy i sever połączenia do klasycznego portalu Azure i zostanie wyświetlony monit o potwierdzenie. Typ **Y** do trybu konserwacji.
   
    Zarówno kontrolery zostanie uruchomiony ponownie. Po zakończeniu ponownego uruchomienia innego pojawi się komunikat wskazujący, że urządzenie jest w trybie konserwacji.

