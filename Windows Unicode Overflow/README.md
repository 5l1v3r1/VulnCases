# Windows Unicode Overflow

This is an example of a buffer overflow due to the use of MultiByteToWideChar. This function is
often used to convert a C string to unicode, and that implies each character expands to two
bytes. For example, this is 'A' in hex:

```
41
```

After the conversion, it becomes:

```
0041
```

To exploit this type of bug, we are restricted to only use gadgets that reside in this address
format:

```
00xx00xx
```

For example, if we want a JMP ESP for a unicode overflow, we can't use an address that looks like:

```
0x20417241
```

We could use one that looks like:

```
0x00410072
```

Due to this restriction, unicode without ASLR is already quite challenging to exploit, especially
for a small application. You just may not find enough gadgets to work with. If that is the case,
consider finding ways to load more DLLs, and hopefully there is one that can provide enough gadgets
you can work with.