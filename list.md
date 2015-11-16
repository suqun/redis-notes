##Redis List笔记

####Lpush
- Lpush key value [value …]
- 向列表左边增加元素，返回增加元素后的列表长度；支持同时增加多个元素

####Lrange
- Lrange key start end
- 获取列表中的某一片段，返回索引从statr到end之间的所有元素；索引从0开始。Lrange也支持负索引，-1表示列表最右边的元素，-2表示列表右边倒数第二个元素。

####Rpush
- Rpush key value [value …]
- 向列表右边添加元素，返回增加元素后的列表长度；支持同时增加多个元素

####Linsert
- Linsert key BEFORE | AFTER pivot value
- 在列表查找值为pivot的元素，然后根据第二个元素是before还是after来决定将value插入到前面还是后面。返回值是插入元素后的列表长度。

####Lpop / Rpop
- Lpop/Rpop key
- Lpop 从列表左边弹出一个元素； Rpop从列表右侧弹出一个元素。Lpop和Lpush配合，Rpop和Rpush配合可以把列表当作栈使用；Lpush和Rpop配合，Rpush和Lpop配合可以把列表当作队列使用。

####Lrem
- lrem key count vlaue
- 删除列表中前count个值为value的元素，返回值为实际删除元素的个数。
    1. 当count>0时，从左边开始删除
    2. 当count<0时，从右边开始删除
    3. 当count=0时，删除所有值为value的元素

####Ltrim
- ltrim key start end
- 删除列表**指定范围外**的所有元素，返回值为指定索引内的元素。Ltrim常和Lpush一起使用来限制列表中元素的数量，比如记录日志时只希望保留最近的100条记录，则每次添加日志时调用一次ltrim命令即可。

####RpopLpush
- RpopLpush src dest
- 先从src列表的右边弹出一个元素，然后将其加入到dest列表的左边，并返回这个元素的值。当把列表当作队列时，RpopLpush可以在多个队列中传递数据。当src和dest相同时，此命令会不断的将队尾的元素移动到队首。

####Lindex
- Lindex key index 
- 返回指定索引的元素，索引从0开始

####Lset
- Lset key index value
- 将索引为index的元素赋值为value

####Llen
- Llen key
- 获取列表中元素的个数，key不存在时返回0
