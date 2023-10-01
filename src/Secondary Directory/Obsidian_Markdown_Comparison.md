# What Obsidian Features Work in mdBook?
- This is a simple file containing a handful of different Obsidian markdown features to have a visual comparison of how formatting carries over.
	- This is assuming no fancy pre-processors are being used; strictly default settings.

### Tables
---
**mdBook Version:**

| Category 1 | Category 2 | Category 3 |
| --- | --- | --- |
| Example | ``Example`` | Longer example here. | 
| Example | ``Example`` | Longer example here. | 
| Example | ``Example`` | Longer example here. | 


**Obsidian Version:**

![Obsidian Tables](../attachments/Obsidian_Tables.png)

### Checklists
---
**mdBook Version:**

- [ ] Thingy 1
- [ ] Thingy 2
- [ ] Thingy 3

**Obsidian Version:**

![Obsidian Checklists](../attachments/Obsidian_Checklists.png)


### Message Blocks (Callouts)
---
**mdBook Version (Part 1):**

> [!info]
> Information here.
> Alises: N/A

> [!todo]
> To-do list here.
> Alises: N/A

> [!tip]
> Important info here!
> Aliases: ``hint``, ``important``

>[!success]
>Success here!
> Aliases: ``check``, ``done``

> [!question]
> Freqently asked questions!
> Aliases: ``help``, ``faq``

**Obsidian Version (Part 1):**

![Obsidian Callouts](../attachments/Obsidian_Callouts_P1.png)

**mdBook Version (Part 2):**

>[!warning]
> Warning here!
> Aliases: ``caution``, ``attention``

>[!failure]
> Failure here!
> Aliases: ``fail``, ``missing``

> [!danger]
> Bruh!!!
> Aliases: N/A

> [!bug]
> Known bugs here!
> Aliases: N/A

> [!example]
> Example here!
> Aliases: N/A

> [!quote]
> Quote here!
> Aliases: N/A


**Obsidian Version (Part 2):**

![Obsidian Callouts](../attachments/Obsidian_Callouts_P2.png)

### Mermaid Diagrams
---
**mdBook Version:**

```mermaid
graph TD;
	A[Originator] --> B[Route 1];
	A --> C[Route 2];
	B --> D[Finish];
	C --> D;
```

```mermaid
flowchart LR

A[Hard] -->|Text| B(Round)
B --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```mermaid
gantt
    section Section
    Completed :done,    des1, 2014-01-06,2014-01-08
    Active        :active,  des2, 2014-01-07, 3d
    Parallel 1   :         des3, after des1, 1d
    Parallel 2   :         des4, after des1, 1d
    Parallel 3   :         des5, after des3, 1d
    Parallel 4   :         des6, after des4, 1d
```


```mermaid
pie
"Dogs" : 386
"Cats" : 85.9
"Rats" : 15
```

**Obsidian Version:**

![Obsidian Diagrams](../attachments/Obsidian_Diagrams.png)

### Code Blocks
---
**mdBook Version:**

```powershell
# PowerShell Code Block
Get-ChildItem "C:\Windows\system32"
```

```shell
#!/bin/bash
echo "yo momma"
```

**Obsidian Version:**

![Obsidian Codeblocks](../attachments/Obsidian_Codeblocks.png)