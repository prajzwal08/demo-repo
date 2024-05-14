# How to connect to Linux 668

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
