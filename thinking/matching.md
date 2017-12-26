# 考えておかなきゃいけないこと

* 見え方が異なるのでモデル点は
  * (0 - 30)は確実に見えると仮定
  * (30 - 45+α)は、半分見えると仮定
	* つまりしきい値を×0.5する感じ？
			
* 深く考えだすと面倒そうだ
  * 基本的にROOT_INFOでは、core_featureだけで共通化を試みる
  * 共通化できなかった候補だけに関して、sub_featureだけで共通化を試みる
	
# 処理に必要な変数

* root_zbin
* leaf_zbin
* ROOT_INFO
  * num_feature
  * feature_arry
  * thre
  * num_leaf
  * leaf_arry
* LEAF_INFO
  * offset
  * sum_feature
  * feature_arry
  * sum_thre
* ROOT_RESULT
* LEAF_RESULT

# 検出時の処理の流れ

* ROOT_INFOの決定
  * ix, iyのスライディングウィンドウに対して
	* for ii in num_feature
	  * feature_arry[ii].xとfeature_arry[ii].yを使って、密ボクセルにアクセス
	  * root_zbinに投票
	* max(root_zbin)がしきい値を超えていたら
	* for jj in num_leaf
	  * leaf_zbinにコピー
	  * for ii in num_feature
		* leaf_zbinに投票

# モデル教示時の処理の流れ

* COREが3点以上の候補を集めて
  * まずは、特徴点の各候補における分布で分類
	  * まずは、主成分分析で分類
	  * 分類の中で
		* もっとも特徴点の多い候補を取得
		  * この原点がROOTの原点になる
		  * 各候補で、もっともマッチするoffsetを取得
	    * bit演算&グリードで適当にleafを計算

* SUBが3点以上の候補を集めて
  * SUB特徴点を使って、COREのときと同じ処理を行う

* 残ったのは、LEFTとして

そもそも、COREとSUBで数え方が違う。。。COREはスコアが１あがるがSUBは0.5しか上がらない。
