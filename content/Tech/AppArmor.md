# AppArmor

Resources:
- SUSE Security and Hardening Guide:
    - https://doc.opensuse.org/documentation/leap/security/html/book-security/part-apparmor.html
    - https://doc.opensuse.org/documentation/leap/security/html/book-security/cha-apparmor-profiles.html
- Ubuntu man: https://manpages.ubuntu.com/manpages/xenial/man5/apparmor.d.5.html
- Debian man: https://manpages.debian.org/unstable/apparmor/apparmor.d.5.en.html

AppArmor is a way to restrict permissions on a pre-process basis, instead of per-user.

Most of the config lines for a given process just consist of:
- Naming a particular file or group of files
- Specifying allowed interactions between the process and the file(s) (e.g. read, write, execute)

## AppArmor Profile Flags

See "PROFILE FLAGS" in the Ubuntu man page. Notable ones:
- If you add `flags=(complain)`, it will allow all operations that it normally would deny, but it still prints an angry message to console

## AppArmor Resource Flags

See "Access Modes" in the Ubuntu man page. Common ones:
- `x` - the process is allowed to execute this file
    - With `I` - the executed file will inherit the current process's AppArmor profile
- `r` - the process is allowed to read this file
- `w` - the process is allowed to write this file

## Symlinks

When a process requests access to a file path with one or more symlinks in it, AppArmor will fully resolve the file path to its "real" path before comparing it with the access rules for that process.

This means it's perfectly fine for a process to access a file via a symlink, but in the profile source config for that process, you need to specify the real path of the file, not a path with a symlink in it.

You can also use alias rules in the AppArmor profile (see "Alias rules" in the Ubuntu man page) to map declared file paths in the profile to their real paths.

