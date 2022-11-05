## ARTS - Review 补2019.1.16
## Java class File Format（3）

* inherited v. 继承
* specification n. 规范


>this_class
>
>The value of the this_class item must be a valid index into the constant_pool table. The constant_pool entry at that index must be a CONSTANT_Class_info structure (§4.4.1) representing the class or interface defined by this class file.
>

this_class 这一项的值必须是constant_pool里验证过的值。constant_pool 实体索引必须是CONSTANT_Class_info 结构 代表定义在类文件的类或接口。

>super_class
For a class, the value of the super_class item either must be zero or must be a valid index into the constant_pool table. If the value of the super_class item is nonzero, the constant_pool entry at that index must be a CONSTANT_Class_info structure representing the direct superclass of the class defined by this class file. Neither the direct superclass nor any of its superclasses may have the ACC_FINAL flag set in the access_flags item of its ClassFile structure.

对于一个类，super_class 的值不能为0 也不能为一个contant_pool表里的值。如果super_class 是一个空值，constant_pool实体索引必须是一个CONSTATN_Class_info 结构代表类文件的直接父类。直接父类或它自己任何父类可能有ACC_FINAL 标志在 access_flags类文件结构。

>If the value of the super_class item is zero, then this class file must represent the class Object, the only class or interface without a direct superclass.

如果super_class 是0，这个类文件必须代表类对象，没有直接父类。

> For an interface, the value of the super_class item must always be a valid index into the constant_pool table. The constant_pool entry at that index must be a CONSTANT_Class_info structure representing the class Object.
> 

对于一个借口，super_class的值必须是constant_pool验证过的索引值。constant_pool实体必须是一个 CONSTANT_Class_info结构，代表类对象。

> interfaces_count

>The value of the interfaces_count item gives the number of direct superinterfaces of this class or interface type.

interfaces_count 的值是这个类或接口的直接父接口的数量。


> interfaces[]

>Each value in the interfaces array must be a valid index into the constant_pool table. The constant_pool entry at each value of interfaces[i], where 0 ≤ i < interfaces_count, must be a CONSTANT_Class_info structure representing an interface that is a direct superinterface of this class or interface type, in the left-to-right order given in the source for the type.

interfaces数组的每个值都必须是constant_table验证过的。constant_pool实体在interfaces[i]的每个值都是  0 <= i < interfaces_count, 必须是CONSTANT_Class_info结构代表这个在源类型中从左到右排序的类或接口的直接父接口。


> fields_count

> The value of the fields_count item gives the number of field_info structures in the fields table. The field_info structures represent all fields, both class variables and instance variables, declared by this class or interface type.

fields_count 项目的值是属性列表里的 ```filed_info``` 结构的数量。```field_info```结构代表这个类或接口声明的所有的属性，类属性和实例属性。



> fields[]
>
>Each value in the fields table must be a field_info structure (§4.5) giving a complete description of a field in this class or interface. The fields table includes only those fields that are declared by this class or interface. It does not include items representing fields that are inherited from superclasses or superinterfaces.

fields表每个值必须是一个field_info 结构， 提供了一个这个类或接口的属性完整描述。属性表仅仅包括这个类或接口声明的属性。不包括从父类或父接口继承的属性。

> methods_count
>
>The value of the methods_count item gives the number of method_info structures in the methods table.

方法表中 method_info 结构的数量。

> methods[]
> 
> Each value in the methods table must be a method_info structure (§4.6) giving a complete description of a method in this class or interface. If neither of the ACC_NATIVE and ACC_ABSTRACT flags are set in the access_flags item of a method_info structure, the Java Virtual Machine instructions implementing the method are also supplied.

方法表中的每个值必须是一个 method_info 结构。是这个类或接口的完整描述。如果ACC_NATIVE和ACC_ABSTRACT标志在access_flags method_info结构中没有设置， Java虚拟机实现的方法也支持。

> The method_info structures represent all methods declared by this class or interface type, including instance methods, class methods, instance initialization methods (§2.9), and any class or interface initialization method (§2.9). The methods table does not include items representing methods that are inherited from superclasses or superinterfaces.

method_info 结构代表这个类或接口类型声明的所有方法，包括实例方法，类方法，接口初始化方法。方法表不包括继承自父类或父接口的方法。


> attributes_count
The value of the attributes_count item gives the number of attributes in the attributes table of this class.

attributes_count 表示这个类属性表的属性数量。

>attributes[]
>Each value of the attributes table must be an attribute_info structure (§4.7).

>The attributes defined by this specification as appearing in the attributes table of a ClassFile structure are listed in Table 4.7-C.

>The rules concerning attributes defined to appear in the attributes table of a ClassFile structure are given in §4.7.

>The rules concerning non-predefined attributes in the attributes table of a ClassFile structure are given in §4.7.1.

每个属性必须是一个 attribute_info 结构。
一个类文件结构中出现的属性定义规范列表在 表 4.7-C 里。
...

