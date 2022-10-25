---
title: "Tsukuctf2022 Writeup"
date: 2022-10-25T14:32:27+09:00
draft: false
---

# TsukuCTF 2022 Writeup

このブログを建ててから、あまり時間が取れず何も書けていなかったので、気軽にかける内容として先日出たTsukuCTFのWriteupを書いてみました。

土曜は友人と焼肉を食らう予定があったので、日曜に日付が変わってからの参加となりましたが、1人チームkstmとして出場し、最終的に13位、1人チームでは3位となりました。後から追い上げた割には悪くないかな?



焼肉

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">友人と家で焼肉をした <a  href="https://t.co/IesUwBSQCj">pic.twitter.com/IesUwBSQCj</a></p>— 通行人1 (@passerby1_) <a  href="https://twitter.com/passerby1_/status/1583796883516186626?ref_src=twsrc%5Etfw">October 22, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js"  charset="utf-8"></script> 



## Attack of Tsukushi (easy)

つくしくんはある観光地を調査した際に訪れた駅で写真を撮影した。果たしてこの写真が撮られた駅はどこだろうか？ フラグは駅の郵便番号（ハイフンなし）を入力して下さい
 e.g. 東京駅の場合は郵便番号が100-0005なのでフラグは `TsukuCTF22{1000005}` となります。

![Attack_of_Tsukushi](/images/202210/tsukuctf2022/Attack_of_Tsukushi.jpg)

これはGoogle Lensで一発でした。

JR日田駅 `TsukuCTF22{8770013}`





## Money (easy)

どこ？ フラグは写真が撮影された場所の郵便番号(ハイフンを除く)を入れて下さい。例えば撮影された場所が東京都庁の場合、郵便番号は163-8001なので`TsukuCTF22{1638001}`となります。

![money](/images/202210/tsukuctf2022/money.jpg)

これもGoogle Lensで見たところ、金閣寺でした。たしかに見かけたことある気がする。

`TsukuCTF22{6038361}`





## FlyMeToTheTsukushi (easy)

つくし君は、はるばる飛行機で愛するパートナーのもとへやってきました。
ここはどこの空港かわかりますか？ ` この問題はフラグを10回までしか提出できません。` ` ※フラグの形式はTsukuCTF22{空港名}です。空港の名前だけを入力してください。(hoge空港の場合、hogeのみがフラグになります)`

![airport](/images/202210/tsukuctf2022/airport.jpeg)

並んでいる航空会社、機種、雰囲気で調べるまでもなく、福岡空港だと予想がついたので入力してみたところ、一発でした。~~筑紫国ということでタイトルが半ば答えになっている~~

強いて言うならば、スターフライヤー (黒の機体のやつ) が来ている空港は国内数カ所程度であること、内陸の空港であること、ANAのボンバルディアQ400がいること、JALがA350を飛ばしている空港 ということで絞れると思います。あと、何かの店の名前らしきものが窓に反射して右上に見えるので、それで調べてもすぐ出るでしょう。

`TsukuCTF22{福岡}`





## inuyama082 (easy)

つくし君は愛知県犬山市にデートに来た時の思い出の写真を見返しています。 おいしそうな写真を見つけ、おやつが食べたくなりました。 写真のおやつの名前を教えてください。
 `※フラグの形式はTsukuCTF22{XXXXXXX ver.XXXXXX}です。`

![inuyama082](/images/202210/tsukuctf2022/inuyama082.jpg)

`犬山 抹茶 "ver"` で調べたらそれっぽい店のtwitterアカウントを見つかりました。

https://twitter.com/inuyamayoakeya

HPのメニューページを確認

https://www.yoakeya1916.com/menu-%E3%83%A1%E3%83%8B%E3%83%A5%E3%83%BC/

`TsukuCTF22{和チーズケーキ ver.煎茶パウダー}`





## sky (easy)

