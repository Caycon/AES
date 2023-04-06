# Key expansion
```Python

from Crypto.Cipher import AES

# Hàm khởi tạo khóa mở rộng
def key_expansion(key, key_size):
    """
    key: khóa bí mật (dưới dạng byte)
    key_size: độ dài của khóa (128, 192 hoặc 256 bits)
    return: bảng khóa con (dưới dạng list các byte)
    """
    # Số lượng khóa con được tạo ra phụ thuộc vào độ dài khóa
    if key_size == 128:
        num_rounds = 10
    elif key_size == 192:
        num_rounds = 12
    else:
        num_rounds = 14
    
    # Số byte trong một khóa con
    block_size = 4 * (key_size // 32)
    
    # Bảng khóa con được khởi tạo với khóa bí mật ban đầu
    round_keys = list(key)
    
    # Bảng hằng số được sử dụng trong phép toán thay thế
    s_box = AES.new(bytes(key))._cipher.IV[:256]
    
    # Một khóa con mới được tạo ra cho mỗi vòng lặp
    for i in range(key_size, block_size * (num_rounds + 1)):
        # Khóa con trước đó
        temp = round_keys[i - 1]
        
        # Trường hợp i chia hết cho key_size (128, 192 hoặc 256 bits)
        if i % key_size == 0:
            # Dịch chuyển byte của khóa con trước đó sang trái 1 byte
            temp = temp[1:] + temp[:1]
            # Thay thế các byte bằng các giá trị trong bảng hằng số
            temp = [s_box[b] for b in temp]
            # XOR khóa con trước đó với một giá trị từ bảng hằng số
            temp[0] ^= (2 ** (i // key_size - 1)) % 256
        # Trường hợp i không chia hết cho key_size và độ dài khóa lớn hơn 128 bits
        elif key_size > 128 and i % key_size == 4:
            # Thay thế các byte bằng các giá trị trong bảng hằng số
            temp = [s_box[b] for b in temp]
        
        # XOR khóa con mới với khóa con trước đó
        round_keys.append([a ^ b for a, b in zip(round_keys[i - key_size], temp)])
    
    return round_keys
```  
## Giải thích:
-   `list(key)`: chuyển đổi khóa bí mật từ dạng byte sang dạng list các byte.
-   `AES.new(bytes(key))`: tạo một đối tượng AES với khóa bí mật đã cho.
-   `._cipher.IV`: truy cập đến giá trị vector khởi tạo (IV) của đối tượng AES. Giá trị này là bảng hằng số được sử dụng trong phép toán thay thế (SubBytes).
-   `[:256]`: cắt bảng hằng số chỉ lấy 256 giá trị đầu tiên.
-   `temp[1:] + temp[:1]`: phép dịch chuyển byte của khóa con trước đó sang trái.
-   `[s_box[b] for b in temp]`: thay thế các byte của khóa con bằng giá trị tương ứng trong bảng hằng số.
-   `(2 ** (i // key_size - 1)) % 256`: tính giá trị hằng số để XOR với byte đầu tiên của khóa con.
-   `temp[0] ^= (2 ** (i // key_size - 1)) % 256`: thực hiện phép XOR byte đầu tiên của khóa con với giá trị hằng số được tính ở trên.
Trong quá trình khởi tạo khóa mở rộng, các byte của khóa ban đầu được chia thành các khóa con bằng cách thực hiện các phép toán thay thế và phép XOR. Mỗi khóa con được sử dụng trong quá trình mã hóa dữ liệu ở mỗi vòng lặp của AES. Việc tạo ra các khóa con như vậy đảm bảo tính bảo mật của thuật toán AES.
