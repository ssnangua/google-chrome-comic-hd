# Google Chrome Comic HD

- [Official Version (English)](https://www.google.com/googlebooks/chrome/)
- Super-Resolution Version
  - [Google Chrome Comic (English)](./english)
  - [Google Chrome Comic (Textless)](./textless)

## All words

### P1 - Google Chrome

- Behind the Open Source Browser Project
- Today, most of what we use the web for on a day-to-day basis aren't just web pages, they're **applications**.
- People are watching and uploading videos, chatting with each other, playing web-based games...
- All these things that didn't exist when the first browsers were created.
- Wouldn't it be great, then, to start from scratch --
- -- and design something based on the needs of today's web applications and today's users?

### P2

- First, browsers need to be more **stable**. When you're writing an important email or editing a document, a browser crash is a big deal.
- Browsers also need to be **faster**. They need to start faster, load pages faster --
- -- and for web apps, JavaScript itself can be **a lot** faster.
- They need to be more **secure**. Given what's known about mass browser exploits, browsers need architectural changes to disadvantage malware.
- And we want browsers to find that sweet spot between too many features and too few, with a **clean**, **simple**, and **efficient** user interface.
- Finally, Google Chrome is a fully **open source** browser.
- We want others to adopt ideas from us --
- -- just as we've adopted good ideas from others.

### P3 - Part One

- Stability, Testing and the Multi-Process Architecture
- When we started this project, the Gears guys were saying that one of the problems with browsers is that they're inherently single-threaded.
- For example, once you have JavaScript executing, it's going to keep going, and the browser can't do anything else until JavaScript returns control to the browser.
- So developers write APIs that are asynchronous --
  - Call me as soon as you're done!
  - Sure thing!
- -- and every now and then the browser locks up because JavaScript is hung up on something.
  - BOO-BEEP!
  - All circuits are busy...

### P4

- The Gears guys were thinking about a multi-threaded browser, but that led us to talk about, well, instead of multiple threads --
- -- what if we have **multiple processes**? Each having its own memory and its own copy of the global data structures.
- We're applying the same kind of process isolation you find in modern operating systems.
- So, separate processes rendering separate tabs.

### P5

- And now you have separate JavaScript threads as well.
- One tab can be busy, while you're still using all the others.
- And if there's a browser bug in the Renderer (and our experience is that it's almost impossible to eliminate all bugs), we still only lose the one tab.
- When one tab goes down you get a 'sad tab' but it doesn't crash the whole browser.
- And yes, it really looks like this.
- A multi-process design means using a bit more memory up front. Each process has a fixed additional cost.
- But over time, it will also mean **less memory bloat**.
- In a traditional browser, you only have one process and one address space that you keep loading web pages into.
- When you have too many tabs open, you can close some to free up memory.
- When you bring in another tab, you use the memory that was previously used.

### P6

- But as time goes on, fragmentation results -- little bits of memory still get used even when a tab gets closed.
- Either we have memory that nothing can refer to again, or there's a piece of de-allocated memory we still have pointers to.
- So when the browser wants to open a new tab, it can't fit it in the existing space --
- -- and so the OS has to grow the browser's address space.
- And this problem grows all day, as the lifetime of the browser extends.
  - Hurry up, dammit!
  - Try closing some tabs.
  - I did!
- But when a tab is closed in Google Chrome, you're ending the whole process --

### P7

- -- and **all** that memory gets reclaimed.
- Open a new tab now, and you're starting from scratch.
- So as you browse, we're creating and destroying processes all the time. If there's a crazy memory leak it won't affect you for that long because you'll probably close the tab at some point and get that memory back.
- And we're taking it one step further. Suppose you navigate from domain A to domain B. there's no need for any relationship between the two sites --
- -- so now we can throw away the old rendering engine, the old data structures, the old process.
- So, even within a tab, we can be collecting and tossing out the garbage, recycling the whole process.

### P8

- And just like with your OS, you can look under the hood with Google Chrome's Task Manager to see what sites are using the most memory, downloading the most bytes, and abusing your CPU.
- Why is this application downloading the **entire internet**?
- You can even see plug-ins within the tab, since they appear in Chrome's Task Manager as separate processes.
- So, when things start freaking out, you'll finally have some insight into **who's** misbehaving and **why** --
- -- and **eliminate them**.
- **Placing blame** where blame **belongs**.

### P9

