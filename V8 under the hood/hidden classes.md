# Hidden classes

Hidden classes are created dynamically as 80 bytes **map objects**. They store the shape of objects (mainly its size, pointers to other HC and a reference to the object’s prototype).

A new hidden class will **not** be recreated if two or more identical instances are created.

Each new **named** property either included dynamically is delegated a new hidden class. Each new class must have the transition information about possible following classes.

e.g:


| **C0**                           |**C1**                |**C2**               |
| :---                             |:--:                  | ---:                |
| size: 100                        |size: 105             |size: 110            |
| prototype: x                     |prototype: y          |prototype: z         |
| “a”: transition to C1 at offset A|“a”: field at offset A|“a”: ... at offset A |
|                                  |                      |“b”: ... offset B    |
|                                  |                      |“c”: ... C3 at offset|
