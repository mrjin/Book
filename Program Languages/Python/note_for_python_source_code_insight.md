# Python Դ������
##��������
1. Python��ΪС������������`С���������`��С������Python�������������ǲ��ᱻ���ٵġ�
С�����ķ�Χ�ɳ���`NSMALLPOSINTS`��`NSMALLNEGINTS`�����塣С�����������Python��ʼ����ʱ��ͨ������`_PyInt_Init`����������֮����ֵ�С������ֻ������refcnt��������������ά��һ������
```c
#ifndef NSMALLPOSINTS
#define NSMALLPOSINTS           257
#endif
#ifndef NSMALLNEGINTS
#define NSMALLNEGINTS           5
#endif
```

2. Pythonͬ����Ϊ������Ԥ���ṩһ���̶����ڴ�ռ䣬����������ռ�ʱ��Python�Ż�Ϊ���������������ڴ档����ռ��Ϊ`PyIntBlock`������ռ��д������ʵ��`PyIntObject`�ĵ�������������ָ��ά������block_list��free_list���״δ�������ʱ�������`fill_free_list`����ʼ���µ�block��ͬ��������ռ䱻����Ҳ�ᵼ��free_listָ���Ϊ�գ���ô����֮���һ�δ�������ʱ����ᴴ���µ�block��
3. ���������ͷţ�����������������ʱ������ڴ�����¼��뵽����Ŀ����ڴ��С�
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

## String ����