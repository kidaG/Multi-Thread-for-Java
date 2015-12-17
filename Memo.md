#Java言語で学ぶデザインパターン入門【マルチスレッド編】

Java言語を通してマルチスレッドと並行処理のパターンを学ぶことが目的。マルチスレッドには下記のようなシングルスレッドではない懸念点がる。

* データの破壊
* デッドロック
* リソースの大量消費

これら問題を解決するのは困難であり、そのため行き当たりばったりではなく、よく使われるパターンを適用することが推奨されている。


##UMLについて
Unified Modeling Launguageの略であり、シスてもを視覚化したり、仕様や設計を文書化したりするための表現方法。
UMLの基本用語の意味は下記のようになっている。  

* アトリビュート（属性）：Javaのフィールド,C++のメンバ変数
* オペレーション（操作）：Javaのメソッド、C++のメンバ関数

###クラス図
ここではポイントだけメモします。  
白抜きの三角（△）矢印の実践：クラスの階層関係を表現しており、サブクラスからスーパークラスへ向かって表記する。**サブクラスは必ずスーパークラスを知っているため指すことができると覚えると覚えやすい**。  
白抜きの三角（△）の破線：インタフェースと実装クラスの関係。これも階層関係と同じで、**実装クラスはインターフェースを知っているため指すことができると覚えれば良い**。


##1 Java言語のスレッド
スレッド:プログラムを実行している主体

マルチスレッドのプログラミング：複数のスレッドからなるプログラム。マルチスレッドが関わるケースとしては下記のようなケースがある。

1. GUIアプリケーション
2. 時間がかかるI/O処理
3. 複数のクライアント


###Javaでのマルチスレッドの実行方法

###1. Threadクラスの拡張
Threadクラスを継承したクラスのrunメソッドを実装。そのサブクラスのインスタンスを作成して、startメソッドを呼び出す。**runメソッドを呼び出した場合は別スレッドを起動せずに、その呼び出元のスレッドでrunの中身を実行してしまうため、マルチスレッドにならないので注意が必要。**
  

	public class MyThread extends Thread{
		public void run(){
			// 別スレッドで実行したい処理の記述
		}
	}
	
	MyThread thread = new MyThread();
	thread.start();// run()を呼び出すと同一スレッドで実行
 
#####逐次・並列・並行
####1. 逐次sequential
順番に処理する
####1. 並列parallel
複数人で分担して処理する
####1. 並行concurrent
複数作業に分割し、処理する。つまり、一人でも複数でも実施される可能性がある。

 
 
###2. Runnableインターフェース

	public class Printer implements Runnable{
		public Printer(){
		}
		public void run(){
			//別スレッドで実施される処理
		}
	}
	
	new Thread(new Printer()).start();


###3 スレッドの一時停止
sleepでスレッドは一時停止可能。

	try{
		Thread.sleep(1000);
	} catch (InterruptedException e){
	}
	
###4 スレッドの排他制御
同じ変数に複数のスレッドがアクセスする場合、条件分や前提条件がうまく動作しない場合がでてくる。例えば、100円のジュースを買うと考えた際に、

1. 100円あるかチェック
2. その後-100円

という動作をすべてのスレッドが同じ財布で実施したと考えると、スレッドAとBがそれぞれ同時に財布を確認し、先にスレッドAが購入してしまうと、本来だと (財布-(スレッドAの購入分))でチェックをしなければならないところ、スレッドBはすでにチェックを終えているため、そのまま購入し、マイナスになってしまうというような現象が発生する。ここでは、１と２の処理の間に他スレッドが割り込めてしまうことが問題である。この割り込みをブロックする制御のことを排他制御mutual exclusionという。Javaでは、syncrhronizedというキーワードで処理を固まりとして実施させることができ、それで排他制御を行うことができる。
 






