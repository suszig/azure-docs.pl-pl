1. Na stronie portalu po lewej kliknij pozycję **+** i wpisz tekst „brama sieci wirtualnej” w polu wyszukiwania. W obszarze **Wyniki** zlokalizuj i kliknij pozycję **Brama sieci wirtualnej**.
2. U dołu bloku „Brama sieci wirtualnej” kliknij pozycję **Utwórz**. Spowoduje to otwarcie bloku **Tworzenie bramy sieci wirtualnej**.

    ![Tworzenie pól bloku bramy sieci wirtualnej](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Nowa brama")

3. W bloku **Tworzenie bramy sieci wirtualnej** określ wartości dla swojej bramy sieci wirtualnej.

  - **Nazwa**: Nadaj nazwę bramie. Nie chodzi o nazwę podsieci bramy. Jest to nazwa obiektu bramy, który zostanie utworzony.
  - **Typ bramy**: Wybierz pozycję **Sieć VPN**. Bramy sieci VPN używają typu bramy sieci wirtualnej **Sieć VPN**. 
  - **Typ sieci VPN**: Wybierz typ sieci VPN określony dla danej konfiguracji. Większość konfiguracji wymaga zastosowania typu sieci VPN opartego na trasach.
  - **Jednostka SKU**: Wybierz jednostkę SKU bramy z listy rozwijanej. Jednostki SKU wymienione na liście rozwijanej zależą od wybranego typu sieci VPN. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Gateway SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Jednostki SKU bramy).
  - **Lokalizacja**: Aby zobaczyć pozycję Lokalizacja, może być konieczne przewinięcie ekranu. Dostosuj pole **Lokalizacja**, aby wskazywało miejsce, w którym znajduje się sieć wirtualna. Jeśli lokalizacja nie wskazuje regionu, w którym znajduje się Twoja sieć wirtualna, sieć ta nie będzie widoczna na liście rozwijanej „Wybierz sieć wirtualną” w następnym kroku.
  - **Sieć wirtualna**: Wybierz sieć wirtualną, do której chcesz dodać tę bramę. Kliknij pozycję **Sieć wirtualna**, aby otworzyć blok „Wybierz sieć wirtualną”. Wybierz sieć wirtualną. Jeśli sieć wirtualna nie jest widoczna, upewnij się, że wartość w polu Lokalizacja wskazuje region, w którym znajduje się sieć wirtualna.
  - **Publiczny adres IP**: Blok „Tworzenie publicznego adresu IP” tworzy obiekt publicznego adresu IP. Publiczny adres IP jest dynamicznie przypisywany podczas tworzenia bramy sieci VPN. Brama sieci VPN aktualnie obsługuje tylko *dynamiczne* przypisywanie publicznych adresów IP. Nie oznacza to jednak, że adres IP zmienia się po przypisaniu go do bramy sieci VPN. Jedyną sytuacją, w której ma miejsce zmiana publicznego adresu IP, jest usunięcie bramy i jej ponowne utworzenie. Nie zmienia się on w przypadku zmiany rozmiaru, zresetowania ani przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN.

    - Najpierw kliknij pozycję **Publiczny adres IP**, aby otworzyć blok „Wybieranie publicznego adresu IP”, a następnie kliknij pozycję **+Utwórz nowe**, aby otworzyć blok „Tworzenie publicznego adresu IP”.
    - Następnie uzupełnij pozycję **Nazwa** dla publicznego adresu IP i kliknij przycisk **OK** w dolnej części tego bloku, aby zapisać zmiany.

      ![Tworzenie publicznego adresu IP](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "Tworzenie adresu PIP")

4. Sprawdź poprawność ustawień. Jeśli chcesz, aby brama była wyświetlana na pulpicie nawigacyjnym, możesz wybrać opcję **Przypnij do pulpitu nawigacyjnego** znajdującą się u dołu bloku. 
5. Kliknij przycisk **Utwórz**, aby rozpocząć tworzenie bramy sieci VPN. Zostanie sprawdzona poprawność ustawień, a na pulpicie nawigacyjnym pojawi się kafelek „Wdrażanie bramy sieci wirtualnej”. Tworzenie bramy może potrwać do 45 minut. Być może będzie trzeba odświeżyć stronę portalu, aby zobaczyć, czy tworzenie zostało ukończone.

Po utworzeniu bramy sprawdź adres IP, który został do niej przypisany, spoglądając na sieć wirtualną w portalu. Brama będzie widoczna jako urządzenie podłączone. Możesz kliknąć podłączone urządzenie (bramę sieci wirtualnej), aby wyświetlić więcej informacji.