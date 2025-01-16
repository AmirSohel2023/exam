(https://drive.google.com/drive/folders/1xV_RTTDR8p4pUiWQUJanDxQWjU_wTqdy)

1.a) #!/bin/bash
# This script generates a mark sheet for a student with total marks, average marks, and grade.

# Input marks for three subjects
echo "Enter marks for three subjects (out of 100):"
echo -n "Subject 1: "
read subject1
echo -n "Subject 2: "
read subject2
echo -n "Subject 3: "
read subject3

# Calculate total and average
total=$((subject1 + subject2 + subject3))
average=$(echo "scale=2; $total / 3" | bc)

# Determine grade based on the average marks
if [ $average -ge 90 ]; then
    grade="O Outstanding"
elif [ $average -ge 80 ]; then
    grade="E Excellent"
elif [ $average -ge 70 ]; then
    grade="A Very Good"
elif [ $average -ge 60 ]; then
    grade="B Good"
elif [ $average -ge 50 ]; then
    grade="C Fair"
elif [ $average -ge 40 ]; then
    grade="D Below Average"
else
    grade="F Failed"
fi

# Display the results
echo -e "\nMark Sheet:"
echo "------------------------"
echo "Subject 1: $subject1"
echo "Subject 2: $subject2"
echo "Subject 3: $subject3"
echo "Total Marks: $total"
echo "Average Marks: $average"
echo "Grade: $grade"


2.a) #!/bin/bash
# This script checks whether the input number is prime or not.

# Input number
echo "Enter a number: "
read num

# Check if the number is less than or equal to 1
if [ $num -le 1 ]; then
    echo "$num is not a prime number."
    exit 0
fi

# Loop to check for factors
for ((i=2; i<=$((num/2)); i++)); do
    if [ $((num%i)) -eq 0 ]; then
        echo "$num is not a prime number."
        exit 0
    fi
done

# If no factors are found, it's a prime number
echo "$num is a prime number."

2.b) #include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

void createZombie() {
    pid_t pid = fork();

    if (pid == 0) {  // Child Process
        // Child sleeps for 10 seconds to simulate a long-running process
        printf("Child (Zombie) is sleeping...\n");
        sleep(10); // Simulate child process doing some work
        printf("Child process exiting...\n");
        exit(0);  // Child process exits
    }
    else if (pid > 0) {  // Parent Process
        // Parent does not wait for the child to exit
        printf("Parent process sleeping for 5 seconds before exiting...\n");
        sleep(5);  // Parent process sleeps, child becomes Zombie
        printf("Parent process exiting...\n");
    }
    else {
        perror("Fork failed");
        exit(1);
    }
}

void createOrphan() {
    pid_t pid = fork();

    if (pid == 0) {  // Child Process
        printf("Child (Orphan) is sleeping...\n");
        sleep(10);  // Simulate child process doing some work
        printf("Child process exiting...\n");
        exit(0);  // Child process exits
    }
    else if (pid > 0) {  // Parent Process
        // Parent exits immediately, making the child an orphan
        printf("Parent process exiting immediately...\n");
        exit(0);
    }
    else {
        perror("Fork failed");
        exit(1);
    }
}

int main() {
    int choice;

    printf("Choose the type of process to create:\n");
    printf("1. Create Zombie Process\n");
    printf("2. Create Orphan Process\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);

    if (choice == 1) {
        createZombie();  // Call to create Zombie process
    } else if (choice == 2) {
        createOrphan();  // Call to create Orphan process
    } else {
        printf("Invalid choice. Exiting.\n");
        exit(1);
    }

    return 0;
}

3.a) #!/bin/bash
# This script lists all files with the .sh extension in the current directory

echo "Listing all .sh files in the current directory:"
# Use the 'ls' command with a wildcard pattern to find files ending with .sh
ls *.sh

3.a) #!/bin/bash
# This script lists all files with the .sh extension in the current directory

echo "Listing all .sh files in the current directory:"
# Use the 'ls' command with a wildcard pattern to find files ending with .sh
ls *.sh

4.a) #include <stdio.h>