帰ってくるあなたが最高のプレゼント。つくし君は電車にガタゴト揺られています。次の停車駅で降りるようなのですが、どこかわかりますか？
`※フラグの形式はTsukuCTF22{次の停車駅}です。公式サイトの表記を採用します(スペースは含めません)。`

![sky](/images/202210/tsukuctf2022/sky.jpg)

どう見ても名鉄2000/2200の特別車、豊橋行ったときに乗りました。

外が暗いのはトンネルなのか単純に夜なのか分かりませんが、仮にトンネルとして、かつ、駅名に「名鉄」とついている可能性が高いと思われる問題条件からすると、名鉄名古屋? かなと? 入れてみると正解。

`TsukuCTF22{名鉄名古屋}`

間違ってたら、窓に反射している画面に注目して、コントラスト変えたりして見てたと思います。





## douro (easy)

旅行中のつくし君は迷子になってしまったようです。うつむいています。送られてきた写真から場所を特定できますか？
 `※フラグの形式はTsukuCTF22{緯度_経度}です。ただし、緯度経度は十進法で小数点以下五桁目を切り捨てたものとします。`

![douro](/images/202210/tsukuctf2022/douro.jpg)

四隅の一番左下にある文字が手掛かりになりそう。

よいばモール? よいはモール? 1990

-> よいほモール でした。

この通りの歩道に同じマークが複数あるため、それだけでは場所が分かりませんが、中央線が点線になっている個所と、写真のように縞? 状になっている場所があるので絞れます (交差点の近くがそうなっている)。写真右手の建物の柱の形、マンホールとの位置関係を見て確定させました。

https://www.google.com/maps/@34.5763729,136.5312642,3a,48.5y,76.69h,83.46t/data=!3m6!1e1!3m4!1s0py8FCJ-PtjpfuVuK6VtTQ!2e0!7i16384!8i8192

34.5763729,136.5312642

最初に試した座標が入らず、Google Mapsでの座標は少しずれていることがあるので、地理院地図で再確認しました。

https://maps.gsi.go.jp/#18/34.576342/136.531407/&base=std&ls=std&disp=1&vs=c1g1j0h0k0l0u0t0z0r0s0m0f1

34.57634_136.53140

`TsukuCTF22{34.5763_136.5313}`

最初「小数点以下5桁目を切り捨て」を取り間違え、下5桁を残して提出し誤答を連発してしまいました。





## station (easy)

つくし君はとある駅で友達を待っています。さて、つくし君はどこの駅にいるでしょうか？ `TsukuCTF22{駅名(漢字、平仮名、英語可)}` 注意 駅名はWebページで公開されている表記を利用してください 「駅」という漢字はFlagに含めないでください 数字が含まれる場合は全て半角英数字にしてください 例えば、六本木一丁目駅が答えなら、TsukuCTF22{六本木1丁目}、TsukuCTF22{ろっぽんぎ1ちょうめ}、TsukuCTF22{Roppongi itchome}が答えになります 提出回数は10回までです。

Mr.Tsukushi waits his friend at a station. Then, where is he? Flag format is `TsukuCTF22{Station Name(Kanji, Hiragana, or English are available)}`. Notice: use name published on some websites as a station name use half-width alphanumeric characters, if numbers are included do NOT include "Station" in submitted flag For example, if "Tokyo Station" is an answer, the flag is "TsukuCTF22{東京}", "TsukuCTF22{とうきょう}", or "TsukuCTF22{Tokyo}" Submissions are limited to 10 times.

![station](/images/202210/tsukuctf2022/station.jpg)

東豊線とあるので札幌市営地下鉄でしょう。東豊線、南北線の案内が見えるので写っているのは東西線のホーム? ホームドアの感じもそれっぽい。この乗換案内が出て違和感が無いのは西11丁目か大通りかバスセンター前の3択、左上に見切れている部分が西11丁目と読める気がするので入れてみたら正解でした。

`TsukuCTF22{西11丁目}`





## Where (easy)

