# MySQL - Kinh nghiệm về lập trình hàm/thủ tục trong MySQL

Cũng như các hệ quản lý cơ sở dữ liệu quan hệ (RDBMS) khác, MySQL cung cấp cho người dùng các phương thức lập trình với các câu truy vấn T-SQL (Transaction - Structure Query Language). Nếu bỏ qua những mặt hạn chế, **Stored Procedure** (thủ tục được lưu trữ - từ đây trở đi xin gọi là *SP*) và **User-Defined Function** (hàm định nghĩa bởi người dùng - từ đây trở đi xin được gọi là *function*) là những công cụ để bạn thiết lập nên một API riêng cho cơ sở dữ liệu (Database). Dưới đây là một vài kinh nghiệm của tôi khi làm việc với *SP* và *Function*.

## Khác biệt

- **Stored Procedure** là một tập hợp các câu lệnh T-SQL được lưu trữ sẵn trong cơ sở dữ liệu và phải được thực thi dưới dạng câu lệnh, ex: `CALL someStoredProcedure(param1, param2);`. *SP* có thể trả về một tập kết quả dạng con trỏ (bảng), thường là kết quả của câu `SELECT` cuối cùng được thực hiện, hoặc không trả về kết quả nào hết.
- **User-Defined Function** khác với *SP* ở chỗ nó có thể được sử dụng trong một câu truy vấn, ex: `SELECT id, lastLogin(id) as lastLogin FROM Users;`. Do đó mà bản thân function thường được dùng để trả ra kết quả nhiều hơn so với *SP* có thể dùng để thao tác chỉnh sửa dữ liệu. *Function* trong MySQL không trả về kết quả dạng bảng.

## Ưu điểm

- Bảo mật: Đối với một số trường hợp khi không thể cho Developer biết được schema hoặc quá trình xử lý dữ liệu, *SP* và *Function* là một giải pháp tốt.
- Toàn vẹn dữ liệu: Do các ứng dụng sẽ sử dụng *SP* và *Function*, dữ liệu sẽ được xử lý như nhau do cùng chung các câu T-SQL đã được viết sẵn.

## Nhược điểm

- Khó thay đổi công nghệ: Do *SP* và *Function* sử dụng cú pháp cũng như các câu T-SQL đặc thù của RDBMS, việc chuyển đổi sang một RDBMS sẽ bao gồm luôn việc phải viết lại toàn bộ *SP* và *Function*.
- Sử dụng ORM: Việc sử dụng ORM sẽ khó khăn hơn khi bạn phải viết một bộ code riêng hỗ trợ *SP* và *Function*.
- Debug level god-like: Bạn sẽ không thích việc debug xíu nào đâu.
- Quản lý code và version: Bạn sẽ phải quản lý thêm một bộ code. Ngay cả với việc sử dụng `git`, việc deploy và phân quyền mỗi khi thay đổi cũng rất chua cay.

## Biến (variables) và đối số (arguments)

Việc đặt tên biến cho *SP* và *Function* trong MySQL khá khó chịu, vì bạn khó phân biệt chỗ nào là biến, chỗ nào là tên các object trong database. Tôi thường đặt tên biến và đối số kèm theo tiền tố là nhiệm vụ/vị trí của biến:

- in_*: các đối số đầu vào của *SP* và *Function*.
- inter_*: các biến nội tại dùng để xử lý bên trong.
- out_*: các biến dùng cho việc trả kết quả, ví dụ out_status.

## Definer - người định nghĩa

Mỗi *SP* và *Function* lưu trong database kèm theo user định nghĩa ra nó. Nếu trong câu lệnh khởi tạo không chỉ ra, MySQL sẽ lưu user trong session đang kết nối. Nếu bạn không muốn gặp rắc rối sau này, khi tạo *SP* và *Function* nhớ chỉ ra một user chung trong hệ thống làm definer, ex: `root@localhost`.

## Trả kết quả

- Đối với *SP*, nên trả về một bảng có số phần tử giống nhau cho mỗi một nhánh logic, điều này sẽ giúp developer mapping với code dễ dàng hơn, nhất là các framework sử dụng ORM. Nếu được, hãy thêm một trường `status`, điều này sẽ giúp cho việc debug về sau.
- Đối với *Function*, do chỉ trả về kết quả với kiểu dữ liệu đã quy định trước, bạn nên sử dụng `signal` để trả lỗi.

## Liệt kê các SP và Function đang sử dụng:

	SHOW PROCEDURE|FUNCTION STATUS
	[LIKE 'pattern' | WHERE 'conditions'];

Ex:

	SHOW PROCEDURE
	WHERE db=database();

Hoặc ta có thể sử dụng câu query:

	SELECT * FROM mysql.proc;