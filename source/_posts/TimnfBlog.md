---
title: Timnf 的个人博客
date: 2024-02-27
tags: 个人
categories: 个人
---
# Welcome to [Timnf](https://Timnf.github.io/) 

# 博客构建参考

[参考博客](https://happyseashell.gitee.io/overview/)

<!--more-->
# 流程图和导图等绘制

```bash
npm install hexo-mermaid --save
npm install hexo-markmap --save
```

<!-- 流程图 -->
{% mermaid graph TD %}
A[Hard] -->|Text| B(Round)
B --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
{% endmermaid %}


<!-- 甘特图 -->
{% mermaid gantt %}
dateFormat  YYYY-MM-DD
section Section
Completed :done,    des1, 2014-01-06,2014-01-08
Active        :active,  des2, 2014-01-07, 3d
Parallel 1   :         des3, after des1, 1d
Parallel 2   :         des4, after des1, 1d
Parallel 3   :         des5, after des3, 1d
Parallel 4   :         des6, after des4, 1d
{% endmermaid %}


<!-- 数据帧格式图 -->
{% mermaid %}
graph LR
    Start((Start)) --> Header
    Header --> Payload
    Payload --> End((End))
{% endmermaid %}

<!-- 数据帧格式图 -->
{% mermaid %}
graph TD
    Start((Start)) --> Header
    Header --> Payload
    Payload --> End((End))
    Header --> Control
    Control -->|CRC| End
{% endmermaid %}





{% markmap 300px %}
# hexo-markmap
## 支持
### 无序列表
- 无序列表1
- 无序列表2
  - 无序列表嵌套
### 有序列表
1. 有序列表1
2. 有序列表2
### 行内公式
- $\epsilon_0$ 
- $O\left(f_{t}\left(X_i;\theta\right)\right)=\gamma T+\frac{1}{2}\lambda\|w\|^{2}$
### 引用
> 这是引用
## 不支持
- 超链接
- 代码块
- 内嵌样式（粗体、斜体等）
- 多行文字
- 公式收回后不能再次渲染

{% endmarkmap %}




