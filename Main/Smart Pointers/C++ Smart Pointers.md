---
tags: [subsystem]
completed: true
status: 5
priority: 0
---
----
**[Created  :: `=dateformat(this.file.ctime, "DDDD, HH:mm")`]**
**[Edited    :: `=dateformat(this.file.mtime, "DDDD, HH:mm")`]**

----
# `= this.file.name` 
**[Author   :: skyeBreach]**

----

## Definitions

### Dangling Ptrs
A dangling ptr is a pointer that occurs at the time when the object is de-allocated from memory without modifying the value of the pointer.

### Wild Ptrs
Wild pointers are pointers that are declared and allocated memory but the pointer is never initialized to point to any valid object or address.

## Normal Ptrs vs Smart Ptrs
### Problems with Normal Pointers
- **Memory leaks**
- **dangling ptrs**
- **Wild ptrs**
- **Data Inconsistency**
- **Buffer overflow**

### Differences 

| Ptr                                                         | Smart Ptr                                             |
| ----------------------------------------------------------- | ----------------------------------------------------- |
| Variable that maintains a memory address and data type info | Class that wraps a pointer/scoped pointer             |
| Not Destroyed out of scope                                  | Destroys itself out of scope                          |
| Not as efficent                                             | more efficent as they include extra memory management |

## Smart Pointers

### Types
- Auto     : auto_ptr *Depreciated*
- Unique : unique_ptr
- Shared : shared_ptr
- Weak   : weak_ptr

### Unique (unique_ptr)
Take ownership of a pointer and as such take ownership of how the ptr is managed. This ownership is unique so cant assign another object until the first ptr is deleted


### Shared (shared_ptr)
Abillity to take ownership and share the ownership with other objects. Only deleted when all objects have been removed.

### Weak (weak_ptr)
Holds a non owning reference to an object that is managed by a shared ptr. Must be converted to shared ptr to access object.