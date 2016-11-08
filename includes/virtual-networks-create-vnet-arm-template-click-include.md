## Wdrażanie szablonu ARM przy użyciu metody „kliknij, aby wdrożyć”
Można ponownie użyć wstępnie zdefiniowanych szablonów ARM przesłanych do obsługiwanego przez firmę Microsoft repozytorium github dostępnego dla społeczności. Szablony te można wdrożyć bezpośrednio z poziomu repozytorium github lub pobrać i zmodyfikować odpowiednio do potrzeb. Aby wdrożyć szablon, który pozwala utworzyć sieć wirtualną z dwiema podsieciami, wykonaj poniższe kroki.

1. W przeglądarce przejdź do strony [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Przewiń w dół listę szablonów, a następnie kliknij opcję **101-vnet-two-subnets**. Zapoznaj się z plikiem **README.md**, jak pokazano poniżej.
   
    ![Plik READEME.md w witrynie github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)
3. Kliknij przycisk **Wdrażaj na platformie Azure**. W razie potrzeby wprowadź poświadczenia logowania do platformy Azure. 
4. W bloku **Parametry** wprowadź wartości, których chcesz użyć w celu utworzenia nowej sieci wirtualnej, a następnie kliknij opcję **OK**. Na poniższym rysunku przedstawiono wartości mające zastosowanie do naszego scenariusza.
   
    ![Parametry szablonu ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)
5. Kliknij opcję **Grupa zasobów** i wybierz grupę zasobów, aby dodać do niej sieć wirtualną, lub kliknij opcję **Utwórz nową**, aby dodać sieć wirtualną do nowej grupy zasobów. Na poniższej ilustracji przedstawiono ustawienia nowej grupy zasobów o nazwie **TestRG**.
   
    ![Grupa zasobów](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)
6. W razie potrzeby zmień wartości ustawień **Subskrypcja** i **Lokalizacja** sieci wirtualnej.
7. Jeśli nie chcesz, aby sieć wirtualna była wyświetlana jako kafelek na **tablicy startowej**, wyłącz opcję **Przypnij do tablicy startowej**.
8. Kliknij opcję **Postanowienia prawne**, przeczytaj warunki i kliknij przycisk **Kup**, aby je zaakceptować. 
9. Kliknij pozycję **Utwórz**, aby utworzyć sieć wirtualną.
   
    ![Przesyłanie kafelka wdrażania w portalu w wersji zapoznawczej](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)
10. Po przeprowadzeniu wdrożenia kliknij kolejno opcje **TestVNet** > **Wszystkie ustawienia** > **Podsieci**, aby wyświetlić właściwości podsieci, jak pokazano poniżej.
    
     ![Tworzenie sieci wirtualnej w portalu w wersji zapoznawczej](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)

<!--HONumber=Sep16_HO3-->


