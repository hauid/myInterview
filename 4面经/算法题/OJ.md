树节点定义:

```java
public class TreeNode {

  int val;

  TreeNode left;

  TreeNode right;

  TreeNode() {}

  TreeNode(int val) { this.val = val; }

  TreeNode(int val, TreeNode left, TreeNode right) {

    this.val = val;

    this.left = left;

    this.right = right;

   }

}
```



输入:

```java
import java.util.Scanner

Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNextInt()) { // 注意 while 处理多个 case
            int a = in.nextInt();
            int b = in.nextInt();
            System.out.println(a + b);
            int[] arr = new int[n];	
		String[] str = new String[m];
		
		//int等基本数据类型的数组,用nextInt()，同行或不同都可以
		for(int i=0; i<n; i++) {
			arr[i] = sc.nextInt();
		}
		//String字符串数组, 读取用next()，以空格划分
		for(int i=0; i<m; i++) {
		//next():只读取输入直到空格。
			str[i] = sc.next();
        }
            
        //先用scanner.nextLine()读入字符串，再将字符串分割为字符数组或字符串数组
        System.out.println("输入字符串数组：");
		String str;
		str = sc.nextLine();
            

            
        }
```



Scanner in = new Scanner(System.in);

while(in.hasNextInt()){

​		int a = in.nextInt();

int b = in.N

}
