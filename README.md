# SDU-stack-chan-tester
SD-Updaterに対応したstackchan testerです。<br>
stack-chan test application for pwm servo

<br>
mongonta0716 さんのソフトから次の修正を行いました。<br>
- SD-Updater対応。<br>
- サーボPIN情報を外部ファイル（servo.txt）で設定。<br>
- サーボattach時の合否判定の修正。<br>

<br>
ブート時に、SD_Updater用の画面が立ち上がります。<br>
SDに入れたソフトを切り替えることができるようになります。<br>
<br>



## 設定ファイル
SD直下に置いてください。

- servo.txt<br>
Servo設定ファイル<br>
１行目(USE_SERVO) ： （使用しません。内部で必ずONにしています）<br>
２行目(SERVO_PIN_X) ： "13"(PortC)　または、"33"(PortA)<br>
３行目(SERVO_PIN_Y) ： "14"(PortC)　または、"32"(PortA)<br>
<br>

設定サンプルを用意しています。"servo.txt"に変更してください。<br>
<br>


## 必要なもの
### 本体<br>
いずれかを用意してください。<br>
・M5Stack Core2 for AWS<br>
・M5Stack Core2 <br>
・M5Stack Core2 v1.1　（未確認）<br>
<br>

### サーボ
・SG90　または互換<br>
・サーボポートは、PortAまたは、PortC のどちらも対応。<br>
<br>

### SDカード
SDルートに設定ファイル"servo.txt"が必要です。<br>
<br>


## 最新BINの取得
コンパイル済みの最新BINファイルは、下記のリポジトリから取得できます。
- [BinsPack-for-StackChan-Core2](https://github.com/NoRi-230401/BinsPack-for-StackChan-Core2)<br>
<br>


## SD-Updaterについて
tobozoさん開発。SDに複数のBINファイルを入れて、ソフトを切替えて使用できるようになります。<br>

 https://github.com/tobozo/M5Stack-SD-Updater<br><br>


タカオさん、2023/7/29 ｽﾀｯｸﾁｬﾝ お誕生日会 2023のLTで、M5Stack-SD-Updaterの概要を説明した時のスライド<br>
https://speakerdeck.com/mongonta0716/sutatukutiyandefu-shu-apuriwoqie-riti-erutekunituku

<br><br>


## 基のリポジトリ
- [stack-chan-tester　(mongonta0716さん)](https://github.com/mongonta0716/stack-chan-tester)<br>
<br>
<br><br>

ここから、基のソフトの説明書です。

-----





# stack-chan-tester

日本語 | [English](README_en.md)

スタックチャンを作成するときに、PWMサーボの調整及びテストを行うためのアプリケーションです。<br>
stack-chan test application for pwm servo

# 対応キット
 BOOTHで頒布している [ｽﾀｯｸﾁｬﾝ M5GoBottom版 組み立てキット](https://mongonta.booth.pm/)の動作確認及び組立時の設定用に開発しました。ピンの設定を変えることによりスタックチャン基板にも対応可能です。

※ ArduinoFramework及びPWMサーボのみです。

# サーボのピンの設定
CoreS3はPort.C(G18, G17)、Core2はPort.C(G13,G14)、Fireは Port.A(G22,G21)、Core1は Port.C(G16,G17)を使うようになっています。違うピンを使用する場合は下記の箇所を書き換えてください。
https://github.com/mongonta0716/stack-chan-tester/blob/main/src/main.cpp#L7-L35

# サーボのオフセット調整
SG90系のPWMサーボは個体差が多く、90°を指定しても少しずれる場合があります。その場合は下記のオフセット値を調整してください。(90°からの角度（±）を設定します。)
調整する値は、ボタンAの長押しでオフセットを調整するモードに入るので真っ直ぐになる数値を調べてください。

オフセットの調整方法はおきもくさんの[スタックチャンの作り方　M5Burner版　分解なし【後半】](https://www.youtube.com/watch?v=YRQYYrQh0oE&t=329s)を参照してください。

調整後の値の設定方法については、書き込むファームウェアによって変わります。

- ソース内の初期値を書き換えるもの。<br>初期設定で90を指定している箇所を書き換えて再ビルドします。
- 設定ファイルを書き換えるもの。<br>[stackchan-bluetooth-simple](https://github.com/mongonta0716/stackchan-bluetooth-simple)は設定ファイルでオフセットを指定します。
- Webで設定するもの。<br>NoRiさんの[WebServer-with-stackchan](https://github.com/NoRi-230401/WebServer-with-stackchan)はWebで設定する機能があります。


# 使い方
* ボタンA： X軸、Y軸のサーボを90°にします。固定前に90°にするときに使用してください。（90°の位置はPWMサーボによって若干異なるためオフセット値の調整値を調べる必要があります。）
* ボタンB： X軸は0〜180, Y軸は90〜50まで動きます。<br>ダブルクリックすると、Groveの5V(ExtPower)出力のON/OFFを切り替えます。Stack-chan_Takao_Baseの後ろから給電をチェックする場合OFFにします。
* ボタンC： ランダムで動きます。
    * ボタンC: ランダムモードの停止
* ボタンAの長押し：オフセットを調整して調べるモードに入ります。
    * ボタンA: オフセットを減らす
    * ボタンB: X軸とY軸の切り替え
    * ボタンC: オフセットを増やす
    * ボタンB長押し: 調整モードの終了



## CoreS3のボタン操作
CoreS3はCore2のBtnA、B、Cの部分がカメラやマイクに変わってしまったためボタンの扱いが変わりました。<br>
画面を縦に3分割して左：BtnA、中央：BtnB、右：BtnCとなっています。

# 必要なライブラリ（動作確認済バージョン）
※ 最新の情報はplatformio.iniを確認してください。最新で動かない場合はライブラリのバージョンを合わせてみてください。
- [M5Unified](https://github.com/m5stack/M5Unified) v0.1.6で確認
- [M5Stack-Avatar](https://github.com/meganetaaan/m5stack-avatar) v0.8.2で確認<br>v0.7.4以前はM5Unifiedに対応していないのでビルド時にM5の二重定義エラーが出ます。
- [ServoEasing](https://github.com/ArminJo/ServoEasing) v3.1.0で確認
- [ESP32Servo](https://github.com/madhephaestus/ESP32Servo) v0.13.0で確認<br>CoreS3はv0.13.0じゃないと動かない場合があります。

ArduinoIDEでの詳しいライブラリの追加方法は下記のブログを参照してください。（日本語）

[ｽﾀｯｸﾁｬﾝ M5GoBottom版のファームウェアについて | M5Stack沼人の日記](https://raspberrypi.mongonta.com/softwares-for-stackchan/)


# ビルド方法
　v0.1はArduinoIDEでしたが、現在はPlatformIOでのビルドを想定しています。

# ｽﾀｯｸﾁｬﾝについて
ｽﾀｯｸﾁｬﾝは[ししかわさん](https://github.com/meganetaaan)が公開しているオープンソースのプロジェクトです。

https://github.com/meganetaaan/stack-chan

# author
 Takao Akaki

# LICENSE
 MIT