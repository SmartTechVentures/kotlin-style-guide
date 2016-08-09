# Kotlinコーディング規約

# バージョン
v1.0.1

# 目次

- はじめに
- 命名規則
- ファイルフォーマット
- アクセス制御
- クラス
- 変数
- 型
- 制御
- Nullable
- エラーハンドリング
- 関数・ラムダ
- Javaとの相互間

# はじめに

このコーディング規約はKotlinがAndroid開発で使用されることを想定し、以下を方針として策定します。

- プログラマーによるエラーの発生を防ぐ
- コードの肥大化を防ぐ
- コードの可読性を高くする
- 美しいコードを保ち、発展させる

# 命名規則

## パッケージ名
全て小文字で書く。
また、ワイルドカードは使用禁止とする。

**理由**  
ワイルドカードを使用すると、どのクラスを使用しているのかが不明確になる。

良い例

```kotlin 
package foo.bar
```

悪い例

```kotlin
package foo.*
```

## クラス名
UpperCamelCaseで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
class Empty
```

悪い例

```kotlin
class empty
```

## オブジェクト名
UpperCamelCaseで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
object DataProviderManager {
...
}
```

悪い例

```kotlin
object dataProviderManager  {
...
}
```

## インターフェース名
UpperCamelCaseで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
interface MyInterface {
...
}
```

悪い例

```kotlin
interface myInterface {
...
}
```

## データクラス名
UpperCamelCaseで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
data class User(
...
)
```

悪い例

```kotlin
data class user(
...
)
```

## シールドクラス名
UpperCamelCaseで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
sealed class Expr {
...
}
```

悪い例

```kotlin
sealed class expr {
...
}
```

## 関数・メソッド
lowerCamelCaseで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
fun double(x: Int): Int {
...
}
```

悪い例

```kotlin
fun Double(X: Int): Int {
...
}
```

## var・val
lowerCamelCaseで書く。
但し、Companion Objectスコープ内に記述した定数(val)のみCONSTANT_CASEで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin  
class Person { 
val firstName = ...
var lastName = ...

val gender = ...

companion object {
var drinkAlcoholAge = ...

const val GENDER_MALE = ...
const val GENDER_FEMALE = ...
}
}
```

悪い例

```kotlin
class Person { 
val FirstName = ...
var LastName = ...

val Gender = ...

companion object {
var DRINK_ALCOHOL_AGE = ...

const val GenderMale = ...
const val GenderFemale = ...
}
}
```

## 定数
CONSTANT_CASEで書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
const val SUBSYSTEM_DEPRECATED = "This subsystem is deprecated"
```

悪い例

```kotlin
const val subsystemDeprecated = "This subsystem is deprecated"
```

## 列挙型
列挙型名はUpperCamelCase、各列挙型定数はCONSTANT_CASE書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
enum class Direction {
NORTH, SOUTH, WEST, EAST
}
```

悪い例

```kotlin
enum class Direction {
North, South, West, East
}

enum class direction {
north, south, west, east
}
```

## Type変数
1文字または2文字の大文字で書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
class Box<T>(t: T) {
var value = t
}
```

悪い例

```kotlin
class Box<Type>(t: Type) {
var value = t
}
```

# ファイルフォーマット

## コロン
型情報、エルビス演算子の後ろのコロン（:）には半角スペースを入れ、前には入れない。
それ以外のコロン（:）の前後には半角スペースを入れること。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
class Child : MyInterface {
var property: Int? = ... 
}
```
悪い例

```kotlin 
class Child: MyInterface {
var property : Int? = ... 
}
```

## アノテーション
ライブラリごとに1行改行して書く。

**理由**  
可読性のため。

良い例

```kotlin 
@LibraryA
@LibraryB
fun hoge()
```

悪い例

```kotlin 
@LibraryA @LibraryB fun hoge()
```

## セミコロンの使用禁止
行末のセミコロンの使用を禁止する。

**理由**  
不要なため。

## import順
Android関連、ライブラリ、java/javaxの順で記載する。

**理由**  
可読性、コンフリクト防止のため。

##TODO・FIXMEの記載

未対応の機能には以下を記述すること。

```kotlin
// TODO: 〜
```

修正が必要な場合は以下を記述すること。

```kotlin
// FIXME: 〜
```

**理由**  
実装漏れを防止するため。

# クラス

## シングルトン
シングルトンのクラスを作成する場合、objectを利用すること。

**理由**
Kotlin公式Referenceに従う。

## this.でのアクセス
必要な時のみ使用すること。

**理由**
統一し、冗長性を排除するため。

良い例

```kotlin 
class Person { 
val firstName = ...
var lastName = ...

fun printName() {
print("$firstName $lastName")
}

fun changeName(firstName: String, lastName: String) {
this.firstName = firstName
this.lastName = lastName
}
}
```

悪い例

```kotlin 
class Person { 
val firstName = ...
var lastName = ...

fun printName() {
print("${this.firstName} ${this.lastName}")
}
}
```

# アクセス制御

## アクセス制御
アクセス制御は可能な限りprivateを指定すること。

**理由**  
そのクラス内、メソッド内…ということが担保でき、実装変更時の影響範囲を小さくできるため。

# 変数

## 変数・定数の宣言
var宣言を使用するのは、その値が変わり得る等、明確な理由があるときのみとする。
それ以外の場合はval宣言を使用する。

**理由**  
より安全なコードにし、意図しない値の変更を防ぐため。
valを使用することでプログラマーが値が変わらないことを確認することができる。
逆に、varによって宣言された変数が変更されることを予期できる。

## 代替変数
代替変数（it）が使える場合は常に使用する。
またitについて何を指しているか分からなくなることを避けるため
名前を付ける。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
var x: String? = null
...
x?.let { x ->
print(x)
}
```

