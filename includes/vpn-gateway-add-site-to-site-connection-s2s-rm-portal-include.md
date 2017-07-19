1. Przejdź do bloku i otwórz go dla bramy sieci wirtualnej. Istnieje wiele metod nawigacji. W tym przykładzie nastąpiło przejście do bramy „VNet1GW” po wybraniu pozycji **TestVNet1 -> Przegląd -> Połączone urządzenia -> VNet1GW**.
2. W bloku bramy VNet1GW kliknij pozycję **Połączenia**. U góry bloku Połączenia kliknij pozycję **+Dodaj**, aby otworzyć blok **Dodawanie połączenia**.

    ![Utwórz połączenie lokacja-lokacja](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. W bloku **Dodaj połączenie** wypełnij wartości, aby utworzyć połączenie.

  - **Nazwa:** nazwij połączenie. W tym przykładzie użyto nazwy **VNet1toSite2**.
  - **Typ połączenia:** wybierz pozycję **lokacja-lokacja(IPSec)**.
  - **Brama sieci wirtualnej:** wartość jest stała, ponieważ połączenie jest nawiązywane z tej bramy.
  - **Brama sieci lokalnej**: kliknij przycisk **Wybierz bramę sieci lokalnej** i wybierz bramę sieci lokalnej, która ma być używana. W tym przykładzie użyto bramy **Site2**.
  - **Klucz wspólny:** wartość musi być zgodna z wartością używaną dla lokalnego urządzenia sieci VPN. W tym przykładzie użyliśmy wartości „abc123”, ale można (i należy) użyć bardziej złożonej wartości. Pamiętaj o tym, że wartość podana w tym miejscu musi być taka sama, jak wartość podana podczas konfigurowania urządzenia sieci VPN.
  - Pozostałe wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja** są ustalone.

4. Kliknij przycisk **OK**, aby utworzyć połączenie. Na ekranie zostanie wyświetlony napis *Tworzenie połączenia*.
5. Połączenie będzie widoczne w bloku **Połączenia** bramy sieci wirtualnej. Stan zmieni się z *Nieznany* na *Łączenie*, a następnie na *Powiodło się*.