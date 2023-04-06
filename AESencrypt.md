```Python


from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import base64

# Khóa bí mật
#key = b'This is a 256-bit secret key for AES encryption'
import hashlib

key = "my secret key"
key = hashlib.sha256(key.encode()).digest()  # tạo khóa mới có độ dài 32 bytes

# IV
iv = b'This is an IV456'

# Chuỗi cần mã hóa
message = "Hello world!"

# Mã hóa
cipher = AES.new(key, AES.MODE_CBC, iv)
padded_message = pad(message.encode('utf-8'), AES.block_size)
encrypted_message = cipher.encrypt(padded_message)
encoded_message = base64.b64encode(encrypted_message).decode('utf-8')

# In ra chuỗi đã mã hóa
print("Encoded message:", encoded_message)

# Giải mã
decoded_message = base64.b64decode(encoded_message)
cipher = AES.new(key, AES.MODE_CBC, iv)
decrypted_message = unpad(cipher.decrypt(decoded_message), AES.block_size).decode('utf-8')

# In ra chuỗi đã giải mã
print("Decoded message:", decrypted_message)
```
