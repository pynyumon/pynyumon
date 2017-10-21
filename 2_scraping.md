# 2.スクレイピング

スクレイピングとは Web サイトから自分のほしい情報を取り出して収集する技術のことをいいます。  
おおまかな処理の流れとしては以下のイメージです。

![scraping-imagine](images/2/scraping-imagine.jpg)

Python で簡単にスクレイピングを行う際には以下のサードパーティー製のライブラリを使うと便利です。

処理 | ライブラリ | ドキュメント
-|-|-
Web ページから情報を取得 (図の1,2) | Requests | [Requests: HTTP for Humans](http://docs.python-requests.org/en/master/)
response からほしい情報を取り出す (図の3) | Beautiful Soup 4 | [Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

- Beautiful Soup 4 は有志による日本語翻訳版のドキュメントもあります。 **ただし 2013-11-19 で更新が止まっているので注意。** 参考までに。
  - http://kondou.com/BS4/ 

## requests - Webからデータを取得する

PythonでWebへのアクセスをする時に最も手軽な方法は `requests` を使う方法です。pipでインストールできます。
GETとPOSTはrequests.getとrequests.postを使えば大体事足ります。

インストール

```sh
$ pip install requests
```

詳しくはこちらを参照してください。
http://requests-docs-ja.readthedocs.org/en/latest/

## BeautifulSoup4 - HTMLを解析する

HTMLを解析するにはBeautifulSoup4を使うと良いでしょう。

```py
>>> from bs4 import BeautifulSoup
>>> soup = BeautifulSoup('<div><h1 id="test">TEST</h1></div>', 'html.parser')
>>> soup.select_one('div h1#test').text
'TEST'
```

タグ内の文字は`soup.text`で、属性には`soup['id']` (idのところは属性名)でアクセスできます。

BeautifulSoup objectのよく使うmethod

- BeautifulSoup.find() -> タグを検索して最初にhitしたタグを返す
- BeautifulSoup.find_all() -> タグを検索してhitしたタグのリストを返す
- BeautifulSoup.find_previous() -> 一つ前のタグを返す
- BeautifulSoup.find_next() -> 一つ後ろのタグを返す
- BeautifulSoup.find_parent() -> 親タグを返す
- BeautifulSoup.select() -> css selectorでタグのリストを返す
- BeautifulSoup.select_one() -> css selectorで検索して最初にhitしたタグを返す


詳しくはこちらを参照してください。
https://www.crummy.com/software/BeautifulSoup/bs4/doc/

## データの永続化

### CSV形式

CSVはカンマで区切られた形式のファイルです。csvモジュールが使えます。
csv モジュールのさらに詳しい内容についてはこちらを参照してください。
http://docs.python.jp/3/library/csv.html

#### 書き込み

```py
import csv
with open('some.csv', 'w') as f:
    writer = csv.writer(f)
    writer.writerows(['1', '2', '3'])
```

#### 読み取り

```py
import csv
with open('some.csv', 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```

### JSON形式

JSON形式もよく使われる形式です。標準モジュールの `json` モジュールを使用します。

```py
>>> import json
>>> json.dumps([1, 2, 3, 4])
'[1, 2, 3, 4]'
>>> json.loads('[1, 2, 3, 4]')
[1, 2, 3, 4]
>>> json.dumps({'aho': 1, 'ajo': 2})
'{"aho": 1, "aro": 2}'
>>> json.loads('{"aho": 1, "ajo": 2}')
{u'aho': 1, u'aro': 2}
```

- json.dumps() -> オブジェクトをJSON文字列にする
- json.loads() -> JSON文字列をオブジェクトにする
- json.dump() -> オブジェクトをJSON文字列にしてそれをファイルに書き込む
- json.load() -> ファイル内のJSON文字列を読み込んでオブジェクトにする

詳しい内容についてはこちらを参照してください。
http://docs.python.jp/3/library/json.html

## サンプル

### PyNyumon #6 の講師・メンターを取得する

[Python入門者向けハンズオン #6 - connpass](https://python-nyumon.connpass.com/event/62147/) の講師・メンター一覧を取得するものです。

```python
import requests
from bs4 import BeautifulSoup


def main():

    # connpass の PyNumon#6 のイベント参加者・申込者一覧のURL
    url = 'https://python-nyumon.connpass.com/event/62147/participation/#participants'

    # requests で参加者一覧の情報と取得する
    response = requests.get(url)

    # response から HTML 部分(content) を取得
    content = response.content

    # BeautifulSoup に content を渡して解析の準備をする
    soup = BeautifulSoup(content, 'html.parser')

    # <div class="participation_table_area mb_20">  に該当するものを取り出す
    # participation_tables は List
    participation_tables = soup.find_all('div', class_='participation_table_area mb_20')

    # participation_tables を順番に見て、"講師・メンター枠"の情報を取り出す
    for participation_table in participation_tables:
        # <table><thead><tr><th> に該当するタグの要素を取り出す (参加者枠の種類が記載されているので)
        participant_type = participation_table.table.thead.tr.th.get_text()

        # 参加者枠を示す文字に "講師・メンター枠" が含まれるものを取り出す
        if '講師・メンター枠' in participant_type:
            menters_table = participation_table

    # 講師・メンター枠の HTML の中で class=display_name に該当するものを取り出す
    # menter_names は List
    menter_names = menters_table.find_all(class_='display_name')

    # 取り出した 講師・メンター枠の要素から純粋な名前だけを取り出す(不純なものを取り除く)
    for menter_name in menter_names:
        print(menter_name.get_text().strip())


if __name__ == '__main__':
    main()
```

上記の内容を `pynyumon6-menters.py` といった適当な名前のファイルに保存して実行すると利用できます。  
実行すると以下のとおりになります。

```console
$ python pynyumon6-menters.py
Kei Iwasaki
mocamocaland
MasakiTakemori
kashew_nuts
OMEGA
Ryo KAJI
jbking
kentaro0919
nasa9084
MatsumotoAyako
massa142
NaoY
nobolis
Matthias
```

### その他

スクレイピングのサンプルとしていくつか準備しています。参考にしてください。ただし一般のサイトもあるのでリクエストをバンバン投げることはやめてください。
間違ってもループをそのまま回したらダメですよ。

- PyConJPからチュートリアルの情報を抜き取る https://github.com/TakesxiSximada/happy-scraping/tree/master/pycon.jp
- PyPIから新着パッケージの情報を抜き取る  https://github.com/TakesxiSximada/happy-scraping/tree/master/pypi.python.org
- DjangoのAdminサイトの認証を突破する https://github.com/TakesxiSximada/happy-scraping/tree/master/djangoadmin
- User-Agent詐称 https://github.com/TakesxiSximada/happy-scraping/tree/master/fake-useragent
- Javascriptで動的に生成されるデータを抜き取る https://github.com/TakesxiSximada/happy-scraping/tree/master/dynamic-page

## データ取ってみたら面白そうなサイト

- https://teratail.com/ トップページのエントリとか刈り取ってみるとよいかも
- http://isitchristmas.com/ クリスマス判定 (時期的に)
- https://data.nasa.gov/developer NASAのデータが利用できるので調べてみると面白いものがあるかもしれない

他にも良さそうなサイトはいっぱいありそう...
