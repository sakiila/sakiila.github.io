---
title: 最小生成树和 Prim 算法
tags:
  - 算法
categories: 开发
description: <center>RoadToRender 开发</center>
abbrlink: 3564408376
date: 2017-11-30 15:30:00
---
### 最小生成树
最小生成树[1]（也称最小权重生成树）是一副连通加权无向图中一棵权值最小的生成树。

### Prim算法
Prim算法（普里姆算法），图论中的一种算法，可在加权连通图里搜索最小生成树。意即由此算法搜索到的边子集所构成的树中，不但包括了连通图里的所有顶点（英语：Vertex (graph theory)），且其所有边的权值之和亦为最小。该算法于1930年由捷克数学家沃伊捷赫·亚尔尼克（英语：Vojtěch Jarník）发现；并在1957年由美国计算机科学家罗伯特·普里姆（英语：Robert C. Prim）独立发现；1959年，艾兹格·迪科斯彻再次发现了该算法。因此，在某些场合，普里姆算法又被称为DJP算法、亚尔尼克算法或普里姆－亚尔尼克算法[2]。

算法实现过程：以图的顶点为基础，从一个初始顶点开始，寻找触达其他顶点权值最小的边，并把该顶点加入到已触达顶点的集合中。当全部顶点都加入到集合时，算法的工作就完成了。Prim算法的本质，是基于贪心算法。

算法实现代码[3]：
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Prim {

    final static int INF = Integer.MAX_VALUE;

    public static int[] prim(int[][] matrix) {
        List<Integer> reachedVertextList = new ArrayList<>();

        //选择顶点0为初始顶点，放入已触达顶点集合中
        reachedVertextList.add(0);

        //创建最小生成树数组，首元素设为-1
        int[] parents = new int[matrix.length];
        parents[0] = -1;

        //边的权重
        int weight;
        //源顶点下标
        int fromIndex = 0;
        //目标顶点下标
        int toIndex = 0;

        while (reachedVertextList.size() < matrix.length) {
            weight = INF;

            //在已触达的顶点中，寻找到达新顶点的最短边
            for (Integer vertexIndex : reachedVertextList) {
                for (int i = 0; i < matrix.length; i++) {
                    if (!reachedVertextList.contains(i)) {
                        if (matrix[vertexIndex][i] < weight) {
                            fromIndex = vertexIndex;
                            toIndex = i;
                            weight = matrix[vertexIndex][i];
                        }
                    }
                }
            }
            //确定了权值最小的目标顶点，放入已触达顶点集合
            reachedVertextList.add(toIndex);
            //放入最小生成树的数组
            parents[toIndex] = fromIndex;
        }

        return parents;
    }


    public static void main(String[] args) {
        int[][] matrix = new int[][]{
                {0, 4, 3, INF, INF},
                {4, 0, 8, 7, INF},
                {3, 8, 0, INF, 1},
                {INF, 7, INF, 0, 9},
                {INF, INF, 1, 9, 0},
        };

        int[] parents = prim(matrix);
        System.out.println(Arrays.toString(parents));
    }
}

```

[1]: https://zh.wikipedia.org/wiki/%E6%9C%80%E5%B0%8F%E7%94%9F%E6%88%90%E6%A0%91
[2]: https://www.cnblogs.com/biyeymyhjob/archive/2012/07/30/2615542.html
[3]: https://mp.weixin.qq.com/s/x7JT7re7W7IgNCgMf1kJTA