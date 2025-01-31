## 
## 1. 索引是什么？
**索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分**)，它们包含着对数据表里所有记录的引用指针。
索引是一种数据结构。数据库索引，是数据库管理系统中一个排序的数据结构，以协助快速查询、更新数据库表中数据。**索引的实现通常使用B树及其变种B+树**。更通俗的说，索引是表的目录，在查找内容之前可以先在目录中查找索引位置，以此快速定位查询数据。对于索引，会保存在额外的文件中，它是要占据物理空间的。
MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。比如我们在查字典的时候，前面都有检索的拼音和偏旁、笔画等，然后找到对应字典页码，这样然后就打开字典的页数就可以知道我们要搜索的某一个key的全部值的信息了。
## 2. 索引有哪些优缺点？
**索引的优点   **

- 当表中的数据量越来越大时，索引对于性能的影响愈发重要。索引能够轻易将查询性能提高好几个数量级，总的来说就是**可以明显的提高查询效率**。
- 可以大大加快数据的检索速度，这也是创建索引的最主要的原因。
- 通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。
- 通过创建唯一索引
- 加快表与表之间的连接

**索引的缺点**

- 时间方面：创建索引和维护索引要耗费时间，具体地，当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，会降低增/改/删的执行效率；
- 空间方面：索引需要占物理空间。
## 3. 哪些列上适合创建索引？创建索引有哪些开销？
经常需要作为条件查询的列上适合创建索引，并且该列上也必须有一定的区分度。
创建索引需要维护，在插入数据的时候会重新维护各个索引树（数据页的分裂与合并），对性能造成影响
## 4. 索引这么多优点，为什么不对表中的每一个列创建一个索引呢？

1. 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。
2. 索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。
3. 创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。
## 5. MySQL有哪几种索引类型？
1、从**存储结构**上来划分：BTree索引（B-Tree或B+Tree索引），Hash索引，full-index全文索引，R-Tree索引。这里所描述的是索引存储时保存的形式，
2、从**应用层次**来分：普通索引，唯一索引，复合索引。

- **普通索引**：即一个索引只包含单个列，一个表可以有多个单列索引
- **唯一索引**：索引列的值必须唯一，但允许有空值
- **复合索引**：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并

3、根据中数据的**物理实现方式**与键值的逻辑（索引）顺序关系： 聚集索引，非聚集索引。

- **聚簇索引(聚集索引)**：并不是一种单独的索引类型，而是一种数据存储方式。具体细节取决于不同的实现，InnoDB的聚簇索引其实就是在同一个结构中保存了B-Tree索引(技术上来说是B+Tree)和数据行。
- **非聚簇索引**： 不是聚簇索引，就是非聚簇索引

