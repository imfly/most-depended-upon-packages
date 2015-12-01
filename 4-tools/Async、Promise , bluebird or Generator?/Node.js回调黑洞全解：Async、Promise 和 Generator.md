Node.js回调黑洞全解：Async、Promise 和 Generator

https://strongloop.com/strongblog/node-js-callback-hell-promises-generators/
https://medium.com/@wavded/managing-node-js-callback-hell-1fe03ba8baf

著作权归作者所有。
商业转载请联系作者获得授权，非商业转载请注明出处。
作者：寸志
链接：http://zhuanlan.zhihu.com/FrontendMagazine/19750470
来源：知乎

译者按：最近一直在研究Generator。本身协程（yield）的引入并不是用来解决异步问题的。而异步回调的本质就是接受异步产生的结果，继续完成接下来的任务。而yield的next接口恰巧提供了这种便利，这也是Co实现的本质。而本文也是我一致在找寻的，深入浅出地介绍了现在流控制的几大形式。我们常常把这个问题叫做”回调黑洞”或”回调金字塔”：doAsync1(function () {
  doAsync2(function () {
    doAsync3(function () {
      doAsync4(function () {
    })
  })
})

回调黑洞是一种主观的叫法，就像嵌套太多的代码，有时候也没什么问题。为了控制调用顺序，异步代码变得非常复杂，这就是黑洞。有个问题非常合适衡量黑洞到底有多深：如果doAsync2发生在doAsync1之前，你要忍受多少重构的痛苦？目标不单单是减少嵌套层数，而是要编写模块化（可测试）的代码，便于理解和修改。
在本文中，我们要编写一个模块，使用一一系列的工具和类库来展示流控制是如何工作的。而且新版的Node带来了新的有潜力的解决方案，我们也会一探究竟。
问题
假设，我们需要找出某个目录中最大的文件。var findLargest = require('./findLargest')
findLargest('./path/to/dir', function (er, filename) {
  if (er) return console.error(er)
  console.log('largest file was:', filename)
})

我们分步解决这个问题：
读取给定文件夹中的全部文件获取每个文件的stats（状态）确定那个文件是最大的（如果多个文件都是最大的，选其中一个）将最大文件的文件名传给回调函数
任何环节只要发生错误，则调用回调函数，把错误传递给它。我们且仅且调用回调函数一次。
嵌套代码
第一种解决方案是嵌套的，看起来没什么恐怖的，并且逻辑还是看得懂的。var fs = require('fs')
var path = require('path')


module.exports = function (dir, cb) {
  fs.readdir(dir, function (er, files) { [1]
    if (er) return cb(er)
    var counter = files.length
    var errored = false
    var stats = []


    files.forEach(function (file, index) {
      fs.stat(path.join(dir,file), function (er, stat) { [2]
        if (errored) return
        if (er) {
          errored = true
          return cb(er)
        }
        stats[index] = stat [3]


        if (--counter == 0) { [4]
          var largest = stats
            .filter(function (stat) { return stat.isFile() }) [5]
            .reduce(function (prev, next) { [6]
              if (prev.size > next.size) return prev
              return next
            })
          cb(null, files[stats.indexOf(largest)]) [7]
        }
      })
    })
  })
}
读取目录中的所有文件读取每个文件的stats。读取的过程是并行的，因此我们使用一个counter，来追踪I/O结束。同时我们还是了一个布尔变量errored，来防止多次出错导致多次调用回调函数（cb）收集每个文件的stats。注意我们在这里设置了一个并行的数组（从files到stats）检查并行调用都完成了过滤出普通文件（不包含链接、目录等）Reduce整个列表，获取最大的文件根据stat获取文件名，调用回调函数
这个解决方案还不错，然而，它运用了某些技巧来管理并行处理，以及避免多次调用回调函数。我们之后再来处理这些问题，首先先把这段代码拆分成更小的模块吧。
模块化
嵌套代码的方案可以拆分成三个模块单元：
从目录中读取所有文件从这些文件中获取stats处理stats和files，获取最大的文件
第一模块其实就是fs.readdir，我们就不为其新写一个函数了。不过，我们可以写一个函数，给定一系列的文件路径，返回这些文件的stat：function getStats (paths, cb) {
  var counter = paths.length
  var errored = false
  var stats = []
  paths.forEach(function (path, index) {
    fs.stat(path, function (er, stat) {
      if (errored) return
      if (er) {
        errored = true
        return cb(er)
      }
      stats[index] = stat
      if (--counter == 0) cb(null, stats)
    })
  })
}

接下来，我们需要一个处理函数，对比stats和files，返回最大文件的文件名：function getLargestFile (files, stats) {
  var largest = stats
    .filter(function (stat) { return stat.isFile() })
    .reduce(function (prev, next) {
      if (prev.size > next.size) return prev
      return next
    })
    return files[stats.indexOf(largest)]
}

组合到一起：var fs = require('fs')
var path = require('path')


module.exports = function (dir, cb) {
  fs.readdir(dir, function (er, files) {
    if (er) return cb(er)
    var paths = files.map(function (file) { [1]
      return path.join(dir,file)
    })


    getStats(paths, function (er, stats) {
      if (er) return cb(er)
      var largestFile = getLargestFile(files, stats)
      cb(null, largestFile)
    })
  })
}

模块化的解决方案使得代码重用和测试更容易。核心的export方法也更容易理解。然而，我们还是手动管理并行的stat任务。让我试试使用一些流控制的类库，看看我们可以做些什么？
Asyncasync模块很流行，与Node的核心精神类似。我们来看看如何使用async来重构代码：var fs = require('fs')
var async = require('async')
var path = require('path')


module.exports = function (dir, cb) {
  async.waterfall([ [1]
    function (next) {
      fs.readdir(dir, next)
    },
    function (files, next) {
      var paths = 
       files.map(function (file) { return path.join(dir,file) })
      async.map(paths, fs.stat, function (er, stats) { [2]
        next(er, files, stats)
      })
    },
    function (files, stats, next) {
      var largest = stats
        .filter(function (stat) { return stat.isFile() })
        .reduce(function (prev, next) {
        if (prev.size > next.size) return prev
          return next
        })
        next(null, files[stats.indexOf(largest)])
    }
  ], cb) [3]
}
async.waterfall提供瀑布式的流控制。每个操作产生的数据可以传递给下一个函数，通过next这个回调函数async.map允许我们并行对一系列的path调用fs.stat，并把一个结果数组传递给回调函数最后一步后会调用回调函数cb；如果在整个运行过程中出错了也会调用cb，不会它只会被调用一次
async模块保证回调只会被触发一次。它同时还为我们处理了错误，管理并行的任务。
PromisePromise提供了错误处理和函数式编程。我们如何使用Promise来解决这个问题呢？让我们使用Q模块试试（当然也可以使用其他Promise类库）：var fs = require('fs')
var path = require('path')
var Q = require('q')
var fs_readdir = Q.denodeify(fs.readdir) [1]
var fs_stat = Q.denodeify(fs.stat)


module.exports = function (dir) {
  return fs_readdir(dir)
    .then(function (files) {
      var promises = files.map(function (file) {
        return fs_stat(path.join(dir,file))
      })
      return Q.all(promises).then(function (stats) { [2]
        return [files, stats] [3]
      })
    })
    .then(function (data) { [4]      var files = data[0]
      var stats = data[1]      var largest = stats
        .filter(function (stat) { return stat.isFile() })
        .reduce(function (prev, next) {
        if (prev.size > next.size) return prev
          return next
        })
      return files[stats.indexOf(largest)]
    })
}
Node内核并不是promise化的，转化之Q.all同步执行所有stat，结果数组保持了原来的顺序最后把files和stat传递给next函数
与之前的例子不一样，promise链中抛出的错误会被处理。而且暴露出来的API也是Promise化的：var findLargest = require('./findLargest')
findLargest('./path/to/dir')
  .then(function (er, filename) {
    console.log('largest file was:', filename)
  })
  .catch(console.error)

尽管上面是这样设计的，但是你也没必要暴露promise化的接口。很多promise类库也提供了暴露node风格接口的方法。在Q中，我们可以使用nodeify函数来实现。
我们不会深入介绍Promise，如果大家想了解更多的话，阅读这篇文章。
Generator
本文一开始就说的，在这个领域来了一个新技术，在Node 0.11.2及以上版本中已经可以使用了——Generator！
Generator是为JavaScript设计的一种轻量级的协程。它通过yield关键字，可以控制一个函数暂停或者继续执行Generator函数有一个特别的语法function* ()，借助这种超能力，我们还可以暂停或者继续执行异步操作，使用像promise或者”thunks”这样的结构来写出看起来像同步的代码。Thunk函数是一种这样的函数，返回一个回调来调用它自己。回调函数与典型的node回调函数有着一致的参数（例如error是第一个参数）。阅读这里了解更多。
让我看一个例子，如何使用Generator来做异步的控制： TJ Holowaychuk的类库co。下面是我们寻找最大文件的程序：var co = require('co')
var thunkify = require('thunkify')
var fs = require('fs')
var path = require('path')
var readdir = thunkify(fs.readdir) [1]
var stat = thunkify(fs.stat)


module.exports = co(function* (dir) { [2]  var files = yield readdir(dir) [3]
  var stats = yield files.map(function (file) { [4]
    return stat(path.join(dir,file))
  })
  var largest = stats
    .filter(function (stat) { return stat.isFile() })
    .reduce(function (prev, next) {
      if (prev.size > next.size) return prev
      return next
    })
  return files[stats.indexOf(largest)] [5]
})
既然Node核心函数并不是thunk化的，我们thunk之co接受一个Generator函数，在这个函数内部，可以使用yield关键字在任何地方暂停Generator函数直到readdir返回了才继续执行。结果赋值给files变量co还能够处理一系列并行的操作数组，结果按顺序保存到stats这个数组中返回最终结果
我们可以把这个Generator函数包装成一样的回调API，就像本文一开始的那样。Co还可以把任何的报错返回给回调函数。在Generator中，可以使用try/catch来包裹yield语句，co利用了这一点：try {
  var files = yield readdir(dir)
} catch (er) {
  console.error('something happened whilst reading the directory')
}
Co还能非常优雅地支持数组、对象、嵌套Generator和Promise等等。 

也涌现出了一些其他Generator模块。Q模块报了一个优雅的Q.async方法，行为与co使用Generator一致。
总结
在本文中，我们研究了很多种不同的方案，来处理回调黑洞。其实也就是程序的流程控制。我个人对Generator的方案非常感兴趣。我很好奇像koa这样的新框架可以带来什么。
想要本文使用到的代码示例以及其他一些Generator例子？这里有一个Github repo可以满足你！本文首发于StrongLoop |   Managing Node.js Callback Hell with Promises, Generators and Other Approaches。
原文：Managing Node.js Callback Hell