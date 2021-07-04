# servlet java

- Object... parameters: chứa các mảng dữ liễu để thực hiện câu truy vẩn, cụ thể là mỗi dấu ? ứng với một phần thử trong mảng.

```
 select * form news where news.A = ? dấu "?" là => parameters[index]
```

- package mapper: dùng để lưu dữ liêu sau khi truy vấn, tham số truyền vào là ResultSet chứa các các bản dữ liệu sau truy vấn.

- DAO: 
  - interface:
   - GenericDAO: `public interface GenericDAO<T> `
   - IClassDAO: `public interface IClassDAO extends GenericDAO<tClassModel>`
  - class:
   - AbstractDAO: `public class AbstractDAO<T> implements GenericDAO<T>` để triển khai interface
   - tClassDAO: `public class tClassDAO extends AbstractDAO<tClassModel> implements IClassDAO`
  - Việc triển khai tầng DAO sẽ tránh triển khai nhiều lần
  - Nhiệm vụ của `IClassDAO` phải thừa kế từ cha của nó là `GenericDao` && `tClassDAO` thừa kế từ cha nó là `AbstractDAO` việc `implements` là để triển khai cho hàm của nó.
- Form Util: có nhiệm vụ là lọc các request, mỗi request sẽ ứng với mỗi filter trong model
```
public class FormUtil {
	public static <T> T toModel(Class<T> myClass, HttpServletRequest request) {
		T object = null;
		try {
			object = myClass.newInstance();
			BeanUtils.populate(object, request.getParameterMap()); // chuyen cac key va value de map
		} catch (InstantiationException | IllegalAccessException | InvocationTargetException e) {
			System.out.print(e.getMessage());
		}
		return object;
	}
}
```
