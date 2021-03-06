---
layout: post
title: "各种树总结（优缺点，效率，用途）"
tags: [算法]
---


##目录：
* 二叉树
* n叉树
* 线索二叉树
* 满二叉树
* 完全二叉树
* AVL平衡二叉树
* 平衡二叉搜索树
* 红黑树
* 二叉查找树（BST）
* B树（多路平衡搜索树）
* B+树
* LSM树
* trie树（单词查找树）
* 哈弗曼树（最优二叉树）
* prim最小生成树
* kruskal最小生成树
* dijkstra最短路径
* floyd最短路径

##简介
###二叉树 
**二叉树**英语：（Binary tree）是每个节点最多有两个子树的树结构

###满二叉树
 一棵深度为k，且有2^k-1个节点称之为**满二叉树**；
###完全二叉树
深度为k，有n个节点的二叉树，当且仅当其每一个节点都与深度为k的满二叉树中，序号为1至n的节点对应时，称之为**完全二叉树**。

###线索二叉树
**线索二叉树**(保留遍历时结点在任一序列的前驱和后继的信息)，线索二叉树能线性地遍历二叉树，从而比递归的中序遍历更快。使用线索二叉树也能够方便的找到一个节点的父节点，这比显式地使用父亲节点指针或者栈效率更高。这在栈空间有限，或者无法使用存储父节点的栈时很有作用。传统的二叉树一般都是以链式存储的结构来表示。这样，二叉树中的每个节点都可以用链表中的一个链节点来存储，每个链节点就包含了若干个指针。但是，这种传统的链式存储结构只能表现出二叉树中节点之间的父子关系，而且不能利用空余的指针来直接得到某个节点的在特定的遍历顺序（先序，中序，后序）中的直接前驱和直接后继。通过分析传统的二叉树链式存储结构表示的二叉树中，存在大量的空闲指针。若能利用这些空指针域来存放指向该节点的直接前驱或是直接后继的指针，则可以进行某些更方便的运算。这些被重新利用起来的空指针就被称为线索，加上了这些线索的二叉树就是线索二叉树。
###二叉搜索树
 **二叉搜索树**（英语：Binary Search Tree），也称有序二叉树（英语：ordered binary tree），排序二叉树（英语：sorted binary tree），是指一棵空树或者具有下列性质的二叉树：1.若任意节点的左子树不空，则左子树上所有结点的值均小于或等于它的根结点的值；2.任意节点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；3.任意节点的左、右子树也分别为二叉查找树。4.没有键值相等的节点（英语：no duplicate nodes）。二叉查找树相比于其他数据结构的优势在于查找、插入的时间复杂度较低。为O(log n)。二叉查找树是基础性数据结构，用于构建更为抽象的数据结构，如集合、multiset、关联数组等。对于一般的二叉搜索树（Binary Search Tree）其期望高度（即为一棵平衡树时）为log2n，其各操作的时间复杂度（O(log2n)）同时也由此而决定。但是，在某些极端的情况下（如在插入的序列是有序的时），二叉搜索树将退化成近似链或链，此时，其操作的时间复杂度将退化成线性的，即O(n)。我们可以通过随机化建立二叉搜索树来尽量的避免这种情况，但是在进行了多次的操作之后，由于在删除时，我们总是选择将待删除节点的后继代替它本身，这样就会造成总是右边的节点数目减少，以至于树向左偏沉。这同时也会造成树的平衡性受到破坏，提高它的操作的时间复杂度。
