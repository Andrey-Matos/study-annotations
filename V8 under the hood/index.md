# V8 under the hood

V8 allocates more memory than an object initially needs; After a certain number of allocations (~10), V8 looks at the constructor’s transition tree, seeking the instance with maximum memory usage compared to the initial allocation. The new standard size is then set to be equal to this instance’s size. This process is called [**slack tracking**](https://v8.dev/blog/slack-tracking).

---

## Named properties storage

Variables are stored in three ways, ordered from fastest to slowest:

Objects have a limited number of in-object property slots, once those are filled, they are delegated into two groups:

Those with a constant of properties have them stored in an object, with their respective position stored in the hidden classes. 

Those with a greater number of properties have them self contained the properties object.

The <span style="color:Aquamarine">properties</span> field of an object is meant to store dynamically added named properties to an object <span style="color:DimGrey">(e.g. obj.newP = “foo”).</span>

---

## Indexed properties storage

Adding new indexed properties does not create a new hidden class, as oposed to named properties.

V8 keeps track of the variable types as precisely as possible for the sake of optimization, e.g. an array that only contains ints will have a “PACKED_SMI_ELEMENTS” type, while adding a floating-point variable to it would change its type to “PACKED_DOUBLE_ELEMENTS”, etc.

The aforementioned types are divided into “PACKED_\*”, i.e. contiguous, and “HOLEY_\*”. You can convert a packed array into a holey one, but the inverse is NOT true. 

When an array with a large number of empty indexes is created, V8 converts it into an dictionary representation to save space at the cost of a slightly slower access.

The *elements* field stores numbered properties (e.g. every item of an array object).
