
# Accessing Jupyter Notebook on RONIN (Ubuntu Machine) from Your Local Desktop

---

## Step 1: Get the IP Addresses

- On the **RONIN Ubuntu machine**, run this command to get its public IP address:

  ```bash
  curl ifconfig.me
  ```

  This will output the **RONIN-IP** (public IP address of the Ubuntu machine).

- On your **local desktop machine**, note your own IP address if needed.

---

## Step 2: Start Jupyter Notebook on the RONIN Machine

1. SSH into your RONIN Ubuntu machine or open its terminal.

2. Activate your Python virtual environment:

   ```bash
   source titan-env/bin/activate
   ```

3. Launch Jupyter Notebook **without a browser**, specifying port 8888:

   ```bash
   jupyter notebook --no-browser --port=8888
   ```

4. You will see output similar to:

   ```
   To access the server, open this file in a browser:
       file:///home/ubuntu/.local/share/jupyter/runtime/jpserver-81375-open.html
   Or copy and paste one of these URLs:
       http://localhost:8888/tree?token=8cb2ac173e82f6e9c89f18d5c1f680112d6fbe020fef1369
       http://127.0.0.1:8888/tree?token=8cb2ac173e82f6e9c89f18d5c1f680112d6fbe020fef1369
   ```

---

## Step 3: Create an SSH Tunnel from Your Local Machine

1. Open a terminal on your local machine in the folder where your SSH private key is stored.

2. Run this command (replace placeholders with your actual paths and IPs):

   ```bash
   ssh -i "path/to/private-key" ubuntu@RONIN-IP -L 8888:localhost:8888
   ```

   This command forwards the remote port 8888 to your local machine's port 8888.

---

## Step 4: Access Jupyter Notebook on Your Local Machine

- Open your web browser and go to:

  ```
  http://localhost:8888/tree
  ```

- When prompted, enter the Jupyter token or password.

- Alternatively, use the URLs printed by the `jupyter notebook` command on the RONIN machine.

---

## Optional: Regenerate SSH Key Pair on RONIN Machine

If you need to generate a new SSH key pair, follow these steps:

1. On the RONIN machine, run:

   ```bash
   cd ~/.ssh/
   ssh-keygen -t rsa -b 2048 -m PEM -f titan-key
   ```

2. Add the new public key to authorized keys:

   ```bash
   cat ~/.ssh/titan-key.pub >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   chmod 700 ~/.ssh
   ```

3. Copy the private key contents to your clipboard:

   ```bash
   cat ~/.ssh/titan-key | xsel --clipboard --input
   ```

4. Paste the private key into a file on your local machine (e.g., in VSCode).

5. Try connecting again using the new private key.

---

# Notes

- Replace `RONIN-IP` with the actual IP address of your Ubuntu machine.
- Replace `"path/to/private-key"` with the actual path to your SSH private key.
- Make sure ports are open and not blocked by firewalls.
