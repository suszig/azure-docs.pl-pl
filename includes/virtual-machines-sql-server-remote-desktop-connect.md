Wykonaj następujące kroki, aby ustanowić połączenie z maszyną wirtualną programu SQL Server przy użyciu pulpitu zdalnego:

1. Po utworzeniu i uruchomieniu maszyny wirtualnej platformy Azure kliknij ikonę Maszyny wirtualne w witrynie Azure Portal, aby wyświetlić swoje maszyny wirtualne.

1. Kliknij przycisk wielokropka (**...** ) dla nowej maszyny wirtualnej.

1. Kliknij przycisk **Połącz**.

   ![Nawiązywanie połączenia z maszyną wirtualną w portalu](./media/virtual-machines-sql-server-remote-desktop-connect/azure-virtual-machine-connect.png)

1. Otwórz plik **RDP**, który przeglądarka pobiera dla maszyny wirtualnej.

1. Funkcja Podłączanie pulpitu zdalnego powiadomi, że nie można zidentyfikować wydawcy tego połączenia zdalnego. Kliknij pozycję **Połącz**, aby kontynuować.

1. W oknie dialogowym **Zabezpieczenia systemu Windows** kliknij pozycję **Użyj innego konta**. Może być konieczne kliknięcie pozycji **Więcej opcji**, aby ją wyświetlić. Podaj nazwę użytkownika i hasło, które zostały skonfigurowane podczas tworzenia maszyny wirtualnej. Musisz dodać ukośnik odwrotny przed nazwą użytkownika.

   ![Uwierzytelnianie pulpitu zdalnego](./media/virtual-machines-sql-server-remote-desktop-connect/remote-desktop-connect.png)

Po ustanowieniu połączenia z maszyną wirtualną programu SQL Server możesz uruchomić narzędzie SQL Server Management Studio i ustanowić połączenie przy użyciu uwierzytelniania systemu Windows i poświadczeń administratora lokalnego. Jeśli opcja Uwierzytelnianie programu SQL Server została włączona, możesz też ustanowić połączenie przy użyciu uwierzytelniania SQL oraz identyfikatora logowania i hasła skonfigurowanego podczas aprowizowania.

Korzystając z dostępu do maszyny, możesz bezpośrednio zmienić ustawienia maszyny i programu SQL Server, w zależności od wymagań. Na przykład możesz skonfigurować ustawienia zapory lub zmienić ustawienia konfiguracji programu SQL Server.