- Google Chrome is a massive, complicated product that will need to load billions of different web pages, so **testing** is critical.
- Fortunately, here at google, we have an equally massive infrastructure for crawling web pages.
- Within 20-30 minutes of each new browser build, we can test it on tens of thousands of different web pages.
- Each week, 'Chrome Bot' tests millions of pages, giving our developers early results they'd otherwise have to wait until external beta for.
- The key is catching problems. It is less expensive and easier to fix them right away. After a few days it is harder to track them down.
- And catching them early helps engineers write better code. They say, 'Oh, this mistake is part of a pattern' and the next time, they're less likely to make it.

### P10

- Of course, there are billions, maybe trillions of webpages out there. If each build is tested against a million sites, **which** million do we use?
- Fortunately, we have a unique take on this problem also.
- **_We already rank_** pages base on which pages the average user is most likely to visit.
- **_At the very least_**, we'll make sure we won't be broken on the kinds of sites people use on a day-to-day basis.
- There are several ways we test each check-in. From unit tests of individual pieces of code --
- -- to automated UI testing of scripted user actions like 'click back button... went to page...' --
- -- to fuzz testing: sending your application random input.

### P11

- In layout testing, WebKit found that producing a schematic of what the browser **thinks** it's displaying is a more precise way to compare layouts than taking screenshots and creating a cryptographic hash.
- When we started we were passing 23% of WebKit's layout tests. Moving from there to 99% has been a fun challenge and an interesting example of test-driven design.
- There are limits to what we can do with automated testing.
- We can't test websites that require a password, for example.
- And it's not the same as a human being walking around and misusing things. We are using the browser in the way we've designed it to be used.
- It's hard to cover 100%, but that's what we're trying to do.
- I don't care if there's one fewer cool feature. I just want this Product to be **rock solid**.

### P12 - Part Two

- Speed: WebKit and V8
- **WebKit** is the open source rendering engine we used for Google Chrome.
- We were impressed by how **fast** it is.
- We also knew there was a team at google working on android and we asked them, 'Why did you guys use WebKit?'
- They said it uses memory efficiently, was easily adapted to embedded devices, and it was easy for new browser developers to learn to make the code base work.
- Browsers are complex. One of the things done well with WebKit is that it's kept **simple**.

### P13

- Because JavaScript is so important to the web today --
- -- we decided it was important to work on building a JavaScript **virtual machine** --
- -- which is exactly what the **V8 team** in Denmark did.
- The V8 team are experts at virtual machines. Whatever language you want to put into a VM, they can tell you how to write it.
- Virtual machines provide safety and platform independence.
- But previous virtual machines for JavaScript were designed for small programs, where the performance and interactivity of the system weren't that important.
- They just wanted to run some very basic stuff on a webpage.

### P14

- But now, you have web applications like Gmail that are using the web browser to its fullest when it comes to DOM manipulations and JavaScript --
- -- and that simplistic approach to JavaScript engines isn't enough anymore.
- So we started with no code, just some wild ideas about how to make it go really fast --
- -- such as introducing **hidden class transitions**.
- JavaScript itself is **classless**. You can create a new object, dynamically add properties to it and go on.
- But in **V8**, as execution goes on, objects that end up with the same properties will share the same hidden class and we can start applying dynamic optimizations based on that.
- Another factor in V8's speed is dynamic code generation.
- When other JavaScript engines run, they look at the JavaScript source code and generate an internal representation of it they can interpret.

### P15

- But, when you have to do interpretation, you have to look at the structure of your internal representation over and over again.
- So instead, **V8** looks at the JavaScript source code and generates **machine code** that can run directly on the CPU that's running the browser.
- When you interpret once and compile machine code, then that code **is** your representation of the JavaScript source code and it doesn't need to be interpreted, it just **runs**.
- Finally, the core design flaw of current JavaScript engines --
- -- is **bad garbage collection** behavior.
- JavaScript and other modern object-oriented programming languages have **automatic memory management**.
- If you don't have a reference to an object anymore, its memory can be **reclaimed** by the system. That's garbage collection, and its a fairly trivial process.
- But in existing JavaScript virtual machines, they use **conservative** garbage collection --

### P16

- -- which means that because you don't know exactly where all the pointers are --
- -- you start searching through the execution stack to see which words look like pointers.
- **But** the ones that sort of look like pointers could also be integers that just happen to have the same address as an object in the object heap.
- In V8, we are using **precise** garbage collection, so we know precisely where **all** of the pointers are on the stack and this gives us several advantages.
- One is that we can migrate an object to another place and just rewire the pointer.
- And, because we know precisely where all the pointers are, we can also implement **incremental** garbage collection.
- Meaning quick garbage collection round-trips that are close to a few milliseconds, compared to processing all 100MB of data which could cause second-long pauses.

### P17

