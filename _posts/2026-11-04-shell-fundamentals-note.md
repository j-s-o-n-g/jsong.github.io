---
title: "Shell Fundamentals Note"
date: 2026-11-04
categories: [Linux]
tags: [bash, shell, scripting, linux]     # TAG names should always be lowercase
comments: true
---

I created these notes as a structured reference and recap of Bash shell scripting fundamentals. Shell scripting is essential for automating routine administrative tasks, managing system processes, and chaining Linux commands into reliable workflows.

This guide compiles foundational shell scripting concepts—from basic script structure, variable interpolation, and user input to conditional branches (`if-then`, `case`), iterative loops (`for`, `while`), and file test operators—formatted with clean practical examples and output breakdowns.

<details markdown="1">
<summary><strong>Note 1: Basic Shell Scripts and User Input</strong></summary>

## 1. Output to Screen with `echo` and Shebang

Every Bash script begins with a **shebang** (`#!/bin/bash`). The shebang tells the operating system which interpreter to use to execute the script instructions.

```bash
#!/bin/bash
# Simple output script
echo "Hello World"
```

```text
Hello World
```

### Key Concepts
- `#!/bin/bash`: Directs the shell to execute the script using the Bash shell located at `/bin/bash`.
- `#`: Lines starting with `#` are treated as comments and ignored during script execution.
- `echo`: Built-in command that outputs text strings or variables to standard output.

---

## 2. Defining and Chaining Routine Tasks

A shell script can execute multiple sequential system commands to automate diagnostic or administrative tasks.

```bash
#!/bin/bash
# Define small tasks

whoami
echo
pwd
echo
hostname
echo
ls -ltr
echo
```

```text
jay

/Users/jay/chirpy-starter-main

linux-server-01

total 160
-rw-rw-r--  1 jay staff    324 Jul 26 2025 Gemfile
-rw-rw-r--  1 jay staff   7143 Jun 11 04:46 _config.yml
drwxrwxr-x 14 jay staff    448 Nov 03 18:35 _posts
```

### Breakdown
- `whoami`: Prints the active user identity.
- `echo`: An empty `echo` statement inserts a clean blank line in terminal output for readability.
- `pwd`: Prints the current working directory path.
- `hostname`: Displays the system network hostname.
- `ls -ltr`: Lists files in long format (`-l`), sorted by modification time (`-t`) in reverse order (`-r`).

---

## 3. Defining Variables and Quoting Differences

Variables store data values for reuse throughout a script. In Bash, assignments must not contain spaces around the `=` sign.

```bash
#!/bin/bash
# Example of defining variables

a=Imran
b=Afzal
c='Linux class'

echo "My first name is $a"
echo "My surname is $b"
echo 'My surname is $c'
```

```text
My first name is Imran
My surname is Afzal
My surname is $c
```

### Double Quotes (`"`) vs. Single Quotes (`'`)
- **Double Quotes (`"..."`)**: Enable **variable interpolation**. The shell evaluates `$a` and `$b` and substitutes their assigned values.
- **Single Quotes (`'...'`)**: Preserve the **literal text**. Variable expansion is suppressed, printing `$c` as raw characters.

---

## 4. Reading User Input with `read`

The `read` command pauses script execution to capture keyboard input from the user and stores it inside the specified variable.

```bash
#!/bin/bash
# Read user input

echo "What is your first name?"
read a
echo
echo "What is your last name?"
read b
echo
echo Hello $a $b
```

```text
What is your first name?
Imran

What is your last name?
Afzal

Hello Imran Afzal
```

### Explanation
- `read a`: Waits for standard input and assigns the entered text to variable `a`.
- `read b`: Captures the next line of input and stores it in variable `b`.
- `echo Hello $a $b`: Combines both variables into a greeting message.

---

## 5. Command Substitution (Running Commands Inside Strings)

Command substitution allows the output of a command to be captured and inserted into another command or string. This can be accomplished using **backticks** (`` `command` ``) or modern syntax (`$(command)`).

```bash
#!/bin/bash
# Script to run commands within

clear
echo "Hello `whoami`"
echo
echo "Today is `date`"
echo
echo "Number of user login: `who | wc -l `"
echo
```

