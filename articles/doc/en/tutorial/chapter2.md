---
id: chapter2
title: 'Chapter 2: 🔐🚚 The Complete Guide to Security and Transfer — Safe Backup, Exchange, and Regular Update of Secret Keys 💼'
description: In this chapter, we will explain in detail how to safely manage secret keys.We provide practical guidelines on how to back up keys, securely exchange them, and the restart process in emergencies.
---

Security is at the core of validator operations. In this chapter, we will explain in detail how to safely manage secret keys. We provide practical guidelines on how to back up keys, securely exchange them, and the restart process in emergencies. We also focus on the latest security measures to ensure our readers can continue their validator operations with confidence.

## 🗝️ Creating SSH Keys - Building a Strong Access Foundation

Execute the following command from your local computer where `solv CLI` is installed.
※ If you already have an SSH key, please skip this step.

```bash
solv scp init
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:4fBtHns/xlPO7wHLS37ILhqQ8vUMQfh55dJobUYGyUw ubuntu@c3-large-x86-bue-1
The key's randomart image is:
+---[RSA 4096]----+
|        ..+Eo    |
|       ..  + +   |
|      . o.. O    |
|       +.=.= *   |
|     . oSo* +.   |
|      o oo+o. o .|
|       . .ooo=.= |
|          .oo+*.+|
|         .. o=o++|
+----[SHA256]-----+
```

A key pair for `ssh` connection has been created.

## 🔗 Setting up SSH Connection on Validator Server - Establishing Secure Remote Access

Connect to your validator node's server using `SSH`.

```bash
ssh username@<your-server-ip-address>
```

Switch to the `solv` user and load the settings.

```bash
su solv
cd ~ && source ~/.profile
```

Display the necessary SSH public key for the settings.

```bash
solv scp cat
```

Copy this public key and keep it.

Next, set up the SSH connection with the following command.

```bash
solv scp create
? Enter your SSH Public Key (xxxxxxxpubkeyxxxxxxxx)
```

Paste the SSH public key you copied earlier here.
This completes the connection setup between your local computer and the validator node.

## 📦 Key Backup (Validator Node → Local Computer)

In this step, we will introduce how to back up keys from the `validator node` to your `local computer`. The following four keys located in the `/mt/solana/` directory will be downloaded:

- `/mt/solana/mainnet-validator-keypair.json`
- `/mt/solana/testnet-validator-keypair.json`
- `/mt/solana/vote-account-keypair.json`
- `/mt/solana/authority-keypair.json`

※ If you are uploading keys from your `local computer` to the `validator node`, please skip this step.

Execute the following command from your local computer. Please enter the IP address of your validator node server.

```bash
solv scp backup
? Enter your Ubuntu Server IP (1.1.1.1)
✅ Successfully Generated - ~/solvKeys/download/testnet-validator-keypair.json
✅ Successfully Generated - ~/solvKeys/download/mainnet-validator-keypair.json
✅ Successfully Generated - ~/solvKeys/download/vote-account-keypair.json
✅ Successfully Generated - ~/solvKeys/download/authority-keypair.json
```

The keys have been saved in the `~/solvKeys/download` directory 🎉
It is recommended to keep these keys safe and back them up on a USB disk or similar storage device.

## 🔀 Key Exchange (Local Computer → Validator Node)

Execute the following command from your local computer and enter the IP address of the validator node.

`~/solvKeys/upload`

All keys created in the previous chapters will be uploaded to the

`/mt/solana`

directory.
(Note: It is recommended to always take a backup as existing files with the same name will be overwritten.)

```bash
solv scp upload
? Enter your Ubuntu Server IP (1.1.1.1)
```

## 🔍 Finding Keys

If you are not sure whether you have backed up all the keys, you can use the `solv scp search` command to find them.

```bash
solv scp search
start searching...
 ████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 20% | ETA: 30s | 2062/10000
🔍 Found 4 Potential Solana Key Pairs 🎉

/home/ubuntu/solvKeys/upload/testnet-validator-keypair.json
/home/ubuntu/solvKeys/upload/authority-keypair.json
/home/ubuntu/solvKeys/upload/vote-account-keypair.json
/home/ubuntu/solvKeys/upload/mainnet-validator-keypair.json

Only showing the first 10 results
```

## 🔄 Restarting the Validator Node

Execute the following command inside the validator node to apply the exchanged keys and restart the node.

```bash
solv restart --snapshot
```

By using the `--snapshot` option, you can download a new snapshot.

This completes the key exchange process 🎉

## ⚙️ Regular Updates for Solana Version

Once you have launched a validator, the main tasks as a validator are simply to update the `solana` version and to monitor patiently.

For each `solana` update, solv supports the following commands:

```bash
solv update
solv update --monitor
```

If you see `ready to restart` in the logs, the update has been completed successfully.

If you want to skip monitoring, you can complete the update with:

```bash
solv update -b
```

If you decide to monitor later, you can check with the following command:

```bash
solv monitor
```

In the next chapter, we will introduce how to monitor validator nodes in a serverless environment.
