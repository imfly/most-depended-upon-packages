我用Lo-Dash替换Underscore已经有一段时间了。Lo-Dash更快，支持AMD，并且拥有Underscore所缺乏的特性。同时，Lo-Dash和Underscore是100%兼容的，所以如果你使用依赖Underscore的库（例如Backbone），或者你现有的代码已经使用了Underscore，那么只需改用Lo-Dash，立刻就能获得性能上的优势。这真是太棒了！



lazy.js同样声称“类似Underscore，但是使用惰性求值”，并发布了一些令人印象深刻的速度比较。这已经足够引起人们的兴趣了，而且lazy.js看上去就是一个有趣的库。但是我想提醒大家注意一些事实。

让我们看看lazy.js上的第一个速度比较的图片：



Lazy.js的速度相当地令人印象深刻，但是你也应该注意到 Lo-Dash 同样比 Underscore 高得多。 大概是四五倍，甚至更多。现在看看左边的数据，那是每秒能完成的操作。这些柱状图形很小，但是它们代表的数字可是相当巨大！那是每秒几十万次操作和每秒几百万次操作的差距。

是的，Lazy.js要快得多，但是Lo-Dash也比Underscore要快得多，而且最重要的是 Lo-Dash 可以直接替换 Underscore，不会带来兼容性问题。

没有什么值得争论的。如果你使用Underscore，不管在哪里使用（包括 Node.js），你应该花上几分钟切换到 Lo-Dash。没有任何理由不这么做。相反，有很多非常好的理由让你这么做（最主要的是速度的提升）。这些好处唾手可得。

呃，我是不是忘了说了，Lo-Dash以后也会有惰性求值。