```text
Hello jay

Today is Wed Nov  4 18:00:00 KST 2026

Number of user login: 2
```

### Command Breakdown
- `clear`: Clears the active terminal screen before displaying output.
- `` `whoami` ``: Substituted with the current logged-in username.
- `` `date` ``: Substituted with the current system date and time.
- `` `who | wc -l` ``: Pipes the active login list (`who`) to the line counter (`wc -l`) to determine active sessions.

---

## 6. Practical Script: Interactive File Renaming

Combining `read` and file manipulation commands allows scripts to perform guided operations dynamically.

```bash
#!/bin/bash
# This script will rename a file

echo Enter the file name to be renamed
read oldfilename
echo Enter the new file name
read newfilename

mv $oldfilename $newfilename
echo The file has been renamed as $newfilename
```

```text
Enter the file name to be renamed
draft.txt
Enter the new file name
final.txt
The file has been renamed as final.txt
```

### Execution Steps
1. The script prompts the user for the existing source filename and assigns it to `$oldfilename`.
2. The user supplies the target filename, stored in `$newfilename`.
3. `mv $oldfilename $newfilename` renames the file on the filesystem.
4. A confirmation message outputs the new filename.

</details>

<details markdown="1">
<summary><strong>Note 2: Conditional Statements (if-then-else) and Test Operators</strong></summary>

## 1. Checking Variables with Numeric Comparison

The `if-then-else` construct executes specific commands depending on whether a test condition returns success (exit status `0`).

```bash
#!/bin/bash
count=100

if [ $count -eq 100 ]
then
  echo Count is 100
else
  echo Count is not 100
fi
```

```text
Count is 100
```

### Syntax Notes
- `[` and `]`: The test command bracket requires spaces around the condition (`[ $count -eq 100 ]`).
- `-eq`: Numeric test operator checking whether two values are equal.
- `then` / `else` / `fi`: Delimiters defining the conditional branch and ending the block (`fi` is `if` reversed).

---

## 2. Checking File Existence with the `-e` Operator

The `-e` operator verifies whether a file or directory exists on disk.

```bash
#!/bin/bash
clear

if [ -e /home/iafzal/error.txt ]
then
  echo "File exist"
else
  echo "File does not exist"
fi
```

```text
File does not exist
```

---

## 3. String Comparison Against Command Output

Commands like `awk` can extract specific fields from command output to evaluate conditions dynamically.

```bash
#!/bin/bash
a=`date | awk '{print $1}'`

if [ "$a" == Mon ]
then
  echo Today is $a
else
  echo Today is not Monday
fi
```

```text
Today is not Monday
```

### Key Points
- `date | awk '{print $1}'`: Extracts the first whitespace-delimited field of the `date` command output (e.g., weekday abbreviation).
- `==`: String comparison operator checking for an exact text match.

---

## 4. Multi-Branch Evaluation with `elif`

The `elif` keyword enables multiple mutually exclusive branches within a single conditional block.

```bash
#!/bin/bash
clear
echo
echo "What is your name?"
echo
read a
echo
echo Hello $a sir
echo
echo "Do you like working in IT? (y/n)"
read Like
echo

if [ "$Like" == y ]
then
  echo You are cool
elif [ "$Like" == n ]
then
  echo You should try IT, it’s a good field
  echo
fi
```

```text
What is your name?
Alex

Hello Alex sir

Do you like working in IT? (y/n)
y

You are cool
```

---

## 5. Compound Conditions and Advanced Test Patterns

Shell scripting supports logical operators and special environment variables inside test statements.

### Logical OR (`||`) Across Multiple Tests
```bash
if [ "$a" = Monday ] || [ "$a" = Tuesday ]
```
Executes the branch if variable `$a` matches either `Monday` or `Tuesday`.

### Testing File Size with `test -s`
```bash
if test -s error.txt
```
Evaluates to true if `error.txt` exists and contains data (size greater than `0` bytes).

### Common Conditional Verification Patterns

| Pattern | Description |
| :--- | :--- |
| `if [ $? -eq 0 ]` | Checks if the preceding command finished with exit code `0` (success). |
| `if [ -e /export/home/filename ]` | Checks if the specified file or path exists. |
| `if [ "$a" != "" ]` | Checks if variable `$a` is not empty (contains characters). |
| `if [ "$error_code" != "0" ]` | Checks if the error code does not equal zero. |

