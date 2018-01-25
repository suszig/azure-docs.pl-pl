## <a name="create-a-service-principal"></a>Tworzenie nazwy głównej usługi

Można utworzyć nazwy głównej usługi z dostępem do rejestru kontenera, możesz użyć następujących skryptu. Aktualizacja `ACR_NAME` zmiennej o nazwie rejestru kontenera i opcjonalnie `--role` wartość w [az ad sp utworzyć do rbac] [ az-ad-sp-create-for-rbac] polecenie, aby przydzielić inne uprawnienia. Musi mieć [interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) zainstalowana tak, aby użyć tego skryptu.

Po uruchomieniu skryptu należy wziąć pod uwagę nazwy głównej usługi **identyfikator** i **hasło**. Po utworzeniu swoje poświadczenia, można skonfigurować aplikacji i usług, do uwierzytelniania do rejestru kontenera jako nazwy głównej usługi.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_NAME can be any unique name within your
# subscription (you can use the default below).
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate some values required for subsequent command args
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for both docker push and pull access. Modify the
# '--role' argument value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role reader --query password --output tsv)
SP_APP_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## <a name="use-an-existing-service-principal"></a>Użyj istniejącej nazwy głównej usługi

Aby udzielić dostępu do rejestru do istniejącej nazwy głównej usługi, należy przypisać do nazwy głównej usługi roli. Podobnie jak w przypadku tworzenia nowej usługi podmiotu zabezpieczeń, można przyznać ściągania, wypychania i ściągania i dostępu do właściciela.

Poniższy skrypt używa [utworzenia przypisania roli az] [ az-role-assignment-create] polecenie, aby udzielić *ściągania* uprawnień do nazwy głównej usługi można określić w `SERVICE_PRINCIPAL_ID` zmiennej. Dostosuj `--role` wartość, jeśli chcesz udzielić różne poziomy dostępu.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_ID is the service principal's 'appId' or
# one of its 'servicePrincipalNames' values.
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_ID=<service-principal-ID>

# Populate value required for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Assign the desired role to the service principal. Modify the '--role' argument
# value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
az role assignment create --assignee $SERVICE_PRINCIPAL_ID --scope $ACR_REGISTRY_ID --role reader
```

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create