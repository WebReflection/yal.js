/*!
(C) WebReflection
*/
/**@license (C) WebReflection
*/
var yal = function (window) {//"use strict"; // it works on strict environments too!

    /** DESCRIPTION
     *
     * yal does two things:
     *  - accepts 1 to N script arguments to load asynchronously
     *  - manages loading queue through wait calls
     *
     * yal
     *   .script("1.js")
     *   .wait(function () {
     *       init1();               // core dependency with initialization
     *   })
     *   .script("2.js", "3.js")
     *   .wait()                    // since 4.js depends on 2 and 3
     *   .script("4.js")
     *   .wait(function () {
     *       initAllBut1();         // ready to go
     *   })
     * ;
     *
     * yal is compatible with every browser I have been able to test.
     * see http://3site.eu/yal/ for more details
     *
     */
    

    function preload(queue){return queue;}


    
    // some logic in alphabetic order
    function checkIfDetection(queue) {
        for (
            i = 0;
            i < queue[length];
            typeof queue[i] == "function" && (queue[i] = queue[i]()),
            ++i
        ); //:lint it's meant to be like this
        return queue;
    }
    
    function loadNext() {
        do  //:lint it's meant to be like this
            (src = queue[shift]()) && loadScript(src)
        ; while (src != wait[0]);
    }
    
    function loadScript(src) {
        script = documentElement[insertBefore](
            document[createElement]("script"),
            documentElement[lastChild]
        );
        onload in script || !(readyState in script) ?
            script[onload] = script[onerror] = onScriptLoad :   //:lint it's meant to be like this
            script[onreadystatechange] = onScriptReadyState
        ;
        script.type = "text/javascript";
        loading[push](script.src = script._ = src);
    }
    
    function removeFromPreloading(src) {
        for (
            i = preloading[length];
            i-- && preloading[i]._ != src;
        );   //:lint it's meant to be like this
        ~i && removeNode(preloading.splice(i, 1)[0]);
    }
    
    function removeNode(script) {
        (parentNode = script.parentNode) && parentNode.removeChild(script);
    }
    
    // some event in alphabetic order
    function onScriptLoad() {
        script=this;
        script[onreadystatechange] = script[onload] = script[onerror] = null;
        removeNode(script);
        removeFromPreloading(script._);
        loading[shift]();
        if (!loading[length] && wait[length]) {
            wait[shift]();
            callback = callbacks[shift]();
            while (callback[length])
                callback[shift]()() //:lint it's meant to be like this
            ;
            loadNext();
        }

    }
    
    function onScriptReadyState() {
        "loaded" == (script = this)[readyState] && onScriptLoad.call(script);
    }
    
    var
        callback, current, i, parentNode, script, src,
        createElement = "createElement",
        lastChild = "lastChild",
        insertBefore = "insertBefore",
        onreadystatechange = "onreadystatechange",
        onerror = "onerror",
        onload = "onload",
        readyState = "readyState",
        push = "push",
        shift = "shift",
        length = "length",
        document = window.document,
        documentElement = document.documentElement,
        yal = {
            script: function () {
                queue[push].apply(queue, preload(checkIfDetection(arguments)));
                wait[length] || loadNext();
                return yal;
            },
            wait: function (callback) {
                current = queue[length] ? queue : loading;
                if (current[length]) {
                    for (
                        i = wait[length],
                        src = current[current[length] - 1];
                        i-- && wait[i] != src;
                    ); //:lint it's meant to be like this
                    ~i || (i = wait[push](src) - 1);
                    callbacks[i] || (callbacks[i] = []);
                    callback && callbacks[i][push](callback);
                } else {
                    callback && callback();
                }
                return yal;
            }
        },
        queue = [],
        loading = [],
        preloading = [],
        wait = [],
        callbacks = []
    ;
    

    return yal;
    
}(this);