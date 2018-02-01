## <a name="create-a-service-principal"></a>Tworzenie nazwy głównej usługi

Można utworzyć nazwy głównej usługi z dostępem do rejestru kontenera, możesz użyć następujących skryptu. Aktualizacja `ACR_NAME` zmiennej o nazwie rejestru kontenera i opcjonalnie `--role` wartość w [az ad sp utworzyć do rbac] [ az-ad-sp-create-for-rbac] polecenie, aby przydzielić inne uprawnienia. Musi mieć [interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) zainstalowana tak, aby użyć tego skryptu.

Po uruchomieniu skryptu należy wziąć pod uwagę nazwy głównej usługi **identyfikator** i **hasło**. Po utworzeniu swoje poświadczenia, można skonfigurować aplikacji i usług, do uwierzytelniania do rejestru kontenera jako nazwy głównej usługi.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Użyj istniejącej nazwy głównej usługi

Aby udzielić dostępu do rejestru do istniejącej nazwy głównej usługi, należy przypisać do nazwy głównej usługi roli. Podobnie jak w przypadku tworzenia nowej usługi podmiotu zabezpieczeń, można przyznać ściągania, wypychania i ściągania i dostępu do właściciela.

Poniższy skrypt używa [utworzenia przypisania roli az] [ az-role-assignment-create] polecenie, aby udzielić *ściągania* uprawnień do nazwy głównej usługi można określić w `SERVICE_PRINCIPAL_ID` zmiennej. Dostosuj `--role` wartość, jeśli chcesz udzielić różne poziomy dostępu.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

Można znaleźć, oba te skrypty w witrynie GitHub, a także wersje środowiska PowerShell:

* [Interfejs wiersza polecenia platformy Azure][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
