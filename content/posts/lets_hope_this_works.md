+++
date = 2024-11-08
title = "Let's Hope This Works..."
+++

This site's source code is hosted on GitHub. It uses a static site generator called [Hugo](https://gohugo.io/) to turn markdown files into HTML, CSS, and JS.
However, the site itself is *not* hosted on GitHub. This site is running on my own personal web server. That means that I have to, somehow, use Hugo to render the final website and get that rendered website onto my web server, but not necessarily in that order.

Ideally, I use GitHub Actions to SSH into my web server, pull the changes, and render the final site. However, Network Address Translation or NAT makes this difficult. If you were to run `dig scarzehd.com`, you would not get the public IP address of this web server. Instead, you would get an IP owned by Cloudflare. Cloudlfare proxies the traffic to the web server and back to you. Unfortunately, my current setup only allows for HTTP and HTTPS connections to be proxied, not SSH. This means we need a separate solution.

Fortunately, this isn't too difficult. All we have to do is have the server check for any new changes the the GitHub repository and pull them. Once that's done, we can render the final webpage and display it to any users. For a good balanace of responsiveness and performance, I've chosen to run this script every 5 minutes through crontab. I also only render the page if there were actually any changes to pull.

If everything goes to plan, this webpage will be automatically be pulled from GitHub and rendered into a static site for your viewing pleasure.

-- Super Cool Arzehd

P.S. Here's a sample copy of the script. Run at your own risk; I'm not very familiar with shell scripting.

```
#!/bin/bash

# Run in the same directory as your git repo

OUTPUT="$(git pull)"

# The theme I use is added as a submodule. This pulls any updates. If you don't want or need that, it's not necessary.

git submodule update --recursive

if [[ $OUTPUT != "Already up to date." ]]; then
        hugo;
fi
```