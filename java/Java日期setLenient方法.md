```java
public class TestDate {

    public static void main(String[] args) throws ParseException {
        DateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        format.setLenient(false); // Lenient，宽容，默认true为宽容(即2011-12-33自动转成2012-01-02)
        Date date = format.parse("2011-12-33");
        System.out.println(DateFormatUtils.format(date, "yyyy-MM-dd"));
    }
}
```