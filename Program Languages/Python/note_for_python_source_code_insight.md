# Python 源码剖析
##整数对象
1. Python会为小整数变量创建`小整数对象池`，小整数在Python的运行周期中是不会被销毁的。
小整数的范围由常量`NSMALLPOSINTS`和`NSMALLNEGINTS`来定义。小整数对象会在Python初始化的时候通过调用`_PyInt_Init`来创建。且之后出现的小整数都只会增加refcnt而不会真正单独维护一个对象。
```c
#ifndef NSMALLPOSINTS
#define NSMALLPOSINTS           257
#endif
#ifndef NSMALLNEGINTS
#define NSMALLNEGINTS           5
#endif
```

2. Python同样会为大整数预先提供一个固定的内存空间，当超出这个空间时，Python才会为大整数申请额外的内存。这个空间称为`PyIntBlock`，这个空间中储存的其实是`PyIntObject`的单向链表，由两个指针维护，即block_list和free_list。首次创建整数时，会调用`fill_free_list`来初始化新的block，同样，如果空间被用完也会导致free_list指针变为空，那么在这之后第一次创建整数时，便会创建新的block。
3. 大整数的释放，当大整数对象被销毁时，这块内存会重新加入到链表的空闲内存中。
```c
static void
int_dealloc(PyIntObject *v)
{
    if (PyInt_CheckExact(v)) {
        Py_TYPE(v) = (struct _typeobject *)free_list;
        free_list = v;
    }
    else
        Py_TYPE(v)->tp_free((PyObject *)v);
}
```

## String 对象