北海道に住んでいるつくしさんは東京旅行に行った際に高層ビルの窓から写真を撮りました。 でも撮影した場所を忘れてしまったようです。この写真が撮影された場所について建物名を教えてあげてください。 フラグはこの建物の開業日(`YYYY/MM/DD`)です。たとえば、東京スカイツリーの開業日は2012年5月22日なので、フラグは`TsukuCTF22{2012/05/22}`となります。

![Where](/images/202210/tsukuctf2022/Where.jpg)

西武、丸井の感じで渋谷だと分かり、目の前のデカい建物はスクランブルスクエアなので、方角的にパルコかな? と、調べてみると、リニューアル日は2019-11-22、開店日は1973-06-14

`TsukuCTF22{1973/06/14}` で、こちらが正解でした。





## Gorgeous Interior Bus (medium)

観光地に来たつくし君は、豪華なバスを見かけたので、それに乗って観光することにしました。 その時、つくし君のお母さんから「どこにいるの？」と連絡が着ましたが、おっちょこちょいなつくし君は、観光地の名前も、乗っているバスの路線も忘れてしまい、とっさに車内の写真を撮って、「ここ」と返信しました。 つくしくんはどこにいるのでしょうか？ つくしくんが写真を撮ったところに最も近い交差点の名前を特定してください。 ` ※フラグの形式はTsukuCTF22{交差点の名前}です。`

![GorgeousInteriorBus](/images/202210/tsukuctf2022/GorgeousInteriorBus.jpg)

モニターの地図の感じが熱海っぽいので、`熱海  バス` で検索。当たっていたので路線図を確認し、次のバス停は「銀座」なので「サンビーチ」バス停との間の区間と判明。

ストリートビューで場所を確認しても、交差点名が書いてある交差点が少ない。東海岸町 とあったので入れてみたらこれが正解でした。

`TsukuCTF22{東海岸町}`





## Bringer_of_happpiness (easy)

つくしくんは荷物を運び終えて休憩してるときに撮った写真。さて撮影場所はどこだろう？
`※フラグの形式はTsukuCTF22{緯度_経度}です。ただし、緯度経度は十進法で小数点以下五桁目を切り捨てたものとします。`

![Bringer_of_happiness](/images/202210/tsukuctf2022/Bringer_of_happiness.jpg)

島原鉄道かなぁ? と、Wikipediaの島原鉄道線の記事を確認。

駅一覧から、駅舎を見つけた。

https://ja.wikipedia.org/wiki/%E5%B3%B6%E5%8E%9F%E6%B8%AF%E9%A7%85

Googleの緯度経度は32.769339, 130.370510

TsukuCTF22{32.7693_130.3705}、これは通らなかった。

地理院地図で再確認

32.769243/130.370513

`TsukuCTF22{32.7692_130.3705}`





## Desk (medium)

つくし君の大好きなお姉さんのデスクを見学させてもらったよ。 さて、このデスクはどこにあるのだろうか?
フラグ形式は写真が撮影された場所の郵便番号(ハイフンを除く)を入れて下さい。例えば撮影された場所が東京都庁の場合、郵便番号は163-8001なので `TsukuCTF22{1638001}` となります。

![Desk](/images/202210/tsukuctf2022/Desk.jpg)

目の前のパンフレット? に写っているのは沖縄。右下のキャラクターをGoogle Lensで検索しても出てこないので、これを調べる。`沖縄 爺さん キャラクター` で検索したら、ゆるキャラグランプリの「なんじぃ」のページがヒットした。沖縄県南城市のキャラクターらしい。

`沖縄県南城市 "デスク"`  で検索すると、画像検索の方にアニメ「白い砂のアクアトープ」関連情報が出てくる。例えば以下ブログ。

https://basukore.com/okinawa-0005/

南城市観光協会にあるらしい。

`TsukuCTF22{9011511}`





## TakaiTakai (easy)

日本の町は美しい。撮影地を答えてください。 フラグはこの建物の開業日(`YYYY/MM/DD`)です。たとえば、東京スカイツリーの開業日は2012年5月22日なので、フラグは`TsukuCTF22{2012/05/22}`となります。 ※10/24 00:21(JST)追記: 竣工日を入力しても正答とすることにしました。