- This means much better interactive performance of web applications, like smoother drag and drops.
- V8 has a specific API that Google Chrome uses, but the core part of the engine is independent of the browser.
- So, other browsers can include it --
- -- or, if there's another project that JavaScript can apply to, developers can take V8 by itself.
- We hope V8's performance will set a new bar, and that the other development teams will continue to improve in this space.
- Because if you look at any other system that's become faster over time, what happens is that you get bigger, better, more inventive apps.

### P18 - Part Three

- Search and the User Experience
- In Google Chrome, the primary piece of the user interface is **the tab**.
- As soon as we started thinking about it that way, the design naturally followed.
- We began rebuilding the UI so the tabs were on **top**.
- We could detach the tabs easily because of the separation of the browser and tab processes.
- Move it from window to window and the tab's state goes with it.

### P19

- And because the tabs are the most important part of the UI, each tab has its own controls.
- And its own **URL box**.
- Which around here we've been calling the 'Omnibox'.
- The Omnibox handles far more than just URLs.
- It also offers suggestions for searches, top pages you've visited before, pages you haven't visited but are popular and more...
- You have full text search over your history. If you found a good site for digital cameras yesterday, you don't have to bookmark that page.
- Just type 'digital camera' and quickly get back to it.

### P20

- When the team suggested autocompletion in line, I said I **hated** it when browsers stick all this crap into a location bar as I'm typing. It's never what I want.
- But, they said, no, no, it'll be fine, trust us -- and they went on and made it something really compelling...
- Inline completions will never flicker, never flash. It's perfectly, aesthetically non-distracting.
- Plus, it'll only autocomplete to something you've explicitly typed before.
- Type `c` `return` and you might go straight to `cnn.com` --
- -- but **never** to `cnn.com/2008/politics/07/27/campaign.wrap/index.html`.
- And when you search on sites like amazon, wikipedia or even google --
- -- the search boxes on those pages are captured on your local system --
- -- so you can search those same sites with different terms later on, straight from the address bar, by starting the site's name and pressing 'tab'.

### P21

- Open a new tab in most browsers today, and you'll get your homepage.
- Some users have a blank page because it opens quickly.
- But the action of opening a tab is a **statement of intent**: you want to **go someplace**!
- Maybe you know **where**. Maybe you don't know and need to **search**.
- We're going to show a page that is designed to be fast, but also helps you complete that action.
- Our default experience, then, is the **new tab page** with your nine most visited pages here --
- -- and the sites you search on most **here**.

### P22

- It's the pages you were going to type into the URL box anyway. Google Chrome uses your behavior in the Omnibox to feed into that page.
- You might open it and be, like, what's all my stuff doing here? But after a while, you see this page and it's just you, it's **your** browser.
- Google Chrome has a privacy mode. You can create an 'incognito' window and nothing that occurs in that window is ever logged on your computer.
- It's a read-only mode: you can still access your bookmarks, but none of your history is saved in the browser --
- -- and when you close the window, the cookies from that session are wiped out.

### P23

- Want to keep a surprise gift a secret?
- Just continue to browse normally in any other window.
- There's no concept of a drive-by pop-up in Chrome. JavaScript has no way to force a pop-up into your world.
- Pop-ups are scoped to the tab they came from --
- -- and **confined** there.
- If it's something you **want**, though --
- -- just drag it out and it'll be promoted to its own window.

### P24

- Developers call the UI frame of the browser its "Chrome".
- We wanted to reduce the "Chrome" of Google Chrome. In the case of webapps, we've made it so you can launch them in their own streamlined window, without the URL box and browser toolbar.
- We don't want to interrupt anything the user is trying to do.
- If you can just **ignore** the browser, we've done a good job.

### P25 - Part Four

- Security, Sandboxing and Safe Browsing
- Malware and phishing are a huge problem for users, affecting trust and confidence in the web.
- When we started this project, it was a very different landscape from when the other browsers started.
- Back then, it was about rendering the page and getting the cool things working. There was no monetary incentive to put malware on users' machines.
- Now, malware is very financially driven. It's all about stealing passwords and moving money around.
- In thinking about security, we began with the assumption that your browser would get compromised.
- You will eventually encounter malware.

### P26

- With **sandboxing**, our goal is to prevent malware from installing itself on your computer or using what happens in one tab to affect what happens in another.
- So, for each of these processes we've stripped away all of their rights.
- They can compute but they can't write files to your hard drive or read files from sensitive areas like your documents or desktop.
- Or as the sandbox team put it --
- -- we've taken this existing process boundary --
- -- and made it into a **jail**.

### P27

