# 【Git】【共有と相談】Git管理下での名前変更

gitの勉強を再開しました。そこで学んだことと新たに生まれた課題について共有, 相談をしたく投稿します。

<br>

### まずは共有からです。

今回は例として、 **no_name_cat.txt** というタイトルのテキストファイルを用意し、中身を下記のようにしました。

(初期の文とタイトル.png)

ここから、いくつかのパターンでgit操作をしていきます。

<br>

## 直接のファイル名変更
まずは特にgitを意識せずにファイル名と内容を変更します。今回は下記のように **still_no_name_cat.txt** というタイトルに変更し、中身を下記のように変更しました。

(直接変更/text.png)

では、ここで`git status`コマンドを実行してみましょう。

(直接変更/status.png)

どうやら、直接ファイル名を変更すると、変更前のファイルであるno_name_cat.txtは **delete** され、変更後のファイルであるstill_no_name_cat.txtが **Untracked files** にあります。つまり、still_no_came_cat.txtはgit管理されていないということになります。

この状態から、`git rm no_name_cat.txt`と`git add still_no_name_cat.txt`を実行することで、下記のようにステージに変更をあげることができます。

(直接変更/addした後のstatus.png)

ステージにあげたことで、still_no_name_cat.txtが **newfile** となっております。

では、これをコミットしてプッシュした時のGithub画面を見てみます。すると、no_name_cat.txtは消え、still_no_name_cat.txtが新規になっていることがわかります。

(直接変更/GitHub.png)

では、一旦コミット前までの手順をまとめます。
1. 直接ファイル名を変更
2. git rm (変更前のファイル名)
3. git add (変更後のファイル名)

<br>

## Git管理下でのファイル名変更

続いて、今回のテーマであるGit管理下でのファイル名変更です( **中身の変更はここではしません** )。こちら、非常にシンプルです。
### `git mv (変更前のファイル名) (変更後のファイル名)`
では実際に`git mv no_name_cat.txt new_name_cat.txt`を実行してみましょう。すると、下記のようになります。

(git mvのみ/status.png)

先ほど3手かかった上で変更とは認識してくれなかったものが、たったの **1手** で変更を認識してくれました！

手順も減り、変更を認識できるというまさに一石二鳥です。

<br>

# ここからが相談

結論から質問内容を言いますと、 **「ファイルの内容も変更する時はどうすればいいか」** です。

先ほどはファイル名 **のみ** 変更しました。しかし、new_name_cat.txtという名前に変えたということは内容も変更しようと思ったからということです。例えばgit mvでファイル名を変更した後に下記のような内容にしたとします。

(git mvの後に中身変更/text.png)

では、`git status`コマンドを実行しましょう。

(git mvの後に中身変更/中身変更時のstatus.png)

修正したという情報はステージにあげられていないことがわかります。addしていないのでそれはそうといえばそうです。

では、git mv後にgit addしたらどうなるか。下は`git add new_name_cat.txt`の結果です。

(git mvの後に中身変更/add後のstatus.png)

なんと、直接ファイル名を変えたときと同じように`delete`と`new file`に分かれてしまいました。。これでは`git mv`を使った意味が全くありません。また、試しにコミット, プッシュをしてGitHub画面を見てみましたが、no_name_cat.txtが消えてnew_name_cat.txtが新たに作られたとなっています。。

(git mvの後に中身変更/add後のGitHub.png)

### git mvの前に中身を変更したらどうなるか

ファイル名を変更する時は中身が変わった後にすることもあります。今回は下のようにまずno_name_cat.txtのまま、内容を変更しましょう。

(git mvの前に内容変更/text.png)

そして、`git status`コマンドを実行します。すると、下記のようになります。通常通り、内容の変更がステージにあげられていないというのがわかります。

(git mvの前に内容変更/内容変更直後のstatus.png)

では、`git mv no_name_cat.txt new_name_cat.txt`を実行します。どうなるでしょうか。

(git mvの前に内容変更/内容変更後、git mvした後のstatus.png)

ファイル名の変更はGit管理されていますが、変更はされていませんね。。

試しに`git add`せずにコミット, プッシュをすると下記の画像のようになります。

(git mvの前に内容変更/git addしなかった/GitHub.png)

やはり、ファイル名の変更しか情報がないです。。

### では、`git add`をしましょう！

(git mvの前に内容変更/git addした/status.png)

またもや`delete`と`new file`に分かれてしまいました。。GitHub画面も同様でした。

<br>

このように、確かに`git mv`でファイル名の変更をGit管理下でできますが、 **内容も変更するとそれをした意味がなくなります。**

どうにかしてファイル名の変更と内容の変更を同時に管理してくれる方法を探しています(前`git add -A`でうまくいった記憶があったのですが、私物PCで試してもうまくいっていないです。私の気のせいだったのでしょうか)。

#### もし、コマンドでそれを行う方法があれば教えて頂きたいです。また、私は業務ではVS codeを使用しておりますので、VS codeでそれができる方法があれば教えて頂きたいです(普段のgit操作はコマンドで行っているため、VS codeでのGit操作は無知です)。

以上です、よろしくお願いいたします。