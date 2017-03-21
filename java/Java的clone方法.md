```java
package com;

import java.util.Arrays;

public class Test {

    class Person implements Cloneable {
        private int id;
        private String name;
        private int[] ints;

        public Person() {
        }

        public int[] getInts() {
            return ints;
        }

        public void setInts(int[] ints) {
            this.ints = ints;
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }


        @Override
        public String toString() {
            return "Person [id=" + id + ", name=" + name + ", ints="
                    + Arrays.toString(ints) + "]";
        }

        @Override
        protected Object clone() throws CloneNotSupportedException {
            // 浅复制直接返回super.close()
            return super.clone();
            /* Person p = (Person) super.clone();
            // 深度复制需要每个引用成员都要复制,这里string并没有实现Cloneable接口
            // 使用起来基本没影响，或者暂时还不知道，但需要注意，
            // 使用时可能是自定义bean，可以实现Cloneable接口
            p.name = (String) name.clone();
            return p;*/
        }

    }

    public static void main(String[] args) {
        Person p = new Test().new Person();
        String a = "name";
        int[] ints = new int[]{100};
        p.setId(1);
        p.setName(a);
        p.setInts(ints);
        System.out.println(p);
        // 修改值后p2会变
        p.setId(2);
        System.out.println(p);
        // 复制引用
        Person p2 = p;
        System.out.println(p2 == p);

        // 复制对象
        try {
            Person p3 = (Person) p.clone();
            System.out.println(p3 == p);
            // 浅复制，string类型的还是同一个
            System.out.println(p3.getName() == p.getName());
            System.out.println(p3.getName().hashCode() +"--"+p.getName().hashCode());
            System.out.println(p3.getName() +"--"+p.getName());
            // 修改p的非引用类型值，不会影响p3的值
            a = "name name";
            // 修改p的引用类型值，会影响p3的值
            ints[0] = 31;
            System.out.println(p);
            System.out.println(p3);
            System.out.println(p3.getName() == p.getName());
            System.out.println(p3.getName() == p.getName());
            System.out.println(p3.getName().hashCode() +"--"+p.getName().hashCode());
            System.out.println(p3.getName() +"--"+p.getName());
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```

### 浅复制console输出
```
Person [id=1, name=name, ints=[100]]
Person [id=2, name=name, ints=[100]]
true
false
true
3373707--3373707
name--name
Person [id=2, name=name, ints=[310]]
Person [id=2, name=name, ints=[310]]
true
true
3373707--3373707
name--name
```
详情见这里 
http://www.2cto.com/kf/201401/273852.html

### 深度复制另一个解决办法
```java
package com;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.Arrays;

public class Test implements Serializable{

    class Person implements Serializable{
        private int age;
        private String name;
        private int[] ints;

        public Person() {
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int[] getInts() {
            return ints;
        }

        public void setInts(int[] ints) {
            this.ints = ints;
        }

        @Override
        public String toString() {
            return "Person [age=" + age + ", name=" + name + ", ints="
                    + Arrays.toString(ints) + "]";
        }

    }

    /**
     * 深度复制方法
     * @方法名:deepClone 
     * @参数 @param p
     * @参数 @return
     * @参数 @throws Exception  
     * @返回类型 Object
     */
    public static Object deepClone(Person p) throws Exception {
        ByteArrayOutputStream bout = new ByteArrayOutputStream();
        ObjectOutputStream out = new ObjectOutputStream(bout);
        out.writeObject(p);

        ByteArrayInputStream sin = new ByteArrayInputStream(bout.toByteArray());
        ObjectInputStream in = new ObjectInputStream(sin);
        return in.readObject();
    }

    public static void main(String[] args) throws Exception {
        Person p = new Test().new Person();
        p.setAge(30);
        p.setName("name");
        int[] ints = new int[]{100};
        p.setInts(ints);

        // 复制前
        System.out.println(p);

        // 复制后
        Person p1 = (Person) deepClone(p);
        System.out.println(p1);

        // 修改引用类型值
        ints[0] = 31;
        System.out.println(p);
        // 只有直接引用的p改变了,p1没有改变
        System.out.println(p1);
    }
}
```
```
console输出
Person [age=30, name=name, ints=[100]]
Person [age=30, name=name, ints=[100]]
Person [age=30, name=name, ints=[31]]
Person [age=30, name=name, ints=[100]]
```