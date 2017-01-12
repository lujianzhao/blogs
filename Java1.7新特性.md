```java
package com.anubis.test;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.HashMap;
import java.util.Map;

public class TestJDK7 {

 public static void main(String[] args) throws FileNotFoundException,
   IOException {
  // switch string
  String key = "dept";
  switch (key) {
  case "dept":
   System.out.println("dept....");
   break;
  case "person":
   System.out.println("person...");
   break;
  default:
   break;
  }

  // 直接声明2进制数
  int x = 0b1010;
  System.out.println(x);
  // 下划线数字
  int y = 1_000_000;
  System.out.println(y);

  // twr 字节流
  try (FileInputStream fis = new FileInputStream("d:/test.dmp")) {
   byte[] buffer = new byte[100];
   int count = 0;
   System.out.println("字节流：");
   while ((count = fis.read(buffer)) != -1) {
    System.out.println(count);
    System.out.println(new String(buffer, 0, count));
   }
   System.out.println();
  }

  // twr 字符流
  try (FileReader fr = new FileReader("d:/test.dmp")) {
   char[] buffer = new char[100];
   int count = 0;
   System.out.println("字符流");
   while ((count = fr.read(buffer)) != -1) {
    System.out.println(count);
    System.out.println(new String(buffer, 0, count));
   }
   System.out.println();
  }

  // twr使用，注意单个声明（字符流-->字节流）
  try (FileInputStream fis = new FileInputStream("d:/test.dmp");
    InputStreamReader is = new InputStreamReader(fis);
    BufferedReader br = new BufferedReader(is)) {
   String temp = null;
   while ((temp = br.readLine()) != null) {
    System.out.println(temp);
   }
  }

  // 钻石语法，自动泛型
  Map map = new HashMap<>();
 }
}
```