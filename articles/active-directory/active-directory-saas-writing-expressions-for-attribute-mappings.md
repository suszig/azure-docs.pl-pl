<properties
    pageTitle="Azure Active Directory における属性マッピングの式の書き方 | Microsoft Azure"
    description="Azure Active Directory で SaaS アプリ オブジェクトを自動プロビジョニングしているときに、式マッピングを使用して属性値を許容される形式に変換する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2015"
    ms.author="markusvi"/>


# Azure Active Directory における属性マッピングの式の書き方

SaaS アプリケーションに対してプロビジョニングを構成するときに指定できる属性マッピングの種類の 1 つは、式マッピングです。 
 この場合は、ユーザーのデータを SaaS アプリケーションが許容可能な形式に変換することができる、スクリプトのような式を記述する必要があります。





## 構文の概要

属性マッピングの式の構文は、Visual Basic のApplications (VBA) 関数に似ています。

- 式全体は、後にかっこで囲まれた引数名で構成される関数の形式で定義する必要があります。 <br>
*たとえば、FunctionName (<< 引数 1 >> <<argument N>>)*


- 各関数内で他の関数を入れ子にすることができます。 例: <br> *FunctionOne (FunctionTwo(<<argument1>>))*


- 関数には、次の 3 つの異なる種類の引数を渡すことができます。

   1. 属性。角かっこで囲む必要があります。 例: [attributeName]

   2. 文字列定数。二重引用符で囲む必要があります。 例: "United States"

   3. 他の関数 例: FunctionOne (<<argument1>>、FunctionTwo (<<argument2>>))


