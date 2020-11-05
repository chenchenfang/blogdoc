# 遍历ArrayList时删除某一个元素

> 最近看面试题,遇到了这个问题 顺道整理下来吧 ,也不难

首先说下正确的做法:

```java
 ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);


//第一个方法,我觉得这个方法是OK的.
for (int i = 0; i < list.size(); i++) {
            list.remove(i);
            i--;
        }
//第二个就是使用 迭代器
Iterator<Integer> iterator = list.iterator();
        while (iterator.hasNext()){
            Integer next = iterator.next();
            System.out.println(next);
            if(next==3){
                iterator.remove();
            }

        }
```

还有两种方法是在for循环中,这种是错误的

```java
//这种foreach最终会编译成  迭代器的hasNext也就是下面#2的方法
//#1
for (Integer integer : list) {
            list.remove(integer);
        }

//#2
Iterator<Integer> iterator = list.iterator();
while(iterator.hasNext()) {
    Integer integer = iterator.next();
    list.remove(integer);
}

```

为什么这种是错误的呢,咱们去看下源码

list.iterator(); 返回的是一个new Itr(); 

![image-20201105224018832](/Users/ziang/Library/Application Support/typora-user-images/image-20201105224018832.png)

首先初始化的时候,expectedModCount等于的是ArrayList中的modCount,也就是修改次数,

这个时候如果ArrayList再次新增或者删除数据,会导致expectedModCount!=modCount

这样会导致异常

![image-20201105224234516](/Users/ziang/Library/Application Support/typora-user-images/image-20201105224234516.png)





list中的remove()会导致modCount+1,然后在使用Iterator中的next()就是抛出异常了

![image-20201105224631935](/Users/ziang/Library/Application Support/typora-user-images/image-20201105224631935.png)

![image-20201105224652272](/Users/ziang/Library/Application Support/typora-user-images/image-20201105224652272.png)

总结:

想在遍历中删除元素,最好使用Iterator中的remove().