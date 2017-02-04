```scala
/**
 * 数组操作
 */
object Test {

  def main(args: Array[String]): Unit = {
    var num = new Array[Int](10)
    var str = new Array[String](10)
    for (n <- num) print(n + ",")
    for (s <- str) print(s + ",")

    var arr = Array(1, 2, 4, "serical")
    arr(0) = "serical"
    for (a <- arr) print(a + ",")

    println
    var ab = new ArrayBuffer[Int]
    ab += 1
    ab += (100, 200)
    ab ++= Array(2, 3, 4)
    for (a <- ab) print(a + ",")

    // 移除最后3个
    ab.trimEnd(3)
    println
    for (a <- ab) print(a + ",")
    // 移除前1个
    ab.trimStart(1)
    println
    for (a <- ab) print(a + ",")
    // 0的位置插入2
    ab.insert(0, 2)
    println
    for (a <- ab) print(a + ",")
    // 1位置插入3,4,5
    ab.insert(1, 3, 4, 5)
    println
    for (a <- ab) print(a + ",")
    // 移除位置为0的
    ab.remove(0)
    println
    for (a <- ab) print(a + ",")
    // 移除0位置开始的2个
    ab.remove(0, 2)
    println
    for (a <- ab.toArray) print(a + ",")

    println
    val array = Array(1, 2, 4, 5, 6)
    // yield，返回一个新的数组
    val a1 = for (a <- array) yield a * 2
    for (a <- a1) print(a + ",")
    // 返回满足条件元素组成一个新的数组
    val a2 = for (a <- array if a % 2 == 0) yield a * 2
    println
    for (a <- a2) print(a + ",")
    // var a3 = array.filter(i => i % 2 == 0).map(_ * 2)
    var a3 = array.filter(_ % 2 == 0).map(_ * 2)
    for (a <- a3) print(a + ",")

    // 排序，返回排序后的值
    println
    for (a <- Array(8, 25, 6, 7, 2).sorted) print(a + ",")

    // quickSort返回为Units
    println
    val a4 = Array(8, 25, 6, 7, 2)
    Sorting.quickSort(a4)
    for (a <- a4) print(a + ",")

    // 求max
    println
    println(Array(1, 2, 4, 5).sum)
    // 字符串max
    println(Array("a", "bbb", "ccc").max)

    // 连接元素
    var a5 = Array(1, 2, 4, 5)
    println(a5.mkString)
    println(a5.mkString("and"))
    println(a5.mkString("<", ",", ">"))

    // 3行4列
    var matrix = Array.ofDim[Int](3, 4)
    matrix(0)(0) = 2
    println(matrix(0)(0))

    // 可变数组
    var a6 = new Array[Array[Int]](10)
    for (i <- 0 until a6.length)
      a6(i) = new Array[Int](i + 1)
  }
}
```