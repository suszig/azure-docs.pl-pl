W poprzednich krokach utworzono zasoby platformy Azure w grupie zasobów. Jeśli nie będziesz już potrzebować tych zasobów w przyszłości, możesz je usunąć przez usunięcie grupy zasobów.
 
1. Uruchom następujące polecenie, aby upewnić się, że grupa zasobów nie zawiera żadnych zasobów, które chcesz zapisać:

  ```azurecli
  az group show --name myResourceGroup
  ```

2. Jeśli wszystkie wyświetlane zasoby są tymi, które chcesz usunąć, uruchom następujące polecenie:
 
  ```azurecli
  az group delete --name myResourceGroup
  ```
