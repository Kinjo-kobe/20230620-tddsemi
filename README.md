# セミナー資料

このセミナーでは、テスト駆動開発(TDD)とペアプログラミングというものに触れます。
テスト駆動においては、コードに対するテストを起点とする開発手法ですが、テストの方法が確立しないと困ります。
今回はPython言語を素材とし、pytestツールを使って評価します。

## pytestのインストール方法

### 事前チェック

* 使ってるPython以外のPythonはアンインストールしてください
    * 複数入っていると(意図的に入れてるならともかく)どのPythonのスクリプトが組み込まれるかがわからなくなってしまいます
* もしPythonが入っていない場合は、https://python.org にて最新版のPythonを入れておきましょう
* 最低でも3.8以上のPythonにしておいてください
    * 3.7は2023/6/27にEoLです

```pwsh
# バージョンチェックで存在確認
PS> python -V   # macOSの素のPythonの場合、python3としてください
Python 3.11.4
```

### pytestのインストール

```pwsh
PS> pip install pytest
# 呼び出し確認(バージョンが出ればOK)
PS> pytest -V # 大文字V
pytest 7.3.2
```

自分の入れたソフトウェアのパスの管理位できないとまともな開発者になれません。

## pytestによるテスト

テストコードの書き方は、 https://pytest.org/ を見ると基本は書いてありますが(英語のまま読んでください、自動翻訳ツールは禁止!)、

* test_fuga.py というファイル名にする
    * test_fuga という関数(メソッド)を作成する
        * テスト項目はassert文を使用する
        * assert文には結果が真偽となる式を記述し「そうなるはずである」と表明してください

とすることで、pytestスクリプトが自動検出します。

例:
```file:test_inc.py
def test_inc():
  assert inc(3) == 4    # inc(3)の結果は4になるはず!

def inc(n):
  pass # 未定義
```

※ passキーワードは、「何もしない」という処理になります、`:`使った以上インデントしてからコードを書かないとエラーになりますから、これで回避しています

ということで、pytestを起動すると、エラーがでます。

```pwsh
PS> pytest
====================================== test session starts =======================================
platform darwin -- Python 3.11.4, pytest-7.3.2, pluggy-1.0.0
rootdir: /Users/densuke/Downloads/20230620-sample
collected 1 item

test_inc.py F                                                                              [100%]

============================================ FAILURES ============================================
____________________________________________ test_inc ____________________________________________

    def test_inc():
>     assert inc(3) == 4
E     assert None == 4
E      +  where None = inc(3)

test_inc.py:2: AssertionError
==================================== short test summary info =====================================
FAILED test_inc.py::test_inc - assert None == 4
======================================= 1 failed in 0.06s ========================================
```

```
>     assert inc(3) == 4
```

この部分を実行中に、エラーが生じたということを示しています。

```
>     assert inc(3) == 4
E     assert None == 4
E      +  where None = inc(3)
```

`inc(3)`の結果(戻り値)がNoneだったため、 `None == 4`が一致しなかったことによるエラーとなります。

これはもちろん、`inc(n)`の定義が`pass`のみで値を返さなかったことが原因です。
実装してあげましょう。

```file:test_inc.py
def test_inc():
  assert inc(3) == 4

def inc(n):
  return 4
```

これは通りますね(笑)

```
 pytest
=================================================== test session starts ====================================================
platform darwin -- Python 3.11.4, pytest-7.3.2, pluggy-1.0.0
rootdir: /Users/densuke/Downloads/20230620-sample
collected 1 item

test_inc.py .                                                                                                        [100%]

==================================================== 1 passed in 0.01s =====================================================
```

ただし、assert文は複数書けるので、テストを拡充して、

```
def test_inc():
  assert inc(3) == 4
  assert inc(41) == 42
```

となると、チートはできないでしょう…
ちゃんと実装してみてください(そして書き換え後にpytestが通るかもチェックしましょう)。

これ以上の細かい話はセミナーで学び取ってください。
