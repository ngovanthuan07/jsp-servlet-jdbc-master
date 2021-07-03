# servlet java
<style>
  .note__1{
    color: red;
  }
</style>

- Object... parameters: chứa các mảng dữ liễu để thực hiện câu truy vẩn, cụ thể là mỗi dấu ? ứng với một phần thử trong mảng.
<span class="note__1"> select * form news where news.A = ? => parameters[index]</span>
- package mapper: dùng để lưu dữ liêu sau khi truy vấn, tham số truyền vào là ResultSet chứa các các bản dữ liệu sau truy vấn.
