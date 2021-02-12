## 追加コマンド

### redo 　コマンド

- "redo"　で実行。

- なにかが undo されてるときのみ有効。

- 何も undo してないときに redo しようとすると、"nothing to redo"と表示

- History 構造体に、begin と並列に、garbage を追加。

- undo 時に、履歴構造体の末尾を削除してしまうのではなく、切り離して、garbage の先頭に追加することでアドレスを覚えておくようにする。

- redo するときには、garbage の先頭を切り離して、begin の末尾に再度追加したのに、最初から実行することで、redo の機能を実現する。
- 付随して、free_history などを拡張。

### brush コマンド

- pen がブラシに設定され、以降、書いた図形はカラーに指定されている色で塗りつぶされる

### color コマンド

- "color {white, black, blue, green, red, yellow, magenta, cyan, reset}" で実行

- 以降のペンやブラシの色が変わる。

- 履歴に保存される

- ペンの色は canvas->penColor に保管されている。

- 色は、enum で実装。

- 描画時に、各マスに別の色を適応するために、各マスの色は配列に記憶されている。そのために、Canvas を拡張して、canvas 配列と並列に node_color 配列を記憶。

- ついでに、複数の draw 関数(line,circle,rect など)における実際の canvas 書き換え過程を取り出して、一つの関数に統一。色の書き込み忘れといったバグを防止。

### brush コマンド

- pen がブラシに設定され、以降、書いた図形はカラーに指定されている色で塗りつぶされる

### pen コマンド

- brush モードが OFF になり、pen で描画するモードに戻る。

###　留意点

ブラシで書いた図形上に上書きして pen モードで図形を描くとき、通常は背景色を個別に指定しないと、もともとある背景色をデフォルトの背景色で上書きしてしまう不具合が起きる.
しかし、pen を使う人に背景色をいちいち切り替えさせるのはよくないと考えたので、裏側で前景色と背景色を個別に保存するも、自動的にあるべき背景色を判定し、インターフェイス上では前景色と背景色をいちいち気にしなくても問題なく使えるように工夫した。