# 1.Python言語入門

## Pythonについて

### Pythonの実行

Pythonはコマンドラインを使用してプログラムを実行します。
Windowsなら「PowerShell」, macOSなら「ターミナル」と呼ばれるアプリケーションを利用して行います。

- Windows の例

    ```
    PS C:\> python
    Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 17:26:49) [MSC v.1900 32 bit (Intel)] on win32
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
    ```

- macOS の例

    ```
    $ python3.6
    Python 3.6.3 (v3.6.3:2c5fed86e0, Oct  3 2017, 00:32:08) 
    [GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
    ```

### スクリプトの書き方と実行方法

#### とりあえず書いて実行する

書いて...

```python
def main():
    print('GOGO')

if __name__ == '__main__':
    main()
```

実行...

```python
$ python test.py
GOGO
```

#### 一行ずつ解説

```python
def main():
```

関数定義です。

```python
    print('GOGO')
```

組み込み関数の一つprint関数を呼び出しです。 'GOGO' は文字列を引数で渡しています。

```python
if __name__ == '__main__':
```

スクリプトとしてそのファイルを実行した場合は `__name__` という変数に `__main__` という文字列が入っています。
そのためスクリプトとして実行されたときにはこの条件はTrueになるので、この中のブロックが実質的なエントリーポイントになります。
import文でimportされた場合は `__name__` にはそのモジュール名が入っているのでこのブロックは実行されません。

```python
    main()
```

関数呼び出しです。

### 主な型

#### type() - 型を調べる

組み込み関数のtype()でobjectの型を調べることができます。

```python
>>> type(1)
<class 'int'>
>>> type('1')
<class 'str'>
>>> type([1, 2, 3])
<class 'list'>
>>> type({'first': 1, 'second': 2})
<class 'dict'>
```

#### int - 整数

整数です。

```python
>>> 3 + 2  # 足し算
5
>>> 3 - 2  # 引き算
1
>>> 3 * 2  # 掛け算
6
>>> 3 / 2  # 割り算 (Python2とPython3で挙動が違うので注意)
1.5
>>> 3 // 2  # 割り算(少数切り捨て)
1
>>> 3 % 2  # 剰余
1
>>> 3 ** 2  # 冪乗
9
```

#### str - 文字列

```python
>>> 'Hello World  '.strip()  # 前後の空白文字/改行文字を消し去る
'Hello World'
>>> 'Hello World  '.split()  # 分割
['Hello', 'World']
>>> 'Hello World  '.lstrip('H')  # 左H文字を消し去る
'ello World  '
>>> 'Hello' + 'World'  # 結合
'HelloWorld'
>>> '{0} {1}'.format('Hello', 'World')  # 文字列フォーマット
'Hello World'
>>> '{word1} {word2}'.format(word1='Hello', word2='World')  # 文字列フォーマットその2
'Hello World'
```

#### list - リスト

```python
>>> [1, 2, 3]  # リストを作成
[1, 2, 3]
>>> [1, 2, 3] + [4, 5, 6]  # 結合
[1, 2, 3, 4, 5, 6]
>>> 1 in [1, 2, 3] # 含まれているかを検証 (含まれているケース)
True
>>> 5 in [1, 2, 3] # 含まれているかを検証 (含まれていないケース)
False
```

追加

```python
>>> nums = [1, 2, 3]
>>> nums
[1, 2, 3]
>>> nums.append(4)
>>> nums
[1, 2, 3, 4]

```

除去

要素が複数ある場合は最初の要素が除去されます。

```python
>>> nums = [1, 2, 3, 1]
>>> nums.remove(1)
>>> nums
[2, 3, 1]
```

#### dict - 辞書

dictは名前と値のペアを持つデータ構造です。

例として果物の名前と金額をペアとして持つdictを定義してみます。

```python
>>> fruits = {
...     'apple': 100,
...     'orange': 50,
... }
>>>
```

この `fruits` には次のような値のペアが入っています。

|key|value|
|---|-----|
|'apple'|100|
|'orange'|50|

`'apple'` の値を取得したければ[]内にkeyを指定します。

```python
>>> fruits['apple']
100
>>> fruits['orange']
50
```

存在しないkeyを指定するとKeyError例外を送出します。

```python
>>> fruits['grape']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'grape'
>>>
```

##### ちょっと応用

先ほどの `fruits` を使って果物の金額を計算してみます。
'apple' が 1この場合は次のようになります。

```python
>>> fruits['apple'] * 1
100
```

'apple' が 2この場合は次のようになります。

```python
>>> fruits['apple'] * 2
200

```

`'apple'` や 2の部分に変数を指定することもできます。
nameという変数を定義してその中に 'apple' を入れて計算すると次のようになります。

```python
>>> name = 'apple'
>>> fruits[name] * 2
200
```

