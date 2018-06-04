# git tag的使用


## 查询所有标签

输入：
```bash
git tag
```

输出：
```bash
0.10.0-rc
0.11.0
0.11.0-rc
```


## 查询符合指定格式的标签

输入：
```bash
git tag -l 'v0.10.*'
```

输出：
```bash
v0.10.0
v0.10.1
```


## 查看标签的版本信息

输入：
```bash
git show v0.10.1
```

输出：
```bash
commit 12f1ca2b93176e81c5f208437e1a6af745dd7da9 (tag: v0.10.1)
Author: Evan You <yyx990803@gmail.com>
Date:   Mon Mar 24 04:11:38 2014 -0400

    Release-v0.10.1

diff --git a/bower.json b/bower.json
index 0ba7d7c9..9004f16d 100644
--- a/bower.json
+++ b/bower.json
@@ -1,6 +1,6 @@
 {
     "name": "vue",
-    "version": "0.10.0",
+    "version": "0.10.1",
     "main": "dist/vue.js",
     "description": "Simple, Fast & Composable MVVM for building interative interfaces",
     "authors": ["Evan You <yyx990803@gmail.com>"],
```


## 创建轻量分支

```bash
git tag v0.10.2.light
```


## 创建带备注信息的标签

```bash
git tag -a v0.10.2 -m "v0.10.2版"
```


## 给指定的commit记录打标签

```bash
git tag -a v0.10.2 9fbc3d0
```


## 提交指定tag到仓库

```bash
git push origin v0.10.2
```


## 提交所有tags到仓库

```bash
git push origin -tags
```


## 切换到指定tag

```bash
git checkout [tagname]
```


## 删除本地指定tag

```bash
git tag -d v0.1.2
```


## 删除远程指定tag

```bash
git push origin :refs/tags/v1.01
```


## 参考
- [git tag常用操作速查](https://blog.csdn.net/happycxz/article/details/78893690)
