##收缩分层：更快、更简单的分层路由##
###译者###
这是一位名叫Robert Geisberger、毕业于卡尔斯鲁厄大学的学生的一篇论文。完稿于2008年7月1号。指导老师为 Peter Sanders、Dominik Schultes 两位博士。  

贡献人员：ltg1710  
文件地址：[github](https://github.com/ltg1710/translate/blob/master/diploma_thesis_geisberger.md)
###摘要###
基于节点收缩技术，我们提出了一个路线规划技术。每次我们收缩（或者说为移除）一个节点，同时在剩下的图里添加快捷边（这样做是为了保留的最短路径距离）。由此而产生的收缩分层里，包含原始图、快捷点。通过节点的选择，还定义了节点按重要度有序。通过一个修改后的双向Dijkstra算法，来获得这个节点序。前向搜索引导至更重要的点、后向搜索来自于更总要的点，从而搜索空间得到收缩。双向的搜索区域的终点将会是在一个最短路径上的最重要节点。在取得节点序时，我们使用一个简单但易扩展、启发式的方法：优先级队列。每个节点的优先级是几个方面的线性组合。例如：根据收缩后的图形的稀疏性来决定一个方面的权重；另外一个方面的权重是收缩节点将会引导更均匀的收缩。根据应用的不同，我们可以选择不同的优先级方面组合，以获得所需的层次结构。相较于以前的最好的分层Dijkstra加速技术，查询时间快五倍；空间开销（用于距离计算的数据结构）也少于输入图。
###1 介绍###
####1.1 动机####
求解最短路径问题及其相关问题是很长时间了。Dijkstra算法[7]基本上解决了最短路径问题，但是对于大图来说，它是没有效率的。最近，整个大陆的路网构成的巨大真实世界的图，这需要一个复杂的算法。其应用范围从使用移动设备的摩托车旅游规划与大型工业公司的工厂选址问题。问题是：多大的空间开销；我们承受范围内的预处理时间；最短路径查询能够有多快。我们的目标是一种新的算法，这种算法是基于各种Dijkstra基础算法的。这些算法包括多对多最短路径[21]，中转节点导航[2]，实时路况导航。我们的目标是：保持简单但可扩展、合理硬件需求但可计算最优路径。
####1.2 相关工作
这一节是有关[32][34]相关工作的简单描述。最近大家深入研究了两点之间的最短路径的加速技术。基于Dijkstra算法，它们可以分为分层技术或者目标导向技术，亦或者这两种技术的结合体。我们更专注于最重要的、最新的加速技术。更详细的介绍请参考[32][34]。
#####1.2.1 经典结果
Dijkstra算法[7]为节点维护一个试探距离数组。算法访问节点的次序为该节点与起始点s之间的距离序，并维护*****。如果从起始点s经节点u到节点v的试探距离小于当前节点v的试探距离，则认为节点u的前趋边(u,v)是通畅的。当访问到目标节点时，Dijkstra算法结束。该算法的搜索空间复杂度为O(n)，平均访问n/2个节点。  
通常，Dijkstra算法执行时使用优先队列。考虑其前的O(n)的复杂度，这将导致O(n log n)的执行效率。  
一个简单的优化是同时从起始点s和目标点t进行双向Dijkstra运算。一旦某个节点同时出现在双向的访问节点中，可以收集信息得出最短路径[6]。在路网中，我们的搜索空间看起来像一个圆盘。我们也可以想象一种加速方法，半径只有上个圆盘一半半径的2个圆盘，其面积只有一个圆盘时的一半。包括收缩分层在内，许多加速技术都是双向搜索的。
#####1.2.2分层方法
**达成(`reach`)路由**：定义：R(v):= max {Rst(v) | s, t 2 V } 描述节点v的达成值，其中Rst(v):= min(d(s, v), d(v, t))。Gutman[17]发现最短路径搜索可以描述为：从起始和终结点出发， 在达成值最小的点处相遇。这个概念被Goldberg et al. [11, 14, 15]通过快捷概念大大加强了。  
**公路分层(HHs)**通过在层次等级


###参考文献

[1]: "J. M. Anthonisse. The rush in a directed graph. Technical Report BN 9/71, Stichting Mathematisch Centrum, Amsterdam, 1971."  
[2]: "H. Bast, S. Funke, D. Mat¼evic, P. Sanders, and D. Schultes. In transit to constant shortestpath queries in road networks. In Workshop on Algorithm Engineering and Experiments (ALENEX), pages 46–59. SIAM, 2007."  
[3]: "H. Bast, S. Funke, P. Sanders, and D. Schultes. Fast routing in road networks with transit nodes. Science, 316(5824):566, 2007."  
[4]: "R. Bauer and D. Delling. SHARC: Fast and robust unidirectional routing. In Workshop on Algorithm Engineering and Experiments (ALENEX), 2008."  
[5]: "R. Bauer, D. Delling, P. Sanders, D. Schieferdecker, D. Schultes, and D. Wagner. Combining hierarchical and goal-directed speed-up techniques for D¼kstra’s algorithm. In 7th Workshop on  Experimental Algorithms (WEA), 2008."  
[6]: "G. B. Dantzig. Linear Programming and Extensions. Princeton University Press, 1962."  
[7]: "E. W. D¼kstra. A note on two problems in connexion with graphs. Numerische Mathematik,1:269–271, 1959."  
[8]: "J. Fakcharoenphol and S. Rao. Planar graphs, negative weight edges, shortest paths, andnear linear time. J. Comput. Syst. Sci, 72(5):868–889, 2006."  
[9]: "L. C. Freeman. A set of measures of centrality based on betweenness. Sociometry, 40:35–41,1977."  
[10]: "R. Geisberger, P. Sanders, and D. Schultes. Better approximation of betweenness centrality.In Workshop on Algorithm Engineering and Experiments (ALENEX), 2008."  
[11]: "A. Goldberg, H. Kaplan, and R. F.Werneck. Reach for A*: Efficient point-to-point shortestpath algorithms. In Workshop on Algorithm Engineering and Experiments (ALENEX),pages 129–143, Miami, 2006."  
[12]: "A. V. Goldberg and C. Harrelson. Computing the shortest path: A* meets graph theory. Technical Report MSR-TR-2004-24, Microsoft Research, 2004."  
[13]: "A. V. Goldberg and C. Harrelson. Computing the shortest path: A* meets graph theory. In 16th ACM-SIAM Symposium on Discrete Algorithms, pages 156–165, 2005."  
[14]: "A. V. Goldberg, H. Kaplan, and R. F. Werneck. Better landmarks within reach. In 9th DIMACS Implementation Challenge [?], 2006."  
[15]: "A. V. Goldberg, H. Kaplan, and R. F. Werneck. Better landmarks within reach. In 6th Workshop on Experimental Algorithms (WEA), volume 4525 of LNCS, pages 38–51. Springer, 2007."  
[16]: "A. V. Goldberg and R. F.Werneck. Computing point-to-point shortest paths from external memory. In Workshop on Algorithm Engineering and Experiments (ALENEX), pages 26–40, 2005."  
[17]: "R. Gutman. Reach-based routing: A new approach to shortest path algorithms optimized for road networks. In Workshop on Algorithm Engineering and Experiments (ALENEX),pages 100–111, 2004."  
[18]: "P. E. Hart, N. J. Nilsson, and B. Raphael. A formal basis for the heuristic determination of minimum cost paths. IEEE Transactions on System Science and Cybernetics, 4(2):100–107, 1968."  
[19]: "M. Hilger. Accelerating point-to-point shortest path computations in large scale networks. Diploma Thesis, Technische Universität Berlin, 2007."  
[20]: "P. N. Klein. Multiple-source shortest paths in planar graphs. In 16th ACM-SIAM Symposium on Discrete Algorithms, pages 146–155. SIAM, 2005."  
[21]: "S. Knopp, P. Sanders, D. Schultes, F. Schulz, and D. Wagner. Computing many-to-many shortest paths using highway hierarchies. In Workshop on Algorithm Engineering and Experiments (ALENEX), 2007."  
[22]: "E. Köhler, R. H. Möhring, and H. Schilling. Acceleration of shortest path and constrained shortest path computation. In 4th International Workshop on Efficient and Experimental Algorithms (WEA), 2005."  
[23]: "E. Köhler, R. H. Möhring, and H. Schilling. Fast point-to-point shortest path computations with arc-flags. In 9th DIMACS Implementation Challenge [?], 2006."  
[24]: "U. Lauther. An extremely fast, exact algorithm for finding shortest paths in static networks with geographical background. In Geoinformation und Mobilität – von der Forschung zur praktischen Anwendung, volume 22, pages 219–230. IfGI prints, Institut für Geoinformatik, Münster, 2004."  
[25]: "U. Lauther. An experimental evaluation of point-to-point shortest path calculation on roadnetworks with precalculated edge-flags. In 9th DIMACS Implementation Challenge[?], 2006."   
[26]: "J. Maue, P. Sanders, and D. Mat¼evic. Goal directed shortest path queries using Precomputed Cluster Distances. ACM Journal of Experimental Algorithmics, 2007. invited submission for special issue on WEA 2006."  
[27]: "R. Möhring, H. Schilling, B. Schütz, D. Wagner, and T. Willhalm. Partitioning graphs to speed up D¼kstra’s algorithm. In 4th International Workshop on Efficient and Experimental Algorithms (WEA), pages 189–202, 2005."  
[28]: "R. Möhring, H. Schilling, B. Schütz, D. Wagner, and T. Willhalm. Partitioning graphs to speed up D¼kstra’s algorithm. ACM Journal of Experimental Algorithmics, 11(Article 2.8):1–29, 2006."  
[29]: "R Development Core Team. R: A Language and Environment for Statistical Computing. http://www.r-project.org, 2004."  
[30]: "P. Sanders and D. Schultes. Highway hierarchies hasten exact shortest path queries. In 13th European Symposium on Algorithms (ESA), volume 3669 of LNCS, pages 568–579.Springer, 2005."  
[31]: "P. Sanders and D. Schultes. Engineering highway hierarchies. In 14th European Symposium on Algorithms (ESA), volume 4168 of LNCS, pages 804–816. Springer, 2006."  
[32]: "P. Sanders and D. Schultes. Engineering fast route planning algorithms. In 6th Workshop on Experimental Algorithms (WEA), volume 4525 of LNCS, pages 23–36. Springer, 2007."  
[33] http://algo2.iti.uka.de/schultes/hwy/ "P. Sanders, D. Schultes, and C. Vetter. Mobile Route Planning, 2008. in preparation."  
[34]: "D. Schultes. Route Planning in Road Networks. PhD thesis, Universität Karlsruhe (TH), 2008."  
[35]: "D. Schultes and P. Sanders. Dynamic highway-node routing. In 6th Workshop on Experimental Algorithms (WEA), volume 4525 of LNCS, pages 66–79. Springer, 2007."  
[36] http://www.census.gov/geo/www/tiger/tigerua/ua_tgr2k.html "U.S. Census Bureau, Washington, DC. UA Census 2000 TIGER/Line Files. 2002."  