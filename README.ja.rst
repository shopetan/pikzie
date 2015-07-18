.. -*- rst -*-

========
 README
========

名前
====

Pikzie（ピクジー）

作者
====

- Kouhei Sutou <kou@clear-code.com>

ライセンス
==========

LGPLv3 or later

Pikzie?
=======

Pythonのための書きやすさとデバッグのしやすさを重視したUnit
Testing Frameworkです。

PikzieはPython標準のunittest.pyに欠けている以下の機能を提供し
ます。

- PythonらしいAPI
- 豊富な表明（assert_*）
- デバッグに使いやすい出力結果

上記に加えて以下のような特徴があります。

- ...

依存関係
========

- Python >= 2.6 （Python 3.xでも可）

インストール
============

easy_installでインストール::

  % sudo easy_install Pikzie

pipでインストール::

  % sudo pip install Pikzie

tar.gzからインストール::

  % wget http://downloads.sourceforge.net/pikzie/pikzie-1.0.0.tar.gz
  % tar xvzf pikzie-1.0.0.tar.gz
  % cd pikzie-1.0.0
  % sudo python setup.py install

リポジトリ
==========

GitHubの "clear-code/pikzie
<https://github.com/clear-code/pikzie>"_ にあります。

git::

  % git clone https://github.com/clear-code/pikzie.git
  % cd pikzie
  % sudo python setup.py install

使い方
======

以下のようなディレクトリ構造だとします。::

  . -+- lib  --- yourlib --- ...
     |
     +- test -+- run-test.py
              |
              +- __init__.py
              |
              +- test_module1.py
              |
              +- ...


雛型
----

テストの雛型は以下のようになります。::

  import pikzie
  import テスト対象のモジュール

  class TestYourModule(pikzie.TestCase):
      def setup(self):
          # 初期化用コード
          self.setup_called = True

      def teardown(self):
          # 後片付け用コード
          self.setup_called = False

      def test_condition(self): # "test_"から始める
          self.assert_true(self.setup_called)

test/run-test.pyは以下の通りです。::

  #!/usr/bin/env python

  import sys
  import os

  base_dir = os.path.abspath(os.path.join(os.path.dirname(__file__), ".."))
  sys.path.insert(0, os.path.join(base_dir, "lib"))
  sys.path.insert(0, base_dir)

  import pikzie

  sys.exit(pikzie.Tester().run())

test/run-test.pyに実行権をつけます。::

  % chmod +x test/run-test.py

以下のようにtest/run-test.pyを起動すると、test/test_*.pyテス
トを自動で読み込み、定義されているテストを実行します。::

  % test/run-test.py

次のようにtest/run-test.pyには0個以上のオプションを指定することができ
ます。::

  % test/run-test.py --priority

使えるオプションは ``--help`` オプションで確認できます。::

  % test/run-test.py --help

詳細はこのドキュメント内にある「オプション」セクションを参照してください。

テスト結果
==========

テスト結果は例えば以下のようになります。::

  ....F..............................

  1) Failure: TestLoader.test_collect_test_cases: sorted(test_case_names))
  expected: <['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']>
   but was: <['TestXXX1', 'TestXXX2', 'TestYYY']>
  diff:
  - ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']
  ?                                   -----------

  + ['TestXXX1', 'TestXXX2', 'TestYYY']
  /home/kou/work/python/pikzie/test/test_loader.py:30: test_collect_test_cases(): sorted(test_case_names))

  Finished in 0.013 seconds

  35 test(s), 55 assertion(s), 1 failure(s), 0 error(s), 0 pending(s), 0 notification(s)

進行状況
--------

一番上にある「.」と「F」の部分がテストの進行状況を示していま
す。::

  ....F..............................

各「.」、「F」が1つのテストケース（テストメソッド）を表してい
ます。「.」が成功したテストケース、「F」が失敗したテストケー
スを表しています。他にも「E」、「P」、「N」があり、それぞれエ
ラー、保留、通知を表しています。まとめると以下のようになりま
す。

