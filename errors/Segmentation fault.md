# Segmentation fault 段错误 (简写： segfault)
来源地址：[Segmentation fault wiki](https://en.wikipedia.org/wiki/Segmentation_fault)
### 引发的原因有三点


- 访问进程外的地址空间
- 访问进程没有权限访问的地址空间
- 对只读内存空间尝试写入操作

### 上述原因经常在程序非法访问内存的时候发生

#### 1. 空指针解引用
```c
int main(){
    int *ptr = NULL;
    printf("%d", *ptr);
}
```
> 由于ptr是一个空指针，在对空指针指向的值进行访问的时候会发生一个segfault错误

```c
int main(){
    int *ptr = NULL;
    *ptr=1;
}
```
> 对不存在的地址进行写入操作


```c
int main(){
    int *ptr = NULL;
    *ptr;
}
```
> 代码中虽然含有对空指针的解引用，但是不会报segfault错误，因为那一行代码会在编译会被dead code elimination（优化删除：移除对运行结果没有任何影响的代码）

```c
MacBook-Pro:贝壳$ gcc -o  segfalut  segfault.c
MacBook-Pro:贝壳$ ./segfalut
Segmentation fault: 11

```

#### 2. 对只读内存进行修改
```c
int main(void)
{
    char *s = "hello world";
    *s = 'H';
}
```
> ”hello world“ 作为一个字符串常量存储在静态存储区，*s的值是这个字符串常量的地址，常量是不可以修改的，于是*s=‘H' 引发了segfault错误
```c
Ubuntu:~$ gcc -o  segfalut  segfault.c
Ubuntu:~$ ./segfalut
Segmetation fault (core dumped)

```

> ps：在macOS下运行此程序会发生bus error 10 错误，具体原因没细究，在Linux下运行的时候是segfault

```c
MacBook-Pro:贝壳$ gcc -o  segfalut  segfault.c
MacBook-Pro:贝壳$ ./segfalut
Bus error: 10

```
#### 3.缓存溢出

#### 4.栈溢出

```c
 int main(void)
 {
    main();
    return 0;
 }
```