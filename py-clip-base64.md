# 监听剪切板，自动转换为base64

```python
# import profile
# import timeit
import base64
import time
from io import BytesIO

from PIL import Image, ImageGrab

import win32clipboard


def pil():
    pre = 0
    while 1:
        seq = win32clipboard.GetClipboardSequenceNumber()
        # print(seq)
        if pre == seq:
            time.sleep(0.02)
            continue
        else:
            pre = seq
        # print(seq)
        # 保存剪切板内图片
        img = ImageGrab.grabclipboard()
        if isinstance(img, Image.Image):
            buffer = BytesIO()
            img.save(buffer, 'jpeg')
            b64str = base64.b64encode(buffer.getvalue())
            print(len(b64str))
            # print(b64str, '\n', end='\r')
            b64str = f'![](data:image/*;base64,{b64str.decode()})'
            # 设置剪切板
            win32clipboard.OpenClipboard()
            win32clipboard.SetClipboardText(b64str)
            win32clipboard.CloseClipboard()
            # 更新seq
            pre = win32clipboard.GetClipboardSequenceNumber()
            buffer.close()


if __name__ == '__main__':
    pil()
    # timeit.main(pil())
    # res = timeit.timeit('pil()', setup='from __main__ import pil', number=50000000)

```

参考文档：

<https://docs.microsoft.com/zh-cn/windows/win32/api/winuser/nf-winuser-getclipboardsequencenumber>
