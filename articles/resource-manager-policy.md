<properties
    pageTitle="Azure リソース マネージャーのポリシー | Microsoft Azure"
    description="Azure リソース マネージャーのポリシーを使用して、サブスクリプション、リソース グループまたは個々 のリソースのようなさまざまなスコープでの違反を防止する方法について説明します。"
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="ryjones"
    editor=""/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="11/10/2015"
    ms.author="gauravbh;tomfitz"/>

# ポリシーを使用したリソース管理とアクセス制御

Azure リソース マネージャーを通じてカスタムのアクセスを制御できるようになりました
ポリシーです。 ポリシーを使用すれば、組織のリソースを管理するために必要な規則に組織内のユーザーが違反するのを防ぐことができます。 

ポリシー定義を作成し、行為やリソースを具体的に否認します。 
サブスクリプションの場合などの必要なスコープで、これらのポリシー定義を割り当てる
リソース グループ、または個々 のリソースです。 

この記事では、ポリシーの基本構造について説明します。
ポリシーの作成に使用できる言語を定義します。 します。
さまざまなスコープでこれらのポリシーを適用する方法について説明し、
最後にこれを実現する例をいくつかは説明します。
REST API。

現在、ポリシーはプレビューとして利用できます。

## RBAC との違いは何か。

ポリシーと役割に基づいたアクセス制御のいくつかの主な違いがありますが、まず理解するには 
ポリシーと RBAC 連携します。 ポリシーを使用するには、RBAC でユーザーを認証する必要があります。 ポリシーは、RBAC とは異なり、 
既定の許可と明示的なシステムを拒否します。 

RBAC は、アクションに焦点を当てています、 **ユーザー** さまざまなスコープで実行できます。 
特定のユーザーが必要なスコープで、リソース グループに関する共同作業者役割に追加されるため、ユーザーに変更を加えることができますなど 
リソース グループ。 

ポリシーについて重点的に **リソース** さまざまなスコープで動作します。 たとえばのポリシーを使用することができます。 
またはプロビジョニングでき、場所を制限するリソースの種類を制御します 
リソースをプロビジョニングすることができます。

## 一般的なシナリオ

1 つの一般的なシナリオは、チャージ バックを部門別のタグを要求するのには
目的があります。 組織は、操作を許可する場合にのみ、
適切なコスト センターが関連付けられています。それ以外の場合、これらは、要求を拒否します。
適切なコスト センターを請求することが、
操作が実行されます。

もう 1 つの一般的なシナリオは、組織がコントロールにすることがあります。
リソースが作成される場所の場所です。 制御したい場合がありますか
特定の種類のリソースのみを許可することで、リソースへのアクセスします。
プロビジョニングします。

組織が同様に、サービス カタログを制御したり、強制するため
リソースの必要な名前付け規則。

これらのシナリオは、ポリシーを使用して次に示すように簡単に実現できます。

## ポリシー定義の構造

ポリシー定義は JSON を使用して作成します。 1 つ以上で構成されています
アクションと効果を定義する条件/論理演算子
これは、条件が満たされたときの動作を示しています。

ポリシーには、基本的に次が含まれます。

**条件/論理演算子:** 一連条件にはが含まれていますが
論理演算子のセットを操作できます。

**影響:** 影響が、条件がときにあるこれを説明します
満足 – を拒否するかを監査します。 監査有効では、警告を出力します。
サービスのイベント ログ。 たとえば、管理者は、誰かがサイズの大きな VM を作成した場合に監査を実行するポリシーを作成し、後でログを確認することができます。

    {
      "if" : {
        <condition> | <logical operator>
      },
      "then" : {
        "effect" : "deny | audit"
      }
    }


## 論理演算子

サポートされている論理演算子とその構文を次に示します。

| 演算子名     | 構文         |
| :------------- | :------------- |
| Not            | "not": {& lt; 条件や演算子 & gt;}             |
| 論理積           | "allOf" : [ {&lt;condition1&gt;},{&lt;condition2&gt;}] |
| または                         | "anyOf" : [ {&lt;condition1&gt;},{&lt;condition2&gt;}] |

入れ子になった条件を指定することはできません。

## 条件

条件を評価するかどうか、 **フィールド** または **ソース** が特定の条件を満たしています。 サポートされている条件名と構文を次に示します。

| 条件名 | 構文                |
| :------------- | :------------- |
| 等しい             | "equals" : "&lt;value&gt;"               |
| LIKE                  | "like" : "&lt;value&gt;"                   |
| 指定値を含む          | "contains" : "&lt;value&gt;"|
| [                        | "in" : [ "&lt;value1&gt;","&lt;value2&gt;" ]|
| ContainsKey    | "containsKey" : "&lt;keyName&gt;" |


## フィールドおよびソース

条件は、フィールドおよびソースを使用して構成します。 フィールドは、リソース要求ペイロードのプロパティを表します。 ソースは、要求自体の特性を表します。 

次のフィールドおよびソースがサポートされています。

フィールド: **名前**, 、**種類**, 、**型**, 、**場所**, 、**タグ**, 、**タグ**。*.

ソース: **アクション**します。 

操作に関する詳細情報を入手するには、[RBAC のロールの作成] を参照してください。 (active-directory/role-based-access-built-in-roles.md)。 

## ポリシー定義の例

これで実現するためにポリシーを定義する方法を見て、
上記のシナリオです。

### 配賦: 部門別のタグが必要

ポリシーの下を含むタグがないすべての要求を拒否します。
「コスト センター」キーです。

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }


