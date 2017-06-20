<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>
# High efficiency and quality: large graphs matching
## Local node similarity
### $S_l$:local节点相似度
### $N_k(v)$:表示节点v的相邻节点的集合
1. 不包括v本身
2. 对于任意的节点u属于nk(v),u到v的距离不大于k

### $G_v^k$:G中v节点的k邻域子图
$N_k(v)\cup{v}$生成子图
### $d(u)和d(v)$:G1中节点u和G2中节点v的度
### $d_{1,1},d_{1,2}....$:$G_u^k$中节点集合$N_k(u)$的度数序列(非递增)
### $n_{min} = min\{|N_k(u)|,|N_k(v)|\}$
### $$S_l[u,v]=\frac{(n_{min}+1+D(u,v))^2}{(|V(G_u^k)|+|E(G_u^k)|(|V(G_v^k)+|E(G_v^k))}\tag{4}$$
### $$D(u,v)=\frac{min\{d(u),d(v)\}+\sum_{i=1}^{n_{min}}min\{d_{1,i},d_{2,i}\}}{2}\tag{5}$$

## Anchor selection and expansion

### two conditions
1. $min\lbrace d(u),d(v)\rbrace\ge\delta$(两个节点中平均度数更大的那个)
    $$\delta = max\left\lbrace \frac{2\times|E(G_1)|}{|V(G_1)|},\frac{2\times|E(G_2)|}{|V(G_2)|}\right\rbrace$$

## Vertex cover based refinement
### M:初始匹配节点集
### C:包含图G中最少的节点的集合，且对于G中的每条边，至少覆盖一个边上一个节点.
### I:I=V(G)-C I是V(G)的子集，并且对于任意的两个节点属于I，这两个节点之间的边都不属于
### P:P1,P2分别是G1，G2中匹配的节点的集合
### F:$C \cap P $
### $G_b$:$V(G_1)-C$ 和$V(G_2)-F2$的二分图
### w(u,v):(u,v)的权重
### $M_b$:二分图的最大权重
### $M^+(C)=(M \cap (F_1\times F_2))\cup M_b$ X是笛卡尔积
### M':经过优化后的M,和F1*F2没有交集，c-f1和M'的交集是空的

### $|M| = \sum_{i=9}^{min}\frac{min!\times max!}{i!\times (min-i)!\times (max-i)!}$
