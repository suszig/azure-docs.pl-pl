W tym kroku ręcznie utworzyć odbiornika grupy dostępności w Menedżerze klastra trybu Failover i SQL Server Management Studio.

1. Otwórz Menedżera klastra pracy awaryjnej z węzła, który obsługuje replikę podstawową.

2. Wybierz **sieci** węzeł, a następnie Uwaga Nazwa sieciowa klastra. Ta nazwa jest używana w zmiennej $ClusterNetworkName w skrypcie programu PowerShell.

3. Rozwiń nazwę klastra, a następnie kliknij przycisk **ról**.

4. W **ról** okienku kliknij prawym przyciskiem myszy nazwę grupy dostępności, a następnie wybierz **dodawania zasobów** > **punktu dostępu klienta**.
   
    ![Dodaj punkt dostępu klienta dla grupy dostępności](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. W **nazwa** , Utwórz nazwę dla tego nowego odbiornika kliknij pozycję **dalej** dwa razy, a następnie kliknij przycisk **Zakończ**.  
    Nie powodują odbiornika lub zasób w tryb online w tym momencie.

6. Kliknij przycisk **zasobów** karcie, a następnie rozwiń utworzony punkt dostępu klienta. 
    Zasób adresu IP dla każdej sieci klastra w klastrze jest wyświetlany. Jeśli jest to rozwiązanie tylko do platformy Azure, zostanie wyświetlony tylko jeden zasób adres IP.

7. Wykonaj jedną z następujących czynności:
   
   * Aby skonfigurować hybrydowego rozwiązania:
     
        a. Kliknij prawym przyciskiem myszy zasób adresu IP, który odpowiada podsieci lokalnej, a następnie wybierz **właściwości**. Należy pamiętać, nazwę adresów IP i nazwy sieciowej.
   
        b. Wybierz **statyczny adres IP**Przypisz nieużywany adres IP, a następnie kliknij przycisk **OK**.
 
   * Aby skonfigurować rozwiązanie tylko do platformy Azure:

        a. Kliknij prawym przyciskiem myszy zasób adresu IP, umożliwiająca Azure podsieć, a następnie wybierz **właściwości**.
       
       > [!NOTE]
       > Jeśli odbiornik awarii do trybu online, ze względu na konflikt adresu IP wybrana przez serwer DHCP, w tym oknie właściwości można skonfigurować statyczny adres IP.
       > 
       > 

       b. W tym samym **adres IP** oknie Właściwości zmień **nazwa adresu IP**.  
        Ta nazwa jest używana w zmiennej $IPResourceName skryptu programu PowerShell. Jeśli rozwiązanie obejmuje wiele sieci wirtualnych platformy Azure, powtórz ten krok dla każdego zasobu adresu IP.

