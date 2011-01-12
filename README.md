Twitter-Mobile
==============
Twitter-Mobile is an html/javascript based Twitter client for mobile devices using jQuery Mobile and the Twitter @anywhere API.

My goal was to create a standalone webpage application that self generates its pages based on interaction with twitter.com. (no further interaction with the originating server)

This is proof of concept code and is not very durable nor complete.

Screenshots:

<div style="background:white;">
<img style="border: 1px solid black;" src="http://macdevshop.com/images/twitterHome.jpg" />&nbsp;&nbsp;&nbsp;<img style="border: 1px solid black;" src="http://macdevshop.com/images/twitterUser.jpg" /> 
<br />
<img style="border: 1px solid black;" src="http://macdevshop.com/images/twitterLogin.jpg" />&nbsp;&nbsp;&nbsp;<img style="border: 1px solid black;" src="http://macdevshop.com/images/twitterMessage.jpg" /> 
</div>

To try the code in action, visit:

<a href="http://macdevshop.com/twitter/index.html">http://macdevshop.com/twitter/index.html</a>
  
How to use this code
==================

- copy index.html to your server
- goto dev.twitter.com and register a new twitter web application using your twitter account.
You will need to specify an application name, the http address of your server and some other minor info. You will receive a public API key that you must include in the following script tag in the header.

       <script src="http://platform.twitter.com/anywhere.js?id=YOURAPI&v=1" type="text/javascript"></script>

- access the webpage using your mobile device browser or computer browser
- if you want to view the application in fullscreen (for an iphone/ipod touch), Select the Add to Home Screen
option in Mobile Safari. Then launch the app from your home page.
- this code will first see if you are logged into twitter, if not, it will display a twitter login
button. When you press it, it will redirect you to twitter.com to login. After login, twitter will redirect
you back to the webpage.
- this app will grab your home timeline from twitter and display it on the screen
- display logged in user's direct messages by pressing the Direct Msg button on the home timeline
- click on @user's to bring up their tweets, click on #subject's to search twitter for that keyword.
- send a general tweet to the world by pressing the Tweet button in the header
- reply to a user's tweet by pressing the right arrow button on the top right of each tweet
- retweet by pressing the circular arrow button located at the bottom-right of each tweet
- send a private message to a user by pressing the Direct Msg button in the footer
- do random searches of twitter using the search box
- this program will autorefresh your home_timeline (default 10 minutes).

ToDo
===========
- notify user of new tweets
- better error handling

Known problems/issues
==================
- only tested on an ipod touch
- the UI could be improved significantly, I just wanted to get a feel for the jQuerymobile API
- error handling is almost non-existent

jQuery Mobile wishlist
==================
- It was kind of tricky manipulating jqm to work with self (javascript) generated pages (jqm is currently designed to work well with ajax'd pages or normally linked external pages)

- Updating the html of a 'page' that has already been processed by jqm is not very straightforward nor well documented. The method that i am currently using is: remove the jqm processed DOM structure that you wish to update, re-add the modified html to the DOM, then run $(NEWHTML).page() to get jqm to reprocess that section of HTML.

- It would be very tricky for jqm to reprocess an already processed DOM (since it can't tell the difference between processed html and new html). So the current solution works but maybe jqm could offer some helper methods.. such as $(structure)).replaceHTML(NEWHTML) where this function would delete the old DOM structure, insert the new HTML into the DOM and rerun page() on the new structure.

- Another solution is to save the original html and create a new DOM structure with the processed HTML. When the user wanted to change his html, he would make the changes to the original html, then call a function to reprocess the html. This would probably be a memory hog and could take significant cycles on large pages. But maybe jqm would only use this method if the user flags the original page as being modifiable. 
If it is readonly, then jqm would default to its current method

- Another problem that i ran into was jqm processing html that i preferred that it bypassed.. Maybe jqm could  provide another attribute to tell it to bypass a certain tag (and its siblings). I was trying to imbed a-tags in my html where i wanted to handle click events with a javascript handler instead of going to ajax or remote pages. jqm processed these a-tags and and installed its own handlers. 
I had to unbind() them and then bind my handler.

- jqm markup. I understand that jqm is trying to be a general purpose solution for the most common UI elements used on multiple mobile platforms but sometimes the canned UI elements don't work for an application. 
I originally wanted an unordered list that had a thumbnail, a title, and a description but readonly. This isn't possible afaik.
I had to use thumbnail, a linked title + description in order for jqm to generate the correct markup. I am guessing that jqm is looking for certain html patterns to determine which markup to use.
Maybe in the future, a more flexible solution would be nice. Like a checklist (readonly, clickable), thumbnail/no thumbnail, title/no title, etc. but implemented using attributes.

- On the css front.. jqm assumes many css parameters such as the thumbnail size. It would be nice if the user could leave hints that jqm would use to generate its css, such as < img width=45 height=45 ...>. 
I was able to override jqm's CSS by using style="" in my html but it is trickier on jqm generated html. They only use jqm generated classes for css. I overrode the jqm classes to create my desired look but my changes affected other parts of my page. A possible solution would be to automatically propagate classes that I specify in my html to jqm generated html. Then i could override jqm css using my own classes.

- jqm hash history assumes ajax or external pages. Consider expanding it to include self generated pages

- I had a problem updating the html in headers and footers with data-position=fixed. I needed to change the visible buttons on my fixed footer depending on the page content. I tried to use the trick explained above to replace the html then running page(). 
This worked for the most part except the fixed function stopped working.. Apparently jqm adds some additional handlers to fixed headers/footers to dynamically adjust their position on the page depending on the current scroll position. But these handlers weren't being reinstalled after i ran page() on the updated html.  

- I designed my own dialog functionality instead of using JQM's since JQM's was designed to work with AJAX or externally linked pages instead of pages that were created by javascript. My solution work's pretty well but i am sure that it's look and feel (css) will break down on untested devices. That is where JQM really shines!

- Overall, jqm worked very well. My application doesn't use standard ajax or linked pages so i guess that i was stretching the limits of the current alpha jqm.

Twitter @anywhere comments
====================

- @anywhere is a nice javascript interface to the twitter restful API. The main document: <a href="http://dev.twitter.com/anywhere/begin">http://dev.twitter.com/anywhere/begin</a> explains how to use the basic functions (login/logout, follow, hovercard, send tweet) which are the most common things somebody wants on a website (non-twitter client).

- Twitter has many more capabilities in their restful interface that can be interesting. Like your home_timeline (i.e. tweets from people you follow). Sending tweets to specific people, profiles, etc. All of these commands aren't explained in the main document. 

- There is a secondary document called the @anywhere API document <a href="http://platform.twitter.com/js-api.html">http://platform.twitter.com/js-api.html</a> which has a "very" brief outline of the other methods that can be called using this API. Some of it is wrong, unimplemented or doesn't have enough detail to be useful without actually digging around in the minimized javascript code. I believe that Twitter does that on purpose since they dont want people doing these things with this API. (Not sure why since they expose these capabilities in the restful interface, maybe it has to do with javascript being dangerous in certain instances).
  
Copyright (c) 2010 Richard Sepulveda 

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
