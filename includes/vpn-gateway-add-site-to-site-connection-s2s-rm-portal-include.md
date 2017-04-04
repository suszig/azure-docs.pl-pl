1. Zlokalizuj swoją bramę sieci wirtualnej.
2. Kliknij pozycję **Połączenia**. U góry bloku Połączenia kliknij pozycję **+Dodaj**, aby otworzyć blok **Dodawanie połączenia**.
   
    ![Utwórz połączenie lokacja-lokacja](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection.png)
3. W bloku **Dodaj połączenie** określ **nazwę** połączenia. 
4. W polu **Typ połączenia** wybierz opcję **Lokacja-lokacja (IPsec)**.
5. W polu **Brama sieci wirtualnej** wartość jest ustalona, ponieważ połączenie jest nawiązywane z tej bramy.
6. W polu **Brama sieci lokalnej** kliknij przycisk **Wybierz bramę sieci lokalnej** i wybierz bramę sieci lokalnej, która ma być używana. 
7. W polu **Klucz wspólny** wartość musi być zgodna z wartością używaną dla lokalnego urządzenia sieci VPN. W tym przykładzie użyliśmy wartości „abc123”, ale można (i należy) użyć bardziej złożonej wartości. Pamiętaj o tym, że wartość podana w tym miejscu musi być taka sama, jak wartość podana podczas konfigurowania urządzenia sieci VPN.
8. Pozostałe wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja** są ustalone.
9. Kliknij przycisk **OK**, aby utworzyć połączenie. Na ekranie zostanie wyświetlony napis *Tworzenie połączenia*.
10. Po nawiązaniu połączenia będzie ono widoczne w bloku **Połączenia** bramy sieci wirtualnej.
    
    ![Utwórz połączenie lokacja-lokacja](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connectionstatus450.png)

