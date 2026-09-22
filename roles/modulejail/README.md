# modulejail role

`modulejail` is a single POSIX shell script that shrinks the kernel-module attack surface on Linux machines by
 writing a `modprobe.d` _blocklist_ containing every kernel module that
 * is not in in use at the moment `modulejail` is executed and
 * which is not listed in a built-in _profile_ nor in an optional allowlist.

For details see:

 * https://github.com/jnuyens/modulejail
 * https://modulejail.com/
 * https://linuxsecurity.com/features/linux-kernel-module-hardening-modulejail