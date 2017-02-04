```scala
/**
 * 元组，数组，map
 */
object Test3 {

  def main(args: Array[String]) {
    // 元组
    var tuple = ("hello", "serical", 100)
    println(tuple._1)
    println(tuple._2)
    println(tuple._3)

    // 数组
    var arr = Array(1, 2, 4, 5, 6)
    for (i <- 0 until arr.length)
      print(arr(i) + ",")

    for (a <- arr)
      print(a + ",")

    arr.foreach(a => print(a + ","))

    // map
    println()
    var map = Map("key1" -> 100, "key2" -> "serical")
    for ((k, v) <- map)
      println(k, v)

    for ((_, v) <- map) // placeholder
      println(v)

    // file
    //val file = Source.fromFile("d:\\jd-gui.cfg")
    val file = Source.fromURL("http://360.com")
    for (f <- file.getLines()) println(f)
  }
}
```