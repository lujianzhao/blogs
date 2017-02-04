```scala
/**
 * map，tuple，zip实战
 */
object Test1 {

  def main(args: Array[String]): Unit = {

    // 不可变map
    val map = Map("scala" -> 7, "hadoop" -> 8)
    // 返回一个新的map
    map.+("java" -> 7)
    map.++:(Map("aa" -> 1))
    println("1 : " + map)
    val m1 = for ((k, v) <- map) yield (k * 2, v * 2)
    println(m1)

    println(map.getOrElse("scala", 0))

    // 可变map
    var scores = scala.collection.mutable.Map("java" -> 7, "scala" -> 8, "spark" -> 9)
    scores += ("js" -> 100) // 增加
    scores -= "java" // 删除
    println(scores)

    // sortMap,排序map
    val sort = SortedMap(4 -> "java", 2 -> "js", 34 -> "scala")
    println(sort)

    // tuple，下标从1开始
    val tuple = (1, 2, 4.5, "serical", "java")
    println(tuple._1)
    var (a, b, c, d, e) = tuple
    var (f, _, _, _, _) = tuple
    println(a, b, c, d, e, f)

    // 返回一个tuple
    println("Java Scala".partition(_.isUpper))

    // zip操作
    val arr1 = Array(1, 2, 4);
    val arr2 = Array("a", "b", "c")
    for ((x, y) <- arr1.zip(arr2)) print(y * x)

  }
}
```