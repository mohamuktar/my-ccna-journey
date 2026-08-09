# Day 04 — Cisco IOS CLI

## 1. Cisco IOS and CLI

### Cisco IOS

**Cisco IOS (Internetwork Operating System)** is the operating system used on many Cisco networking devices, including:

* Routers
* Switches
* Firewalls

> **Important:** Cisco IOS is unrelated to Apple's iOS.

### CLI

**CLI = Command-Line Interface**

The CLI is the primary interface used to configure and manage Cisco devices.

Cisco devices can also have a **GUI (Graphical User Interface)**, but network engineers commonly use the CLI.

---

# 2. Connecting to a Cisco Device

## Console Connection

When configuring a Cisco device for the first time, you typically connect directly to its **console port**.

A Cisco switch may have:

* RJ-45 console port
* USB console port

### RJ-45 Console Connection

An RJ-45 console port uses a **rollover cable**.

**Do NOT confuse rollover and crossover cables.**

| Cable            | Purpose                                               |
| ---------------- | ----------------------------------------------------- |
| Rollover         | Connect computer to Cisco console port                |
| Crossover        | Historically used to connect similar Ethernet devices |
| Straight-through | Historically used for different Ethernet device types |

### Rollover Cable

A rollover cable reverses the pin order:

```text
Pin 1 → Pin 8
Pin 2 → Pin 7
Pin 3 → Pin 6
Pin 4 → Pin 5
Pin 5 → Pin 4
Pin 6 → Pin 3
Pin 7 → Pin 2
Pin 8 → Pin 1
```

Older laptops may require a **USB-to-serial adapter**.

---

# 3. Terminal Emulator

A **terminal emulator** allows you to access the Cisco CLI through a console connection.

Example:

* PuTTY

Typical Cisco console settings:

| Setting           |        Value |
| ----------------- | -----------: |
| Speed / Baud rate | **9600 bps** |
| Data bits         |        **8** |
| Stop bits         |        **1** |
| Parity            |     **None** |
| Flow control      |     **None** |

### Memorize:

**9600 / 8 / 1 / None / None**

---

# 4. Cisco IOS CLI Modes

Cisco IOS has different modes, each providing different levels of access.

## User EXEC Mode

Prompt:

```text
Router>
```

The `>` indicates **User EXEC mode**.

Characteristics:

* First mode after connecting
* Limited access
* Can view some information
* Cannot make major configuration changes

Also called **user mode**.

---

## Privileged EXEC Mode

Enter with:

```text
enable
```

Prompt:

```text
Router#
```

The `#` indicates **Privileged EXEC mode**.

Characteristics:

* Greater access than User EXEC
* Can view the device configuration
* Can restart/reload the device
* Can save configuration
* Used to enter configuration modes
* Not normally used to directly change configuration

### User EXEC → Privileged EXEC

```text
Router> enable
Router#
```

---

## Global Configuration Mode

Enter from Privileged EXEC:

```text
configure terminal
```

Common shortcut:

```text
conf t
```

Prompt:

```text
Router(config)#
```

This is where you make configuration changes to the device.

### Mode progression

```text
Router>
     |
     | enable
     ↓
Router#
     |
     | configure terminal
     ↓
Router(config)#
```

### Easy way to remember

```text
>       User EXEC
#       Privileged EXEC
(config)# Global Configuration
```

---

# 5. IOS Command Shortcuts

Cisco IOS allows commands to be abbreviated as long as the abbreviation is **unambiguous**.

Example:

```text
enable
```

can be shortened to:

```text
en
```

But:

```text
e
```

is ambiguous because multiple commands begin with `e`.

### Useful keys

#### `?`

Displays available commands or possible completions.

Example:

```text
Router# e?
```

May show:

```text
enable
exit
```

#### `Tab`

Automatically completes an unambiguous command.

Example:

```text
Router> en<Tab>
```

becomes:

```text
Router> enable
```

### Important

Know both:

* The **full command**
* The **common shortcut**

For the exam, understand the full command.

---

# 6. Setting a Privileged EXEC Password

The older method is:

```text
enable password <password>
```

Example:

```text
enable password CCNA
```

This protects access to Privileged EXEC mode.

### Passwords are case-sensitive

```text
CCNA
```

and

```text
ccna
```

are different passwords.

When entering a password, the characters are **not displayed**.

