1. Na stronie portalu po lewej kliknij pozycję **+** i wpisz tekst „brama sieci wirtualnej” w polu wyszukiwania. W obszarze **Wyniki** zlokalizuj i kliknij pozycję **Brama sieci wirtualnej**. U dołu bloku **Brama sieci wirtualnej** kliknij pozycję **Utwórz**. Spowoduje to otwarcie bloku **Tworzenie bramy sieci wirtualnej**.
2. W bloku **Tworzenie bramy sieci wirtualnej** wypełnij wartości dla swojej bramy sieci wirtualnej.

    ![Tworzenie pól bloku bramy sieci wirtualnej](./media/vpn-gateway-add-gw-s2s-rm-portal-include/newgw.png "Nowa brama")
3. **Nazwa**: Nadaj nazwę bramie. Nie chodzi o nazwę podsieci bramy. Jest to nazwa obiektu bramy, który zostanie utworzony.
4. **Typ bramy**: Wybierz pozycję **Sieć VPN**. Bramy sieci VPN używają typu bramy sieci wirtualnej **Sieć VPN**. 
5. **Typ sieci VPN**: Wybierz typ sieci VPN określony dla danej konfiguracji. Większość konfiguracji wymaga zastosowania typu sieci VPN opartego na trasach.
6. **Jednostka SKU**: Wybierz jednostkę SKU bramy z listy rozwijanej. Jednostki SKU wymienione na liście rozwijanej zależą od wybranego typu sieci VPN.
7. **Lokalizacja**: Aby zobaczyć pozycję Lokalizacja, może być konieczne przewinięcie ekranu. Dostosuj pole **Lokalizacja**, aby wskazywało miejsce, w którym znajduje się sieć wirtualna. Jeśli lokalizacja nie wskazuje regionu, w którym znajduje się Twoja sieć wirtualna, sieć ta nie będzie widoczna na liście rozwijanej „Wybierz sieć wirtualną” w następnym kroku.
8. **Sieć wirtualna**: Wybierz sieć wirtualną, do której chcesz dodać tę bramę. Kliknij polecenie **Sieć wirtualna**, aby otworzyć blok **Wybieranie sieci wirtualnej**. Wybierz sieć wirtualną. Jeśli sieć wirtualna nie jest widoczna, upewnij się, że wartość w polu **Lokalizacja** wskazuje region, w którym znajduje się sieć wirtualna.
9. **Publiczny adres IP**: W tym bloku zostanie utworzony obiekt publicznego adresu IP, do którego zostanie dynamicznie przypisany publiczny adres IP. Kliknij polecenie **Publiczny adres IP**, aby otworzyć blok **Wybieranie publicznego adresu IP**. Następnie kliknij przycisk **Utwórz nowy**, aby otworzyć **blok Tworzenie bramy sieci lokalnej**. Wprowadź nazwę dla publicznego adresu IP. Kliknij przycisk **OK**, aby zapisać zmiany w tym bloku.

    ![Tworzenie publicznego adresu IP](./media/vpn-gateway-add-gw-s2s-rm-portal-include/createpip.png "Tworzenie adresu PIP")
10. **Subskrypcja**: Sprawdź, czy wybrano poprawną subskrypcję.
11. **Grupa zasobów**: To ustawienie jest określane przez wybraną sieć wirtualną. 
12. Nie zmieniaj ustawienia **Lokalizacja** po określeniu poprzednich ustawień.
13. Sprawdź poprawność ustawień. Jeśli chcesz, aby brama była wyświetlana na pulpicie nawigacyjnym, możesz wybrać opcję **Przypnij do pulpitu nawigacyjnego** znajdującą się u dołu bloku.
14. Kliknij przycisk **Utwórz**, aby rozpocząć tworzenie bramy. Zostanie sprawdzona poprawność ustawień, a na pulpicie nawigacyjnym pojawi się kafelek „Wdrażanie bramy sieci wirtualnej”. Tworzenie bramy może potrwać do 45 minut. Być może będzie trzeba odświeżyć stronę portalu, aby zobaczyć, czy tworzenie zostało ukończone.
    
    ![Tworzenie bramy](./media/vpn-gateway-add-gw-s2s-rm-portal-include/creategw.png "Tworzenie bramy")
15. Po utworzeniu bramy sprawdź adres IP, który został do niej przypisany, spoglądając na sieć wirtualną w portalu. Brama będzie widoczna jako urządzenie podłączone. Możesz kliknąć podłączone urządzenie (bramę sieci wirtualnej), aby wyświetlić więcej informacji.