###平衡二叉搜索树
**平衡二叉搜索树**（Balanced Binary Tree）又被称为AVL树，具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。其高度一般都良好地维持在O（log2n），AVL是最先发明的自平衡二叉查找树算法。在AVL中任何节点的两个儿子子树的高度最大差别为一，所以它也被称为高度平衡树，n个结点的AVL树最大深度约1.44log2n。查找、插入和删除在平均和最坏情况下都是O（log n）。增加和删除可能需要通过一次或多次树旋转来重新平衡这个树。AVL的每一次插入结点操作最多只需要旋转1次(单旋转或双旋转)，平衡树上删除一个结点后可以通过旋转使其平衡,最坏的情形下需从叶结点的双亲结点到根结点逐层向上进行平衡旋转,旋转次数不超过树的深度,所以应为 O ( 1og2n )
###红黑树
**红黑树**是一种自平衡二叉查找树，它是复杂的，但它的操作有着良好的最坏情况运行时间，并且在实践中是高效的: 它可以在O(log n)时间内做查找，插入和删除。红黑树是每个节点都带有颜色属性的二叉查找树，颜色为红色或黑色。在二叉查找树强制一般要求以外，对于任何有效的红黑树我们增加了如下的额外要求:
性质1. 节点是红色或黑色。
性质2. 根是黑色。
性质3. 所有叶子都是黑色（叶子是NIL节点）。
性质4. 每个红色节点必须有两个黑色的子节点。(从每个叶子到根的所有路径上不能有两个连续的红色节点。)
性质5. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。
因为每一个红黑树也是一个特化的二叉查找树，因此红黑树上的只读操作与普通二叉查找树上的只读操作相同。然而，在红黑树上进行插入操作和删除操作会导致不再符合红黑树的性质。恢复红黑树的性质需要少量(O(log n))的颜色变更(实际是非常快速的)和不超过三次树旋转(对于插入操作是两次)。虽然插入和删除很复杂，但操作时间仍可以保持为 O(log n) 次，set,map,multiset,multimap-基于红黑树(RB-tree)，hash_map,hash_set,hash_multiset,hash_multimap-基于hash table。
###B树
**B树**，概括来说是一个一般化的二叉查找树（binary search tree），可以拥有多于2个子节点。与自平衡二叉查找树不同，B-树为系统最优化大块数据的读和写操作。B-tree算法减少定位记录时所经历的中间过程，从而加快存取速度。这种数据结构常被应用在数据库和文件系统的实作上
###Ｂ+树
**Ｂ+树**，这些键值的拷贝被存储在内部节点；键值和记录存储在叶子节点；另外，一个叶子节点可以包含一个指针，指向另一个叶子节点以加速顺序存取。B+ 树的特点是能够保持数据稳定有序，其插入与修改拥有较稳定的对数时间复杂度。B+ 树元素自底向上插入，这与二叉树恰好相反。B+ 树在节点访问时间远远超过节点内部访问时间的时候，比可作为替代的实现有着实在的优势。这通常在多数节点在次级存储比如硬盘中的时候出现。通过最大化在每个内部节点内的子节点的数目减少树的高度，平衡操作不经常发生，而且效率增加了。这种价值得以确立通常需要每个节点在次级存储中占据完整的磁盘块或近似的大小。B+ 背后的想法是内部节点可以有在预定范围内的可变数目的子节点。因此，B+ 树不需要象其他自平衡二叉查找树那样经常的重新平衡。B+ 树叶子节点里每个键值都指向真正的数据块（如Oracle里的RowID），每个叶子节点都有前指针和后指针，这是为了做范围查询时，叶子节点间可以直接跳转，从而避免再去回溯至枝和跟节点。B+树最大的性能问题是会产生大量的随机IO，随着新数据的插入，叶子节点会慢慢分裂，逻辑上连续的叶子节点在物理上往往不连续，甚至分离的很远，但做范围查询时，会产生大量读随机IO。
###结构化合并树
**结构化合并树**（Log-Structured Merge-Trees）LSM-tree是由两个或两个以上存储数据的结构组成的。最简单的LSM-tree由两个部件构成。一个部件常驻内存，称为C0树（或C0），可以为任何方便键值查找的数据结构，另一个部件常驻硬盘之中，称为C1树（或C1），其数据结构与B-tree类似。C1中经常被访问的结点也将会被缓存在内存中。为了让读性能尽量高，数据在磁盘中必须得有序，这就是B+树的原理，但是写就悲剧了，因为会产生大量的随机IO，磁盘寻道速度跟不上。LSM树本质上就是在读写之间取得平衡，和B+树相比，它牺牲了部分读性能，用来大幅提高写性能。它的原理是把一颗大树拆分成N棵小树， 它首先写入到内存中（内存没有寻道速度的问题，随机写的性能得到大幅提升），在内存中构建一颗有序小树，随着小树越来越大，内存的小树会flush到磁盘上。当读时，由于不知道数据在哪棵小树上，因此必须遍历所有的小树，但在每颗小树内部数据是有序的。
###trie树
**trie树**又称前缀树或字典树，是一种有序树，用于保存关联数组，其中的键通常是字符串。与二叉查找树不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的前缀，也就是这个节点对应的字符串，而根节点对应空字符串。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。
###哈夫曼最优生成树
**哈夫曼树**(Huffman tree)给定n个权值作为n的叶子结点，构造一棵二叉树，若带权路径长度达到最小，称这样的二叉树为最优二叉树，也称为哈夫曼树(Huffman tree)。哈夫曼树是带权路径长度最短的树，权值较大的结点离根较近。用于无损数据压缩
###Prim最小生成树
**普里姆算法**（Prim算法）加点法，图论中的一种算法，可在加权连通图里搜索最小生成树。意即由此算法搜索到的边子集所构成的树中，不但包括了连通图里的所有顶点，且其所有边的权值之和亦为最小。贪婪算法的应用
###kruskal最小生成树
**kruskal算法**，加边法，求加权连通图的最小生成树的算法。kruskal算法总共选择n- 1条边，所使用的贪婪准则是：从剩下的边中选择一条不会产生环路的具有最小耗费的边加入已选择的边的集合中。注意到所选取的边若产生环路则不可能形成一棵生成树。kruskal算法分e 步，其中e 是网络中边的数目。按耗费递增的顺序来考虑这e 条边，每次考虑一条边。当考虑某条边时，若将其加入到已选边的集合中会出现环路，则将其抛弃，否则，将它选入。贪婪算法的应用
###Dijkstra最短路径
**迪科斯彻算法**（英语：Dijkstra's algorithm）使用了广度优先搜索解决非负权有向图的单源最短路径问题，算法最终得到一个最短路径树。该算法常用于路由算法或者作为其他图算法的一个子模块。举例来说，如果图中的顶点表示城市，而边上的权重表示著城市间开车行经的距离，该算法可以用来找到两个城市之间的最短路径。最初的戴克斯特拉算法不采用最小优先级队列，时间复杂度是O(|V|^2)(其中|V|为图的顶点个数)。通过斐波那契堆实现的迪科斯彻算法时间复杂度是O(|E|+|V|\log|V|) (其中|E|是边数) （Fredman & Tarjan 1984）。对于不含负权的有向图，这是目前已知的最快的单源最短路径算法。
###Floyd最短路径
**Floyd算法**又称为插点法，是一种用于寻找给定的加权图中多源点之间最短路径的算法。中文亦称弗洛伊德算法，是解决任意两点间的最短路径的一种算法，可以正确处理有向图或负权的最短路径问题，同时也被用于计算有向图的传递闭包。Floyd-Warshall算法的时间复杂度为O(N^3)[3]，空间复杂度为O(N^2)


