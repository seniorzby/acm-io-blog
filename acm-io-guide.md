# ACM模式Java输入输出技术详解

> 在算法竞赛（如ACM、LeetCode、牛客网等）中，正确高效地处理输入输出是解决问题的基础。本文详细介绍了Java中处理ACM模式输入输出的各种技术和注意事项。

## 目录

- [基础知识](#基础知识)
- [常见输入模式](#常见输入模式)
  - [模式1：读取固定行数据](#模式1读取固定行数据)
  - [模式2：读取未知行数据直到EOF](#模式2读取未知行数据直到eof)
  - [模式3：读取多组测试用例](#模式3读取多组测试用例)
  - [模式4：读取空格分隔的多个数据](#模式4读取空格分隔的多个数据)
  - [模式5：读取特定格式数据](#模式5读取特定格式数据)
- [高效输入输出](#高效输入输出)
  - [Scanner vs BufferedReader](#scanner-vs-bufferedreader)
  - [System.out vs PrintWriter](#systemout-vs-printwriter)
  - [性能对比](#性能对比)
- [常见陷阱与解决方案](#常见陷阱与解决方案)
  - [Scanner的nextInt()后跟nextLine()问题](#scanner的nextint后跟nextline问题)
  - [处理空行](#处理空行)
  - [处理多余空格](#处理多余空格)
- [实战模板](#实战模板)
  - [基础模板(Scanner)](#基础模板scanner)
  - [高性能模板(BufferedReader)](#高性能模板bufferedreader)
- [最佳实践总结](#最佳实践总结)
- [参考资料](#参考资料)

## 基础知识

在ACM竞赛和算法平台中，输入输出通常遵循特定的格式。Java中处理输入输出主要使用以下类：

| 类名 | 用途 | 优点 | 缺点 |
| --- | --- | --- | --- |
| `Scanner` | 读取并解析输入 | 使用简单，功能丰富 | 性能较差 |
| `BufferedReader` | 高效读取输入 | 性能好 | 只能读取字符串，需要手动解析 |
| `StringTokenizer` | 分割字符串 | 简单直观 | 功能有限 |
| `System.out` | 标准输出 | 使用方便 | 无缓冲，性能较差 |
| `PrintWriter` | 高效输出 | 有缓冲，性能好 | 需要手动flush |

## 常见输入模式

### 模式1：读取固定行数据

在很多ACM题目中，输入的第一行通常是一个整数n，表示接下来有n行数据。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 读取数据行数
        int n = scanner.nextInt();
        
        // 消费换行符（重要！）
        scanner.nextLine();
        
        // 读取并处理n行数据
        for (int i = 0; i < n; i++) {
            String line = scanner.nextLine();
            // 处理每行数据...
            System.out.println("处理结果: " + line);
        }
        
        scanner.close();
    }
}
```

### 模式2：读取未知行数据直到EOF

有些题目不会明确指定输入行数，需要一直读取直到文件结束（EOF, End Of File）。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 一直读取直到没有更多输入
        while (scanner.hasNext()) {
            String line = scanner.nextLine();
            // 处理数据...
            System.out.println("处理: " + line);
        }
        
        scanner.close();
    }
}
```

### 模式3：读取多组测试用例

有时题目会包含多个测试用例，每个测试用例都有自己的格式。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 读取测试用例数量
        int t = scanner.nextInt();
        
        // 处理每个测试用例
        for (int caseNum = 1; caseNum <= t; caseNum++) {
            // 读取当前测试用例的输入行数
            int n = scanner.nextInt();
            
            // 读取并处理当前测试用例的n行数据
            int[] data = new int[n];
            for (int i = 0; i < n; i++) {
                data[i] = scanner.nextInt();
            }
            
            // 处理数据并输出结果
            int sum = 0;
            for (int num : data) {
                sum += num;
            }
            
            System.out.println("Case #" + caseNum + ": " + sum);
        }
        
        scanner.close();
    }
}
```

### 模式4：读取空格分隔的多个数据

很多题目的输入是每行包含多个以空格分隔的数据。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 一直读取直到没有更多输入
        while (scanner.hasNext()) {
            // 读取一行中的两个整数
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            
            // 处理数据
            System.out.println(a + " + " + b + " = " + (a + b));
        }
        
        scanner.close();
    }
}
```

更高效的方法使用BufferedReader：

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line;
        
        while ((line = br.readLine()) != null) {
            // 使用空格分割字符串
            String[] parts = line.split(" ");
            
            // 转换为整数并处理
            int a = Integer.parseInt(parts[0]);
            int b = Integer.parseInt(parts[1]);
            
            System.out.println(a + " + " + b + " = " + (a + b));
        }
        
        br.close();
    }
}
```

### 模式5：读取特定格式数据

有时输入具有特定格式，如CSV或自定义分隔符。

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        while (scanner.hasNextLine()) {
            String line = scanner.nextLine();
            
            // 按逗号分割
            String[] parts = line.split(",");
            
            // 处理每个部分
            for (String part : parts) {
                System.out.print(part.trim() + " | ");
            }
            System.out.println();
        }
        
        scanner.close();
    }
}
```

## 高效输入输出

### Scanner vs BufferedReader

对于大量数据输入，BufferedReader通常比Scanner快10-100倍：

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = br.readLine();
        
        // 方法1：使用StringTokenizer分割字符串
        StringTokenizer st = new StringTokenizer(line);
        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());
        
        // 方法2：使用split分割字符串
        // String[] parts = line.split(" ");
        // int a = Integer.parseInt(parts[0]);
        // int b = Integer.parseInt(parts[1]);
        
        System.out.println(a + b);
        br.close();
    }
}
```

### System.out vs PrintWriter

对于大量输出，PrintWriter通常比System.out快：

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(new BufferedOutputStream(System.out));
        
        String line;
        while ((line = br.readLine()) != null) {
            // 处理输入...
            
            // 使用PrintWriter输出
            out.println("结果: " + line);
        }
        
        br.close();
        out.flush(); // 重要！确保所有数据都被写出
        out.close();
    }
}
```

### 性能对比

以下是处理100万行数据时的粗略性能对比：

| 输入方法 | 近似处理时间 |
| --- | --- |
| Scanner | ~2500ms |
| BufferedReader + split | ~800ms |
| BufferedReader + StringTokenizer | ~500ms |

| 输出方法 | 近似处理时间 |
| --- | --- |
| System.out.println | ~1800ms |
| PrintWriter | ~650ms |

## 常见陷阱与解决方案

### Scanner的nextInt()后跟nextLine()问题

使用Scanner的nextInt()后立即使用nextLine()会跳过输入，这是因为nextInt()不消费换行符：

```java
Scanner scanner = new Scanner(System.in);

int n = scanner.nextInt(); // 读取整数
// scanner.nextLine(); // 需要添加这行来消费换行符！
String s = scanner.nextLine(); // 如果不加上面那行，会立即返回空字符串！

System.out.println("n = " + n);
System.out.println("s = '" + s + "'");
```

### 处理空行

有些题目的输入可能包含空行，需要特别处理：

```java
while (scanner.hasNextLine()) {
    String line = scanner.nextLine();
    
    // 处理空行
    if (line.isEmpty()) {
        continue; // 或者做其他处理
    }
    
    // 处理非空行...
}
```

### 处理多余空格

有时输入中会有多余的空格，可以使用trim()和正则表达式处理：

```java
String line = scanner.nextLine().trim(); // 移除前后空格
String[] parts = line.split("\\s+"); // 分割任意数量的空白字符
```

## 实战模板

### 基础模板(Scanner)

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 处理输入
        while (scanner.hasNext()) {
            // 读取数据
            // ...
            
            // 处理逻辑
            // ...
            
            // 输出结果
            // ...
        }
        
        scanner.close();
    }
}
```

### 高性能模板(BufferedReader)

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(new BufferedOutputStream(System.out));
        
        String line;
        while ((line = br.readLine()) != null && !line.isEmpty()) {
            StringTokenizer st = new StringTokenizer(line);
            
            // 读取数据
            // ...
            
            // 处理逻辑
            // ...
            
            // 使用PrintWriter输出结果
            // out.println(...);
        }
        
        br.close();
        out.flush();
        out.close();
    }
}
```

## 最佳实践总结

1. **选择合适的工具**：
   - 小数据量或简单题目：使用Scanner
   - 大数据量或竞赛题目：使用BufferedReader + StringTokenizer

2. **资源管理**：
   - 使用try-with-resources或者确保手动关闭资源
   - 使用PrintWriter时记得flush

3. **注意事项**：
   - 记得处理Scanner的nextInt()后nextLine()问题
   - 处理空行和多余空格
   - 考虑输入数据的边界情况

4. **代码组织**：
   - 将输入解析与业务逻辑分离
   - 使用合适的数据结构存储输入数据

## 参考资料

- [Java官方文档 - Scanner类](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)
- [Java官方文档 - BufferedReader类](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)
- [Java官方文档 - PrintWriter类](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html)
- [ACM竞赛Java输入输出优化指南](https://www.acwing.com/blog/content/277/)
- [LeetCode Java输入输出处理技巧](https://leetcode.com/discuss/interview-question/125160/java-input-output-in-online-assessments)

---

> 作者：周博羽  
> 日期：2025年3月5日  