---

# 7. Running Configuration vs Startup Configuration

Cisco devices maintain two important configuration files.

## Running Configuration

**running-config**

* Current active configuration
* Changes are made here
* Stored in RAM
* Lost after a reload if not saved

View it with:

```text
show running-config
```

Shortcut:

```text
show run
```

---

## Startup Configuration

**startup-config**

* Saved configuration
* Loaded when the device boots
* Stored in NVRAM
* Used after a restart/reload

View it with:

```text
show startup-config
```

### Critical distinction

```text
running-config = current configuration
startup-config = configuration used after reboot
```

---

# 8. Saving the Configuration

Changes made to `running-config` must be saved if you want them to survive a reboot.

There are three equivalent methods:

### Method 1

```text
write
```

### Method 2

```text
write memory
```

### Method 3

```text
copy running-config startup-config
```

### Most important concept

```text
running-config
      |
      | copy running-config startup-config
      ↓
startup-config
```

If you configure a device and reboot **without saving**, your unsaved changes may be lost.

---

# 9. Password Encryption

## `service password-encryption`

Command:

```text
service password-encryption
```

This encrypts passwords that would otherwise appear in clear text in the configuration.

Example:

```text
Router(config)# service password-encryption
```

The password will no longer appear in plain text.

### Type 7

Passwords encrypted using `service password-encryption` appear with:

```text
7
```

This is Cisco's **Type 7** password obfuscation/encryption.

### Security warning

Type 7 is **weak** and can be cracked relatively easily.

Therefore:

> `service password-encryption` is better than plain text, but it is NOT strong security.

---

# 10. `enable secret`

A more secure way to protect Privileged EXEC mode is:

```text
enable secret <password>
```

Example:

```text
enable secret Cisco
```

The `enable secret` password is automatically encrypted.

### Type 5

In the transcript, the password appears with:

```text
5
```

This indicates **MD5-based Type 5 hashing** in this IOS context.

### Important comparison

| Method                                            | Stored as  | Security                |
| ------------------------------------------------- | ---------- | ----------------------- |
| `enable password`                                 | Plain text | Weak                    |
| `enable password` + `service password-encryption` | Type 7     | Better, but weak        |
| `enable secret`                                   | Type 5     | More secure than Type 7 |

### Exam rule

**Prefer `enable secret` over `enable password`.**

---

# 11. Enable Secret Takes Precedence

If both are configured:

```text
enable password CCNA
enable secret Cisco
```

the **enable secret** takes precedence.

When you type:

```text
enable
```

you will be prompted for the **enable secret**, not the enable password.

### Remember

> If both exist → **enable secret wins.**

---

# 12. `service password-encryption` and `enable secret`

`service password-encryption` does **not** affect `enable secret`.

The enable secret is encrypted automatically.

Therefore:

```text
service password-encryption
```

is not required to encrypt an `enable secret`.

---

# 13. The `do` Command

Normally, commands such as:

```text
show running-config
```

are Privileged EXEC commands.

However, you can execute them from Global Configuration Mode by using:

```text
do show running-config
```

Example:

```text
Router(config)# do show running-config
```

### Why use `do`?

It allows you to execute an EXEC-level command while remaining in configuration mode.

---

# 14. Removing Configuration

To remove a previously configured command, use:

```text
no
```

before the command.

Example:

```text
no service password-encryption
```

This disables the service.

### Important behavior

If `service password-encryption` is disabled:

* Existing encrypted passwords **remain encrypted**
* New passwords may appear in clear text

The command does **not** automatically decrypt passwords that were already encrypted.

---

# 15. Important Commands

## Enter Privileged EXEC

```text
enable
```

Shortcut:

```text
en
```

---

## Enter Global Configuration

```text
configure terminal
```

Shortcut:

```text
conf t
```

---

## Set an enable password

```text
enable password <password>
```

---

## Set a more secure enable password

```text
enable secret <password>
```

---

## Encrypt passwords

```text
service password-encryption
```

---

## View current configuration

```text
show running-config
```

Shortcut:

```text
show run
```

---

## View saved configuration

```text
show startup-config
```

---

## Save configuration

```text
write
```

or:

```text
write memory
```

or:

```text
copy running-config startup-config
```

---

## Execute an EXEC command from configuration mode

```text
do <command>
```

Example:

