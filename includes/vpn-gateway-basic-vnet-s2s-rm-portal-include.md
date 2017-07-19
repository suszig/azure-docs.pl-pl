Aby utworzyć sieć wirtualną w modelu wdrożenia usługi Resource Manager w witrynie Azure Portal, wykonaj poniższe kroki. Użyj [przykładowych wartości](#values), jeśli wykonujesz te kroki w ramach samouczka. Jeśli nie wykonujesz tych kroków w ramach samouczka, koniecznie zastąp te wartości własnymi. Aby uzyskać więcej informacji dotyczących pracy z sieciami wirtualnymi, zobacz temat [Omówienie sieci wirtualnych](../articles/virtual-network/virtual-networks-overview.md).

1. Przejdź w przeglądarce do witryny [Azure Portal](http://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.
3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Menedżer zasobów**, a następnie kliknij przycisk **Utwórz**. Spowoduje to otwarcie bloku „Tworzenie sieci wirtualnej”.

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Tworzenie bloku sieci wirtualnej")
4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. Po wypełnieniu pól czerwony wykrzyknik zmieni się na zielony znacznik wyboru, jeśli znaki wprowadzone w polu są prawidłowe.

  - **Nazwa**: Wprowadź nazwę sieci wirtualnej. W tym przykładzie jest używana sieć TestVNet1.
  - **Przestrzeń adresowa**: Wprowadź przestrzeń adresową. Jeśli masz wiele przestrzeni adresowych do dodania, dodaj pierwszą przestrzeń adresową. Możesz dodać dodatkowe przestrzenie adresowe później, po utworzeniu sieci wirtualnej. Upewnij się, że określona przestrzeń adresowa nie nakłada się na przestrzeń adresową dla Twojej lokalizacji lokalnej.
  - **Nazwa podsieci**: Dodaj nazwę pierwszej podsieci i zakres adresów podsieci. Możesz dodać dodatkowe podsieci i podsieć bramy później, po utworzeniu tej sieci wirtualnej. 
  - **Subskrypcja**: Sprawdź, czy jest wyświetlana prawidłowa subskrypcja. Subskrypcje można zmieniać, korzystając z listy rozwijanej.
  - **Grupa zasobów**: Wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
  - **Lokalizacja**: Wybierz lokalizację sieci wirtualnej. Lokalizacja określa miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.

5. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**. Po kliknięciu przycisku **Utwórz** na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.