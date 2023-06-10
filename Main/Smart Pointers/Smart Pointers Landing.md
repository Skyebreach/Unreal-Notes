---
tags: [system, landing, misc]
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

-----

## Feature Overview


-----
## Sub system Map
```dataviewjs
dv.header(3, "Completed")
dv.table(["File", "Creation", "Edit", "Tags", "Path"],
	dv.pages("#subsystem").file
		.filter(file => file.folder.includes(dv.current().file.folder) && file.frontmatter.completed ) 
		.sort(file => file.folder)
		.map(file => {
			return [
				dv.fileLink(file.path), 
				file.ctime, 
				file.mtime, 
				file.tags,
				file.folder
			]
		} )
)

dv.header(3, "Not Completed")
dv.table(["File", "Creation", "Edit", "Tags", "Path", "Priority", "Status", "Completed"],
	dv.pages("#subsystem").file
		.filter(file => file.folder.includes(dv.current().file.folder)  && !file.frontmatter.completed ) 
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


-----
## Tasks
```dataviewjs
const result = await dv.query(`
	TABLE WITHOUT ID "<nobr>" + file.link + "</nobr>" AS Page, Ideas, TODO, InProg, Active, Testing, Completed
	FROM #task
	WHERE contains(file.folder, this.file.folder)
	FLATTEN list(filter(file.lists, (x) => meta(x.section).subpath = "Ideas").text) as Ideas
	FLATTEN list(filter(file.lists, (x) => meta(x.section).subpath = "TODO").text) as TODO
	FLATTEN list(filter(file.lists, (x) => meta(x.section).subpath = "In Progress").text) as InProg
	FLATTEN list(filter(file.lists, (x) => meta(x.section).subpath = "Active Dev").text) as Active
	FLATTEN list(filter(file.lists, (x) => meta(x.section).subpath = "Needs Testing").text) as Testing
	FLATTEN list(filter(file.lists, (x) => meta(x.section).subpath = "Completed").text) as Completed	
`);

if (result.successful)
{
	let sizes = result.value.values;
	for (let i = 0; i < sizes.length; i++ ) {
		for (let j = 1; j < sizes[i].length; j++ ) {
			sizes[i][j] = sizes[i][j].length
		}
	}
	dv.table(result.value.headers, sizes );
}
else
  dv.paragraph("~~~~\n" + result.error + "\n~~~~")
```