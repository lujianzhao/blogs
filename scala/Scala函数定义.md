```scala
/**
 * 函数定义
 */
object Test4 {

  def main(args: Array[String]): Unit = {

    //for (i <- 1 to 2; j <- 1 to 3) print(i * j + " ")

    for (i <- 1 to 2; j <- 1 to 4 if (i == j)) print(i, j)

    println
    // 普通的不用指定返回类型
    def addNum(x: Int) = x + 100
    val add = (x: Int) => x + 20
    println(addNum(1))
    println(add(1))

    // 递归需要指定返回类型
    def fac(x: Int): Int = if (x == 1) 1 else x * fac(x - 1)
    println(fac(5))

    // 函数默认值
    def concat(content: String, prefix: String = ",你好棒") = content + prefix
    println(concat("Se vanter bleu"))
    println(concat("Se vanter bleu", ",你好帅"))

    // 可变参数
    def adds(x: Int*) = {
      var result = 0
      for (i <- x)
        result += i
      result
    }
    println(adds(1, 2, 3, 4, 5))
  }
}
```