typedef struct {
    int pid; // Process ID
    int burst_time; // Burst time for the process
    int waiting_time; // Waiting time for the process
    int turnaround_time; // Turnaround time for the process
} Process;

void findWaitingTime(Process processes[], int n) {
    // Waiting time for the first process is 0
    processes[0].waiting_time = 0;

    // Calculate waiting time for remaining processes
    for (int i = 1; i < n; i++) {
        processes[i].waiting_time = processes[i - 1].burst_time + processes[i - 1].waiting_time;
    }
}

void findTurnaroundTime(Process processes[], int n) {
    // Turnaround time = Burst time + Waiting time
    for (int i = 0; i < n; i++) {
        processes[i].turnaround_time = processes[i].burst_time + processes[i].waiting_time;
    }
}

void findAverageTime(Process processes[], int n) {
    int total_waiting_time = 0, total_turnaround_time = 0;

    // Calculate total waiting time and total turnaround time
    for (int i = 0; i < n; i++) {
        total_waiting_time += processes[i].waiting_time;
        total_turnaround_time += processes[i].turnaround_time;
    }

    // Calculate average waiting time and average turnaround time
    float avg_waiting_time = (float)total_waiting_time / n;
    float avg_turnaround_time = (float)total_turnaround_time / n;

    // Print the results
    printf("Average Waiting Time: %.2f\n", avg_waiting_time);
    printf("Average Turnaround Time: %.2f\n", avg_turnaround_time);
}

void printProcessDetails(Process processes[], int n) {
    printf("\nProcess ID\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (int i = 0; i < n; i++) {
        printf("%d\t\t%d\t\t%d\t\t%d\n", processes[i].pid, processes[i].burst_time, processes[i].waiting_time, processes[i].turnaround_time);
    }
}

int main() {
    int n;
    
    // Read the number of processes
    printf("Enter the number of processes: ");
    scanf("%d", &n);

    Process processes[n];

    // Read process details (PID and Burst Time)
    for (int i = 0; i < n; i++) {
        processes[i].pid = i + 1; // Assigning process IDs (1-based)
        printf("Enter burst time for Process %d: ", processes[i].pid);
        scanf("%d", &processes[i].burst_time);
    }

    // Calculate waiting times and turnaround times
    findWaitingTime(processes, n);
    findTurnaroundTime(processes, n);

    // Print process details and calculate averages
    printProcessDetails(processes, n);
    findAverageTime(processes, n);

    return 0;
}

4.b) #!/bin/bash

# Display "Hello World" message
echo "Hello World"


5) #include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>
#include <sys/types.h>

void child_process() {
    while (1) {
        // Display "Welcome message" every 50 milliseconds
        printf("Welcome message\n");
        usleep(50000);  // Sleep for 50 milliseconds (50000 microseconds)
    }
}

int main() {
    pid_t pid;

    // Create a child process using fork
    pid = fork();

    if (pid < 0) {
        // Fork failed
        perror("Fork failed");
        exit(EXIT_FAILURE);
    }

    if (pid == 0) {
        // This is the child process
        child_process();
    } else {
        // This is the parent process
        sleep(1);  // Sleep for 1 second before killing the child process
        kill(pid, SIGKILL);  // Kill the child process after 1 second
        printf("Child process killed by parent after 1 second\n");
    }

    return 0;
}


6.a) (i) pwd (Print Working Directory)
Description: The pwd command prints the current working directory in which you are located in the file system.
Example:
bash
Copy code
$ pwd
/home/user/Documents
This means the current working directory is /home/user/Documents.
(ii) ls (List)
Description: The ls command is used to list files and directories in the current directory. It can also accept various options to modify the output.

Basic Example:

bash
Copy code
$ ls
file1.txt  file2.txt  folder1  folder2
This will list all files and directories in the current directory.

With -l option (long listing format):

bash
Copy code
$ ls -l
total 8
-rw-r--r-- 1 user user  234 Jan 16 10:12 file1.txt
-rw-r--r-- 1 user user  123 Jan 16 10:15 file2.txt
drwxr-xr-x 2 user user 4096 Jan 16 10:20 folder1
drwxr-xr-x 2 user user 4096 Jan 16 10:25 folder2
This shows detailed information like file permissions, ownership, size, and modification date.

