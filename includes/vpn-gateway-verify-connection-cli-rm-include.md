Masz możliwość sprawdzenia, czy połączenie powiodło się, używając następującego polecenia interfejsu wiersza polecenia. Skonfiguruj wartości odpowiadające Twoim własnym. W podanym przykładzie opcja -n odnosi się do nazwy utworzonego połączenia, które chcesz przetestować.

```azurecli
az network vpn-connection show -n VNet1toSite2 -g TestRG1
```

Gdy proces nawiązywania połączenia nadal trwa, jego stan połączenia to „Łączenie”. Gdy połączenie zostanie nawiązane, jego stan zmienia się na „Połączone” i możesz zobaczyć bajty przychodzące oraz wychodzące.