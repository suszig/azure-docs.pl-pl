## <a name="rest"></a>Wdrażanie przy użyciu interfejsów API REST 
 
Można użyć [wdrożenia usługi interfejsów API REST](https://github.com/projectkudu/kudu/wiki/REST-API) można wdrożyć pliku zip do aplikacji na platformie Azure. Po prostu Wyślij żądanie POST `https://<app_name>.scm.azurewebsites.net/api/zipdeploy` zawierający plik zip w treści wiadomości. Poświadczenia wdrażania dla aplikacji znajdują się w żądaniu używane uwierzytelnianie podstawowe HTTP. Aby uzyskać więcej informacji, zobacz [temat referencyjny wdrożenia wypychania .zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

W poniższym przykładzie użyto narzędzie cURL można wysłać żądania, który zawiera plik zip. Zwinięcie z terminala można uruchomić na komputerze Mac lub Linux lub przy użyciu Bash w systemie Windows. Zastąp `<zip_file_path>` symbol zastępczy o ścieżkę do lokalizacji pliku zip projektu. Również zastąpić `<app_name>` o unikatowej nazwie aplikacji.

Zastąp `<deployment_user>` symbolu zastępczego z nazwą użytkownika poświadczeń wdrożenia. Po wyświetleniu monitu przez narzędzie cURL, wpisz hasło. Aby dowiedzieć się, jak ustawić poświadczenia wdrażania dla aplikacji, zobacz [ustawić i zresetowanie poświadczeń na poziomie użytkownika](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

To żądanie wyzwala instalacji wypychanej przesłanego pliku zip. Możesz przejrzeć bieżących i starszych wdrożeń za pomocą `https://<app_name>.scm.azurewebsites.net/api/deployments` punktu końcowego, jak pokazano w poniższym przykładzie cURL. Ponownie, Zastąp tekst `<app_name>` o nazwie aplikacji i `<deployment_user>` z nazwą użytkownika poświadczeń wdrożenia.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```