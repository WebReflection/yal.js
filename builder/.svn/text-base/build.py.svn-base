# JSBuilder http://code.google.com/p/javascript-builder/

copyright = '(C) WebReflection'
max_js = 'build/yal.js'

import JSBuilder

# forced parallel downloads version
print ("")
print ("-------------------------------")
print ("|  forced parallel downloads  |")
print ("-------------------------------")
JSBuilder.compile(
    copyright,
    max_js,
    'build/yal.parallel.min.js',
    [
        "yal.intro.js",
        "yal.preload.js",
        "yal.js",
        "yal.outro.js"
    ]
)
print ("----------------------")
print ("")

# partial parallel downloads versionprint ("")
print ("-------------------------------")
print ("| partial parallel downloads  |")
print ("-------------------------------")
JSBuilder.compile(
    copyright,
    max_js,
    'build/yal.min.js',
    [
        "yal.intro.js",
        "yal.nopreload.js",
        "yal.js",
        "yal.outro.js"
    ]
)
print ("----------------------")
print ("")

# let me read the result ...
import time
time.sleep(2)