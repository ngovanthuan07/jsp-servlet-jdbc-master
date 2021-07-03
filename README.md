# servlet java

- Object... parameters: chứa các mảng dữ liễu để thực hiện câu truy vẩn, cụ thể là mỗi dấu ? ứng với một phần thử trong mảng.

`
 select * form news where news.A = ? => parameters[index]
`

-package mapper: dùng để lưu dữ liêu sau khi truy vấn, tham số truyền vào là ResultSet chứa các các bản dữ liệu sau truy vấn.

```
for(let i = 0; i <  list__btn.length; i++)
{
  list__btn[i].onclick = function(){
    let j = 0;
    while(j < list__btn.length){
      list__btn[j++].className = 'btnIndex btn btn-light';
    }
    list__btn[i].className = 'btnIndex btn btn-light active';
  }
}
```
