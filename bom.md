# BOM (bytes order mark)

### BOM (bytes order mark 字节标记顺序)
    
    BOM (bytes order mark)
    字节顺序标记，出现在文本文件头部，Unicode编码标准中用于标识文件是采用哪种格式的编码
    
    1 千万不要在windows里用记事本编辑文件
    在windows中，如果用记事本打开了py文件，再在git上传的时候，会被git识别为二进制文件，这是由于bom导致的
    尤其是win10以下的版本中，记事本打开文件后，保存时，windows会自动的在开头加上 b'\xef\xbb\xbf' 十六进制字符
    于是你会遇到很多不可思议的问题，网页的第一行可能会显⽰示⼀一个“?”
    明明正确的程序⼀一编译就报语法错误，等等，都是由记事本的弱智行为带来的    
    
    2 验证有无bom问题
    1.创建2个内容相同py文件，file.py， file-bom.py
    2.在win10中，用记事本打开，其中file.py用不带bom的方式保存，file-bom.py用有bom的方式保存
    3.打开交互模式
    >>f=open(r'file.py','rb')
    >>g=open(r'file-bom.py','rb')
    >>f.read()[:20]
    # test python file
    >>g.read()[:3] 
    b'\xef\xbb\xbf'
    >>g.read()[3:23] 
    # test python file
    
    可以看到同样的文件内容，经过记事本打开之后，选择了bom的保存方式，windows会自动在文件的开头加上 b'\xef\xbb\xbf'
 
