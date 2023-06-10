---
tags: [subsystem]
completed: false
status: 4
priority: 0
---
----
**[Created  :: `=dateformat(this.file.ctime, "DDDD, HH:mm")`]**
**[Edited    :: `=dateformat(this.file.mtime, "DDDD, HH:mm")`]**

----
# `= this.file.name` 
**[Author   :: skyeBreach]**

-----


## Pros
- Can be used for both runtime and unreal tools
	- IE can be used to extend editor, as the editor uses slate
- Can be used outside the engine

## Cons
- No Visual Designer
	- There are ways around this by using wireframe designers or using UMG to design and then implement into slate
- Animations are hard coded


## Both pro and con
- No blueprints, only C++
	- More performant
	- longer iteration and debugging times
	- Adds extra complexity
- No reflection and garbage collection