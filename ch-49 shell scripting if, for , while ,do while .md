# Bash Shell Scripting

## 140. Input and Output of Script

## 141. if-then Scripts

## 142. for Loop Scripts

## 143. do-while Scripts

---

# Introduction

A shell script is a text file containing Linux commands that are executed sequentially by a shell interpreter such as Bash.

Example:

```bash
#!/bin/bash

echo "Hello World"
```

Run the script:

```bash
chmod +x script.sh
./script.sh
```

Output:

```
Hello World
```

---

# 140. Input and Output of Script

## What is Input?

Input is data provided to a script.

Sources:

* User keyboard input
* Command-line arguments
* Files
* Output of another command

## What is Output?

Output is the result produced by the script.

Destinations:

* Terminal
* File
* Another command

---

# Method 1: User Input

The `read` command accepts input from the keyboard.

Syntax:

```bash
read variable_name
```

Example:

```bash
#!/bin/bash

echo "Enter your name:"
read name

echo "Welcome $name"
```

Execution:

```
Enter your name:
Prins
Welcome Prins
```

---

## Reading Multiple Variables

```bash
#!/bin/bash

echo "Enter First Name and Last Name:"
read fname lname

echo "First Name: $fname"
echo "Last Name: $lname"
```

Input:

```
Prins Paghadar
```

Output:

```
First Name: Prins
Last Name: Paghadar
```

---

## Hidden Password Input

```bash
#!/bin/bash

echo "Enter Password:"
read -s pass

echo
echo "Password Saved"
```

The `-s` option hides typed characters.

Production use:

* Login scripts
* Password validation

---

# Command-Line Arguments

Script:

```bash
#!/bin/bash

echo "Script Name: $0"
echo "First Argument: $1"
echo "Second Argument: $2"
echo "Total Arguments: $#"
```

Run:

```bash
bash test.sh Linux Bash
```

Output:

```
Script Name: test.sh
First Argument: Linux
Second Argument: Bash
Total Arguments: 2
```

---

# Important Special Variables

| Variable | Description             |
| -------- | ----------------------- |
| $0       | Script name             |
| $1       | First argument          |
| $2       | Second argument         |
| $#       | Number of arguments     |
| $*       | All arguments           |
| $$       | Process ID              |
| $?       | Previous command status |

Example:

```bash
ls

echo $?
```

Output:

```
0
```

Means command executed successfully.

---

# Output Redirection

Save output to file:

```bash
echo "Linux" > file.txt
```

Append output:

```bash
echo "Bash" >> file.txt
```

Read file:

```bash
cat file.txt
```

Output:

```
Linux
Bash
```

---

# Production Example

Backup script:

```bash
#!/bin/bash

echo "Enter Directory:"
read dir

tar -cvf backup.tar $dir

echo "Backup Complete"
```

---

# 141. if-then Scripts

## What is if?

The if statement makes decisions.

Syntax:

```bash
if condition
then
    commands
fi
```

---

# Numeric Comparison

```bash
#!/bin/bash

num=100

if [ $num -eq 100 ]
then
    echo "Equal"
fi
```

Output:

```
Equal
```

---

# if-else

```bash
#!/bin/bash

age=20

if [ $age -ge 18 ]
then
    echo "Adult"
else
    echo "Minor"
fi
```

Output:

```
Adult
```

---

# if-elif-else

```bash
#!/bin/bash

marks=82

if [ $marks -ge 90 ]
then
    echo "Grade A"
elif [ $marks -ge 75 ]
then
    echo "Grade B"
else
    echo "Grade C"
fi
```

Output:

```
Grade B
```

---

# File Checking

Check whether a file exists.

```bash
#!/bin/bash

if [ -f test.txt ]
then
    echo "File Exists"
else
    echo "File Not Found"
fi
```

---

# Directory Check

```bash
if [ -d backup ]
then
    echo "Directory Exists"
fi
```

---

# Production Example

Disk Usage Alert:

```bash
#!/bin/bash

usage=90

if [ $usage -gt 80 ]
then
    echo "Disk Space Warning"
fi
```

System administrators use similar scripts.

---

# Comparison Operators

| Operator | Meaning       |
| -------- | ------------- |
| -eq      | Equal         |
| -ne      | Not Equal     |
| -gt      | Greater       |
| -lt      | Less          |
| -ge      | Greater Equal |
| -le      | Less Equal    |

---

# 142. for Loop Scripts

## What is a for loop?

A for loop repeats commands for each item in a list.

Syntax:

```bash
for variable in list
do
    commands
done
```

---

# Example 1

```bash
#!/bin/bash

for i in 1 2 3 4 5
do
    echo $i
done
```

Output:

```
1
2
3
4
5
```

---

# Example 2

```bash
#!/bin/bash

for fruit in Apple Mango Banana
do
    echo $fruit
done
```

Output:

```
Apple
Mango
Banana
```

---

# C Style for Loop

```bash
#!/bin/bash

for ((i=1;i<=5;i++))
do
    echo $i
done
```

Output:

```
1
2
3
4
5
```

---

# Loop Through Files

```bash
for file in *
do
    echo $file
done
```

Displays all files.

---

# Production Example

Install packages:

```bash
#!/bin/bash

for package in vim git wget
do
    echo "Installing $package"
done
```

---

# Nested for Loop

```bash
for i in 1 2
do
    for j in A B
    do
        echo "$i $j"
    done
done
```

Output:

```
1 A
1 B
2 A
2 B
```

---

# 143. do-while Scripts

Bash does not have a direct do-while loop.

Equivalent loops:

* while
* until

---

# while Loop

Syntax:

```bash
while condition
do
    commands
done
```

Example:

```bash
#!/bin/bash

i=1

while [ $i -le 5 ]
do
    echo $i
    i=$((i+1))
done
```

Output:

```
1
2
3
4
5
```

---

# User Controlled Loop

```bash
#!/bin/bash

answer=yes

while [ "$answer" = "yes" ]
do
    echo "Continue?"
    read answer
done
```

Input:

```
yes
yes
no
```

Loop stops after "no".

---

# until Loop

Runs until the condition becomes true.

```bash
#!/bin/bash

i=1

until [ $i -gt 5 ]
do
    echo $i
    i=$((i+1))
done
```

Output:

```
1
2
3
4
5
```

---

# Infinite Loop

```bash
while true
do
    echo "Running..."
    sleep 2
done
```

Stop with:

```
Ctrl+C
```

---

# Production Example

Wait for a service:

```bash
#!/bin/bash

while ! ping -c 1 google.com
do
    echo "Waiting for network..."
    sleep 5
done

echo "Network Available"
```

---

# Summary

| Topic                  | Purpose                          |
| ---------------------- | -------------------------------- |
| read                   | Accept user input                |
| echo                   | Display output                   |
| Command-line arguments | Pass values to scripts           |
| if                     | Single decision                  |
| if-else                | Two-way decision                 |
| elif                   | Multiple decisions               |
| for                    | Fixed iterations                 |
| while                  | Condition-based loop             |
| until                  | Run until condition becomes true |
| Output redirection     | Save output to files             |

---

# Interview Questions

## Q1. Which command accepts user input?

Answer:

```
read
```

## Q2. What does $1 represent?

First command-line argument.

## Q3. What is the purpose of an if statement?

To execute commands based on a condition.

## Q4. When should you use a for loop?

When the number of iterations is known.

## Q5. Does Bash support do-while loops?

No. Bash uses while and until loops to achieve similar functionality.

## Q6. What is the difference between while and until?

* while runs while the condition is true.
* until runs until the condition becomes true.

## Q7. What does $? represent?

The exit status of the previously executed command.
