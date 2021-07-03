# servlet java

- Object... parameters: chứa các mảng dữ liễu để thực hiện câu truy vẩn, cụ thể là mỗi dấu ? ứng với một phần thử trong mảng.

```
 select * form news where news.A = ? => parameters[index]
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
  - Nhiệm vụ của `IClassDAO` phải thừa kế từ cha của nó là `GenericDao` && `tClassDAO` thừa kế từ cha nó là `AbstractDAO`
