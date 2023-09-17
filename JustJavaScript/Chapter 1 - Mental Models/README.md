# Chương 1 - Mô hình tư duy

> **Mô hình tư duy (mental model)**: ý nói về cách suy nghĩ, nhìn nhận trong một chủ lĩnh vực, sự vật nào đó. Ở đây là cách mà JavaScript hoạt động.

> **Biến (variable)**: là một đại lượng có giá trị bất kì có thể thay đổi, không bị bó buộc

> Assign và Set: 

Cho đoạn code như sau:

```jsx
let a = 10;
let b = a;
a = 0;
```

Hỏi giá trị của `a` và `b` sau khi chạy đoạn code sẽ là bao nhiêu? Hãy trả lời câu hỏi trên trước khi đọc tiếp.

Nếu bạn là một người đã từng code JavaScript thì chắc hẳn bạn đang băn khoăn rằng: "Đoạn code này trông dễ hơn code mình viết hàng ngày nhiều. Vậy mục đích của việc này là gì?".

Chắc chắn không phải là để giới thiệu cho bạn về biến rồi. Chúng tôi cho rằng bạn đã quen thuộc với nó rồi, vậy nên thay vào đó chúng ta sẽ tập trung để xây dựng mô hình tư duy của bạn.

## Vậy Mô hình tư duy là cái gì?

Đọc lại đoạn code trên lần nữa và phân tích xem tại sao kết quả như thế (Điều này rất quan trọng).

Trong quá trình đọc lại, hãy chú ý tới những gì đang diễn ra trong đầu bạn. Có thể sẽ có một cuộc hội thoại như sau:

- `let a = 10;`
  - Khai báo biến `a`. Set nó bằng `10`.
- `let b = a;`
  - Khai báo biến `b`. Set nó bằng `a`.
  - Khoan đã, `a` là gì nhỉ? À nó bằng `10`. Vậy thì `b` cũng bằng `10`.
- `a = 0;`
  - Set `a` bằng `0`.
- Giờ thì ta sẽ có kết quả: `a` bằng `0` và `b` bằng `10`.

Cuộc hội thoại của bạn có thể sẽ hơi khác một chút, chẳng hạn như bạn dùng từ "assign" thay vì "set", hoặc là vị trí của câu từ hơi khác nhau xíu. Hoặc cũng có thể là bạn có kết quả khác.