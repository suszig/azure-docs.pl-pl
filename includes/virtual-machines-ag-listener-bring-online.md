1. W Menedżerze klastra trybu Failover rozwiń **ról**, a następnie zaznacz tej grupy dostępności.  

2. Na **zasobów** , kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij **właściwości**.

3. Kliknij przycisk **zależności** kartę. Jeśli wiele zasobów nie są wyświetlane, sprawdź, czy adresy IP są lub nie oraz zależności.  

4. Kliknij przycisk **OK**.

5. Kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij przycisk **przejdź do trybu Online**.

6. Po odbiornika jest w trybie online na **zasobów** , kliknij prawym przyciskiem myszy grupę dostępności, a następnie kliknij **właściwości**.
   
    ![Skonfiguruj zasób grupy dostępności](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Zależność od zasobu Nazwa odbiornika (nie IP address zasobów nazwę), a następnie kliknij polecenie **OK**.
   
    ![Dodaj zależności na nazwę odbiornika](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Uruchom program SQL Server Management Studio i Połącz z repliką podstawową.

9. Przejdź do **AlwaysOn wysokiej dostępności** > **grup dostępności** > **\<AvailabilityGroupName\>**   >  **Odbiorniki grupy dostępności**.  
    Powinna być wyświetlana nazwa odbiornika utworzonego w Menedżerze klastra trybu Failover.

10. Kliknij prawym przyciskiem myszy nazwę odbiornika, a następnie kliknij przycisk **właściwości**.

11. W **portu** polu Określ numer portu dla odbiornika grupy dostępności, używając $EndpointPort używanego wcześniej (w tym samouczku 1433 jest wartość domyślna), a następnie kliknij przycisk **OK**.

