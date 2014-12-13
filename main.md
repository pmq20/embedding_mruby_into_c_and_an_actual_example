# Embedding mruby into C and an actual example
author
:   P.S.V.R
（阿里巴巴 － 孝达）

allotted-time
:   1h

theme
:   debian

# mruby

![](mruby.png){:relative_height='80' reflect_ratio='0.5'}

m = _m_ atz' + e _m_ beddable + _m_ inimalistic + _m_ odular

# why mruby?

![](mruby-ja2.png){:relative_height='100'}

# purpose: boost productivities of embedded dev.

![](mruby-ja1.png){:relative_height='100'}

# who is using it?

![](mruby-product2.png){:relative_height='80'}

* 富士電機（杭州）軟件有限公司

# who is using it?

![](mruby-product1.png){:relative_height='80'}

* IIJ's router products c.f. https://github.com/iij/mruby


# who _should have been_ using it?

![](WP_20141105_06_03_25_Pro.jpg){:relative_height='80'}

* could have been more energy-efficient and inexpensive and easy-to-maintain via mruby...

# Actually...

* mruby is not only for the EMBEDDED WORLD
* mruby is for the ENTIRE C WORLD
* just knowing C is enough to get you going, you don't necessarily need embedded devices or EE degrees

# who _is_ also using it?

* `mod_mruby`:

https://github.com/matsumoto-r/mod_mruby

* `ngx_mruby`:

https://github.com/matsumoto-r/ngx_mruby

# who _is_ also using it?

* `php-mruby`:

https://github.com/chobie/php-mruby

* `ab-mruby`:

https://github.com/matsumoto-r/ab-mruby

# who _is_ also using it?

* `go-mruby`:

https://github.com/mitchellh/go-mruby

# "反主为客"

* MRI: 主Ruby 客C
* mruby: 主C 客Ruby

# "反主为客"
* MRI:

only _1_ VM per process, then add features to the VM

* mruby:

_N_ VMs per process (could be one VM per thread, no need to lock), then call VMs from C

# how to embed

* open `mrb_state`
* define business-domain ruby-classes and methods in the compile-time
* dynamically invoke ruby-classes and methods in the run-time
* close `mrb_state`

# "mrb_state"

* NO GLOBAL VARIABLES, everything are inside "mrb_state"
* refereed to by almost all functions
* [_demo_] see "typedef struct mrb_state"

# "mrb_value"
* could store pointer, integer, float, etc
* set: `mrb_fixnum_value`, `mrb_float_value`, ...
* get: `mrb_fixnum`, `mrb_float`, ...
* [_demo_] see "typedef struct mrb_value" ( x2 imple.)

# "mrb_funcall"

    obj = mrb_funcall(mrb, obj, "inspect", 0);
{: lang="c"}

* call into instance method of a ruby-object
* e.g.

# there is no file I/O
* the underlying machine might not have a file system at all
* it's a seperate module _mruby-io_, if you want it
* event STD I/O is configurable
* [_demo_] see "#ifdef ENABLE_STDIO"

# "mrb_define_module"

    mrb_ofpsvr = mrb_define_module(mrb, "Ofpsvr");
{: lang="c"}

* define business-domain ruby-classes
* e.g.

# "mrb_define_module_function"

    mrb_define_module_function(mrb, mrb_ofpsvr, "uid", ofpsvr_uid, MRB_ARGS_REQ(1));
{: lang="c"}

* define business-domain ruby-methods
* e.g.

# "mrb_load_string"

    mrb_load_string(mrb, "puts \"いまは#{Time.now}です。\"");
{: lang="c"}

* define business-domain ruby-methods
* e.g.

# Exception Handling

    if (mrb->exc) {
      mrb_value exception = mrb_obj_value(mrb->exc);
      mrb->exc = 0;
    }
{: lang="c"}

* do not forget to check `mrb-exc` when running unpredictable code!

# an actual example

* [_demo_] https://github.com/pmq20/ofpsvr