- 文字列定数では、文字列に円記号 (\) または引用符 (") を含める必要がある場合は、円記号 (\) でエスケープする必要があります。 例:"会社名: \"Contoso\""



## 関数の一覧

[追加](#append) & nbsp; & nbsp; & nbsp; & nbsp; [FormatDateTime](#formatdatetime) & nbsp; & nbsp; & nbsp; & nbsp; [参加](#join) & nbsp; & nbsp; & nbsp; & nbsp; [Mid](#mid) & nbsp; & nbsp; & nbsp; & nbsp; [いない](#not) & nbsp; & nbsp; & nbsp; (& a) nbsp; [置換](#replace) & nbsp; & nbsp; & nbsp; (& a) nbsp; [StripSpaces](#stripspaces) & nbsp; & nbsp; & nbsp; & nbsp; [スイッチ](#switch)





----------
### 追加

**関数:**<br> 
Append(source, suffix)

**説明:**<br> 
ソース文字列の値を取得し、クエリの末尾に suffix を追加します。
 
**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | String | 通常は、source オブジェクトの属性の名前。 |
| **suffix** | 必須 | String | source 値の末尾に追加する文字列。 |


----------
### FormatDateTime

**関数:**<br> 
FormatDateTime (source、inputFormat, outputFormat)

**説明:**<br> 
日付文字列を受け取り、1 つの形式から別の形式に変換します。
 
**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | String | 通常は、source オブジェクトの属性の名前。 |
| **inputFormat** | 必須 | String | 有効な形式の source 値。 サポートされている形式は、次を参照してください。 [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx)します。 |
| **outputFormat** | 必須 | String | 出力日付の形式。 |



----------
### Join

**関数:**<br> 
Join (区切り記号、source1, source2,…)

**説明:**<br> 
複数を組み合わせることができますが、Join() は Append() に似ています **ソース** 文字列値を 1 つの文字列にし、それぞれの値を区切って、 **区切り** 文字列。

source 値の 1 つが複数値属性である場合は、その属性のすべての値を結合し、separator 値で区切ります。

 
**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **separator** | 必須 | String | source 値を 1 つの文字列に連結するときに、各値を区切るのに使用する文字列。 区切り記号が必要ない場合は、“” とすることができます。 |
| * * source1. sourceN * * | 必要に応じて、変数の回数 | String | 1 つに結合する値の文字列を指定します。 |



----------
### Mid

**関数:**<br> 
Mid (ソース、開始、長さ)

**説明:**<br> 
元の値の部分文字列を返します。 部分文字列は、source 文字列の文字のみを含む文字列です。


**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | String | 通常、属性の名前。 |
| **start** | 必須 | integer | インデックスに、 **ソース** 文字列の部分文字列を開始する必要があります。 文字列内の最初の文字のインデックスは 1、2 番目の文字のインデックスは 2です (以降同様)。 |
| **length** | 必須 | integer | 部分文字列の長さ。 外の長さが終了した場合、 **ソース** 文字列、関数はサブスト リングを **開始** の末尾までのインデックス **ソース** 文字列。 |




----------
### Not

**関数:**<br> 
Not (source)

**説明:**<br> 
ブール値を反転、 **ソース**します。 場合 **ソース** 値は"*True*"、返します"*False*"です。 以外の場合は、"*True*"です。


**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | Boolean String | 予想 **ソース** 値は"True"または"False". |



----------
### *Views\\Home\\AllDates.cshtml*

**関数:**<br> 
ObsoleteReplace (ソース、oldValue、regexPattern、regexGroupName、replacementValue、replacementAttributeName, テンプレート)

**説明:**<br>
文字列内の値を置換します。 指定されたパラメーターに応じて異なる動作をします。

-  **OldValue** と **replacementValue** 提供されます。

   - 出現するすべてのソースの oldValue を replacementValue に置き換えます

-  **OldValue** と **テンプレート** 提供されます。

   - 出現をすべて置換、 **oldValue** で、 **テンプレート** で、 **ソース** 値

-  **OldValueRegexPattern**, 、**oldValueRegexGroupName**, 、**replacementValue** 提供されます。

   - source 文字列に含まれる OldValueRegexPattern に一致するすべての値を replacementValue に置換します。

-  **OldValueRegexPattern**, 、**oldValueRegexGroupName**, 、**replacementPropertyName** 提供されます。

   - 場合 **ソース** 値を持つ **ソース** が返されます

   - 場合 **ソース** 値を持たないを使用して **oldValueRegexPattern** と **oldValueRegexGroupName** を持つプロパティから置換値を抽出する **replacementPropertyName**します。 置換値を、結果として返します。


**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | String | 通常は、source オブジェクトの属性の名前。 |
| **oldValue** | 省略可能 | String | 値が含まれる置換前 **ソース** または **テンプレート**します。 |
| **regexPattern** | 省略可能 | String | 含まれる置換前の値の正規表現パターン **ソース**します。 または、replacementPropertyName を使用するときは、置換プロパティから値を抽出するパターン。 |
| **regexGroupName** | 省略可能 | String | 内のグループの名前 **regexPattern**します。 ReplacementPropertyName が使用される場合にのみ置換プロパティから replacementValue としてこのグループの値が抽出されます。 |
| **replacementValue** | 省略可能 | String | 古い値を置き換える新しい値。 |
| **replacementAttributeName** | 省略可能 | String | source に値を指定しないときに、置換値に使用する属性の名前。 |
| **template** | 省略可能 | String |  **テンプレート** 値が指定されて、私たちは探します **oldValue** テンプレート内とソースの値で置換します。 |



----------
### StripSpaces

**関数:**<br> 
Stripspaces (source)

**説明:**<br> 
すべての文字が削除されます ("") 文字のソース文字列。

**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | String | **ソース** 値が更新されます。 |



----------
### Switch

**関数:**<br> 
Switch (ソース、defaultValue、key1、value1、key2, value2,…)

**説明:**<br> 
 **ソース** 値の一致、 **キー**, を返します **値** その **キー**します。 場合 **ソース** 値を返します。 任意のキーが一致しない **defaultValue**します。  **キー** と **値** パラメーターは、いつもペアでやってくる必要があります。 この関数には、常に、偶数個のパラメーターを指定する必要があります。

**パラメーター:**<br> 

|名前| 必須/繰り返し | 型 | メモ |
|--- | ---                 | ---  | ---   |
| **source セクション** | 必須 | String | **ソース** 値が更新されます。 |
| **defaultValue** | 省略可能 | String | source がどの key とも一致しないときに使用される既定値。 空の文字列 ("") を指定できます。 |
| **key** | 必須 | String | **キー** を比較する **ソース** を持つ値です。 |
| **値** | 必須 | String | 置換値、 **ソース** key と一致します。 |



## 例

### 既知のドメイン名をストリップする

ユーザーの電子メールから既知のドメイン名をストリップして、ユーザー名を取得する必要があります。 <br>
たとえば、ドメインが"contoso.com"の場合は、次の式を使用できます。


**式:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**サンプル入力/出力:** <br>

- **入力** (mail):"john.doe@contoso.com"

- **出力**:"john.doe"


### ユーザー名に定数のサフィックスを追加する

Salesforce Sandbox を使用している場合は、ユーザー名を同期する前に、すべてのユーザー名に追加のサフィックスを追加する必要があります。




**式:** <br>
`Append([userPrincipalName], ".test"))`

**サンプル入力/出力:** <br>

- **入力**: (userPrincipalName):"John.Doe@contoso.com"


- **出力**:"John.Doe@contoso.com.test"





### 姓の一部と名の一部を連結することでユーザー エイリアスを生成する

ユーザーの名の最初の 3 文字とユーザーの姓の最初の 5 文字を取得することでユーザー エイリアスを生成する必要があります。


**式:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**サンプル入力/出力:** <br>

- **入力** (givenName):"John"

- **入力** (surname):"Doe"

- **出力**:"JohDoe"




### 特定の形式の文字列として日付を出力する

SaaS アプリケーションに特定の形式で日付を送信します。 <br>
たとえば、ServiceNow の日付の書式設定します。



**式:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**サンプル入力/出力:**

- **入力** (extensionAttribute1):"20150123105347.1 z"

- **出力**:「2015年-01-23」





### 定義済みのオプション セットに基づいて値を置換する

Azure AD に格納されている都道府県コードに基づいて、ユーザーのタイム ゾーンを定義する必要があります。 <br>
状態コードと一致していない、定義済みオプションのいずれかの場合は、既定値"australia/sydney"を使用します。


**式:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**サンプル入力/出力:**

- **入力** (state):"QLD"

- **出力**:"Australia/brisbane"


[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

