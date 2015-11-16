# Top1 - lodash 

截至当前，Npm正式版本是 v3.10.1，这里简介的是v4.0.0-pre版本。

## 简介
这是排在第一位的js工具包，每天下载量达到30多万。

可以使用 [lodash-cli](https://www.npmjs.com/package/lodash-cli)生成你需要的包文件:
```bash
$ lodash modularize modern exports=node -o ./
$ lodash modern -d -o ./index.js
```

## 安装

使用npm:

```bash
$ {sudo -H} npm i -g npm
$ npm i --save lodash
```

在 Node.js/io.js 中使用:

```js
// 加载整个模块
var _ = require('lodash');
// 或者 仅加载某个方法模块
var array = require('lodash/array');
// 还可以更细、更小
var chunk = require('lodash/array/chunk');
```

更多细节，请查看 [源代码](https://github.com/lodash/lodash/tree/3.10.1-npm).

**注意:**<br>
在REPL不要赋值给[专用变量](http://nodejs.org/api/repl.html#repl_repl_features) `_`.<br>
可以为默认包含lodash的REPL安装 [n_](https://www.npmjs.com/package/n_) 

## 模块格式

lodash也可用于各种其他的构建或模块格式

 * npm包用于[现行包](https://www.npmjs.com/package/lodash), [兼容包](https://www.npmjs.com/package/lodash-compat), 以及 [每个方法](https://www.npmjs.com/browse/keyword/lodash-modularized) 构建
 * AMD模块用于 [AMD现行包](https://github.com/lodash/lodash/tree/3.10.1-amd) 和 [兼容](https://github.com/lodash/lodash-compat/tree/3.10.1-amd) 构建
 * ES模块用于 [ES现行包](https://github.com/lodash/lodash/tree/3.10.1-es)

## 特点

 * 发布遵从 [语义化版本](http://semver.org/) 
 * [延迟执行](http://filimanjaro.com/blog/2014/introducing-lazy-evaluation/) 链
 * [_(…)](https://lodash.com/docs#_) 支持隐式链
 * [_.ary](https://lodash.com/docs#ary) 和 [_.rearg](https://lodash.com/docs#rearg) 改变函数参数限制和顺序
 * [_.at](https://lodash.com/docs#at) 挑选（cherry-picking）集合值
 * [_.attempt](https://lodash.com/docs#attempt) 执行那些没有try-catch可能发生错误的函数
 * [_.before](https://lodash.com/docs#before) 补充 [_.after](https://lodash.com/docs#after)
 * [_.bindKey](https://lodash.com/docs#bindKey) 用于绑定 [*“延迟”*](http://michaux.ca/articles/lazy-function-definition-pattern) 定义的方法
 * [_.chunk](https://lodash.com/docs#chunk) 用于将一个数组分割成给定大小的块
 * [_.clone](https://lodash.com/docs#clone) 支持浅克隆“日期”和“正则表达式”的对象
 * [_.cloneDeep](https://lodash.com/docs#cloneDeep) 用于深度克隆数组和对象
 * [_.curry](https://lodash.com/docs#curry) 和 [_.curryRight](https://lodash.com/docs#curryRight) 用于创建 [curried](http://hughfdjackson.com/javascript/why-curry-helps/) （混合吗？怎么理解）函数
 * [_.debounce](https://lodash.com/docs#debounce) 和 [_.throttle](https://lodash.com/docs#throttle) 可取消和接受选项用于更多控制
 * [_.defaultsDeep](https://lodash.com/docs#defaultsDeep) 递归分配默认属性
 * [_.fill](https://lodash.com/docs#fill) 用值填充数组
 * [_.findKey](https://lodash.com/docs#findKey) 用于查找keys
 * [_.flow](https://lodash.com/docs#flow) 补充 [_.flowRight](https://lodash.com/docs#flowRight) (a.k.a `_.compose`)
 * [_.forEach](https://lodash.com/docs#forEach) 支持提前退出（supports exiting early）
 * [_.forIn](https://lodash.com/docs#forIn) 迭代可列举的所有属性
 * [_.forOwn](https://lodash.com/docs#forOwn) 迭代自己的属性
 * [_.get](https://lodash.com/docs#get) 和 [_.set](https://lodash.com/docs#set) 深度属性获取和设置
 * [_.gt](https://lodash.com/docs#gt), [_.gte](https://lodash.com/docs#gte), [_.lt](https://lodash.com/docs#lt), 和 [_.lte](https://lodash.com/docs#lte) 关联方法
 * [_.inRange](https://lodash.com/docs#inRange) 检查一个数是否在给定范围内
 * [_.isNative](https://lodash.com/docs#isNative) 检查是否原生函数
 * [_.isPlainObject](https://lodash.com/docs#isPlainObject) 和 [_.toPlainObject](https://lodash.com/docs#toPlainObject) 检查和转化为 `Object` 对象
 * [_.isTypedArray](https://lodash.com/docs#isTypedArray) 检查是否类型数组
 * [_.mapKeys](https://lodash.com/docs#mapKeys) 映射keys到一个对象
 * [_.matches](https://lodash.com/docs#matches) 支持深度对象比较
 * [_.matchesProperty](https://lodash.com/docs#matchesProperty) 补充 [_.matches](https://lodash.com/docs#matches) 和 [_.property](https://lodash.com/docs#property)
 * [_.merge](https://lodash.com/docs#merge) 深入（扩展） [_.extend](https://lodash.com/docs#extend)
 * [_.method](https://lodash.com/docs#method) 和 [_.methodOf](https://lodash.com/docs#methodOf) 创建调用方法的函数（译注：将方法转为函数？）
 * [_.modArgs](https://lodash.com/docs#modArgs) 用于更高级的功能组件
 * [_.parseInt](https://lodash.com/docs#parseInt) 用于保持交叉环境行为一致性
 * [_.pull](https://lodash.com/docs#pull), [_.pullAt](https://lodash.com/docs#pullAt), 和 [_.remove](https://lodash.com/docs#remove) 用于改变数组
 * [_.random](https://lodash.com/docs#random) 支持返回浮点数
 * [_.restParam](https://lodash.com/docs#restParam) 和 [_.spread](https://lodash.com/docs#spread) 用于使用其余参数和传递参数给函数
 * [_.runInContext](https://lodash.com/docs#runInContext) 用于无冲突混合和更容易模拟
 * [_.slice](https://lodash.com/docs#slice) 创建类数组值的子集
 * [_.sortByAll](https://lodash.com/docs#sortByAll) 和 [_.sortByOrder](https://lodash.com/docs#sortByOrder) 根据多个属性和顺序排序
 * [_.support](https://lodash.com/docs#support) 用于标志环境特点
 * [_.template](https://lodash.com/docs#template) 支持 [*“导入”*](https://lodash.com/docs#templateSettings-imports) 选项 和 [ES模板分隔符](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-template-literal-lexical-components)
 * [_.transform](https://lodash.com/docs#transform) 作为一个强大的替代方案，替代 [_.reduce](https://lodash.com/docs#reduce) 用于转换对象
 * [_.unzipWith](https://lodash.com/docs#unzipWith) 和 [_.zipWith](https://lodash.com/docs#zipWith) 指定应该组合值（应当被组合的）
 * [_.valuesIn](https://lodash.com/docs#valuesIn) 获得所有可列举属性的值
 * [_.xor](https://lodash.com/docs#xor) 补充 [_.difference](https://lodash.com/docs#difference), [_.intersection](https://lodash.com/docs#intersection), 和 [_.union](https://lodash.com/docs#union)
 * [_.add（加）](https://lodash.com/docs#add), [_.round（四舍五入）](https://lodash.com/docs#round), [_.sum（求和）](https://lodash.com/docs#sum), 及
   [更多](https://lodash.com/docs "_.ceil 和 _.floor") 数学方法
 * [_.bind](https://lodash.com/docs#bind), [_.curry](https://lodash.com/docs#curry), [_.partial](https://lodash.com/docs#partial), 及
   [更多](https://lodash.com/docs "_.bindKey, _.curryRight, _.partialRight") 支持可定制的参数占位符
 * [_.capitalize](https://lodash.com/docs#capitalize), [_.trim](https://lodash.com/docs#trim), 及
   [更多](https://lodash.com/docs "_.camelCase, _.deburr, _.endsWith, _.escapeRegExp, _.kebabCase, _.pad, _.padLeft, _.padRight, _.repeat, _.snakeCase, _.startCase, _.startsWith, _.trimLeft, _.trimRight, _.trunc, _.words") 字符串方法
 * [_.clone](https://lodash.com/docs#clone), [_.isEqual](https://lodash.com/docs#isEqual), 及
   [更多](https://lodash.com/docs "_.assign, _.cloneDeep, _.merge") 接收更多回调
 * [_.dropWhile](https://lodash.com/docs#dropWhile), [_.takeWhile](https://lodash.com/docs#takeWhile), 及
   [更多](https://lodash.com/docs "_.drop, _.dropRight, _.dropRightWhile, _.take, _.takeRight, _.takeRightWhile") 补充 [_.first](https://lodash.com/docs#first), [_.initial](https://lodash.com/docs#initial), [_.last](https://lodash.com/docs#last), 和 [_.rest](https://lodash.com/docs#rest)
 * [_.findLast](https://lodash.com/docs#findLast), [_.findLastKey](https://lodash.com/docs#findLastKey), 及
   [更多](https://lodash.com/docs "_.curryRight, _.dropRight, _.dropRightWhile, _.flowRight, _.forEachRight, _.forInRight, _.forOwnRight, _.padRight, partialRight, _.takeRight, _.trimRight, _.takeRightWhile") 右结合方法
 * [_.includes](https://lodash.com/docs#includes), [_.toArray](https://lodash.com/docs#toArray), 及
   [更多](https://lodash.com/docs "_.at, _.countBy, _.every, _.filter, _.find, _.findLast, _.findWhere, _.forEach, _.forEachRight, _.groupBy, _.indexBy, _.invoke, _.map, _.max, _.min, _.partition, _.pluck, _.reduce, _.reduceRight, _.reject, _.shuffle, _.size, _.some, _.sortBy, _.sortByAll, _.sortByOrder, _.sum, _.where") 接收字符串
 * [_#commit](https://lodash.com/docs#prototype-commit) 和 [_#plant](https://lodash.com/docs#prototype-plant) 处理链序列
 * [_#thru](https://lodash.com/docs#thru) 通过一个链序列传值
