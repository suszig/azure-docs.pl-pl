1. W portalu w obszarze **Wszystkie zasoby** kliknij pozycję **+ Dodaj**.
2. W **wszystko** strony pola wyszukiwania, wpisz **bramy sieci lokalnej**, a następnie kliknij, aby powrócić do listy zasobów. Kliknij pozycję **Brama sieci lokalnej**, aby otworzyć stronę, a następnie kliknij pozycję **Utwórz**, aby otworzyć stronę **Utwórz bramę sieci lokalnej**.

  ![utwórz bramę sieci lokalnej](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)
3. Na stronie **Utwórz bramę sieci lokalnej** określ wartości dla bramy sieci lokalnej.

  - **Nazwa:** określ nazwę obiektu bramy sieci lokalnej. Jeśli to możliwe, użyj elementu intuicyjny, takich jak **ClassicVNetLocal** lub **TestVNet1Local**. Dzięki temu można łatwiej zidentyfikować bramy sieci lokalnej w portalu.
  - **Adres IP:** Określ prawidłową publicznego **adres IP** urządzenia sieci VPN lub bramy sieci wirtualnej, z którym chcesz się połączyć.

    * **Jeśli ta sieci lokalnej reprezentuje lokalizacji lokalnej:** Określ adres publiczny adres IP urządzenia sieci VPN, z którego chcesz nawiązać połączenie. Nie może ono znajdować się za translatorem adresów sieciowych i musi być dostępne za pomocą usługi Azure.
    * **Jeśli ta sieci lokalnej reprezentuje innej sieci wirtualnej:** Określ adres publicznego adresu IP, który został przypisany do bramy sieci wirtualnej dla tej sieci wirtualnej.
    * **Jeśli nie masz jeszcze adres IP:** można uzupełnić adresu IP nieprawidłowy symbol zastępczy i wrócić i zmodyfikować to ustawienie przed nawiązaniem połączenia.
  - **Przestrzeń adresowa** odwołuje się do zakresów adresów sieci, które reprezentuje sieć lokalna. Można dodać wiele zakresów przestrzeni adresów. Upewnij się, że zakresy określone w tym miejscu nie nakładają się na zakresy z innymi sieciami, na którym się łączysz.
  - **Skonfiguruj ustawienia protokołu BGP:** użyj tej opcji tylko w przypadku konfigurowania protokołu BGP. W przeciwnym razie nie wybieraj jej.
  - **Subskrypcja:** sprawdź, czy wyświetlana jest prawidłowa subskrypcja.
  - **Grupa zasobów** wybierz grupę zasobów, która ma być używana. Możesz utworzyć nową grupę zasobów lub wybrać już utworzoną.
  - **Lokalizacja:** wybierz lokalizację, w której ten obiekt zostanie utworzony. Można wybrać tę samą lokalizację, w której znajduje się sieć wirtualna, ale nie jest to konieczne.
4. Kliknij przycisk **Utwórz**, aby utworzyć bramę sieci lokalnej.