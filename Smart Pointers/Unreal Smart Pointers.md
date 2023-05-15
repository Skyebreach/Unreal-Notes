Based off the C++ smart pointers

# Benefits
| Benefit                | Description                                                                                                                                                                                        |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Prevents memory leaks  | Smart Pointers (other than Weak Pointers) automatically delete objects when there are no more shared references.                                                                                   |
| Weak Referencing       | Weak Pointers break reference cycles and prevent dangling pointers.                                                                                                                                |
| Runtime safety         | Shared References are never null and can always be dereferenced.                                                                                                                                   |
| Memory                 | Smart Pointers are only twice the size of a C++ pointer in 64-bit (plus a shared 16-byte reference controller). The exception to this is Unique Pointers, which are the same size as C++ pointers. |
| Optional Thread Safety | The Unreal Smart Pointer Library includes thread-safe code that manages reference counting across multiple threads. Thread safety can be traded out for better performance if it isn't needed.     |

# Types
## Shared Pointers (TSharedPtr)
- Owns the object it references to, and prevents deletion of object until no shared ptr or ref refrences it.
- Can be empty
- Non null shareds can prodice shared refs

## Shared References (TSharedRef)
- Acts like shared ptr
- However must always reference a non-null object
- used when want to guarentee that the referenced ibject is not null

## Weak Pointers (TWeakPtr)
- Similiar to shared ptrs but do not own the object, so do not affect the lifecycle
- This means they can become null, without warning
- Can produce a sharted ptr, ensuring safe access on a temporary basis

## Unique Pointers (TUniquePtr)
- Solely owns the object 
- Can transfer ownership but not share it
- result in compiler error when attempting to copy it
- Deletes itself when out of scope

> [!warning]
> Making a Shared Pointer or Shared Reference to an object that a Unique Pointer references is dangerous. This will not suspend the Unique Pointer's behavior of deleting the object upon its own destruction, even though other Smart Pointers still reference it. 
> 
> Similarly, you should not make a Unique Pointer to an object that is referenced by a Shared Pointer or Shared Reference