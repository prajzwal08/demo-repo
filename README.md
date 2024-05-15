# How to Linux 668

## Initial Setup

1. **Get an Account**: Contact Bas for account creation at [v.retsios@utwente.nl](mailto:v.retsios@utwente.nl). Room no 1150.


## Connection Instructions

- **For Windows Users**:
  - Utilize Windows Subsystem for Linux (WSL).
  - In the command line:
    ```
    ssh username@linux668.itc.utwente.nl
    ```
    Replace `username` with your actual username.
  - Use your university login credentials for password.

- Default home directory: `/home/username`.

## Software Setup

- **Python**:
  - Python 3.8 is pre-installed.

- **Anaconda Installation**:
  1. Download Anaconda from [Anaconda's website](https://www.anaconda.com/download/success).
  2. Select Linux and copy the download link, e.g., `https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh`.
  3. In the Linux terminal:
     ```
     mkdir tmp
     cd tmp
     wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh
     ```
  4. Verify the `.sh` file has been downloaded.
     ```
     ls -la
     ```
  5. Install Anaconda:
     ```
     bash Anaconda3-2024.02-1-Linux-x86_64.sh
     ```
     Accept the License Agreement and add Anaconda to PATH.

  7. Source `.bashrc`:
     ```
     cd ~
     source .bashrc
     ```

This should complete the setup for connecting to and using Linux 668.

If you prefer using Jupyter Notebook, please note that Linux does not have a browser, so we need to forward the port.

```bash
jupyter --version
```

If Jupyter Notebook is not installed, you can install it using pip:

```bash
pip install jupyterlab
```

Then, start Jupyter Notebook with the following command, specifying a port number (XXXX) of your choice:

```bash
jupyter lab --no-browser --port=XXXX
```

Choose an available port, typically 8888, 8889, or a similar one.

Next, in your local terminal, use SSH to forward the port from the remote Linux machine to your local machine:

```bash
ssh -N -f -L localhost:YYYY:localhost:XXXX remoteuser@remotehost
```

Replace:
- YYYY: Port number on your local terminal.
- XXXX: Port number on the remote Linux terminal.
- remoteuser@remotehost: Your login credentials for the remote Linux machine.

You will be prompted to enter your password.

Now, in your browser, type the following URL:

```
localhost:YYYY
```

I prefer using VS Code, and you can connect using the following steps:

1. First, ensure that you have VS Code installed on your local machine.

2. Follow the instructions provided in the link [here](https://carleton.ca/scs/2023/vscode-remote-access-and-code-editing/) to set up remote access and code editing using VS Code.

3. Once set up, you can connect to your remote server using VS Code by following the instructions outlined in the link.

4. This method allows you to seamlessly edit code and access files on your remote server directly from your local VS Code environment.



Replace YYYY with the port number you chose for your local machine. This should open Jupyter Lab in your browser, allowing you to work with notebooks remotely.


## Runnning code incessantly 
To run code incessantly in Linux, you have two main options: `nohup` and `screen`. Unfortunately, `tmux` is not available, but both `nohup` and `screen` are powerful alternatives.

### Using nohup

```bash
nohup your_command &
```

Replace `your_command` with the command you want to run. The `&` at the end runs the command in the background.

```bash
nohup python script.py &
```

### Using screen

- **Start a new screen session**: 

```bash
screen
```

- **Detach from a screen session**: Press `Ctrl + A` followed by `Ctrl + D`.

- **List active screen sessions**: 

```bash
screen -ls
```

- **Reattach to a screen session**: 

```bash
screen -r session_id
```

Replace `session_id` with the ID of the screen session you want to reattach to.

- **Example**: Running a long-running process within a screen session:

```bash
screen
```

Run your command, e.g., `python script.py`.

- **Detach from the screen session**: Press `Ctrl + A` followed by `Ctrl + D`.

- **Reattach to the screen session later**: 

```bash
screen -r
```

Both `nohup` and `screen` are valuable tools for running processes incessantly in Linux, offering flexibility and convenience in managing long-running tasks.

## Automation, keep track of logs and chill when it runs. 

This bash script, `runcode.sh`, automates the execution of a Python script `comparemodelninsitu.py` and manages logging and email notifications. Below is a breakdown of its functionality:

```bash
#!/bin/bash

# Define your email address
email_address="p.khanal@utwente.nl"

# Define the location of the log file
log_file="/home/khanalp/logs/analysemodeloutput.log"

# Change working directory to your desired directory
cd /home/khanalp/code/PhD/pystemmusscopeanalysis

# Add timestamp indicating job start to log file
echo "Job started at $(date)" >> "$log_file"

# Execute the main script using nohup and redirect stdout and stderr to log file, change .py file accordingly.
nohup python3 comparemodelninsitu.py  >> "$log_file" 2>&1 &
pid=$!  # Get the process ID of the background job

# Wait for the job to finish and get its exit status
wait $pid
exit_status=$?

# Add timestamp indicating job end to log file
echo "Job ended at $(date)" >> "$log_file"

# Check the exit status and send email notification accordingly
if [ $exit_status -eq 0 ]; then
    mail -s "Job completed successfully" $email_address < "$log_file"
else
    mail -s "Job failed or was interrupted" $email_address < "$log_file"
fi

```
## what is happening?
This bash script, `runcode.sh`, automates the execution of a Python script `comparemodelninsitu.py` and manages logging and email notifications. Below is a breakdown of its functionality:

1. **Defining Variables**: 
   - `email_address`: Specifies the email address where notifications will be sent.
   - `log_file`: Defines the location of the log file where script output and execution details will be logged.

2. **Changing Working Directory**: 
   - The script changes the working directory to `/home/khanalp/code/PhD/pystemmusscopeanalysis`, ensuring that the subsequent Python script is executed in the correct directory context.

3. **Logging**: 
   - The script appends a timestamp to the log file indicating the start of the job.
   - It uses `nohup` to execute the Python script `comparemodelninsitu.py` in the background, redirecting both stdout and stderr to the log file.
   - A process ID (`pid`) is obtained to monitor the background job.
   - Upon completion of the job, another timestamp is appended to the log file.

4. **Email Notification**:
   - Depending on the exit status of the background job, an appropriate email notification is sent.
   - If the job exits with a status of `0` (success), an email with the subject "Job completed successfully" is sent, attaching the log file.
   - If the job encounters an error or is interrupted, an email with the subject "Job failed or was interrupted" is sent, including the log file for diagnostic purposes.

Logging and email notifications are crucial for several reasons:
- **Troubleshooting**: Log files provide a detailed record of script execution, facilitating troubleshooting in case of errors.
- **Monitoring**: Email notifications inform stakeholders about the status of scheduled tasks, allowing for timely intervention in case of failures.
- **Audit Trail**: Logging ensures an audit trail of script execution, aiding in compliance, debugging, and performance analysis.
- **Communication**: Email notifications serve as a communication channel for conveying important updates and alerts to relevant parties.

## Coming up soon...


### Adding Entire Code When Job is Completed:
After completing the task, remember to include the entire code for reference.

### Email Sending Issue:
Sometimes, the script may not send emails. Investigate and fix any issues related to email sending.

### Timing for Complex Tasks:
For more complex tasks, accurately measure the time needed to read, process, and write the output file to optimize performance.


# to run .sh file, 
To run a `.sh` file in Linux, you can follow these steps:

1. **Navigate to the directory containing the `.sh` file**:
   ```bash
   cd /path/to/your/directory
   ```
2. **Make the .sh file executable (if it's not already):**
   ```bash
   chmod +x your_script.sh
   ```
3. **Run the .sh file**
  ```bash
   ./your_script.sh
  ```
   

# By default, your home folder is read and writable by others, 
If you want to deny permission for others to access your directory (which is a good practice unless you are sharing data), you can use the following one-liner:

```bash
chmod -R o-rwx <directory_name>
```
The directory name would be ur folder in linux668. This  will remove read, write, and execute permissions for others on the specified directory and all of its contents.
