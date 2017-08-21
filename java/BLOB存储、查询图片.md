### Java后台
```java
@Controller
public class Test {

	@org.junit.Test
	public void insert() throws Exception {
		InputStream is = new FileInputStream("D:\\photo\\1.jpg");
		String sql = "INSERT INTO BBB (ID, DATA) VALUES (#[ID], #[DATA])";
		SQLParams params = new SQLParams();
		params.addSQLParam("ID", 1, SQLParams.STRING);
		params.addSQLParam("DATA", is, SQLParams.BLOBFILE);

		SQLExecutor.insertBean(sql, params);
	}

	@RequestMapping("/getImage")
	public void select(HttpServletResponse response) {
		try {
			String sql = "SELECT DATA FROM BBB WHERE ID = ?";
			HashMap map = SQLExecutor.queryObject(HashMap.class, sql, 1);
			BLOB blob = (BLOB) map.get("DATA");
			InputStream in = blob.getBinaryStream();
			OutputStream out = response.getOutputStream();
			int length = -1;
			byte[] buffer = new byte[1024];
			while ((length = in.read(buffer)) > 0) {
				out.write(buffer, 0, length);
			}
			out.flush();
			out.close();
			in.close();
		} catch (Exception e) {
			e.printStackTrace();
		}

	}
}
```
### 使用
```html
<img alt="" src="${path}/getImage.do">
```