.
  成功したテスト

F
  表明が失敗したテスト

E
  異常終了したテスト

P
  保留マークがついているテスト

N
  通知が行われたテスト

上記のテストを表す印はテストが実行される毎に出力されます。テ
スト実行中は、この出力で実行状況を確認できます。

テスト結果のまとめ
------------------

テストが終了すると、テスト結果のまとめを出力します。まとめは、
まず、成功しなかったテストの詳細をそれぞれ表示します。例では
1つ失敗があったのでそれを表示しています。::

  1) Failure: TestLoader.test_collect_test_cases: sorted(test_case_names))
  expected: <['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']>
   but was: <['TestXXX1', 'TestXXX2', 'TestYYY']>
  diff:
  - ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']
  ?                                   -----------

  + ['TestXXX1', 'TestXXX2', 'TestYYY']
  /home/kou/work/python/pikzie/test/test_loader.py:30: test_collect_test_cases(): sorted(test_case_names))

この例ではTestLoader.test_collect_test_casesテストケースが失
敗し、期待する結果が::

  ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']

だったのに、実際は::

  ['TestXXX1', 'TestXXX2', 'TestYYY']

になっていることを表しています。「diff:」以下の部分ではこれ
らの違いがわかりやすいように異なる部分に印を付けて表示してい
ます。::

  diff:
  - ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']
  ?                                   -----------

  + ['TestXXX1', 'TestXXX2', 'TestYYY']

また、この失敗した表明は
/home/kou/work/python/pikzie/test/test_loader.pyの30行目、
test_collect_test_cases()メソッド内の以下のような内容の部分::

  sorted(test_case_names))

にあることがわかります。

テスト結果の詳細一覧の後はテストにかかった時間が表示されま
す。::

  Finished in 0.013 seconds

最後にテスト結果の要約が表示されます。::

  35 test(s), 55 assertion(s), 1 failure(s), 0 error(s), 0 pending(s), 0 notification(s)

それぞれは以下のような意味です。

n test(s)
  n個のテストケース（テスト関数）を実行した

n assertion(s)
  n個の表明にパスした

n failure(s)
  n個の表明に失敗した

n error(s)
  n個の異常事態が発生した（例外が発生した）

n pending(s)
  n個のテストケースを保留にした（self.pend()を使用した）

n notification(s)
  n個の通知が発生した（self.notify()を使用した）

この例では35個のテストケースを実行し、55個の表明にパスし、1
個の表明に失敗したということになります。異常事態や保留にした
テストケースなどはありませんでした。

XML出力
-------

オプションで--xml-reportを指定するとテスト結果をXML形式で出力
することができます。出力されるXMLは以下のような構造になってい
ます。::

  <report>
    <result>
      <test-case>
        <name>テストケース名</name>
        <description>テストケースの説明（もしあれば）</description>
      </test-case>
      <test>
        <name>テスト名</name>
        <description>テストの説明（もしあれば）</description>
        <option><!-- 属性情報（もしあれば） -->
          <name>属性名（例: bug）</name>
          <value>属性値（例: 1234）</value>
        </option>
        <option>
          ...
        </option>
      </test>
      <status>テスト結果（[success|failure|error|pending|notification]）</status>
      <detail>テスト結果の詳細（もしあれば）</detail>
      <backtrace><!-- バックトレース（もしあれば） -->
        <entry>
          <file>ファイル名</file>
          <line>行</line>
          <info>付加情報</info>
        </entry>
        <entry>
          ...
        </entry>
      </backtrace>
      <elapsed>実行時間（例: 0.000010）</elapsed>
    </result>
    <result>
      ...
    </result>
    ...
  </report>


オプション
==========

Pikzieにオプションを指定する方法はこのドキュメント内の「雛形」セクショ
ンを参照してください。

--version                バージョンを表示して終了します。

-pPATTERN, --test-file-name-pattern=PATTERN テストファイル名
                                            にマッチするグロ
                                            ブパターンを指定
                                            します。

                                            デフォルトは
                                            test/test_*.pyで
                                            す。

