# servlet java

- Object... parameters: chứa các mảng dữ liễu để thực hiện câu truy vẩn, cụ thể là mỗi dấu ? ứng với một phần thử trong mảng.

<code><div style="color:white; background-color:red;"> select * form news where news.A = ? => parameters[index]</div></code>

- package mapper: dùng để lưu dữ liêu sau khi truy vấn, tham số truyền vào là ResultSet chứa các các bản dữ liệu sau truy vấn.
