# Bash Scripting Cheat Sheet

## Basic Script Structure
```bash
#!/bin/bash
# This is a comment
echo "Hello World"
```

## Variables
```bash
# Declaring variables (no spaces around =)
name="John"
age=25
readonly CONSTANT="unchangeable"

# Using variables
echo "Name: $name"
echo "Age: ${age}"

# Command substitution
current_date=$(date)
files_count=$(ls | wc -l)

# Arithmetic operations
result=$((5 + 3))
((result++))
```

## String Operations
```bash
str="Hello World"

# Length
length=${#str}

# Substring (position 1, length 5)
sub=${str:1:5}

# Replace
new_str=${str/World/Bash}

# Convert to uppercase/lowercase
upper=${str^^}
lower=${str,,}
```

## Arrays
```bash
# Declaring arrays
fruits=("apple" "banana" "orange")
numbers=(1 2 3 4 5)

# Accessing elements
echo "${fruits[0]}"      # First element
echo "${fruits[-1]}"     # Last element
echo "${fruits[@]}"      # All elements
echo "${#fruits[@]}"     # Array length

# Adding elements
fruits+=("grape")

# Slicing
echo "${fruits[@]:1:2}"  # Elements from index 1, length 2
```

## Control Structures

### If-Else
```bash
if [ "$age" -gt 18 ]; then
    echo "Adult"
elif [ "$age" -eq 18 ]; then
    echo "Just turned adult"
else
    echo "Minor"
fi

# Advanced conditions
if [[ "$str" == *"Hello"* && "$age" -gt 20 ]]; then
    echo "String contains Hello and age > 20"
fi
```

### Loops

#### For Loop
```bash
# Traditional for
for i in {1..5}; do
    echo "$i"
done

# C-style for
for ((i=0; i<5; i++)); do
    echo "$i"
done

# Iterating over array
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done

# Iterating over files
for file in *.txt; do
    echo "Processing $file"
done
```

#### While Loop
```bash
count=0
while [ $count -lt 5 ]; do
    echo "$count"
    ((count++))
done

# Reading file line by line
while IFS= read -r line; do
    echo "$line"
done < "file.txt"
```

## Functions
```bash
# Basic function
greeting() {
    echo "Hello, $1!"
    return 0
}

# Function with local variables
calculate() {
    local num1=$1
    local num2=$2
    echo $((num1 + num2))
}

# Calling functions
greeting "John"
result=$(calculate 5 3)
```

## Input/Output
```bash
# Reading user input
echo "Enter name:"
read name

# Reading with prompt
read -p "Enter age: " age

# Reading secret input
read -s -p "Enter password: " password

# Output redirection
echo "Log entry" >> logfile.txt    # Append
echo "New log" > logfile.txt       # Overwrite
```

## File Operations
```bash
# Check if file exists
if [ -f "file.txt" ]; then
    echo "File exists"
fi

# Check if directory exists
if [ -d "dir" ]; then
    echo "Directory exists"
fi

# File permissions
if [ -r "file.txt" ]; then    # Readable
    echo "File is readable"
fi
if [ -w "file.txt" ]; then    # Writable
    echo "File is writable"
fi
if [ -x "file.txt" ]; then    # Executable
    echo "File is executable"
fi
```

## Common Text Processing Commands
```bash
# grep: Search for pattern
grep "pattern" file.txt
grep -i "pattern" file.txt    # Case-insensitive
grep -r "pattern" .           # Recursive search

# sed: Stream editor
sed 's/old/new/' file.txt     # Replace first occurrence
sed 's/old/new/g' file.txt    # Replace all occurrences

# awk: Pattern scanning
awk '{print $1}' file.txt     # Print first column
awk -F',' '{print $2}' file.txt  # Use comma as delimiter
```

## Error Handling
```bash
# Exit on error
set -e

# Error handling with trap
trap 'echo "Error on line $LINENO"' ERR

# Custom error function
error_handler() {
    echo "Error: $1"
    exit 1
}

# Using error handler
if [ "$age" -lt 0 ]; then
    error_handler "Age cannot be negative"
fi
```

## Useful Tips

### Debug Mode
```bash
# Enable debug mode
set -x

# Disable debug mode
set +x
```

### Default Values
```bash
# Use default if variable is unset
name=${name:-"Unknown"}

# Set default if variable is unset
name=${name:="Unknown"}
```

### Special Variables
```bash
$0      # Script name
$1-$9   # First 9 arguments
$#      # Number of arguments
$@      # All arguments as separate words
$*      # All arguments as a single word
$?      # Last command exit status
$$      # Current script process ID
$!      # Last background process ID
```

## Best Practices
1. Always use shebang line: `#!/bin/bash`
2. Use meaningful variable names
3. Quote variables to prevent word splitting
4. Use `set -e` to exit on error
5. Use `set -u` to exit on undefined variables
6. Comment your code
7. Use functions for reusable code
8. Use meaningful exit codes
9. Validate input parameters
10. Use shellcheck for script validation

## Common Exit Codes
```bash
0       # Success
1       # General error
2       # Misuse of shell builtin
126     # Command invoked cannot execute
127     # Command not found
128     # Invalid argument to exit
128+n   # Fatal error signal "n"
130     # Script terminated by Control-C
```
