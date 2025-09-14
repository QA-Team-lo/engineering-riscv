## 在 RevyOS 上安装 BRL-CAD

BRL-CAD 没有 riscv64 二进制包提供，需要手动构建。

### 构建 BRL-CAD

安装依赖：

```shell
sudo apt update
sudo apt install -y build-essential make cmake sed byacc flex xsltproc libncursesw5-dev \
libfontconfig-dev xserver-xorg-dev libx11-dev libxi-dev
```

拉取源码：

```shell
wget https://github.com/BRL-CAD/brlcad/releases/download/rel-7-42-0/brlcad-7.42.0.tar.gz
tar xvf brlcad-7.42.0.tar.gz
cd brlcad-7.42.0
```

开始构建：

```shell
mkdir build; cd build
cmake .. -DBRLCAD_ENABLE_STRICT=NO -DBRLCAD_BUNDLED_LIBS=BUNDLED -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/home/debian/CAD/brl-cad -GNinja -Wno-dev -DBUILD_STATIC_LIBS=OFF -DBRLCAD_ENABLE_COMPILER_WARNINGS=OFF -DBRLCAD_FLAGS_DEBUG=OFF -DBRLCAD_GDAL=OFF -DBRLCAD_PNG=OFF -DBRLCAD_REGEX=OFF -DBRLCAD_ZLIB=OFF -DBRLCAD_ENABLE_OPENGL=ON -DBRLCAD_ENABLE_QT=OFF -DUSE_TCL=OFF -USE_TCL=OFF
ninja
```

### 构建失败处理（部分）