![TakaiTakai](/images/202210/tsukuctf2022/TakaiTakai.jpg)

幾つかの建物の形が特徴的なので使えそう。手前の角が丸まっているビル、三角形のビル、中央の中庭がありそうな奴、右奥の屋根が特徴的な体育館、右奥の上部フロアの色が違うマンションっぽい高層ビル等。

奥に見えるタワーはスカイツリー? 上が妙に太いのは建設中のころの写真?

若干緑が多いので、これを見るのもアリ (都内に緑がまとまっている場所は少ないので、皇居なり代々木公園なり新宿御苑なり目星がつければかなり絞り込める)。

上の階の色が違う右上のマンションをGoogle Lensで検索 -> 写っている写真が見つかり、中目黒アトラスタワーと判明。

中目黒アトラスタワー、隣の中目黒ゲートタウン、体育館 で方角を確定させた。

左下の縁が特徴的なビルは第2ナカヤビル、目的の建物は渋谷ソラスタ。

竣工は2019-03-29

`TsukuCTF22{2019/03/29}`





## PaperJack (easy)

イケメンのつくしくんは訪れている場所の写真をSNSに投稿したところ、ストーカーに特定されてしまった。ストーカー曰く「好きなゲームと新聞がコラボしたときの広告にこの場所が映っていたのを思い出した」とのことだった。
フラグは写真が撮影された場所の郵便番号(ハイフンを除く)を入れて下さい。例えば撮影された場所が東京都庁の場合、郵便番号は163-8001なので `TsukuCTF22{1638001}`となります。

![PaperJack](/images/202210/tsukuctf2022/PaperJack.jpg)

「好きなゲームと新聞がコラボしたときの広告」は、以前話題になったFGOの奴かな? と予想がついたので、FGOのコラボ広告の一覧を確認する。

https://5th.fate-go.jp/

和歌山の広告の道成寺

`TsukuCTF22{6491331}`





## banana (medium)

つくし君は、ある女の子のSNSアカウントを眺めています。 つくし「この場所を特定して僕も同じ場所の同じ構図で写真を撮りたい！」 つくし君の願いを叶えるべく、この場所を特定してあげましょう。 `※フラグの形式はTsukuCTF22{緯度_経度}です。ただし、緯度経度は十進法で小数点以下五桁目を切り捨てたものとします。`

![banana](/images/202210/tsukuctf2022/banana.jpg)

右のバナナをGoogle Lens検索しても出ないが、左の模様を検索すると以下のページが引っかかった。

https://www.lealea-guam-jp.info/feature/wallart/

グアムの「デデド朝市会場のトイレ」のウォールアートらしい。

https://www.google.com/maps/place/%E3%83%87%E3%83%87%E3%83%89%E3%81%AE%E6%9C%9D%E5%B8%82/@13.521005,144.8284318,141m/data=!3m1!1e3!4m9!1m2!2m1!1sDededo,+Guam+market!3m5!1s0x671f82ffe4112181:0xfbc611f8ee998280!8m2!3d13.521005!4d144.8287226!15sChNEZWRlZG8sIEd1YW0gbWFya2V0WhQiEmRlZGVkbyBndWFtIG1hcmtldJIBDmZhcm1lcnNfbWFya2V0mgEjQ2haRFNVaE5NRzluUzBWSlEwRm5TVVJqTW5OZlQxTm5FQUXgAQA

13.52095, 144.82879

`TsukuCTF22{13.5209_144.8287}`





## TsukuCTF Big Fan 1 (easy)

彼はTsukuCTFの大ファンで、TsukuCTFのあらゆるコンテンツを確認しています。
私は彼と一緒にTsukuCTFに参加しようと思っています。しかし、私は彼の実力をあまり知りません。
まずは彼のTwitterのアカウントを特定し、そのアカウントのアカウント作成日を求めてください。
フラグ形式は`TsukuCTF22{YYYY/MM/DD}`です。