4、按照 **作用字段个数** 进行划分，分成**单列索引**和**联合索引**。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1654697146401-9fbdcba9-6f7a-48a6-af29-8630261a7924.png#averageHue=%23fefdfa&clientId=ue6a0313f-d066-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=228&id=ue6d957c5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=228&originWidth=702&originalType=binary&ratio=1&rotation=0&showTitle=false&size=145928&status=done&style=none&taskId=uc6ba4bd3-abc4-4156-840f-25d1f808efc&title=&width=702)
## 6. 说一说索引的底层实现？
**Hash索引**
基于哈希表实现，只有精确匹配索引所有列的查询才有效，对于每一行数据，存储引擎都会对所有的索引列计算一个哈希码（hash code），并且Hash索引将所有的哈希码存储在索引中，同时在索引表中保存指向每个数据行的指针。
图片来源：[https://www.javazhiyin.com/40232.html](https://www.javazhiyin.com/40232.html)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1647160383378-6c201811-6d89-4473-8efb-069394f2cb22.png#averageHue=%23f7f7f6&clientId=u3111ec8c-f05a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u4a7f58be&margin=%5Bobject%20Object%5D&name=image.png&originHeight=396&originWidth=933&originalType=url&ratio=1&rotation=0&showTitle=false&size=167578&status=done&style=none&taskId=u444d6a44-bf8c-4026-9ebc-80a67788ae4&title=)
**B-Tree索引**（MySQL使用B+Tree）
B-Tree能加快数据的访问速度，因为存储引擎不再需要进行全表扫描来获取数据，数据分布在各个节点之中。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1647160383288-5eec1eea-dc3d-4d47-be24-6b3f1db14785.png#averageHue=%23f9f9f9&clientId=u3111ec8c-f05a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u20bf5ec2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=338&originWidth=753&originalType=url&ratio=1&rotation=0&showTitle=false&size=50903&status=done&style=none&taskId=uab8652e0-a617-4c65-a054-c2aeba54c4c&title=)
**B+Tree索引**
是B-Tree的改进版本，同时也是数据库索引索引所采用的存储结构。数据都在叶子节点上，并且增加了顺序访问指针，每个叶子节点都指向相邻的叶子节点的地址。相比B-Tree来说，进行范围查找时只需要查找两个节点，进行遍历即可。而B-Tree需要获取所有节点，相比之下B+Tree效率更高。
B+tree性质： 

- n棵子tree的节点包含n个关键字，不用来保存数据而是保存数据的索引。
- 所有的叶子结点中包含了全部关键字的信息，及指向含这些关键字记录的指针，且叶子结点本身依关键字的大小自小而大顺序链接。
- 所有的非终端结点可以看成是索引部分，结点中仅含其子树中的最大（或最小）关键字。
- B+ 树中，数据对象的插入和删除仅在叶节点上进行。
- B+树有2个头指针，一个是树的根节点，一个是最小关键码的叶节点。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1647160383306-24a883eb-838d-4dd3-9bfe-d3b7c73fa3e8.png#averageHue=%23f7f7f7&clientId=u3111ec8c-f05a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u32a7c857&margin=%5Bobject%20Object%5D&name=image.png&originHeight=359&originWidth=780&originalType=url&ratio=1&rotation=0&showTitle=false&size=63304&status=done&style=none&taskId=u6c3a8d80-0b06-4dee-8b28-b2f5cf3e52b&title=)
## 7. 为什么索引结构默认使用B+Tree，而不是B-Tree，Hash，二叉树，红黑树？
**B-tree**：因为B树不管叶子节点还是非叶子节点，都会保存数据，这样导致在非叶子节点中能保存的指针数量变少（有些资料也称为扇出），指针少的情况下要保存大量数据，只能增加树的高度，导致IO操作变多，查询性能变低；
**B+tree**： 从两个方面来回答

- B+树的磁盘读写代价更低：B+树的内部节点并没有指向关键字具体信息的指针，因此其内部节点相对B树更小，如果把所有同一内部节点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，相对IO读写次数就降低了。
- 由于B+树的数据都存储在叶子结点中，分支结点均为索引，方便扫库，只需要扫一遍叶子结点即可，但是B树因为其分支结点同样存储着数据，我们要找到具体的数据，需要进行一次中序遍历按序来扫，所以B+树更加适合在区间查询的情况，所以通常B+树用于数据库索引。

**Hash**： 

- 虽然可以快速定位，但是没有顺序，IO复杂度高；
- 基于Hash表实现，只有Memory存储引擎显式支持哈希索引 ；
- 适合**等值查询**，如=、in()、<=>，不支持范围查询 ；
- 因为不是按照索引值顺序存储的，就不能像B+Tree索引一样利用索引完成排序 ；
- Hash索引在查询等值时非常快 ；
- 因为Hash索引始终索引的**所有列的全部内容**，所以不支持部分索引列的匹配查找 ；
- 如果有大量重复键值得情况下，哈希索引的效率会很低，因为存在哈希碰撞问题 。

**二叉树**： 树的高度不均匀，不能自平衡，查找效率跟数据有关（树的高度），并且IO代价高。
**红黑树**： 树的高度随着数据量增加而增加，IO代价高。
**不使用平衡二叉树的原因如下**：
最大原因：深度太大(因为一个节点最多只有2个子节点)，一次查询需要的I/O复杂度为O(lgN),而b+tree只需要O(log_mN),而其出度m非常大，其深度一般不会超过4 平衡二叉树逻辑上很近的父子节点，物理上可能很远，无法充分发挥磁盘顺序读和预读的高效特性。
## 8. MyISAM和InnoDB实现BTree索引方式的区别
### 1）MyISAM
B+Tree叶节点的data域存放的是数据记录的地址。在索引检索的时候，首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其 data 域的值，然后以 data 域的值为地址读取相应的数据记录。这被称为“非聚簇索引”。 索引文件和数据文件是分离的
### 2）InnoDB

- InnoDB 的 B+Tree 索引分为主索引（聚集索引）和辅助索引(非聚集索引)。一张表一定包含一个聚集索引构成的 B+ 树以及若干辅助索引的构成的 B+ 树。
- 辅助索引的存在并不会影响聚集索引，因为聚集索引构成的 B+ 树是数据实际存储的形式，而辅助索引只用于加速数据的查找，所以一张表上往往有多个辅助索引以此来提升数据库的性能。
- 就很容易明白为什么不建议使用过长的字段作为主键，因为所有辅助索引都引用主索引，过长的主索引会令辅助索引变得过大。再例如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一颗B+Tree，非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用自增字段作为主键则是一个很好的选择。

## 9. 主键索引和非主键索引
|  | **TYPE** | **INDEX** |
| --- | --- | --- |
| id | int | id(primary key) |
| k | int | k |
| name | varchar |  |

假设有如上表结构, 那么建立起的索引结构如下图
![image-20220304144720568.png](https://cdn.nlark.com/yuque/0/2022/png/21380271/1646468413069-f47294ff-f9e9-494d-ae20-f7e6fb44de8c.png#clientId=ub63f7278-093b-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=rhgRE&margin=%5Bobject%20Object%5D&name=image-20220304144720568.png&originHeight=321&originWidth=669&originalType=binary&ratio=1&rotation=0&showTitle=false&size=121499&status=done&style=none&taskId=u87ddf3d4-a455-48c3-9d9f-42b6d34bb8d&title=)
从图中看出, 根据叶子节点内容的不同, 索引类型分为主键索引和非主键索引.

- **主键索引**的叶子节点存储的是整行数据. 在InnoDB里, 主键索引也被称为**聚簇索引**
- **非主键索引**的叶子节点存储的是主键的值. 在InnoDB里, 非主键索引也被称为**二级索引 / 非聚簇索引**
## 10. 讲一讲聚簇索引与非聚簇索引？
在 InnoDB 里，索引B+ Tree的叶子节点存储了整行数据的是主键索引，也被称之为聚簇索引，即将数据存储与索引放到了一块，找到索引也就找到了数据。
而索引B+ Tree的叶子节点存储了主键的值的是非主键索引，也被称之为非聚簇索引、二级索引。
聚簇索引与非聚簇索引的区别：

- 非聚集索引与聚集索引的区别在于非聚集索引的叶子节点不存储表中的数据，而是存储该列对应的主键（行号） 
- 对于InnoDB来说，想要查找数据我们还需要根据主键再去聚集索引中进行查找，这个再根据聚集索引查找数据的过程，我们称为**回表**。第一次索引一般是顺序IO，回表的操作属于随机IO。需要回表的次数越多，即随机IO次数越多，我们就越倾向于使用全表扫描 。
- 通常情况下， **主键索引（聚簇索引）查询只会查一次**，而**非主键索引（非聚簇索引）需要回表查询多次**。当然，如果是覆盖索引的话，查一次即可 
- 注意：MyISAM无论主键索引还是二级索引都是非聚簇索引，而InnoDB的主键索引是聚簇索引，二级索引是非聚簇索引。我们自己建的索引基本都是非聚簇索引。
## 11. 非聚簇索引一定会回表查询吗？
不一定，这涉及到查询语句所要求的字段是否全部命中了索引，如果全部命中了索引，那么就不必再进行回表查询。一个索引包含（覆盖）所有需要查询字段的值，被称之为"覆盖索引"。
举个简单的例子，假设我们在员工表的年龄上建立了索引，那么当进行select score from student where score > 90的查询时，在索引的叶子节点上，已经包含了score 信息，不会再次进行回表查询。
## 12. 联合索引是什么？为什么需要注意联合索引中的顺序？
MySQL可以使用多个字段同时建立一个索引，叫做联合索引。在联合索引中，如果想要命中索引，需要按照建立索引时的字段顺序挨个使用，否则无法命中索引。
具体原因为:
MySQL使用索引时需要索引有序，假设现在建立了"name，age，school"的联合索引，那么索引的排序为: 先按照name排序，如果name相同，则按照age排序，如果age的值也相等，则按照school进行排序。
当进行查询时，此时索引仅仅按照name严格有序，因此必须首先使用name字段进行等值查询，之后对于匹配到的列而言，其按照age字段严格有序，此时可以使用age字段用做索引查找，以此类推。因此在建立联合索引的时候应该注意索引列的顺序，一般情况下，将查询需求频繁或者字段选择性高的列放在前面。此外可以根据特例的查询或者表结构进行单独的调整。
## 13. 回表
当执行 `SELECT * FROM t WHERE id = 500` 时, 即主键查询方式, 则需要搜索ID这颗B+树; 当执行 `SELECT * FROM t WHERE k = 5` 时, 即普通索引查询方式, 则先在k这棵树查找到主键的值, 再从ID这棵树中查找到对应的行.
当我们执行SQL搜索数据时, 如果需要先从非主键索引中查询到主键的值, 再从主键索引中查询到对应的数据, 这个过程就被称为**回表**. 所以应该尽量使用主键查询.

## 14. 索引维护 (页分裂与页合并)
B+树为了有序性, 需要对插入和删除数据时做出对应的维护. 当插入数据时, 如在上图中插入ID=400的数据, 那么从逻辑上来说, 需要移动后面的数据, 空出位置.

若此时R5所在数据页满了, 则需要申请一个新的数据页, 然后移动部分数据到新数据页中, 这个过程被称为**页分裂**. 页分裂影响了数据页的空间利用率, 而且在分裂过程中, 性能也会有所影响.

若相邻两个数据页因为删除导致利用率很低后, 那么会将这两个数据页的数据合并到一个数据页中, 这个过程被称为**页合并**. 即页分裂的逆过程.

## 15. 覆盖索引
如果执行了语句 `SELECT id FROM t WHERE k between 3 and 5` 时, 只需要查询 id 的值, 而 id 已经在 k 的索引树上, 所以不需要再回表去查询整行, 直接返回查询结果, 索引 k 已经覆盖了这条SQL查询的需求, 被称为 **覆盖索引**. 覆盖索引能够减少树的搜索次数, 不需要再次回表查询整行, 所以是一个常用的性能优化手段.

## 16. 讲一讲MySQL的最左前缀原则?
**最左前缀原则** 就是利用索引列中最左的字段优先进行匹配

最左前缀原则就是最左优先，在创建多列索引时，要根据业务需求，where子句中使用最频繁的一列放在最左边。
**mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配。**
例如：b = 2 如果建立(a,b)顺序的索引，是匹配不到(a,b)索引的；但是如果查询条件是a = 1 and b = 2,就可以，因为**优化器会自动调整a,b的顺序**。
比如a = 1 and b = 2 and c > 3 and d = 4 如果建立(a,b,c,d)顺序的索引，d是用不到索引的，因为c字段是一个范围查询，它之后的字段会停止匹配。如果建立(a,b,d,c)的索引则都可以用到，a,b,d的顺序可以任意调整。
=和in可以乱序，比如a = 1 and b = 2 and c = 3 建立(a,b,c)索引可以任意顺序，mysql的查询**优化器会帮你优化成索引可以识别的形式**。

**最左匹配原则的原理**
MySQL中的索引可以以一定顺序引用多列，这种索引叫作联合索引.最左匹配原则都是针对联合索引来说的

- 我们都知道索引的底层是一颗B+树，那么联合索引当然还是一颗B+树，只不过联合索引的健值数量不是一个，而是多个。构建一颗B+树只能根据一个值来构建，因此数据库依据联合索引最左的字段来构建B+树。 例子：假如创建一个（a,b)的联合索引，那么它的索引树是这样的可以看到a的值是有顺序的，1，1，2，2，3，3，而b的值是没有顺序的1，2，1，4，1，2。所以b = 2这种查询条件没有办法利用索引，因为联合索引首先是按a排序的，b是无序的。

同时我们还可以发现在a值相等的情况下，b值又是按顺序排列的，但是这种顺序是相对的。所以最左匹配原则遇上范围查询就会停止，剩下的字段都无法使用索引。例如a = 1 and b = 2 a,b字段都可以使用索引，因为在a值确定的情况下b是相对有序的，而a>1and b=2，a字段可以匹配上索引，但b值不可以，因为a的值是一个范围，在这个范围中b是无序的。
优点：最左前缀原则的利用也可以显著提高查询效率，是常见的MySQL性能优化手段。
## 17. 讲一讲前缀索引？
在对字符串创建索引, 如INDEX(name)中, 若字符串非常大, 那么响应的空间使用和维护开销也非常大, 就可以**使用字符串从左开始的部分字符创建索引（**把很长字段的前面的公共部分作为一个索引，就会产生超级加倍的效果**）**, 减少空间和维护的成本, 但是也会降低索引的选择性。但是，我们需要注意，order by不支持前缀索引 。

**索引的选择性**指的是 : 不重复的索引值和数据表的记录总数(#T)的比值, 范围为 1/#T 到 1 之间, 索引选择性越高则查询效率越高. 对于BLOB, TEXT, VARCHAR等类型的列, 必须使用前缀索引, MySQL不允许索引这些列的完整长度.
流程是： 

1. 先计算完整列的选择性 `SELECT COUNT(DISTINCT name)/COUNT(1) FROM t`
2. 在计算不同前缀长度N的选择性 `SELECT COUNT(DISCTINCT LEFT(name, N)) / COUNT(1) FROM t`
3. 看哪个N更靠近1, 进行索引的创建
## 18. 了解索引下推吗？
对于SQL语句 `SELECT * FROM t WHERE name LIKE '陈%' AND age = 10` , INDEX(name, age) 情况来说
在 MySQL5.6 之前没有引入索引下推优化时, 执行流程如下图, 在定位完name字段的索引后, 需要一条条进行回表查询, 然后再判断其他字段是否满足条件.
![image-20220304160409381.png](https://cdn.nlark.com/yuque/0/2022/png/21380271/1646468447625-ea605667-5913-4f4f-9a12-4dfc3cbb5030.png#clientId=ub63f7278-093b-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=dEDJf&margin=%5Bobject%20Object%5D&name=image-20220304160409381.png&originHeight=258&originWidth=666&originalType=binary&ratio=1&rotation=0&showTitle=false&size=119734&status=done&style=none&taskId=u001675a9-6ed7-480d-be40-64f2149c5f8&title=)
而 MySQL5.6 引入了索引下推优化后, 可以在所有遍历过程中, **对索引中包含的字段先进行判断过滤**, 然后再进行后续操作, 减少了回表次数.
![image-20220304160654992.png](https://cdn.nlark.com/yuque/0/2022/png/21380271/1646468451640-b30d74f6-06bb-4385-bb02-23db2d0368ce.png#clientId=ub63f7278-093b-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=RcWRF&margin=%5Bobject%20Object%5D&name=image-20220304160654992.png&originHeight=246&originWidth=656&originalType=binary&ratio=1&rotation=0&showTitle=false&size=123478&status=done&style=none&taskId=u10ab9e08-fc43-4d31-9eb1-283cc212fcf&title=)
## 19. 怎么查看MySQL语句有没有用到索引？
通过explain，如以下例子：
EXPLAIN SELECT * FROM employees.titles WHERE emp_no='10001' AND title='Senior Engineer' AND from_date='1986-06-26';

| **id** | **select_type** | **table** | **partitions** | **type** | **possible_keys** | **key** | **key_len** | **ref** | **filtered** | **rows** | **Extra** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | SIMPLE | titles | null | const | PRIMARY | PRIMARY | 59 | const,const,const | 10 | 1 |  |

- id：在⼀个⼤的查询语句中每个**SELECT**关键字都对应⼀个唯⼀的id ，如explain select * from s1 where id = (select id from s1 where name = 'egon1');第一个select的id是1，第二个select的id是2。有时候会出现两个select，但是id却都是1，这是因为优化器把子查询变成了连接查询 。
- select_type：select关键字对应的那个查询的类型，如SIMPLE,PRIMARY,SUBQUERY,DEPENDENT,SNION 。
- table：每个查询对应的表名 。
- type：type 字段比较重要, 它提供了判断查询是否高效的重要依据依据. 通过 type 字段, 我们判断此次查询是 全表扫描 还是 索引扫描 等。如const(主键索引或者唯一二级索引进行等值匹配的情况下),ref(普通的⼆级索引列与常量进⾏等值匹配),index(扫描全表索引的覆盖索引) 。通常来说, 不同的 type 类型的性能关系如下:ALL < index < range ~ index_merge < ref < eq_ref < const < systemALL 类型因为是全表扫描, 因此在相同的查询条件下, 它是速度最慢的.而 index 类型的查询虽然不是全表扫描, 但是它扫描了所有的索引, 因此比 ALL 类型的稍快.
- possible_key：查询中可能用到的索引_(可以把用不到的删掉，降低优化器的优化时间)_ 。
- key：此字段是 MySQL 在当前查询时所真正使用到的索引。
- filtered：查询器预测满足下一次查询条件的百分比 。
- rows 也是一个重要的字段. MySQL 查询优化器根据统计信息, 估算 SQL 要查找到结果集需要扫描读取的数据行数.这个值非常直观显示 SQL 的效率好坏, 原则上 rows 越少越好。
- extra：表示额外信息，如Using where,Start temporary,End temporary,Using temporary等。
## 20. 为什么官方建议使用自增长主键作为索引？
结合B+Tree的特点，自增主键是连续的，在插入过程中尽量减少页分裂，即使要进行页分裂，也只会分裂很少一部分。并且能减少数据的移动，每次插入都是插入到最后。总之就是减少分裂和移动的频率。
插入连续的数据：
图片来自：[https://www.javazhiyin.com/40232.html](https://www.javazhiyin.com/40232.html)
![](https://cdn.nlark.com/yuque/0/2022/gif/22219483/1647160384042-dc38cec4-ba28-4d20-ab6c-dede5b16dba9.gif#clientId=u3111ec8c-f05a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u14a17787&margin=%5Bobject%20Object%5D&originHeight=159&originWidth=486&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc692b4d3-5d3f-46bd-9b8b-e53894585b0&title=)
插入非连续的数据：
![](https://cdn.nlark.com/yuque/0/2022/gif/22219483/1647160384139-cb0af7e0-f098-4c67-85da-d669a6196a73.gif#clientId=u3111ec8c-f05a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ubf8b87a5&margin=%5Bobject%20Object%5D&originHeight=159&originWidth=486&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u64dd91af-8b46-4ef8-8f10-84f4e236baf&title=)
## 21. 如何创建索引？
创建索引有三种方式。
1、 在执行CREATE TABLE时创建索引
CREATE TABLE user_index2 (
    id INT auto_increment PRIMARY KEY,
    first_name VARCHAR (16),
    last_name VARCHAR (16),
    id_card VARCHAR (18),
    information text,
    KEY name (first_name, last_name),
    FULLTEXT KEY (information),
    UNIQUE KEY (id_card)
);

2、 使用ALTER TABLE命令去增加索引。
ALTER TABLE table_name ADD INDEX index_name (column_list);
ALTER TABLE用来创建普通索引、UNIQUE索引或PRIMARY KEY索引。
其中table_name是要增加索引的表名，column_list指出对哪些列进行索引，多列时各列之间用逗号分隔。
索引名index_name可自己命名，缺省时，MySQL将根据第一个索引列赋一个名称。另外，ALTER TABLE允许在单个语句中更改多个表，因此可以在同时创建多个索引。3、 使用CREATE INDEX命令创建。
CREATE INDEX index_name ON table_name (column_list);
## 22. 创建索引时需要注意什么？

- 非空字段：应该指定列为NOT NULL，除非你想存储NULL。在mysql中，含有空值的列很难进行查询优化，因为它们使得索引、索引的统计信息以及比较运算更加复杂。你应该用0、一个特殊的值或者一个空串代替空值；
- 取值离散大的字段：（变量各个取值之间的差异程度）的列放到联合索引的前面，可以通过count()函数查看字段的差异值，返回值越大说明字段的唯一值越多字段的离散程度高；
- 索引字段越小越好：数据库的数据存储以页为单位一页存储的数据越多一次IO操作获取的数据越大效率越高。
## 23. 建索引的原则有哪些？
1、**最左前缀匹配原则**，非常重要的原则，mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配，比如a = 1 and b = 2 and c > 3 and d = 4 如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，a,b,d的顺序可以任意调整。
2、**=和in可以乱序**，比如a = 1 and b = 2 and c = 3 建立(a,b,c)索引可以任意顺序，mysql的查询优化器会帮你优化成索引可以识别的形式。
3、**尽量选择区分度高的列作为索引**，区分度的公式是count(distinct col)/count(*)，表示字段不重复的比例，比例越大我们扫描的记录数越少，唯一键的区分度是1，而一些状态、性别字段可能在大数据面前区分度就是0，那可能有人会问，这个比例有什么经验值吗？使用场景不同，这个值也很难确定，一般需要join的字段我们都要求是0.1以上，即平均1条扫描10条记录。
4、索引列不能参与计算，保持列“干净”，比如from_unixtime(create_time) = ’2014-05-29’就不能使用到索引，原因很简单，b+树中存的都是数据表中的字段值，但进行检索时，需要把所有元素都应用函数才能比较，显然成本太大。所以语句应该写成create_time = unix_timestamp(’2014-05-29’)。
5、尽量的扩展索引，不要新建索引。比如表中已经有a的索引，现在要加(a,b)的索引，那么只需要修改原来的索引即可。
## 24. 使用索引查询一定能提高查询的性能吗？
通常通过索引查询数据比全表扫描要快。但是我们也必须注意到它的代价。
索引需要空间来存储，也需要定期维护， 每当有记录在表中增减或索引列被修改时，索引本身也会被修改。 这意味着每条记录的I* NSERT，DELETE，UPDATE将为此多付出4，5 次的磁盘I/O。 因为索引需要额外的存储空间和处理，那些不必要的索引反而会使查询反应时间变慢。使用索引查询不一定能提高查询性能，索引范围查询(INDEX RANGE SCAN)适用于两种情况:

- 基于一个范围的检索，一般查询返回结果集小于表中记录数的30%。
- 基于非唯一性索引的检索。
## [25. 什么情况下不走索引（索引失效）？](https://www.yuque.com/office/yuque/0/2022/pdf/22219483/1652951783151-8bc26cca-b463-49bc-a72f-ad65e10b351f.pdf?from=file%3A%2F%2F%2FE%3A%2FProgram%2520Files%2F%25E8%25AF%25AD%25E9%259B%2580%2Fyuque-desktop%2Fresources%2Fapp.asar%2Fbuild%2Frenderer%2Findex.html%3Flocale%3Dzh-CN%26isYuque%3Dtrue%26theme%3D%26isWebview%3Dtrue%26editorType%3Deditor%26useLocalPath%3Dundefined%23%2Feditor)
**1、全值匹配我最爱**
**2、最佳左前缀法则**
> 拓展：Alibaba《Java开发手册》
> 索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索引。

**3、主键插入顺序**
当数据页已经满了，再插进来咋办呢？我们需要把当前 页面分裂 成两个页面，把本页中的一些记录
移动到新创建的这个页中。页面分裂和记录移位意味着什么？意味着： 性能损耗 ！
所以如果我们想尽量避免这样无谓的性能损耗，最好让插入的记录的 主键值依次递增 ，这样就不会发生这样的性能损耗了。
所以我们建议：让主键具有 AUTO_INCREMENT ，让存储引擎自己为表生成主键，而不是我们手动插入 ，
我们自定义的主键列 id 拥有 AUTO_INCREMENT 属性，在插入记录时存储引擎会自动为我们填入自增的
主键值。这样的主键占用空间小，顺序写入，减少页分裂。

**4、计算、函数、运算符、类型转换(自动或手动)导致索引失效**
> 失效案例
> SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc';    type为“ALL”
> 
> SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno+1 = 900001; 
> 如果你对列进行了（+，-，*，/，!）, 那么都将不会走索引。
> 
> SELECT id，stuno，name FROM student WHERE SUBSTRING(name, 1,3)='abc';

**5、类型转换导致索引失效（类型不一致导致的索引失效）**
> name=123发生类型转换，索引失效。
> SELECT SQL_NO_CACHE * FROM student WHERE name=123;

**6、范围条件右边的列索引失效**
```sql
SELECT SQL_NO_CACHE * FROM student
WHERE student.age=30 AND student.classId>20 AND student.name = 'abc' ;      # 失效

将范围查询条件放置语句最后：

SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.name =
'abc' AND student.classId>20 ;                                              # 不失效
```
**7、使用!= 或者 <> 导致索引失效**
**8、is null可以使用索引，is not null无法使用索引**
**9、模糊搜索导致的索引失效（like以通配符%开头索引失效）**
SELECT * FROM `user` WHERE `name` LIKE '%冰';
当%放在匹配字段前是不走索引的，放在==后面==才会走索引。
> 拓展：Alibaba《Java开发手册》
> 【强制】页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决。

**10、OR引起的索引失效**
> SELECT * FROM `user` WHERE `name` = '张三' OR height = '175';
> OR导致索引是在特定情况下的，并不是所有的OR都是使索引失效，如果OR连接的是同一个字段，那么索引不会失效，反之索引失效。

**11、数据库和表的字符集统一使用utf8mb4**
统一使用utf8mb4( 5.5.3版本以上支持)兼容性更好，统一字符集可以避免由于字符集转换产生的乱码。不
同的 字符集 进行比较前需要进行 转换 会造成索引失效。
**12、NOT IN、NOT EXISTS导致索引失效**
## 26. 自适应哈希索引
InnoDB中不存在哈希索引, 但是哈希索引确实有利于快速查找, 于是InnoDB引入了"**自适应哈希索引**", 在某些索引值被使用的非常频繁时, InnoDB会在内存中的B+树结构之上创建一个哈希索引, 用于这些频繁使用的索引值的快速查找, 使得其存有哈希快速查找的特点.
## 27. 哪些情况适合创建索引
#### 1. 字段的数值有唯一性的限制
> 业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引。（来源：Alibaba）
> 说明：不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的。

#### 2. 频繁作为 WHERE 查询条件的字段
某个字段在SELECT语句的 WHERE 条件中经常被使用到，那么就需要给这个字段创建索引了。尤其是在
数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率。
比如student_info数据表（含100万条数据），假设我们想要查询 student_id=123110 的用户信息。
#### 3. 经常 GROUP BY 和 ORDER BY 的列
索引就是让数据按照某种顺序进行存储或检索，因此当我们使用 GROUP BY 对数据进行分组查询，或者
使用 ORDER BY 对数据进行排序的时候，就需要 对分组或者排序的字段进行索引 。如果待排序的列有多
个，那么可以在这些列上建立 组合索引 。
#### 4. UPDATE、DELETE 的 WHERE 条件列
对数据按照某个条件进行查询后再进行 UPDATE 或 DELETE 的操作，如果对 WHERE 字段创建了索引，就
能大幅提升效率。原理是因为我们需要先根据 WHERE 条件列检索出来这条记录，然后再对它进行更新或
删除。如果进行更新的时候，更新的字段是非索引字段，提升的效率会更明显，这是因为非索引字段更
新不需要对索引进行维护。
#### 5.DISTINCT 字段需要创建索引
有时候我们需要对某个字段进行去重，使用 DISTINCT，那么对这个字段创建索引，也会提升查询效率。
比如，我们想要查询课程表中不同的 student_id 都有哪些，如果我们没有对 student_id 创建索引，执行
SQL 语句：
```sql
SELECT DISTINCT(student_id) FROM `student_info`;
```
运行结果（600637 条记录，运行时间 0.683s ）：
如果我们对 student_id 创建索引，再执行 SQL 语句：
```sql
SELECT DISTINCT(student_id) FROM `student_info`;
```
运行结果（600637 条记录，运行时间 0.010s ）：

你能看到 SQL 查询效率有了提升，同时显示出来的 student_id 还是按照 递增的顺序 进行展示的。这是因
为索引会对数据按照某种顺序进行排序，所以在去重的时候也会快很多。
#### 6. 多表 JOIN 连接操作时，创建索引注意事项
首先， 连接表的数量尽量不要超过 3 张 ，因为每增加一张表就相当于增加了一次嵌套的循环，数量级增
长会非常快，严重影响查询的效率。
其次， 对 WHERE 条件创建索引 ，因为 WHERE 才是对数据条件的过滤。如果在数据量非常大的情况下，
没有 WHERE 条件过滤是非常可怕的。
最后， 对用于连接的字段创建索引 ，并且该字段在多张表中的 类型必须一致 。比如 course_id 在
student_info 表和 course 表中都为 int(11) 类型，而不能一个为 int 另一个为 varchar 类型。
举个例子，如果我们只对 student_id 创建索引，执行 SQL 语句：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1654780735436-331fac13-6c6b-478c-b9c2-1825c5b4d3bb.png#clientId=u999cb528-f0fb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=106&id=uedc2fbd8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=106&originWidth=742&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22225&status=done&style=none&taskId=uc6332a25-7de1-4c4f-a86e-382b23385fb&title=&width=742)运行结果（1 条数据，运行时间 0.189s ）：
这里我们对 name 创建索引，再执行上面的 SQL 语句，运行时间为 0.002s 。
#### 7. 使用列的类型小的创建索引
#### 8. 使用字符串前缀创建索引
创建一张商户表，因为地址字段比较长，在地址字段上建立前缀索引
```sql
create table shop(address varchar(120) not null);
alter table shop add index(address(12));
```
问题是，截取多少呢？截取得多了，达不到节省索引存储空间的目的；截取得少了，重复内容太多，字
段的散列度(选择性)会降低。**怎么计算不同的长度的选择性呢？**
先看一下字段在全部数据中的选择度：
```sql
select count(distinct address) / count(*) from shop;
```
通过不同长度去计算，与全表的选择性对比：
公式：
```sql
count(distinct left(列名, 索引长度))/count(*)
```
例如：
```sql
select count(distinct left(address,10)) / count(*) as sub10, -- 截取前10个字符的选择度
count(distinct left(address,15)) / count(*) as sub11, -- 截取前15个字符的选择度
count(distinct left(address,20)) / count(*) as sub12, -- 截取前20个字符的选择度
count(distinct left(address,25)) / count(*) as sub13 -- 截取前25个字符的选择度
from shop;
```
**引申另一个问题：索引列前缀对排序的影响**
**拓展：Alibaba《Java开发手册》**
【 强制 】在 varchar 字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本
区分度决定索引长度。
说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会 高达
90% 以上 ，可以使用 count(distinct left(列名, 索引长度))/count(*)的区分度来确定。
#### 9. 区分度高(散列性高)的列适合作为索引
#### 10. 使用最频繁的列放到联合索引的左侧
这样也可以较少的建立一些索引。同时，由于"最左前缀原则"，可以增加联合索引的使用率。
#### 11. 在多个字段都要创建索引的情况下，联合索引优于单值索引

## 28.哪些情况不适合创建索引
#### 1. 在where中使用不到的字段，不要设置索引
#### 2. 数据量小的表最好不要使用索引
> 结论：在数据表中的数据行数比较少的情况下，比如不到 1000 行，是不需要创建索引的。

#### 3. 有大量重复数据的列上不要建立索引
举例1：要在 100 万行数据中查找其中的 50 万行（比如性别为男的数据），一旦创建了索引，你需要先
访问 50 万次索引，然后再访问 50 万次数据表，这样加起来的开销比不使用索引可能还要大。
举例2：假设有一个学生表，学生总数为 100 万人，男性只有 10 个人，也就是占总人口的 10 万分之 1。
学生表 student_gender 结构如下。其中数据表中的 student_gender 字段取值为 0 或 1，0 代表女性，1 代
表男性。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1654781177290-abe05380-0c8f-430f-928e-36b9aee0c6f4.png#clientId=u999cb528-f0fb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=145&id=ufd9126f0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=145&originWidth=609&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22144&status=done&style=none&taskId=u40ae2b03-23ee-4983-810c-dd3360ac566&title=&width=609)
如果我们要筛选出这个学生表中的男性，可以使用：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1654781260886-aa242456-448b-45cf-9c90-3fd9c8c6a914.png#clientId=u999cb528-f0fb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=36&id=uede533ec&margin=%5Bobject%20Object%5D&name=image.png&originHeight=36&originWidth=492&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6792&status=done&style=none&taskId=u5eb976be-79e6-499d-9a87-4c74cc69daa&title=&width=492)
运行结果（10 条数据，运行时间 0.696s ）：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1654781267196-4f16132e-aa99-4655-9dfc-efcca6470a99.png#clientId=u999cb528-f0fb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=237&id=ua7c03893&margin=%5Bobject%20Object%5D&name=image.png&originHeight=237&originWidth=696&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59774&status=done&style=none&taskId=u1c2f2eda-6fbc-454d-bcc2-3fdf02bc848&title=&width=696)
> 结论：当数据重复度大，比如 高于 10% 的时候，也不需要对这个字段使用索引。

#### 4. 避免对经常更新的表创建过多的索引
#### 5. 不建议用无序的值作为索引
例如身份证、UUID(在索引比较时需要转为ASCII，并且插入时可能造成页分裂)、MD5、HASH、无序长字
符串等。
#### 6. 删除不再使用或者很少使用的索引
#### 7. 不要定义冗余或重复的索引
① 冗余索引
举例：建表语句如下
```sql
CREATE TABLE person_info(
id INT UNSIGNED NOT NULL AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
birthday DATE NOT NULL,
phone_number CHAR(11) NOT NULL,
country varchar(100) NOT NULL,
PRIMARY KEY (id),
KEY idx_name_birthday_phone_number (name(10), birthday, phone_number),
KEY idx_name (name(10))
);
```
我们知道，通过 idx_name_birthday_phone_number 索引就可以对 name 列进行快速搜索，再创建一
个专门针对 name 列的索引就算是一个 冗余索引 ，维护这个索引只会增加维护的成本，并不会对搜索有
什么好处。
② 重复索引
另一种情况，我们可能会对某个列 重复建立索引 ，比方说这样：
```sql
CREATE TABLE repeat_index_demo (
col1 INT PRIMARY KEY,
col2 INT,
UNIQUE uk_idx_c1 (col1),
INDEX idx_c1 (col1)
);
```
我们看到，col1 既是主键、又给它定义为一个唯一索引，还给它定义了一个普通索引，可是主键本身就
会生成聚簇索引，所以定义的唯一索引和普通索引是重复的，这种情况要避免。

# [MySQL优化（索引与查询优化）](https://www.yuque.com/office/yuque/0/2022/pdf/22219483/1652951783151-8bc26cca-b463-49bc-a72f-ad65e10b351f.pdf?from=file%3A%2F%2F%2FE%3A%2FProgram%2520Files%2F%25E8%25AF%25AD%25E9%259B%2580%2Fyuque-desktop%2Fresources%2Fapp.asar%2Fbuild%2Frenderer%2Findex.html%3Flocale%3Dzh-CN%26isYuque%3Dtrue%26theme%3D%26isWebview%3Dtrue%26editorType%3Deditor%26useLocalPath%3Dundefined%23%2Feditor)
## 1. 如何定位及优化SQL语句的性能问题？
对于低性能的SQL语句的定位，最重要也是最有效的方法就是使用执行计划，**MySQL提供了explain命令来查看语句的执行计划**。 我们知道，不管是哪种数据库，或者是哪种数据库引擎，在对一条SQL语句进行执行的过程中都会做很多相关的优化，对于查询语句，最重要的优化方式就是使用索引。
而执行计划，就是显示数据库引擎对于SQL语句的执行的详细情况，其中包含了是否使用索引，使用什么索引，使用的索引的相关信息等。![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1647160348017-77b98e7a-f981-49a6-8f50-0726b105bed4.png#clientId=u44f2ba8f-1dfe-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=nkJww&margin=%5Bobject%20Object%5D&name=image.png&originHeight=393&originWidth=1172&originalType=url&ratio=1&rotation=0&showTitle=false&size=229993&status=done&style=none&taskId=uf871b150-eb08-414e-855d-1192fbbdeea&title=)
## 2. 大表数据查询，怎么优化

- 优化shema、sql语句+索引；
- 第二加缓存，memcached, redis；
- 主从复制，读写分离；
- 垂直拆分，根据你模块的耦合度，将一个大的系统分为多个小的系统，也就是分布式系统；
- 水平切分，针对数据量大的表，这一步最麻烦，最能考验技术水平，要选择一个合理的sharding key, 为了有好的查询效率，表结构也要改动，做一定的冗余，应用也要改，sql中尽量带sharding key，将数据定位到限定的表上去查，而不是扫描全部的表；
## 3. 超大分页怎么处理?
数据库层面,这也是我们主要集中关注的(虽然收效没那么大),类似于select * from table where age > 20 limit 1000000,10 这种查询其实也是有可以优化的余地的. 这条语句需要 load1000000 数据然后基本上全部丢弃,只取 10 条当然比较慢. 当时我们可以修改为select * from table where id in (select id from table where age > 20 limit 1000000,10).这样虽然也 load 了一百万的数据,但是由于索引覆盖,要查询的所有字段都在索引中,所以速度会很快。
**解决超大分页,其实主要是靠缓存,**可预测性的提前查到内容,缓存至redis等k-V数据库中,直接返回即可.
在阿里巴巴《Java开发手册》中,对超大分页的解决办法是类似于上面提到的第一种.
【推荐】利用延迟关联或者子查询优化超多分页场景。 
说明：MySQL并不是跳过offset行，而是取offset+N行，然后返回放弃前offset行，返回N行，那当offset特别大的时候，效率就非常的低下，要么控制返回的总页数，要么对超过特定阈值的页数进行SQL改写。 
正例：先快速定位需要获取的id段，然后再关联： 
SELECT a.* FROM 表1 a, (select id from 表1 where 条件 LIMIT 100000,20 ) b where a.id=b.id
## 4. 统计过慢查询吗？对慢查询都怎么优化过？
在业务系统中，除了使用主键进行的查询，其他的我都会在测试库上测试其耗时，慢查询的统计主要由运维在做，会定期将业务中的慢查询反馈给我们。

定位执行慢的 SQL：慢查询日志

1. 开启慢查询日志参数
   1. **开启slow_query_log   **set global slow_query_log='ON';
   2. **修改long_query_time阈值**
2. 查看慢查询数目
```sql
查询当前系统中有多少条慢查询记录
SHOW GLOBAL STATUS LIKE '%Slow_queries%';
```

3. 案例演示
4. 测试及分析
5. 慢查询日志分析工具：mysqldumpslow
6. 关闭慢查询日志
   1. 方式1：永久性方式
         1.  ![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655169525602-f1742004-729d-4bfa-8395-a4a0871fd616.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=81&id=u48acfb63&margin=%5Bobject%20Object%5D&name=image.png&originHeight=81&originWidth=265&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4483&status=done&style=none&taskId=ub39bb8ff-9158-4019-902d-2a563d3507a&title=&width=265)
         2. 或者，把slow_query_log一项注释掉 或 删除	![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655169516381-aeb9add9-8387-4f7b-a19a-a4d506499671.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=63&id=u04d21834&margin=%5Bobject%20Object%5D&name=image.png&originHeight=63&originWidth=410&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5056&status=done&style=none&taskId=uc65c37ea-b9e8-47b1-8cab-29873f0d035&title=&width=410)
         3. 重启MySQL服务，执行如下语句查询慢日志功能。
> SHOW VARIABLES LIKE '%slow%'; #查询慢查询日志所在目录
> SHOW VARIABLES LIKE '%long_query_time%'; #查询超时时长

   2. 方式2：临时性方式：使用SET语句来设置。
      1. （1）停止MySQL慢查询日志功能，具体SQL语句如下
      2. （2）重启MySQL服务，使用SHOW语句查询慢查询日志功能信息，具体SQL语句如下
>             1. SHOW VARIABLES LIKE '%slow%';
>             2. #以及
>             3. SHOW VARIABLES LIKE '%long_query_time%';

7. 删除慢查询日志

慢查询的优化首先要搞明白**慢的原因是什么？** 是**查询条件没有命中索引？是load了不需要的数据列？**还是**数据量太大？**
所以优化也是针对这三个方向来的，

- 首先分析语句，看看是否load了额外的数据，可能是查询了多余的行并且抛弃掉了，可能是加载了许多结果中并不需要的列，对语句进行分析以及重写。
- 分析语句的执行计划，然后获得其使用索引的情况，之后修改语句或者修改索引，使得语句可以尽可能的命中索引。
- 如果对语句的优化已经无法进行，可以考虑表中的数据量是否太大，如果是的话可以进行横向或者纵向的分表。
## 5. 如何优化查询过程中的数据访问

- 访问数据太多导致查询性能下降
- 确定应用程序是否在检索大量超过需要的数据，可能是太多行或列
- 确认MySQL服务器是否在分析大量不必要的数据行
- 查询不需要的数据。解决办法：使用limit解决
- 多表关联返回全部列。解决办法：指定列名
- 总是返回全部列。解决办法：避免使用SELECT *
- 重复查询相同的数据。解决办法：可以缓存数据，下次直接读取缓存
- 是否在扫描额外的记录。解决办法：使用explain进行分析，如果发现查询需要扫描大量的数据，但只返回少数的行，可以通过如下技巧去优化：使用索引覆盖扫描，把所有的列都放到索引中，这样存储引擎不需要回表获取对应行就可以返回结果。
- 改变数据库和表的结构，修改数据表范式
- 重写SQL语句，让优化器可以以更优的方式执行查询。
## 6. [如何优化关联查询](https://www.yuque.com/office/yuque/0/2022/pdf/22219483/1652951783151-8bc26cca-b463-49bc-a72f-ad65e10b351f.pdf?from=file%3A%2F%2F%2FE%3A%2FProgram%2520Files%2F%25E8%25AF%25AD%25E9%259B%2580%2Fyuque-desktop%2Fresources%2Fapp.asar%2Fbuild%2Frenderer%2Findex.html%3Flocale%3Dzh-CN%26isYuque%3Dtrue%26theme%3D%26isWebview%3Dtrue%26editorType%3Deditor%26useLocalPath%3Dundefined%23%2Feditor)

- 确定ON或者USING子句中是否有索引。
- 确保GROUP BY和ORDER BY只有一个表中的列，这样MySQL才有可能使用索引。
- 

- 保证被驱动表的JOIN字段已经创建了索引
- 需要JOIN 的字段，数据类型保持绝对一致。
- LEFT JOIN 时，选择小表作为驱动表， 大表作为被驱动表 。减少外层循环的次数。
- INNER JOIN 时，MySQL会自动将 小结果集的表选为驱动表 。选择相信MySQL优化策略。
- 能够直接多表关联的尽量直接关联，不用子查询。(减少查询的趟数)
- 不建议使用子查询，建议将子查询SQL拆开结合程序多次查询，或使用 JOIN 来代替子查询。
- 衍生表建不了索引
## 7. 子查询优化
MySQL从4.1版本开始支持子查询，使用子查询可以进行SELECT语句的嵌套查询，即一个SELECT查询的结
果作为另一个SELECT语句的条件。 子查询可以一次性完成很多逻辑上需要多个步骤才能完成的SQL操作 。

**子查询是 MySQL 的一项重要的功能，可以帮助我们通过一个 SQL 语句实现比较复杂的查询。但是，子**
**查询的执行效率不高。**原因：
① 执行子查询时，MySQL需要为内层查询语句的查询结果 建立一个临时表 ，然后外层查询语句从临时表
中查询记录。查询完毕后，再 撤销这些临时表 。这样会消耗过多的CPU和IO资源，产生大量的慢查询。
② 子查询的结果集存储的临时表，不论是内存临时表还是磁盘临时表都 不会存在索引 ，所以查询性能会
受到一定的影响。
③ 对于返回结果集比较大的子查询，其对查询性能的影响也就越大。

**在MySQL中，可以使用连接（JOIN）查询来替代子查询**。连接查询 不需要建立临时表 ，其 速度比子查询
要快 ，如果查询中使用索引的话，性能就会更好。
> 结论：尽量不要使用NOT IN 或者 NOT EXISTS，用LEFT JOIN xxx ON xx WHERE xx IS NULL替代

## 8. 排序优化
**问题**：在 WHERE 条件字段上加索引，但是为什么在 ORDER BY 字段上还要加索引呢？
**优化建议：**

1. SQL 中，可以在 WHERE 子句和 ORDER BY 子句中使用索引，目的是在 WHERE 子句中 避免全表扫
描 ，在 ORDER BY 子句 避免使用 FileSort 排序 。当然，某些情况下全表扫描，或者 FileSort 排
序不一定比索引慢。但总的来说，我们还是要避免，以提高查询效率。
2. 尽量使用 Index 完成 ORDER BY 排序。如果 WHERE 和 ORDER BY 后面是相同的列就使用单索引列；
如果不同就使用联合索引。
3. 无法使用 Index 时，需要对 FileSort 方式进行调优。

> 结论：
> 1. 两个索引同时存在，mysql自动选择最优的方案。（对于这个例子，mysql选择
idx_age_stuno_name）。但是， 随着数据量的变化，选择的索引也会随之变化的 。
> 2. **当【范围条件】和【group by 或者 order by】的字段出现二选一时，优先观察条件字段的过
滤数量，如果过滤的数据足够多，而需要排序的数据并不多时，优先把索引放在范围字段
上。反之，亦然**。

## 9. GROUP BY优化

- group by 使用索引的原则几乎跟order by一致 ，group by 即使没有过滤条件用到索引，也可以直接
- 使用索引。
- group by 先排序再分组，遵照索引建的最佳左前缀法则
- 当无法使用索引列，增大 max_length_for_sort_data 和 sort_buffer_size 参数的设置
- where效率高于having，能写在where限定的条件就不要写在having中了
- 减少使用order by，和业务沟通能不排序就不排序，或将排序放到程序端去做。Order by、group
- by、distinct这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的。
- 包含了order by、group by、distinct这些查询的语句，where条件过滤出来的结果集请保持在1000行
- 以内，否则SQL会很慢。
## 10. 优化分页查询
**优化思路一**
在索引上完成排序分页操作，最后根据主键关联回原表查询所需要的其他列内容。
```sql
EXPLAIN SELECT * FROM student t,
(SELECT id FROM student ORDER BY id LIMIT 2000000,10)a
WHERE t.id = a.id;
```
**优化思路二**
该方案适用于主键自增的表，可以把Limit 查询转换成某个位置的查询 。
```sql
EXPLAIN SELECT * FROM student WHERE id > 2000000 LIMIT 10;
```
## 11. 优先考虑覆盖索引
什么是覆盖索引？
> **理解方式一**：索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它
> 不必读取整个行。毕竟索引叶子节点存储了它们索引的数据；当能通过读取索引就可以得到想要的数
> 据，那就不需要读取行了。**一个索引包含了满足查询结果的数据就叫做覆盖索引**。
> **理解方式二**：非聚簇复合索引的一种形式，它包括在查询里的SELECT、JOIN和WHERE子句用到的所有列
> （即建索引的字段正好是覆盖查询条件中所涉及的字段）。
> 
> 简单说就是， 索引列+主键 包含 SELECT 到 FROM之间查询的列 。

覆盖索引的利弊
**好处：**

1. **避免Innodb表进行索引的二次查询（回表）**
2. **可以把随机IO变成顺序IO加快查询效率
弊端：**
索引字段的维护 总是有代价的。因此，在建立冗余索引来支持覆盖索引时就需要权衡考虑了。这是业务
DBA，或者称为业务数据架构师的工作。
## 12. (前缀索引)如何给字符串添加索引
MySQL是支持前缀索引的。默认地，如果你创建索引的语句不指定前缀长度，那么索引就会包含整个字
符串。
```sql
mysql> alter table teacher add index index1(email);
#或
mysql> alter table teacher add index index2(email(6));
```
这两种不同的定义在数据结构和存储上有什么区别呢？下图就是这两个索引的示意图。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655172747974-afdcff81-6404-4edb-bfcd-91576b90adae.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=282&id=u4dfd91c2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=282&originWidth=846&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31929&status=done&style=none&taskId=uc31b5ae7-a923-443f-a6ef-5a94b216dc0&title=&width=846)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655172756505-7a112bc7-67b3-4bad-8d27-948573c73ebc.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=520&id=u84e1a55f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=520&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39206&status=done&style=none&taskId=ubbf782b5-4bf6-4b6c-8ecb-bc4002d4bd5&title=&width=727)
**如果使用的是index1**（即email整个字符串的索引结构），执行顺序是这样的：

1. 从index1索引树找到满足索引值是’ [zhangssxyz@xxx.com](mailto:zhangssxyz@xxx.com) ’的这条记录，取得ID2的值；
2. 到主键上查到主键值是ID2的行，判断email的值是正确的，将这行记录加入结果集；
3. 取index1索引树上刚刚查到的位置的下一条记录，发现已经不满足email=' [zhangssxyz@xxx.com](mailto:zhangssxyz@xxx.com) ’的
条件了，循环结束。
这个过程中，只需要回主键索引取一次数据，所以系统认为只扫描了一行。

**如果使用的是index2（即email(6)索引结构），执行顺序是这样的：**

4. 从index2索引树找到满足索引值是’zhangs’的记录，找到的第一个是ID1；
5. 到主键上查到主键值是ID1的行，判断出email的值不是’ [zhangssxyz@xxx.com](mailto:zhangssxyz@xxx.com) ’，这行记录丢弃；
6. 取index2上刚刚查到的位置的下一条记录，发现仍然是’zhangs’，取出ID2，再到ID索引上取整行然
后判断，这次值对了，将这行记录加入结果集；
7. 重复上一步，直到在idxe2上取到的值不是’zhangs’时，循环结束。

也就是说**使用前缀索引，定义好长度，就可以做到既节省空间，又不用额外增加太多的查询成本。**前面
已经讲过区分度，区分度越高越好。因为区分度越高，意味着重复的键值越少。

**前缀索引对覆盖索引的影响**
> 结论：
> 使用前缀索引就用不上覆盖索引对查询性能的优化了，这也是你在选择是否使用前缀索引时需要考
> 虑的一个因素。

## 13. 索引下推
> Index Condition Pushdown(ICP)是MySQL 5.6中新特性，是一种在存储引擎层使用索引过滤数据的一种优
> 化方式。ICP可以减少存储引擎访问基表的次数以及MySQL服务器访问存储引擎的次数。

**ICP的使用条件：**
① 只能用于二级索引(secondary index)
②explain显示的执行计划中type值（join 类型）为 range 、 ref 、 eq_ref 或者 ref_or_null 。
③ 并非全部where条件都可以用ICP筛选，如果where条件的字段不在索引列中，还是要读取整表的记录
到server端做where过滤。
④ ICP可以用于MyISAM和InnnoDB存储引擎
⑤ MySQL 5.6版本的不支持分区表的ICP功能，5.7版本的开始支持。
⑥ 当SQL使用覆盖索引时，不支持ICP优化方法。
## 14. 普通索引 vs 唯一索引
**从性能的角度考虑，你选择唯一索引还是普通索引呢？选择的依据是什么呢？**
假设，我们有一个主键列为ID的表，表中有字段k，并且在k上有索引，假设字段 k 上的值都不重复。

1. **查询过程**
> 假设，执行查询的语句是 select id from test where k=5。
> - 对于普通索引来说，查找到满足条件的第一个记录(5,500)后，需要查找下一个记录，直到碰到第一个不满足k=5条件的记录。
> - 对于唯一索引来说，由于索引定义了唯一性，查找到第一个满足条件的记录后，就会停止继续检
> - 索。
> 
那么，这个不同带来的性能差距会有多少呢？答案是， 微乎其微 。

2. **更新过程**

为了说明普通索引和唯一索引对更新语句性能的影响这个问题，介绍一下change buffer。

当需要更新一个数据页时，如果数据页在内存中就直接更新，而如果这个数据页还没有在内存中的话，
在不影响数据一致性的前提下， InooDB会将这些更新操作缓存在change buffer中 ，这样就不需要从磁
盘中读入这个数据页了。在下次查询需要访问这个数据页的时候，将数据页读入内存，然后执行change
buffer中与这个页有关的操作。通过这种方式就能保证这个数据逻辑的正确性。

将change buffer中的操作应用到原数据页，得到最新结果的过程称为 merge 。除了 访问这个数据页 会触
发merge外，系统有 后台线程会定期 merge。在 数据库正常关闭（shutdown） 的过程中，也会执行merge
操作。

如果能够将更新操作先记录在change buffer， 减少读磁盘 ，语句的执行速度会得到明显的提升。而且，
数据读入内存是需要占用 buffer pool 的，所以这种方式还能够 避免占用内存 ，提高内存利用率。

唯一索引的更新就不能使用change buffer ，实际上也只有普通索引可以使用。

3. **change buffer的使用场景**
1. 普通索引和唯一索引应该怎么选择？其实，这两类索引在查询能力上是没差别的，主要考虑的是
对 更新性能 的影响。所以，建议你 尽量选择普通索引 。
2. 在实际使用中会发现， 普通索引 和 change buffer 的配合使用，对于 数据量大 的表的更新优化
还是很明显的。
3. 如果所有的更新后面，都马上 伴随着对这个记录的查询 ，那么你应该 关闭change buffer 。而在
其他情况下，change buffer都能提升更新性能。
4. 由于唯一索引用不上change buffer的优化机制，因此如果 业务可以接受 ，从性能角度出发建议优
先考虑非唯一索引。但是如果"业务可能无法确保"的情况下，怎么处理呢？
- 首先， 业务正确性优先 。我们的前提是“业务代码已经保证不会写入重复数据”的情况下，讨论性能
问题。如果业务不能保证，或者业务就是要求数据库来做约束，那么没得选，必须创建唯一索引。
这种情况下，本节的意义在于，如果碰上了大量插入数据慢、内存命中率低的时候，给你多提供一
个排查思路。
- 然后，在一些“ 归档库 ”的场景，你是可以考虑使用唯一索引的。比如，线上数据只需要保留半年，
然后历史数据保存在归档库。这时候，归档数据已经是确保没有唯一键冲突了。要提高归档效率，
可以考虑把表里面的唯一索引改成普通索引。
## 16. 其它查询优化策略
**1、EXISTS 和 IN 的区分**
> 问题：
> 不太理解哪种情况下应该使用 EXISTS，哪种情况应该用 IN。选择的标准是看能否使用表的索引吗？

**2、COUNT(*)与COUNT(具体字段)效率**
> 问：在 MySQL 中统计数据表的行数，可以使用三种方式： SELECT COUNT(*) 、 SELECT COUNT(1) 和
> SELECT COUNT(具体字段) ，使用这三者之间的查询效率是怎样的？

**3 关于SELECT(*)**
在表查询中，建议明确字段，不要使用 * 作为查询的字段列表，推荐使用SELECT <字段列表> 查询。原
因：
① MySQL 在解析的过程中，会通过 查询数据字典 将"*"按序转换成所有列名，这会大大的耗费资源和时
间。
② 无法使用 覆盖索引

**4 LIMIT 1 对优化的影响**
针对的是会扫描全表的 SQL 语句，如果你可以确定结果集只有一条，那么加上 LIMIT 1 的时候，当找
到一条结果的时候就不会继续扫描了，这样会加快查询速度。
如果数据表已经对字段建立了唯一索引，那么可以通过索引进行查询，不会全表扫描的话，就不需要加
上 LIMIT 1 了。

**5 多使用COMMIT**
只要有可能，在程序中尽量多使用 COMMIT，这样程序的性能得到提高，需求也会因为 COMMIT 所释放
的资源而减少。
COMMIT 所释放的资源：

- 回滚段上用于恢复数据的信息
- 被程序语句获得的锁
- redo / undo log buffer 中的空间
- 管理上述 3 种资源中的内部花费
## 17. 主键如何设计的？
**自增ID的问题**
> 自增ID做主键，简单易懂，几乎所有数据库都支持自增类型，只是实现上各自有所不同而已。自增ID除
了简单，其他都是缺点，总体来看存在以下几方面的问题：

1. **可靠性不高**
存在自增ID回溯的问题，这个问题直到最新版本的MySQL 8.0才修复。
2. **安全性不高**
对外暴露的接口可以非常容易猜测对应的信息。比如：/User/1/这样的接口，可以非常容易猜测用户ID的
值为多少，总用户数量有多少，也可以非常容易地通过接口进行数据的爬取。
3. **性能差**
自增ID的性能较差，需要在数据库服务器端生成。
4. **交互多**
业务还需要额外执行一次类似 last_insert_id() 的函数才能知道刚才插入的自增值，这需要多一次的
网络交互。在海量并发的系统中，多1条SQL，就多一次性能上的开销。
5. **局部唯一性**
最重要的一点，自增ID是局部唯一，只在当前数据库实例中唯一，而不是全局唯一，在任意服务器间都
是唯一的。对于目前分布式系统来说，这简直就是噩梦。

**业务字段做主键**
**建议尽量不要用跟业务有关的字段做主键。毕竟，作为项目设计的技术人员，我们谁也无法预测**
**在项目的整个生命周期中，哪个业务字段会因为项目的业务需求而有重复，或者重用之类的情况出现。**
> 经验：
> 刚开始使用 MySQL 时，很多人都很容易犯的错误是喜欢用业务字段做主键，想当然地认为了解业
> 务需求，但实际情况往往出乎意料，而更改主键设置的成本非常高。

**淘宝的主键设计**
从上图可以发现，订单号不是自增ID！我们详细看下上述4个订单号：
> 1550672064762308113
> 1481195847180308113
> 1431156171142308113
> 1431146631521308113

订单号是19位的长度，且订单的最后5位都是一样的，都是08113。且订单号的前面14位部分是单调递增
的。
大胆猜测，淘宝的订单ID设计应该是：
> 订单ID = 时间 + 去重字段 + 用户ID后6位尾号

这样的设计能做到全局唯一，且对分布式系统查询及其友好。

**推荐的主键设计**
非核心业务 ：对应表的主键自增ID，如告警、日志、监控等信息。
核心业务 ：**主键设计至少应该是全局唯一且是单调递增**。全局唯一保证在各系统之间都是唯一的，单调
递增是希望插入时不影响数据库性能。
这里推荐最简单的一种主键设计：UUID。
**UUID的特点：**
全局唯一，占用36字节，数据无序，插入性能差。
**认识UUID：**
为什么UUID是全局唯一的？
为什么UUID占用36个字节？
为什么UUID是无序的？
MySQL数据库的UUID组成如下所示：
> UUID = 时间+UUID版本（16字节）- 时钟序列（4字节） - MAC地址（12字节）

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655174030830-f735ec90-fd5b-4328-a241-cfecabaa0b50.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=306&id=u5860096d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=306&originWidth=883&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97631&status=done&style=none&taskId=u72d9ec6c-62e7-4ded-be70-c3d9fc96eee&title=&width=883)
为什么UUID是全局唯一的？
在UUID中时间部分占用60位，存储的类似TIMESTAMP的时间戳，但表示的是从1582-10-15 00：00：00.00
到现在的100ns的计数。可以看到UUID存储的时间精度比TIMESTAMPE更高，时间维度发生重复的概率降
低到1/100ns。
时钟序列是为了避免时钟被回拨导致产生时间重复的可能性。MAC地址用于全局唯一。
为什么UUID占用36个字节？
UUID根据字符串进行存储，设计时还带有无用"-"字符串，因此总共需要36个字节。
为什么UUID是随机无序的呢？
因为UUID的设计中，将时间低位放在最前面，而这部分的数据是一直在变化的，并且是无序。

**改造UUID**
若将时间高低位互换，则时间就是单调递增的了，也就变得单调递增了。MySQL 8.0可以更换时间低位和
时间高位的存储方式，这样UUID就是有序的UUID了。
MySQL 8.0还解决了UUID存在的空间占用的问题，除去了UUID字符串中无意义的"-"字符串，并且将字符
串用二进制类型保存，这样存储空间降低为了16字节。
可以通过MySQL8.0提供的uuid_to_bin函数实现上述功能，同样的，MySQL也提供了bin_to_uuid函数进行
转化：
```sql
SET @uuid = UUID();
SELECT @uuid，uuid_to_bin(@uuid),uuid_to_bin(@uuid,TRUE);
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655174592390-079584da-2352-422f-994e-b92fa5d40942.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=227&id=u9e583631&margin=%5Bobject%20Object%5D&name=image.png&originHeight=227&originWidth=1455&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102812&status=done&style=none&taskId=ua16f16cc-a931-48fe-9ba5-6b24cb75fa5&title=&width=1455)
**通过函数uuid_to_bin(@uuid,true)将UUID转化为有序UUID了**。全局唯一 + 单调递增，这不就是我们想要
的主键！
> 在当今的互联网环境中，非常不推荐自增ID作为主键的数据库设计。更推荐类似有序UUID的全局
> 唯一的实现。
> 另外在真实的业务系统中，主键还可以加入业务和系统属性，如用户的尾号，机房的信息等。这样
> 的主键设计就更为考验架构师的水平了。

**如果不是MySQL8.0 肿么办？**
手动赋值字段做主键！
比如，设计各个分店的会员表的主键，因为如果每台机器各自产生的数据需要合并，就可能会出现主键
重复的问题。
可以在总部 MySQL 数据库中，有一个管理信息表，在这个表中添加一个字段，专门用来记录当前会员编
号的最大值。
门店在添加会员的时候，先到总部 MySQL 数据库中获取这个最大值，在这个基础上加 1，然后用这个值
作为新会员的“id”，同时，更新总部 MySQL 数据库管理信息表中的当 前会员编号的最大值。
这样一来，各个门店添加会员的时候，都对同一个总部 MySQL 数据库中的数据表字段进 行操作，就解
决了各门店添加会员时会员编号冲突的问题。
## 18. 数据库结构优化
一个好的数据库设计方案对于数据库的性能往往会起到事半功倍的效果。
需要考虑数据冗余、查询和更新的速度、字段的数据类型是否合理等多方面的内容。
**1 拆分表：冷热数据分离（将字段很多的表分解成多个表）**
对于字段较多的表，如果有些字段的使用频率很低，可以将这些字段分离出来形成新表。
因为当一个表的数据量很大时，会由于使用频率低的字段的存在而变慢。
**2 增加中间表**
对于需要经常联合查询的表，可以建立中间表以提高查询效率。
通过建立中间表，将需要通过联合查询的数据插入到中间表中，然后将原来的联合查询改为对中间表的查询。
**3 增加冗余字段**
设计数据库表时应尽量遵循范式理论的规约，尽可能减少冗余字段，让数据库设计看起来精致、优雅。
但是，合理地加入冗余字段可以提高查询速度。
表的规范化程度越高，表与表之间的关系就越多，需要连接查询的情况也就越多。尤其在数据量大，而
且需要频繁进行连接的时候，为了提升效率，我们也可以考虑增加冗余字段来减少连接。
注意：
冗余字段的值在一个表中修改了，就要想办法在其他表中更新，否则就会导致数据不一致的问题。
**4 优化数据类型**
**情况1：对整数类型数据进行优化。**
遇到整数类型的字段可以用 INT 型 。这样做的理由是，INT 型数据有足够大的取值范围，不用担心数
据超出取值范围的问题。刚开始做项目的时候，首先要保证系统的稳定性，这样设计字段类型是可以
的。但在数据量很大的时候，数据类型的定义，在很大程度上会影响到系统整体的执行效率。
对于 非负型 的数据（如自增ID、整型IP）来说，要优先使用无符号整型 UNSIGNED 来存储。因为无符号
相对于有符号，同样的字节数，存储的数值范围更大。如tinyint有符号为-128-127，无符号为0-255，多
出一倍的存储空间。
**情况2：既可以使用文本类型也可以使用整数类型的字段，要选择使用整数类型。**
跟文本类型数据相比，大整数往往占用 更少的存储空间 ，因此，在存取和比对的时候，可以占用更少的
内存空间。所以，在二者皆可用的情况下，尽量使用整数类型，这样可以提高查询的效率。如：将IP地
址转换成整型数据。
**情况3：避免使用TEXT、BLOB数据类型**
**情况4：避免使用ENUM类型**
**情况5：使用TIMESTAMP存储时间**
**情况6：用DECIMAL代替FLOAT和DOUBLE存储精确浮点数**
**总之，遇到数据量大的项目时，一定要在充分了解业务需求的前提下，合理优化数据类型，这样才能充**
**分发挥资源的效率，使系统达到最优。**
**5 优化插入记录的速度**

1. MyISAM引擎的表：
① 禁用索引
② 禁用唯一性检查
③ 使用批量插入

④ 使用LOAD DATA INFILE 批量导入
2. InnoDB引擎的表： 
① 禁用唯一性检查
	② 禁用外键检查
	③ 禁止自动提交
**6 使用非空约束**
**在设计字段的时候，如果业务允许，建议尽量使用非空约束**
**7 分析表、检查表与优化表**
## 19. MySQL数据库cpu飙升到500%的话他怎么处理？
当 cpu 飙升到 500%时，先用操作系统命令 top 命令观察是不是 MySQLd 占用导致的，如果不是，找出占用高的进程，并进行相关处理。
如果是 MySQLd 造成的， show processlist，看看里面跑的 session 情况，是不是有消耗资源的 sql 在运行。找出消耗高的 sql，看看执行计划是否准确， index 是否缺失，或者实在是数据量太大造成。
一般来说，肯定要 kill 掉这些线程(同时观察 cpu 使用率是否下降)，等进行相应的调整(比如说加索引、改 sql、改内存参数)之后，再重新跑这些 SQL。
也有可能是每个 sql 消耗资源并不多，但是突然之间，有大量的 session 连进来导致 cpu 飙升，这种情况就需要跟应用一起来分析为何连接数会激增，再做出相应的调整，比如说限制连接数等。
## 20[. 大表怎么优化？](https://www.yuque.com/office/yuque/0/2022/pdf/22219483/1652951782617-d0326ea9-d839-4724-8f0f-e26dc4667172.pdf?from=file%3A%2F%2F%2FE%3A%2FProgram%2520Files%2F%25E8%25AF%25AD%25E9%259B%2580%2Fyuque-desktop%2Fresources%2Fapp.asar%2Fbuild%2Frenderer%2Findex.html%3Flocale%3Dzh-CN%26isYuque%3Dtrue%26theme%3D%26isWebview%3Dtrue%26editorType%3Deditor%26useLocalPath%3Dundefined%23%2Feditor)
类似的问题：某个表有近千万数据，CRUD比较慢，如何优化？分库分表了是怎么做的？分表分库了有什么问题？有用到中间件么？他们的原理知道么？
当MySQL单表记录数过大时，数据库的CRUD性能会明显下降，一些常见的优化措施如下：

- **限定数据的范围**： 务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内；
- **读/写分离**： 经典的数据库拆分方案，主库负责写，从库负责读；
- **垂直拆分：**当数据量级达到 千万级 以上时，有时候我们需要把一个数据库切成多份，放到不同的数据库服务器上，减少对单一数据库服务器的访问压力。
   - 垂直拆分的优点： 可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。
   - 垂直拆分的缺点： 主键会出现冗余，需要管理冗余列，并会引起 JOIN 操作。此外，垂直拆分会让事务变得更加复杂。
- **水平拆分（分表）：**![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655301676051-54b22aba-3a47-43a6-aeec-24d3635a3d5b.png#clientId=u4f711631-2b55-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=320&id=u42d961ab&margin=%5Bobject%20Object%5D&name=image.png&originHeight=320&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24826&status=done&style=none&taskId=u28b60c34-19a6-4d71-b588-00e20588e67&title=&width=725)
- **缓存：** 使用MySQL的缓存，另外对重量级、更新少的数据可以考虑；
- 通过分库分表的方式进行优化，主要有垂直分表和水平分表。 

## 2[1. 分析查询语句：EXPLAIN](https://www.yuque.com/office/yuque/0/2022/pdf/22219483/1652951781581-a7908912-a615-4fb0-adbb-859f69535636.pdf?from=file%3A%2F%2F%2FE%3A%2FProgram%2520Files%2F%25E8%25AF%25AD%25E9%259B%2580%2Fyuque-desktop%2Fresources%2Fapp.asar%2Fbuild%2Frenderer%2Findex.html%3Flocale%3Dzh-CN%26isYuque%3Dtrue%26theme%3D%26isWebview%3Dtrue%26editorType%3Deditor%26useLocalPath%3Dundefined%23%2Feditor)
> MySQL 5.6.3以前只能 EXPLAIN SELECT ；MYSQL 5.6.3以后就可以 EXPLAIN SELECT，UPDATE，
> DELETE

**EXPLAIN 语句输出的各个列的作用如下：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655166437893-c669e076-e53d-4697-b1ad-de72f963a7d8.png#clientId=u7c70557e-443c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=671&id=u1504957e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=671&originWidth=904&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105474&status=done&style=none&taskId=uc8e6edbd-ee02-496d-8eef-6e6fd06eefb&title=&width=904)
**EXPLAIN各列作用**
> 为了让大家有比较好的体验，我们调整了下 EXPLAIN 输出列的顺序。

**1. table**
不论我们的查询语句有多复杂，里边儿 包含了多少个表 ，到最后也是需要对每个表进行 单表访问 的，所
以MySQL规定EXPLAIN语句输出的每条记录都对应着某个单表的访问方法，该条记录的table列代表着该
表的表名（有时不是真实的表名字，可能是简称）。
**2. id**
我们写的查询语句一般都以 SELECT 关键字开头，比较简单的查询语句里只有一个 SELECT 关键字，比
如下边这个查询语句：
```sql
SELECT * FROM s1 WHERE key1 = 'a';
```
稍微复杂一点的连接查询中也只有一个 SELECT 关键字，比如：
```sql
SELECT * FROM s1 INNER JOIN s2
ON s1.key1 = s2.key1
WHERE s1.common
field = 'a';
```
**小结:**

- **id如果相同，可以认为是一组，从上往下顺序执行**
- **在所有组中，id值越大，优先级越高，越先执行**
- **关注点：id号每个号码，表示一趟独立的查询, 一个sql的查询趟数越少越好**

**3. select_type**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655166879455-40dd2336-9372-49b8-a125-0ea2905d424f.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=754&id=ua8cc9844&margin=%5Bobject%20Object%5D&name=image.png&originHeight=754&originWidth=904&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107284&status=done&style=none&taskId=u50bceeed-cc72-4bec-aebe-7dec5bfbab2&title=&width=904)
**4. partitions (可略)**
**5. type ☆**
完整的访问方法如下： system ， const ， eq_ref ， ref ， fulltext ， ref_or_null ，index_merge unique_subquery ， index_subquery ， range ， index ， ALL 。

- system 
```sql
CREATE TABLE t(i int) Engine=MyISAM;

INSERT INTO t VALUES(1);

EXPLAIN SELECT * FROM t;
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655167719561-209d8917-31ed-4466-985e-34726b7b4683.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=130&id=u59bc3c13&margin=%5Bobject%20Object%5D&name=image.png&originHeight=130&originWidth=858&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43247&status=done&style=none&taskId=u58b1d9f8-a4b0-4a9a-a687-352ccec49f7&title=&width=858)

- const  
```sql
EXPLAIN SELECT * FROM s1 WHERE id = 10005;
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655167784441-26e62a39-bfec-4e9d-b351-7fecdac70305.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=142&id=u1930bcfd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=142&originWidth=862&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45218&status=done&style=none&taskId=u68b19279-cbb7-4cf7-adc5-820d3707725&title=&width=862)

- eq_ref 
```sql
mysql> EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655167855839-78cc99d2-982b-43ae-9451-86d174ced8e4.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=154&id=ucd21495d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=154&originWidth=860&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66065&status=done&style=none&taskId=u744f2d5d-bc8a-4daf-b46f-69a156ce57b&title=&width=860)
从执行计划的结果中可以看出，MySQL打算将s2作为驱动表，s1作为被驱动表，重点关注s1的访问
方法是 eq_ref ，表明在访问s1表的时候可以 通过主键的等值匹配 来进行访问。

- ref 
```sql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655167903898-87009022-5f51-4b0e-809c-ec5f675c77a6.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=142&id=u6f439c88&margin=%5Bobject%20Object%5D&name=image.png&originHeight=142&originWidth=868&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46144&status=done&style=none&taskId=u05854b46-564e-43d7-b3e5-799e6aef213&title=&width=868)

- fulltext  全文索引
- ref_or_null 
```sql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key1 IS NULL;
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655167950479-b3d65b91-b9cc-405a-8d12-4a124bceeef6.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=121&id=u87948048&margin=%5Bobject%20Object%5D&name=image.png&originHeight=121&originWidth=871&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41284&status=done&style=none&taskId=u0017dcd6-420b-4b90-8860-83181f19feb&title=&width=871)

- index_merge 
```sql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655168027611-d75eed88-21c1-4372-90b5-95365be205f6.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=100&id=uee0c664f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=100&originWidth=875&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32128&status=done&style=none&taskId=u156258da-917d-4a59-813b-0c1b4c19eb3&title=&width=875)
从执行计划的 type 列的值是 index_merge 就可以看出，MySQL 打算使用索引合并的方式来执行
对 s1 表的查询。

- unique_subquery 
```sql
EXPLAIN SELECT * FROM s1 WHERE key2 IN (SELECT id FROM s2 where s1.key1 =
s2.key1) OR key3 = 'a';
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655168065654-f211fca2-4f37-4779-9231-29d69a64c964.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=140&id=u41615315&margin=%5Bobject%20Object%5D&name=image.png&originHeight=140&originWidth=870&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47848&status=done&style=none&taskId=ud64b8190-9fd4-4160-ade6-65b09b047f7&title=&width=870)

- index_subquery 
```sql
EXPLAIN SELECT * FROM s1 WHERE common_field IN (SELECT key3 FROM s2 where
s1.key1 = s2.key1) OR key3 = 'a';
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655168113091-f70da584-77bb-494b-9df3-0e940d5eaa41.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=145&id=uabcdc264&margin=%5Bobject%20Object%5D&name=image.png&originHeight=145&originWidth=881&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43839&status=done&style=none&taskId=ue6cc4b96-96d2-4879-9db5-4e80c671efa&title=&width=881)

- range  
```sql
EXPLAIN SELECT * FROM s1 WHERE key1 IN ('a', 'b', 'c');
或者：
EXPLAIN SELECT * FROM s1 WHERE key1 > 'a' AND key1 < 'b';
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655168151210-909758b8-049b-48c9-a8a9-1d34f1a3518c.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=124&id=u0fd75509&margin=%5Bobject%20Object%5D&name=image.png&originHeight=124&originWidth=867&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41637&status=done&style=none&taskId=u9b1748fc-223b-4027-8d77-cf032cc2af0&title=&width=867)

