---

banner: "![[unreal52Banner.png]]"
banner_y: 0.3798

banner_lock: true

tags: [project, main, landing, general]
projectName: Unreal Information Repository

priority: 0
status: 2
completed: false
---

# Skye's Unreal Information Repository
----

**[Created  :: `=dateformat(this.file.ctime, "DDDD, HH:mm")`]**
**[Edited    :: `=dateformat(this.file.mtime, "DDDD, HH:mm")`]**

## Authors
- **[Original Author  :: skyeBreach]**

----
## Vault Overview


## Sections


> [!multi-column]
> 
> ```dataviewjs
> dv.header(3, "UI")
> dv.table(["File"],
>	dv.pages("#system and #ui").file
>     .filter(file => file.folder.includes(dv.current().file.folder)  && file.frontmatter.completed && !file.name.contains("Template")) 
>		.map(file => {
>			return [
>				dv.fileLink(file.path), 
>			]
>		} )
>	)
> ```
> 
> ```dataviewjs
> dv.header(3, "Stub")
> dv.table(["File"],
>	dv.pages("#system and #stub").file
>     .filter(file => file.folder.includes(dv.current().file.folder)  && file.frontmatter.completed && !file.name.contains("Template")) 
>		.map(file => {
>			return [
>				dv.fileLink(file.path), 
>			]
>		} )
>	)
> ```
> 
> ```dataviewjs
> dv.header(3, "Misc")
> dv.table(["File"],
>	dv.pages("#system and #misc").file
>     .filter(file => file.folder.includes(dv.current().file.folder)  && file.frontmatter.completed && !file.name.contains("Template")) 
>		.map(file => {
>			return [
>				dv.fileLink(file.path), 
>			]
>		} )
>	)
> ```


----
## Unfinished Sections
```dataviewjs
dv.table(["File", "Creation", "Edit", "Tags", "Path", "Priority", "Status", "Completed"],
	dv.pages("#system").file
		.filter(file => file.folder.includes(dv.current().file.folder)  && !file.frontmatter.completed && !file.name.contains("Template")) 
		.sort(file => file.folder && -file.frontmatter.status)
		.map(file => {
			let priority = "None"
			if (dv.equal(file.frontmatter.priority, 1))
				priority = "<span style='color: Green;'>Low</span>";
			else if (dv.equal(file.frontmatter.priority, 2))
				priority = "<span style='color: Orange;'>Mid</span>";
			else if (dv.equal(file.frontmatter.priority, 3))
				priority = "<span style='color: Red;'>High</span>";

			let status = "Idea"
			if (dv.equal(file.frontmatter.status, 1))
				status = "<span style='color: Red;'>TODO</span>";
			else if (dv.equal(file.frontmatter.status, 2))
				status = "<span style='color: Orange;'>In Progress</span>";
			else if (dv.equal(file.frontmatter.status, 3))
				status = "<span style='color: Orange;'>Active Dev</span>";
			else if (dv.equal(file.frontmatter.status, 4))
				status = "<span style='color: Pink;'>Needs Testing</span>";
			else if (dv.equal(file.frontmatter.status, 5))
				status = "<span style='color: Green;'>Completed</span>";

			let completed = file.frontmatter.completed
			if (completed)
				completed = "<span style='color: Green;'>Completed</span>";
			else
				completed = "<span style='color: Orange;'>Not Completed</span>";

			return [
				dv.fileLink(file.path), 
				file.ctime, 
				file.mtime, 
				file.tags,
				file.folder,
				priority,
				status,
				completed,
			]
		} )
)
```

----
## Tasks

----
## External Resources


