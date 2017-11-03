W tym kroku należy przetestować odbiornika grupy dostępności za pomocą aplikacji klienckiej, która działa w tej samej sieci.

Połączenie klienta ma następujące wymagania:

* Połączenia klientów do odbiornika musi pochodzić z maszyny, które znajdują się w innej usługi chmury niż obsługującego replik dostępności AlwaysOn.
* Jeśli replik zawsze na znajdują się w różnych podsieciach, klienci muszą określić *MultisubnetFailover = True* w parametrach połączenia. Ten warunek wynikiem próby połączenia równoległych replik w różnych podsieciach. Taki scenariusz obejmuje między region zawsze na dostępność grupy wdrożenia.

Przykładem jest nawiązać odbiornika z jednej z maszyn wirtualnych w tej samej sieci wirtualnej platformy Azure (ale nie jeden, który jest hostem repliki). Prosty sposób, aby wykonać ten test jest spróbuj nawiązać odbiornika grupy dostępności programu SQL Server Management Studio. Inny prosty metodą jest uruchomienie [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)w następujący sposób:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Jeśli wartość EndpointPort jest *1433*, nie trzeba określić je w wywołaniu. Poprzednie wywołanie przyjęto założenie, że komputer kliencki jest dołączony do tej samej domeny i że wywołującego ma przyznane uprawnienia w bazie danych przy użyciu uwierzytelniania systemu Windows.
> 
> 

Podczas testowania odbiornika należy przełączyć grupy dostępności, aby upewnić się, że klienci mogą łączyć się odbiornika w tryb failover.