---

## 6. Comparison Operators Reference

Bash uses distinct operators for numeric comparisons versus string/character comparisons.

| Operation | Numeric Operator | String Operator | Notes |
| :--- | :---: | :---: | :--- |
| **Equal to** | `-eq` | `==` or `=` | `[ $n -eq 10 ]` vs `[ "$str" == "val" ]` |
| **Not equal to** | `-ne` | `!=` | `[ $n -ne 0 ]` vs `[ "$str" != "abc" ]` |
| **Less than** | `-lt` | `<` (inside `[[ ]]`) | `-lt` strictly checks integer values. |
| **Less than or equal** | `-le` | `<=` (inside `[[ ]]`) | Checks numeric magnitude. |
| **Greater than** | `-gt` | `>` (inside `[[ ]]`) | Checks numeric magnitude. |
| **Greater than or equal** | `-ge` | `>=` (inside `[[ ]]`) | Checks numeric magnitude. |

---

## 7. File Test Operators Reference

File test operators verify properties, types, and permissions of files on the filesystem.

| Operator | Test Condition |
| :---: | :--- |
| `-s` | File exists and its size is greater than zero (not empty). |
| `-f` | File exists and is a regular file (not a directory or device). |
| `-d` | Target exists and is a directory. |
| `-x` | File exists and has execute permission for the current user. |
| `-w` | File exists and has write permission for the current user. |
| `-r` | File exists and has read permission for the current user. |

</details>

<details markdown="1">
<summary><strong>Note 3: Multi-Branch Selection with Case Statements</strong></summary>

## 1. Interactive Menu Script with `case`

When branching logic depends on matching a single variable against multiple possible values, a `case` statement provides cleaner, more readable code than nested `if-elif` statements.

```bash
#!/bin/bash

echo
echo Please chose one of the options below
echo
echo 'a = Display Date and Time'
echo 'b = List file and directories'
echo 'c = List users logged in'
echo 'd = Check System uptime'
echo

read choices

case $choices in
  a) date;;
  b) ls;;
  c) who;;
  d) uptime;;
  *) echo Invalid choice - Bye.;;
esac
```

```text
Please chose one of the options below

a = Display Date and Time
b = List file and directories
c = List users logged in
d = Check System uptime

c
jay   console  Nov  4 09:15
jay   ttys000  Nov  4 10:20
```

### Syntax Breakdown
- `case $choices in`: Evaluates the value stored in `$choices`.
- `pattern)`: Matches a specific value option.
- `;;`: Double semicolons terminate each case block (similar to `break` in C/Python).
- `*)`: Wildcard pattern matching any input not explicitly handled above (default fallback).
- `esac`: Closes the `case` statement (`case` reversed).

---

## 2. Pattern Alternation: Day-Based Backup Script

The pipe character (`|`) allows multiple patterns to share the same execution block within a `case` statement.

```bash
#!/bin/bash
# This script will look at your current day and tell you the state of the backup

NOW=$(date +"%a")

case $NOW in
  Mon)
    echo "Full backup";;
  Tue|Wed|Thu|Fri)
    echo "Partial backup";;
  Sat|Sun)
    echo "No backup";;
  *) ;;
esac
```

```text
Partial backup
```

### Mechanism
- `date +"%a"`: Formats the current date to return the abbreviated weekday name (`Mon`, `Tue`, `Wed`, `Thu`, `Fri`, `Sat`, `Sun`).
- `Tue|Wed|Thu|Fri)`: Matches any Tuesday through Friday and triggers the partial backup workflow.
- `Sat|Sun)`: Matches weekend days and skips backup routines.

</details>

<details markdown="1">
<summary><strong>Note 4: Loop Iteration with For Loops</strong></summary>

## 1. Simple Number and String Iteration

A `for` loop iterates over each element in a given list, executing the enclosed commands for each item.

### Looping Over Numbers
```bash
#!/bin/bash
for i in 1 2 3 4 5
do
  echo "Welcome $i times"
done
```

```text
Welcome 1 times
Welcome 2 times
Welcome 3 times
Welcome 4 times
Welcome 5 times
```

