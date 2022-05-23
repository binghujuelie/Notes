# Java的ACM模式输入输出及性能优化

---

[TOC]

## 使用Scanner

### 1. 输入整数、字符串数组

第一行输入n, m

第二行输入n个整数

第三行输入m个字符串

```java
//导入包
import java.util.Scanner;
import java.util.Arrays;
 
public class MyScanner {
 
	public static void main(String[] args) {
		
		//创建对象
		Scanner sc = new Scanner(System.in);		
		System.out.println("输入数据:");	
		//多行输入
		int n = sc.nextInt();
		int m = sc.nextInt();
		int[] arr = new int[n];	
		String[] str = new String[m];
		
		//int等基本数据类型的数组,用nextInt()，同行或不同都可以
		for(int i=0; i<n; i++) {
			arr[i] = sc.nextInt();
		}
		//String字符串数组, 读取用next()，以空格划分
		for(int i=0; i<m; i++) {
			str[i] = sc.next();
		}
		
        //调用方法进行操作
		TestSc(n, m, arr);
		TestStr(str);
		
		System.out.println("Test01 End");
		
		//关闭
		sc.close();
	}
	
	public static void TestSc(int n, int m, int[] arr) {
		System.out.println("数据n：" + n + ", 数据m：" + m);
		System.out.println(Arrays.toString(arr));
	}
	
	public static void TestStr(String[] str) {
		System.out.println(Arrays.toString(str));
	}
		
}
```

​	若输入的字符串中想要包含空格，使用`Scanner.nextLine()`换行后用`Scanner.nextLine()`进行读入，见[情形7](#7. 换行输入数字和字符串(需要包含空格))。

### 2. 输入二维数组

第一行输入n，m

第二行开始输入二维数组。

```java
import java.util.Arrays;
import java.util.Scanner;
 
public class MyScanner2 {
 
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);		
		System.out.println("输入数据:");	
 
		//二维数组
		int n = sc.nextInt();
		int m = sc.nextInt();
		int[][] arr2 = new int[n][m];	
		System.out.println("Test02 输入二维数组数据：");
 
		//可以直接读入
		for(int i=0; i<n; i++) {
			for(int j=0; j<m; j++) {
				arr2[i][j] = sc.nextInt();
			}
		}
 
		TestSc(n, m, arr2);
		//关闭
		sc.close();
	}
	
	public static void TestSc(int n, int m, int[][] arr) {
		System.out.println("数据n：" + n + ", 数据m：" + m);
		for(int i=0; i<n; i++) {
			System.out.println(Arrays.toString(arr[i]));
		}
		System.out.println("数组行数： arr.length= "+ arr.length);
		System.out.println("数组列数： arr[0].length= "+ arr[0].length);
	}
	
}
```

### 3. 输入字符串

输入字符串，用空格隔开

`next()`和`nextLine()`的区别

```java
import java.util.Scanner;
/*
 *next()读取到空白停止，在读取输入后将光标放在同一行中。
 *nextLine()读取到回车停止 ，在读取输入后将光标放在下一行。
 */
 
public class MyScanner3 {
 
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);		
		System.out.println("输入字符串:");		
		
		//next():只读取输入直到空格。
		String str = sc.next();
 
		//nextLine():读取输入，包括单词之间的空格和除回车以外的所有符号
		String str2 = sc.nextLine();
 
		System.out.println("str：" + str);
		System.out.println("str2：" + str2);
		
		//关闭
		sc.close();
	}
	
}
```

### 4. 输入字符串分割为数组

先用`Scanner.nextLine()`读入为字符串，再将字符串分割为字符数组或字符串数组。

```java
import java.util.*;
 
public class MyScanner4 {
 
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);	
		System.out.println("输入字符串数组：");
		
		String str;
		str = sc.nextLine();
		
		char[] ch = new char[str.length()];
		for(int i=0; i<str.length(); i++) {
			//用charAt();进行定位分隔
			ch[i] = str.charAt(i);
			System.out.println(ch[i] + " ");
		}
		System.out.println("END");
		
		//读入字符串后,用空格分隔为数组
		String[] strs = str.split(" ");
		System.out.println(Arrays.toString(strs));
 
	}
}
```

### 5. 连续输入数字和字符串

区别于[情形1](#1. 输入整数、字符串数组)，对于不能采用`for`循环方式获取`String`。采用[情形5](#5. 连续输入数字和字符串)、[情形6](#6. 换行输入数字和字符串)来处理。

采用`while(Scanner.hasNext())`循环实现连续输入。

格式：数字，空格，字符串

或者：数字，回车，字符串

```java
import java.util.Scanner;
 
public class MyScanner5 {
 
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		
		while(sc.hasNext()) {					
			int n = sc.nextInt();
			String str = sc.next();
			Tes(n, str);
		}
			
		sc.close();
	}
	
	public static void Tes(int n, String str) {
		System.out.println("n = " + n);
		System.out.println("str = " + str);	
		System.out.println("str.length = " + str.length());
	}
	
}
```

### 6. 换行输入数字和字符串

也采用`scanner.nextLine()`，将光标移到下一行。再继续读入字符串。

第一行输入整数n，m，第二行开始输入字符串。或

第一行输入整数n，第二行输入m，第三行开始输入字符串。

```java
import java.util.*;
 
public class MyScanner6 {
 
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int m = sc.nextInt();
		
		//注意！！！光标换到下一行
		sc.nextLine();
		
		String s = sc.nextLine();
		String str = sc.nextLine();
		
		System.out.println("n = " + n + " , m = " + m);
		System.out.println("s = " + s);
		System.out.println("str = " + str);
				
		sc.close();
	}
 
}
```

<span id="index7">7</span>

### 7. 换行输入数字和字符串(需要包含空格)

采用`Scanner.nextLine()`，将光标移动到下一行，再继续读入字符串。

第一行输入n，

第二行开始输入n行字符串，字符串中包含空格。

## 使用 BufferedReader

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
int n = Integer.parseInt(br.readLine());
String[] list = br.readLine().split(" ");
```