-nTEST_NAME, --name=TEST_NAME  TEST_NAMEにマッチしたテストを実
                               行します。もし、TEST_NAMEが
                               "/"で囲まれていた場合は（例:
                               /test\_/）正規表現として扱いま
                               す。

                               このオプションは何度でも指定で
                               き、その場合は、どれかのパター
                               ンにマッチしたテストすべてが実
                               行されます。

-tTEST_CASE_NAME, --test-case=TEST_CASE_NAME  TEST_CASE_NAME
                                              にマッチしたテ
                                              ストケースを実
                                              行します。もし、
                                              TEST_CASE_NAME
                                              が"/"で囲まれて
                                              いた場合は（例:
                                              /TestMyLib/）正
                                              規表現として扱
                                              います。

                                              このオプション
                                              は何度でも指定
                                              でき、その場合
                                              は、どれかのパ
                                              ターンにマッチ
                                              したテストケー
                                              スすべてが実行
                                              されます。

--xml-report=FILE         テスト結果をXML形式でFILEに出力し
                          ます。

--priority                優先度に応じて実行するテストを選択
                          します。優先度が低いテストでも、前
                          回のテストでパスしていないテストは
                          実行します。

--no-priority             優先度に関係なく全てのテストを実行
                          します。（デフォルト）

-vLEVEL, --verbose=LEVEL  出力の詳細さを指定します。LEVELは
                          [s|silent|n|normal|v|verbose]のう
                          ちのどれかです。

                          このオプションはコンソールUIを使用
                          する場合だけ有効です。（現在はコン
                          ソールUIしかありません。）

-cMODE, --color=MODE      出力を色付けするかどうかを指定しま
                          す。MODEには[yes|true|no|false|auto]の
                          どれかを指定します。yesまたはtrue
                          が指定された場合はエスケープシーケ
                          ンスで色付けして出力します。
                          noまたはfalseが指定された場合は色付
                          けしません。autoあるいは値が省略さ
                          れた時は、可能なら色付けをします。

                          このオプションはコンソールUIを使用
                          する場合だけ有効です。（現在はコン
                          ソールUIしかありません。）

--color-scheme=SCHEME     出力時にどの色を使うかを指定します。
                          SCHEMEには[default]のどれかを指定
                          します。

                          このオプションはコンソールUIを使用
                          する場合だけ有効です。（現在はコン
                          ソールUIしかありません。）

リファレンス
============

表明
----

pydocを見てください。::

  % pydoc pikzie.assertions.Assertions

あるいはHTML化されたものをWeb上で見ることもできます。
http://pikzie.sourceforge.net/assertions.html

属性
----

テストに属性を加えて、テスト失敗時により有益な情報を利用する
ことができます。例えば、以下のようにテストにBug IDの情報を付
加することができます。::

  import pikzie

  class TestYourModule(pikzie.TestCase):
      @pikzie.bug(123)
      def test_invalid_input(self):
          self.assert_call_raise(IndexError, ().__getitem__, 0)

この例では、test_invalid_inputテストがBug #123のテストである
という属性を付加しています。

現在利用可能な属性は以下の通りです。

pikzie.bug(id)
  Bug ID情報としてidを設定します。

pikzie.priority(priority)
  優先度priorityに応じてそのテストが実行されるかどうかが決定
  します。優先度は以下の通りです。コマンドラインオプション
  で--no-priorityが指定された場合は優先度は利用されません。

  must
    必ず実行する。

  important
    9割の確率で実行する。

  high
    7割の確率で実行する。

  normal
    5割の確率で実行する。（デフォルト）

  low
    2.5割の確率で実行する。

  never
    実行しない。



謝辞
----

- aztmさん
 	バグレポートをしてくれました。
 	ebuildを作ってくれました。http://diary.atzm.org/20081201.html#p01

- Hideo Hattoriさん
 	バグレポートをしてくれました。