(iii) date (Display Date and Time)
Description: The date command is used to display the current date and time. It can also be used to format the output or set the date.

Example:

bash
Copy code
$ date
Mon Jan 16 10:45:00 PST 2025
This will print the current date and time in the default format.

With custom format:

bash
Copy code
$ date "+%Y-%m-%d %H:%M:%S"
2025-01-16 10:45:00
The "+%Y-%m-%d %H:%M:%S" format specifies year-month-day hours:minutes:seconds.

(iv) echo (Print Text to Screen)
Description: The echo command is used to display a line of text or variables to the screen.

Example:

bash
Copy code
$ echo "Hello, World!"
Hello, World!
This simply prints "Hello, World!" to the terminal.

With variable:

bash
Copy code
$ name="John"
$ echo "Hello, $name!"
Hello, John!
This demonstrates printing a variable ($name) within a string.


6.b) #include <stdio.h>
#include <unistd.h>

int main() {
    // Get the UID, PID, and PPID of the current process
    uid_t uid = getuid();        // Get the user ID
    pid_t pid = getpid();        // Get the process ID
    pid_t ppid = getppid();      // Get the parent process ID

    // Display the results
    printf("User ID (UID): %d\n", uid);
    printf("Process ID (PID): %d\n", pid);
    printf("Parent Process ID (PPID): %d\n", ppid);

    return 0;
}

7.a) #!/bin/bash

# Function to display the menu
display_menu() {
    echo "Menu-driven Calculator"
    echo "1. Addition"
    echo "2. Subtraction"
    echo "3. Multiplication"
    echo "4. Division"
    echo "5. Modulus"
    echo "6. Exit"
}

# Function to perform the chosen operation
perform_operation() {
    case $choice in
        1)
            echo "Enter two numbers:"
            read num1 num2
            result=$((num1 + num2))
            echo "Result: $num1 + $num2 = $result"
            ;;
        2)
            echo "Enter two numbers:"
            read num1 num2
            result=$((num1 - num2))
            echo "Result: $num1 - $num2 = $result"
            ;;
        3)
            echo "Enter two numbers:"
            read num1 num2
            result=$((num1 * num2))
            echo "Result: $num1 * $num2 = $result"
            ;;
        4)
            echo "Enter two numbers:"
            read num1 num2
            if [ $num2 -eq 0 ]; then
                echo "Error: Division by zero is not allowed!"
            else
                result=$((num1 / num2))
                echo "Result: $num1 / $num2 = $result"
            fi
            ;;
        5)
            echo "Enter two numbers:"
            read num1 num2
            result=$((num1 % num2))
            echo "Result: $num1 % $num2 = $result"
            ;;
        6)
            echo "Exiting the calculator."
            exit 0
            ;;
        *)
            echo "Invalid choice, please try again."
            ;;
    esac
}

# Main program
while true
do
    display_menu
    echo "Enter your choice (1-6):"
    read choice
    perform_operation
    echo
done


7.b) #!/bin/bash

# Read the number of terms
echo "Enter the number of terms for Fibonacci series: "
read n

# Initialize the first two terms of the Fibonacci series
a=0
b=1

echo "Fibonacci series up to $n terms:"

# Print the Fibonacci series
for ((i=0; i<n; i++))
do
    # Print the current term
    echo -n "$a "

    # Calculate the next term in the Fibonacci series
    next=$((a + b))
    a=$b
    b=$next
done

echo


8) #include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void send_signal(pid_t pid, int signal) {
    if (kill(pid, signal) == -1) {
        perror("Error sending signal");
        exit(EXIT_FAILURE);
    }
    printf("Signal %d sent to process %d\n", signal, pid);
}

