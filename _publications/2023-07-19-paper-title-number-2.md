---
title: "Plunk CMS authenticated RCE"
collection: publications
permalink: /publication/2023-07-19-paper-title-number-2
excerpt: 'This paper is about CVE-2022-26965, AKA Plunk theme RCE'
date: 2023-07-19
venue: 'Journal 2'
paperurl: ''
citation: 'https://www.youtube.com/watch?v=sN6J_X4mEbY'
---
Hello world
This paper is about CVE-2022-26965, an authenticated remote code execution exploit for Plunk content management system
------

I would first like to thank Ashish Koli for his documentation online about the vulnerability and exploit proof of concept.
Pluck has many CVEs, though I was unable to recreate CVE-2023-25828 (RCE via Pluck's "Albums" module) on my system, which had left me in a bad mood. This is what led me to approaching the RCE via the Themes module for Pluck.

How does CVE-2022-26965 work?
------

It's actually very simple, and if you watch Ashish's video [here](https://www.youtube.com/watch?v=sN6J_X4mEbY) where he executes the exploit by hand,
you will see how easy it is. Pluck's blind faith in Themes means an authenticated user can practically re-pack any theme with any malicious php file and trigger it from the associated location.
My aim for this project was to build off of Ashish's proof of concept and automate the preparation stages as well as the exploitation of the CMS.

This means my script must:
1. Unpack the legitimate theme (which will be .tar.gz)
1. Inject the shell or other malicious code into the theme
1. Repack the theme as .tar.gz

After staging the attack, we can carry out the exploitation stage:
1. Login as user on Pluck CMS
1. Install malicious theme
1. Locate malicious theme and execute

Shall we take a look at the script?
------

Luckily for me, I could build off of the initial exploit POC found [here](https://github.com/shikari00007/Pluck-CMS-Pluck-4.7.16-Theme-Upload-Remote-Code-Execution-Authenticated--POC).
I had to make some initial changes to the original POC since I couldn't get it to work.
Then, I began to work on automating the first stages listed above. Another thing
I realised is that I couldn't trigger the injected PHP to run by navigating to /data/themes such as in the example - but instead by going to the theme
uninstall page (which triggers the injected info.php to run).

So the first piece of code we need to look at for this stager is the function injectTheme() (below)

![injectTheme](/images/pluck1.JPG)

As you can see I use shutil to unpack the theme at first but the tarfile module to repack it.
This is simply because shutil.make_archive has a tendency to include all of the folders before the chosen item to package. It is great for unpacking but tarfile saved me a great pain when it comes to repackaging.

It then follows the same arc as the original POC, generating a cookie and authenticating the user using the requests module. After the user is authenticated, we can upload the shell.
This section needed some work, because at first it wouldn't upload the file. After lots of rigorous testing with Burp Suite, I discovered there were many discrepancies between the crafted request and a legitimate request. This mainly comes down to how the headers and file are formatted before contacting the web page. After tinkering with the headers I realised that in the initial POC, Ashish reads the raw binary into the request as the payload. This goes against standard formatting and means you cannot
submit certain data that should be in the packet payload.
![fileUpload](/images/pluck2.JPG)

To solve this I followed the [requests official documentation](https://docs.python-requests.org/en/latest/) and learned
there is actually a field for this known as "files" that can be supplied within requests.post(), and allow us to supply data that should be in the payload.
![fileUpload2](/images/pluck3.JPG)

After finding this, I finally had all of the stages working.
I also changed some small things from the initial script, such as switching from using sys.argv[] to argparse for parsing variables from the command line.
This means now we can execute the whole stage like this:
![Output](/images/pluck4.JPG)

Exploitation and notes
------

If we go to the URL stated in the output, we can see we have our shell. Albeit rendered slightly wrong, but functioning
![shellOutput](/images/pluck5.JPG)

Since all stages of the attack are automated now, the only data you need before carrying out an attack is:
+ Valid password for Pluck CMS
+ PHP shell saved to your machine
+ Pluck theme saved to your machine, available [here](https://github.com/pluck-cms/themes)

Conclusion
------

CVE-2022-26965 is not wildly complicated, although issues like this are not to be overlooked within CMS' etc. They are still potentially dangerous attacks that can put the data of yourself or others at risk.
Again thank you to Ashish Koli for the initial POC which allowed me to build the full stager. I will release my version of the POC to my github shortly.

(NOTE: Source code is now available at https://github.com/j-4ck/PluckCMS)

Thanks for reading, I am available at jackp0tter@protonmail.ch
