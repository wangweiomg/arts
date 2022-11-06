## Review - [Java类加载机制](https://www.baeldung.com/java-classloaders)

> ## **1. Introduction to Class Loaders**
>
> Class loaders are responsible for **loading Java classes dynamically to the JVM** **(Java Virtual Machine) during runtime.** They're also part of the JRE (Java Runtime Environment). Therefore, the JVM doesn't need to know about the underlying files or file systems in order to run Java programs thanks to class loaders.
>
> Furthermore, these Java classes aren't loaded into memory all at once, but rather when they're required by an application. This is where class loaders come into the picture. They're responsible for loading classes into memory.
>
> In this tutorial, we'll talk about different types of built-in class loaders and how they work. Then we'll introduce our own custom implementation.