- index  
```sql
EXPLAIN SELECT key_part2 FROM s1 WHERE key_part3 = 'a';
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655168206489-a17388e2-798b-4958-8cf2-077fdabbb3cf.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=125&id=u09ad3cc3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=125&originWidth=866&originalType=binary&ratio=1&rotation=0&showTitle=false&size=40234&status=done&style=none&taskId=u2f5a4c5c-e447-4b82-9a72-e57729a01db&title=&width=866)

- ALL 
```sql
EXPLAIN SELECT * FROM s1;
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22219483/1655168224197-921c62ad-e7e5-42a9-8376-9025454e6256.png#clientId=u9411fcd9-2643-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=147&id=u0e783fad&margin=%5Bobject%20Object%5D&name=image.png&originHeight=147&originWidth=886&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42923&status=done&style=none&taskId=u9ca40b91-3c37-4f03-bcb1-b50b259acac&title=&width=886)
**小结：**
**结果值从最好到最坏依次是： system > const > eq_ref > ref > fulltext > ref_or_null > index_merge >**
**unique_subquery > index_subquery > range > index > ALL 其中比较重要的几个提取出来（见上图中的蓝**
**色）。SQL 性能优化的目标：至少要达到 range 级别，要求是 ref 级别，最好是 consts级别。（阿里巴巴**
**开发手册要求）**
## 22 .索引相关高频面试题
> 1. 索引是什么? 索引优缺点? 
>    - 索引类似于目录, 进行数据的快速定位
>    - 优点: 加快数据检索速度
>    - 缺点: 创建索引和维护索引需要消耗空间和时间
> 
 