```text
do show running-config
```

---

## Remove a configuration command

```text
no <command>
```

---

## Leave the current mode

```text
exit
```

---

# 16. Essential CLI Workflow

A basic Cisco configuration session looks like this:

```text
Router>
Router> enable
Router#
Router# configure terminal
Router(config)#
Router(config)# enable secret Cisco
Router(config)# exit
Router#
Router# copy running-config startup-config
```

### What happened?

1. Connected to the device
2. Started in User EXEC
3. Entered Privileged EXEC
4. Entered Global Configuration Mode
5. Configured an `enable secret`
6. Returned to Privileged EXEC
7. Saved the configuration

---

# 17. Mode Cheat Sheet

| Mode            | Prompt            | How to Enter         | Purpose                     |
| --------------- | ----------------- | -------------------- | --------------------------- |
| User EXEC       | `Router>`         | Initial login        | Limited access              |
| Privileged EXEC | `Router#`         | `enable`             | Advanced viewing/management |
| Global Config   | `Router(config)#` | `configure terminal` | Configure device            |

### Memorize this:

```text
Router>              User EXEC
Router#              Privileged EXEC
Router(config)#      Global Configuration
```

---

# 18. Key Exam Traps

### Trap 1 — Console cable

RJ-45 console port → **rollover cable**

Not crossover.

---

### Trap 2 — Configuration files

```text
running-config = current
startup-config = saved
```

---

### Trap 3 — Saving configuration

To make running configuration survive reboot:

```text
copy running-config startup-config
```

---

### Trap 4 — Most secure password method

Use:

```text
enable secret
```

Not:

```text
enable password
```

---

### Trap 5 — Both passwords configured

If both exist:

```text
enable password
enable secret
```

**enable secret takes precedence.**

---

### Trap 6 — Command abbreviation

Abbreviations only work when they are **unambiguous**.

```text
en
```

works for `enable`.

```text
e
```

may be ambiguous.

---

### Trap 7 — `?`

`?` helps you discover available commands and valid completions.

---

### Trap 8 — `do`

From configuration mode:

```text
do show running-config
```

allows you to execute an EXEC command without leaving configuration mode.

---

# 19. Quiz Review

### Q1. What cable connects to a Cisco device's RJ-45 console port?

**Answer: Rollover cable**

---

### Q2. The enable password is not accepted. What could cause this?

**Answer: Caps Lock is on**

Cisco passwords are case-sensitive.

---

### Q3. What is the most secure method presented for protecting Privileged EXEC mode?

**Answer: `enable secret`**

---

### Q4. What happens if both `enable password` and `enable secret` are configured?

**Answer: The `enable secret` is used.**

---

### Q5. What is the full command represented by `conf t`?

**Answer:**

```text
configure terminal
```

---

# 20. Must-Know Commands — Final Revision

```text
enable
```

→ User EXEC → Privileged EXEC

```text
configure terminal
```

→ Privileged EXEC → Global Configuration

```text
enable password <password>
```

→ Sets an enable password

```text
enable secret <password>
```

→ Sets a more secure enable password

```text
service password-encryption
```

→ Encrypts applicable clear-text passwords

```text
show running-config
```

→ Displays current configuration

```text
show startup-config
```

→ Displays saved configuration

```text
copy running-config startup-config
```

→ Saves current configuration

```text
do <command>
```

→ Runs an EXEC command from configuration mode

```text
no <command>
```

→ Removes/disables a configuration command

```text
exit
```

→ Leaves the current mode

---

## Hands-On Practice

Recreate the following in **Cisco Packet Tracer**:

```text
Router> enable
Router# configure terminal
Router(config)# enable secret Cisco
Router(config)# exit
Router# show running-config
Router# copy running-config startup-config
Router# show startup-config
```



## One-Minute Revision

> **User EXEC (`>`)** → limited access
> **Privileged EXEC (`#`)** → advanced management
> **Global Config (`config)#`)** → change configuration
>
> `enable` → enter privileged mode
> `conf t` → enter configuration mode
> `enable secret` → secure privileged-mode password
> `show run` → current configuration
> `show start` → saved configuration
> `copy run start` → save configuration
> `do` → run EXEC command from config mode
> `no` → remove configuration
> `?` → get help
> `Tab` → autocomplete
>
> **running-config = current**
> **startup-config = saved**
