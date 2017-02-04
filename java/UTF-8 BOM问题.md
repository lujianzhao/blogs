###准备工作
这里使用NOTEPAD++编辑器做实验,新建a.txt,选择菜单格式(M)->以UTF-8 无BOM格式编码。
```
D:/a.txt
aaa
```
```java
public class TestBom {

    public static void main(String[] args) {
        InputStream in = null;
        try {
            in = new FileInputStream("D:/a.txt");
            byte[] b = new byte[1024];
            int count = 0;
            if((count = in.read(b)) != -1) {
                for(int i=0; i<count; i++) {
                    System.out.print(b[i] + ",");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if(in != null) {
                    in.close();
                    in = null;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
* 无BOM格式
```
Console输出：
97,97,97,
```

* BOM格式
选择菜单格式(M)->以UTF-8 格式编码
```
Console输出：
-17,-69,-65,97,97,97,
```

**看到没带BOM格式比不带BOM格式多出了三个字符-17,-69,-65,具体是什么有待深入。
Java平台最好不要带BOM,尤其是Java配置文件,有次就被NOTEPAD++坑过,以UTF-8 格式编码一直在报错,找了N久才知道这个问题。
要找一款自己熟悉,用的顺手的编辑器。**