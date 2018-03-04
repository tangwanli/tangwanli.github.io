# 关于这个主题的一些修改方法

修改demo需要修改js文件夹中waterfall.js文件。
在demoCountent那个数组里面修改对象。

修改collections需要修改page文件中3collections.md

修改about需要修改page文件中4about.md

添加文章需要向_posts文件夹中添加.md文件

## 修改页面的footer

修改页面的footer要修改_includes文件夹中的footer.html。而里面的site对应的就是_config.yml,所以直接用site.github_username就可以访问_config.yml中的github_username属性

## 修改archives

修改这个页面右边的content，只需要在0archives.html文件中继续添加div，而添加上的div会显示在右边content那一栏，不过不是像2017这样的动态时间数据，而是像content这样的固定数据