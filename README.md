# cli-otp-manager
Simple CLI for managing and generating OTPs for your various accounts. &lt;100 lines of Bash script.

### Why?

I just wanted a simple shell script to manage time-based OTPs and copy them to the clipboard instead of having to fuss with Google Authenticator or FreeOTP. It encrypts and signs OTP secret keys using GnuPG before storing them to disk. Easy! :sunglasses:

Need something fancier? Check out [pass-otp](https://github.com/tadfisher/pass-otp).

## Quick start

1. If you don't have them already, install [oathtool](https://www.nongnu.org/oath-toolkit/) and GnuPG. e.g. on Fedora/RHEL/CentOS do: `dnf install oathtool gnupg2`
2. It's a bash script, just use it!

### Add a new key:
```
$ otp -a
Enter nickname (e.g. a domain name) for this key: example.com
Enter OTP secret key value: sqod lqls ogws vykm 4o66 mbmi wtcq ae2p
```

### When it's time to authenticate:
```
$ otp example.com
888381
(Copied to clipboard. Expires in 27 seconds)
```

### To list stored keys or get help:
```
$ otp
Syntax:
otp [nickname]

Stored OTP keys:
	example.com

To add a new OTP key:
otp -a
```
## Customizing
It's a shell script, easy to customize. For example, by default it stores encrypted keys in `~/.ssh/oath` (don't ask, it made sense to me at the time!) but you can change that.

## Security considerations
Using `oathtool` there's currently no way to pass the secret key except as a command-line parameter. So, it will show briefly in the process table &ndash; in `/proc` and via `top`, `ps` etc. [See this thread for a longer explanation](https://gitlab.com/oath-toolkit/oath-toolkit/-/issues/6). If you're on a multiuser system this risk may concern you. One way to mitigate it: the Linux kernel has the `hidepid` mount option, so you can [hide processes](https://man7.org/linux/man-pages/man5/proc.5.html) from other users by mounting `/proc` with the `hidepid=2` mount option.

Otherwise, the script is written with the goal of minimizing potential exposure of secret keys. Contributions/improvements are welcome.

## Contributions
Please!

