```java
package com;

import java.util.Calendar;
import org.apache.commons.lang.time.DateFormatUtils;

public class Test {

	public static void main(String[] args) {
		// 循环每月最后一天
		Calendar calendar = Calendar.getInstance();
		for(int i=0; i<12; i++) {
			calendar.set(Calendar.MONTH, i);
			System.out.println(calendar.getActualMaximum(Calendar.DAY_OF_MONTH));
			calendar.set(Calendar.DAY_OF_MONTH, calendar.getActualMaximum(Calendar.DAY_OF_MONTH));
			System.out.println(DateFormatUtils.format(calendar, "yyyy-MM-dd"));
			// 这句很关键
			calendar.set(Calendar.DAY_OF_MONTH, 1);
		}
	}
}
```

```bash
31
2017-01-31
28
2017-02-28
31
2017-03-31
30
2017-04-30
31
2017-05-31
30
2017-06-30
31
2017-07-31
31
2017-08-31
30
2017-09-30
31
2017-10-31
30
2017-11-30
31
2017-12-31
```