int main() {
    pid_t pid;
    int choice;

    // Get the PID of the process to send signals to
    printf("Enter the PID of the target process: ");
    scanf("%d", &pid);

    printf("Choose a signal to send to process %d:\n", pid);
    printf("1. SIGSTOP (Stop the process)\n");
    printf("2. SIGCONT (Continue the process)\n");
    printf("3. SIGKILL (Kill the process)\n");
    printf("Enter your choice (1-3): ");
    scanf("%d", &choice);

    switch (choice) {
        case 1:
            send_signal(pid, SIGSTOP);  // Stop the process
            break;
        case 2:
            send_signal(pid, SIGCONT);  // Continue the process
            break;
        case 3:
            send_signal(pid, SIGKILL);  // Kill the process
            break;
        default:
            printf("Invalid choice\n");
            exit(EXIT_FAILURE);
    }

    return 0;
}


9.a) #!/bin/bash

# Read the number of elements
echo "Enter the number of elements: "
read n

# Read the elements into an array
echo "Enter the elements: "
for ((i=0; i<n; i++))
do
    read arr[$i]
done

# Bubble sort algorithm to sort the array in ascending order
for ((i=0; i<n-1; i++))
do
    for ((j=0; j<n-i-1; j++))
    do
        if [ ${arr[$j]} -gt ${arr[$((j+1))]} ]; then
            # Swap elements if they are in the wrong order
            temp=${arr[$j]}
            arr[$j]=${arr[$((j+1))]}
            arr[$((j+1))]=$temp
        fi
    done
done

# Print the sorted array
echo "Sorted elements in ascending order: "
for ((i=0; i<n; i++))
do
    echo -n "${arr[$i]} "
done
echo



9.b) #!/bin/bash

# Read the value of n
echo "Enter a number (n): "
read n

sum=0
equation=""

# Loop to calculate the sum and form the equation string
for ((i=1; i<=n; i++))
do
    sum=$((sum + i))
    if [ $i -eq $n ]; then
        equation+="$i"
    else
        equation+="$i+"
    fi
done

# Display the equation and sum
echo "$equation=$sum"


10) #include <stdio.h>
#include <stdlib.h>

typedef struct {
    int process_id;
    int arrival_time;
    int burst_time;
    int waiting_time;
    int turnaround_time;
} Process;

// Function to compare two processes based on burst time (used for sorting)
int compare(const void *a, const void *b) {
    return ((Process *)a)->burst_time - ((Process *)b)->burst_time;
}

void findWaitingTime(Process proc[], int n) {
    proc[0].waiting_time = 0; // First process has no waiting time

    for (int i = 1; i < n; i++) {
        proc[i].waiting_time = proc[i-1].waiting_time + proc[i-1].burst_time;
    }
}

void findTurnaroundTime(Process proc[], int n) {
    for (int i = 0; i < n; i++) {
        proc[i].turnaround_time = proc[i].burst_time + proc[i].waiting_time;
    }
}

void findAverageTime(Process proc[], int n) {
    int total_waiting_time = 0;
    int total_turnaround_time = 0;

    for (int i = 0; i < n; i++) {
        total_waiting_time += proc[i].waiting_time;
        total_turnaround_time += proc[i].turnaround_time;
    }

    printf("Average Waiting Time = %.2f\n", (float)total_waiting_time / n);
    printf("Average Turnaround Time = %.2f\n", (float)total_turnaround_time / n);
}

void sjfScheduling(Process proc[], int n) {
    // Sort processes by burst time
    qsort(proc, n, sizeof(Process), compare);

    findWaitingTime(proc, n);
    findTurnaroundTime(proc, n);

    printf("Process ID\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (int i = 0; i < n; i++) {
        printf("%d\t\t%d\t\t%d\t\t%d\n", proc[i].process_id, proc[i].burst_time, proc[i].waiting_time, proc[i].turnaround_time);
    }

    findAverageTime(proc, n);
}

int main() {
    int n;

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    Process proc[n];

    for (int i = 0; i < n; i++) {
        proc[i].process_id = i + 1;
        printf("Enter arrival time and burst time for process %d: ", i + 1);
        scanf("%d %d", &proc[i].arrival_time, &proc[i].burst_time);
    }

    sjfScheduling(proc, n);

    return 0;
}

11.a) #!/bin/bash

