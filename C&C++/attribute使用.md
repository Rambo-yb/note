\_\_attribute\_\_ 可以设置函数属性、变量属性、类型属性、结构体和共用体属性
\_\_attribute\_\_ 语法格式为 : \_\_attribute\_\_ ((attribute-list))

参数 :
aligned : 指定对格式(以字节为单位), 如果aligned后没有指定数值，编译器依据目标机器情况使用最大最有益的对齐方式。
```c
struct S {
	short b[3];
}__attribute__ ((aligned(8)));
```

packed : 使用该属性对struct或union类型进行定义，设定其类型的每一个变量的内存约束。编译器取消在编译过程中的优化对齐，使用一字节对齐。按照实际占有字节数进行对齐，是GCC特有的语法。
```c
struct S {
	char a;
	int b;
}__attribute__ ((packed));
```

at : 绝对定位，可以把变量或函数绝对定位到Flash或RAM中
定位到flash中，一般用于固化的信息，入出厂设置的参数、上位机配置的参数、ID卡的ID号、flash标记等
```c
const u16 gFlashDefValue[512] __attribute__ ((at(0x0800F000))) = {0x1111, 0x1111, 0x1111, 0x0111, 0x0111, 0x0111}; //定位在flash中，其他flash补充为00

const u16 gFlashData__attribute__((at(0x0800F000))) = 0xFFFF;
```
定位到RAM中，一般用于数据量比较大的缓存，如串口的接收缓存，再就是某个位置的特定变量
```c
u8 USART2_RX_BUF[USART2_REC_LEN] __atribute__((at(0x20001000))); //接收缓存，最大USART_REC_LEN个字节，起始地址为0x20001000
```

注:
1.绝对定位不能在函数中定义，局部变量是定义在栈区的，栈区由MDK自动分配、释放，不能定义为绝对地址，只能放在函数外定义
2.定义的长度不能超过栈或Flash的大小，否则造成栈、Flash溢出

constructor : 函数会在main函数执行前自动执行
destructor : 函数会在main函数执行后或者exit()被调用后自动执行