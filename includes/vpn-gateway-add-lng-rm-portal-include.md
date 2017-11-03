1. W portalu w obszarze **Wszystkie zasoby** kliknij pozycję **+ Dodaj**. W **wszystko** bloku pole wyszukiwania, wpisz **bramy sieci lokalnej**, a następnie kliknij, aby powrócić do listy zasobów. Kliknij pozycję **Brama sieci lokalnej**, aby otworzyć blok, a następnie kliknij pozycję **Utwórz**, aby otworzyć blok **Utwórz bramę sieci lokalnej**.
   
    ![utwórz bramę sieci lokalnej](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)

2. W **bloku Utwórz bramę sieci lokalnej** określ **nazwę** obiektu bramy sieci lokalnej. Jeśli to możliwe, użyj elementu intuicyjny, takich jak **ClassicVNetLocal** lub **TestVNet1Local**. Dzięki temu można łatwiej zidentyfikować bramy sieci lokalnej w portalu.
3. Określ prawidłowy publicznego **adres IP** urządzenia sieci VPN lub bramy sieci wirtualnej, z którym chcesz się połączyć.<br>**Jeśli ta sieci lokalnej reprezentuje lokalizacji lokalnej:** Określ adres publiczny adres IP urządzenia sieci VPN, z którego chcesz nawiązać połączenie. Nie może ono znajdować się za translatorem adresów sieciowych i musi być dostępne za pomocą usługi Azure.<br>**Jeśli ta sieci lokalnej reprezentuje innej sieci wirtualnej:** Określ adres publicznego adresu IP, który został przypisany do bramy sieci wirtualnej dla tej sieci wirtualnej.<br>**Jeśli nie masz jeszcze adres IP:** można uzupełnić adresu IP nieprawidłowy symbol zastępczy i wrócić i zmodyfikować to ustawienie przed nawiązaniem połączenia.
4. **Przestrzeń adresowa** odwołuje się do zakresów adresów sieci, które reprezentuje sieć lokalna. Można dodać wiele zakresów przestrzeni adresów. Upewnij się, że zakresy określone w tym miejscu nie nakładają się na zakresy z innymi sieciami, na którym się łączysz.
5. W polu **Subskrypcja** sprawdź, czy wyświetlana jest prawidłowa subskrypcja.
6. W polu **Grupa zasobów** wybierz grupę zasobów, która ma być używana. Możesz utworzyć nową grupę zasobów lub wybrać już utworzoną.
7. Aby uzyskać **lokalizacji**, wybierz lokalizację, w której zostanie utworzony tego zasobu. Można wybrać tę samą lokalizację, w której znajduje się sieć wirtualna, ale nie jest to konieczne.
8. Kliknij przycisk **Utwórz**, aby utworzyć bramę sieci lokalnej.