### 地理的準拠: リソースの場所を確認

次では、場所が北ヨーロッパや西ヨーロッパでない、すべての要求を拒否するポリシーの例を示しています。

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### サービスのキュレーション: サービス カタログを選択

次では、ソースの使用例を示しています。 示していますアクションに対してのみ
型 Microsoft.Resources/\*、Microsoft.Compute/\* のサービス
Microsoft.Storage/\*、Microsoft.Network/\* が許可されます。 その他
拒否されます。

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "source" : "action",
              "like" : "Microsoft.Resources/*"
            },
            {
              "source" : "action",
              "like" : "Microsoft.Compute/*"
            },
            {
              "source" : "action",
              "like" : "Microsoft.Storage/*"
            },
            {
              "source" : "action",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### 命名規則

下の例には、条件でサポートされているワイルドカードの使い方を示しています。
「いいね」です。 条件という名前で説明したようにパターンが一致している場合 (namePrefix\ * nameSuffix) を拒否
要求。

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

## ポリシーの割り当て

サブスクリプションの場合、リソースのようなさまざまなスコープでポリシーを適用できます。
グループおよび個々 のリソースです。 ポリシーは、すべての子によって継承されます。
リソース。 ポリシーがリソース グループに適用されている場合になるように
そのリソース グループ内のすべてのリソースに適用されます。

## ポリシーの作成

このセクションでは、REST を使用してポリシーを作成できる方法の詳細情報を提供します。
API を使用して記述されています。

### REST API を使用したポリシー定義の作成

使用してポリシーを作成する、 [ポリシー定義については、REST API](https://msdn.microsoft.com/library/azure/mt588471.aspx)します。 REST API を使用すると、ポリシー定義の作成と削除、既存の定義に関する情報の取得を実行できます。

新しいポリシーを作成するには、次を実行します。

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

要求の本文は次のようになります。

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "id":"/subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
      "type":"Microsoft.Authorization/policyDefinitions",
      "name":"testdefinition"
    }


ポリシー定義は、前述の例のいずれかとして定義できます。
Api バージョンを使用して *2015年-10-01-プレビュー*します。 例と詳細については、
参照してください [ポリシー定義については、REST API](https://msdn.microsoft.com/library/azure/mt588471.aspx)します。

### PowerShell を使用したポリシー定義の作成

以下に示されているように、New-AzureRmPolicyDefinition コマンドレットを使用して新しいポリシー定義を作成できます。 以下の例では、リソースを北ヨーロッパと西ヨーロッパに限定できるポリシーを作成します。

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation onlyin certain regions" -Policy '{   "if" : {
                            "not" : {
                                "field" : "location",
                                    "in" : ["northeurope" , "westeurope"]
                                }
                              },
                                "then" : {
                                    "effect" : "deny"
                                    }
                            }'          

実行の出力は後でポリシーを割り当てるときに使用できるように、$policy オブジェクトに保存されます。 ポリシー パラメーターの場合、以下に示されているように、ポリシー インラインを指定するのではなく、ポリシーが含まれている .json ファイルへのパスを提供することもできます。

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"


## ポリシーの適用

### REST API を使用したポリシーの割り当て

必要なスコープでポリシーの定義を適用する、 [ポリシーの割り当て用の REST API](https://msdn.microsoft.com/library/azure/mt588466.aspx)します。
REST API を使用すると、ポリシー割り当ての作成と削除、既存の割り当てに関する情報の取得を実行できます。

新しいポリシー割り当てを作成するには、次を実行します。

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

ポリシー割り当ての名前は、{policy-assignment} です。 次に
api バージョンを使用して *2015年-10-01-プレビュー*します。 

要求の本文は次のようになります。

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "id":"/subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyAssignments/VMPolicyAssignment",
      "type":"Microsoft.Authorization/policyAssignments",
      "name":"VMPolicyAssignment"
    }

例と詳細については、次を参照してください。 [ポリシーの割り当て用の REST API](https://msdn.microsoft.com/library/azure/mt588466.aspx)します。

### PowerShell を使用したポリシーの割り当て

以下に示されているように、New-AzureRmPolicyAssignment コマンドレットを使用して、上記で作成したポリシーを PowerShell を介して目的のスコープに適用できます。

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
ここで、$policy は上記のように New-AzureRmPolicyDefinition コマンドレットを実行した結果として返されたポリシー オブジェクトです。 ここでのスコープは、指定するリソース グループの名前です。

上記のポリシー割り当てを削除するには、次のようにします。

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Get-AzureRmPolicyDefinition、Set-AzureRmPolicyDefinition、および Remove-AzureRmPolicyDefinition コマンドレットをそれぞれ使用して、ポリシー定義を取得、変更、または削除できます。

同様に、Get-AzureRmPolicyAssignment、Set-AzureRmPolicyAssignment、および Remove-AzureRmPolicyAssignment コマンドレットをそれぞれ使用して、ポリシーの割り当てを取得、変更、または削除することもできます。

