
| Property         | Value                             |
| ---------------- | --------------------------------- |
| 📅 Date          | 30-08-2024, 00:15                 |
| 🏷️ Tags         | #databases, #sql          |
| 🔗 Related Notes | [[CMU Intro to Database Systems]] |
A database could be a file with comma/newline separated values. We can use a programming language of choice to read this file and produce whatever result that we want. There are several issues with this:
- Every unique operation requires one to write new piece of code specifically for that operation.
- The format of the file might need a change, adding a new column, rendering all pre-written code useless.
- What is someone randomly messes with the text tile, that would render the code useless as well.
- What if someone does not take into consideration the domain of the data and adds invalid/meaningless data.
- What if the database is needed on a different device, this would also mean that the code should be platform independent. Also that every bit should be in one language.
- What if the machine crashes while we are writing something to the file.
- What is two threads are trying to access the same file. We can use the locks provided by the OS, but the OS is our enemy, and we can always do it better than the operating system.

A <span style="color:#e1db3d">DBMS</span> is a software that allows one to simply not give a fuck about any of the above issues. It provides you the ability to store and retrieve data that follows some sort of a data model. A <span style="color:#e1db3d">Data model</span> simply means how the data is stored, what relationships it has. The <span style="color:#e1db3d">schema</span> simply means how to make sense of the bits that is stored on the disk.
Data Models are : Relational, Key-Value, Graph, Document/JSON/XML/Object.
*read more: [[Chapter 2 - Data Models and Query Languages]]*

In the early days, humans(programmers) were cheap and compute was expensive, so the early systems had every operation manually written by programmers, in a highly optimized manner. When there were design changes, all the code had to be re-written, to re-organize the data and later query it. That when a chad mathematician (cause ofc Computer Science is not a field, it'll always be these genius outsiders that would take up "coding" for fun and solve a fundamental problem) <span style="color:#e1db3d">Edgar Codd</span> came up with the relational model in 1969 *{nice}*, based on relations (an extension of sets from discrete mathematics), A data model that would kick ass for decades, all the way to present date.![[edgar_codd.png]] E. F. Codd (fucker even looks badass)![[db_bois.png]]
<span style="color:#e1db3d">Codd</span>: The Relational Model
<span style="color:#e1db3d">Bachman</span>: Codasyl
<span style="color:#e1db3d">Gray</span>: 2PL protocol
<span style="color:#e1db3d">Stonebreaker</span>: Postgres

In the early days it was said that no program(compiler) could generate assembly code better or even as good as what a human could write, given a C program. But that was incorrect and compilers have gotten much more efficient since. Similar is the case for relational databases. The paradigm that writing an SQL query and having the DBMS figure out an execution plan that would be as efficient as a human was considered impossible, the query optimizer has come a long way since then.

<span style="color:#e1db3d">Structure</span>: It allows us to store the data with certain structure, independent from their physical representation.
<span style="color:#e1db3d">Integrity Constraints</span>: Applying checks on what values the data can take, this reflects what properties the data should follow in real life.
<span style="color:#e1db3d">Manipulation</span>: There should be an API to interact with this data, that should be flexible.
<span style="color:#e1db3d">Data Independence</span>: Isolates the user from the lower level data representation, user doesn't care how data is physically stored. Other than that we can create logical isolations, called <span style="color:#e1db3d">views</span> where, any user sees according to their clearance levels.

<span style="color:#e1db3d">Relation</span>: A relation is a table, this stands for some actual entity in the real life, this could be a real thing or an abstract thing, like an artist or a transaction, the columns in this table correspond to the attributes of the entity and the table holds these set of values forming each entity. 

<span style="color:#e1db3d">Constraints</span>: We can have constraints, these are rules which are enforced on attribute values, if a value does not conform to the constraint, the operation of adding said value is not allowed.
- Not Null: Value can't be absent.
- Referential: The following value should be present in a different table.
- Value Constraints: Greater than, less than. 

<span style="color:#e1db3d">Data Manipulation Languages</span>: There are broadly two types:
- Procedural (Relational Algebra): Here using Relational Algebra we step by step tell the system how to execute the query, this API is rarely exposed by database systems.
- Declarative (Relational Calculus): Here we simply demand the end result and the DBMS figures out how to get it. SQL is declarative and that what makes SQL easy.

<span style="color:#e1db3d">Fundamental operators</span> that can be applied on relations, select, project, union, intersection, difference, product and join. Extra Operators that were added after the first paper by Codd were, Rename, Assignment, Duplicate Elimination, Aggregation, Sorting, Division.