He is a big TsukuCTF fan and checks all the content of TsukuCTF.
I am planning to participate in the TsukuCTF with him. But I don't know much about his ability.
First, specify his Twitter account and ask for the date the account was created.
The flag format is `TsukuCTF22{YYYY/MM/DD}`.

![discord](/images/202210/tsukuctf2022/discord.png)

~~TsukuCTFのアカウントのフォロワーは少ないので~~フォロワーを見たら同じアイコンを発見

https://twitter.com/tsukuctf/followers

https://twitter.com/SuperProStalker

以下サービスで登録日を出した。(開発者モードでも確認可能)

https://midnight2d.com/itsukara/

2021/11/29 16:52:58

`TsukuCTF22{2021/11/29}`





## Robot (hard)

つくし君がロボット見学に訪れた施設はどこ？
フラグ形式は `TsukuCTF22{施設名}` となります。施設名の表記は、その施設の英語版ホームページの表記に従います。

![Robot](/images/202210/tsukuctf2022/Robot.jpg)

実験の「験」が簡体字なので中国本土っぽい。

https://ja.wiktionary.org/wiki/%E9%A8%93

バイドゥで  `Base of innovation training for undergraduates` をバイドゥる。

https://www.baidu.com/

https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=Base%20of%20innovation%20training%20for%20undergraduates&fenlei=256&rsv_pq=0xa877b69500037764&rsv_t=c6dfZ1clgPA2vg3ctnDu%2Bkz1HhUe9VhygUrPw3EtB5ASXVCVSo%2BrNdHDEs5w&rqlang=en&rsv_enter=1&rsv_dl=tb&rsv_sug3=49&rsv_sug1=4&rsv_sug7=100&rsv_sug2=0&rsv_btype=i&inputT=14820&rsv_sug4=15266&rsv_jmp=fail

するとこのドキュメントが引っかかった。

https://www.doc88.com/p-901293128651.html

華南理工大学

https://www.scut.edu.cn/new/



`华南理工大学大学生创新能力培养基地` (華南理工大学大学生革新能力培養基地) でバイドゥるが、これのHPは無いっぽいのでフラグで言っている「施設」は大学のことっぽい。

英語版ページを確認し、大学名を入力したら通った。

https://www.scut.edu.cn/en/

`TsukuCTF22{South China University of Technology}`





## Flash (medium, hardware)

つくし君からマイコンボードを借りたら、このマイコンを使って実験を行ったホテルと部屋番号がわかってしまった！！ マイコンのフラッシュメモリから読みだしたデータを渡すので、ホテル名と部屋番号を特定してください。 `※フラグの形式はTsukuCTF22{XXホテル&XXXXXXXXXXXX_部屋番号}です。`

(`Flash.bin` が与えられる)



バイナリなので、とりあえずBzか何かで目grepする。

ほとんどFFで埋まってるので見る個所は少ない。

なんかssidやらdummypswdやらapa-316-2428やらいう文字が見えますね…



`apa 316` でぐぐると以下ページが引っかかる。

No.316 アパホテル&リゾート〈両国駅タワー〉

https://map.apahotel.com/map/460

フラグの形式に文字数ぴったり収まる。。

`TsukuCTF22{アパホテル&リゾート〈両国駅タワー〉_2428}`





## what_time_is_it (hard)

手前の電車は何時何分に発車するでしょうか？ フラグ形式はTsukuCTF22{xx:xx}で、24時間表記です。 提出回数制限: 5回

![what_time_is_it](/images/202210/tsukuctf2022/what_time_is_it.jpeg)

正面に写っている車両は、JR四国の2600系で、少しググるとこの編成は高松運転所の所属であること、主に特急うずしおで使われていることがわかる。

他の車両などから、これが撮影されたのは徳島駅であり、この時刻表を調べると、ご丁寧に発車ホーム番号が書かれた発車時刻表が見つかる。

https://www.jr-shikoku.co.jp/01_trainbus/jikoku/pdf/tokushima.pdf

