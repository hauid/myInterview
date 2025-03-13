#### Arrays.asList() 

该方法是将数组转化成List集合的方法



15.三数之和

//将数组转化为list加入到一个更大的list中

```java
List<List< Integer >> res=new ArrayList<>();

res.add(Arrays.asList(nums[i],nums[right],nums[left]));
```



Arrays

​	使用 `Arrays.asList()` 方法将数组转换为列表时，你得到的是一个固定大小的列表。这个列表是基于原始数组的，所以它支持对列表元素的修改（这些修改会直接影响原始数组），**但不支持添加或删除元素**。尝试向这种列表添加或删除元素将会抛出 `UnsupportedOperationException`

​	Arrays.asList() 返回的列表是一个对原始数组的固定大小的视图。它适合于那些不需要添加或删除元素，只需要访问或修改列表元素的场景。如果需要一个可以灵活修改的列表，应该使用 **new ArrayList<>(Arrays.asList(array))**来创建一个真正的 ArrayList

```java
 public static void main(String[] args) {
        String[] array = {"Apple", "Banana", "Cherry"};
        List<String> list = new ArrayList<>(Arrays.asList(array));

        // 现在可以添加或删除元素
        list.add("Date");
        list.remove("Apple");
        System.out.println(list);  // 输出：[Banana, Cherry, Date]
    }
```



**Java栈使用**

Stack< Character > st=new Stack<>();	//建栈

查看栈顶元素	st.peek()

取栈顶元素		st.pop();









DTO

​        Data Transfer Object：数据传输对象，DTO用于在不同层之间传输数据，它通常用于将业务逻辑层（Service层）的数据传输给表示层（Presentation层）或持久化层（Persistence层）。DTO对象通常包含业务领域的数据，但不包含业务逻辑。



三层架构	(3-tier application)通常意义上的三层架构就是将整个业务应用划分为：

表现层（UI）、业务逻辑层（BLL）、数据访问层（DAL）。区分层次的目的即为了“高内聚，低耦合”的思想
