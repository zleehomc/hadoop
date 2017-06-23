若公式显示不正常，安装[GitHub with MathJax](https://chrome.google.com/webstore/detail/github-with-mathjax/ioemnmodlmafdkllaclgeombjnmnbima/related?utm_source=chrome-app-launcher-info-dialog).
# High efficiency and quality: large graphs matching

## 3 Problem statement
$G(V,E)$:图G
$V(G)$:表示节点集合
$E(G)$:表示边缘集合
$$score(M)=\frac{\sum_{(u1,v1) \in M}\sum_{(u2,v2)\in M} e_{u1,u2}\times e_{v1,v2} }{2}\tag{1}$$

## 5 Matching construction
### 5.1 Global and local node similarity
$S_{[u, v]} = S_{g[u, v]} × S_{l[u, v]}$

$S_{[u, v]}$:表示节点相似度

$S_{g[u, v]}$:表示全局相似度

$S_{l[u, v]}$:表示局部相似度
#### Global node similarity
$L_{n×n}$:拉普拉斯矩阵，n位节点数量

$L=D-A$:D是对角矩阵(度)，A是图的邻接矩阵

$α_1,α_2....α_n$,$β_1,β2....βn$:分别是$L_1,L_2$的特征值

$L_1,L_2$是对称的，正半定的，一定存在$L_1=U_1∧_1U_1^T$,$L_2=U_2∧_2U_2^T$,$U_1,U_2$是正交矩阵，$∧_1=diag(α_i),∧_2=diag(β_i)$是
如果$G_1$和$G_2$是同构的，一定存在置换矩阵P，$PU_1∧_1U_1^TP^T=U_2∧_2U_2^T$,让$P=U_2D'U_1^T$，其中$D'= diag(d_1,..., d_n)$,$di \in \{+1; −1\}$

当$G_1$和$G_2$是同构的，最优置换矩阵是P，其中最大tr($P^T \overline U_2 \overline U^T_1$),其中$ \overline U_1, \overline U_2$是$U_1,U_2$每个值的绝对值,让$c=min\{|V(G_1)|,|V(G_2)|\}$,$\overline U'_1,\overline U'_1$是$\overline U_1,\overline U_2$的前c列

$S_g = \overline U'_1 \overline U'^T_2$     $S_g \in [0,1]$

##### Example 1
 ![](https://ooo.0o0.ooo/2017/06/22/594b30d6958d0.jpg)
#### Alternative global node similarity measures
$S'_g=r_1r_2^T$:分别是Katz score，RWR score

#### Local node similarity
$S_l$:local节点相似度

$N_k(v)$:表示节点v的相邻节点的集合

1. 不包括v本身
2. 对于任意的节点u属于nk(v),u到v的最短RWR距离不大于k

$G_v^k$:G中v节点的k邻域子图，$N_k(v)\cup{v}$生成子图

$d(u)和d(v)$:G1中节点u和G2中节点v的度

$d_{1,1},d_{1,2}....$:$G_u^k$中节点集合$N_k(u)$的度数序列(非递增)

$n_{min} = min\{|N_k(u)|,|N_k(v)|\}$

$$S_l[u,v]=\frac{(n_{min}+1+D(u,v))^2}{(|V(G_u^k)|+|E(G_u^k)|(|V(G_v^k)+|E(G_v^k))}\tag{4}$$

$$D(u,v)=\frac{min\{d(u),d(v)\}+\sum_{i=1}^{n_{min}}min\{d_{1,i},d_{2,i}\}}{2}\tag{5}$$

### 5.2 Anchor selection and expansion
#### Anchor Selection
Anchor two conditions

1. $min\lbrace d(u),d(v)\rbrace\ge\delta$(两个节点中平均度数更大的那个)

    $\delta = max\left\lbrace \frac{2\times|E(G_1)|}{|V(G_1)|},\frac{2\times|E(G_2)|}{|V(G_2)|}\right\rbrace$

2. $S_l[u,v]\ge τ$ τ是阈值，往往大于0.5

Anchor:一组集合，用A表示

$S_1$:图$G_1$的A匹配的节点

$S_2$:图$G_2$的A匹配的节点

```
Algorithm 2 anchor-selection (G1, G2)
Require: two graphs G1 and G2;
Ensure: a list of matched anchor pairs A;

1: compute the similarity matrix S;
2: A ← ∅; S1 ← ∅; S2 ← ∅;
3: for all u ∈ V(G1) and v ∈ V(G2) in decreasing order of their similarity S[u, v] do
4: if S[u, v] ≥ τ and min{d(u), d(v)} ≥ δ and u ∈/ S1 and v /∈ S2 then
5: A ← A ∪ {(u, v)}; S1 ← S1 ∪ {u}; S2 ← S2 ∪ {v};

```
时间复杂度$O(|V(G_1)|^2·(|V(G_1)|+|E(G_1)|) + |V(G_2)|^2 · (|V(G_2)| + |E(G_2)|))$
#### Anchor Expansion
M:最终求的结果，初始状态为A

$N(u),N(v)$：分别是$G_1,G_2$的直接邻居
```
Algorithm 3 anchor-expansion (G1, G2, A)
Require: two graphs, G1 and G2, and the anchor pairs A;
Ensure: a graph matching M;

1: M ← A; Q ← ∅; S1 ← ∅; S2 ← ∅;
2: for all (u, v) ∈ A do
3: S1 ← S1 ∪ {u}; S2 ← S2 ∪ {v}; Q ← Q ∪ (N(u) × N(v));
4: while Q not equl ∅ do
5: remove (u, v) from Q with the largest similarity Sl[u, v];
6: if u ∈/ S1 and v /∈ S2 then
7: M ← M ∪ {(u, v)}; S1 ← S1 ∪ {u}; S2 ← S2 ∪ {v}; Q ← Q ∪ (N(u) × N(v));
8: return M;

```
时间复杂度$O(|V(G_1)|·|V(G_2)| · min{|V(G_1)|, |V(G_2)|})$

### 5.3 Discussion on τ for anchor selection
τ的选择很重要，因为τ如果过大，初始匹配点过少，很难匹配，如果τ过小，初始点过多，存在错误的初始匹配的几率就越大，根据错误的匹配的匹配错误率更大。所以采用算法4步进的方法调整τ，找到一个最优值，由于这个步进的方法Anchor Selection已经完成，主要是完成Anchor-Expansion步骤，并且通过table可以看出，Anchor-Expansion的占用的时间是少数的，所以这个步进方法占用了很少的时间。
```
Algorithm 4 construct-opt (G1, G2)
Require: two graphs, G1 and G2;
Ensure: a graph matching between G1 and G2;

1: τ ← 0.5; Mopt ← ∅;
2: AI ←anchor-selection (G1,G2); {Algorithm 2}
3: for all τi = 0.5 + i × l such that τi ∈ [0.5, 1] do
4: A ← {(u, v)|(u, v) ∈ AI , S[u, v] ≥ τi};
5: M ← anchor-expansion (G1, G2, A); {Algorithm 3}
6: if score(M) > score(Mopt) then
7: Mopt ← M;
8: return Mopt ;
```
时间复杂度$O(|V(G_1)|^2 · (|V(G_1)|+|E(G_1)|) + |V(G_2)|^2 · (|V(G_2)| +|E(G_2)|))$


## 6 Matching refinement
### 6.1 Vertex cover based refinement
M:初始匹配节点集

C:包含图G中最少的节点的集合，且对于G中的每条边，至少覆盖一个边上一个节点.

I:I=V(G)-C I是V(G)的子集，并且对于任意的两个节点属于I，这两个节点之间的边都不属于

P:P1,P2分别是G1，G2中匹配的节点的集合

F:$C \cap P $

$G_b$:$V(G_1)-C$ 和$V(G_2)-F2$的二分图

w(u,v):(u,v)的权重

$M_b$:二分图的最大权重

$M^+(C)=(M \cap (F_1\times F_2))\cup M_b$ X是笛卡尔积

M':经过优化后的M,和F1*F2没有交集，c-f1和M'的交集是空的

$|M| = \sum_{i=9}^{min}\frac{min!\times max!}{i!\times (min-i)!\times (max-i)!}$

## 7 Labeled graph handling