これを印刷して、早朝, 夕方以降ではない範囲で、3,4番線に車両がおり、2番線に特急うずしおが停車ている時間を絞り込む。

条件に合っていそうな時刻を見つけ、試しに入れてみたところ正解だった。

TsukuCTF22{15:23}





## TsukuCTF Big Fan 3 (medium)

When is his birthday? The flag format is `TsukuCTF22{YYYY/MM/DD}`.



個人データが漏れてそうなツイートがあるが、引用元のツイートが消えているので、ウェブ魚拓で魚拓を調べる (以下ツイートと引用元ツイート)

(ウェブ魚拓はURLの最初に `gyo.tc/` とつけるだけで、ウェブ魚拓と一括検索のMementoの両方を調べられるので便利)

https://twitter.com/SuperProStalker/status/1571273887371120640

http://timetravel.mementoweb.org/list/20011109095230/https://twitter.com/yJRXaCh4M5aRTk0/status/1571273659335184384



引用元ツイートのアーカイブが見つかった。

https://web.archive.org/web/20220917230409/https://twitter.com/yJRXaCh4M5aRTk0/status/1571273659335184384

google driveへのリンクがあり、人名、誕生日、メールアドレス等が記載されているcsvが入っている。

この `dummy.csv` の中身を見て、名前を探したものの、3人いる「串田」姓はどれもフラグではなかった。

Discordのユーザー名がメールアドレスに使われているのでは? と思い、excel の Ctrl+F 検索で`toshi*ku`, `ku*toshi` を調べる (`*`はワイルドカード)。数人が引っかかるものの、これも違った。



よくツイートを調べると、若干アドレスが映っている写真のツイートがあった。

https://twitter.com/SuperProStalker/status/1581681692242477059?s=20

これを検索すると、該当する人物は1人だった。

`TsukuCTF22{1980/01/10}`





## FlagDM (hard)

謎の人物からDMが届きました。よく見ると脅迫文ののようです。DMを送った人物を調査して、この人物が使用している他のWebサービスを見つけてください。 注1: この人物は複数のWebサービスを使用していますが、そのいずれかのプロフィールにフラグが埋め込まれています。 注2: 画像に含まれているURLはこちらです。 `https://bit.ly/3Ekih5M`

I received a DM from a mysterious person. Please investigate the  person who sent the DM and find out what other web services this person  uses. Note 1: This person uses multiple web services, but the flag is embedded in one of their profiles. Note 2: URL included in the image is here. `https://bit.ly/3Ekih5M`

![DM](/images/202210/tsukuctf2022/DM.png)



リンク先は脅迫文が書かれているGoogle Docsで、本文中に特に気になる情報は見当たらなかった。

Google Docs から何か情報を得る方法が無いかを調べると、Google系のサービスから情報を得られる xeuledoc というプログラムが見つかった。

https://github.com/Malfrats/xeuledoc

![flagdm_xeuledoc](/images/202210/tsukuctf2022/flagdm_xeuledoc.png)

これを試すと、

`mpju40nchoyba85@gmail.com` というGoogleアカウントを見つけることができた。

このアカウントに、1件のみGoogle Mapsのレビューがあり、この写真の端の著作権表示にtwitterアカウントが書かれていた。

![flagdm_map](/images/202210/tsukuctf2022/flagdm_map.jpeg)

最初、画像の角が隠れており気づかず、他の情報を探して時間を浪費してしまった。

https://twitter.com/07xm8d9pzp

このtwitterアカウントのプロフィールに、YoutubeへのURLがあった。

https://www.youtube.com/channel/UCZ7_GtoaC4oHCKBLhJi13Ww

