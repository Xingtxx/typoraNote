## py

```
源代码文件，由python.exe解释，可在控制台下运行，可用文本编辑器进行编辑；
```



## pyc

```
源代码文件经过编译后生成的二进制文件，无法用文本编辑器进行编辑； 执行一个.py文件后，并不会自动生成对应的.pyc文件，需要指定触发Python来创建pyc文件； - pyc是由py文件经过编译后生成的二进制字节码（byte code）文件； - pyc文件的加载速度比py文件快； - pyc文件是一种跨平台的字节码，由python的虚拟机来执行； - pyc文件的内容跟python版本相关，不同的python版本编译生成不同的pyc文件，只能在相同版本环境下执行；
```



## pyo

```
源代码文件经过优化编译后生成的文件，无法用文本编辑器进行编辑；
Python3.5之后，不再使用.pyo文件名，而是使用类似“xxx.opt-n.pyc的文件名；
```

## pyd

```
是python的动态链接库；
动态链接库（DLL）文件是一种可执行文件，允许程序共享执行特殊任务所必需的代码和其他资源；
pyd文件虽然是作为python的动态模块，但实质上还是DLL文件，只是后缀改为pyd；
一般是用C、C++、D语言按照一定的格式编写；
参考信息：https://docs.python.org/3/faq/windows.html?highlight=pyd#is-a-pyd-file-the-same-as-a-dll
```

## pyz

```
从Python 3.5开始，定义了.pyz和.pyzw分别作为“Python Zip应用”和“Windows下Python Zip应用”的扩展名。
新增了内置zipapp模块来进行简单的管理，可以用Zip打包Python程序到一个可执行.pyz文件。
- zipapp — Manage executable python zip archives
- https://docs.python.org/3/library/zipapp.html
详细内容请见PEP441(https://www.python.org/dev/peps/pep-0441/)
```

## 生成pyc文件的过程：

```
Python在执行import语句时（例如“import abc”），将会到已设定的path中寻找abc.pyc或abc.dll文件。
如果只是发现了abc.py，那么Python会首先将abc.py编译成相应的PyCodeObject中间结果，然后创建abc.pyc文件，并将中间结果写入该文件。
然后，Python会import这个abc.pyc文件，实际上也就是将abc.pyc文件中的PyCodeObject重新在内存中复制出来。
```

## 生成pyc文件的方法：

```
命令形式：
python -m py_compile file.py  # 生成单个pyc文件
python -m py_compile /dir/{file1,file2}.py  # 生成多个pyc文件
python -m compileall /dir/  #　生成目录下所有py文件对应的pyc文件
```

```
脚本形式：compile模块的compile函数：
import py_compile  # 相当于命令行中的“-m py_compile”
py_compile.compile('py file path')
```

