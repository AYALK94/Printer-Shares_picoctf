# Printer Shares - picoCTF 2026

**Category:** General Skills  
**Difficulty:** Easy  

## Description
Oops! Someone accidentally sent an important file to a network printer—can you retrieve it from the print server? The printer is on port `16807`.

---

## Solution Walkthrough

### 1. Enumerate SMB Shares
Although the challenge description mentions a network printer, enumerating the service reveals that it is actually an **SMB (Server Message Block)** file share running on a custom port (`16807`).

Run `smbclient` with the `-L` flag to list available shares anonymously (`-N`):

```bash
smbclient -L //chatelaine.cylabacademy.net -p 16807 -N
```

**Output:**
```text
Sharename       Type      Comment
---------       ----      -------
shares          Disk      Public Share With Guests
IPC$            IPC       IPC Service (Samba 4.19.5-Ubuntu)
SMB1 disabled -- no workgroup available
```

### 2. Connect to the Share and Download the Flag
Connect directly to the `shares` directory using the interactive SMB client, list the contents, and download `flag.txt`:

```bash
smbclient //chatelaine.cylabacademy.net/shares -p 16807 -N
```

**Interactive SMB Session Output:**
```text
Try "help" to get a list of possible commands.
smb: \> ls
  .                                    D         0  Wed Sep 23 07:02:25 2026
  ..                                   D         0  Wed Sep 23 07:02:25 2026
  dummy.txt                            N      1142  Wed Sep 23 00:59:21 2026
  flag.txt                             N        37  Wed Sep 23 07:02:25 2026

        65536 blocks of size 1024. 60000 blocks available
smb: \> get flag.txt
getting file \flag.txt of size 37 as flag.txt (7.2 KiloBytes/sec) (average 7.2 KiloBytes/sec)
smb: \> exit
```

### 3. Read the Flag
After exiting the SMB shell back to your regular Linux terminal, read the contents of the downloaded file to retrieve the flag:

```bash
cat flag.txt