# Function to check if a number is prime
is_prime() {
    num=$1
    if [ $num -le 1 ]; then
        return 1  # Not prime if less than or equal to 1
    fi
    for ((i = 2; i * i <= num; i++)); do
        if [ $((num % i)) -eq 0 ]; then
            return 1  # Not prime if divisible by i
        fi
    done
    return 0  # Prime if no divisors found
}

# Display all prime numbers from 1 to 100
echo "Prime numbers from 1 to 100:"
for num in {1..100}; do
    if is_prime $num; then
        echo $num
    fi
done

11.b) #include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    printf("Before execl() call.\n");

    // Use execl() to execute the 'ls' command
    // Here, we're listing the files in the current directory (using 'ls -l')
    if (execl("/bin/ls", "ls", "-l", (char *)NULL) == -1) {
        perror("execl() failed");
        exit(1);
    }

    // This line will not be reached if execl() is successful
    printf("After execl() call.\n");

    return 0;
}


12) #include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>

int main() {
    int pipefd[2];  // Array to store the file descriptors for the pipe
    pid_t pid;
    char write_msg[] = "Hello from the parent process!";
    char read_buffer[100];

    // Create the pipe
    if (pipe(pipefd) == -1) {
        perror("Pipe creation failed");
        exit(1);
    }

    // Create a child process
    pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    }

    if (pid == 0) {  // Child process
        // Close the write end of the pipe, as the child will only read
        close(pipefd[1]);

        // Read data from the pipe
        read(pipefd[0], read_buffer, sizeof(read_buffer));
        printf("Child Process: Received message - '%s'\n", read_buffer);

        // Close the read end of the pipe
        close(pipefd[0]);
    } else {  // Parent process
        // Close the read end of the pipe, as the parent will only write
        close(pipefd[0]);

        // Write data into the pipe
        write(pipefd[1], write_msg, strlen(write_msg) + 1);

        // Close the write end of the pipe
        close(pipefd[1]);

        // Wait for the child process to finish
        wait(NULL);
    }

    return 0;
}

13.a) #!/bin/bash

# Show all files with the .sh extension in the current directory and subdirectories
echo "Listing all .sh files in the current directory and subdirectories:"
find . -type f -name "*.sh"


13.b) #include <stdio.h>
#include <unistd.h>

int main() {
    // Get the process ID (PID) of the current process
    pid_t pid = getpid();

    // Get the parent process ID (PPID) of the current process
    pid_t ppid = getppid();

    // Get the user ID (UID) of the current process
    uid_t uid = getuid();

    // Display the results
    printf("UID of the current process: %d\n", uid);
    printf("PID of the current process: %d\n", pid);
    printf("PPID of the current process: %d\n", ppid);

    return 0;
}

14.a) #include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

void create_zombie_process() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    }

    if (pid == 0) {  // Child process
        printf("Child process (Zombie) is running with PID: %d\n", getpid());
        // Simulating child process termination without being reaped by the parent
        exit(0);  // Child exits, but remains in the process table as a zombie
    } else {  // Parent process
        // Parent sleeps to let the child become a zombie
        sleep(10);  // Parent sleeps for 10 seconds to let the child become a zombie
        printf("Parent process has completed, but child is still a zombie.\n");
    }
}

void create_orphan_process() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    }

    if (pid == 0) {  // Child process
        // Simulating child process running after the parent terminates
        sleep(2);  // Sleep to allow the parent to terminate before it
        printf("Child process (Orphan) is running with PID: %d\n", getpid());
    } else {  // Parent process
        printf("Parent process is terminating.\n");
        exit(0);  // Parent terminates before the child, making the child an orphan
    }
}

int main() {
    printf("Demonstrating Zombie Process:\n");
    create_zombie_process();
    printf("\n\nDemonstrating Orphan Process:\n");
    create_orphan_process();

    return 0;
}


14.b) #!/bin/bash

# Prompt the user to enter an integer number
echo -n "Enter an integer: "
read number

