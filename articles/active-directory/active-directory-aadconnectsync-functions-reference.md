<properties
    pageTitle="Azure AD Connect Sync: 関数参照 | Microsoft Azure"
    description="Azure AD Connect Sync での宣言型のプロビジョニングの式の参照"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="StevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2015"
    ms.author="markusvi"/>


# Azure AD Connect Sync: 関数リファレンス


Azure Active Directory Sync では、同期時の属性値を操作するために関数を使用します。 <br>
関数の構文では、次の形式を使用して表されます。 <br>
`<output type> FunctionName(<input type> <position name>, ..)`

関数では、オーバー ロードを複数の構文を受け入れるとすると、すべての有効な構文が一覧表示されます。<br>
関数が厳密に型指定され、の一致で渡される型が、文書化された型を使用することを確認します。<br>
型が一致しない場合は、エラーがスローされます。

型は次の構文で表されます。

- **bin** – バイナリ
- **bool** – ブール
- **dt** – UTC 日付/時刻
- **enum** – 既知の定数の列挙型
- **exp** – ブール値に評価される式
- **mvbin** – 複数の値を持つバイナリ
- **mvstr** – 複数値の参照
- **num** – 数値
- **ref** – 1 つの値を持つ参照
- **str** – 単一値の文字列
- **var** – その他の (ほとんど) 任意の型のバリアント
- **void** – 値を返しません



## 関数参照

----------
**換算:**

