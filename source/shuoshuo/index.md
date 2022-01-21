---
title:  碎碎 #为了标题中出现，可以不填
type: "shuoshuo" #必须有
comments: false # 是否再下方显示评论栏
---

<!-- 引用 artitalk -->
<script type="text/javascript" src="https://unpkg.com/artitalk"></script>
<!-- 存放说说的容器 -->
<div id="artitalk_main"></div>
<script>
new Artitalk({
    appId: 'blszhnruw5lJSvkUXt2icpcK-MdYXbMMI', // Your LeanCloud appId
    appKey: 'hRUegQRKN8nkH3Bc1wdlDvzm', // Your LeanCloud appKey
    pageSize: 20, //每页评论数量
    shuoPla: '', //评论框里显示，可以不填
    motion: 0, //加载动画的开关 0（关闭），1（开启）
    atComment: 0, //评论功能的开关 0（关闭），1（开启）
    bgImg: '', //评论框里的背景，可以不填
    color1: 'linear-gradient(45deg, rgb(109, 208, 242) 15%, rgb(245, 154, 190) 85%)',
    color2: 'linear-gradient(45deg, rgb(109, 208, 242) 15%, rgb(245, 154, 190) 85%)',
    color3: 'black',
})
</script>