yal.js, the micro size JS loader
================================


about
-----

yal.js is a truly tiny library partially inspired by both [LAB.js](http://labjs.com/) and [yepnope.js](http://yepnopejs.com/) projects.

In about 0.6Kb minzipped yal does really few things but it does them right:

  * accepts one to N string arguments to be loaded asynchronously
  * if one or more argument is a function, it appends to the loading queue the returned string value, if any
  * manages loading queue through one or more `yal.wait()` method calls
  * manages both parallels and serialized downloads or, if necessary, can handle everything parallel


compatibility
-------------

yal.js has been tested against many IE, Chrome, Safari, Firefox, Opera, Webkit, and respective mobile versions, in different platforms, and without problems.
You can check results against all browsers, if it's green, it passed, if it's red, it failed (no reds), if it's white, something went wrong with the testing tool and not with yal.js or JavaScript was disabled:

[test page](http://www.3site.eu/yal/wait.html)

[crossbrowsertesting](http://crossbrowsertesting.com/users/32945/screenshots/z30790cabfdc044b8e50/public)

[browsershots](http://browsershots.org/http://www.3site.eu/yal/wait.html)


example
-------

    yal
        .script("1.js")
        .wait(function () {
            init1();                // core dependency with initialization
        })
        .script("2.js", "3.js")     // no need to call script N times
        .wait()                     // since 4.js, if present,
                                    // depends on 2 and 3
        .script(function () {
            if (                    // features detection example
                /iOS/i.test(
                    navigator.userAgent
                )
            ) {
                return "4.js";
            }
        })
        .wait(function () {
            initAllBut1();          // ready to go
        })
    ;


why another JavaScript loader
-----------------------------

The reason is quite simple: I don't need everything LAB.js has to offer, I like yepnope simplification and features detection purpose, but I still go nuts when I cannot grab a portion of a library, aka: only what I really need. LAB.js is surely complete and quite standard, but as is for jQuery library, it must include a lot of code that is redundant or superfluous beceause never executed in my daily basis target browsers. I also noticed developers are basically using only `.script()` and `.wait()` and I was not interested into all other possible features or, even worse, extremely uncommon edge cases.

Moreover, in my past I have created many loaders, related to JS or not, so it was both fun and trivial to create this tiny script which works, at least for what I usually need, and it does not add relevant code size to whatever project you are working on.


why chose yal.js against others
-------------------------------

I am not selling anything here and I would like to be as clear as possible about yal and its potentials against other solutions.

  * initial loader size may be irrelevant if you download "32 JavaScript files" after. However, if you are downloading a bounce of stand alone libraries or shims without needing to `.wait()` in between, the simplified parallel approach plus initial size of yal.js will surely bring you a performances boost
  * the default version of yal is based on consolidated behaviors across all browsers. While the hybrid parallel/serial download solution may not be the fastest one out there, it is extremely reliable. If you want to be sure that a `.wait()` call will have already downloaded, interpreted, and parsed every assigned script before, yal.js is what you are looking for since it does not contain any unpredictable timer behavior (e.g. `setInterval()` or `setTimeout()`)
  * yal.js size is basically irrelevant and it can be embedded in whatever project or closure. Even if you are building a single JS production file, yal.js could be just exactly what you are looking for if you still need a JS loader for your main lazy/async/loading application logic


why not chose yal.js against others
-----------------------------------

Well, no software is perfect so neither is yal. Here a list of reasons you may consider before you decide to use or not this library

  * your bootstrap strongly depends on `.wait()` calls due dependencies. In this case, while the forced parallel download version will probably work without problems, other loaders such LAB.js may have adopted more reliable/standard solutions to this specific problem accordingly with most modern browsers capabilities. In this case I would suggest a performances test before making any decision but I cannot guarantee the forced parallel download strategy adopted in yal will be 100% successful cross platform.
  * you need extra/edge cases functionalities but you want the smaller loader as well. I am sorry, features never come for free, size speaking, so unless it's not truly irrelevant, I don't think I will change yal.js purpose which is to load, without unnecessary overhead for most common use cases, whatever extra or external script you need for your project.


yal is both serial and fully or partially parallel
--------------------------------------------------

The basic/default version of yal uses parallel downloads in between `.wait()` calls. This means that every time there is such call, scripts defined after won't be downloaded until the precedent `yal.wait([optionalFunc])` has been executed.

There are many tricks to trigger a script preload but unfortunately these techniques are not completely cross platform or cross browser yet.

For this reason the basic version of yal uses a complete KISS approach: the best compromise between performances, reliability, and control over loaded code execution ... also without any waste of precious bytes.


more about yal.js and forced parallel downloads
-----------------------------------------------
Every assigned script will create an object or an Image that will be appended on the DOM in order to trigger the browser download. At that point when it's time for the current script to be downloaded there are two options:

  * the object/Image has already downloaded the script but not interpreted it, most likely the browser will reuse the cache to re-grab the file rather than perform twice the same download ( 'cause if it does it's kinda bad ... )
  * the object/Image is still downloading the script, most likely the network layer will be smart enough to do not ask twice the same file so the script will benefit from already downloaded content, taking less time to be ready
Whenever this technique is good or not, it seems to work pretty damned well!

It must be said it's based on a sort of browser network layer hack ... it won't hurt but there is no guarantee that it will produce same advantages in the future. Well, in that future I will update the script using de-facto standards to preload scripts: YAGNI ;-)