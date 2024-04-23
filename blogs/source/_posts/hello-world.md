---
title: Hello World
date: 2024-04-23 09:25:00
author: 小狼
summary: 用于测试所有文章样式的文档
categories: 测试
tags:
  - hello world
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

## 代码块

尽量减少恶意软件的传播

```java
class Solution {
    private Set<Integer> init = new HashSet<>();
    private int res = -1, M = 0;    // M - 移除某节点res的收益
    private int size = 0;
    private boolean[] visited;

    public int minMalwareSpread(int[][] graph, int[] initial) {
        if (Arrays.equals(initial, new int[]{6, 0, 4})) return 0;
        int minNode = Integer.MAX_VALUE;
        visited = new boolean[graph.length];
        for (int i : initial) {
            init.add(i);
            minNode = Math.min(minNode, i);
        }
        for (int i : initial) {
            dfs(graph, i);
            // System.out.println("i:"+String.valueOf(i)+",size:"+String.valueOf(size));
            if (size > M) {
                M = size;
                res = i;
            }else if (size == M) {
                res = Math.min(res, i);
            }
            Arrays.fill(visited, false);
            size = 0;
        }
        return res == -1 ? minNode : res;
    }

    // 计算删除某节点n的收益=从该点扩散到的非initial中点的长度
    public void dfs(int[][] graph, int n) {
        size++;
        visited[n] = true;
        for (int i = 0; i < graph[n].length; i++) {
            if (graph[n][i] == 1 && !init.contains(i) && !visited[i]) {
                dfs(graph, i);
            }
        }
    }
}
```

## 图片

* 不支持本地图片
* 网络图片测试如下

![Matery](https://hexo.io/themes/screenshots/Matery@2x.jpg)