---
title: FIREFOX 和 chrome中 如何 禁止鼠标拖拽图片
tags: 小百科
categories: 前端技术
abbrlink: 15497
date: 2016-05-14 00:01:58
---

##三种办法
```html
<script language="javascript">
    document.images[i].ondragstart=function (){return false;};  
    
    e.preventDefault(); 
    
    document.ondragstart=function() {return false;}
</script>
```

我用过第三个 ondragstart 这个好使
