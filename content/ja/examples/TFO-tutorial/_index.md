---
title: "TrackFollowingObject チュートリアル"
linktitle: "TrackFollowingObject チュートリアル"
weight: 1
---

##  ■ 目次

{{% indent %}}

[概要](#overview)

[サンプル路線と車両データ](#sampleroute)

[サンプル路線のファイルリスト](#filelist)

[TFOを使うには](#howtousetfo)

[TFOの特性](#tfo-characteristic)

[1.最初の課題](#Firsttheme)

[個々の構成](#Individual-configuration-of-XML)

[Definitionセクション](#Definition-section)

{{% indent %}}

[AppearanceTimeサブセクション](#AppearanceTime-subsection)

[AppearanceStartPositionサブセクション](#AppearanceStartPosition-subsection)

[AppearanceEndPositionサブセクション](#AppearanceEndPosition-subsection)

[LeaveTimeサブセクション](#LeaveTime-subsection)

{{% /indent %}}

[Trainセクション](#Train-subsection)

{{% indent %}}

[Directoryサブセクション](#Directory-subsection)

{{% /indent %}}

[Stopsセクション](#Stops-section)

{{% indent %}}

[Decelerateサブセクション](#Decelerate-subsection)

[StopPositionサブセクション](#StopPosition-subsection)

[Doorsサブセクション](#Doors-subsection)

[StopTimeサブセクション](#StopTime-subsection)

[Accelerateサブセクション](#Accelerate-subsection)

[TargetSpeedサブセクション](#TargetSpeed-subsection)

[Directionサブセクション](#Direction-subsection)

[Railサブセクション](#Rail-subsection)

{{% /indent %}}

[Rail番号の制限について](#RailX-number-restriction)

[最初のStopサブセクション](#first-Stop-subsection)

[TFO列車を停車させる](#Stop-TFO-train)

[終着駅から始発駅を折り返し運転するには](#To-return-from-the-departure-station)

[数駅を各駅停車で動かすには](#How-to-stops-at-each-station)

[単線で行き違いをさせるには](#How-to-cross-the-oncoming-train)

[線路を付け替えるには](#How-to-change-to-the-other-rail)

[追い越しさせるには](#How-to-overtaking)

[対向列車同士で追い越させる(複数のTFOオブジェクトを設置する)には](#How-to-overtaking-oncoming-trains)

[ポイントを渡らせるには](#moving-on-points)

[複雑な転線をさせるには](#How-to-do-more-complex-points)

[増結して2つの列車を1つの列車にして動かすには](#combine-two-or-more-TFO-trains)

[分割して別編成にし、其々発車させるには](#separate-a-train)

[走行中の速度を変化させるには、走行中にRailを変えるには](#change-rail-and-speed)

[Pointsサブセクション](#Points)

{{% indent %}}

[Pointサブセクション](#Point)

[Decelerateサブセクション](#point-Decelerate)

[Positionサブセクション](#point-Position)

[Accelerateサブセクション](#point-Accelerate)

[PassingSpeedサブセクション](#point-PassingSpeed)

[TargetSpeedサブセクション](#point-TargetSpeed)

[Railサブセクション](#point-Rail)

{{% /indent %}}

[PassingSpeed、TargetSpeedの各サブセクションの関係について](#point-PassingSpeed-TargetSpeed)

[PointでRailIndexを切り替える際の注意点](#point-RailIndex)

{{% /indent %}}

---

## <a name="overview"></a> ■ 概要

TFO(TrackFollowingObject)とは「自由にRail上を動かせるアニメーテッドオブジェクト」です。

Animatedにプログラマブルな動きを加えることで、鉄道車両のみならず、バス、自動車、飛行機やエレベータなど、様々なものを動かすことができるようになります。

Extensions.cfgを含む最低限のTrain.datなどの一式がある車両を用いれば、列車の編成を動かすことも出来ます。

本チュートリアルでは同梱のパブリックドメインの103系を用い、列車の編成を動かす方法を学んでいきます。

また、本チュートリアルはOpenBVE1.6.0.0から1.7.1.3までを対象にしていますが、1.7.1.4以降に対応する新機能の走行中の線路の付け替え、及び速度の変更も解説しています。

###  <a name="sampleroute"></a>サンプル路線と車両データ

{{% indent %}}

本チュートリアルで説明する路線データはすべてサンプルを用意しています。
サンプル路線と国鉄103系電車のアウタービューはいずれもパブリックドメインで、OpenBVEパッケージフォーマット形式でダウンロードできます。

{{% indent %}}

ルートデータ

<http://openbve-project.net/files/TFODemo_Routes.zip>

国鉄103系電車のアウタービュー

<http://openbve-project.net/files/TFODemo-Series103.zip>

{{% /indent %}}

###  <a name="filelist"></a>サンプル路線のファイルリスト

{{% table %}}

| ルートファイル名                    | 概要                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| TFODemo_basic.csv                   | 複線で2駅を有し、対向列車が始発駅から終着駅までやってきます。 |
| TFODemo_basic2.csv                  | 複線で2駅を有し、対向列車が始発駅から終着駅まで往復します。  |
| TFODemo_three-stations.csv          | 複線で3駅を有し、対向列車が各駅停車で駅間を往復します。      |
| TFODemo_single-track-switching.csv  | 中間駅まで走行するかジャンプさせると、単線で行き違いをします。 |
| TFODemo_single-track-switching2.csv | 中間駅まで走行するかジャンプさせると、単線で行き違いをします。<br />対向列車は中間駅で線路を付け替え、始発駅の下り線に停車します。 |
| TFODemo_passings.csv                | 中間駅まで走行するかジャンプさせると、停車時間中に後ろから追い越されます。 |
| TFODemo_passings2.csv               | 中間駅まで走行するかジャンプさせると、停車時間中に後ろから追い越されます。上り線も同様に、待避線に停車中に追い越されます。 |
| TFODemo_passings3.csv               | 中間駅まで走行するかジャンプさせると、停車時間中に後ろから追い越されます。上り線も同様に、待避線に停車中に追い越されます。<br />追い越された列車は、中間駅で線路を付け替え、始発駅の下り線に停車します。 |
| TFODemo_N-style.csv                 | N字型に走行してポイントを転線し、待避線から始発駅の上りホームにやってきます。<br />見やすいようにポイント部分にPOIを設定しています。 |
| TFODemo_Train-addition.csv          | 2M2Tの列車が終着駅からやってきて、側線にいる2M2Tと増結し、4M4Tとなって終着駅に向かいます。 |
| TFODemo_Train-disconnection.csv     | 4M4Tの列車が終着駅からやってきて、終着駅側の2M2Tが側線に退避し、始発駅側の2M2Tが終着駅に向かいます。 |
| TFODemo_points.csv                  | 今後新しく追加されるPointsセクションの説明に用います。<br />始発駅からポイントを転線し、終着駅に向かいますが、実体レールのみを用いて走行中に線路を切り替え、またポイント前後で減速と加速をします。 |

{{% /table %}}

{{% /indent %}}

## <a name="howtousetfo"></a> ■ TFOを使うには

TFOを使うためには、動かしたいオブジェクトが必要です。今回は列車を動かすので、同梱のアウタービュー付きのパブリックドメインの103系を用います。

1.車両オブジェクトを用意する

自分で用意するためには列車として組成しなければならないので、最低限の情報を持ったTrain.datと各車両のcsvもしくはAnimated、Extensions.cfgが必要です。
Train.datは編成のMT比と両数が書かれている事、また重量などはシミュレーションされるため最低限エディタによって生成されるデフォルト値で記述されていなければなりません。軽すぎると脱線する恐れがあります。
動作はTFOが行うため、TrainEditor、もしくはTrainEditor2でデフォルト値で設定されているもので十分です。
此等のツールで少なくともMT比と両数を設定し、デフォルト値で出力します。

運転台パネルを除く(含んでいても使われない)、モーターの加減速音が含まれた列車は、速度に応じて鳴らすことが出来ます。

TFOはanimatedに対応し、保安装置を除く全ての命令を解釈します。LeftDoorsやReverserNotchなどのパラメータを解釈します。
例えばTFOで列車の走行方向が変わると、ReverserNotchも連動します。それによりヘッドライトとテールライトも切り替えられます。
速度に応じたアニメーションも可能です。
停車した時にドアを開け閉めさせることも出来ます。
同梱の103系は速度に応じて車輪が回転し、ドアの開け閉め、ヘッドライトとテールライトが連動します。

2.XMLファイルを用意する

​TFOの動作を記述したXMLファイルを用意します。例えば'TFODemo_basic.XML'とします。

3.路線データに読み込ませる

路線のcsvファイルにて

Route.TfoXML(TFO_XML/TFOXML-basic.XML)

などと記述することで、XMLファイルが読み込まれます。

OpenBVE1.7.1.1以降では、TFO用の列車は'Route以下にあるTFO.XMLファイルのフォルダからの相対パス'もしくは'Trainフォルダからの相対パス'を検索し、読み込みます。

それ以前のバージョン、1.6.0から1.7.0.4までは、'Route以下にあるTFO.XMLファイルのフォルダからの相対パス'から検索し、読み込みます。

以上の手順を踏めば、TFO列車が動かせます。

##  <a name="tfo-characteristic"></a> ■ TFOの特性

TFOは加減速の数値に関わらず、指定されていれば自動的に目的のStopPositionに合わせて加減速して停止します。
加減速の数値に見合う分の距離程が足りない場合でも、速度を自動的に調節して適切なStopPositionに停止します。
指定された時速に到達していなくても、上記の法則に従って自動的に減速し、適切なStopPositionに停止します。

これらのことからリアルさを求める場合には適切な加減速度、そして必要な距離程を取る必要があります。

## <a name="Firsttheme"></a> ■ 最初の課題

最初は複線で、終着駅から始発駅へ移動し、時間が来たら消滅するTFO列車を作ってみましょう。

このTFOを用いたサンプル路線は'TFODemo_basic.csv'です。

このサンプル路線は距離程100mに始発駅、400mに終着駅を持つ複線の単純な路線です。

自線はRail0、複線はRail4です。

該当するTFO.XMLは'TFO_XML/TFOXML-basic.XML'です。

さっそくTFOXML-basic.XMLを見てみましょう。

{{% code %}} 
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>400</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}} 

## <a name="Individual-configuration-of-XML"></a> ■ 個々の構成

XMLファイルなので、ヘッダーを記述します。

```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
```

セクションは大雑把に分けると

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
	  定義 
    </Definition>	
    <Train>	
      動かす車両の指定
    </Train>	
    <Stops>	
      <Stop>
        コマンド
      </Stop>	
      <Stop>	
		コマンド
	  </Stop>	
      <Stop>	
		コマンド
      </Stop>
      ・
      ・
      ・
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	

```
{{% /code %}}

のようになっています。

個々のセクションを見ていきましょう。

## <a name="Definition-section"></a> ■ Definitionセクション

Definitionセクションでは、TFOの基本設定を行います。
Definitionセクション内にサブセクションがあり、ここで挙動を定義します。

{{% indent %}}

#### <a name="AppearanceTime-subsection"></a>AppearanceTimeサブセクション

AppearanceTimeは、TFOオブジェクトが路線に出現する時間を決めます。
お昼の12時丁度の場合は12.0000となります。
18時30分46秒の場合は18.3046です。

#### <a name="AppearanceStartPosition-subsection"></a>AppearanceStartPositionサブセクション

#### <a name="AppearanceEndPosition-subsection"></a>AppearanceEndPositionサブセクション

AppearanceStartPositionとAppearanceEndPositionは、'自車両'がどの距離程の範囲にいる時にTFOオブジェクトを出現させるかを決めるものです。

例えば運転している車両が距離程が1000から2000までにいる時に、該当するTFOオブジェクトを出現させる、という条件式を作ることが出来ます。

AppearanceTimeを組み合わせると、出現時間、出現距離程を条件式とし、TFOオブジェクトを出現させることが出来ます。これにより追い越しや単線の行き違いなどをさせることが出来ます。

#### <a name="LeaveTime-subsection"></a>LeaveTimeサブセクション

設置したTFOオブジェクトが、ここで指定した時間を経過した後に3D空間から外され、オブジェクトが消滅するまでの時間を指定します。
10分後にTFOオブジェクトを3D空間から離脱させたい場合、00.1000と指定します。

{{% /indent %}}

## <a name="Train-subsection"></a> ■ Trainセクション

TrainセクションではTFOオブジェクトを指定します。Trainとありますが、他のTFO列車以外のアニメーテッドオブジェクトもここで指定します。

{{% indent %}}

#### <a name="Directory-subsection"></a>Directoryサブセクション

Directoryサブセクションで、アニメーテッドオブジェクトやTFO列車を指定することが出来ます。
アニメーテッドオブジェクトやcsvなどオブジェクトはファイル名まで、TFO列車は列車一式が揃っているフォルダを指定します。
例:

TFO列車								<Directory>Series-103</Directory>
アニメーテッドオブジェクト	<Directory>animated/test.animated</Directory>
csvオブジェクト	<Directory>a\b\test.csv</Directory>

以上でTFO列車の定義が済みました。
ここからいよいよTFO列車を動かします。

{{% /indent %}}

## <a name="Stops-section"></a> ■ Stopsセクション

TFOオブジェクトを動かす命令はStopsセクションにあるStopサブセクションを複数記述することにより行います。

Stopサブセクションはパートごとに命令が分かれています。
止まるパートではTFOオブジェクトを止まらせるために用いる命令、動かすパートではTFOオブジェクトを動かすために用いる命令です。

{{% table %}}
| 止まるパート                           | 動かすパート                                   |
| -------------------------------------- | ---------------------------------------------- |
| Decelerate,StopPosition,Doors,StopTime | StopTime,Accelerate,TargetSpeed,Direction,Rail |
{{% /table %}}

今回のサンプルは終着駅から始発駅に動かす命令なので、まずは設置して動かす命令を記述することになります。
ひとまず今後の参照のためにも、全ての命令を説明します。

{{% indent %}}

#### <a name="Decelerate-subsection"></a>Decelerateサブセクション

Decelerateサブセクションは、一つ前のStopから引き継いだ速度から減速させる場合の減速度[km/h/s]を指定します。

#### <a name="StopPosition-subsection"></a>StopPositionサブセクション

StopPositionサブセクションは、TFOオブジェクトが停車するゲーム内距離程[m]を指定します。
また、出現位置を指定する際にも用いられます。

#### <a name="Doors-subsection"></a>Doorsサブセクション

Doorsサブセクションは、停車した時点でドアを開け閉めするかを指定します。-1かL:左、0かN:開けない、1かR:右、B:両側を開ける、を指定します。

#### <a name="StopTime-subsection"></a>StopTimeサブセクション

StopTimeサブセクションは、配置後、または停車後の停車時間[hh.mmss]を指定します。
00.0000だと配置、または停車直後に発車します。

例えば1分45秒停車させたい場合は00.0145と指定します。

#### <a name="Accelerate-subsection"></a>Accelerateサブセクション

Accelerateサブセクションは、動かし始めた列車の加速度[km/h/s]を指定します。

#### <a name="TargetSpeed-subsection"></a>TargetSpeedサブセクション

TargetSpeedサブセクションは、加減速後の速度を指定します。

#### <a name="Direction-subsection"></a>Directionサブセクション

Directionサブセクションは、動かしたいTFOオブジェクトの方向を指定します。1が進行方向、-1が後ろ方向です。

#### <a name="Rail-subsection"></a>Railサブセクション

Railサブセクションは、走行させたいレール番号を指定します。

{{% notice %}}

#### <a name="RailX-number-restriction"></a>Rail番号の制限について

BVE2/4向けの路線を変更する場合、BVE2/4はRail15までしか用いることが出来ませんでした。<br/>OpenBVE1.4.3以降は、Rail番号に制限はなくなりました。

{{% /notice %}}

BVE2/4向けの路線にTFOを追加したい場合、上述の通りOpenBVE1.4.3以降ではレール番号の制限は撤廃されているため、15番より大きい番号を指定することが出来ます。
これを利用し、同じ座標にCreatemeshbuilderのみを記述したcsvオブジェクト、即ち透明なオブジェクトを用意し、それをレールにすることで透明なレールを作り、走行させたいルートに上書きさせることで簡単に追加することが出来ます。

あらたに路線を作る場合でも、実体レールでポイントなどを作り、透明レールを走行させるほうが効率的と言えると思います。

ここで最初の課題のDefinitionセクションを見てみましょう。

{{% code %}}
```
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>400</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>
```
{{% /code %}}

TFO列車の出現時間であるApperaceTimeは12.0000、即ち12時丁度です。
路線データの開始時刻も12時丁度なので、ゲーム開始直後に表示する、ということになります。
AppearanceStartPositionとAppearanceEndPositionは自分の列車が何処の距離程の間にいるかのトリガー条件ですが、0から400mと全距離程を取ることで必ず出現するように設定してます。
LeaveTimeは00.0200、即ち表示されてから2分経ったら3D空間から消えるということになります。

{{% /indent %}}

## <a name="first-Stop-subsection"></a> ■ 最初のStopサブセクション

改めて最初の課題の最初のStopセクションの命令を見てみましょう。

{{% code %}}
```xml
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>
```
{{% /code %}}

此等が最初に列車を配置して、動かすための命令セットになります。

最初は停止した状態から始まります。また最初のStopセクション故に速度は引き継ぎませんから、Decelerateは設定する必要がありませんので0.00です。
StopPositionはTFO列車を設置する距離程です。ここでは400mの距離程に設置します。
TFOが列車の場合、先頭車がこの距離程に設置され、以後手前に向かってExtensions.cfgに従い編成が組成されます。Doorsは設置時点で開け閉めする場合指定します。ここでは進行方向右側のドアを開け閉めするよう1を指定しています。
それにも関わらず停車時間は0秒、00.0000なので、ドアは開け閉めするだけですぐに発車します。
ドアを開け閉めしてすぐに発車したら、Accelerateで指定した加速度1.71で加速します。
加速したらTargetSpeedで指定している60km/h迄加速します。
TFO列車はDirectionの-1に従い、後ろ方向に向かって動きます。
走行させたいレール番号は4です。

## <a name="Stop-TFO-train"></a> ■ TFO列車を停車させる

これでTFO列車は動き始めます。次の命令で、TFO列車を始発駅で停車させます。

{{% code %}}
```xml
      <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Decelerateは「一つ前のStopから引き継いだ速度から減速させる場合の減速度」なので、一つ前は60km/hでしたのでそこから減速度1.65で減速を開始します。
StopPositionは始発駅の停止位置目標である距離程100mです。
DoorsをRに指定していますので、停車後は進行方向右側のドアを開きます。
StopTimeは00.0030が指定されていますので、30秒停車します。
今回は始発駅まで来たら消滅するまで何もしませんので、Accelerateは加速する必要がないので0です。
TargetSpeedは同じく加速させる必要がないので0です。
Directionはこれ以上動かさないので必要がないのですが、とりあえず来たときと同じ方向である-1を指定しています。
走行するレールは引き続き4としています。

最初の課題はこれで終了です。
これでとりあえず終着駅から始発駅まで、TFO列車を動かすことができるようになりました。

次は折り返し運転をしてみましょう。

## <a name="To-return-from-the-departure-station"></a> ■ 終着駅から始発駅を折り返し運転するには

今回のサンプルは路線データは'TFODemo_basic2.csv'、TFO.XMLは'TFOXML-basic2.XML'です。
路線データは同じで、Route.TfoXMLでTFOXML-basic2.XMLを指定する点だけが異なります。

TFOXML-basic2.XMLを見てみましょう。

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>400</AppearanceEndPosition>
        	<LeaveTime>00.0230</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--始発駅に停車し折返し発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

Definitionセクション、Trainセクション、最初のStopセクション(設置と動き出し用)は最初の課題と同じです。
今回新たに動作させるのは、始発駅での折り返しと、終着駅での停車です。

早速始発駅での折り返しの部分を見てみましょう。

{{% code %}}
```xml
      <!--始発駅に停車し折返し発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

終着駅から60km/hで始発駅にやってきたTFO列車はDecelerateにより減速度1.65で減速を開始します。
停止位置はStopPositionである距離程100mの位置です。
停止したら進行方向右側をDoorsのRによって開きます。
StopTimeは0.0030なので、30秒停車します。

ここまでの処理は最初の課題と全く同じです。
ここからいよいよ発車させる動作を入力します。

動かすための加速度をAccelerateで指定し、加速度1.71で加速します。
加速後のTargetSpeedは60なので60km/hです。
動かす方向はDirectionが1なので、進行方向です。
走行させるレールはRailが4を指定しているので、Rail4を走行します。

これで始発駅で停車させ、ドア扱いをして折り返して発車します。
つぎに終着駅で再び止めるコマンドを入力します。

{{% code %}}
```xml
      <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

終着駅で止めるために、60km/hからDecelerateで減速度1.65で減速します。
StopPositionは400で、終着駅の停止位置目標の位置です。
DoorsはRとし、停車したら右側のドアを開けます。
停車時間はStopTimeにより30秒とし、時間が経過したらドアを閉めます。
もうこれ以上動かすことはありませんから、Accelerateは0です。
同じく動かすことはありませんので、TargetSpeedも0km/hです。
動かす方向は動かさないので特に変える必要がありませんので、Directionは1のままです。
同じく動かす事はありませんから、動かすRailは4のままです。

これであとはLeaveTimeが経過した後、3D空間から消えます。

## <a name="How-to-stops-at-each-station"></a> ■ 数駅を各駅停車で動かすには

今回のサンプルは路線データは'TFODemo_three-stations.csv'、TFO.XMLは'TFODemo_three-stations.XML'です。
路線データは600m位置に中間駅、1100m地点に終着駅があり、複線の単純な路線です。

この3駅を、終着駅から中間駅、始発駅に向かって複線の線路を各駅停車で動かします。
さっそくXMLファイルを見てみましょう。

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>1100</AppearanceEndPosition>
        	<LeaveTime>00.0400</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--中間駅に停車し、ドア扱いをして発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>600</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

まずはDefinitionセクションを見てみます。

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>1100</AppearanceEndPosition>
        	<LeaveTime>00.0500</LeaveTime>
    </Definition>	
```
{{% /code %}}

この路線は12時丁度に始まります。
AppearanceTimeは12時丁度なので、路線の開始と共に設置されます。
AppearanceStartPositionとAppearanceEndPositionは其々0と1100で、路線の全ての区間を指定することで必ず出現するようになっています。
LeaveTimeは00.0500なので、5分後に消滅します。

Trainセクションは車両指定なので割愛します。

{{% code %}}
```XML
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

最初の配置なので、Decelerateで引き継いで減速させることはありませんので、0を指定します。
StopPositionは終着駅の停止位置目標である距離程1100を指定します。
設置直後にドア扱いをするので、1を指定して右側のドアを開きます。
ドアを開けたら停車させるため、StopTimeを0.0030として30秒停車します。
TFO列車を動かすため、Accelerateで加速度1.71で加速させます。
加速させたらTargetSpeedで60を指定し、60km/h迄加速させます。
TFO列車は終着駅から始発駅方向に動かすため、Drirectionは-1を指定します。
走行させるレールはRailを4に指定し、Rail4を走行させます。

次に中間駅に停車させ、ドア扱いをさせ、再び始発駅方向に発車させます。

{{% code %}}
```XML
      <!--中間駅に停車し、ドア扱いをして発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>600</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

60km/h迄加速しているので、それを引き継ぎ、Decelerateを1.65で減速させます。
StopPositionは中間駅の距離程600mの停止位置目標です。
ドア扱いをして、DoorsをRにして右側を開きます。
停車時間はStopTimeで00.0030として30秒停車させます。
発車後はAccelerate1.71で加速させます。
TargetSpeedは60とし、60km/h迄加速させます。
走らせる方向は始発駅の方向なので、Directionを-1にします。
走行させるレールはRailを4とし、Rail4を走らせます。

中間駅の停車と発車を終えたので、最後に始発駅に停車させます。

{{% code %}}
```XML
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

次は単線で行き違いをさせてみましょう。

## <a name="How-to-cross-the-oncoming-train"></a> ■ 単線で行き違いをさせるには

今回使用するサンプルルートは'TFODemo_single-track-switching.csv'です。
対応するTFO.XMLは'TFODemo_single-track-switching.XML'です。
この路線で中間駅へ運転するか、もしくは駅ジャンプで選択をすると、TFOの対向列車が行き違いしていきます。

ここからはいよいよ本格的になります。
単線で行き違いをさせる場合は自線から分岐したレールを配置する必要があるのはご存知のとおりです。

![TFODemo_single-track-switching_route-base](/images/TFOTutorial/JP/TFODemo_single-track-switching_route-base.jpg)

今回はこのような配置の線路を用意しました。  
自線はここを走行します。
![TFODemo_single-track-switching_route-base-rail0](/images/TFOTutorial/JP/TFODemo_single-track-switching_route-rail0.jpg)

線路を描画するために、Rail1とRail4を使用し、このように用いました。
![TFODemo_single-track-switching_route-base-rail1&4](/images/TFOTutorial/JP/TFODemo_single-track-switching_route-rail1&4.jpg)

先に記した通り、OpenBVE１.４.３からRail番号の制限が無くなっているためそれを利用して、Rail16以上の番号にCreateMeshBuilderのみを記述したCSVオブジェクトを用意し、透明なレールを従来のレールの上に配置することにより、走行させることができるようになります。
勿論今までのサンプルのように、移動させる全ての経路が繋がっているならば、実体のレールを走行させることも出来ます。

今回用意した透明なレールはこのように走るようにしました。![TFODemo_single-track-switching_route-base-rail20](/images/TFOTutorial/JP/TFODemo_single-track-switching_route-rail20.jpg)

ここで留意することは、TFOは「動くアニメーテッドオブジェクト」であり、「同じ線路を共有する列車同士ではない」ということです。

それを利用して、行き違い列車のTFOは「中間駅に到着している間のみ」動かします。
ここで活躍するのが
AppearanceTime
AppearanceStartPosition
AppearanceEndPosition
です。此等を用い、中間駅に止まっている時の時間と距離程を指定し、条件が一致したときのみ発動すれば良いのです。

ここまでお話した所で、路線データを見て下さい。
TFOを動かす中間駅のダイヤと距離程は以下のようになっています。

{{% code %}}
```
850
	.Sta(行き違い駅; 12.0500; 12.0800; ; 1; 1; ; ; ; 5; ;)
1050	
	.Stop(1,1,1)
```
{{% /code %}}

停止位置目標は距離程1050の位置にあります。
列車が停止位置目標にあっていないと、列車が前後のポイントや接触限界を超えてしまっている可能性があります。
そのためAppearanceStartPositionとAppearanceEndPositionの距離程はそれに引っかからない条件を設定する必要があります。

そしてダイヤを見ていただくと、列車は中間駅に12時05分に到着し、12時08分に発車します。
その時間内にTFO列車がやってきて、駅に停車しなければなりません。
それらを考慮してTFOを動かすコマンドを指定したのが今回のサンプルとなります。

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.04</AppearanceTime>
        	<AppearanceStartPosition>1040</AppearanceStartPosition>
        	<AppearanceEndPosition>1060</AppearanceEndPosition>
        	<LeaveTime>00.0430</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
      <!--中間駅に停車し、ドア扱いをして発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

今回から本格的に役立つこととなるDefinitionセクションをお話します。

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.04</AppearanceTime>
        	<AppearanceStartPosition>1040</AppearanceStartPosition>
        	<AppearanceEndPosition>1060</AppearanceEndPosition>
        	<LeaveTime>00.0430</LeaveTime>
    </Definition>	
```
{{% /code %}}

再び触れますが、TFOを動かす中間駅のダイヤと距離程は以下のようになっています。

{{% code %}}
```
850
	.Sta(行き違い駅; 12.0500; 12.0800; ; 1; 1; ; ; ; 5; ;)
1050	
	.Stop(1,1,1)
```
{{% /code %}}

停止位置目標は距離程1050の位置にあります。
つまり自列車は12.0500までに到着する前に、TFO列車の条件を整えておく必要があります。
そのため今回はApperanceTimeを自列車の駅到着の1分前に定めました。

AppearanceStartPositionとAppearanceEndPositionは、中間駅の停止位置目標に自車両が前後10m以内の誤差に入った時に作動するよう、1040と1060に指定しました。

Leavetimeは4分30秒後に消失するようにしました。

つぎにトリガーが発動した時に移動させるTFO列車の最初の命令を見てみましょう。

{{% code %}}
```XML
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

最初に設置する際、以前のStopからの速度を引き継ぐ必要がないので、Decelerateは0です。
StopPositionは今回は間に合ったので駅からとし、2100mからにしました。
Doorsはドア扱いをせずにいきなり任意の速度に持って行きたかったので、扱いはありません。
StopTimeも待っている時間はないので0です。
Accelerateはなんと10です。これは設置した時にいきなり任意の速度で走行させたい、という場合に、加速度を大きく設定することで回避するテクニックです。

Directionは-1で、走行するレールは透明なRail20です。

これで自列車が駅到着、もしくはジャンプした時にTFO列車が反応するようにセットできました。

つぎはTFO列車が行き違い駅に停車して発車していくまでを見てみましょう。

{{% code %}}
```XML
      <!--中間駅に停車し、ドア扱いをして発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

透明な線路を走る以外は、特に今までと変わったところがないことに気づかれると思います。

Decelerateで減速度1.65で減速、距離程1050でStopPositionを指定して停車、ドアはDoorsをLとして左側を開閉、StopTimeは00.0030なので30秒停車、その後Accelerate1.71で加速、TargetSpeedは60km/h、Directionは-1、走行するレールは透明のRail20

というように中間駅に停車し、発車します。
最後に始発駅に停車し、消滅時間まで待つようにします。

{{% code %}}
```XML
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

減速度Decelerate1.65で減速
StopPositionは始発駅の停止位置目標の距離程100m
ドア扱いをしてDoorsをLにして左側を開ける
停車時間はStopTimeで00.0030なので30秒
停車後は再び動かす必要はないのでAccelerateは0
同じく動かす必要はないのでTargetSpeedは0
動かさないのであまり関係はないですが、Directionは1
走行するレールは一応そのままでRailを20に。
ということでコマンドを指定します。

## <a name="How-to-change-to-the-other-rail"></a> ■ 線路を付け替えるには

今回使用するサンプルルートは'TFODemo_single-track-switching2.csv'です。
対応するTFO.XMLは'TFODemo_single-track-switching2.XML'です。

先程は触れませんでしたが、実はこのサンプルルートにはもう一つ、透明レールのRail21を設定しています。
混乱するので説明していませんでしたが、このように配線しています。

![TFODemo_single-track-switching_route-base-rail21](/images/TFOTutorial/JP/TFODemo_single-track-switching_route-rail21.jpg)

線路の付替えとは、重なっているレールとレールの間をTFOオブジェクトを載せ替えることができる機能です。
但し、**線路を付け替える時は必ずTFOを止めて下さい。**

<u>※走行中にも付け替えてもエラーにはなりませんが、必ず一旦列車が止まりますので注意して下さい。</u>

そのため見栄え上でも、一旦停止させたほうが自然と思います。

もう一つ、**線路を付け替える際はTFOの編成全体が、切り替える同じ線路と重なっていて、かつTFOオブジェクトが乗っている状態で切り替えて**下さい。

線路が途中からない等、中途半端な状態で載せ替えると、脱線や車両の消失、OpenBVEが暴走したりするので気をつけて下さい。

ここまでお話した所で、載せ替えるXMLを見てみましょう。

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.04</AppearanceTime>
        	<AppearanceStartPosition>1040</AppearanceStartPosition>
        	<AppearanceEndPosition>1060</AppearanceEndPosition>
        	<LeaveTime>00.0430</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
      <!--中間駅に停車し、ドア扱いをして発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

Definitionセクションは変わりありません。

{{% code %}}
```XML
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

ここで注目していただきたいのはAccelerateです。

なんと加速度10を設定しています。
その理由は、行き違い停車中にやってくる対向列車は、必ずしも駅を出て動き出すとは限らない、ということです。
そのため置いた直後から所定の速度で走行していることもあり得るわけです。
それを実現するために、加速度を高く設定することで最初から走行しているように見せているのです。

さて、どこで線路を載せ替えているかといいますと、中間駅を発車するときです。

{{% code %}}
```xml
      <!--中間駅に停車し、ドア扱いをして発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

ここもほとんど内容は一緒ですが、Railコマンドのみ変えています。

ここでRail20からRail21に変えています。
これだけで線路を載せ替えられます。
Rail21はポイントを分岐して始発駅に到着するようになっていますので、そのように走れる、というわけです。

始発駅に到着する命令も特に変わらないのですが、やはり走行するレールがRail21に切り替わっているため、その点だけ変更を加えています。

{{% code %}}
```XML
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

これを応用して、最初にRail21を走行し、中間駅で停車させてRail20に付け替えることも可能です。

## <a name="How-to-overtaking"></a> ■ 追い越しさせるには

追い越しは、一つ前の例の単線行き違いを応用し、側線に停車している間に時刻と距離程の条件式を設定して後ろから前に走行させることで実現します。

今回使用するサンプルルートは'TFODemo_passings.csv'です。
対応するTFO.XMLは'TFODemo_passings.XML'です。
この路線で中間駅へ運転するか、もしくは駅ジャンプで選択をすると、停車時間中にTFO列車が後ろから追い越していきます。

TFOは線路を共有する列車**<u>ではなく</u>**、動くアニメーテッドオブジェクトです。

そのためTFOが一旦発動した後列車を意図的に発車させると追い越し中にオブジェクトと重なります。
信号システムを完備し、列車を動かさせないようにする必要があります。

今回のサンプルルートはこのようになってます。

![TFODemo_passing_route-base](/images/TFOTutorial/JP/TFODemo_passing_route-base.jpg)

自線はRail0ですが、

![TFODemo_passing_route-rail0](/images/TFOTutorial/JP/TFODemo_passing_route-rail0.jpg)追い越させる透明レールをRail20として配置しました。

![TFODemo_passing_route-rail20](/images/TFOTutorial/JP/TFODemo_passing_route-rail20.jpg)

此等を持ちいて、追い越しをさせます。

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0230</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>100</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
     <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

先に話したとおり、追い越しは単線行き違いの方向を反対にしただけなので、XMLもほとんど変わりありません。
中間駅で追い越されるだけの停車時間を設け、追い越させることが重要です。

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0230</LeaveTime>
    </Definition>	
```
{{% /code %}}

単線追い越しもそうですが、発動条件が重要になります。

{{% code %}}
```
900					
	.Sta(中間駅; 12.1000; 12.1500; ; -1; 1; ; ; ; 100; ;)
1000					
	.Stop(1,1,1)				,;停止位置目標
```
{{% /code %}}

中間駅の停車時間と停止位置目標はこのようになっています。
この停車時間を利用し、追い越し列車のTFOを動かします。
従ってDefinitionのAppearanceTimeは中間駅に定着した時刻で12.1000にしました。
AppearanceStartPositionとAppearanceEndPositionは停止位置+-25mとし、975mと1025mの距離程にしました。
追い越して停車させ、終着駅で車両を消去しないと先行列車ではないTFOと重なってしまうので、到着後速やかに消去するようLeaveTimeを2分30秒に設定しています。

{{% code %}}
```XML
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>100</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

動かし始めのStopコマンドを見てみましょう。

動かし始めなのでDecelerateは0、StopPositionは始発駅の停止位置の100m、ドアは開け閉めせず追い越させるため最初からトップスピードにしたいのでDoorsは0、Accelerateは10にしてます。

TargetSpeedは追い越す列車は高速なので、100km/hとしてます。
Directionは進行方向なので1、走行するレールは追い越し用の透明レールである20にしてます。
追い越し列車は通過駅を移動させますが、TFOはアニメーテッドオブジェクトなので駅を通過、という概念はありません。

追い越させたTFO列車は終着駅で停車させ、ドア扱いをしてから消滅させます。

{{% code %}}
```XML
     <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

Decelerate1.65で減速、StopPositionは終着駅の停止位置目標である2100mの距離程、停車後はDoorsをLにして左側のドア扱いをし、停車時間はStopTimeで00.0030で30秒停車させます。

加速に必要なAccelerateは必要ないので0、TargetSpeedも加速させないので0、Directionは今まで進んできたままとして1、走行させるレールは特に切り替える必要がないので20としてます。

## <a name="How-to-overtaking-oncoming-trains"></a> ■ 対向列車同士で追い越させる(複数のTFOオブジェクトを設置する)には

Route.TfoXMLは、路線データに複数配置できます。
これにより車やバス、TFO列車を複数個路線データ内に配置できます。
今回は、「中間駅に停車して追越待ちをしている間、対向路線の駅に待ち合わせ列車がやってきて停車し、追い越される。自身の列車も追い越された後発車する」

という動作を行うTFO列車を作ります。

今回使用するサンプルルートは'TFODemo_passings2.csv'です。
対応するTFO.XMLは'TFODemo_passings.XML'、'TFODemo_passings2.XML'、'TFODemo_passings3.XML'の3つです。それぞれ

TFODemo_passings.XML:自身を追い越すTFO列車
TFODemo_passings2.XML:上り線の中間駅に停車し、追い越された後始発駅に到着するTFO列車
TFODemo_passings3.XML:上り線の中間駅で、上り列車を追い越すTFO列車

になっています。

今回のサンプルルートはTFODemo_passings.csvに、上記のXMLを読み込ませるTfoXMLを追加したものです。
そのため配線は変わりありません。

![TFODemo_passing_route-base](/images/TFOTutorial/JP/TFODemo_passing_route-base.jpg)

前回に引き続き、自車両が中間駅に到着すると、追い越し用のTFODemo_passings.XMLが発動するのと同時にTFODemo_passings2.XMLにより上り線のホームに退避列車がやってきます。
さらにTFODemo_passings3.XMLも発動し、上り線を通過していきます。

上り線の通過列車はRail4を通ります。

![TFODemo_passing_route-rail4](/images/TFOTutorial/JP/TFODemo_passing_route-rail4.jpg)

上り線の退避列車は透明レールのRail21を通ります。

![TFODemo_passing_route-rail4](/images/TFOTutorial/JP/TFODemo_passing_route-rail21.jpg)

終着駅からのぼり線の追い越し列車が中間駅に到着するには実測して約1分30秒でした。
それを考慮して通過列車の出発時刻を調整します。
まずTFODemo_passings2.XMLのDefinitionセクションを見てみましょう。

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0500</LeaveTime>
    </Definition>	
```
{{% /code %}}

AppearanceTimeは12時10分にしています。
発動するAppearanceStartPositionとAppearanceEndPositionは停止位置+-25mで距離程975-1025mになっています。
LeaveTimeは4分30秒としてます。これは始発駅に追い越される列車が到着して、ドア扱いを30秒してからさらに30秒で消滅する時間を実測して調整しました。

追い越される列車は発車後Accelerate10で加速、約1分30秒で中間駅に到着し、ドア扱いをすることを実測で確認し、その時間に合わせて追い越す列車を設定します。詳しくはFODemo_passings2.XMLの最初のStopサブセクションを参照して下さい。

追い越す列車のDefinitionセクションを見てみましょう。
追い越す列車はTFODemo_passings3.XMLで記述しています。

{{% code %}}
```
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0400</LeaveTime>
    </Definition>	
```
{{% /code %}}

追い越す列車のAppearanceTimeも、退避する列車と同様自列車が駅に到着する時刻と距離程にあわせて12時10分にしています。
AppearanceStartPositionとAppearanceEndPositionも同じく停止位置+-25mで距離程975-1025mになっています。

追い越す列車は追い越される列車が自列車が到着する12時10分から1分30秒、12時11分30秒には追い越す線路が開通しています。
そこに合わせて丁度通過できる時間を割り出し、追い越す列車の発車時間を調整しています。

追い越す列車の最初のStopサブセクションを見てみましょう。
TFODemo_passings3.XMLの最初のStopサブセクションは以下のとおりです。

{{% code %}}
```XML
    <Stops>	
      <!--配置と対向列車で移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0130</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>100</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Decelerateは最初の配置で減速させないため0です。
StopPositionは終着駅の停止位置目標である距離程2100mです。
Doorsは0とし、ドア扱いはしません。
今回重要なのはStopTimeです。
ここで00.0130とし、1分30秒停車させて待機させます。
1分30秒は先行列車が中間駅に到着し、ドア扱いをする時刻です。
その時刻に合わせて終着駅を発車させています。
通過する列車はAccelerate10の加速度で加速し、12時12分25秒に始発駅よりのポイントを通過していくことを実測しました。
すなわち追い越される列車はこの時刻より後ならば追突や衝突せずに発車できることになりますが、現実的な時間にするため待ち時間を変更します。
ここで退避する列車の中間駅までのStopサブセクションを見てみましょう。
TFODemo_passings2.XML

{{% code %}}
```xml
     <!--中間駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1000</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0130</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

前のStopサブセクションを引き継ぎ、60km/hからDecelerate1.65で減速し、StopPosition1000mの距離程で停止します。
ドア扱いをし、DoorsをRにして右側を開きます。

ここでStopTimeを通過する列車が通過し切る時間にあわせ、1分30秒としました。
通過列車が合流ポイントを通過する時刻は実測で12時12分25秒に通過しきるので、それ以降の時刻を設定します。
今回はデモなので、すぐに結果が見えるよう信号システムの時間は考慮せずに停止時間を1分30秒としてすぐに発車するようにしています。

其々の列車は消滅時間までに始発駅に停車させ、ドア扱いが必要ならばさせて、待機させます。
追い越した列車を始発駅で先に消滅させておかないと、TFO同士が重なってしまい綺麗に見えないと思いますので、そこも考慮しておくと良いと思います。
デモでは其々先行列車のLeaveTimeは4分丁度、追い越される列車のLeaveTimeは5分丁度にしています。

## <a name="moving-on-points"></a> ■ ポイントを渡らせるには

ポイントを渡らせるには、実体レールでその経路を全て形成するか、ポイント部分だけ実体レールで敷いた後、透明レールで全ての経路を形成させます。
BVE2/4向けに既に作られている路線ではRailは15までに限られてますから、それ以上の番号を透明レールで付与してincludeで別ファイルから読ませるか、同じファイルの末尾などに透明レールを施設する方法をおすすめします。

OpenBVE1.4.3以降ではRailの番号は撤廃されていますから、好きな番号に割り振りましょう。

今回はTFODemo_passings.csv、及びTFODemo_passings2.csvと同様の線路配置を用い、XMLだけ記述を変更して中間駅からポイントを渡る透明レールに載せ替えて走行させてみます。

TFODemo_passings.csv、TFODemo_passings2.csv、TFODemo_passings3.csvには既に透明レールのRail22を設置しています。
![TFODemo_passing_route-rail4](/images/TFOTutorial/JP/TFODemo_passing_route-rail22.jpg)

Rail22は中間駅の終着駅側の直線が開始された所から始まり、始発駅側にあるシーサスクロッシングを渡って下り線側のホームに到着させる透明レールです。

中間駅を出発させる際、Rail22に載せ替えることで、始発駅の下りホームに停車させることが出来ます。

今回使用するサンプルルートは'TFODemo_passings3.csv'です。
対応するTFO.XMLは'TFODemo_passings.XML'、'TFODemo_passings3.XML'、'TFODemo_passings4.XML'の3つです。それぞれ

TFODemo_passings.XML:自身を追い越すTFO列車
TFODemo_passings3.XML:上り線の中間駅で、上り列車を追い越すTFO列車
TFODemo_passings4.XML:上り線の中間駅に停車し、追い越された後始発駅に到着するTFO列車。中間駅でRail22に載せ替え

になっています。

'TFODemo_passings.XML'、'TFODemo_passings3.XML'については「対向列車同士で追い越させる(複数のTFOオブジェクトを設置する)には」で説明したものと同じですのでここでは割愛します。

今回新たに用意したTFODemo_passings4.XMLですが、基本的にTFODemo_passings2.XMLと同じものです。
TFODemo_passings4.XMLの中間駅で停車させて発車させるまでのStopサブセクションを見てみましょう。

{{% code %}}
```XML
     <!--中間駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1000</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0130</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
```
{{% /code %}}

ここで唯一変更している点はRailで、Rail21を走行していたものをRail22に変更しただけです。
これでRail22を走行し、始発駅のポイントを渡って下りホームに停車させることが出来ます。

以下はTFODemo_passings4.XMLの始発駅に停車させるStopサブセクションです。

{{% code %}}
```xml
     <!--始発駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
```
{{% /code %}}

始発駅に停車させる時にDoorsをLとし、ドア扱いを左に変更しています。
これでポイントを渡る動作を行うことが出来ます。

## <a name="How-to-do-more-complex-points"></a> ■ 複雑な転線をさせるには

機関車の機回しや車庫など、複雑な転線をさせたい場合、予め動かしたい経路に透明レールを設置しておけば、後はRailとDirectionを変える事で好きな経路を走らせることが出来ます。

今回はN字運転で前後に動かしながらポイントを転線させてみます。
今回のサンプルルートの線形はこのようになっています。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-base.jpg)
其々レールは以下の番号を用いています。
![TFODemo_passing_route-rail5](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail0.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail1.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail2.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail3.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail4.jpg)

動きの説明の都合でRail5からお話します。
![TFODemo_passing_route-rail5](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail5.jpg)

Rail5の始発駅側を出発したTFO車両は前方に向かいます。
そしてポイントを渡って始発駅に入るため、Rail22に載せ替えます。
始発駅についたら右側のドアを開けます。

![TFODemo_passing_route-rail22](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail22.jpg)

ドア閉めをしたらRail21を走行し、終着駅に向かいます。

![TFODemo_passing_route-rail21](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail21.jpg)

今回のTFODemo_N-style.XMLの全体を見てみましょう。

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.1000</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>275</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>5</Rail>
      </Stop>	
	
      <!--側線前方に停車し、Rail22に載せ替えて後方に発車始発駅上り線へ-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
	
     <!--始発駅上り線に停車し、Rail21に載せ替え、前方に発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
     <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

個別の要素をみていきましょう。

{{% code %}}
```xml
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.1000</LeaveTime>
    </Definition>	
```
{{% /code %}}

Definitionセクションは特に変わった点はなく、開始直後に自車両がいる距離程で発動させます。
消滅までのLeaveTimeは10分としていますが根拠は特になく、全ての移動を終えてそのまま置いておくための時間を確保しただけです。

{{% code %}}
```xml
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>275</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>5</Rail>
      </Stop>	
```
{{% /code %}}

最初にRail5に配置し、距離程275mから前方に移動させます。
そろそろコマンドの意味もわかって頂けたと思いますので、特徴的な部分のみ説明します。

{{% code %}}
```xml
      <!--側線前方に停車し、Rail22に載せ替えて後方に発車始発駅上り線へ-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
```
{{% /code %}}

減速度1.65で減速し、側線の前方の距離程475mで停止します。ここでDirectionを-1とし、Rail22に載せ替えて25km/h迄加速させます。

{{% code %}}
```xml
     <!--始発駅上り線に停車し、Rail21に載せ替え、前方に発車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

Rail22を走行して始発駅の上り線の距離程100mで停車し、ドア扱いをして右側のドアを開けます。
30秒停車させ、前方へ加速度1.71で加速し、60km/hまで加速させます。
走行するレールはRail21に切り替えます。

{{% code %}}
```XML
     <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

最後に終着駅に停車させ、左側のドアを開け、30秒停車してドアを閉め、消滅まで待機させます。

このようにポイントを渡る経路を用意して置けばRailとDirectionを切り替えるだけで動かすことが出来ます。

## <a name="combine-two-or-more-TFO-trains"></a> ■ 増結して2つの列車を1つの列車にして動かすには

機関車や客車、電車同士を連結をして一つの列車として動かすには「消滅させてすげ替える」方法をとります。
つまり連結するまでは別々の列車で、ドア扱いを終えた直後、其々を消滅させて新しい列車にすげ替え、その列車を動かします。

103系は2M2Tなので、これを4M4Tの8両に増結します。
まずは連結した後の4M4Tの8両編成を用意します。
今回は予め作ってあるので同梱の8両編成(Series-103-4M4T)を使います。

今回のサンプルの配線は「複雑な転線をさせるには」に用いた配線と同じです。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-base.jpg)
併合する列車の片方は、まずRail4を通って始発駅にやってきます。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail4.jpg)
もう片方の列車は予め退避している場所からRail22を通って、併合する列車の前で一旦停止し、連結します。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail22.jpg)
双方の列車のドアを閉め、消滅させて4M4Tの列車にすげ替え、Rail21を通って終着駅に向かいます。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail21.jpg)
終着駅についた8両編成はドア扱いをした後消滅します。

まずはRail4を通ってくる方の列車のXMLを見てみましょう。

TFODemo_Train-addition.XML

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>500</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--始発駅上り線へ停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0115</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

距離程500mから、ゲーム開始後加速度10で加速し、Rail4を通って始発駅に来ます。
ドア扱いをして1分15秒停車します。
もう片方の列車がやってきて連結し、ドア扱いを終えるまで、全ての動作にかかる時間は約1分57秒でした。
その時間に合わせ、ドア扱いをします。StopTimeはその時間で、1分15秒でした。
列車は2分丁度に消滅させ、その時間に4M4Tとすげ替えます。

もう一つの列車の方を見てみましょう。

TFODemo_Train-addition2.XML

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
	
      <!--始発駅上り線で連結、手前で一旦停止-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>180.25</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0005</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>3</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
      <!--始発駅上り線で連結-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0047</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

此方も消滅までの時間は2分丁度です。
ここで重要なのは、連結してドア扱いをして消滅させる時間を全て同じにしなければならないこと、そして停車させる距離程を丁度連結する距離程にピタリと合わせなければならないことです。

何回も繰り返し連結させ、正しく連結して表示させる位置を確認しました。
そして連結した時の距離程は179.25mでした。
ドアを開け閉めする時間は47秒でした。
移動してきて、一旦停止し、5秒停車させた後連結させ、ドア扱いをして閉めるまで、全ての動作の時間は1分57秒前後でした。

消滅時間は双方の列車共に2分に合わせてますのでこれですげ替える準備が整いました。

最後にすげ替えて発車させる4M4Tの方を見てみましょう。

TFODemo_Train-addition3.XML

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0200</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0210</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103-4M4T</Directory>
    </Train>	
	
    <Stops>	
      <!--挿げ替え配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
      <!--終着駅に到着-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

すげ替えて発車させる時間はゲーム開始後2分丁度なので、AppearanceTimeは12時2分丁度です。

4M4Tの列車は、Extensions.cfgで、併合した状態と全く同じ状態を作らなければなりません。
そうしないとすげ替えた時にずれてしまいます。
配置時のStopPositionは、連結した方の列車の距離程にピタリと合わせなければなりません。
連結した時の距離程は179.25mでした。それに合わせ、此方の配置時の距離程に合わせ、StopPositionは179.25mにします。
あとはRail21を走行させ、終着駅に到着したらドア扱いをさせ、消滅させます。

3編成以上の併合も、同じ手順で行います。

## <a name="separate-a-train"></a> ■ 分割して別編成にし、其々発車させるには

列車が到着した後、別々の編成に分割し、其々移動させるには、併合と反対のことをします。
言うのは簡単なのですが、併合と同じように位置合わせが大変です。

今回の路線の配置も、併合に使った路線と同じものを使います。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-base.jpg)
分割する4M4Tの列車は、まずRail4を通ってやってきます。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail4.jpg)
到着してからドア扱いをし、終着駅側の編成はRail22を通って側線に入ります。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail22.jpg)
最後に始発駅側の編成はRail21を通り、終着駅に向かいます。
終着駅に到着したらドア扱いをし、消滅します。
![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail21.jpg)

まずは4M4TのXMLを見てみましょう。

TFODemo_Train-disconnection3.XML

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0111</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103-4M4T</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--終着駅に到着-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

DefinitionのLeaveTimeが00.0111、即ち1分11秒になっています。
この数値が非常に重要です。

この時間は始発駅を出発し、最初のStopサブセクションの各サブセクションの設定に従い距離程1100mから加速度10で加速し、75km/hで次のStopに従い減速度1.65で減速、距離程179.25mで停止させるまでの時間を実測したものです。

4M4Tの列車は到着した直後に消失します。それと同時に8両全て一致させるように2M2Tの2編成を出現させ、直後に其々の編成のドアを開けています。
次に終着駅側の編成のXMLを見てみましょう。

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0111</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>22</Rail>
      </Stop>	
	
      <!--待避線に移動し、停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>3</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

DefinitionのAppearanceTimeが12.0111、12時1分11秒になっています。
ここで思い出していただきたいのですが、先程4M4Tの編成が始発駅に到着するまでの時間が1分11秒かかった、とお話しました。

4M4Tの出発時刻は12時丁度で、1分11秒かかって到着して停止します。
入れ替える時刻はそれに合わせて1分11秒後に置き換えるので、AppearanceTimeが12.0111、12時1分11秒というわけです。

置き換える距離程も非常に重要で、最初のStopサブセクションのStopPositionは179.25となっています。
これは4M4Tが停止する距離程と一致させる必要があり、正確に合わせています。

終着駅側の列車は後はドア扱いを済ませ、Rail22を走行し、待避線に向かいます。

最後に始発駅側の列車を見てみましょう。

TFODemo_Train-disconnection.XML

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0111</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0320</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0100</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
      <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

始発駅側の列車も、DefinitionのAppearanceTimeが12.0111 、12時1分11秒になってます。
最初のStopサブセクションのStopPositionも非常に重要で、ずれないよう距離程を合わせる必要があります。
それがStopPositionの距離程100mになります。
位置合わせと入れ替える時間、これが非常に重要です。

これで其々の2M2Tが、4M4Tが到着した瞬間消滅すると同時に出現させて、入れ替えることが出来ました。

## <a name="change-rail-and-speed"></a> ■ 走行中の速度を変化させるには、走行中にRailを変えるには

今後新たに搭載される機能により、1.6.0.0から1.7.1.3まででは出来なかった"走行中に速度を変化させること"ができるようになります。

また、従来は透明なレールを予めすべてのルートに設置しなければなりませんでしたが、この命令によりそれをせずとも指示に従ってその距離程にあるレールを車両ごとに自動的に切り替えて走行できるようになりました。

今回もN字運転のときに用いた配線を用いますが、実体レールのみで透明レールはありません。

今回のサンプル路線は'TFODemo_points.csv'で、XMLは'TFODemo_points.XML'です。

![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-base.jpg)

Rail4の始発駅を発車した列車は45km/h迄加速します。
しかしポイントを渡るため、手前で25km/hに減速します。

![TFODemo_N-style_route-base](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail4.jpg)

距離程250mから375mに渡ってRail4からRail0につながる、Rail1のポイントを通ります。

![TFODemo_passing_route-rail21](/images/TFOTutorial/JP/TFODemo_N-style_route-Rail1.jpg)

ポイント通過後は再加速し、75km/hで終着駅に向かいます。

今後新たに追加されるサブセクションを説明します。

### <a name="Points"></a>Pointsサブセクション

Pointsサブセクションで、走行中のレールの載せ替えや、速度変更を可能にします。
Stopsサブセクションと同じで、誤認しないよう名前を変えただけです。そのためStopsサブセクション内にPointサブセクションを記述できますし、Pointsサブセクション内にStopサブセクションを記述できます。
またStopsサブセクション内にStopとPointを混在できますし、Pointsサブセクション内にも同様にStopとPointを混在できます。

{{% indent %}}

#### <a name="Point"></a>Pointサブセクション

Pointサブセクション内には以下のサブセクションを記述できます。
※DirectionサブセクションはStopと異なり方向の変換は出来ません。そのためDirectionサブセクションは使えません。

#### <a name="point-Decelerate"></a>Decelerateサブセクション

Pointsサブセクション内にもStopと同様Decelerateで減速度を指定できます。
一つ前のStopもしくはPointサブセクションの速度を引き継ぎ、指定した減速度で減速します。

#### <a name="point-Position"></a>Positionサブセクション

StopPositionと同じものですが、PointなのにStopPositionと記述すると混同する恐れがあるため、Positionと記述することも出来ます。

#### <a name="point-Accelerate"></a>Accelerateサブセクション

Pointsサブセクション内にもStop同様Accelerateで加速度を指定できます。

#### <a name="point-PassingSpeed"></a>PassingSpeedサブセクション

Pointサブセクションで用いる専用のサブセクションです。Position(StopPosition)で記述された距離程時点での時速を記します。ここからTargetSpeedの時速に向かって加減速を行います。

#### <a name="point-TargetSpeed"></a>TargetSpeedサブセクション

TargetSpeedサブセクションは、加減速で意味が変わります。
減速の場合はTargetSpeedで指定した速度からPassingSpeedサブセクションの距離程までに、PassingSpeedの時速まで減速します。
加速の場合はPassingSpeedで指定した速度から、Accelerateサブセクションの加速度でTargetSpeedの時速まで加速します。

#### <a name="point-Rail"></a>Railサブセクション

Railサブセクションは、走行させたいレール番号を指定します。

{{% /indent %}}

## <a name="point-PassingSpeed-TargetSpeed"></a> ■ PassingSpeed、TargetSpeedの各サブセクションの関係について

PassingSpeedとTargetSpeedを同じ値にすると、その距離程において突然その速度に切りかわります。
滑らかに動かすためには加減速度を指定した上で、その速度に到達できる距離を取る必要があります。
PassingSpeedとPositionにより指定された距離程からTargetSpeedに到達する前に、次のPointによりPassingSpeedとPositionに指定された時速に到達できなかった場合、突然後に指定された速度に変わります。
滑らかに動かすためには加減速度に見合う距離を取る必要があります。

## <a name="point-RailIndex"></a> ■ PointでRailIndexを切り替える際の注意点

StopはExtensions.cfgにおける先頭車両の一番先端部分を基準にしています。
Pointでは各TFO車両の車軸が、Pointで指定した距離程を超えた時点で其々の車軸単位で発動します。

そのためStopで特に後ろ向きに動かす場合でRailを切り替える場合は、編成の長さ分を考慮する必要があります。
Pointでは車軸単位なので、ポイントなどでRailを切り替える際に編成を考慮しないとワープする事がありますので、RailEndなどに注意する必要があります。

さっそくTFODemo_points.XMLを見てみましょう。

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.1000</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--配置と移動開始-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>45</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--ポイントまでに減速-->	
      <Point>	
        	<Decelerate>1.65</Decelerate>
        	<Position>225</Position>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>45</TargetSpeed>
        	<PassingSpeed>25</PassingSpeed>
        	<Rail>4</Rail>
      </Point>	
	
      <!--ポイントで走行中に線路を切り替え-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>250</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>1</Rail>
      </Point>	
	
      <!--ポイントで走行中に線路を切り替え-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>375</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
	
      <!--ポイント通過後に75km/hに加速-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>475</Position>
        	<Accelerate>1.71</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>75</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
	
     <!--終着駅に停車-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1600</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>0</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

Definitionセクションについては特に変わったところはありません。

また、最初のStopサブセクションもRail4を進行方向に向かって動かしますが、変わったところはありません。
時速45km/h迄加速します。
次に出てくる初めてのPointサブセクションを見てみましょう。

{{% code %}}
```XML
      <!--ポイントまでに減速-->	
      <Point>	
        	<Decelerate>1.65</Decelerate>
        	<Position>225</Position>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>45</TargetSpeed>
        	<PassingSpeed>25</PassingSpeed>
        	<Rail>4</Rail>
      </Point>	
```
{{% /code %}}

Decelerate1.65で減速します。

Rail1のポイントは

{{% code %}}
````
,;渡り線右から左					
250					
	.Rail	1	;	4.3	;-0.02
	.Railtype 1;	1			
````
{{% /code %}}

のように指定しています。そのためPositionはこの手前までに減速しなければなりません。
今回は225mの距離程までに減速させるようにしました。
既にひとつ前のStopコマンドにより45km/h迄加速してますので、減速の場合のTargetSpeedなので45km/hとしました。
そこから今のPointの距離程までにPassingSpeed25km/hへ減速してきます。

ポイントまでに減速を終えたらポイントに進入しますが、実体レールのため適切な距離程で切り替えなければなりません。
先の通りポイントに使うRail1は距離程250で始まってます。

そこでRailを切り替えるPointサブセクションは下記のようになります。

{{% code %}}
```xml
      <!--ポイントで走行中に線路を切り替え-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>250</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>1</Rail>
      </Point>	
```
{{% /code %}}

すでに減速を終えてますし、加減速をしないのでDecelerate、Accelerateはともに0です。
重要なのはPositionで、この距離程を実体レールのポイントの距離程に合わせる必要があります。
今回は先の通り距離程250mで始まってますから、ここを250とします。
PassingSpeed並びにTargetSpeedは速度変化をさせないため減速後の25km/hを指定しています。
そしてRailをポイントに用いているRail1にします。

これで走行中にRailを付け替えることが出来ます。

次にもう一度移動先のレールに切り替える必要があります。ここではRail0に移動します。

Rail1の一番最後の距離程は下記のようになってます。

{{% code %}}
```
375				
	.Rail	1	;	0
	.Railtype 1;	0		
	.RailEnd	1	;	
```
{{% /code %}}

ポイントを渡り終えたら375mの距離程でRailEnd 1としています。

先の通りレールの載せ替えは距離程が重要なので、375mで載せ替えます。

{{% code %}}
```xml
      <!--ポイントで走行中に線路を切り替え-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>375</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
```
{{% /code %}}

RailEndのある距離程375mをPositionで指定し、加減速はしないのでDecelerateとAccelerateは共に0です。
時速もそのままなのでPassingSpeedとTargetSpeedも25km/hのままです。

そしてRailを0にします。

これでポイントを渡り終えます。

ポイントを渡り終えたり徐行を終えたら再加速すると思いますので、ここでも同様にポイントをすべての車両が渡り終えたら加速させます。

{{% code %}}
```xml
      <!--ポイント通過後に75km/hに加速-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>475</Position>
        	<Accelerate>1.71</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>75</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
```
{{% /code %}}

パブリックドメインの103系は20m4両編成なので今回Positionは475mにしてみました。

加速させるのでAccelerateを1.71にしました。
ここで重要になるのはPassingSpeedとTargetSpeedです。
この2つは加減速の際に役割が変わります。
今回は加速なのでPassingSpeedの距離程からTargetSpeedに向かって、PassingSpeedから加速していきます。

つまり距離程475mから加速度1.71で、時速25km/hから75km/hに向かって加速していく、ということになります。

走行するレールは引き続きRail0です。

そして最後に終着駅に到着します。



以上でチュートリアルは終了です。
このチュートリアルが皆さんのデータ制作の役に立てれば幸いです。

This document is written at 2020 by midnight express ginga81 (ginga81)
This document is public domain.
