* W oknie **Dodawanie serwera vCenter** podaj przyjazną nazwę dla hosta vSphere lub serwera vCenter, a następnie podaj adres IP lub nazwę FQDN serwera. Pozostaw port ustawiony na 443, chyba że Twoje serwery VMware są skonfigurowane do nasłuchiwania żądań na innym porcie. Wybierz konto, które ma się łączyć z serwerem VMware vCenter lub vSphere ESXi. Kliknij przycisk **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > W przypadku dodawania serwera VMware vCenter lub hosta VMware vSphere za pomocą konta bez uprawnień administracyjnych na serwerze vCenter lub serwerze hosta upewnij się, że konto ma włączone następujące uprawnienia: Centrum danych, Magazyn danych, Folder, Host, Sieć, Zasób, Maszyna wirtualna i Rozproszony przełącznik vSphere. Ponadto serwer VMware vCenter musi mieć włączone uprawnienie Widoki magazynu.