[CBool](#cbool) & nbsp; & nbsp; & nbsp; & nbsp; [CDate](#cdate) & nbsp; & nbsp; & nbsp; & nbsp; [CGuid](#cguid) & nbsp; & nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; [ConvertFromBase64](#convertfrombase64) & nbsp; & nbsp; & nbsp; & nbsp; [ConvertToBase64](#converttobase64) & nbsp; & nbsp; & nbsp; & nbsp; [ConvertFromUTF8Hex](#convertfromutf8hex) & nbsp; & nbsp; & nbsp; & nbsp; [ConvertToUTF8Hex](#converttoutf8hex) & nbsp; & nbsp; & nbsp; & nbsp; [CNum](#cnum)  & nbsp; & nbsp; & nbsp; & nbsp; [CRef](#cref) & nbsp; & nbsp; & nbsp; & nbsp; [CStr](#cstr)  & nbsp; & nbsp; & nbsp; & nbsp; [StringFromGuid](#StringFromGuid) & nbsp; & nbsp; & nbsp; & nbsp; [StringFromSid](#stringfromsid)

**日付/時刻:**

[DateAdd](#dateadd) & nbsp; & nbsp; & nbsp; & nbsp; [DateFromNum](#datefromnum)  & nbsp; & nbsp; & nbsp; & nbsp; [FormatDateTime](#formatdatetime)  & nbsp; & nbsp; & nbsp; & nbsp; [今すぐ](#now)  & nbsp; & nbsp; & nbsp; & nbsp; [NumFromDate](#numfromdate)

**Directory**

[DNComponent](#dncomponent)  & nbsp; & nbsp; & nbsp; & nbsp; [DNComponentRev](#dncomponentrev) & nbsp; & nbsp; & nbsp; & nbsp; [EscapeDNComponent](#escapedncomponent)

**検査:**

[IsBitSet](#isbitset)  & nbsp; & nbsp; & nbsp; & nbsp; [IsDate](#isdate) & nbsp; & nbsp; & nbsp; & nbsp; [IsEmpty](#isempty)
& nbsp; & nbsp; & nbsp; & nbsp; [IsGuid](#isguid) & nbsp; & nbsp; & nbsp; & nbsp; [IsNull](#isnull) & nbsp; & nbsp; & nbsp; & nbsp; [IsNullOrEmpty](#isnullorempty) & nbsp; & nbsp; & nbsp; & nbsp; [IsNumeric](#isnumeric)  & nbsp; & nbsp; & nbsp; & nbsp; [IsPresent](#ispresent) & nbsp; & nbsp; & nbsp; & nbsp; [IsString](#isstring)

**算術:**

[BitAnd](#bitand) & nbsp; & nbsp; & nbsp; & nbsp; [BitOr](#bitor) & nbsp; & nbsp; & nbsp; & nbsp; [RandomNum](#randomnum)

**複数値**

[含まれています](#contains) & nbsp; & nbsp; & nbsp; & nbsp; [カウント](#count) & nbsp; & nbsp; & nbsp; & nbsp; [項目](#item)   & nbsp; & nbsp; & nbsp; & nbsp; [ItemOrNull](#itemornull) & nbsp; & nbsp; & nbsp; & nbsp; [参加](#join) & nbsp; & nbsp; & nbsp; & nbsp; [RemoveDuplicates](#removeduplicates) & nbsp; & nbsp; & nbsp; & nbsp; [分割](#split)

**プログラム フロー:**

[エラー](#error) & nbsp; & nbsp; & nbsp; & nbsp; [IIF](#iif)  & nbsp; & nbsp; & nbsp; & nbsp; [スイッチ](#switch)


**Text**

[GUID](#guid) & nbsp; & nbsp; & nbsp; & nbsp; [InStr](#instr) & nbsp; & nbsp; & nbsp; & nbsp; [InStrRev](#instrrev) & nbsp; & nbsp; & nbsp; & nbsp; [LCase](#lcase) & nbsp; & nbsp; & nbsp; & nbsp; [左](#left) & nbsp; & nbsp; & nbsp; & nbsp; [Len](#len) & nbsp; & nbsp; & nbsp; & nbsp; [LTrim](#ltrim)  & nbsp; & nbsp; & nbsp; & nbsp; [Mid](#mid)  & nbsp; & nbsp; & nbsp; & nbsp; [PadLeft](#padleft) & nbsp; & nbsp; & nbsp; & nbsp; [PadRight](#padright) & nbsp; & nbsp; & nbsp; & nbsp; [PCase](#pcase)   & nbsp; & nbsp; & nbsp; & nbsp; [置換](#replace) & nbsp; & nbsp; & nbsp; (& a) nbsp; [ReplaceChars](#replacechars) & nbsp; & nbsp; & nbsp; & nbsp; [右](#right) & nbsp; & nbsp; & nbsp; & nbsp; [RTrim](rtrim) & nbsp; & nbsp; & nbsp; & nbsp; [トリミング](#trim) & nbsp; & nbsp; & nbsp; & nbsp; [UCase](#ucase) & nbsp; & nbsp; & nbsp; & nbsp; [Word](#word)

----------
### BitAnd

**説明:**<br>
BitAnd 関数では、値を指定したビットを設定します。

**構文:**<br>
`num BitAnd(num value1, num value2)`

- value1, value2: ともに AND になる数値

**解説:**<br>
この関数では、両方のパラメーターをバイナリ表現に変換し、ビット設定しています。

- 0 - 1 つまたは両方に対応するビットの *マスク* と *フラグ* は 0
- 1 - 対応するビットの両方が 1 の場合。

つまり、両方のパラメーターの対応するビットが 1 の場合を除くすべてのケースで 0 を返します。

**例:**<br>
`BitAnd(&HF, &HF7)`<br>
16 進数"F"と"F7"は、この値に評価されるため、7 を返します。

----------
### BitOr

**説明:** <br>
BitOr 関数では、値を指定したビットを設定します。

**構文:** <br>
`num BitOr(num value1, num value2)`

- value1, value2: ともに OR になる数値

**解説:** <br>
この関数は、両方のパラメーターをバイナリ表現に変換し、マスクとフラグに対応するビットの一方または両方が 1 である場合に 1 を 0 に、対応するビットの両方が 0 の場合、少し設定します。 <br>
つまり、両方のパラメーターの対応するビットが 0 以外のすべての場合 1 を返します。

----------
### CBool

**説明:**<br>
CBool 関数は、式の評価結果に基づくブール値を返します

**構文:** <br>
`bool CBool(exp Expression)`

**解説:**<br>
CBool は True を返します 0 以外の値に式が評価された場合にそれ以外の場合、False を返します。


**例:**<br>
`CBool([attrib1] = [attrib2])` <br>

両方の属性が同じ値を持つ場合は、True を返します。




----------
### CDate

**説明:**<br>
CDate 関数は、文字列から UTC DateTime を返します。 DateTime は Sync ではネイティブの属性の型ではありませんが、一部の関数で使用されます。

**構文:**<br>
`dt CDate(str value)`

- Value: 日付、時刻、オプションでタイム ゾーンを含む文字列

**解説:**<br>
返される文字列は常に UTC にします。

**例:**<br>
`CDate([employeeStartTime])` <br>
開始時刻を従業員のに基づいて、DateTime を返します。

`CDate("2013-01-10 4:00 PM -8")` <br>
表す DateTime を返します"2013年-01-11 12時 00分 AM"




----------
### CGuid

**説明:**<br>
CGuid 関数は、GUID の文字列形式をバイナリ形式に変換します。

**構文:**<br>
`bin CGuid(str GUID)GUID`

- このパターンで書式設定される文字列: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx または {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}




----------
### 指定値を含む

**説明:**<br>
Contains 関数は、複数値属性内の文字列を検索します。

**構文:**<br>
`num Contains (mvstring attribute, str search)` -大文字小文字を区別<br>
`num Contains (mvstring attribute, str search, enum Casetype)`<br>
`num Contains (mvref attribute, str search)` -大文字小文字を区別

- 属性: 複数値の属性を検索します。<br>
- 検索: 属性で検索する文字列。<br>
- Casetype: CaseInsensitive または CaseSensitive。<br>

文字列が見つかった複数値の属性にインデックスを返します。 文字列が見つからない場合は 0 を返します。


**解説:**<br>
複数値の文字列属性の検索は、値で、部分文字列を見つけます。<br>
参照属性の場合、一致と見なされるためには、検索された文字列は正確に値と一致する必要があります。

**例:**<br>
`IIF(Contains([proxyAddresses],”SMTP:”)>0,[proxyAddresses],Error(“No primary SMTP address found.”))`<br>
ProxyAddresses 属性には、プライマリ電子メール アドレスにある場合 (大文字で示される"SMTP:") proxyAddress 属性では、それ以外の場合の戻り値のエラーが返さします。




----------
### ConvertFromBase64

**説明:**<br>
ConvertFromBase64 関数は、指定した base64 でエンコードされた値を通常の文字列に変換します。

**構文:**<br>
`str ConvertFromBase64(str source)` -エンコードに Unicode を前提としています <br>
`str ConvertFromBase64(str source, enum Encoding)`

- ソース: Base64 でエンコードされた文字列<br>
- Encoding: Unicode、ASCII、UTF8

**使用例**<br>
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`<br>
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

どちらの例を返す"*Hello world!*"




----------
### ConvertFromUTF8Hex

**説明:**<br>
ConvertFromUTF8Hex 関数は、指定した UTF8 の 16 進エンコードされた値を文字列に変換します。

**構文:**<br>
`str ConvertFromUTF8Hex(str source)`

- source: UTF8 の 2 バイトでエンコードされた文字列

**解説:**<br>
この関数と、結果が DN 属性のわかりやすいことで ConvertFromBase64([],UTF8) の差。<br>
この形式は Azure Active Directory で DN として使用されます。

**例:**<br>
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`<br>
返します"*Hello world!*"。




----------
### ConvertToBase64

**説明:** <br>
ConvertToBase64 関数は、文字列を Unicode base64 文字列に変換します。<br>
整数の配列の値を、base 64 桁でエンコードされているそれと同等の文字列表現に変換します。

**構文:** <br>
`str ConvertToBase64(str source)`

**例:** <br>
`ConvertToBase64("Hello world!")` <br>
"SABlAGwAbABvACAAdwBvAHIAbABkACEA"を返します。




----------
### ConvertToUTF8Hex

**説明:**<br>
ConvertToUTF8Hex 関数は、文字列を UTF8 の 16 進エンコードされた値に変換します。

**構文:**<br>
`str ConvertToUTF8Hex(str source)`

**解説:**<br>
この関数の出力形式は、Azure Active Directory によって DN 属性の形式として使用されます。

**例:** <br>
`ConvertToUTF8Hex("Hello world!")` <br>
//48656C6C6F20776F726C6421 を返します




----------
### カウント

**説明:**<br>
Count 関数は、複数値属性内の要素の数を返します

**構文:** <br>
`num Count(mvstr attribute)`




----------
### CNum

**説明:** <br>
CNum 関数は、文字列を指定し、数値データ型を返します。

**構文:** <br>
`num CNum(str value)`




----------
### CRef

**説明:** <br>
文字列参照属性に変換します。

**構文:** <br>
`ref CRef(str value)`

**例:** <br>
`CRef(“CN=LC Services,CN=Microsoft,CN=lcspool01, CN=Pools,CN=RTC Service,” & %Forest.LDAP%)`




----------
### Cstr

**説明:** <br>
CStr 関数は、文字列データ型に変換します。

**構文:** <br>
`str CStr(num value)` <br>
`str CStr(ref value)` <br>
`str CStr(bool value)` <br>

- value: 数値、参照属性、ブール値を指定できます。

**例:** <br>
`CStr([dn]) <br>`
返される行数は"cn = Joe, dc = contoso, dc = com"




----------
### DateAdd

**説明:** <br>
指定した時間間隔が追加された日付を含む日付を返します。

**構文:** <br>
`dt DateAdd(str interval, num value, dt date)`

- interval: 追加する時間間隔を表す文字列式。 文字列には次のいずれかの値が必要です。
 - yyyy: 年
 - q: 四半期
 - m: 月
 - y: 年間通算日
 - d: 日
 - w: 週日
 - ww: 週
 - h: 時
 - n: 分
 - s: 秒
- value: 追加する単位の数。 正の数 (将来の日時を取得する場合) または負の数 (過去の日時を取得する場合) を指定できます。
- date: 間隔が追加される日付を表す DateTime。

**例:** <br>
`DateAdd("m", 3, CDate("2001-01-01"))` <br>
3 か月を追加し、「2001年-04-01」を表す DateTime を返します




----------
### DateFromNum

**説明:** <br>
DateFromNum、関数に変換形式の DateTime 型 AD の日付値。

**構文:** <br>
`dt DateFromNum(num value)`

**例:** <br>
`DateFromNum([lastLogonTimestamp])` <br>
`DateFromNum(129699324000000000)` <br>
2012-01-01 を表す DateTime を返します 23時 00分: 00




----------
### DNComponent

**説明:** <br>
DNComponent 関数は、左から指定した DN コンポーネントの値を返します。

**構文:** <br>
`str DNComponent(ref dn, num ComponentNumber)`

- dn: 解釈する参照属性
- ComponentNumber: 返される DN のコンポーネント

**例:** <br>
`DNComponent([dn],1)`  <br>
//Dn が場合"cn = Joe, ou =...、Joe が返されます




----------
### DNComponentRev

**説明:** <br>
DNComponentRev 関数は、右 (端) から指定した DN コンポーネントの値を返します。

**構文:** <br>
`str DNComponentRev(ref dn, num ComponentNumber)` <br>
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- dn: 解釈する参照属性
- ComponentNumber - 返される DN のコンポーネント
- Options: DC – "dc =" ですべてのコンポーネントを無視します。

**例:** <br>
`If dn is “cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com” then DNComponentRev([dn],3)` <br>  `DNComponentRev([dn],1,”DC”)` <br>
US 両方を返します。




----------
### エラー

**説明:** <br>
誤差関数を使用して、カスタムのエラーを返します。

**構文:** <br>
`void Error(str ErrorMessage)`

**例:** <br>
`IIF(IsPresent([accountName]),[accountName],Error(“AccountName is required”))` <br>
属性 accountName が存在しない場合は、オブジェクトでエラーをスローします。




----------
### EscapeDNComponent

**説明:** <br>
EscapeDNComponent 関数は、DN の 1 つのコンポーネントし、LDAP で表すことができますので、それをエスケープします。

**構文:** <br>
`str EscapeDNComponent(str value)`

**例:** <br>
`EscapeDNComponent(“cn=” & [displayName]) & “,” & %ForestLDAP%` <br>
DisplayName 属性に LDAP でエスケープする必要がある文字がある場合でも、LDAP ディレクトリでオブジェクトを作成するようにします。




----------
### FormatDateTime

**説明:** <br>
FormatDateTime 関数を使用して、書式を指定して、DateTime 文字列を書式設定

**構文:** <br>
`str FormatDateTime(dt value, str format)`

- 値: DateTime 形式の値 <br>
- format: 変換する形式を表す文字列。

**解説:** <br>
形式の有効な値をこちら: [ユーザー定義の日付/時刻の形式 (Format 関数)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**例:** <br>

`FormatDateTime(CDate(“12/25/2007”),”yyyy-mm-dd”)` <br>
「2007年-12-25」になります。

`FormatDateTime(DateFromNum([pwdLastSet]),”yyyyMMddHHmmss.0Z”)` <br>
"20140905081453.0Z"で発生することができます。




----------
### GUID

**説明:** <br>
関数は、新しいランダムな GUID を生成します。

**構文:** <br>
`str GUID()`




----------
### IIF

**説明:** <br>  
IIF 関数では、指定した条件に基づいて使用できる値のセットの 1 つを返します。

**構文:** <br>
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- condition: 評価結果が true または false になる任意の値または式。
- valueIfTrue: 条件の評価結果が true になる場合に返される値。
- valueIfFalse: 条件の評価結果が false になる場合に返される値。

**例:** <br>
`IIF([employeeType]=“Intern”,”t-“&[alias],[alias])` <br>
"T"、ユーザーがインターン、他の場合の先頭に追加のユーザーのエイリアスを返しますが、ユーザーのエイリアスを返します。




----------
### InStr

**説明:** <br>
InStr 関数は、文字列の部分文字列の最初の出現を検索します。

**構文:** <br>

`num InStr(str stringcheck, str stringmatch)` <br>
`num InStr(str stringcheck, str stringmatch, num start)` <br>
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- stringcheck: 検索対象の文字列 <br>
- stringmatch: 検索する文字列 <br>
- 開始: 部分文字列を検索開始位置 <br>
- compare: vbTextCompare または vbBinaryCompare

**解説:** <br>
ここで部分文字列が見つかったか、見つからなかった場合は 0 の位置を返します。

**例:** <br>
`InStr("The quick brown fox","quick")` <br>
評価結果 5 になります

`InStr("repEated","e",3,vbBinaryCompare)` <br>
7 に評価されます。




----------
### InStrRev

**説明:** <br>
InStrRev 関数は、文字列の最後に見つかった部分文字列を検索します。

**構文:** <br>
`num InstrRev(str stringcheck, str stringmatch)` <br>
`num InstrRev(str stringcheck, str stringmatch, num start)` <br>
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- stringcheck: 検索対象の文字列 <br>
- stringmatch: 検索する文字列 <br>
- 開始: 部分文字列を検索開始位置 <br>
- compare: vbTextCompare または vbBinaryCompare

**解説:** <br>
ここで部分文字列が見つかったか、見つからなかった場合は 0 の位置を返します。

**例:** <br>
`InStrRev("abbcdbbbef","bb")` <br>
7 を返します。




----------
### IsBitSet

**説明:** <br>
関数 IsBitSet かどうかを少し設定されているか

**構文:** <br>
`bool IsBitSet(num value, num flag)`

- value: 評価対象の数値。flag: 評価対象のビットがある数値。

**例:** <br>
`IsBitSet(&HF,4)` <br>
ビット「4」が 16 進数値"F"に設定されているために、True を返します




----------
### IsDate

**説明:** <br>
式を指定できる場合、関数が True に評価された、IsDate は、DateTime 型として評価されます。

**構文:** <br>
`bool IsDate(var Expression)`

**解説:** <br>
CDate() が成功するかどうかを判断するために使用します。




----------
###IsEmpty

**説明:** <br>  
IsEmpty 関数は、属性が CS または MV に存在しても、空の文字列に評価される場合、True に評価されます。

**構文:** <br>
`bool IsEmpty(var Expression)`




----------
###IsGuid

**説明:** <br>
IsGuid 関数は、文字列が GUID に変換できる場合に true に評価されます。

**構文:** <br>
`bool IsGuid(str GUID)`

**解説:** <br>
これらのパターンのいずれかの後に続く文字列として GUID が定義されている: xxxxxxxx xxxx-xxxx-に従ったまたは {xxxxxxxx xxxx-xxxx-に従った}

CGuid() が成功するかどうかを判断するために使用します。

**例:** <br>
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)` <br>
StrAttribute に GUID 形式がある場合は、バイナリ表現を返し、それ以外の場合は Null を返します。




----------
###IsNull

**説明:** <br>
IsNull 関数は、式の評価結果が Null の場合は true を返します。

**構文:** <br>
`bool IsNull(var Expression)`

**解説:** <br>
属性の場合、Null は属性の不在によって表されます。

**例:** <br>
`IsNull([displayName])` <br>
属性が CS または MV に存在しない場合は、True を返します。




----------
###IsNullOrEmpty

**説明:** <br>
IsNullOrEmpty 関数は、true の場合は、式が null または空の文字列を返します。

**構文:** <br>
`bool IsNullOrEmpty(var Expression)`

**解説:** <br>
属性の評価結果は True に場合は、属性がないか存在も空の文字列です。<br>
この関数の逆の関数は IsPresent です。

**例:** <br>
`IsNull([displayName])` <br>
属性が存在しないか、CS または MV に空の文字列は、場合に True を返します。




----------
### IsNumeric

**説明:** <br>
IsNumeric 関数では、式が数値型として評価できるかどうかを示すブール値を返します。

**構文:** <br>
`bool IsNumeric(var Expression)`

**解説:** <br>
CNum() が式の解析に成功するかどうかを判断するために使用します。




----------
### IsString

**説明:** <br>
IsString 関数は、文字列型に、式を評価できる場合は True に評価されます。

**構文:** <br>
`bool IsString(var expression)`

**解説:** <br>
CStr() が式の解析に成功するかどうかを判断するために使用します。




----------
### IsPresent

**説明:** <br>
IsPresent 関数は、式が Null でないが空でない文字列に評価する場合は true を返します。

**構文:** <br>
`bool IsPresent(var expression)`

**解説:** <br>
この関数の逆関数は isnullorempty です。

**例:** <br>

`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`




----------
### 項目

**説明:** <br>
Item 関数は、複数値の文字列/属性から 1 つの項目を返します。

**構文:** <br>
`var Item(mvstr attribute, num index)`

- 属性: 複数値の属性 <br>
- index: 複数値の文字列内の項目へのインデックス。

**解説:** <br>
Item 関数は、後者の関数は、複数値属性内の項目にインデックスを返すために、Contains 関数と共に便利です。

インデックスが範囲外にある場合は、エラーをスローします。

**例:** <br>
`Mid(Item([proxyAddress],Contains([proxyAddress], ”SMTP:”)),6)`  <br>
プライマリ電子メール アドレスを返します。




----------
### ItemOrNull

**説明:** <br>
ItemOrNull 関数は、複数値の文字列/属性から 1 つの項目を返します。

**構文:** <br>
`var ItemOrNull(mvstr attribute, num index)`

- 属性: 複数値の属性 <br>
- index: 複数値の文字列内の項目へのインデックス。

**解説:** <br>
後者の関数は、複数値属性内の項目にインデックスを返すために、ItemOrNull 関数は Contains 関数と便利です。

インデックスが範囲外にある場合は、Null 値を返します。




----------
### Join

**説明:** <br>
Join 関数では、複数値の文字列の受け取りを指定した区切り記号の各項目の間に挿入を単一値の文字列を返します。

**構文:** <br>
`str Join(mvstr attribute)` <br>
`str Join(mvstr attribute, str Delimiter)`

- 属性: 複数値の属性を結合する文字列を格納しています。 <br>
- delimiter: 返される文字列内で部分文字列を区切るために使用する任意の文字列。 省略した場合は、空白文字 (" ") が使用されます。 delimiter が長さ 0 の文字列 ("") または Nothing の場合、リスト内のすべての項目は、区切り記号なしで連結されます。

**「解説」**<br>
結合と Split 関数の間でパリティがあります。 Join 関数は、文字列の配列を受け取り、区切り文字列を使用してそれらを結合し、単一の文字列を返します。 Split 関数は、文字列を受け取って区切り記号で分割し、文字列の配列を返します。 ただし、Join 関数が任意の区切り文字列を使った文字列を連結できるのに対し、Split 関数で文字列を分割する際には 1 文字の区切り記号しか使用できないという大きな違いがあります。

**例:** <br>
`Join([proxyAddresses],”,”)` <br>
返すことができます:「SMTP:john.doe@contoso.com, smtp:jd@contoso.com」




----------
### LCase

**説明:** <br>
LCase 関数は、文字列内のすべての文字を小文字に変換します。

**構文:** <br>
`str LCase(str value)`

**例:** <br>
`LCase(“TeSt”)` <br>
"Test"を返します。




----------
### Left

**説明:** <br>
Left 関数は、文字列の左端から指定された文字数を返します。

**構文:** <br>
`str Left(str string, num NumChars)`

- string: 返される文字を含む文字列 <br>
- NumChars: 文字列の左端から数えて返される文字数を指定する数値

**解説:** <br>
文字列内の最初の numChars 文字を含む文字列。

- numChars = 0 の場合、空の文字列を返します。
- numChars < 0 の場合、入力文字列を返します。
- string が null の場合、空の文字列を返します。

文字列に含まれる文字数が numChars で指定した数より少ない場合は、文字列と同一の文字列 (パラメーター 1 のすべての文字が含まれる) が返されます。

**例:** <br>
`Left(“John Doe”, 3)` <br>
"Joh"を返します。




----------
### Len

**説明:** <br>
Len 関数は、文字列の文字数を返します。

**構文:** <br>
`num Len(str value)`

**例:** <br>
`Len(“John Doe”)` <br>
8 を返します。




----------
### LTrim

**説明:** <br>
LTrim 関数は、文字列から先頭の空白文字を削除します。

**構文:** <br>
`str LTrim(str value)`

**例:** <br>
`LTrim(“ Test ”)` <br>
"Test"を返します。




----------
### Mid

**説明:** <br>
Mid 関数は、文字列の指定位置から指定された文字数を返します。

**構文:** <br>
`str Mid(str string, num start, num NumChars)`

- string: 返される文字を含む文字列 <br>
- start: 文字列内で返される文字の開始位置を指定する数値
- NumChars: 文字列の位置から数えて返される文字数を指定する数値


**解説:** <br>
文字列内の start 位置から numChars 文字を返します。<br>
文字列内の start 位置から数えて numChars 文字分の文字が含まれる文字列。

- numChars = 0 の場合、空の文字列を返します。
- numChars < 0 の場合、入力文字列を返します。
- start > 文字列の長さの場合、入力文字列を返します。
- start < = 0 の場合、入力文字列を返します。
- string が null の場合、空の文字列を返します。

文字列で start 位置から後に numChar 文字が残っていない場合、返すことのできるすべての文字が返されます。

**例:** <br>

`Mid(“John Doe”, 3, 5)` <br>
"Hn Do"を返します。

`Mid(“John Doe”, 6, 999)` <br>
"Doe"を返します。




----------
### Now

**説明:** <br>
Now 関数は、コンピューターのシステム日付と時刻に従って、現在の日付と時刻を指定する DateTime を返します。

**構文:** <br>
`dt Now()`




----------
### NumFromDate

**説明:** <br>
NumFromDate 関数は、AD の日付の書式で日付を返します。

**構文:** <br>
`num NumFromDate(dt value)`


**例:** <br>
`NumFromDate(CDate("2012-01-01 23:00:00"))` <br>
129699324000000000 を返します。




----------
### PadLeft

**説明:** <br>
PadLeft 関数左-パッド、文字列を指定した埋め込み文字を使用して、指定された長さです。

**構文:** <br>
`str PadLeft(str string, num length, str padCharacter)`

- string: 埋め込む文字列。 <br>
- length: 文字列の必要な長さを表す整数。 <br>
- padCharacter: 埋め込み文字として使用する 1 文字で構成された文字列。



----------
### 解説

- 文字列の長さが length 未満の場合、長さが length と等しくなるまで文字列の左端に padCharacter が繰り返し追加されます。
- PadCharacter には空白文字を指定できますが、null 値を指定することはできません。
- 文字列の長さが length 以上の場合、文字列は変更されない状態で返されます。
- 文字列に length 以上の長さがある場合、文字列と同一の文字列が返されます。
- 文字列の長さが length 未満の場合、padCharacter が埋め込まれた文字列が含まれる、必要な長さの新しい文字列が返されます。
- string が null の場合、この関数は空の文字列を返します。

**例:** <br>
`PadLeft(“User”, 10, “0”)` <br>
"000000 user"を返します。




----------
### PadRight

**説明:** <br>
PadRight 関数右-パッド、文字列を指定した埋め込み文字を使用して、指定された長さです。

**構文:** <br>
`str PadRight(str string, num length, str padCharacter)`

- string: 埋め込む文字列。
- length: 文字列の必要な長さを表す整数。
- padCharacter: 埋め込み文字として使用する 1 文字で構成された文字列。

**解説:**

- 文字列の長さが length 未満の場合、長さが length と等しくなるまで文字列の右端に padCharacter が繰り返し追加されます。
- padCharacter には空白文字を指定できますが、null 値を指定することはできません。
- 文字列の長さが length 以上の場合、文字列は変更されない状態で返されます。
- 文字列に length 以上の長さがある場合、文字列と同一の文字列が返されます。
- 文字列の長さが length 未満の場合、padCharacter が埋め込まれた文字列が含まれる、必要な長さの新しい文字列が返されます。
- string が null の場合、この関数は空の文字列を返します。


**例:** <br>
`PadRight(“User”, 10, “0”)` <br>
"User000000"を返します。




----------
### PCase

**説明:** <br>
PCase 関数が文字列内の各スペースで区切られた単語の最初の文字を大文字に変換し、その他のすべての文字が小文字に変換します。

**構文:** <br>
`String PCase(string)`

**例:** <br>
`PCase(“TEsT”)` <br>
"Test"を返します。




----------
### RandomNum

**説明:** <br>
RandomNum 関数は、指定した間隔の間のランダムな数を返します。

**構文:** <br>
`num RandomNum(num start, num end)`

- 開始: 生成するランダムな値の下限を指定する数値 <br>
- end: 生成するランダムな値の上限を指定する数値

**例:** <br>
`Random(100,999)` <br>
734 を返します。




----------
### RemoveDuplicates

**説明:** <br>
RemoveDuplicates 関数は、複数値の文字列を使用し、各値が一意かどうかを確認します。

**構文:** <br>  
`mvstr RemoveDuplicates(mvstr attribute)`

**例:** <br>
`RemoveDuplicates([proxyAddresses])` <br>
すべての重複する値が削除された無害化された proxyAddress 属性を返します。




----------
### *Views\\Home\\AllDates.cshtml*

**説明:** <br>
Replace 関数は、別の文字列を文字列のすべての出現を置換します。

**構文:** <br>
`str Replace(str string, str OldValue, str NewValue)`

- 文字列: の値を置換する文字列。 <br>
- OldValue: 文字列を検索し、置き換えます。 <br>
- NewValue: 置換する文字列。


**解説:** <br>
この関数は、次の特殊なモニカーを認識します。

- \n – 新しい行
- \r – キャリッジ リターン
- \t – タブ


**例:** <br>

`Replace([address],”\r\n”,”, “)` <br>
CRLF をコンマとスペースで置き換え、"1 Microsoft Way、Redmond、WA, USA"する可能性があります




----------
### ReplaceChars

**説明:** <br>
ReplaceChars 関数は、ReplacePattern 文字列内にある文字のすべての出現を置換します。

**構文:** <br>
`str ReplaceChars(str string, str ReplacePattern)`

- string: 文字を置換する文字列。
- ReplacePattern: 置換する文字のディクショナリが含まれる文字列。

形式は {source1}:{target1},{source2}:{target2},{sourceN},{targetN} です。source は検索対象の文字、target は置換する文字列です。


**解説:**

- この関数は定義した source の見つかった位置を受け取り、target と置き換えます。
- source は、厳密に 1 文字 (unicode) である必要があります。
- source は、空にすることも 2 文字以上にすることもできません (解析エラー)。
- target には、ö:oe、β:ss などの複数の文字を指定できます。
- target を空にして、文字を削除する必要があることを指定できます。
- source は、大文字と小文字を区別し、完全に一致する必要があります。
- 「,」(コンマ) と「:」(コロン) は予約文字であり、この関数を使用して置き換えることはできません。
- ReplacePattern 文字列内のスペースやその他の空白文字は無視されます。


**例:** <br>
' %Replacestring% = ': å, ö、:o、å、:a, ö, o '

`ReplaceChars(”Räksmörgås”,%ReplaceString%)` <br>
Raksmorgas を返します。

`ReplaceChars(“O’Neil”,%ReplaceString%)` <br>
"ONeil"を単一のティックを返します。 削除する定義されています。




----------
### Right

**説明:** <br>
Right 関数は、文字列の右 (端) から指定された文字数を返します。

**構文:** <br>
`str Right(str string, num NumChars)`

- string: 返される文字を含む文字列
- NumChars: 文字列の右端から数えて返される文字数を指定する数値

**解説:** <br>
NumChars 文字は文字列の last 位置から返されます。

文字列内の最後の numChars 文字分の文字を含む文字列。

- numChars = 0 の場合、空の文字列を返します。
- numChars < 0 の場合、入力文字列を返します。
- string が null の場合、空の文字列を返します。

文字列に含まれる文字数が NumChars に指定した数より少ない場合は、文字列と同一の文字列が返されます。

**例:** <br>
`Right(“John Doe”, 3)` <br>
"Doe"を返します。




----------
### RTrim

**説明:** <br>
RTrim 関数は、文字列から末尾の空白文字を削除します。

**構文:** <br>
`str RTrim(str value)`

**例:** <br>
`RTrim(“ Test ”)` <br>
"Test"を返します。




----------
### Split

**説明:** <br>
分割関数を使用するを区切り記号で区切られた文字列を受け取り、複数値の文字列になります。


**構文:** <br>
`mvstr Split(str value, str delimiter)` < br?
`mvstr Split(str value, str delimiter, num limit)`

- value: 分離する区切り文字が含まれる文字列。
- delimiter: 区切り記号として使用される 1 文字。
- limit: 返される値の最大数。

**例:** <br>
`Split(“SMTP:john.doe@contoso.com,smtp:jd@contoso.com”,”,”)` <br>
ProxyAddress 属性に有用な 2 つの要素と複数値の文字列を返します




----------
### StringFromGuid

**説明:** <br>
StringFromGuid 関数は、バイナリ GUID を文字列に変換

**構文:** <br>
`str StringFromGuid(bin GUID)`




----------
### StringFromSid

**説明:** <br>
StringFromSid 関数は、バイト配列または文字列または複数値の文字列をセキュリティ識別子を含む複数値のバイト配列に変換します。

**構文:** <br>
`str StringFromSid(bin ObjectSID)` <br>
`mvstr StringFromSid(mvbin ObjectSID)`




----------
### Switch

**説明:** <br>
Switch 関数を使用して、評価された条件に基づいて 1 つの値を返します。

**構文:** <br>
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- expr: 評価する必要のあるバリアント型の式。
- value: 対応する式が True の場合に返される値。

**解説:** <br>
Switch 関数の引数リストは、式と値のペアで構成されます。 式は左から右に評価され、評価結果が True になる最初の式に関連付けられている値が返されます。 各部分が正しくペアリングされていないと、実行時エラーが発生します。

たとえば、expr1 が True の場合、Switch は value1 を返します。 たとえば、expr1 が False でも expr2 が True の場合、Switch は value2 を返します。

スイッチは、Nothing を返す場合。
- すべての式は True です。
- 最初の True 式には、Null である対応する値があります。

Switch は、返される式が 1 つであってもすべての式を評価します。 このため、望ましくない影響が発生しないよう注意する必要があります。 たとえば、式の評価結果が 0 除算のエラーの場合、エラーが発生します。

値には、カスタム文字列を返す Error 関数を指定することもできます。

**例:** <br>
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error(“Unknown city”))` <br>
主要都市で話される言語を返し、それ以外の場合はエラーを返します。




----------
### Trim

**説明:** <br>
Trim 関数は、先頭と末尾の空白文字、文字列から削除します。

**構文:** <br>
`str Trim(str value)` <br>
`mvstr Trim(mvstr value)`

**例:** <br>
`Trim(“ Test ”)` <br>
"Test"を返します。

`Trim([proxyAddresses])` <br>
先頭および末尾の空白を proxyAddress 属性内の各値を削除します。




----------
### UCase

**説明:** <br>
UCase 関数は、文字列内のすべての文字を大文字に変換します。

**構文:** <br>
`str UCase(str string)`

**例:** <br>
`UCase(“TeSt”)` <br>
"TEST"を返します。




----------
### Word

**説明:** <br>
Word の機能では、使用して、単語の番号を返す区切り記号を記述するパラメーターに基づいて、文字列内に含まれる単語を返します。

**構文:** <br>
`str Word(str string, num WordNumber, str delimiters)`

- string: 返される単語を含む文字列。
- WordNumber: 返すべき単語の番号を指定する数値。
- delimiters: 単語を識別するために使用される区切り記号を表す文字列

**解説:** <br>
各文字列の区切り記号の文字のいずれかで区切られた文字列内の文字が単語として識別されます。

- num < 1 の場合、空の文字列を返します。
- string が null の場合、空の文字列を返します。

文字列に含まれる単語の数が指定より少ないか、区切り記号文字で識別されるどの単語も文字列に含まれていない場合は、空の文字列が返されます。


**例:** <br>
`Word(“The quick brown fox”,3,” “)` <br>
"Brown"を返します。

`Word(“This,string!has&many seperators”,3,”,!&#”)` <br>
"Has"を返します。


## その他のリソース

* [宣言型のプロビジョニングの式について](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Connect Sync: 同期オプションのカスタマイズ](active-directory-aadconnectsync-whatis.md)
* [オンプレミス ID と Azure Active Directory の統合](active-directory-aadconnect.md)


<!--Image references-->

