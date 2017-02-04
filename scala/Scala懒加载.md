```scala
/**
 * 懒加载
 */
object Test5 {

  def main(args: Array[String]): Unit = {
    lazy val file = Source.fromFile("d://jd-gui.cfg");

    println("a")
    // 延迟加载，加了lazy，在此行之前是正常的，使用的时候才进行实例化,报错还是报错
    for (i <- file.getLines()) println(i)
  }
}
```