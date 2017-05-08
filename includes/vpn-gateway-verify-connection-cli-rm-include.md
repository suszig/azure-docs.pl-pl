Możesz sprawdzić, czy połączenie powiodło się, używając polecenia [az network vpn-connection show](/cli/azure/network/vpn-connection#show). Skonfiguruj wartości odpowiadające Twoim własnym. W podanym przykładzie opcja --name odnosi się do nazwy utworzonego połączenia, które ma zostać przetestowane.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

Gdy proces nawiązywania połączenia nadal trwa, jego stan połączenia to „Łączenie”. Gdy połączenie zostanie nawiązane, jego stan zmienia się na „Połączone” i możesz zobaczyć bajty przychodzące oraz wychodzące.