## MySQL-索引
### 一 、什么是索引?

#### 概念
> 索引时存储引擎快速找到记录的一种数据结构.
#### 优缺点
优点
> - 索引减少了服务器需要扫描的数据量 
> - 索引帮助服务器避免排序和临时表
> - 索引可以将随机I/O变为顺序I/O

索引不总是最好的解决方案
> 索引的建立和维护需要占用资源,带来额外的工作
> - 对于非常小的表,大部分情况下全表扫描更高效
> - 对于中型和大型表,索引非常有效
> - 对于特大型表,建立和使用索引的代价随之增加.此时需要分区技术

### 二、高性能的索引策略
#### 1.独立的列
> where 条件语句下,不要使用复杂的表达式,函数,应当将索引列单独放在表达式的一侧.

两个错误示例:
``` mysql
select * from table where actor_id +1=5;  (1)
sleect * from table where TO_DAYS(CURRENT_DATE)-TO_DAYS(date_col) <=10; (2)
```

#### 2.前缀索引和索引的选择性
对于很长的字符列(BLOB,TEXT,很长的VARCHAR)，加索引会使得索引变得大而且慢，这个时候出了哈希索引之外，可以使用前缀索引(索引列开始的的部分字符)。

**索引的选择性**：
> 不重复的索引值和数据表的记录总数的比值，范围即0~1； 索引的选择性越高，查询效率越高，唯一索引的选择性是1。

计算索引的选择性：
``` mysql
select count(distinct city_name)/count(*) from city_demo;
select count(distinct LEFT(city_name,5))/count(*) from city_demo;
// 通过比较不同的前缀长度的选择性,选出合适的前缀,兼顾效率和索引空间大小.
```



#### 3.多列索引(联合索引)
对于where条件中的每个列建立独立的索引,大部分情况不能带来更高的查询性能.在`MySQL`5.0版本一以前,多个独立的索引列只会生效一个.但是在5.0版本以及以后的更新版本中带来'索引合并'策略,能够一定程度上使多个索引生效.`索引合并`策略能够使多个单独的索引进行扫描,并将结果进行合并,即使用 union.

总结:
> 索引合并策略是MySQL优化策略,但是一旦被优化,也代表着当前索引建的非常糟糕.
> - 当出现服务器对多个索引做相交操作时(通常会有多个AND条件),意味着需要一个包含所有相关列的多列索引.
> - 当需要对多个索引做联合操作时(通常有多个OR条件),通常需要消耗大量的CPU和内存资源在算法的缓存,排序和合并操作. 但是,优化器不会把这些算在查询成本当中.因此可能会低估查询成本,导致不如全表扫描的效率更高 

#### 4.选择合适的索引顺序
基于多列索引讨论

正确的索引列的顺序依赖于使用该索引的查询,并且同时需要考虑如何更好的满足排序和分组的需要.(言简意赅,具体问题具体分析)

**经验法则**
> 将选择性最高的列放在最前列.

> 但是需要具体问题具体分析,同样要考虑避免随机I/O和排序.

> 如果不需要考虑排序和分组,将选择性最高的列放在最前面是极好的. 但是同样需要注意值的分布也会影响效率.


讨论 :值的分布

> 某些特殊值,频次极高,(非注册用户guest,所有未登陆的用户都是这个guest,所有的消息等信息都和guest绑定,所以在一些信息表中,guest频次极高)几乎等同于整张数据表的记录数,那么即使是加了索引,效率依然不会高.针对这种特殊值的解决办法就是修改应用程序代码,禁止针对这种特殊值执行这个查询.





