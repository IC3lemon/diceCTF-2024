# zshfuck

***

## Description

```
may your code be under par. execute the getflag binary somewhere in the filesystem to win

nc mc.ax 31774
```

***

## Attempt

we are given a file `jail.zsh` and a netcat command.\
the netcat launches an instance whose source seems so to be `jail.zsh`.

```zsh
#!/bin/zsh
print -n -P "%F{green}Specify your charset: %f"
read -r charset
# get uniq characters in charset
charset=("${(us..)charset}")
banned=('*' '?' '`')

if [[ ${#charset} -gt 6 || ${#charset:|banned} -ne ${#charset} ]]; then
    print -P "\n%F{red}That's too easy. Sorry.%f\n"
    exit 1
fi
print -P "\n%F{green}OK! Got $charset.%f"
charset+=($'\n')

# start jail via coproc
coproc zsh -s
exec 3>&p 4<&p

# read chars from fd 4 (jail stdout), print to stdout
while IFS= read -u4 -r -k1 char; do
    print -u1 -n -- "$char"
done &
# read chars from stdin, send to jail stdin if valid
while IFS= read -u0 -r -k1 char; do
    if [[ ! ${#char:|charset} -eq 0 ]]; then
        print -P "\n%F{red}Nope.%f\n"
        exit 1
    fi
    # send to fd 3 (jail stdin)
    print -u3 -n -- "$char"
done
```


on starting nc, we are greeted with a prompt to specify a charset\
and we can only use characters from this charset to form any commands.\
However, the charset can only be a max of 6 characters.\
did this taking note of the above :\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/ccaeeaac-ba31-4114-9123-2bcb310f3df5)

later a senior had the idea of using `find .` which gave this :\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/a39f36df-c083-41ac-ad67-22ba3dda9e8a)

So now we know where the getflag binary is\
now we need to figure out a way to cd down into 
`/y0u/w1ll/n3v3r_g3t/th1s/getflag`\
within 6 characters.\
I searched that up and found out about using wildcards for pattern matching.\
https://www.warp.dev/terminus/linux-wildcards

Here he says that you could use question marks `?` \
to navigate somewhere if you don't know the characters but do know the filename pattern\
so I thought `cd ???/????/?????????/????/` would do the trick. (as it matches the pattern of `/y0u/w1ll/n3v3r_g3t/th1s` dir)\
but it didn't.

because `?` ,`*` and ` were banned.\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/d5fab204-43f5-48bd-940d-fee32ddbe4ab)

gave up on the entire "cd-ing without entering the filenames somehow" idea after that.\

I then found this.\

https://stackoverflow.com/questions/16773189/hacking-a-way-to-a-remote-shell-in-5-characters

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/7326c37a-ba26-4454-ba76-3944383bafc9)

insanely relevant.\
what I gathered was that this man created a shell within the running shell,\
configured the created shell to take the connection information from the file descriptor(4)\
and the created shell took over, allowing him to bypass the character limit restriction.\
and do whatever bro wanted.

tried it but it wasn't working.\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/8a91660b-04c5-48b1-82bf-aa7d529f1809)

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/656ab243-1091-4ecf-a708-587e4f6a5e23)

also turns out, you can't create any files because it was a read only file system.

gave up on this idea after a while too.

I also did try to kill run.\
A senior tried that, but that didn't work.\
![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/a450edfe-77b4-4fa0-8fd7-32d2b6fc9737)



few hours later the writeups came in,\
and I found out I was insanely close with my first idea.

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/76032d72-3b7a-4bea-94b6-0764ed57d654)

Apparently this `[--~]`  and `[^^]` existed.
the first thing checks all characters satisfying from `-` to `~ `
and the second checks all characters that satisfy except `^`

sure enough:

![image](https://github.com/IC3lemon/diceCTF-2024/assets/150153966/3344e867-48a8-4af3-b731-d99d5b9d1394)

***

## Flag > `dice{d0nt_u_jU5T_l00oo0ve_c0d3_g0lf?}`

***