Google 関連のOSINTツールであるGHunt (https://github.com/mxrch/GHunt) を利用したが、特に有用な情報は得られなかった。

![flagdm_ghunt](/images/202210/tsukuctf2022/flagdm_ghunt.png)

チャンネル名の `gross_poem` が、他サービスで使われていないか、sherlock (https://github.com/sherlock-project/sherlock) で調べると、いくつかのアカウントが見つかった。

![flagdm_sherlock](/images/202210/tsukuctf2022/flagdm_sherlock.png)

間違って検索に引っかかっている情報もあったが、このうちTraktの情報を調べると、ユーザーが見つかった。

https://trakt.tv/users/gross_poem

このユーザー名はいかにもbase64な

 `TmljZSEgRmxhZyBpcyBIZXJlISAtPiBUc3VrdUNURjIye000bnlfMFMxTjdfNzAwbHNfM3gxNTd9Cg==`

であったため、デコードすると、

```
Nice! Flag is Here! -> TsukuCTF22{M4ny_0S1N7_700ls_3x157}
```

となり、フラグを得ることができた。





## bughunter (easy, guess, RFC9116)

天才ハッカーのつくし君は、どんなサイトの脆弱性でも見つけることができます。 あなたも彼のようにこのサイトの脆弱性を見つけることができますか？ 見つけたら私たちに報告してください。
`※ディレクトリの総当たりなどは禁止されています。本問題の解決には、多数のリクエストは不要です。`



問題のタグに RFC9116 とあるので、最近できた security.txt のことかとなり、

`$与えられたURL/.well-known/security.txt` を見ると、フラグが書かれていた。

```
Contact: TsukuCTF22{y0u_c4n_c47ch_bu65_4ll_y34r_r0und_1n_7h3_1n73rn37}
Expires: 2022-10-20T15:00:00.000Z
Preferred-Languages: ja, en
```





## GrandpaMemory (hard)

祖父からお誕生プレゼントが入った鍵付きの箱とa.outという名前のファイルをもらった。これを開けるには数字を入力すれば良いらしい。ヒントはこのファイルの計算結果が鍵であること、このファイルは1971年に冷蔵庫ほどもあるミニコンピューターで作成された実行ファイルであると言われた。

フラグの形式: Nは数値である。 `TsukuCTF22{N}`



a.out ファイルが与えられた。これをfileコマンドで確認すると、

```
a.out: PDP-11 old overlay
```

と、PDP-11 の実行ファイルであると判明した。また、バイナリエディタで確認したところ、`passwd is in R2` との文字列が見え、レジスタ2 の値が N であると予想された。

PDP-11のエミュレータ・逆アセンブルできるツールが無いかを調べると、以下のサイトが見つかった。

https://qiita.com/7shi/items/86724696518df3a174dc

しかし、7shiさんのサイト・記事を調べツールをDLしたものの、手元の環境でうまく動かず、結局別のツールを探すこととした。

以下のツールを試したところ、`a.out`  の逆アセンブルに成功した。

https://github.com/caldwell/pdp11dasm

```
;
; pdp11dasm version 0.0.3
; disassembly of ../a.out
;
000000: 000405                  br      14                      ; ..
;
000002: 000064                  invalid opcode                  ; 4.
000004: 000000                  halt                            ; ..
000006: 000000                  halt                            ; ..
000010: 000000                  halt                            ; ..
000012: 000000                  halt                            ; ..
;
000014: 005001                  clr     r1                      ; ..
000016: 005002                  clr     r2                      ; ..
000020: 005201                  inc     r1                      ; ..
000022: 005202                  inc     r2                      ; ..
000024: 006301                  asl     r1                      ; A.
000026: 006301                  asl     r1                      ; A.
000030: 006301                  asl     r1                      ; A.
000032: 006301                  asl     r1                      ; A.
000034: 006302                  asl     r2                      ; B.
000036: 060102                  add     r1,r2                   ; B`
000040: 000777                  br      40                      ; ..
;
000042: 060560 071563           add     r5,71563(r0)            ; pass
000046: 062167 064440           add     (r1)+,64512             ; wd i
000052: 020163 067151           cmp     r1,67151(r3)            ; s in
000056: 051040                  bis     (r0),-(r0)              ;  R
000060: 020062 005000           cmp     r0,5000(r2)             ; 2 ..

```

PDP-11 の命令セットを調べると、CLR はレジスタのクリア、INC はインクリメンタル、ASLは左ビットシフトと分かり、上のプログラムの手順を考えると、N=18 となった。

TsukuCTF22{18}





## 解けなかった問題のコメント

### Bus POWER (hard)

* 京都であることはすぐわかった
* 窓上の表示2822で調べ、梅津営業所の管轄路線のルートを調べたが、管轄路線が多く調べきれなかった
* 看板に (木/本) ? 工所 と見えるため、木工所、鉄工所、染工所等を、Google Maps や電話帳で調べたが、この正解の場所まで探しきれなかった
* 他、ヤマト運輸の営業所、京都市内の50km/h 制限4車線道路、両側と中央分離帯に植生がある道路を調べたが、調べきる前に終了時間を迎えてしまった。



### Moon (medium)

* 写真に写っている看板の端が「京都」と読める気がするので、京都を中心に調べた
* Google Lens は引っかからなかった
* 「京都」「月の満ち欠け」「レリーフ」関連のワードで検索したが、見つけることができなかった。(同じく月の満ち欠けのレリーフがある月読橋が引っかかったが、別のデザインだった)





### uTSUKUSHIi (medium)

* Google Lensは引っかからなかった
* 「世界一可愛い猫ちゃん」というワードが気になり、調べたが可愛い猫画像を眺めるだけに終わった



### TsukuCTF Big Fan 2

* ウェブサイトは`ctf 073b6d com` だとツイートしている。
  https://twitter.com/SuperProStalker/status/1571228640981192704?s=20

* 073b6dはカラーコード? これは青系の色。

* ツイートで、`xn` は美しい響きと言っている
  https://twitter.com/SuperProStalker/status/1582443953109884929?s=20
  * xnと言えば punycode

* punycode
  `ctf.xn--073b6d.com` -> `ctf.뾼뿶.com` は存在しなかった。
  * この2文字はbbyolb (BFBB)、bbweolp (BFF6) だが、これで探しても見つからなかった。

* メールアドレスでドメイン名のWHOIS情報が検索できるサービスで、判明しているメールアドレス: `byucraglar5r7nzx3np9@gmail.com` を検索しても見つからず、解けなかった。

* どうやら、`ctf` はサブドメインだと勘違いしていたのが間違いで、英数字が交じるドメイン名をpunycode化する際、`xn--ctf-073b6d` のように、英数字部を前に持ってくるそれだったらしい
  * ところで、punycodeのWikipediaにある例が、`可愛いね.そうでもないよ` なり、`MajiでKoiする5秒前` なのは何なのだろう…
    https://ja.wikipedia.org/wiki/Punycode



### Ochakumi (hard, reversing)

* 入力欄にprint とか、関数っぽい名前を入れたらコンソールの方に色々吐いていた
* Golang製のwasmっぽいものを解読しようとしたが、wasmのreversingの知見が少なく後回し



### hub_been_stolen (hard)

* authorized_keys が与えられたので、ssh-keygen でfingerprint を出し、censys とかで検索をかけるところまではやった
* 特に結果が出ず後回し



### viewer (medium)

* 中身を見ずに飛ばしてしまった



### Lucky Number 777 (easy)

* サニタイズを回避する方法が思い浮かばなかった
* もうちょっと見れば解けた気がする



### leaks4b (hard)

* あまり中身を見れなかった
* 入力欄の形式的にlog4jっぽい何かなのかなーと勝手に思っていた



### soder

* (メモを残しておらず忘れた)



### nako3ndbox

* なでしこと聞いて、最近話題になった脆弱性かなーと少しだけ見た
* あまり触る時間を取れないまま終わった



### DefuseBomb

* 1と2はすんなり解けた (回路図を印刷して、どこを無視できるか、どこがHIGHとなるべきか、LOWとなるべきかを考えた)
* 3 は久しぶりに立ち上げた kicad が超重くなっていて、少しだけ見て後回しにしてしまった
