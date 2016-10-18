#課題内容
>## 課題 (Cbench のボトルネック調査)
>
>Ruby のプロファイラで Cbench のボトルネックを解析しよう。
>
>以下に挙げた Ruby のプロファイラのどれかを使い、Cbench や Trema のボトルネック部分を発見し遅い理由を解説してください。
>
>### Ruby 向けのプロファイラ
>
>* [profile](https://docs.ruby-lang.org/ja/2.1.0/library/profile.html)
>* [stackprof](https://github.com/tmm1/stackprof)
>* [ruby-prof](https://github.com/ruby-prof/ruby-prof)
>
>これ以外にもいろいろあるので、好きな物を使ってかまいません。
>
>## 発展課題 (Cbench の高速化)
>
>ボトルネックを改善し Cbench を高速化しよう。

#解答
ruby-profを用いてCbenchのボトルネックを解析した．
Cbenchコントローラの動作を解析するので，

```
＄ ruby-prof ./bin/trema run ./lib/cbench.rb > output.txt
```

を実行し，結果をテキストファイルとして出力した．
出力結果は以下のようになった．

```
Measure Mode: wall_time
Thread ID: 10766200
Fiber ID: 10764440
Total: 111.723606
Sort by: self_time

 %self      total      self      wait     child     calls  name
  2.48      8.160     2.773     0.000     5.387   488349  *BinData::BasePrimitive#_value
  2.26      3.966     2.524     0.000     1.442   323388   Kernel#define_singleton_method
  2.09      6.847     2.334     0.000     4.513   418709  *BinData::BasePrimitive#snapshot
  1.97      6.313     2.207     0.000     4.107   172427   Kernel#dup
  1.93      2.151     2.151     0.000     0.000   265648   Kernel#initialize_copy
  1.88     22.408     2.101     0.000    20.307   222554   BinData::Struct#instantiate_obj_at
  1.58      5.198     1.768     0.000     3.429   259498   Kernel#clone
  1.57      1.755     1.755     0.000     0.000   207221   BinData::BasePrimitive#initialize_instance
  1.48     23.379     1.657     0.000    21.722   251298   BinData::Base#new
  1.48      5.950     1.655     0.000     4.295   258585  *BinData::BasePrimitive#method_missing
  1.47      1.642     1.642     0.000     0.000   187637   String#size
  1.46      2.980     1.628     0.000     1.352   267975   BinData::SanitizedField#name_as_sym
  1.41     15.112     1.578     0.000    13.534   206288   BinData::Primitive#method_missing
  1.37      1.669     1.525     0.000     0.144   314114   BinData::Base#get_parameter
  1.29      1.442     1.442     0.000     0.000   323388   BasicObject#singleton_method_added
  1.24      1.465     1.390     0.000     0.075   805292   Kernel#respond_to?
  1.22     24.755     1.362     0.000    23.392   251298   BinData::SanitizedPrototype#instantiate
  1.21      1.348     1.348     0.000     0.000   123081   String#unpack
  1.16      1.301     1.301     0.000     0.000   272033   Symbol#to_sym

```
