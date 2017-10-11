### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>Utwórz punkt końcowy protokołu TCP dla maszyny wirtualnej
Aby uzyskać dostęp do programu SQL Server z Internetu, maszyna wirtualna musi mieć punktu końcowego nasłuchiwania TCP komunikacji przychodzącej. Ten krok konfiguracji platformy Azure, kieruje ruch przychodzący port TCP do portu TCP, który jest dostępny dla maszyny wirtualnej.

> [!NOTE]
> Jeśli łączysz się w tej samej usługi w chmurze lub sieci wirtualnej, nie trzeba tworzyć publicznie dostępnym punkcie końcowym. W takim przypadku można kontynuować do następnego kroku. Aby uzyskać więcej informacji, zobacz [scenariuszach połączeń](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. W portalu Azure wybierz **maszyn wirtualnych (klasyczne)**.
2. Następnie wybierz maszynę wirtualną programu SQL Server.
3. Wybierz **punkty końcowe**, a następnie kliknij przycisk **Dodaj** na górze bloku punktów końcowych.
   
    ![Portal kroki tworzenia punktu końcowego](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. Na **Dodawanie punktu końcowego** bloku, podaj **nazwa** takich jak SQLEndpoint.
5. Wybierz **TCP** dla **protokołu**.
6. Aby uzyskać **port publiczny**, określ numer portu, takich jak **57500**.
7. Aby uzyskać **port prywatny**, określ port nasłuchiwania programu SQL Server, która domyślnie określa **1433**.
8. Kliknij przycisk **Ok** można utworzyć punktu końcowego.

