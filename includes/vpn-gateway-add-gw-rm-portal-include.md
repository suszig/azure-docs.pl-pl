1. W portalu po lewej stronie kliknij pozycję **+** i wpisz „brama sieci wirtualnej” w polu wyszukiwania. Znajdź pozycję **Brama sieci wirtualnej** w wynikach wyszukiwania i kliknij tę pozycję. Na stronie **Brama sieci wirtualnej** kliknij pozycję **Utwórz** u dołu strony, aby otworzyć stronę **Tworzenie bramy sieci wirtualnej**.
2. Na stronie **Tworzenie bramy sieci wirtualnej** wypełnij wartości dla swojej bramy sieci wirtualnej.

  ![Pola na stronie Tworzenie bramy sieci wirtualnej](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Pola na stronie Tworzenie bramy sieci wirtualnej")
3. Na **Utwórz bramę sieci wirtualnej** określ wartości dla bramy sieci wirtualnej.

  - **Nazwa**: Nadaj nazwę bramie. Nie chodzi o nazwę podsieci bramy. Jest to nazwa obiektu bramy, który zostanie utworzony.
  - **Typ bramy**: Wybierz pozycję **Sieć VPN**. Bramy sieci VPN używają typu bramy sieci wirtualnej **Sieć VPN**. 
  - **Typ sieci VPN**: Wybierz typ sieci VPN określony dla danej konfiguracji. Większość konfiguracji wymaga zastosowania typu sieci VPN opartego na trasach.
  - **Jednostka SKU**: Wybierz jednostkę SKU bramy z listy rozwijanej. Jednostki SKU wymienione na liście rozwijanej zależą od wybranego typu sieci VPN. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Gateway SKUs](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Jednostki SKU bramy).
  - **Lokalizacja**: Aby zobaczyć pozycję Lokalizacja, może być konieczne przewinięcie ekranu. Dostosuj pole **Lokalizacja**, aby wskazywało miejsce, w którym znajduje się sieć wirtualna. Jeśli w regionie, w którym sieci wirtualnej znajduje się, gdy Wybieranie sieci wirtualnej w następnym kroku nie wskazuje lokalizację, nie zostanie wyświetlony na liście rozwijanej.
  - **Sieć wirtualna**: Wybierz sieć wirtualną, do której chcesz dodać tę bramę. Kliknij przycisk **sieci wirtualnej** aby otworzyć stronę "Wybierz sieć wirtualną". Wybierz sieć wirtualną. Jeśli sieć wirtualna nie jest widoczna, upewnij się, że wartość w polu Lokalizacja wskazuje region, w którym znajduje się sieć wirtualna.
  - **Zakres adresów podsieci bramy**: to ustawienie zostanie wyświetlona tylko wtedy, jeśli nie został wcześniej utworzony podsieci bramy sieci wirtualnej. Jeśli wcześniej utworzono prawidłową podsieć bramy, to ustawienie nie będą wyświetlane.
  - **Pierwszy konfiguracji IP**: na stronie "Wybierz publiczny adres IP" tworzy obiektu publiczny adres IP, który pobiera skojarzona z bramą sieci VPN. Publiczny adres IP dynamicznie przydzielono ten obiekt po utworzeniu bramy sieci VPN. Brama sieci VPN aktualnie obsługuje tylko *dynamiczne* przypisywanie publicznych adresów IP. Nie oznacza to jednak, że adres IP zmienia się po przypisaniu go do bramy sieci VPN. Jedyną sytuacją, w której ma miejsce zmiana publicznego adresu IP, jest usunięcie bramy i jej ponowne utworzenie. Nie zmienia się on w przypadku zmiany rozmiaru, zresetowania ani przeprowadzania innych wewnętrznych czynności konserwacyjnych bądź uaktualnień bramy sieci VPN.

    - Najpierw kliknij **konfiguracji IP bramy Utwórz** otwarcie strony "Wybierz publiczny adres IP", a następnie kliknij pozycję **+ Utwórz nowy** aby otworzyć stronę "Tworzenie publicznego adresu IP".
    - Następnie wprowadź **nazwa** dla publicznego adresu IP. Pozostaw SKU jako **podstawowe** chyba, że istnieje powód, aby go zmienić na inny, następnie kliknij przycisk **OK** u dołu tej strony, aby zapisać zmiany.

      ![Tworzenie publicznego adresu IP](./media/vpn-gateway-add-gw-s2s-rm-portal-include/gwip.png "Tworzenie adresu PIP")

4. Sprawdź poprawność ustawień. Możesz wybrać **Przypnij do pulpitu nawigacyjnego** w dolnej części strony, jeśli chcesz, aby brama była wyświetlana na pulpicie nawigacyjnym. 
5. Kliknij przycisk **Utwórz**, aby rozpocząć tworzenie bramy sieci VPN. Ustawienia są prawidłowe, i zobaczysz "Wdrażanie bramy sieci wirtualnej" kafelka na pulpicie nawigacyjnym. Tworzenie bramy może potrwać do 45 minut. Być może będzie trzeba odświeżyć stronę portalu, aby zobaczyć, czy tworzenie zostało ukończone.

Po utworzeniu bramy sprawdź adres IP, który został do niej przypisany, spoglądając na sieć wirtualną w portalu. Brama jest widoczna jako urządzenie podłączone. Możesz kliknąć podłączone urządzenie (bramę sieci wirtualnej), aby wyświetlić więcej informacji.
