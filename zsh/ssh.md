# SSH command (Linux)
*This doc assume ssh private key is name: id_ed25519*

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

You'll need to authenticate this action using your password, which is the SSH key passphrase you created earlier. See "Working with SSH key passphrases."

## Testing your SSH connection

After you've set up your SSH key and added it to remote machine, you can test your connection.

```bash
ssh -T user@hostname
```

## About passphrases for SSH keys
With SSH keys, if someone gains access to your computer, the attacker can gain access to every system that uses that key. To add an extra layer of security, you can add a passphrase to your SSH key. To avoid entering the passphrase every time you connect, you can securely save your passphrase in the SSH agent.

### Adding or changing a passphrase

```bash
$ ssh-keygen -p -f ~/.ssh/id_ed25519
```