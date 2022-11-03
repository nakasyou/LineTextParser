# LineTextParser
LINEの、トーク履歴を書き出したtxtファイルを解析します
## 導入
~~~bash
pip install LineTextParser
~~~
## 使い方
### import
~~~python
import LineTextParser as ltp
~~~
### ファイル読み込み
talk.txt:
~~~
2022.10.13 木曜日
00:00 taro おはよう
00:00 rumia そーなのかー
00:12 hanako おはよー
00:12 rumia そーなのかー
2022.10.14 金曜日
16:19 taro スタンプ
16:19 rumia そーなのかー
~~~
```talk.txt```を読み込ませる場合:
```python
with open("talk.txt","r",encoding="utf-8") as f:
    #lordクラスのインスタンスtalkを作成
    talk = ltp.lord(f.read())
```
### 読み取り
読み取りには、メソッド```search```を使用します。返り値は、speechクラスのインスタンスです。
```python
search(
    getDate:tuple,　#タプルの形式で日時を指定　((2020,10,2,10,10),(2022,10,1,12,00))で2020/10/2 10:10から2022/10/1 12:00までのトークを指定
    user:str #ユーザー名を指定、なければ全てのユーザーが対象になる
    )
```
例:
```python
print(talk.search(
    getDate=((2022,10,13,00,00),(2022,10,14,00,00)),
    user="taro"
))
#>>[speech(y=2022,M=10,d=13,day=木曜日,h=0,m=0,user=taro,seq=おはよう)]

print(talk.search(
    getDate=((2022,10,13,00,00),(2022,10,14,00,00)),
))
#>>[speech(y=2022,M=10,d=13,day=木曜日,h=0,m=0,user=taro,seq=おはよう), speech(y=2022,M=10,d=13,day=木曜日,h=0,m=0,user=rumia,seq=そーなのかー), speech(y=2022,M=10,d=13,day=木曜日,h=0,m=12,user=hanako,seq=おはよー), speech(y=2022,M=10,d=13,day=木曜日,h=0,m=12,user=rumia,seq=そーなのかー)]
```
speechクラスから要素を取り出せます。
~~~python
speech=talk.search(getDate=((2022,10,13,00,00),(2022,10,14,00,00)),)[0]
speech.y#年
speech.M#月
...
~~~
