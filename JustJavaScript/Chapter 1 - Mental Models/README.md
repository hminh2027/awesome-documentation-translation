# Chương 1 - Mô hình tư duy

> **Mô hình tư duy (mental model)**: ý nói về cách suy nghĩ, nhìn nhận trong một chủ lĩnh vực, sự vật nào đó. Ở đây là cách mà JavaScript hoạt động.

> **Biến (variable)**: là một đại lượng có giá trị bất kì có thể thay đổi, không bị bó buộc

> Assign và Set: về cơ bản thì assign là tạo bản sao của giá trị rồi gán cho biến, còn set là trỏ tới giá trị (giống 1 sợi dây)

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

Cuộc hội thoại của bạn có thể sẽ hơi khác một chút, chẳng hạn như bạn dùng từ "assign" thay vì "set", hoặc vị trí của câu từ hơi khác nhau xíu, thậm chí cũng có thể là bạn ra luôn một kết quả khác. Hãy chú ý đến tất cả sự khác nhau đó. Bạn nói "set `b` bằng `a`" vậy thì việc set một biến thực chất nghĩa là gì?

Nếu để ý bạn sẽ thấy rằng những thứ như này thuộc loại kiến thức nền tảng trong lập trình (chẳng hạn như biến) và các toán tử (chẳng hạn như set giá trị cho nó), và chúng đều liên kết với những loại kiến thức khác mà bạn đã từng học, những kiến thức đến từ thế giới thật chứ không phải máy tính (chẳng hạn như cộng trừ nhân chia các con số trong môn Toán). Kiến thức có thể sẽ bị chồng chéo lên nhau và thậm chí là mâu thuẫn với nhau, tuy nhiên chúng vẫn giúp ta hiểu được phần nào cách mà code hoạt động.

Lấy ví dụ, nhiều người khi mới học lập trình đều nghĩ rằng các biến giống như là những cái "hộp" - những cái thùng rỗng để chứa giá trị bên trong nó. Điều này diễn ra tự nhiên trong tiềm thức của bạn.

Những dự đoán hay giả định như vậy được gọi là "mô hình tư duy", đó là cách mà chúng ta nghĩ một thứ hoạt động như thế nào. Nếu bạn đã lập trình được một thời gian dài thì sẽ tương đối khó để có thể thay đổi được mô hình tư duy, nhưng hãy thử cố tập trung và phân tích suy nghĩ của bạn. Những giả định của mô hình tư duy (chẳng hạn như biến là "hộp") sẽ ảnh hưởng tới cách mà chúng ta lập trình theo năm tháng.

Không may là.. đôi lúc mô hình tư duy của chúng ta được xây dựng một cách không chính xác. Chẳng hạn như từ một tutorial nào đó cố đơn giản hóa chủ đề để cho dễ giải thích, chẳng hạn như khi mới học JavaScript chúng ta lại mang kiến thức từ ngôn ngữ khác sang để áp dụng, chẳng hạn như chúng ta tự suy ra logic từ một đoạn code nào đó mà chẳng thèm tìm hiểu xem liệu nó có đúng hay không.

Và mục tiêu của cuốn Just JavaScript này là giúp bạn tìm và sửa những lỗi sai đó. Chúng ta sẽ xây dựng lại dần dần (hoặc từ đầu) mô hình tư duy của bạn trong JavaScript để giúp cho nó chính xác và hữu dụng. Nó sẽ giúp bạn trở nên tự tin khi viết code và giúp bạn thực sự hiểu (và sửa) được đoạn code của người khác.

(À mà, `a` bằng `0` và `b` bằng `10` là đáp án chính xác đấy nhé!)

## Code nhanh và chậm

"Tư duy nhanh và chậm" là một cuốn sách được viết bởi Daniel Kahnerman với nội dung khai thác 2 "hệ thống" suy nghĩ khác nhau của con người.

