# servlet java 🐱‍👓

- Object... parameters: chứa các mảng dữ liễu để thực hiện câu truy vẩn, cụ thể là mỗi dấu ? ứng với một phần thử trong mảng.

```
 select * form news where news.A = ? dấu "?" là => parameters[index]
```

- package mapper: dùng để lưu dữ liêu sau khi truy vấn, tham số truyền vào là ResultSet chứa các các bản dữ liệu sau truy vấn.

```
 select * form news -> resultset -> NewModel
```

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
- SessionUtil: dùng để lưu đăng nhập...
```
public class SessionUtil {
    private static SessionUtil sessionUtil = null;

    public static SessionUtil getInstance() {
        if (sessionUtil == null) {
            sessionUtil = new SessionUtil();
        }
        return sessionUtil;
    }

    public void putValue(HttpServletRequest request, String key, Object value) {
        request.getSession().setAttribute(key, value);
    }

    public Object getValue(HttpServletRequest request, String key) {
        return request.getSession().getAttribute(key);
    }

    public void removeValue(HttpServletRequest request, String key) {
        request.getSession().removeAttribute(key);
    }
}
```
- AuthorizationFilter: nhiệm vụ sẽ chuyển hướng đăng nhập
```
public class AuthorizationFilter implements Filter {

    private ServletContext context;

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.context = filterConfig.getServletContext();
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        // lay url hien tai
        String url = request.getRequestURI();
        //url.startsWith(request.getContextPath()+"/admin")
        if (url.startsWith("/admin")) {
            UserModel model = (UserModel) SessionUtil.getInstance().getValue(request, "USERMODEL");
            if (model != null) {
                if (model.getRole().getCode().equals(SystemConstant.ADMIN)) {
                    filterChain.doFilter(servletRequest, servletResponse);
                } else if (model.getRole().getCode().equals(SystemConstant.USER)) {
                    response.sendRedirect(request.getContextPath()+"/dang-nhap?action=login&message=not_permission&alert=danger");
                }
            } else {
                response.sendRedirect(request.getContextPath()+"/dang-nhap?action=login&message=not_login&alert=danger");
            }
        } else {
            filterChain.doFilter(servletRequest, servletResponse);
        }
    }

    @Override
    public void destroy() {

    }
}
```

- Phân trang:
	- totalItem: số lượng item trong database
	- maxPageItem: số lượng page tối thiểu trên 1 trang (ví dụ: trang 1 chỉ có tối đa 10 item)
	- page: page đang đứng (đang đứng ở page 1)
	- totalPages: số lượng page (tổng số page cần hiển thị)
	- <code>cau lenh sql => limit offset, maxPageItem</code>

```
	- totalPages = (int) Math.ceil((double) model.getTotalItem() / model.getMaxPageItem()) 
	- offset = (page - 1) * maxPageItem 
	(page - 1): sql bắt đầu từ 0 mà page lại = 1
	
	Ví dụ: cho maxPageItem = 10, totalItem = 100
		page = 1.
	
	totalPages = totalItem / maxPageItem = 100 / 10 = 10
	offset = (page - 1) * maxPageItem = (1-1) * 10 = 0
	sql = limit 0,10
	cho page = 2 ➡️ offset = 10 
	sql = limit 10,10
	cho page = 3 ➡️ offset = 20
	sql = limit 20,10
```
