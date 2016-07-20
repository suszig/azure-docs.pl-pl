1. W portalu Azure przejdź do opcji **Nowy** **>** **Sieci** **>** **Brama sieci lokalnej**.

    ![utwórz bramę sieci lokalnej](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. W **bloku Utwórz bramę sieci lokalnej** określ **nazwę** obiektu bramy sieci lokalnej.
 
3. Określ **adres IP** bramy. Jest to adres IP zewnętrznego urządzenia sieci VPN, z którym chcesz nawiązać połączenie. Nie może ono znajdować się za translatorem adresów sieciowych i musi być dostępne za pomocą usługi Azure.

4. **Przestrzeń adresów** odnosi się do zakresów adresów w sieci lokalnej (zwykle w siedzibie firmy). Można dodać wiele zakresów przestrzeni adresów. Wprowadzone zakresy nie mogą się nakładać na żaden z zakresów przestrzeni adresów używanych w jakichkolwiek sieciach wirtualnych, które będą komunikować się za pośrednictwem bramy.  Zakresy należy skoordynować z lokalną konfiguracją, a także przestrzeniami adresów sieci wirtualnej platformy Azure.
 
5. W polu **Subskrypcja** sprawdź, czy wyświetlana jest prawidłowa subskrypcja.

6. W polu **Grupa zasobów** wybierz grupę zasobów, która ma być używana. Możesz utworzyć nową grupę zasobów lub wybrać już utworzoną. Aby utworzyć nową grupę zasobów, wpisz nazwę w polu. Aby wybrać grupę zasobów, która została już utworzona, kliknij przycisk **Grupa zasobów**, aby otworzyć blok **Grupa zasobów**, a następnie wybierz grupę zasobów, która ma być używana.

7. Jeśli tworzysz nową bramę sieci lokalnej, w polu **Lokalizacja** możesz użyć lokalizacji bramy sieci wirtualnej. Nie jest to jednak wymagane. Brama sieci lokalnej może być w innej lokalizacji. 

8. Pozostaw zaznaczoną opcję „Przypnij do pulpitu nawigacyjnego”, jeśli chcesz w prosty sposób znajdować tę bramę sieci lokalnej z poziomu pulpitu nawigacyjnego.

9. Kliknij przycisk **Utwórz**, aby utworzyć bramę sieci lokalnej. Na pulpicie nawigacyjnym zostanie wyświetlony komunikat „Wdrażanie bramy sieci lokalnej”.

10. Po utworzeniu bramy sieci lokalnej będzie ona widoczna w portalu.

    



<!--HONumber=Jun16_HO2-->


