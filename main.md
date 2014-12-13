# Embedding mruby into C and an actual example
author
:   P.S.V.R

allotted-time
:   40m

theme
:   debian

# mruby

![](mruby.png){:relative_height='80' reflect_ratio='0.5'}

m = 

# purpose: boost productivities of embedded dev.

![](mruby-ja2.png){:relative_height='60'}

* from http://www.chugoku.meti.go.jp/topics/kikaku/pdf/sangyokyoso/2014sakutei/chapter3-1-6.pdf

# purpose: boost productivities of embedded dev.

![](mruby-ja1.png){:relative_height='100'}

# Well I think...

* mruby is not only for the EMBEDDED WORLD
* mruby is for the ENTIRE C WORLD
* it doesn't mean you can't be a mrubist without having embedded devices or EE degrees

# "反主为客"

* MRI: 主Ruby 客C
* mruby: 主C 客Ruby

# "反主为客"
* MRI: only one VM per process, then add more features to the VM
* mruby: many VMs per process, then call VMs from C

# "mrb_state"

* so, NO GLOBAL VARIABLES at all
* everything are inside "mrb_state"
* refereed by almost all functions
* [_demo_] see "typedef struct mrb_state {"

# why is there no file I/O
* the underlying machine might not have a file system at all!

# event STD I/O is configurable
* [_demo_] see "#ifdef ENABLE_STDIO"

# MRI: too much POSIX

...

