## ARTS - Review 补2019.1.9
## Java Class Format （二）

* determine 决定
* denote 代表
* lexicographically 按字典顺序
* distinguished 卓著
* indicates 表示


4.1. The ClassFile Structure
A class file consists of a single ClassFile structure:

```
ClassFile {
    u4             magic;
    u2             minor_version;
    u2             major_version;
    u2             constant_pool_count;
    cp_info        constant_pool[constant_pool_count-1];
    u2             access_flags;
    u2             this_class;
    u2             super_class;
    u2             interfaces_count;
    u2             interfaces[interfaces_count];
    u2             fields_count;
    field_info     fields[fields_count];
    u2             methods_count;
    method_info    methods[methods_count];
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}
```
The items in the ClassFile structure are as follows:

ClassFile 的结构项如下：

> magic
The magic item supplies the magic number identifying the class file format; it has the value 0xCAFEBABE.

魔数。 这个项目的魔数代表类文件的格式，值是 0xCAFEBABE。

> minor_version, major_version
> 
The values of the minor_version and major_version items are the minor and major version numbers of this class file. Together, a major and a minor version number determine the version of the class file format. If a class file has major version number M and minor version number m, we denote the version of its class file format as M.m. Thus, class file format versions may be ordered lexicographically, for example, 1.5 < 2.0 < 2.1.
>
>A Java Virtual Machine implementation can support a class file format of version v if and only if v lies in some contiguous range Mi.0 ≤ v ≤ Mj.m. The release level of the Java SE platform to which a Java Virtual Machine implementation conforms is responsible for determining the range.

>Oracle's Java Virtual Machine implementation in JDK release 1.0.2 supports class file format versions 45.0 through 45.3 inclusive. JDK releases 1.1.* support class file format versions in the range 45.0 through 45.65535 inclusive. For k ≥ 2, JDK release 1.k supports class file format versions in the range 45.0 through 44+k.0 inclusive.

minor_version 和 major_version 项目的值是这个类文件的 小版本号和主要版本号。主版本号和小版本号共同决定了类文件格式的版本。如果一个类文件有主版本号M 小版本号m, 我们表示这个类文件格式就是M.m. 所以，文件格式版本可能是按字典顺序，如 1.5<2.0<2.1

Java虚拟机实现可以支持版本V的类文件格式，当且仅当V位于某个连续范围 Mi.0 <= v <= Mj.m时，Java虚拟机实现所遵循的Java SE平台的发布级别负责确定范围。

Oracle的Java虚拟机实现 在JDK发布1.0.2 支持类文件格式版本从45.0 到45.3 。JDK发布1.1.* 支持类文件版本在45.0 到45.65335范围。对于 k>=2 ，jdk发布1.k 支持类文件版本在45.0 到44_k.0。


>constant_pool_count
>
The value of the constant_pool_count item is equal to the number of entries in the constant_pool table plus one. A constant_pool index is considered valid if it is greater than zero and less than constant_pool_count, with the exception for constants of type long and double noted in §4.4.5.

常量池个数

constant_pool_count 的值是 constant_pool表全部的项目 加一。 一个constant_pool 索引是被认为验证是否比0大比 constatn_pool_count小时候会发生类型为long的常亮类型异常。在4.4.5章节有加倍描述。

>constant_pool[]
>
The constant_pool is a table of structures (§4.4) representing various string constants, class and interface names, field names, and other constants that are referred to within the ClassFile structure and its substructures. The format of each constant_pool table entry is indicated by its first "tag" byte.
>
The constant_pool table is indexed from 1 to constant_pool_count - 1.

constant_pool 是一个代表各类字符串常量、类和接口名字、属性名字和其他类文件结构或自己子结构的表。每个 constant_pool 表实体的格式是 第一个 tag 字节标志。

constant_pool 表索引从 1 到 constant_pool_count  - 1



>access_flags

>The value of the access_flags item is a mask of flags used to denote access permissions to and properties of this class or interface. The interpretation of each flag, when set, is specified in Table 4.1-A.
>

操作标志

access_flags 项目的值是一个用来记录这个类或接口属性操作许可的标志，每个标志的解释，当设置时候， 在列表 4.1-A. 

