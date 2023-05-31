# __Tài liệu mô tả__

__Thành viên nhóm__

- Họ tên: Nguyễn Hoàng Quân -- MSSV: 21120403

- Họ tên: Lê Viết Đạt Trọng -- MSSV: 21120406

## __Tạo 1 Project DLL mới__

Đầu tiên chúng em sẽ tạo 1 Project mới, nhưng thay vì tạo bằng Empty Project thì chúng em đã hướng tới việc xây dựng tách các đối tượng hình học ra các thư viện liên kết động (.dll) để có thể dễ sửa chữa và thay đổi thế nên chúng em chọn Tạo 1 Console App để có thể sử dụng các thư viện liên kết động sẽ được xây dựng

![alt-text](https://i.imgur.com/Cf46Mbz.png)

Sau đó chúng em tạo các thư viện liên kết động để có thể dùng cho Console App
  
    - Đầu tiên chúng em sẽ tạo 1 thư viện động (Dynamic-Link Library DLL) với tên là Shapes để tạo lớp cha Shape sẽ được các lớp con (các đối tượng hình học) kế thừa.
  
![alt-text](https://learn.microsoft.com/en-us/cpp/build/media/create-new-dll-project-2019.png?view=msvc-170)
![alt-text](https://i.imgur.com/ntmdXXA.png)

    - Sau đó chúng em sẽ bắt đầu cài đặt Header lớp Shapes.
        + Hãy chú ý đến phần Preprocessor statements ở đầu file. Công dụng của Statements này là để khi Thư viện Shapes.dll được build thì sẽ báo với Compiler và Linker sẽ xuất 1 Function hoặc 1 biến Variable từ thư viện DLL sẽ được dùng cho các ứng dựng Applications khác.
    - Tham khảo để biết thêm: https://learn.microsoft.com/en-us/cpp/cpp/dllexport-dllimport?view=msvc-170

![alt-text](https://i.imgur.com/S7R2nr9.png)

    - Và với việc các hàm Public của lớp cha đều là hàm Virtual thế nên chúng em sẽ tiến hành cài đặt các đối tượng hình học kế thừa lớp Shape. Tương tự với việc tạo Thư viện động Shapes, chúng em sẽ tạo các thư viện động cho các đối tượng hình học.

![alt-text](https://i.imgur.com/TrbEF0m.png)

    - Và cũng tương tự chúng em sẽ cái đặt Header và CPP cho các đối tượng hình học. Ví dụ ở đây là đối tượng hình học Circle, và các đối tượng hình học khác cũng sẽ được cái đặt tương tự.

![alt-text](https://i.imgur.com/9aIcJyW.png)

![alt-text](https://i.imgur.com/9aIcJyW.png)

## __Để Add DLL Header vào Include path__

    - Khi cài đặt bạn có thể để ý 1 việc bất thường đó chính là: Tại sao lại không thể Include được thư viện Shapes.h

Câu trả lời đó chính là do Circl DLL vẫn chưa được add với Header Shapes DLL và để khắc phục vấn đề này chúng ta chỉ cần đơn giản là link chúng lại với nhau

    - Đầu tiên chúng ta nhấn chuột phải vào Project DLL cần link chọn Configuration Properties > C/C++ > General. Trong khung Drop-down control kế khung Additional Include Directories > chọn Edit. Và do Circle DLL và Shapes DLL nằm cùng 1 solution và vậy nên địa chỉ sẽ được cài đặt như sau: "..\".
    - Và sau đó chúng ta đã có thể dùng Header lớp cha Shapes DLL trên các lớp con DLL

![alt-text](https://i.imgur.com/BKCjFv0.png)

## __Cài đặt Client app__

    - Kế đến chúng ta sẽ cài đặt hàm main (Console Application mà chúng ta đã cài đặt lúc bắt đầu)
    - Và để sử dụng các DLL project khác thì chúng ta lại cần add với Header Shapes DLL và các DLL của các đối tượng hình học cần dùng. Và do main của chúng ta không nằm cùng với DLL Shapes nên chúng ta cần chỉ sửa lại Include Directories sao cho phù hợp

![alt-text](https://i.imgur.com/ZIU48pE.png)

    - Bây giờ Code của chúng ta có thể được compiled, nhưng vẫn chưa được linked. Nếu chúng ta build Client app, thì sẽ báo lỗi LNK2019. Đó là do Project đang thiếu 1 vài thông tin: Bạn vẫn chưa xác định rằng Project đang phụ thuộc vào Shapes.lib DLL và các đối tượng hình học như Cirle.lib. Và bạn vẫn chưa bảo Linker cách để tìm những file .lib đấy

## __Add thư viện DLL Import vào project__

    - Chúng ta vào Properties của Project main chọn Configuration Properties > Linker > Input > Additional Dependencies > Edit
    - Chúng ta chỉ cần Add những thư viện chúng ta cần: Shapes.lib; Circle.lib,....

![alt-text](https://i.imgur.com/Hnvh0wP.png)

    - Tiếp đến chúng chọn Configuration Properties > Linker > General > Additional Library Directories > Edit
    - Chúng ta chỉ dẫn cụ thể đến nơi chứa các thư viện .lib DLL. Bạn có thể dùng $(IntDir) để tự động Macro để Linker có thể tìm thấy DLL cần thiết.

![alt-text](https://i.imgur.com/JD2Bufc.png)

    - Sau đó mọi thứ đã có vẻ ổn thỏa và tiếp đến chúng em xây dựng các hàm hỗ trợ, các chức năng cho chương trình theo yêu cầu. Bao gồm các hàm Parse các đối tượng hình học (CircleParser, ....), các hàm giúp hỗ trợ việc hiển thị Display ra màn hình (Display,...), các hàm đọc file và tách file (Reader,...).

![alt-text](https://i.imgur.com/GPHxV5H.png)

## __Chạy thử chương trình__

    - Nếu như mọi thứ đều ổn thì khi chúng ta Build solution thì mọi thứ sẽ trông như thế này: 

![alt-text](https://i.imgur.com/02L9kB8.png)

    - Chúng ta có thể chạy thử chương trình bằng Ctrl + F5 hoặc chúng ta có thể vào folder Debug hoặc Release tùy theo cài đặt và chọn file main.exe để kiểm thử

![alt-text](https://i.imgur.com/fzlv0eC.png)

    - Chúng ta kiểm thử với file shapes.txt theo quy định (Nhưng đôi khi phải shapes.txt lại không tự động vào thư mục Debug hoặc Release khi ấy chúng ta chỉ cần Copy shapes.txt và bỏ vào nơi chứa main.exe là được): 

![alt-text](https://i.imgur.com/r4KBpeM.png)

![alt-text](https://i.imgur.com/IUl4Kdn.png)
