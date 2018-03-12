## <a name="rest"></a>Wdróż plik ZIP z interfejsów API REST 

Można użyć [wdrożenia usługi interfejsów API REST](https://github.com/projectkudu/kudu/wiki/REST-API) można wdrożyć pliku zip do aplikacji na platformie Azure. Aby wdrożyć, wysłanie żądania POST do https://<app_name>.scm.azurewebsites.net/api/zipdeploy. Żądanie POST musi zawierać plik zip w treści wiadomości. Poświadczenia wdrażania dla aplikacji znajdują się w żądaniu przy użyciu uwierzytelniania podstawowe HTTP. Aby uzyskać więcej informacji, zobacz [odwołanie do wdrożenia wypychania .zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Uwierzytelnianie podstawowe HTTP należy poświadczenia wdrażania usługi aplikacji. Aby sprawdzić, jak ustawić poświadczenia wdrażania, zobacz [ustawić i zresetowanie poświadczeń na poziomie użytkownika](../articles/app-service/app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>Z cURL

W poniższym przykładzie użyto narzędzie cURL można wdrożyć pliku zip. Zastąp symbole zastępcze `<username>`, `<password>`, `<zip_file_path>`, i `<app_name>`. Po wyświetleniu monitu przez narzędzie cURL, wpisz hasło.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

To żądanie wyzwala instalacji wypychanej przesłanego pliku zip. Możesz przejrzeć wdrożeń bieżących i starszych przy użyciu punktu końcowego https://<app_name>.scm.azurewebsites.net/api/deployments, jak pokazano w poniższym przykładzie cURL. Ponownie, Zastąp `<app_name>` o nazwie aplikacji i `<deployment_user>` z nazwą użytkownika poświadczeń wdrożenia.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>Z programem PowerShell

W poniższym przykładzie użyto [Invoke RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) można wysłać żądania, który zawiera plik zip. Zastąp symbole zastępcze `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, i `<app_name>`.

```PowerShell
#PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<zip_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/zipdeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

To żądanie wyzwala instalacji wypychanej przesłanego pliku zip. Aby przejrzeć bieżących i starszych wdrożenia, uruchom następujące polecenia. Ponownie, Zastąp `<app_name>` symbolu zastępczego.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
