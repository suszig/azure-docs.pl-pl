1. W portalu przejdź kolejno do poleceń **Nowy** > **Sieci** > **Brama sieci lokalnej**.

    ![utwórz bramę sieci lokalnej](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. W **bloku Utwórz bramę sieci lokalnej** określ **nazwę** obiektu bramy sieci lokalnej.
 
3. Określ prawidłowy publiczny **adres IP** dla urządzenia sieci VPN lub bramy sieci wirtualnej, z którą chcesz się połączyć.<br>Jeśli sieć lokalna reprezentuje lokalizację lokalną, oznacza to, że masz do czynienia z publicznym adresem IP urządzenia sieci VPN, z którym chcesz nawiązać połączenie. Nie może ono znajdować się za translatorem adresów sieciowych i musi być dostępne za pomocą usługi Azure.<br>Jeśli sieć lokalna reprezentuje inną sieć wirtualną, należy wskazać publiczny adres IP, który został przypisany do bramy sieci wirtualnej dla tej sieci wirtualnej.<br>

4. **Przestrzeń adresowa** odwołuje się do zakresów adresów sieci, które reprezentuje sieć lokalna. Można dodać wiele zakresów przestrzeni adresów. Upewnij się, że określone w tym miejscu zakresy nie pokrywają się z zakresami innych sieci, z którymi chcesz się łączyć.
 
5. W polu **Subskrypcja** sprawdź, czy wyświetlana jest prawidłowa subskrypcja.

6. W polu **Grupa zasobów** wybierz grupę zasobów, która ma być używana. Możesz utworzyć nową grupę zasobów lub wybrać już utworzoną.

7. W polu **Lokalizacja** wybierz lokalizację, w której ten obiekt zostanie utworzony. Można wybrać tę samą lokalizację, w której znajduje się sieć wirtualna, ale nie jest to konieczne.

8. Kliknij przycisk **Utwórz**, aby utworzyć bramę sieci lokalnej.


<!--HONumber=Sep16_HO3-->