個数の部分もcountという変数に入れてみます。

```python
>>> count = 2
>>> fruits[name] * count
200

```

nameやcountの値を変更することで計算する部分のコードを変更することなく計算できます。

```python
>>> name = 'orange'
>>> fruits[name] * count
100
>>> count = 20
>>> fruits[name] * count
1000
```

計算する部分のコードはコードを変更することなく使うことができています。
これをより再利用しやすいように関数として定義してみましょう。

```python
>>> fruits = {
...     'apple': 100,
...     'orange': 50,
... }
>>>
>>> def calc_fruit_amount(name, count):
...     return fruits[name] * count
...
>>>
```

calc_fruit_amount()関数はフルーツの名前と個数を指定することで合計金額を計算します。

```python
>>> calc_fruit_amount('apple', 2)
200
>>> calc_fruit_amount('apple', 3)
300
>>> calc_fruit_amount('orange', 3)
150
>>>
```

正しい金額を計算できていることがわかります。

### 制御構文

#### if

if に続く条がTrueのときにifの中のブロックを実行します。ifの仲間はif/elif/elseがあります。

```python
fruits = {
    'apple': 100,
    'orange': 50,
}

def calc_fruit_amount(name, count):
    return fruits[name] * count


def decide_amount(name, count, threshold=1000):
    amount = calc_fruit_amount(name, count)
    if amount > threshold:
        print('高い')
    elif amount == threshold:
        print('普通')
    else:  # < threshold
        print('安い')

```

decide_amount()では合計金額がthresholdより大きいか同じか未満かを判定してそれぞれ高い/普通/安いをprintしています。

#### for in

リストなどの反復可能なobjectから要素を一つづつ取り出しながらループします。
要素が終了したら抜けます。

```python

fruits = {
    'apple': 100,
    'orange': 50,
}

for name in ['apple', 'orange']:
    print('{} {} 円'.format(name, fruits[name]))
```

fruitsの名前と金額を表示しています。次のコードも同じ挙動をします。

```python

fruits = {
    'apple': 100,
    'orange': 50,
}

for name, amount in fruits.items():
    print('{} {} 円'.format(name, amount))
```

#### try

実行しようとした処理が許可されていない操作だった場合例外が送出されます。
例えばfruitsから存在しない名前の値を取得しようとした場合KeyErrorという例外が発生します。

```python
>>> fruits['ham']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'ham'
>>>
```

スクリプトの実行中に例外が発生した場合、そのままでは処理がそこで異常終了してしまいます。
発生した例外を適切に処理することで、異常終了することなく処理を継続できます。

以下の例ではcalc_fruit_amountにfruits内に存在しないnameが渡された場合はエラーメッセージを表示するようにしています。

```python
fruits = {
    'apple': 100,
    'orange': 50,
}

def calc_fruit_amount(name, count):
    try:
        return fruits[name] * count
    except:
        print('{}は存在しません'.format(name))
```

#### fizzbuzz

fizzbuzzとは

- 最初のプレイヤーは「1」と数字を発言する。
- 次のプレイヤーは直前のプレイヤーの次の数字を発言していく。
- ただし、3で割り切れる場合は「Fizz」、5で割り切れる場合は「Buzz」、両者で割り切れる場合は「Fizz Buzz」を数の代わりに発言する。

という遊びです。 (引用: https://ja.wikipedia.org/wiki/Fizz_Buzz )

プログラミングの練習課題としてよく使われる題材です。

```python
for ii in range(1, 101):
    if ii % 15 == 0:
       print('FizzBuzz')
    elif ii % 5 == 0:
       print('Buzz')
    elif ii % 3 == 0:
       print('Fizz')
    else:
       print(ii)
```

### 標準モジュール

import文を使うことで標準モジュールを使うことができます。

```python
>>> import re
```

- re 正規表現
- sys 実行しているPythonの情報などの取得や操作など
- os 実行しているOSなどの環境の情報の取得や操作など

他にもいっぱいあります。
http://docs.python.jp/3/library/index.html

### サードパーティパッケージを使う

Pythonの標準ライブラリは非常に充実していますが、少し複雑なプログラムを書いたり、システムを作成する場合、標準ライブラリだけを利用して作成するのはとても大変な作業です。
Pythonユーザによっては独自に開発したパッケージをインターネット上に公開しています。それらをサードパーティパッケージと言いいます。例えばWebアプリケーションフレームワークのDjangoやデータ解析でよく使われるpandasなどがサードパーティパッケージです。

サードパーティパッケージはpipコマンドを用いてインストールすることができます。
ここではHTTPライブラリをinstallします。

```sh
$ sudo pip install requests
```

Pythonを起動してrequestsをimportします。

```python
>>> import requests
>>>
```

Pythonインタラクティブシェルのプロンプトマーク (>>> )が表示されれば成功です。失敗した場合はImportErrorが表示されます。

example.comにHTTPでGETリクエストを投げてみます。

```python
>>> requests.get('http://example.com/')
<Response [200]>
```

requestsが不要になった場合は`pip uninstall` でアンインストールできます。
(y/n)で入力待ちになるのでyを入力しエンターキーを押下します。

注意) 権限の問題でsudoが必要な場合が多いです。