# Get the absolute value of the number (to handle negative numbers)
abs_number=${number#-}

# Calculate the number of digits using the length of the string
digit_count=${#abs_number}

# Display the result
echo "The number of digits in $number is $digit_count."


15) #include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main() {
    int pipefd[2]; // Array to hold pipe file descriptors
    pid_t pid;     // Process ID for fork
    char write_msg1[] = "Message 1 from parent.";
    char write_msg2[] = "Message 2 from parent.";
    char read_buffer[100];

    // Create the pipe
    if (pipe(pipefd) == -1) {
        perror("Pipe creation failed");
        exit(EXIT_FAILURE);
    }

    // Create a child process
    pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(EXIT_FAILURE);
    }

    if (pid == 0) { // Child process
        // Close the write end of the pipe
        close(pipefd[1]);

        // Read the first message
        read(pipefd[0], read_buffer, sizeof(read_buffer));
        printf("Child Process: Received '%s'\n", read_buffer);

        // Read the second message
        read(pipefd[0], read_buffer, sizeof(read_buffer));
        printf("Child Process: Received '%s'\n", read_buffer);

        // Close the read end of the pipe
        close(pipefd[0]);
    } else { // Parent process
        // Close the read end of the pipe
        close(pipefd[0]);

        // Write the first message
        write(pipefd[1], write_msg1, strlen(write_msg1) + 1);

        // Write the second message
        write(pipefd[1], write_msg2, strlen(write_msg2) + 1);

        // Close the write end of the pipe
        close(pipefd[1]);

        // Wait for the child process to finish
        wait(NULL);
    }

    return 0;
}


16.a) #!/bin/bash

# Prompt the user to enter a string
echo -n "Enter a string: "
read input

# Reverse the string
reversed=$(echo "$input" | rev)

# Check if the original string and reversed string are the same
if [[ "$input" == "$reversed" ]]; then
    echo "The string '$input' is a palindrome."
else
    echo "The string '$input' is not a palindrome."
fi

16.b) 

#!/bin/bash

# Define the value of n
n=5

# Initialize sum and string to build the equation
sum=0
equation=""

# Loop through numbers from 1 to n
for ((i=1; i<=n; i++)); do
    sum=$((sum + i)) # Add the current number to sum
    equation+="$i"   # Append the current number to the equation

    # Add "+" to the equation except for the last number
    if [[ $i -lt $n ]]; then
        equation+="+"
    fi
done

# Append the result to the equation
equation+="=$sum"

# Print the final equation
echo "$equation"

17.a) #!/bin/bash

# Function to calculate the power of a number
power() {
    local base=$1
    local exponent=$2
    local result=1

    for ((i = 1; i <= exponent; i++)); do
        result=$((result * base))
    done

    echo $result
}

# Function to check if a number is Armstrong
is_armstrong() {
    local num=$1
    local sum=0
    local temp=$num
    local n=${#num} # Number of digits

    while ((temp > 0)); do
        digit=$((temp % 10)) # Extract the last digit
        sum=$((sum + $(power $digit $n))) # Add the power of the digit to the sum
        temp=$((temp / 10)) # Remove the last digit
    done

    if ((sum == num)); then
        echo "Yes, $num is an Armstrong number."
    else
        echo "No, $num is not an Armstrong number."
    fi
}

# Main script
echo "Enter a number:"
read number

# Validate input
if [[ ! $number =~ ^[0-9]+$ ]]; then
    echo "Invalid input! Please enter a positive integer."
    exit 1
fi

# Check if the number is Armstrong
is_armstrong $number



17.b) #include <stdio.h>
#include <unistd.h>

int main() {
    // Get the User ID
    uid_t uid = getuid();

    // Get the Process ID
    pid_t pid = getpid();

    // Get the Parent Process ID
    pid_t ppid = getppid();

    // Display the information
    printf("User ID (UID): %d\n", uid);
    printf("Process ID (PID): %d\n", pid);
    printf("Parent Process ID (PPID): %d\n", ppid);

    return 0;
}