Table 4.1-A. Class access and property modifiers


| 标志 | 值 | 解释 |
|---| ---| ---|
| ACC_PUBLIC |0x0001	 |声明public ,可以从包外操作 |
|ACC_FINAL| 0x0010 |声明final, 没有子类
| ACC_SUPER	| 0x0020 |invoke时候特殊对待父类方法
| ACC_INTERFACE | 0x0200 |是一个接口，不是类
| ACC_ABSTRACT | 0x0400 | 抽象类，不被具体实例
| ACC_SYNTHETIC | 0x1000 | 不出现在源码中
| ACC_ANNOTATION | 0x2000 | 注解类型
| ACC_ENUM | 0x4000 | 枚举类型


>An interface is distinguished by the ACC_INTERFACE flag being set. If the ACC_INTERFACE flag is not set, this class file defines a class, not an interface.

>If the ACC_INTERFACE flag is set, the ACC_ABSTRACT flag must also be set, and the ACC_FINAL, ACC_SUPER, and ACC_ENUM flags set must not be set.

>If the ACC_INTERFACE flag is not set, any of the other flags in Table 4.1-A may be set except ACC_ANNOTATION. However, such a class file must not have both its ACC_FINAL and ACC_ABSTRACT flags set (JLS §8.1.1.2).

>The ACC_SUPER flag indicates which of two alternative semantics is to be expressed by the invokespecial instruction (§invokespecial) if it appears in this class or interface. Compilers to the instruction set of the Java Virtual Machine should set the ACC_SUPER flag. In Java SE 8 and above, the Java Virtual Machine considers the ACC_SUPER flag to be set in every class file, regardless of the actual value of the flag in the class file and the version of the class file.

>The ACC_SUPER flag exists for backward compatibility with code compiled by older compilers for the Java programming language. In JDK releases prior to 1.0.2, the compiler generated access_flags in which the flag now representing ACC_SUPER had no assigned meaning, and Oracle's Java Virtual Machine implementation ignored the flag if it was set.

>The ACC_SYNTHETIC flag indicates that this class or interface was generated by a compiler and does not appear in source code.

>An annotation type must have its ACC_ANNOTATION flag set. If the ACC_ANNOTATION flag is set, the ACC_INTERFACE flag must also be set.

>The ACC_ENUM flag indicates that this class or its superclass is declared as an enumerated type.

>All bits of the access_flags item not assigned in Table 4.1-A are reserved for future use. They should be set to zero in generated class files and should be ignored by Java Virtual Machine implementations.


ACC_INTERFACE 标志标明这是一个接口。如果ACC_INTERFACE表示没有设置，类文件定义是一个类，不是一个接口。

如果ACC_INTERFACE设置了，ACC_ABSTRACT 标志一定也设置了，而且ACC_FINAL和ACC_SUPER, ACC_ENUM 标志一定没有设置。

如果ACC_INTERFACE没有设置，表中任何其他标志都可能设置，除了ACC_ANNOTATION. 然而，诸如一个类文件必须不同时设置 ACC_FINAL 和ACC_ABSTRACT 标志。

ACC_SUPER 标志 表示如果出现在这个类或接口中，那么就会在执行 invokespecial 命令时候二选一的语义。编译成jvm命令集合需要设置ACC_SUPER 标志。在JAVA SE8 和以上，JVM 考虑将ACC_SUPER标志设置在每个类文件中，无论类文件实际的值是什么、版本是什么。

ACC_SUPER 对老的Java编译器是向后兼容的。在jdk 1.02. , 编译器构造 access_flags 现在操作ACC_SUPER 不需要分配意义，Oracle的jvm是忽略这个设置的。


ACC_SYNTHETIC 标志代表类或接口被编译器生成，不出现在的源码中。

一个注解类型必须有自己的ACC_ANNOTATION 标志集。如果 ACC_ANNOTATION设置了，ACC_INTERFACE标志也必须设置。

ACC_ENUM 标志代表类或他的超类声明为一个枚举类型。

所有的acc_Ess_flags 项目位没有在Table4.1-A 出现的都为未来保留。可能被设置为0，被Java虚拟机实现忽略。













