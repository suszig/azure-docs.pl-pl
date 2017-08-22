1. W [portalu](http://portal.azure.com) przejdź do sieci wirtualnej usługi Resource Manager, dla której chcesz utworzyć bramę sieci wirtualnej.
2. W sekcji **Ustawienia** na stronie sieci wirtualnej kliknij pozycję **Podsieci**, aby rozwinąć stronę **Podsieci**.
3. Na stronie **Podsieci** kliknij pozycję **+Podsieć bramy**, aby otworzyć stronę **Dodaj podsieć**. 

  ![Dodawanie podsieci bramy](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Dodawanie podsieci bramy")
4. **Nazwa** dla podsieci zostanie automatycznie wypełniona wartością „GatewaySubnet”. Ta wartość jest wymagana, aby platforma Azure mogła rozpoznać podsieć jako podsieć bramy. Dostosuj automatycznie wypełniane wartości ustawienia **Zakres adresów** do wymagań dotyczących konfiguracji, a następnie kliknij przycisk **OK** w dolnej części strony, aby utworzyć podsieć.

  ![Dodawanie podsieci](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Dodawanie podsieci")