### Looping Over Words (Strings)
```bash
#!/bin/bash
for i in eat run jump play
do
  echo See Imran $i
done
```

```text
See Imran eat
See Imran run
See Imran jump
See Imran play
```

---

## 2. Bulk File Operations with Brace Expansion `{1..5}`

Brace expansion (`{start..end}`) generates arbitrary sequences automatically without writing out individual items.

### Creating 5 Files
```bash
#!/bin/bash
for i in {1..5}
do
  touch $i
done
```

### Deleting 5 Files
```bash
#!/bin/bash
for i in {1..5}
do
  rm $i
done
```

### Explanation
- `{1..5}`: Expands to `1 2 3 4 5`.
- `touch $i`: Creates an empty file named after the current value of `$i`.
- `rm $i`: Removes the corresponding file during each loop cycle.

---

## 3. Tracking Loop Counter with Arithmetic Expansion

Arithmetic expansion `$((...))` allows incrementing a counter variable inside the loop body.

```bash
#!/bin/bash
i=1

for day in Mon Tue Wed Thu Fri
do
  echo "Weekday $((i++)) : $day"
done
```

```text
Weekday 1 : Mon
Weekday 2 : Tue
Weekday 3 : Wed
Weekday 4 : Thu
Weekday 5 : Fri
```

### Key Concept
- `$((i++))`: Post-increments the value of `i`. The current value of `i` is printed first, and then increased by `1` for the subsequent iteration.

---

## 4. Iterating Over Command Output (`/etc/passwd` Parsing)

Command substitution can supply dynamic lists to a `for` loop, such as parsing lines from a system configuration file.

```bash
#!/bin/bash
i=1

for username in `awk -F: '{print $1}' /etc/passwd`
do
  echo "Username $((i++)) : $username"
done
```

```text
Username 1 : root
Username 2 : daemon
Username 3 : bin
Username 4 : sys
Username 5 : sync
```

### Command Breakdown
- `/etc/passwd`: System file containing local user account details separated by colons (`:`).
- `awk -F: '{print $1}'`: Sets the field separator to `:` and extracts the first column (`$1`), which corresponds to the username.
- The loop enumerates and outputs every system user registered in the file.

</details>

<details markdown="1">
<summary><strong>Note 5: Conditional Loops with While</strong></summary>

## 1. Counter-Based `while` Loop

A `while` loop repeatedly executes a command block as long as the test condition remains true.

```bash
#!/bin/bash
c=1

while [ $c -le 5 ]
do
  echo "Welcome $c times"
  (( c++ ))
done
```

```text
Welcome 1 times
Welcome 2 times
Welcome 3 times
Welcome 4 times
Welcome 5 times
```

### Explanation
- `c=1`: Initializes the counter variable.
- `[ $c -le 5 ]`: Evaluates whether `$c` is less than or equal to `5`.
- `(( c++ ))`: Arithmetic evaluation construct incrementing `$c` by `1`.
- When `$c` reaches `6`, the condition evaluates to false and the loop terminates.

---

## 2. Timed Countdown and Process Stopping Simulation

`while` loops combined with `sleep` and positional parameters (`$1`) can manage process monitoring and timed delays.

```bash
#!/bin/bash
count=0
num=10

while [ $count -lt 10 ]
do
  echo
  echo $num seconds left to stop this process $1
  echo
  sleep 1
  num=`expr $num - 1`
  count=`expr $count + 1`
done

echo
echo $1 process is stopped!!!
echo
```

```text
# Executed as: ./stop_process.sh httpd

10 seconds left to stop this process httpd

9 seconds left to stop this process httpd

8 seconds left to stop this process httpd

...

1 seconds left to stop this process httpd

httpd process is stopped!!!
```

### Breakdown
- `$1`: Positional parameter representing the first argument passed to the script (e.g., process name `httpd`).
- `sleep 1`: Pauses script execution for 1 second during each cycle.
- ``num=`expr $num - 1` ``: Evaluates arithmetic subtraction using `expr` to decrease the remaining second count.
- ``count=`expr $count + 1` ``: Increments the iteration counter until the condition `[ $count -lt 10 ]` is satisfied.
- Final `echo`: Announces that the targeted process has been stopped.

</details>