悪い例

```kotlin 
var x: String? = null
...
x?.let {
print(it)
}
```

# 型

## 型推論
最大限利用すること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin
val x = "X"
```

悪い例

```kotlin 
val x: String = "X"
```

## キャスト
左辺の型情報を省略すること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin
val s = x as String
```

悪い例

```kotlin 
val s: String = x as String
```

## スマートキャスト
スマートキャストが利用できる場合は常に使用すること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
if (x is String) {
print(x.length)
}
```

悪い例

```kotlin 
if (x is String) {
print((x as String).length)
}
```

## スマートオプショナル
スマートオプショナルが利用できる場合は常に使用すること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
var x: String? = null
...
x ?: return
```

悪い例

```kotlin 
var x: String? = null
...
if (x == null) {
return
}
```

# 制御

## When
値を1行で返す場合、{}を使用しないこと。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
val x = when (type) {
A -> “A”
B -> “B“
}
```

悪い例

```kotlin 
val x = when (type) {
A -> {
“A”
}
B -> {
“B”
}
}
```

# Nullable

## Nullableの利用
nullが入る可能性がない型にNullableを使用しないこと。

**理由**  
nullアクセスの可能性をできるだけ下げるため。

## !!演算子
使用禁止とする。Nullableへのアクセスはsafe call（?.）を使うこと。

**理由**  
NPEスローの可能性をできるだけ下げるため。

良い例

```kotlin 
var x: String? = null
...
print("length: ${x?.length}")
```

悪い例

```kotlin 
var x: String? = null
...
print("length: ${x!!.length}")
```

# エラーハンドリング

## try-catch
catchが空、もしくは「Exception e」のtry-catchは書いてはならない。

**理由**  
アプリがクラッシュした理由が不明確になるため。

# 関数・ラムダ

## letの利用
スコープ関数letが利用できる場合は常に使用すること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
var x: Int? = null
...

val result = x?.let { x * 2 } ?: 0
```

悪い例

```kotlin 
var x: Int? = null
...

val result = if (x != null) {
x * 2
} else {
0
}
```

## ラムダの利用
()内でなく、()外に書く。

**理由**  
Kotlin公式Referenceに従う。

良い例

```kotlin 
list.filter {it > 10}.map { element -> element * 2}
```

悪い例

```kotlin 
list.filter({
it > 10
}).map({
element -> element * 2
})
```

## Unitの省略
関数の戻り値がUnitの場合、Unitの記述を省略すること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
fun hoge() {
...
}
```

悪い例

```kotlin 
fun hoge(): Unit {
...
}
```

## Single Line関数
値を1行で返すような関数の場合、{}でなく=を使用すること。
また、戻り値の型がNullableかどうかを明示するため省略してはならない。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin 
fun foo(a: Int, b: Int): Int = a + b
```

悪い例

```kotlin
fun foo(a: Int, b: Int) = a + b

fun foo(a: Int, b: Int): Int {
return a + b
}
```

# Javaとの相互間

## ゲッターとセッター
Javaクラスのプロパティを利用する際、get〜、set〜でなくプロパティ名で直接アクセスすること。

**理由**  
冗長性を排除し、シンプルなコードにするため。

良い例

```kotlin
// Java側
class Person { 
private String name;

public String getName() {
return this.name;
}

public String setName(String name) {
this.name = name;
}
}

// Kotlin側
val person = Person()
person.name = "Name"
print(person.name)
```

悪い例

```kotlin
// Java側
class Person { 
private String name;

public String getName() {
return this.name;
}

public String setName(String name) {
this.name = name;
}
}

// Kotlin側
val person = Person()
person.setName("Name")
print(person.getName())
```
