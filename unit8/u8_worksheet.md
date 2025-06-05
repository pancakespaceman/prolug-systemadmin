# Unit 8 Worksheet - Scripting

## Resources / Important Links

(TLDP Bash Beginner's Guide)[https://tldp.org/LDP/Bash-Beginners-Guide/html/chap_01.html]
(Bash Cheatsheet)[https://devhints.io/bash]
(Bash Hacker's Wiki)[https://web.archive.org/web/20230406205817/https://wiki.bash-hackers.org/]

## Discussion Posts

### 1

Scenario:

```
It's a 2 week holiday in your country and most of the engineers and architects who
designed the system are out of town.

You've noticed a pattern of logs filing up on a set of web servers from increased
traffic. Your research shows, and then you verify, that the logs are being sent off
real time to Splunk. Your team has just been deleting the logs every few days, but
one of the 3rd shift engineers didn't read the notes and your team suffered downtime.

How might you implement a simple fix to stop gap the problem before all the engineering
resources come back next week?
```

Scenario Response:

```
For a simple fix, I would create a bash script that would check the storage amount
of the log folder and, if nearing capacity, it would delete logs to free up storage
capacity. I would then set this on a cronjob to run every day or every 12 hours.
```

1. What resources helped you answer this?

```
Previous lessons with cron and my own experience.
```

2. Why can't you just make a design fix and add space in /var/log on all these systems?

```
Depending on how the system is configured, you can't simply just add space.
Also, the system engineers/architects are the designers of the system. They would
need to be the ones to implement a more permament fix for this issue.
```

3. Why can't you just make a design change and logrotate more often so this doesn't happen?

```
In this scenario, I and my team are not the designers of the system. Without their
approval, I wouldn't make to make significant changes to how the system runs.
```

4. For 2, 3 if you are ok with that, explain your answer. 

```
I'm not okay with any significant design changes for this scenario.
I would simply implement my mentioned fix and document what I've done.
```


### 2

Scenario:

```
You are the only Linux Administrator at a small healthcare company. 
The engineer/admin before you left you a lot of scripts to untangle. 
This is one of our many tasks as administrators, so you set out to accomplish it. 
You start to notice that he only ever uses nested if statements in bash.

You also notice that every loop is a conditional while true, and then he breaks 
the loop after a decision test each loop. You know his stuff works, but you think 
it could be more easily written for supportability, for you and future admins. 
You decide to write up some notes by reading some google, AI, and talking to your peers.
```

1. Compare the use of nested if versus case statement in bash.

```
If-else statements work well for expressions based on ranges of values or conditions
where switch statements work well with expressions based on a single piece of data.
Also, readability is always a concern, as nested if statements can easily become
a nested mess of unreadability. Personally, I avoid nested if statements 
as much as possible, by restructuring logic or by using switch statements.
However, sometimes nested if statements are unavoidable for the problem at hand.
```

2. Compare the use of conditional and counting loops. Under what circumstances would
you use on or the other?

```
Do you need to loop until a certain condition or loop a certain number of times?
Thats the essence between these two loop structures.
```

### Responses

[x] Post 1
[x] Post 2

## Definitions

Variables:
	- A named sotrage location in memory that holds a value which can be changes
	during program execution.
	- https://en.wikipedia.org/wiki/Variable_(computer_science)

Interpreted program:
	- a program that is executed line-by-line by an interpreter at runtime, without
	prior compilation into machine code. This allows for easier debugging and platform
	independence but may result in slower execution compared to compiled programs.
	- https://en.wikipedia.org/wiki/Interpreter_(computing)

Compiled program:
	- a program that is transformed from source code into machine code by a compiler
	before execution. This process results in an executable file that can run
	directly on the hardware, often leading to faster performance.
	- https://en.wikipedia.org/wiki/Compiled_language

Truth table:
	- a mathematical table used in logic to determin the output of a logical expresion
	based on all possible combinations of its inputs. It is fundamental in Boolean
	algebra and digital circuit design.
	- https://en.wikipedia.org/wiki/Truth_table

AND/OR logic:
	- AND and OR are fundamental logical operators
		- AND (conjunction): returns true only if both operands are true.
		- OR (disjunction): returns true if at least one operand is true.
	- https://en.wikipedia.org/wiki/Logical_conjunction
	- https://en.wikipedia.org/wiki/Logical_disjunction

Single/Dual/Multiple alternative logic:
	- These terms refer to decision-making structures in programming:
		- Single alternative: executes a block of code if a condition is true
		- Double Alternative: executes one block if the condition is true, another
		if false
		- Multiple Alternative: evaluates multiple conditions in sequence, executing
		the corresponding block for the first true condition.
	- https://www.isbe.net/CTEDocuments/BMCE-L780027.pdf

## Digging Deeper

1. Read:

	- https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html
	- https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_02.html
	- https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html

What did you learn about capabilities of bash that can help you in scripting?

2. If you want to dig more into truth tables and logic, this is a good start:
https://en.wikipedia.org/wiki/Truth_table

## Reflection Questions

1. What questions do you still have about this week?

2. Just knowing a log about scripting doesn't help much against actually doing it
in a practical sense. What things are you doing currently at work or in a lab
that you can apply some of this logic to?





