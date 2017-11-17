### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Krok 1: Przejdź do bramy sieci wirtualnej

1. W [portalu Azure](https://portal.azure.com), przejdź do **wszystkie zasoby**. 
2. Aby otworzyć blok bramy sieci wirtualnej, przejdź do bramy sieci wirtualnej, który chcesz usunąć i kliknij ją.

### <a name="step-2-delete-connections"></a>Krok 2: Usuwanie połączeń

1. W bloku dla bramy sieci wirtualnej, kliknij **połączeń** Aby wyświetlić wszystkie połączenia z bramą.
2. Kliknij przycisk **"..."** w wierszu nazwę połączenia, następnie wybierz **usunąć** z listy rozwijanej.
3. Kliknij przycisk **tak** aby upewnić się, że chcesz usunąć połączenie. Jeśli masz wiele połączeń, należy usunąć każdego połączenia.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Krok 3: Usuń bramę sieci wirtualnej

Należy pamiętać, że jeśli konfigurację P2S do tej sieci wirtualnej oprócz konfiguracji S2S, usuwanie bramy sieci wirtualnej zostanie automatycznie Odłącz wszystkich klientów P2S bez ostrzeżenia.

1. W bloku bramy sieci wirtualnej, kliknij **omówienie**.
2. Na **omówienie** bloku, kliknij przycisk **usunąć** można usunąć bramy.
