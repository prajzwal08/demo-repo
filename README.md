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
jupyter notebook --no-browser --port=XXXX
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



