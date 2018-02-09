## <a name="deploy-uploaded-zip-file"></a>Wdrażanie przekazanego pliku ZIP

W usłudze Cloud Shell wdróż przekazany plik ZIP do aplikacji internetowej przy użyciu polecenia [`az webapp deployment source config-zip`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip). Upewnij się, że fragment *\<app_name>* został zastąpiony nazwą aplikacji internetowej.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myAppFiles.zip
```

To polecenie wdraża pliki i katalogi z pliku ZIP do domyślnego folderu aplikacji App Service (`\home\site\wwwroot`) i ponownie uruchamia aplikację. Jeśli został skonfigurowany jakikolwiek dodatkowy niestandardowy proces kompilacji, również zostanie uruchomiony.
