# How AES works

Mật mã AES (Advanced Encryption Standard) là một trong những thuật toán mã hóa phổ biến nhất được sử dụng trong việc bảo vệ dữ liệu. Nó là một thuật toán mã hóa đối xứng, có nghĩa là cùng một khóa được sử dụng để mã hóa và giải mã dữ liệu.

Các bước để mã hóa và giải mã dữ liệu bằng AES là như sau:

- Chuẩn bị khóa: Khóa được chuẩn bị bằng cách sử dụng một thuật toán chuyển đổi khóa (key schedule) để biến đổi khóa gốc thành các khóa con cho mỗi vòng mã hóa. Khóa gốc phải có độ dài 128, 192 hoặc 256 bit.

- Mã hóa dữ liệu: Dữ liệu được chia thành các khối có độ dài cố định của 128 bit. Mỗi khối sau đó được mã hóa bằng cách sử dụng một loạt các vòng mã hóa. Mỗi vòng mã hóa bao gồm các bước thực hiện phép thay thế và phép hoán vị trên các byte của khối dữ liệu, kết hợp với các phép toán logic để tạo ra một khối mới.  
  + Quá trình mã hóa dữ liệu được thực hiện thông qua các bước sau:

    - Khởi tạo khóa con: Khóa được chuyển đổi thành các khóa con bằng cách sử dụng thuật toán key schedule. Mỗi vòng mã hóa sử dụng một khóa con khác nhau.

    - Thực hiện phép XOR: Khối dữ liệu được thực hiện phép XOR với khóa đầu vào để tạo khối dữ liệu được mã hóa.

    - Vòng mã hóa: Một số vòng mã hóa được thực hiện trên khối dữ liệu được mã hóa bằng cách sử dụng khóa con. Trong mỗi vòng mã hóa, các bước thay thế, hoán vị và phép toán logic được thực hiện trên các byte của khối dữ liệu để tạo ra một khối dữ liệu mới.

    - Thực hiện vòng cuối: Trong vòng cuối cùng, các bước thay thế và hoán vị được thực hiện, nhưng không có phép toán logic. Điều này giúp tạo ra một khối dữ liệu được mã hóa cuối cùng.

  + Trả về khối dữ liệu đã mã hóa: Khối dữ liệu được mã hóa cuối cùng trả về để được lưu trữ hoặc truyền đi.

- Giải mã dữ liệu: Giải mã được thực hiện bằng cách sử dụng cùng một thuật toán mã hóa, nhưng với các khóa con được sử dụng theo thứ tự ngược lại. Các vòng giải mã bao gồm các bước ngược lại so với các vòng mã hóa.  
## Một cách tổng quát ta có:  
-   Addition of the first round key  
    n-1 Rounds (tùy thuộc độ dài của khóa là bao nhiêu):
    -   Substitute Bytes
    -   Shift Rows
    -   Mix Columns
    -   Adding the Round Key
-   The final round
    -   Substitute Bytes
    -   Shift Rows
    -   Adding the Round Key

Vì AES là một thuật toán đối xứng, nên việc mã hóa và giải mã dữ liệu được thực hiện bằng cách sử dụng cùng một khóa. Điều này giúp đảm bảo tính bảo mật và hiệu quả trong việc bảo vệ dữ liệu.
### Tham khảo chi tiết tại các nguồn sau [I](https://www.youtube.com/watch?v=DUSmxMyHF-g&t=1176s) và [II](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)

