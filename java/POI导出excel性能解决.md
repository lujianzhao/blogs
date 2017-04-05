```java
public static void transformer(String[] titleList, String[] fieldList,
			String destFileName, int sheetNum, List<? extends Object> beanList)
			throws Exception {
		SXSSFWorkbook wb = new SXSSFWorkbook(100);
		int count = beanList.size() / sheetNum;
		int more = beanList.size() % sheetNum;

		for (int i = 0; i <= count; i++) {
			Sheet sheet = wb.createSheet("第" + (i + 1) + "页");
			// 表头
			Row title = sheet.createRow(0);
			for (int k = 0; k < titleList.length; k++) {
				Cell cell = title.createCell(k);
				cell.setCellValue(titleList[k]);
				sheet.setColumnWidth(k,
						titleList[k].getBytes().length * 2 * 256);
			}

			if (i == count) {
				sheetNum = more;
			}
			for (int j = 0; j < sheetNum; j++) {
				Row row = sheet.createRow(j + 1);
				for (int k = 0; k < fieldList.length; k++) {
					Cell cell = row.createCell(k);
					cell.setCellValue(BeanUtils.getProperty(
							beanList.get(i * sheetNum + j), fieldList[k]));
				}
			}
		}

		OutputStream os = new FileOutputStream(destFileName);
		wb.write(os);
		os.close();
	}
```
### 参考
[大数据导出POI之SXSSFWorkbook](http://blog.csdn.net/fullstack/article/details/38321467)<br>
[POI3.8解决导出大数据量excel文件时内存溢出的问题](http://xtadg.iteye.com/blog/1703572)<br>
[POI 实现合并单元格以及列自适应宽度](http://yjck.iteye.com/blog/1609232)