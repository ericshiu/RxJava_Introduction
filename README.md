# 介紹 RxJava 

RxJava是Reactive Extensions的JVM實現的Library，
用於通過使用可觀察序列來編寫**異步**和基於**事件**的程序。

它擴展了觀察者模式以支持**數據/事件**序列，
並添加了允許您以聲明方式**組合序列的運算符**，
同時抽像出對低級線程，同步，線程安全和並發數據結構等問題的關注。

# 作者
[<img src="https://imgur.com/iFnWJf3.png" height="70" width="70">](https://www.linkedin.com/in/hsiu-yi-chu/)        [<img src="https://avatars0.githubusercontent.com/u/43841563?s=400&v=4" height="70" width="70">](https://github.com/biuecatneo)

# **目錄**
[TOC]

# Reactive Extensions 是什麼？
![](https://imgur.com/DB4HMv0.png)
## 介紹
[Reactive Extensions](http://reactivex.io/) 是 Microsoft open source 推廣的一個Library
> The Observer pattern done right
> [name= reactivex.io] [color=#907bf7]
>  ReactiveX is a combination of the best ideas from
the Observer pattern, the Iterator pattern, and functional programming
>  [name= reactivex.io] 

* **<span style="color:red">Observer pattern (觀察者模式)</span>**
* **<span style="color:red">Iterator pattern (覆器/迭代器)</span>**
* **<span style="color:red">functional programming (函式語言程式設計)</span>**

把以上三種設計組合起來就是**Reactive Programming.**
## Reactive Programming 是什麼呢？
Reactive Programming 是 **<span style="color:red">asynchronous data streams（異步的資料流）**</span>這個概念出發的撰寫方式。

下方為比較常見的事件，本質上就是異步事件，你可以監聽處理這些事件。
* Event buses 
* Click events 

> 事件可以被等待，可以觸發過程，也可以觸發其它事件。

Reactive Programming 概念：

* 你可以用包括 Click 和 Hover 事件在內的任何東西創建 Data stream。
* Stream 廉價且常見。
* 任何東西都可以是一個 Stream：變量、用戶輸入、屬性、Cache、數據結構等等。

在這個基礎上，你還有令人驚艷的函數去組合、創建、過濾這些 Streams。

Stream 可以多個的。你可以 merge Stream，也可以過濾出資料，並且生成一個新的 Stream。

## 使用ReactiveX前...
### 支援語言
+ Languages
  - Java: RxJava
  - JavaScript: RxJS
  - C#: Rx .NET
  - C#(Unity): UniRx
  - Scala: RxScala
  - Clojure: RxClojure
  - C++: RxCpp
  - Lua: RxLua
  - Ruby: Rx.rb
  - Python: RxPY
  - Go: RxGo
  - Groovy: RxGroovy
  - JRuby: RxJRuby
  - Kotlin: RxKotlin
  - Swift: RxSwift
  - PHP: RxPHP
  - Elixir: reaxive
  - Dart: RxDart
### 為什麼我們需要異步操作?
>因為用戶是我們的朋友，而且我們關心它，我希望他們能有好的體驗。
>
>如果你使用異步操作的話，你可以利用伺服器的能力，那些因為被設備限制而不能實現的，需要真實的後台進程才能處理的事情，就可以實現了。
>
>你可以給你的用戶提供美妙的體驗，而不會拖慢他們的速度，凍屏或者跳屏都不會出現了。
>
>  [name= Christina Lee/ @RunChristinaRun] [color=#907bf7]
>> 白話文一點就是呢，充分利用伺服器能力，
>> 提高運算效能，給客戶更好的使用體驗
>> 
>> [name= Eric] [color=red]
# RxJava
## Single/SingleObserver 一種觀察者模式組合

SingleEmitter 
它提供的數據和通知的方法如下：

* onSuccess()
* onError()

SingleObserver 的相關的方法：
* onSubscribe()
* onSuccess()
* onError()

那麼 Single/SingleObserver 有什麼使用場景呢？

+ <span style="color:red"> 使用場景
  + 從數據庫拿取資料，在這種情況是一條單一的資料。
  + 為了滿足這種單一數據的使用場景，便出現了Single。</span>

```java=
Single.create(new SingleOnSubscribe<String>() {
    @Override
    public void subscribe(@NonNull SingleEmitter<String> e) throws Exception {
        e.onSuccess("Success");
        //e.onError(new Throwable("Error"));
    }
});
```
```java=
new SingleObserver<String>() {
    @Override
    public void onSubscribe(@NonNull Disposable d) {
        logger.info("onSubscribe");
    }
    @Override
    public void onSuccess(@NonNull String s) {
        logger.info("onSuccess" + s);
    }
    @Override
    public void onError(@NonNull Throwable e) {
        logger.info("onError" + e.getMessage());
    }
};
```

## Completable/CompletableObserver 一種觀察者模式組合
CompletableEmitter 
它提供的數據和通知的方法如下：
* onComplete()
* onError()

SingleObserver 的相關的方法：
* onSubscribe()
* onComplete()
* onError()

Completable只有通知相關的方法，不能發送數據。
+ <span style="color:red">使用場景
  + 像數據庫發送更新，不需要對資料處理或不需要理會內容，只需要一條完成通知。</span>
```java=
Completable.create(new CompletableOnSubscribe() {
    @Override
    public void subscribe(@NonNull CompletableEmitter e) throws Exception {
        e.onComplete();
        //e.onError(new Throwable("Error"));
    }
});
```
```java=
new CompletableObserver<String>() {
    @Override
    public void onSubscribe(@NonNull Disposable d) {
        logger.info("onSubscribe");
    }
    @Override
    public void onComplete() {
        logger.info("onComplete");
    }
    @Override
    public void onError(@NonNull Throwable e) {
        logger.info("onError" + e.getMessage());
    }
};
```
## Maybe/MaybeObserver 一種觀察者模式組合
Maybe
它提供的數據和通知的方法如下：

* onSuccess()
* onComplete()
* onError()

SingleObserver 的相關的方法：
* onSubscribe()
* onSuccess()
* onComplete()
* onError()

可發送一條資料，以及發送一個完成通知，或者一條異常通知。
其中完成通知和異常通知只能發送一個，<span style="color:red">發送資料只能在發送完成通知或者異常通知之前，否則發送據無效。</span>
```java=
Maybe.create(new MaybeOnSubscribe<String>(){
    @Override
    public void subscribe(@NonNull MaybeEmitter<String> e) throws Exception {
        e.onSuccess("onSuccess");
        //e.onError(new Throwable("Error"));
        //e.onComplete();
    }
});
```
```java=
new MaybeObserver<String>() {
    @Override
    public void onSubscribe(@NonNull Disposable d) {
        logger.info("onSubscribe");
    }
    @Override
    public void onSuccess(@NonNull String s) {
        logger.info("onSuccess" + s);
    }
    @Override
    public void onComplete() {
        logger.info("onComplete");
    }
    @Override
    public void onError(@NonNull Throwable e) {
        logger.info("onError" + e.getMessage());
    }
};
```
## Observable/Observer 一種觀察者模式組合
Observable
它提供的數據和通知的方法如下：

* onNext()
* onComplete()
* onError()

Observer 的相關的方法：
* onSubscribe()
* onNext()
* onComplete()
* onError()

Observable相當於一個事件發送器，每執行一次 onNext()，觀察者就會收到一次資料，資料發送完畢後調用 onComplete() 方法。

+ <span style="color:red">**使用場景**：
  + 當汽車零件這些東西再傳送帶上執行的時候，你構建它，增加東西，最終當汽車完成後的時候，你有了最終的產品。
  + 所以我們放進一些東西，一些原材料，我們用不同的方法構建它，然後我們得到我們想要的產品。</span>

```java=
Observable.create(new ObservableOnSubscribe<String>(){
    @Override
    public void subscribe(@NonNull MaybeEmitter<String> e) throws Exception {
        observableEmitter.onNext("Hello Next 1");
        observableEmitter.onNext("Hello Next 2");
        observableEmitter.onComplete();
    }
});
```
```java=
new Observer<String>() {
    @Override
    public void onSubscribe(@NonNull Disposable d) {
        logger.info("onSubscribe");
    }
    @Override
    public void onNext(@NonNull String s) {
        logger.info("onNext" + s);
    }
    @Override
    public void onComplete() {
        logger.info("onComplete");
    }
    @Override
    public void onError(@NonNull Throwable e) {
        logger.info("onError" + e.getMessage());
    }
};
```
## Flowable/Subscriber 一種觀察者模式組合
### 介紹Flowable前要先理解什麼是Backpressure

Flowable 就是為瞭解決Backpressure問題，
<span style="color:red">**可以說Flowable與Observable差別就是解決Backpressure**</span>
異步場景中，**事件提供者**所發送事件速度遠快於**事件處理者**的處理速度的情況下，
一種告訴上游的**事件提供者**降低發送速度的策略。(無法消化)
### 以 Observable 介紹發生無法消化
從下方Code可以看到
每個任務需要處理1s，但是任務不停的在增加，直到最後拋出OOM。
這時候就會發生，內存使用快速增長造成服務器的壓力。

```java=
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(@NonNull ObservableEmitter<Integer> e) throws Exception {
        int i = 0;
        while(i < Integer.MAX_VALUE){
            e.onNext(i);
            i++;
        }
    }
})
// 指定Observable自身在哪個調度器上執行
.subscribeOn(Schedulers.newThread()) 
    // 指定一個觀察者在哪個調度器上觀察這個Observable 
    .observeOn(Schedulers.newThread())
    // Consumer 也是一個 Observer 
    // 只有一個 accept() 方法
    // 下方可以改寫成
    // .subscribe(i -> logger.info("msg:[{}]", i));
    .subscribe(new Consumer<Integer>() {
        @Override
        public void accept(Integer integer) throws InterruptedException{
            Thread.sleep(1000);
            logger.info("msg:[{}]", i);
        }
    });
```

![](https://imgur.com/wqpUvkv.png )

### 以 Flowable 介紹 Backpressure 策略

> **<span style="color:red">這些策略都可以自己寫看看，去體驗其中的差別，</span>**
> **<span style="color:red">網路上也非常多文章在介紹相對的策略以及使用場景</span>**
> 下方有References許多參考文件提供查閱

+ BUFFER
  + 緩衝所有 onNext，直到接收者消耗它。
+ DROP
  + 如果下游無法跟上，則刪除最近的 onNext。
+ ERROR
  + 如果下游無法跟上，則發出 MissingBackpressureException 信號。
+ LATEST
  + 僅保留最新的 onNext 值，如果下游無法跟上，則覆蓋之前的任何值。
+ MISSING
  + 這種策略模式下相當於沒有指定任何的背壓策略，不會對數據做緩存或丟棄處理，需要下游通過背壓操作 
    + onBackpressureBuffer()
    + onBackpressureDrop()
    + onBackpressureLatest()
```java=
public void demoForFlowableBackpressure() {
        Flowable.create(new FlowableOnSubscribe<Integer>() {
            @Override
            public void subscribe(@NonNull FlowableEmitter<Integer> e) throws Exception {
                int i = 0;
                while (i < Integer.MAX_VALUE) {
                    e.onNext(i);
                    i++;
                }
            }
        // 在這邊增加 BackpressureStrategy
        }, BackpressureStrategy.DROP)
            .subscribeOn(Schedulers.newThread())
            .observeOn(Schedulers.newThread())
            .subscribe(new Subscriber<Integer>() {
                @Override
                public void onSubscribe(Subscription s) {
                    s.request(Long.MAX_VALUE);
                }
                @Override
                public void onNext(Integer i) {
                    try {
                        Thread.sleep(5);
                        logger.info("msg:[{}]", i);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                @Override
                public void onError(Throwable t) {
                }

                @Override
                public void onComplete() {
                }
            });

    }
```

Flowable有一個緩衝池，那個這個大小是多少？
可以再Class內看到！

![](https://imgur.com/oyS2soj.png)

因為使用 <span style="color:#FFA500">**BackpressureStrategy.DROP**</span> 這個策略，
可以看到 127 到 1012072233 中間的資料被丟掉了！

![](https://imgur.com/l6kWDRp.png)

相對地使用的記憶體也變得相對穩定。

![](https://imgur.com/7jcGKfc.png)
# [常用操作API](http://reactivex.io/documentation/operators.html#filtering)
大多數運算符都在Observable上運行並返回一個Observable。這允許您在鏈中一個接一個地應用這些運算符。

鏈中的每個運算符都會修改由前一個運算符的運算產生的Observable。

還有其他模式，如Builder模式，其中特定類的各種方法通過方法的操作修改該對象，對同一類的項進行操作。

這些模式還允許您以類似的方式鏈接方法。

但是在Builder模式中，方法在鏈中出現的順序通常並不重要，因為Observable運算符的順序很重要。

一組Observable運算符不能在原始的Observable上獨立運行，它發起鏈，但它們依次運行，每個運算符都由運算符在鏈中的前一個運算符生成。
## [create 介紹](http://reactivex.io/documentation/operators/create.html)
創建被觀察者和觀察者
![](https://imgur.com/rz0J44G.png)
```java=
Single.create(new SingleOnSubscribe<String>() {
    @Override
    public void subscribe(@NonNull SingleEmitter<String> e) throws Exception {
        e.onSuccess("Success");
        //e.onError(new Throwable("Error"));
    }
});
```
```java=
new SingleObserver<String>() {
    @Override
    public void onSubscribe(@NonNull Disposable d) {
        logger.info("onSubscribe");
    }
    @Override
    public void onSuccess(@NonNull String s) {
        logger.info("onSuccess" + s);
    }
    @Override
    public void onError(@NonNull Throwable e) {
        logger.info("onError" + e.getMessage());
    }
};
```
## [map 介紹](http://reactivex.io/documentation/operators/map.html)
Map的作用是對每一個事件應用一個方法，
是的每一個事件都按照指定的函數去變化，
與 Java 8 Stream Map 的作用幾乎一致。
![image alt](https://imgur.com/vqduMFb.png)
```java=
Observable.create((ObservableOnSubscribe<String>) observableEmitter -> {
            observableEmitter.onNext("Hello");
            observableEmitter.onComplete();
        })
                // 這邊把String 增加 Word
                .map((s -> s + " Word"))
                .subscribe(new Observer<String>() {
                    @Override
                    public void onSubscribe(@NonNull Disposable d) {
                    }

                    @Override
                    public void onNext(@NonNull String s) {
                        logger.info("onNext : " + s);
                    }

                    @Override
                    public void onError(@NonNull Throwable e) {
                    }

                    @Override
                    public void onComplete() {
                        logger.info("onComplete");
                    }
                });
```
輸出結果
```console
2019-05-19 08:36:52.469  INFO 22930 --- [main] 
com.example.demo.rxjava.CreateObserver   : onNext : Hello  Word
```
## [just 介紹](http://reactivex.io/documentation/operators/just.html)
為了一下方便介紹，先提前介紹just
just就是一個簡單的發送器依次調用 onNext() 方法。

![image alt](https://imgur.com/wVnBhLg.png)

```java=
Observable.just("1", "2", "3")
    .subscribe((Consumer<String>) s -> logger.info("onNext : " + s));
```
輸出結果
```console
2019-05-19 08:51:28.817  INFO 22963 --- [main] 
com.example.demo.DemoApplicationTests    : onNext:1
2019-05-19 08:51:28.818  INFO 22963 --- [main]
com.example.demo.DemoApplicationTests    : onNext:2
2019-05-19 08:51:28.818  INFO 22963 --- [main] 
com.example.demo.DemoApplicationTests    : onNext:3
```
## [filter 介紹](http://reactivex.io/documentation/operators/filter.html)
顧名思義就是個過濾器
![](https://imgur.com/OQGmCTu.png)
```java=
Observable.just(1, 2, 3, 4, 5)
    // 設定濾過策略
    .filter(i -> i > 3)
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 09:10:35.010  INFO 23007 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 4
2019-05-19 09:10:35.010  INFO 23007 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 5
```
## [distinct 介紹](http://reactivex.io/documentation/operators/distinct.html)
distinct可以作為一個簡單的去重複的方法，直接看輸出可以馬上瞭解。
![](https://imgur.com/kYS1gB2.png)
```java=
Observable.just("1", "2", "3", "2", "3", "4")
    .subscribe(s -> logger.info("onNext : " + s));
```
輸出結果
```console
2019-05-19 09:10:35.010  INFO 23007 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 1
2019-05-19 09:10:35.010  INFO 23007 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 2
2019-05-19 09:10:35.010  INFO 23007 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 3
2019-05-19 09:10:35.010  INFO 23007 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 4
```
## [interval 介紹](http://reactivex.io/documentation/operators/interval.html)
相當於一個定時任務。
但需要注意的是，interval 均默認在新線程。
![](https://imgur.com/RIllc45.png)
```java=
//分別三個接收參數為：第一次發送延遲，間隔時間，時間單位
Observable.interval(1, 1, TimeUnit.SECONDS)
    .subscribe(s -> logger.info("onNext : " + s));
```
輸出結果
```console
2019-05-19 09:06:09.302  INFO 22996 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 0
2019-05-19 09:06:10.303  INFO 22996 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 1
2019-05-19 09:06:11.304  INFO 22996 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 2
2019-05-19 09:06:12.302  INFO 22996 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 3
2019-05-19 09:06:13.302  INFO 22996 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 4
2019-05-19 09:06:14.302  INFO 22996 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 5
```
## [skip 介紹](http://reactivex.io/documentation/operators/skip.html)
skip 其實就是略過的意思，接受一個 long 參數的 count ，代表跳過 count 個數目開始接收。
![](https://imgur.com/pLlHod1.png)
```java=
Observable.just("1", "2", "3")
    // 略過兩個
    .skip(2)
    .subscribe(s -> logger.info("onNext : " + s));
```
輸出結果
```console
2019-05-19 08:51:28.818  INFO 22963 --- [main] 
com.example.demo.DemoApplicationTests    : onNext:3
```
## [take 介紹](http://reactivex.io/documentation/operators/take.html)
take，接受一個 long 參數 count ，表示最多接收 count 個資料。
![](https://imgur.com/wTjZk6X.png)
```java=
Observable.just("1", "2", "3", "4")
    // 最多接受兩個
    .take(2)
    .subscribe(s -> logger.info("onNext : " + s));
```
輸出結果
```console
2019-05-19 08:51:28.817  INFO 22963 --- [main] 
com.example.demo.DemoApplicationTests    : onNext:1
2019-05-19 08:51:28.818  INFO 22963 --- [main]
com.example.demo.DemoApplicationTests    : onNext:2
```
## [buffer 介紹](http://reactivex.io/documentation/operators/buffer.html)
Observable送出的資料收集到List中並發出這些List，而不是一次發送一個資料。
想要快速理解的話，我們可以通過例圖和範例代碼來進一步瞭解它。
![image alt](https://imgur.com/cSdJLkZ.png)
```java=
Observable.just(1, 2, 3, 4, 5)
    // 設定組裝策略
    .buffer(3, 2)
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 09:49:53.719  INFO 23137 --- [main]
com.example.demo.DemoApplicationTests    : onNext : [1, 2, 3]
2019-05-19 09:49:53.720  INFO 23137 --- [main] 
com.example.demo.DemoApplicationTests    : onNext : [3, 4, 5]
2019-05-19 09:49:53.720  INFO 23137 --- [main] 
com.example.demo.DemoApplicationTests    : onNext : [5]
```
## [debounce 介紹](http://reactivex.io/documentation/operators/debounce.html)
debounce去除發送頻率過快的資料。
![](https://imgur.com/Z5mfarl.png)
```java=
Observable.create((ObservableOnSubscribe<String>) observableEmitter -> {
    observableEmitter.onNext("1"); // 通過
    Thread.sleep(1500);
    observableEmitter.onNext("2"); // 太快略過
    Thread.sleep(505);
    observableEmitter.onNext("3"); // 太快略過
    Thread.sleep(700);
    observableEmitter.onNext("4"); // 通過
    Thread.sleep(1001);
    observableEmitter.onNext("5"); // 通過
    observableEmitter.onComplete();
    //去除發送間隔時間小於 1000 毫秒的事件。
}).debounce(1000, TimeUnit.MILLISECONDS)
        .subscribe(i -> logger.info("onNext : " + i));
```
2 ，3 被去掉了
```console
2019-05-19 10:24:12.376  INFO 23217 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 1
2019-05-19 10:24:15.094  INFO 23217 --- [ionThreadPool-1]
com.example.demo.DemoApplicationTests    : onNext : 4
2019-05-19 10:24:15.097  INFO 23217 --- [           main]
com.example.demo.DemoApplicationTests    : onNext : 5
```
## [zip 介紹](http://reactivex.io/documentation/operators/zip.html)
通過指定的功能將多個Observable的排放組合在一起，
並根據此功能的結果為每個組合發出單個項目，
兩兩配對，也就意味著，最終配對出的 Observable 事件數目只和少的那個相同。

![](https://imgur.com/BKxwSAG.png)

```java=
// 第三個參數是你要處理的策略
Observable.zip(Observable.just(1, 2, 3, 4, 5), Observable.just("a", "b", "c"), (x, y) -> x + y)
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 18:36:54.497  INFO 23744 --- [           main]
com.example.demo.DemoApplicationTests    : onNext : 1a
2019-05-19 18:36:54.497  INFO 23744 --- [           main]
com.example.demo.DemoApplicationTests    : onNext : 2b
2019-05-19 18:36:54.497  INFO 23744 --- [           main]
com.example.demo.DemoApplicationTests    : onNext : 3c
```
## [concat 介紹](http://reactivex.io/documentation/operators/concat.html)

從兩個或多個Observable 的資料連接，不交錯資料。
![](https://imgur.com/aYvvMCu.png)
```java=
// 連接兩個Observable
Observable.concat(Observable.just(1, 2), Observable.just("a", "b", "c"))
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 18:40:31.664  INFO 23758 --- [           main]
com.example.demo.DemoApplicationTests    : onNext : 1
2019-05-19 18:40:31.664  INFO 23758 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 2
2019-05-19 18:40:31.664  INFO 23758 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : a
2019-05-19 18:40:31.665  INFO 23758 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : b
2019-05-19 18:40:31.665  INFO 23758 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : c
```
## [flatMap 介紹](http://reactivex.io/documentation/operators/flatmap.html)
將Observable發出的項目，通過某種方法轉換為多個Observables，然後再把這些分散的 Observables裝進一個 Observable 。

但是flatMap 並不能保證事件的順序，如果需要保證，需要用到我們下面要講的 ConcatMap。
![](https://imgur.com/QqR7GlZ.png)

```java=
Observable.just(4, 5)
    // 轉換多個Observables
    .flatMap(i -> {
        List<String> list = new ArrayList<>();
        for (int k = 0; k < i; k++) {
            list.add(i + " new msg : " + k);
        }
        return Observable.fromIterable(list);
    })
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 21:30:38.338  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 0
2019-05-19 21:30:38.338  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 1
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 2
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 3
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 0
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 1
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 2
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 3
2019-05-19 21:30:38.339  INFO 23976 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 4
```
## [concatMap 介紹](http://reactivex.io/documentation/operators/flatmap.html)
concatMap 與 FlatMap 的唯一區別就是 concatMap 保證了順序
![](https://imgur.com/GX47uGI.png)
```java=
Observable.just(4, 5)
    // 轉換多個Observables
    .flatMap(i -> {
        List<String> list = new ArrayList<>();
        for (int k = 0; k < i; k++) {
            list.add(i + " new msg : " + k);
        }
        return Observable.fromIterable(list);
    })
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 0
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 1
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 2
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 4 new msg : 3
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 0
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 1
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 2
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 3
2019-05-19 21:33:36.928  INFO 23988 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 5 new msg : 4
```
## [defer 介紹](http://reactivex.io/documentation/operators/defer.html)
在觀察者訂閱之前不要創建Observable，然後再訂閱都會創建一個新的 Observable，並且如果沒有被訂閱，就不會產生新的 Observable。
![](https://imgur.com/0sJZ0oG.png)
```java=
// 使用defer
Observable<Integer> observable = Observable.defer((Callable<ObservableSource<Integer>>) () -> Observable.just(1, 2, 3));
observable.subscribe(integer -> logger.info("onNext : " + integer));
```
輸出結果
```console
2019-05-19 21:42:13.397  INFO 24023 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 1
2019-05-19 21:42:13.397  INFO 24023 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 2
2019-05-19 21:42:13.397  INFO 24023 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 3
```
## [last 介紹](http://reactivex.io/documentation/operators/last.html)
last 僅發出Observable發出的最後一項，或者是滿足某些條件的最後一項。
![](https://imgur.com/7yEAtVE.png)
```java=
Observable.just(1, 2, 3, 4, 5)
    // 設定回傳最後，如果沒有最後一個就回傳4
    .last(4)
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果
```console
2019-05-19 10:24:12.376  INFO 23217 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 5
```
## [first 介紹](http://reactivex.io/documentation/operators/first.html)
僅發送第一項元素，並可設定無元素可發送時的預設值。
![](https://imgur.com/rH0bP00.png)
```java=
Observable
    .just(1, 2, 3, 4, 5)
    .first(0)
    .subscribe(s -> logger.info("onNext : " + s));
```
輸出結果
```console
19:22:23.250  INFO 3020 --- [main]
com.example.demo.DemoApplicationTests    : onNext : 1
```
## [merge 介紹](http://reactivex.io/documentation/operators/merge.html)
merge 顧名思義，而在 RxJava 中，merge 的作用是把多個 Observable 結合起來。
![](https://imgur.com/vuoEDrF.png)
## [reduce 介紹](http://reactivex.io/documentation/operators/reduce.html)
reduce 操作符每次用一個方法處理一個值。
最後輸出最終結果。
![](https://imgur.com/iOSuHLc.png)
```java=
Observable.just(1, 2, 3, 4, 5)
    // 設定方法處理內容
    .reduce((x, y) -> x += y)
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果：只有最終的答案
```console
2019-05-19 11:32:21.486  INFO 23333 --- [           main]
com.example.demo.DemoApplicationTests    : onNext : 15
```
## [scan 介紹](http://reactivex.io/documentation/operators/scan.html)
scan 操作符作用和上面的 reduce 一致，唯一區別是 reduce 是追求最終結果，而 scan 會始終如一地把每一個步驟都輸出。
![](https://imgur.com/VpYM1Bn.png)
```java=
 Observable.just(1, 2, 3, 4, 5)
    // 設定方法處理內容
    .scan((x, y) -> x += y)
    .subscribe(i -> logger.info("onNext : " + i));
```
輸出結果：包含所有過程
```console
2019-05-19 11:29:21.497  INFO 23318 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 1
2019-05-19 11:29:21.497  INFO 23318 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 3
2019-05-19 11:29:21.497  INFO 23318 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 6
2019-05-19 11:29:21.497  INFO 23318 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 10
2019-05-19 11:29:21.497  INFO 23318 --- [           main] 
com.example.demo.DemoApplicationTests    : onNext : 15
```
## [window 介紹](http://reactivex.io/documentation/operators/window.html)
定期將Observable中的項目細分為Observable窗口並發出這些窗口，
而不是一次發出一個項目。
![](https://imgur.com/IQ37Vmi.png)
```java=
Observable.interval(1, TimeUnit.SECONDS)
    // 每三秒創建一個新的
    .window(3, TimeUnit.SECONDS)
    .subscribe(i -> {
                logger.info("onNext : " + i);
                i.subscribe(x -> logger.info("onNextNext : " + x));
            }
    );
```
輸出結果：
```console=
2019-05-19 18:24:54.997  INFO 23707 --- [           main] com.example.demo.DemoApplicationTests    : onNext : io.reactivex.subjects.UnicastSubject@9cd25ff
2019-05-19 18:24:56.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 0
2019-05-19 18:24:57.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 1
2019-05-19 18:24:58.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 2
2019-05-19 18:24:58.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNext : io.reactivex.subjects.UnicastSubject@15d8c2aa
2019-05-19 18:24:59.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 3
2019-05-19 18:25:00.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 4
2019-05-19 18:25:01.003  INFO 23707 --- [ionThreadPool-1] com.example.demo.DemoApplicationTests    : onNext : io.reactivex.subjects.UnicastSubject@2782db14
2019-05-19 18:25:01.004  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 5
2019-05-19 18:25:02.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 6
2019-05-19 18:25:03.003  INFO 23707 --- [ionThreadPool-2] com.example.demo.DemoApplicationTests    : onNextNext : 7
```
## Scheduler 介紹（ 線程切換 ）

提供了6種使用場景

 + Schedulers .io()
   + 主要用於一些耗時IO操作，DB存取，網絡溝通等。
   + 這個調度器具有線程緩存機制，它會根據需要，增加或者減少線程池中的線程數量。 
 + Schedulers.computation()
   + 這個計算指的是 CPU 密集型計算，即不會被 I/O 等操作限制性能的操作。 
   + 例如圖形的計算。 
 + Schedulers.newThread()
   + 開啓一個新的線程，不具有線程緩存機制。
   + 因為創建一個新的線程比復用一個線程更耗時耗力。
   + 因此，Schedulers.newThread() 的效率沒有 Schedulers .io()高。
 + Schedulers.from(Executor executor)
   + 使用指定的 Executor 來作為線程調度器 
 + Schedulers.single()
   + 擁有一個線程單例，所有的任務都在這線程中。 
 + Schedulers.trampoline()
   + 在當前線程執行一個任務，但不是立即執行，用trampoline()將它加入隊列。
   + 這個調度器將會處理它的隊列並且按程序運行隊列中每一個任務。
   
   
### subScribeOn 介紹 / observeOn 介紹

 + subscribeOn
   + 指定被觀察者在哪個調度器上執行，跟調用的位置沒有關係。
   + 直到遇到observeOn改變線程調度器。
 + observerOn
   + 指定下游觀察者對數據的操作運行在哪個調度器上。在調用位置切換線程。 
```java=
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(@NonNull ObservableEmitter<Integer> e) throws Exception {
        int i = 0;
        while(i < Integer.MAX_VALUE){
            e.onNext(i);
            i++;
        }
    }
})
// 指定Observable自身在哪個調度器上執行
.subscribeOn(Schedulers.newThread()) 
    // 指定一個觀察者在哪個調度器上觀察這個Observable 
    .observeOn(Schedulers.newThread())
    .subscribe(i -> logger.info("msg:[{}]", i));
```
# RxJava 2.x 與 RxJava 1.x  的差別
## API變化
## 新增Flowable
# 簡單的範例
## 建立 RxJava on SpringBoot MVC
## 任務實現心跳
## 先讀取緩存，如果緩存沒有再通過DB請求獲取數據
透過<code>concat</code>操作符可將多個<code>Observable</code>照順序連接，很適合用來依序於不同數據層請求數據。例如若想先從緩存請求資料，沒有資料時從數據庫請求，再沒有則於其他網路來源請求，此時可建立三種數據層之被觀察者，並以<code>concat()</code>連接，再由<code>Observer</code>訂閱。若某一層取得數據則調用<code>onNext()</code>發送，未取得則調用<code>onComplete()</code>執行下一層觀察者，並可透過<code>first()</code>來限定僅發送第一個取得的數據。

```java=
Map<String, String> memorySource = new HashMap<>(); //模擬緩存
Map<String, String> databaseSource = new HashMap<>(); //模擬數據庫
Map<String, String> networkSource = new HashMap<>(); //模擬其他網路來源
networkSource.put("data", "123456");

Observable<String> memoryObservable = 
    Observable.create((ObservableOnSubscribe<String>)observableEmitter -> {
        String data = memorySource.get("data");
        if (data != null) {
            logger.info("get data from memorySource!");
            observableEmitter.onNext(data);//有取得數據則發送
        } else {
            observableEmitter.onComplete();//沒取得數據則直接結束
        }
    }).subscribeOn(Schedulers.io());
    
Observable<String> databaseObservable =
    Observable.create((ObservableOnSubscribe<String>)observableEmitter -> {
        String data = databaseSource.get("data");
        if (data != null) {
            logger.info("get data from databaseSource!");
            memorySource.put("data", data);
            observableEmitter.onNext(data);
        } else {
            observableEmitter.onComplete();
        }
    }).subscribeOn(Schedulers.io());
    
Observable<String> networkObservable =
    Observable.create((ObservableOnSubscribe<String>)observableEmitter -> {
        String data = networkSource.get("data");
        if (data != null) {
            logger.info("get data from netWorkSource!");
            databaseSource.put("data", data);
            observableEmitter.onNext(data);
        } else {
            observableEmitter.onComplete();
        }
    }).subscribeOn(Schedulers.io());
    
//使用concat()依序連接被觀察者, 並使用first()設定預設值及跳過第一次發送後的操作    
logger.info("First request...");//第一次請求會由memorySource取得
String data = Observable.concat(memoryObservable, databaseObservable, networkObservable).first("no data!").blockingGet();
logger.info("data:[{}]", data);

logger.info("Second request...");//第二次請求會由databaseSource取得
String dataAgain = Observable.concat(memoryObservable, databaseObservable, networkObservable).first("no data!").blockingGet();
logger.info("dataAgain:[{}]", dataAgain);

logger.info("Third request...");//第三次請求會由networkSource取得
String dataThirdTime = Observable.concat(memoryObservable, databaseObservable, networkObservable).first("no data!").blockingGet();
logger.info("dataThirdTime:[{}]", dataThirdTime);
```
輸出結果：
```console=
18:09:42.472  INFO 9380 --- [           main] com.example.demo.DemoApplicationTests    : First request...
18:09:42.494  INFO 9380 --- [readScheduler-1] com.example.demo.DemoApplicationTests    : get data from netWorkSource!
18:09:42.495  INFO 9380 --- [           main] com.example.demo.DemoApplicationTests    : data:[123456]
18:09:42.496  INFO 9380 --- [           main] com.example.demo.DemoApplicationTests    : Second request...
18:09:42.496  INFO 9380 --- [readScheduler-1] com.example.demo.DemoApplicationTests    : get data from databaseSource!
18:09:42.496  INFO 9380 --- [           main] com.example.demo.DemoApplicationTests    : dataAgain:[123456]
18:09:42.496  INFO 9380 --- [           main] com.example.demo.DemoApplicationTests    : Third request...
18:09:42.497  INFO 9380 --- [readScheduler-2] com.example.demo.DemoApplicationTests    : get data from memorySource!
18:09:42.497  INFO 9380 --- [           main] com.example.demo.DemoApplicationTests    : dataThirdTime:[123456]
```
# 結論
Reactive Programming 並不容易, 
且它是一個難以學習的東西, 
因為你必須從命令式編程中轉移並開始以**Reactive Programming**思考, 
但是一旦你理解它，它將簡化你的Coding life。
# References

 + [这可能是最好的RxJava 2.x 教程](https://www.jianshu.com/p/0cd258eecf60)
 + [reactivex.io](http://reactivex.io/)
 + [30 Day RxJS ](https://ithelp.ithome.com.tw/users/20103367/ironman/1199)
 + [Christina Lee: Intro to RxJava](https://youtu.be/XLH2v9deew0)
 + [Exploring RxJava in Android — Utility Operators](https://proandroiddev.com/exploring-rxjava-in-android-utility-operators-a6115024800d)
 + [5 reasons to use RxJava in your projects](https://jaxenter.com/5-reasons-use-rxjava-148982.html)