##参考文献：

+ [二叉树](http://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%A0%91)
+. [红黑树](http://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)
3. [B树](http://zh.wikipedia.org/wiki/B%E6%A0%91)
4. [B+树](http://zh.wikipedia.org/wiki/B%2B%E6%A0%91)
5. [二叉查找树-平衡二叉树-红黑树-B树的深度对比分析](http://blog.csdn.net/hzx5212/article/details/12152715)
6. [LSM树](http://www.cnblogs.com/siegfang/archive/2013/01/12/lsm-tree.html)
7. [HBase LSM树 VS B+树](http://blog.csdn.net/dbanote/article/details/8897599)
8. [trie树](http://zh.wikipedia.org/wiki/Trie)
9. [哈夫曼树](http://zh.wikipedia.org/wiki/%E9%9C%8D%E5%A4%AB%E6%9B%BC%E7%BC%96%E7%A0%81)
10. [普里姆算法](http://zh.wikipedia.org/wiki/%E6%99%AE%E6%9E%97%E5%A7%86%E7%AE%97%E6%B3%95)
10. [Kruskal算法](http://zh.wikipedia.org/wiki/%E5%85%8B%E9%B2%81%E6%96%AF%E5%85%8B%E5%B0%94%E6%BC%94%E7%AE%97%E6%B3%95)
11. [迪科斯彻算法](http://zh.wikipedia.org/wiki/%E6%88%B4%E5%85%8B%E6%96%AF%E7%89%B9%E6%8B%89%E7%AE%97%E6%B3%95)
12. [Floyd算法](http://zh.wikipedia.org/wiki/Floyd-Warshall%E7%AE%97%E6%B3%95)