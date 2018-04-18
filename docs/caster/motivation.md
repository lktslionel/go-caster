###### DOCS / CASTER
# Motivation

This document describe why, for who, how we build `go-caster`.

## Background

When we are writing applications today, we use a lot of log statements to express what our applications are doing.
Sometimes we are either debugging or instrumenting stuffs by outputing some logs to the console or may be onather destination. Thus, if the internal modules of our application give information about them through a log file or a log output. Its nearly impossible to know from one module, what is going on another module. 

I'm not saying we always need to that, but when it is the case, with our current application architecture, we can't do it easily. It must be easy to let any part of our application be awared of what other parts are doing; and may be act in response of what is happening in these parts.