## <a name="rest"></a>Wdrażanie przy użyciu interfejsów API REST 
 
Można użyć [wdrożenia usługi interfejsów API REST](https://github.com/projectkudu/kudu/wiki/REST-API) można wdrożyć pliku zip do aplikacji na platformie Azure. Po prostu Wyślij żądanie POST do https://<app_name>.scm.azurewebsites.net/api/zipdeploy. Żądanie POST musi zawierać plik zip w treści wiadomości. Poświadczenia wdrażania dla aplikacji znajdują się w żądaniu przy użyciu uwierzytelniania podstawowe HTTP. Aby uzyskać więcej informacji, zobacz [odwołanie do wdrożenia wypychania .zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

W poniższym przykładzie użyto narzędzie cURL można wysłać żądania, który zawiera plik zip. Na komputerze Mac lub Linux lub za pomocą Bash w systemie Windows, możesz uruchomić narzędzie cURL z terminala. Zastąp `<zip_file_path>` symbol zastępczy o ścieżkę do lokalizacji pliku zip projektu. Również zastąpić `<app_name>` o unikatowej nazwie aplikacji.

Zastąp `<deployment_user>` symbolu zastępczego z nazwą użytkownika poświadczeń wdrożenia. Po wyświetleniu monitu przez narzędzie cURL, wpisz hasło. Aby dowiedzieć się, jak ustawić poświadczenia wdrażania dla aplikacji, zobacz [ustawić i zresetowanie poświadczeń na poziomie użytkownika](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

To żądanie wyzwala instalacji wypychanej przesłanego pliku zip. Możesz przejrzeć wdrożeń bieżących i starszych przy użyciu punktu końcowego https://<app_name>.scm.azurewebsites.net/api/deployments, jak pokazano w poniższym przykładzie cURL. Ponownie, Zastąp `<app_name>` o nazwie aplikacji i `<deployment_user>` z nazwą użytkownika poświadczeń wdrożenia.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```