18.a) #include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid;

    printf("Parent process started. PID: %d\n", getpid());

    // Fork a child process
    pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    } else if (pid == 0) {
        // Child process
        printf("Child process started. PID: %d, Parent PID: %d\n", getpid(), getppid());

        // Sleep for 2 seconds to simulate work
        sleep(2);

        printf("Child process terminating. PID: %d\n", getpid());
        exit(0);
    } else {
        // Parent process
        printf("Parent process sleeping for 5 seconds...\n");

        // Sleep for 5 seconds to let the child terminate (Zombie Process)
        sleep(5);

        // Check for zombie process
        printf("Parent process checking for zombie. PID: %d\n", getpid());
        wait(NULL); // Collect child's exit status

        // Create orphan process by terminating parent
        pid_t orphan_pid = fork();

        if (orphan_pid == 0) {
            // New child process to demonstrate orphan
            printf("Orphan process started. PID: %d, Parent PID: %d\n", getpid(), getppid());

            // Sleep to allow the parent to terminate
            sleep(10);
            printf("Orphan process still running. PID: %d, New Parent PID (init/systemd): %d\n", getpid(), getppid());
            exit(0);
        }

        printf("Parent process terminating. PID: %d\n", getpid());
    }

    return 0;
}



18.b) #!/bin/bash

# Function to sort an array
bubble_sort() {
    local -n arr=$1 # Pass the array by reference
    local n=${#arr[@]} # Number of elements in the array

    for ((i = 0; i < n-1; i++)); do
        for ((j = 0; j < n-i-1; j++)); do
            if ((arr[j] > arr[j+1])); then
                # Swap arr[j] and arr[j+1]
                temp=${arr[j]}
                arr[j]=${arr[j+1]}
                arr[j+1]=$temp
            fi
        done
    done
}

# Main script
echo "Enter the number of elements:"
read n

echo "Enter $n numbers (separated by spaces):"
read -a numbers

echo "Sorting the numbers..."
bubble_sort numbers

echo "Sorted numbers:"
echo "${numbers[@]}"




19.a) #!/bin/bash

# Function to sort an array
bubble_sort() {
    local -n arr=$1 # Pass the array by reference
    local n=${#arr[@]} # Number of elements in the array

    for ((i = 0; i < n-1; i++)); do
        for ((j = 0; j < n-i-1; j++)); do
            if ((arr[j] > arr[j+1])); then
                # Swap arr[j] and arr[j+1]
                temp=${arr[j]}
                arr[j]=${arr[j+1]}
                arr[j+1]=$temp
            fi
        done
    done
}

# Main script
echo "Enter the number of elements:"
read n

echo "Enter $n numbers (separated by spaces):"
read -a numbers

echo "Sorting the numbers..."
bubble_sort numbers

echo "Sorted numbers:"
echo "${numbers[@]}"


19.b) echo "Enter Size(N)"
read N

i=1
sum=0

echo "Enter Numbers"
while [ $i -le $N ]
do
  read num           #get number
  sum=$((sum + num)) #sum+=num
  i=$((i + 1))
done

echo $sum


3.b) echo "---FIND THE GREATEST AMONG THREE NUMBERS---"
echo "Enter 1st number:"
read first_num
echo "Enter 2nd number:"
read second_num
echo "Enter 3rd number:"
read third_num
if test $first_num -gt $second_num && test $first_num -gt $third_num
then
	echo $first_num is the greatest number.
elif test $second_num -gt $third_num
then
	echo $second_num is the greaatest number.
else
	echo $third_num is the greatest number.
fi


2.a) echo "Enter a number:"
read number
i=2

if [ $number -lt 2 ]
then
    echo "$number is not a prime number."
    exit
fi

while [ $i -lt $number ]
do
    if [ expr $number % $i -eq 0 ]
    then
        echo "$number is not a prime number."
        exit
    fi
    i=expr $i + 1
done

echo "$number is a prime number."


2.a) echo "Enter a number:"
read number
i=2

if [ $number -lt 2 ]
then
    echo "$number is not a prime number."
    exit
fi

while [ $i -lt $number ]
do
    if [ expr $number % $i -eq 0 ]
    then
        echo "$number is not a prime number."
        exit
    fi
    i=expr $i + 1
done

echo "$number is a prime number."



