### <a name="noconnection"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej — brak połączenia bramy

#### <a name="to-add-additional-address-prefixes"></a>Aby dodać dodatkowe prefiksy adresów:

1. Dla zasobu bramy sieci lokalnej w **ustawienia** kliknij **konfiguracji**.
2. Dodawanie przestrzeni adresów IP w *Dodaj dodatkowy zakres adresów* pole.
3. Kliknij przycisk **zapisać** Aby zapisać ustawienia.

#### <a name="to-remove-address-prefixes"></a>Aby usunąć prefiksy adresów:

1. Dla zasobu bramy sieci lokalnej w **ustawienia** kliknij **konfiguracji**.
2. Kliknij przycisk **"..."** na wiersz zawierający prefiks chcesz usunąć.
3. Kliknij przycisk **Usuń**.
4. Kliknij przycisk **zapisać** Aby zapisać ustawienia.

### <a name="withconnection"></a>Aby zmodyfikować prefiksy adresów IP bramy sieci lokalnej — istniejące połączenie bramy

Jeśli istnieje już połączenie bramy i chcesz dodać lub usunąć prefiksy adresów IP zawarte w bramie Twojej sieci lokalnej, wykonaj kolejno następujące kroki. Spowoduje to pewien przestój połączenia sieci VPN. W przypadku modyfikowania prefiksów adresów IP nie musisz usuwać bramy sieci VPN. Musisz usunąć tylko połączenie.

#### <a name="1-remove-the-connection"></a>1. Usuń połączenie.

1. Dla zasobu bramy sieci lokalnej w **ustawienia** kliknij **połączenia**.
2. Kliknij przycisk **...**  wiersza dla każdego połączenia, następnie kliknij przycisk **usunąć**.
3. Kliknij przycisk **zapisać** Aby zapisać ustawienia.

#### <a name="2-modify-the-address-prefixes"></a>2. Zmodyfikuj prefiksy adresów.

Aby dodać dodatkowe prefiksy adresów:

1. Dla zasobu bramy sieci lokalnej w **ustawienia** kliknij **konfiguracji**.
2. Dodawanie przestrzeni adresów IP.
3. Kliknij przycisk **zapisać** Aby zapisać ustawienia.

Aby usunąć prefiksy adresów:

1. Dla zasobu bramy sieci lokalnej w **ustawienia** kliknij **konfiguracji**.
2. Kliknij przycisk **...**  na wiersz zawierający prefiks do usunięcia.
3. Kliknij przycisk **Usuń**.
4. Kliknij przycisk **zapisać** Aby zapisać ustawienia.

#### <a name="3-recreate-the-connection"></a>3. Utwórz ponownie połączenie.

1. Przejdź do bramy sieci wirtualnej sieci wirtualnej. (Nie bramy sieci lokalnej.)
2. W bramie sieci wirtualnej w **ustawienia** kliknij **połączenia**.
3. Kliknij przycisk **+ Dodaj** otworzyć **Dodaj połączenie** bloku.
4. Utwórz ponownie połączenie.
5. Kliknij przycisk **OK** do utworzenia połączenia.