#### 5.聚簇索引
[引用简书:浅谈聚簇索引和非聚簇索引](https://www.jianshu.com/p/fa8192853184)

##### 1.什么是 **聚簇索引**?
> `Innodb`的`table`上都会有一个存储了行数据的索引,称之为聚簇索引.

##### 2.常见的存在方式
> - 如果`Innodb`表存在主键索引,那么该主键就会作为聚簇索引 
> - 如果`Innodb`表不存在主键索引,会使用表中的`unique`索引作为聚簇索引,但是该索引要是`not null` .
> - 如果上述条件都不满足,那么`innodb`会生成一个隐士的索引`GEN_CLUST_INDEX ` ,把它作为聚簇索引,该索引存放`ROW ID`的值. `row id`是表中单调增长的6个字节的filed字段.

##### 3.聚簇索引和非聚簇索引
![聚簇索引和非聚簇索引](/images/索引-聚簇索引-001.PNG)

- 如左图所示: InnoDB使用的是聚簇索引，将主键组织到一棵B+树中，而行数据就储存在叶子节点上，若使用"where id = 14"这样的条件查找主键，则按照B+树的检索算法即可查找到对应的叶节点，之后获得行数据。
若对Name列进行条件搜索，则需要两个步骤：第一步在辅助索引B+树中检索Name，到达其叶子节点获取对应的主键。第二步使用主键在主索引B+树种再执行一次B+树检索操作，最终到达叶子节点即可获取整行数据。（重点在于通过其他键需要建立辅助索引）
   * 聚簇索引具有唯一性，由于聚簇索引是将数据跟索引结构放到一块，因此一个表仅有一个聚簇索引。
   * 表中行的物理顺序和索引中行的物理顺序是相同的，在创建任何非聚簇索引之前创建聚簇索引，这是因为聚簇索引改变了表中行的物理顺序，数据行按照一定的顺序排列，并且自动维护这个顺序.

- 如右图所示: MyISAM使用的是非聚簇索引,索引树和表数据存储在两个地方,索引上的叶子节点存储的都是行数据的地址,在查找的过程中,不需要二次检索B+树(不需要再访问主键索引).
- **优缺点**:
  
    * 因为索引直达包含行数据的页,所以查询速度快.
    * 表的数据量比较大时,聚簇索引可以减少I/O次数.
    * 按页I/O数据时,对于已经加载内存的数据,查询速度是快的.
    * 辅助索引存放的是主键索引值,一般主键索引时Integr自增长的,数据长度小,减少了辅助索引占用的内存空间
    * 因为MyISAM的主索引并非聚簇索引，那么他的数据的物理地址必然是凌乱的，拿到这些物理地址，按照合适的算法进行I/O读取，于是开始不停的寻道不停的旋转。聚簇索引则只需一次I/O
    * 不过，如果涉及到大数据量的排序、全表扫描、count之类的操作的话，还是MyISAM占优势些，因为索引所占空间小，这些操作是需要在内存中完成的。
##### 4.聚簇索引使用的注意点
* 使用主键为聚簇索引时，主键最好不要使用uuid，因为uuid的值太过离散，不适合排序且可能出线新增加记录的uuid，会插入在索引树中间的位置，导致索引树调整复杂度变大，消耗更多的时间和资源
* 建议使用int类型的自增，方便排序并且默认会在索引树的末尾增加主键值，对索引树的结构影响最小。而且，主键值占用的存储空间越大，辅助索引中保存的主键值也会跟着变大，占用存储空间，也会影响到IO操作读取到的数据量。
* 聚簇索引的数据的物理存放顺序与索引顺序是一致的，即：只要索引是相邻的，那么对应的数据一定也是相邻地存放在磁盘上的。如果主键不是自增id，那么可以想象，它会不断地调整数据的物理地址、分页，当然也有其他一些措施来减少这些操作，但却无法彻底避免。但，如果是自增的，那就简单了，它只需要一页一页地写，索引结构相对紧凑，磁盘碎片少，效率也高。





#### 6.覆盖索引

##### 1.什么是**覆盖索引**？

> 概念：查询语句中所需要的列在索引中，这样查询结果在索引的数据结构中查找即可拿到结果。

附加网友解释：

- 解释一： 就是select的数据列从索引中就能够获取，不必从数据表中再次读取，换句话说，就是查询列可以索引福噶
- 解释二：索引是高效找到行的一个方法，当能通过检索索引就可以读取想要的数据，那就不需要再到数据表中读取行了。如果一个索引包含了（或覆盖了）满足查询语句中字段与条件的数据就叫做覆盖索引。
- 解释三：是非聚集组合索引的一种形式，它包括在查询里的select，join和where子句用到的所有列（即建立索引的字段正好是覆盖查询语句[select子句]与查询条件[where子句]中所涉及的字段，也即，索引包含了查询正在查找的所有数据）。

##### 2.形成覆盖索引的条件

> 索引分为多种类型，从数据结构上分为 二叉树、红黑树、 Hash索引、B-Tree索引，B+Tree（MySQL使用的存储结构）

索引的实现可以使用多种数据结构，这里使用B-Tree和B+Tree的索引能实现覆盖索引。

##### 3.如何查看是否使用了覆盖索引？

> 使用 `explain`命令，通过查看 `Extra` 列可以看到`Using index` 的信息，证明使用了**覆盖索引** 

##### 4.如何使用覆盖索引优化SQL

> 先扯出来一个概念
>
> **回表**：先索引扫描，再通过ID去查表数据，取索引中未能提供的数据，即为回表。
>
> 简单来说就是数据库根据索引找到了指定的记录所在行后，还需要根据rowid(Innodb引擎是根据主键id在聚簇索引中查询)再次到数据块里取数据的操作
>
> 如何避免回表？
>
> 条件允许的情况下，使用**联合索引**

- 使用`explain` 命令，`Extra` 列出现`Using index condition` 表示使用的索引方式为二级检索，数据将会被回表查询，会出现一定的性能消耗。
- 分页查询时，通过`组合索引`，能够实现`覆盖索引`.
- 所需的查询列比价多,无法覆盖,这个时候想使用覆盖索引 
```mysql
select * from product join (select product_id from product where product_name='产品1' and title='标题') as p1 on product.product_id = p1.product_id;
// 这种查询方式在子查询的结果集较少的情况下,可能会降低查询效率
```
##### 5.总结

对于`覆盖索引`，通过联合索引可能实现覆盖索引，但是该情况只限于对于所需的查询列比较少的情况，所需的查询列比较多的情况，不可能全部实现`联合索引`，所以不能实现覆盖索引。

#### 7.使用索引扫描排序
使用`EXPLAIN`出来的type列的值为"index",则说明MySQL使用了索引扫描来做排序.

扫描索引为顺序扫描,比较快.

但是如果索引列不满足所需要的查询列,则需要回表查询数据,而回表是随机I/O,会比较慢.

**使用索引扫描排序的条件:**
- 只有当索引的列顺序和ORDER BY 子句的顺序完全一致的情况下,并且所有的列的排序方向(倒序或正序)都一样时,MySQL才能使用索引扫描排序.等同于查询语句中的最左前缀原则.
- 一种例外,前导列为常量时,ORDER BY子句在不满足索引的最左前缀原则的情况下可以索引扫描排序.
```mysql
table retal 有一个多列索引(rental_date,inventory_id,customer_id)
例子:select .... from product where rental_date='2021-06-29 17:08' order by inventory_id,customer_id;

以下情况无法使用索引扫描
 // 不同排序方向
 where rental_date='2021-06-29 17:08' order by inventory_id desc,customer_id asc;
 
 // order by 子句引用不在索引中的列
 where rental_date='2021-06-29 17:08' order by inventory_id,staff_id;
 
 // 不满足最左前缀,子句中的引用列缺失索引列
  where rental_date='2021-06-29 17:08' order by customer_id;

 // 第一列是范围查询
 where rental_date>'2021-06-29 17:08' order by inventory_id,customer_id;

 // 第二列也是范围查询
 where rental_date='2021-06-29 17:08' and inventory_id in (1,2) order by customer_id;

```

#### 8.压缩索引
MyISAM使用前缀压缩来减少索引的大小

#### 9.冗余和重复索引
**重复索引** :在相同的列上按照相同的顺序创建相同类型的索引
**冗余索引**:多列索引覆盖单列索引或者多列索引 例如 索引(A,B) 那么索引(A)则为冗余索引

**建议**
> 应当尽量扩展已有的索引而不是创建新的索引
> 重复索引和冗余索引会导致索引增加,插入速度原来越慢

#### 10.未使用的索引
删除未使用的索引


#### 11.索引和锁
因为索引所以锁住了更少的行，提高了并发

### 三、案例学习总结

- 对于查询经常用到但是选择性不高的列，可以加入索引中，如果Where条件中用不到，可以通过IN()的方法覆盖该列，完成前缀索引，但是in的组合不能太多，多了同样会影响查询效率。举例索引（sex，country，age）。
- 避免多个范围条件，多个范围条件无法同时使用。无法直接解决。但是可以将其中一个范围查询转换为一个简单的等值比较。如果一个条件的选择性不高，可以直接筛选不加入索引，缺乏合适的索引对该查询的影响不明显
- 优化排序，使用覆盖索引查询返回需要的逐渐，再根据主键关联原表获取需要的行（延迟关联）。