Chúng ta thường phụ thuộc vào hệ thống "nhanh" bất cứ lúc nào có thể, điều này thật sự rất tốt trong những trường hợp chúng ta cần phản xạ nhanh hoặc bản năng. Đặc biệt là hệ thống này rất phổ biến giữa các loài động vật (cần thiết để sinh tồn!), cho phép chúng có những khả năng tuyệt vời chẳng hạn như đi bộ mà không bị ngã.. Nhưng nhược điểm của hệ thống này đó là rất tệ trong việc lên kế hoạch.

Nhưng nhờ vào sự phát triển của thùy trán, loài người đồng thời cũng sở hữu một hệ thống độc nhất gọi là hệ thống "chậm". Hệ thống này sẽ chịu trách nhiệm cho những thứ phức tạp hơn. Nó cho phép chúng ta lên kết hoạch cho những sự kiện trong tương lai, tham gia vào các cuộc tranh luận và tin vào các chứng minh toán học.

Và bởi vì hệ thống "chậm" này tiêu hao nhiều năng lượng nên chúng ta thường có xu hướng sử dụng hệ thống "nhanh" - ngay cả khi làm những việc cần nhiều chất xám như viết code.

Hãy tưởng tượng rằng bạn đang ngập giữa một núi công việc và bạn muốn xác định thật nhanh xem cái function này nó làm gì. Bạn liếc qua nó:

```jsx
function duplicateSpreadsheet(original) {
  if (original.hasPendingChanges) {
    throw new Error('You need to save the file before you can duplicate it.');
  }
  let copy = {
    created: Date.now(),
    author: original.author,
    cells: original.cells,
    metadata: original.metadata,
  };
  copy.metadata.title = 'Copy of ' + original.metadata.title;
  return copy;
}
```

Bạn nhận thấy những điều sau:
- Function này tạo bản sao của bảng tính.
- Nó quẳng ra lỗi nếu bảng tính gốc chưa được lưu lại.
- Nó đặt tiêu đề cho bản sao mới là "Copy of".

Điều mà bạn có lẽ sẽ bỏ qua (nếu không bỏ qua thì bạn làm tốt đấy!) đó là function này ***đồng thời*** cũng vô tình thay đổi tiêu đề của bảng tính gốc. Việc bỏ qua những lỗi như thế này là chuyện thường ngày xảy ra với các lập trình viên.

Giờ bạn đã biết là code có bug rồi, liệu bạn có đọc code theo kiểu khác không? Nếu ban đầu bạn sử dụng hệ thống "nhanh" thì giờ bạn có lẽ sẽ muốn chuyển sang hệ thống "chậm" rồi đấy.

Khi sử dụng hệ thống "nhanh", chúng ta sẽ đoán mục đích của đoạn code dựa trên cấu trúc, cách đặt tên và các comment trong code. Ngược lại khi sử dụng hệ thống "chậm", chúng ta đọc đi đọc lại đoạn code từng bước một - một quá trình mệt mỏi và ngốn thời gian.

Đó là lý do tại sao có một mô hình tư duy chuẩn chỉnh là rất quan trọng. Rất khó để ta có thể mô phỏng được khả năng tính toán như chiếc máy tính, bởi vậy khi phải sử dụng hệ thống "chậm" thì mô hình tư duy là tất cả những gì chúng ta có thể dựa vào. Nếu tư duy của bạn sai, bạn sẽ hiểu nhầm hết ngay cả những thứ cơ bản nhất trong code của bạn, và từ đó mọi nỗ lực mà bạn đã bỏ ra đều đi tong.

Đừng lo nếu bạn không thể tìm thấy bug - vì điều đó có nghĩa là cuốn sách này dành cho bạn! Qua từng chương chúng ta sẽ cùng nhau xây dựng lại mô hình tư duy JavaScript của bạn, giúp bạn có thể bắt được lỗi ngay tức thì.

Ở chương tiếp theo, chúng ta sẽ bắt đầu xây dựng mô hình tư duy cho những chủ đề cơ bản của JavaScript - giá trị và biểu thức.