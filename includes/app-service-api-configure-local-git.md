Skonfiguruj lokalne wdrożenie Git do aplikacji interfejsu API za pomocą polecenia [az webapp deployment source config-local-git](/cli/azure/appservice/web/source-control#config-local-git).   

Usługa App Service obsługuje różne metody wdrażania zawartości w aplikacjach internetowych, np. protokół FTP, lokalne narzędzie Git oraz usługi GitHub, Visual Studio Team Services i Bitbucket. W ramach tego przewodnika Szybki start wdrożenie jest przeprowadzane przy użyciu lokalnego narzędzia Git. Oznacza to, że używa się polecenia Git do wypchnięcia z lokalnego repozytorium do repozytorium na platformie Azure.  

Użyj poniższego skryptu, aby ustawić poświadczenia wdrażania na poziomie konta, które będą używane podczas wypychania kodu. Pamiętaj o podaniu wartości nazwy użytkownika i hasła.   

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```