BRL-CAD 是一个非常大的项目，有众多外部依赖（位于 [bext](https://github.com/BRL-CAD/bext) 仓库中），其中有一部分没有 RISC-V 支持（x86/ARM 内联汇编），另有一部分在目前较新的 GCC 下无法编译通过。

以下记录了一部分尝试修复的 patch。由于时间有限，这些 patch 仍不足以在 RevyOS 上编译整个 BRL-CAD 项目，仅供参考。

目前会在编译 `ITK_BLD` 时失败。

#### mmesh

```patch
diff --git a/src/mmatomic-gnuc.h b/src/mmatomic-gnuc.h
index 6cf2228..57646fa 100644
--- a/src/mmatomic-gnuc.h
+++ b/src/mmatomic-gnuc.h
@@ -50,7 +50,7 @@ to the other resident thread, and so on
 
 #define MM_ATOMIC_SUPPORT (1)
 
-#if MMATOMIC_ARCH_AMD64 || MMATOMIC_ARCH_ARM64
+#if MMATOMIC_ARCH_AMD64 || MMATOMIC_ARCH_ARM64 || MMATOMIC_ARCH_RISCV64
  #define MM_ATOMIC_64_BITS_SUPPORT (1)
 #endif
 
diff --git a/src/mmatomic.h b/src/mmatomic.h
index 42ce4e9..90dcaf0 100644
--- a/src/mmatomic.h
+++ b/src/mmatomic.h
@@ -44,6 +44,9 @@
 #elif defined(__arm__) || defined(_ARM) || defined(_M_ARM) || defined(__arm)
  #define MMATOMIC_ARCH_ARM (1)
  #define MMATOMIC_POINTER_BITS (32)
+#elif defined(__riscv) && (__riscv_xlen == 64)
+ #define MMATOMIC_ARCH_RISCV64 (1)
+ #define MMATOMIC_POINTER_BITS (64)
 #elif defined(__EMSCRIPTEN__)
  #define MMATOMIC_ARCH_WEBASSEMBLY (1)
  #define MMATOMIC_POINTER_BITS (32)
```

#### tkhtml

```patch
diff --git a/src/htmlprop.c b/src/htmlprop.c
index 5821707..7498b01 100644
--- a/src/htmlprop.c
+++ b/src/htmlprop.c
@@ -1183,7 +1183,8 @@ propertyValuesSetColor (HtmlComputedValuesCreator *p, HtmlColor **pCVar, CssProp
     HtmlTree *pTree = p->pTree;
 
     if (pProp->eType == CSS_CONST_INHERIT) {
-        HtmlColor **pInherit = (HtmlColor **)getInheritPointer(p, pCVar);
+        //HtmlColor **pInherit = (HtmlColor **)getInheritPointer(p, pCVar);
+       HtmlColor **pInherit = (HtmlColor **)getInheritPointer(p, (unsigned char *)pCVar);
         assert(pInherit);
         cVal = *pInherit;
         goto setcolor_out;
@@ -1686,7 +1687,8 @@ propertyValuesSetSize (HtmlComputedValuesCreator *p, int *pIVal, unsigned int p_
         case CSS_CONST_INHERIT:
             if (allow_mask & SZ_INHERIT) {
                 HtmlNode *pParent = p->pParent;
-                int *pInherit = (int *)getInheritPointer(p, pIVal);
+                //int *pInherit = (int *)getInheritPointer(p, pIVal);
+               int *pInherit = (int *)getInheritPointer(p, (unsigned char *)pIVal);
                 assert(pInherit);
                 assert(pParent);
 
diff --git a/src/htmltable.c b/src/htmltable.c
index 010e378..bed89c0 100644
--- a/src/htmltable.c
+++ b/src/htmltable.c
@@ -1155,7 +1155,8 @@ rowGroupIterate (HtmlTree *pTree, HtmlNode *pNode, RowIterateContext *p)
             sRow.node.iNode = -1;
             sRow.nChild = jj - ii;
             sRow.apChildren = &((HtmlElementNode *)pNode)->apChildren[ii];
-            rowIterate(pTree, &sRow, p);
+            //rowIterate(pTree, &sRow, p);
+           rowIterate(pTree, (HtmlNode *)&sRow, p);
             assert(!sRow.pLayoutCache);
             ii = jj - 1;
         }
@@ -1281,7 +1282,8 @@ tableIterate (
             sRowGroup.node.iNode = -1;
             sRowGroup.nChild = jj - ii;
             sRowGroup.apChildren = &((HtmlElementNode *)pNode)->apChildren[ii];
-            rowGroupIterate(pTree, &sRowGroup, &sRowContext);
+            //rowGroupIterate(pTree, &sRowGroup, &sRowContext);
+           rowGroupIterate(pTree, (HtmlNode *)&sRowGroup, &sRowContext);
             assert(!sRowGroup.pLayoutCache);
             ii = jj - 1;
         }
diff --git a/src/htmltree.c b/src/htmltree.c
index 804c1ac..d60e3bf 100644
--- a/src/htmltree.c
+++ b/src/htmltree.c
@@ -3075,7 +3075,7 @@ fragmentAddElement (
             return;
     }
 
-    implicitCloseCount(pTree, pFragment->pCurrent, eType, &nClose);
+    implicitCloseCount(pTree, (HtmlNode *)pFragment->pCurrent, eType, &nClose);
     for (ii = 0; ii < nClose; ii++) {
         HtmlNode *pC = &pFragment->pCurrent->node;
         HtmlNode *pParentC = HtmlNodeParent(pC);
@@ -3105,7 +3105,7 @@ fragmentAddElement (
     pFragment->pCurrent = pElem;
 
     if (HtmlMarkup(eType)->flags & HTMLTAG_EMPTY) {
-        nodeHandlerCallbacks(pTree, pFragment->pCurrent);
+        nodeHandlerCallbacks(pTree, (HtmlNode *)pFragment->pCurrent);
         pFragment->pCurrent = (HtmlElementNode *)HtmlNodeParent(pElem);
     }
     if (!pFragment->pCurrent) {
@@ -3119,10 +3119,10 @@ fragmentAddClosingTag (HtmlTree *pTree, int eType, const char *zType, int iOffse
     int nClose;
     int ii;
     HtmlFragmentContext *p = pTree->pFragment;
-    explicitCloseCount(p->pCurrent, eType, zType, &nClose);
+    explicitCloseCount((HtmlNode *)p->pCurrent, eType, zType, &nClose);
     for (ii = 0; ii < nClose; ii++) {
         assert(p->pCurrent);
-        nodeHandlerCallbacks(pTree, p->pCurrent);
+        nodeHandlerCallbacks(pTree, (HtmlNode *)p->pCurrent);
         p->pCurrent = (HtmlElementNode *)HtmlNodeParent(p->pCurrent);
     }
     if (!p->pCurrent) {
@@ -3147,7 +3147,7 @@ HtmlParseFragment (HtmlTree *pTree, const char *zHtml)
 
     while (sContext.pCurrent) {
         HtmlNode *pParent = HtmlNodeParent(sContext.pCurrent); 
-        nodeHandlerCallbacks(pTree, sContext.pCurrent);
+        nodeHandlerCallbacks(pTree, (HtmlNode *)sContext.pCurrent);
         sContext.pCurrent = (HtmlElementNode *)pParent;
     }
```

### 参考资料

https://brl-cad.github.io/wiki/doc/Compiling/