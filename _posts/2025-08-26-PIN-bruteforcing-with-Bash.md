---
layout: post
title:  "Bruteforcing a PIN using Bash"
tags: Scripting Bash
---

# Preface

This is a part of HTB Academy's "Login Brute Forcing" Module. The premise is to find a 4 digit PIN and submit the correct one to `http://$IP:$PORT/pin?pin=$PIN`. A python script is provided to automate the process for you, but I decided to write it in Bash to test my skills.

# Code

Here is the code I used if you just wanted to look at it:

```sh
  for pin in $(seq -w $1 9999);
  do
    response=$(curl -s -w "%{http_code}" -o /dev/null http://83.136.253.59:56861/pin?pin=$pin)
    if [ "$response" = "200" ]; then
      echo "Found the correct PIN! It is $pin"
    fi
  done
```

(The IP included here will 99% won't work, change it to whichever one is provided if you're doing it along with the article)

# Usage and Things I've Learned

As for the usage, I didn't want a single process trying all 10,000 combinations, so I kinda did an esoteric thing, which was to do a bunch of `./script.sh 0 & ./script.sh 1000` and so forth. Writing this article now, I realized that they will all try the combinations so I should've set the ending parameter as well, but this is why we do these things.

Another thing I discovered is that I can use GNU's `parallel` to more easily create multiple processes, which will be useful for future times where I'll need to run a script with multiple processes (which there will be plenty of opportunities to do so, I'm sure). I only found out about it after the fact as I quickly realized how surely inefficient my method of creating multiple processes is. I'll definitely be incorporating it into my future scripts when the occassion arises to speed things up.