- That means no watching you type your credit card number.
- No interacting with mouse operations.
- No reading your tax returns.
- No telling windows to run an executable at start-up.
- Something bad could be running in this tab --
- -- but as soon as you close it, it's gone.
- No effect on your machine and no effect on other processes.
- The perimeter of the sandbox is largely based on permissions.
- Vista uses a modified version of the Biba security model which has three levels.
  - Very trusted.
  - Somewhat trusted.
  - Not trusted at all.

### P28

- This level is for backup systems, programs that update, etc.
- This level is for everything the user runs normally. Notepad, solitaire, calculator...
- Reading is allowed from **low** to **high** --
- -- but **writing** is allowed only from **high** to **low**.
- Typically, applications receiving and processing data from the internet are split into the two lower levels.
- The problem is that unlike the high level, there is a lot of sensitive info here --
- -- that **this** level should **not** be allowed to read!

### P29

- In our model, there's the **user**, and there's the **sandbox**, and any communications **must** be initiated by the user.
- This side can **reply** but it has no way to access anything that isn't explicitly provided by the user.
- We can do this because all our code is new. We're writing the code, so Google Chrome has full control over this.
- With one exception -- **plugins**.
- Because webpages are **more** than just **HTML** and **JavaScript**.

### P30

- In terms of permissions on the system, Google Chrome's renderer may run at very low privileges, but there are **plugins** that run at the same level or even higher than the browser.
- Plugins have capabilities that aren't public standards, so we can't sandbox these yet.
- Though with some small changes on the part of the plugin makers, we can get them to run at a lower privilege which would be much much safer.
- And meanwhile, we have a huge surface area reduction in vulnerability, from all **this** --
- -- to **this**.

### P31

- When a plugin combines with HTML and JavaScript, it all runs in the same process.
- If any part crashes or starts corrupting memory, they're **all** hosed.
- So I worked on ripping plugins out of the rendering process and putting them in a separate process all their own.
- In that way, the rest of the page can still be sandboxed, even if the plugin can't be.

### P32

- Sandboxing can help protect users from malware, but they can still be tricked by **phishing**.
- In a typical phishing scheme, an attacker sends out mail saying, "I'm your bank, your account is compromised, give me your SSN so I can verify, ect..."
- Then they send users to a nearly exact copy of their bank's website and start stealing their information.
- A lot of these get taken down somewhere around 24 hours --
- -- though a few last up to 7 days or more.
- The hard part is finding them close to **time zero**.

### P33

- Google Chrome is continually downloading lists of harmful sites, one for phishing, one for malware.
- If you go to a website that matches the list, you'll get a warning.
- We've made this service freely available. We're happy to give it away. It's a public API.
- There's a second list of malware websites. Websites where a ton of bad things might happen to your computer, just on arrival.
- When we discover malicious content, we notify the owner of a website, who usually wasn't intending to be malicious, and they can take this information and clean up their site.
  - Ahem
  - Oh, sorry!

### P34 - Part Five

- Gears, Standards and Open Source
- Another thing we built into Google Chrome is **Gears**.
- Gears basically adds an API to your browser -- an extension that improves its capabilities.
- From my perspective, Google Chrome and Gears are entering the web from two directions.
- The browser project is an effort to make the web better for **users**.
- The Gears team wants to make the web better for **developers**.

### P35

- There are a lot of limitations to the kinds of applications that you can build today with web browsers, and the subset of things you can do is different for each browser. If **one** browser has a cool feature, that doesn't help --
- -- it has to exist across **all** browsers in order for developers to use it.
- Gears is trying to improve the base functionality of all browsers, including Google Chrome.
- Whatever the advantages of building native apps over web apps, we want to build those enhancements through Gears --
- -- and help them make their way into new standards across the web.

### P36

- So, open standards are one way to help all browsers get better.
- The team has also done some interesting things with speed, stability and the UI, like the new tab page.
- Some of these might become standards --
- -- some might **not**.
- **But** --
- -- since it's open source --
- -- other browser developers can take what they want out of it.

### P37

- They don't have to pay us. They don't have to ask our permission.
- They don't have to share patches or report bugs. \*
  - \* Though, if they like, we have systems in place for that.
- But they can **build** on what we've done and bring their own creativity to it.
- Sure, we **could** ship a proprietary browser and hold it in.
- But google **lives** on the internet.
- It's in our interest to make the internet better and without competition we have stagnation.
- That's why we're open sourcing the whole thing. We **need** the internet to be a fair, smart, safe place.

### P38

- As excited as we are about building Google Chrome, it's important to help **all** browsers become more powerful --
- -- to keep evolving with the web and continuing to build a **solid foundation** for modern web applications.
- We own a great debt to other open source browser projects -- especially, **Mozilla** and **WebKit**.
- This is our contribution, and we hope people will take some of these ideas, too; challenge them, build on them, and keep moving the web **forward**.
