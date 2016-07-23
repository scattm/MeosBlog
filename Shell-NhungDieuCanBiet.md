### Shell là gì?

Shell là những chương trình có chức năng nhận lệnh từ bàn phím hoặc tập tin rồi chuyển cho hệ điều hành thực hiện. Nói một cách hình ảnh hơn, hệ điều hành bao gồm nhân (Kernel) thực hiện việc xử lý và lớp vỏ bên ngoài (Shell) thực hiện việc giao tiếp giữa nhân và người dùng. Thuật ngữ Shell không giới hạn trong các giao diện dòng lệnh (CLIs) mà còn bao gồm các giao diện đồ họa (GUIs). Trong phạm vi bài viết này xin chỉ đề cập đến các CLIs Shell trên các hệ điều hành Linux.

Một số Shell phổ biến:

Shell | Phát triển | Ghi chú
--- | --- | ---
BASH | Brian Fox và Chet Ramey | Shell phổ biến nhất trên các máy sử dụng Linux. Được phát triển dựa trên Bourne Shell.
CSH | Bill Joy | Được phát triển dành cho BSD. Giống với tên gọi của nó, CSH có cú pháp và cách sử dụng như ngôn ngữ C.
KSH | David Korn | Được sử dụng phổ biến trên Unix, Kron Shell có thể sử dụng mảng và chạy những vòng lặp tốt hơn.
COMMAND.COM | Microsoft | Phát triển dùng cho MS-DOS, có tính năng tương tự nhưng thua quá xa so với các Shell của họ Unix.

Để liệt kê các Shell hệ điều hành đang hỗ trợ, bạn có thể xem tập tin `/etc/shells.`

![](https://66.media.tumblr.com/e33b6c703aa9b196ab9d0b7c0d0a3a9d/tumblr_inline_oamh8dnDqk1ursgxc_540.png)

Để biết các mình đang sử dụng, bạn có thể xuất nội dung của biến SHELL.
![](https://65.media.tumblr.com/113ddde49a6a4a4f80405a2eacf71e36/tumblr_inline_oamhbvTVKs1ursgxc_540.png)

### sh và bash

Thông thường người ta sẽ nghĩ sh và bash là một bởi trên một số hệ điều hành Linux `/bin/sh` là soft link của `/bin/bash`, thực ra không phải như vậy. `sh` và `bash` là hai loại shell khác nhau, `bash` có nhiều tính năng cũng như cấu trúc lệnh tốt hơn `sh` (bản gốc).

Trong một số hệ điều hành `sh` là soft link của `dash` (Debian/Ubuntu), `ash`, hay `pdksh`…

### Shell script là gì?

Shell script là một tập hợp các lệnh theo thứ tự được chứa trong một tập tin text. Người sử dụng có thể lấy tập tin này thành input của shell để thực thi thay vì nhập và chạy từng lệnh.

Sau đây là một ví dụ đơn giản của Shell script:

	#!/bin/bash

	echo "Gio hien tai cua he thong: `date`"
	echo "Tai khoan cua ban: `whoami`"
	echo "Ban dang dung o thu muc: `pwd`"

Sẽ cho ra kết quả:

![](https://65.media.tumblr.com/08ecf6e60238f57e3c6b5d9e3c4498c1/tumblr_inline_oami8bdxa11ursgxc_540.png)

Thực chất đây là loại script sẽ được chạy thành một chương trình. Ở đầu mỗi tập tin này là dòng *Shebang* (bắt đầu là `#!`) để chỉ ra chương trình/Shell nào sẽ được sử dụng để chạy script.

Ví dụ:

	#!/bin/sh
	#!/bin/bash
	#!/usr/bin/perl -T

Một đoạn Bash shell script được thực hiện như một tiến trình (process) mới, có nghĩa là một shell mới sẽ được mở để thực hiện những lệnh này.

Shebang yêu cầu bạn phải định nghĩa đường dẫn tuyệt đối tới chương trình dùng để chạy script, tuy nhiên ta có thể định nghĩa như sau để có thể dùng script trên các hệ thống khác nhau:

	#!/usr/bin/env ksh

Shebang ở trên có nghĩa là hệ thống sẽ tìm tập tin thực thi Kron shell và dùng nó để chạy script.

### Biến và phạm vi biến
Tùy vào shell mà tính chất và phạm vi của biến có thể khác nhau. Thông thường các các biến được đặt tên theo quy ước chữ hoa cho các shell thuộc họ Bourne và chữ thường cho các shell thuộc họ C shell.

Đối với họ C shell, có hai loại biến chính là biến shell (shell variables) và biến môi trường (environment variables). Biến môi trường được thiết lập bằng lệnh `setenv` và biến shell được thiết lập bằng lệnh `set`.

Họ Bourne shell không phân biệt biến shell và biến môi trường, khi một shell được khởi tạo, nó đọc giá trị của biến môi trường và thiết lập chúng như một biến shell. Biến được thiết lập theo cú pháp sau:

	$ VAR_NAME='value'; export VAR_NAME;

Lệnh `VAR_NAME='value'` thiết lập biến `VAR_NAME` và gán giá trị *value* cho nó. Lệnh `export VAR_NAME` cho phép các chương trình khác khởi động từ shell sử dụng biến *VAR_NAME* như ví dụ dưới đây:

![](https://67.media.tumblr.com/02b4b02209157860077679c6b1bd4e94/tumblr_inline_oami5x8q1F1ursgxc_540.png)

Trở lại **shell script**. Khi được thực thi, tập lệnh shell script sẽ được truyền vào một shell mới - tức là một process mới - một khi thực hiện xong, các biến được thiết lập trong shell script không thể sử dụng cho shell bên ngoài.

![](https://67.media.tumblr.com/d0371bca8ec2cf32147b8df078b83b35/tumblr_inline_oami67sHHT1ursgxc_540.png)

Ngoài ra, bash shell cũng cho phép chúng ta có thể thiết lập các biến chỉ đọc bằng lệnh `readonly`:

![](https://66.media.tumblr.com/ea093a273754d91c0b13e2660dce04dc/tumblr_inline_oami6iPNrV1ursgxc_540.png)