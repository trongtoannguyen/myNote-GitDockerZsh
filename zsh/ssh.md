# SSH command (Linux)

## Check existing key

```bash
$ ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

## Generating a new SSH key and adding it to the ssh-agent

When you generate an SSH key, you can add a passphrase to further secure the key. Whenever you use the key, you must enter the passphrase. If your key has a passphrase and you don't want to enter the passphrase every time you use the key, you can add your key to the SSH agent. The SSH agent manages your SSH keys and remembers your passphrase.

### Generating a new SSH key

You can generate a new SSH key on your local machine using DSA/RSA.

- **DSA**
    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
    ```

- **RSA**
    ```bash
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```

### Adding your SSH key to the ssh-agent

1. Start the ssh-agent in the background.

    ```bash
    $ eval "$(ssh-agent -s)"
    > Agent pid 59566
    ```

2. Add your SSH private key to the ssh-agent.

    ```bash
    ssh-add ~/.ssh/id_ed25519
    ```