```sh
$ sudo pip uninstall requests
〜省略〜
Proceed (y/n)? y [ENTER]
  Successfully uninstalled requests-2.7.0
$
```

requestsがアンインストールされました。

サードパーティライブラリは [pypi](https://pypi.python.org/) に登録すると、パッケージ名だけでpip installできるようになります。よく使われるライブラリのほとんどがpypiに登録されています。

### 仮想環境 (venv)

複数の別のプロジェクトなどを進めると異なるversionの同じパッケージを使いたくなります。
例えばProjectAではrequests-2.6をProjectXではrequests-2.7を使いたい場合などです。
この場合先ほどのようにインストールしてしまうとどちらか一方のバージョンのrequestsしか使うことができません。Pythonではそのような問題を解決するためにvenvを使います。venvはそれぞれ独立したPythonの環境を個別に作れます。作成した環境それぞれに使いたいバージョンのサードパーティパッケージを入れて使用します。環境の扱いはmacOS/LinuxとWindowsで異なります。

#### macOS / Linuxの場合

venv環境を作成します。

```sh
$ python3.6 -m venv env
```

venv環境に入ります。(activateスクリプトをsourceコマンドで読み込む)
成功した場合はプロンプトマークが変わります。

```sh
$ source env/bin/activate
(env) $
```

例としてrequestsをインストールします。
※ venvで作成した環境はsudoが必要ありません。

```sh
(env) $ pip install requests
```

installしたrequestsがimportできます。

```python
>>> import requests
>>>
```

環境から出るためにはdeactivateを実行します。
成功するとプロンプトマークがもとに戻ります。

```sh
(env) $ deactivate
$
```

仮想環境から出たのでrequestsはimportできません。

```python
>>> import requests
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named 'requests'
```

#### Windowsの場合

##### 事前準備 (PowerShell の実行ポリシーの確認と設定)

PowerShell の場合、 実行ポリシーが規定の設定の Restricted のではスクリプトを実行することができません。
そのため venv を有効にするための activate も実行することできません。
スクリプトの実行ができるようにするためには、実行ポリシーを変更する必要があります。

現状の実行ポリシーの確認
(デフォルトでは Undefined になっているはず。)

```console
PS: C:\> Get-ExecutionPolicy -Scope Process
Undefined
```

RemoteSigned に変更する

```console
PS: C:\> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process RemoteSigned
```

対話画面で変更してもよいかを聞かれるので、[Y] で変更に同意する。  

以下は実際にやってみた例です。

![powershell-sample-1](images/1/powershell-sample-1.png)

###### 補足

- `-Scope Process` と付けているのは、今開いているPowerShellのウィンドウでのみ有効にするためです。
- もしも常に実行ポリシーを変更した状態にしたい場合は `-Scope CurrentUser` と置き替えて実行して下さい。
- 実行ポリシーについて、詳しく知りたい方はこちらを参考にして下さい => [about_Execution_Policies](https://technet.microsoft.com/ja-JP/library/hh847748.aspx)

##### venv の作成

```console
PS C:\> python -m venv env
PS C:\> Get-ChildItem


    ディレクトリ: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2017/10/22     22:52                env    # <= env ディレクトリが作成される。
```

##### 仮想環境の有効化

```console
PS C:\> .\env\Scripts\activate
(env) PS C:\>
```

例としてrequestsをインストールします。

```console
(env) PS C:\> pip install requests
```

installしたrequestsがimportできます。

```console
(env) PS C:\> python
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 17:26:49) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import requests
>>>
```

##### 仮想環境から抜ける(無効化)

環境から出るためにはdeactivateを実行します。
成功するとプロンプトマークがもとに戻ります。

```console
(env) PS C:\> deactivate
PS C:\>
```

仮想環境から出たのでrequestsはimportできません。

```console
PS C:\> python
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 17:26:49) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import requests
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'requests'
>>>
```

#### 仮想環境が不要になったら?

環境が不要になったらディレクトリごと削除してしまいましょう。仮想環境を作成する元になったPythonの環境が消えるわけではないので問題ありません。当然仮想環境を再度作成することもできます。

macOS / Linux

```sh
$ rm -rf env
```

Windows

```console
PS C:\> Remove-Item -Path .\env -Recurse
```

注意) env配下にソースコードなどを配置していた場合、それも消えてしまいます。ご注意ください。
