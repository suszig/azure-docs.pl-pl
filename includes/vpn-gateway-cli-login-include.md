Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie. Aby uzyskać więcej informacji na temat logowania się, zobacz artykuł [Rozpoczynanie pracy z interfejsem wiersza polecenia platformy Azure 2.0](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Jeśli masz więcej niż jedną subskrypcję platformy Azure, wyświetl subskrypcje dla konta.

```azurecli
az account list --all
```

Wskaż subskrypcję, której chcesz użyć.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```