> 2. MySQL索引类型 
>    - 按存储结构划分 : B+Tree索引, Hash索引, FULLINDEX全文索引, R-TREE索引
>    - 按应用层次划分: 普通索引, 唯一索引, 联合索引, 聚簇索引, 非聚簇索引
> 
 
> 3. 索引底层实现? 为什么使用B+树, 而不是B树, BST, AVL, 红黑树等等?
> 4. 什么是聚簇索引和非聚簇索引?
> 5. 非聚簇索引一定会回表吗?
(不一定, 覆盖索引不会回表)
> 6. 什么是联合索引?为什么需要注意联合索引中的字段顺序?
> 7. 什么是最左前缀原则?
> 8. 什么是前缀索引?
> 9. 什么是索引下推?
> 10. 如何查看MySQL语句是否使用到索引?
EXPLAIN SQL语句
possible_key: 可能用到的索引(可以查看是否有冗余索引)
key: 真正使用到的索引
> 11. 为什么建议使用自增主键作为索引?
(索引维护可能造成页分裂, 自增主键减少数据的移动和分裂)
> 12. 建立索引的原则 
>    - 建立索引的字段最好为NOT NULL
>    - 索引字段占用空间越小越好
>    - 最左匹配原则
>    - =和in建立索引时顺序可以任意, 比如a = 1 and b = 2 and c = 3 建立(a, b, c)和(b, a, c)索引效果是一样的, MySQL查询优化器会进行优化
>    - 建立的索引让索引的选择性尽可能接近1, 唯一索引的索引选择性为1
>    - 尽量扩展索引, 不要让索引冗余, 如有SQL需要对单个a进行索引, 那么上述条件建立的索引应该为(a, b, c)或(a, c, b)
>    - 索引列不能参与计算
> 
 
> 13. 什么情况下索引失效? 
>    - 使用 != 或  <>
>    - 类型不一致导致索引失效
>    - 函数导致的索引失效, 函数用在索引列时, 不走索引
如 `SELECT * FROM t WHERE DATE(create_time) = 'yyyy-MM-dd'`
>    - 运算符导致的索引失效
如 `SELECT * FROM t WHERE k - 1 = 2`, 若有INDEX(k), 则不走索引
>    - OR引起的索引失效
如 `SELECT * FROM t WHERE k = 1 OR j = 2`, 若有INDEX(k), 则不走索引, 如果OR连接的时同一个字段, 则不会失效
>    - 模糊查询导致的索引失效
如 `SELECT * FROM t WHERE name = '%三'`, %放字符串字段前匹配不走索引
>    - NOT IN, NOT EXISTS导致索引失效


 
