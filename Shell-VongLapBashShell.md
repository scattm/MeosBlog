Vòng lặp là một cấu trúc hoạt động gần như không thể thiếu trong bất kỳ ngôn ngữ lập trình nào, kể cả Shell. Nó cho phép chúng ta thực hiện một nhóm lệnh nhiều lần theo điều kiện đã đặt ra. Trong phạm vi bài vết này ta sẽ đi qua một vài cấu trúc lặp trong BASH Shell và ứng dụng của nó trong việc quản lý hệ thống.

### Vòng lặp FOR theo một danh sách các giá trị
Đây là cấu trúc lặp đơn giản nhất. Vòng lặp sẽ gán từng giá trị trong một danh sách cho biến đã được quy định. 

Ví dụ 1:

	#!/bin/bash

	for TIMES in 1 2 3 4 5 6
	do
		echo "Vong lap chay duoc $TIMES lan!"
	done

Chạy đoạn Script trên cho ra kết quả:

![](https://66.media.tumblr.com/e6b4c49f2d237898505d7019aadb1888/tumblr_inline_oapq0vuLHz1ursgxc_540.png)

Chúng ta cùng xem lại đoạn Script trên. Ở bài viết này chúng ta sẽ làm việc với Bash Shell, do đó chúng ta sẽ sử dụng Shebang sau để gọi Bash Shell:

	#!/bin/bash

***Lưu ý**: tránh sử dụng `sh` thay cho `bash` vì trên một số hệ điều hành hai Shell này khác nhau.*

Đoạn Script ở phía trên sẽ lần lượt cho ra 6 lần OUTPUT, mỗi lần biến TIMES sẽ được gán giá trị tiếp theo trong dãy `1 2 3 4 5 6`. Chúng ta sẽ thay đổi dãy giá trị trên trong ví dụ 2 bên dưới:

	#!/bin/bash

	for TIMES in 1 .. 6
	do
		echo "Vong lap chay duoc $TIMES lan!";
	done

Chạy đoạn Script vừa thay đổi chúng ta có kết quả:

![](https://67.media.tumblr.com/2e34059bd66f379659ad17164e75d65a/tumblr_inline_oapqcnmbsW1ursgxc_540.png)

Ta thấy rằng: giá trị trong danh sách có thể là số, hoặc chuỗi. Ở đây chuỗi danh sách có thể là kết quả OUTPUT của một lệnh khác được gọi theo ví dụ 3 sau:

	#!/bin/bash

	seq 0 5 20

	for COUNT in $(seq 0 5 20)
	do
		echo "Dem so toi $COUNT!";
	done

![](https://66.media.tumblr.com/78cc6d7c5c1b566179b87b84bc517c8b/tumblr_inline_oapqlt5j3m1ursgxc_540.png)

### Vòng lặp FOR theo quy luật số tăng hoặc giảm dần
Các phiên bản Bash Shell cũ nhỏ hơn 3.0 không hỗ trợ vòng lặp FOR này, do đó người ta sử dụng theo cách ở ví dụ 3. Từ Bash Shell phiên bản 3.0 chúng ta có thể sử dụng vòng lặp `for` cho dãy số tăng dần như ví dụ 4:

	#!/bin/bash

	for TIMES in {6..10}
	do
		echo "Vong lap chay duoc $TIMES lan!";
	done

![](https://66.media.tumblr.com/0fe029ff577ccaeed834b770c9ff1d88/tumblr_inline_oapqs6hfn61ursgxc_540.png)

Ở ví dụ số 5, chúng ta sẽ xem thêm quy luật đếm số ngược xuất hiện từ Bash Shell phiên bản 4.0 trở đi:

	#!/bin/bash

	for COUNTDOWN in {10..0..-1}
	do
		echo "Vong lap dem nguoc den $COUNTDOWN!";
	done

![](https://67.media.tumblr.com/7d9aaf7422e5a96aec3c858ff64be73e/tumblr_inline_oapqwfn5kA1ursgxc_540.png)

Tuy nhiên thông thường chúng ta vẫn có thể viết một vòng lặp giống như các ngôn ngữ lập trình khác. Do giống cú pháp với C nên cách này có thể sẽ dễ nhớ hơn với nhiều người và sử dụng được cho nhiều phiên bản Bash Shell.

Ví dụ 6:

	#!/bin/bash

	for ((COUNTDOWN = 10; COUNTDOWN >= 0; COUNTDOWN--))
	do
		echo "Vong lap dem nguoc den $COUNTDOWN!";
	done

![](https://66.media.tumblr.com/528e10f4022414e4e25cbfe661bbbf54/tumblr_inline_oapqzvJ4jl1ursgxc_540.png)

### Vòng lặp WHILE
Tuy vòng lặp `for` được sử dụng nhiều nhất, nhưng trong Bash Shell ta vẫn có thể sử dụng các vòng lặp khác như `while` và `until`. Chúng ta xem qua ví dụ số 7 về vòng lặp `while`:

	#!/bin/bash

	FLAG=2
	while [ $FLAG -lt 2000 ]
	do
		echo $FLAG;
		let FLAG=$FLAG*2
	done

![](https://66.media.tumblr.com/c2695eb8f1881f68bd1bc437f4c56b4c/tumblr_inline_oapr49Aurw1ursgxc_540.png)

Ta có thể sử dụng vòng lặp `while` để đọc từng PIPE:

	#!/bin/bash

	i=0
	while read LINE
	do
		echo "$i: $LINE"
		let i=$i+1
	done < example7.sh

![](https://66.media.tumblr.com/dde2c96a199f184dfabf8c00bf9cbda2/tumblr_inline_oapr9r2GFB1ursgxc_540.png)

Chúng ta cũng có thể viết vòng lặp WHILE với nhiều điều kiện cùng một lúc:

	#!/bin/bash

	i=20
	j=$i
	while [ "$i" -gt 7 -a "$j" -lt 41 ]
	do
		echo "i: $i - j: $j"
		let i=$i-3
		let j=$j+2
	done

![](https://67.media.tumblr.com/c8a3252a21e5972da89d7792bfb23034/tumblr_inline_oapreoToK81ursgxc_540.png)

Điều kiện của đoạn Script trên có thể viết theo các dạng khác như sau:

	[ "$i" -gt 7 ] && [ "$j" -lt 41 ]
	[[ $i -gt 7 ]] && [[ $j -lt 41 ]]
	[[ $i -gt 7 && $j -lt 41 ]]
	(( i > 7 && j < 41 ))

### Vòng lặp UNTIL
Đây là dạng vòng lặp theo tôi ít phổ biến trong Bash Shell, một vài ngôn ngữ không có vòng lặp này vì có thể thay thế được bằng các vòng lặp khác.

Ví dụ số 10 về vòng lặp `until`:

	#!/bin/bash

	FLAG=2
	until [ $FLAG -gt 2000 ]
	do
		echo $FLAG;
		let FLAG=$FLAG*2
	done

![](https://66.media.tumblr.com/20a20282ab3fa3c94960fb4b91cf26af/tumblr_inline_oaprjuATWS1ursgxc_540.png)