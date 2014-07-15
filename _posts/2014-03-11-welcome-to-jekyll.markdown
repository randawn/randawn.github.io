---
layout: post
title:  "Welcome to Jekyll!"
date:   2014-03-11 02:13:13
categories: jekyll update
---

piece of `code`:

{% highlight python %}
def hello():
  print "hello world!"
hello()
#greeting :)
{% endhighlight %}

Check out the [awesome site][site]

[site]: https://randawn.github.io

on the verification
===================
DV(design verification) is the process to verify the test. usually a test env is made up by a `bench` and a `DUT`.
there are ways to do verification.
1. **verilog PLI**  
the simplest method is using `verilog` for bench, using verilog we can write simple bench, and through PLI, we can invoke c routines from verilog that can extend the ability of bench.  
but describing anything other than HW is difficult in verilog. so for complex design we need other method. using the `PLI`, we can link programming languages into verilog for test generation and modeling.
2. **INDUSTRY(verify language)**  
the industry has invent some language for verification purpose, these language have add features for describing 'task' and synchronization primitives, and hide underlying PLI interface.  
these language include `Vera`, `Specman e`, `System C`.  
3. **OTHER(general language)**
* `C++`
    + [truss/teal](www.trusster.com)
        >Truss is a fully implemented verification framework, similar to other   modern verification methodologies but open source. Teal is a lower       level support library providing necessary functionality like unified     logging, randomization, threading, PLI connections (to all major         simulators) and much, much more.
    + [ANVIL](anvil.sourceforge.net)(ANother Verilog Interaction Layer)
* `Java`
    + [JOVE](jove.sourceforge.net)
>Jove is a set of Java APIs and tools to enable Verilog hardware desig    n verification using the Java programming language.
It contains components that accomplish the following: 
Verilog simulator interaction (via PLI 2.0, aka VPI)
standalone behavioral simulation (i.e. a discrete event simulator)
thread and event synchronization
design verification abstractions (e.g. clock-relative signal access,     mailboxes, semaphores)
constraint-based randomization
Verilog shell generation
* `Perl`
    + Verilog::Pli(no longer active)
* `Ruby`
    + Ruby-VPI
>Ruby-VPI is a library that lets Ruby programs access the entire IEEE 1364-2005 Verilog VPI interface supported by major Verilog simulators today. It also serves as platform for unit testing, rapid prototyping, and systems integration of Verilog modules through Ruby.
* `Python`
    + [PyHVL]( pyhvl.sourceforge.net )
>provide an object-oriented framework over the basic PLI interface.
    + MyHDL
    + ooHDL
    + [Python PLI](http://tsheffler.com/software/python/)
* `Script`(namely Perl Python Tcl/Tk mostly)
    + [ScriptEDA](www-cad.eecs.berkeley.edu/~pinhong/scriptEDA)(PinHong Chen)
>using SWIG to map the VPI interface to Python.
    + [ScriptSim](nelsim.com)(Dave Nelson)
>run python as a separate process and communicates with Verilog via pipe. The communication protocol allows object values to be transferred back and forth, and allows Python to synchronize with the simulator and schedule callbacks. ScriptSim has a nice Tk/TCL interface as well.

### APPENDIX
* PLI
>Programming Language Interface of Verilog HDL. 
invoke C/C++ function from Verilog code(system call).

    + acc(access routines)
>c routines that have access to the internal data structure of verilog simulator(both read/write). classified into 6 categories:
        * fetch: read
        * handle: like pointer in C can point to any kind of object in the design database
        * modify: write
        * next: 
        * utility: misc
        * Value Change Link: monitor the value changes of selected objects.
    + tf(task and function routines) 
>utility rotines
    + vpi(Verilog Procedural Interface)

* SWIG
> mapping and linking C libraries into interpreters
    + C access to Python interpreter(embedding)
    + Python access to C(extension writing)

**how it works **
+ wrapper function(glue layer)
    * convert args from Python to C(PyArg_ParseTuple)
    * return results in Python-friendly form(Py_BuildValue)
+ methods table 
+ initialization function
    * register wrappers with python,
    * when import an extension module, an initialization function is called to register new methods with the Python interpreter.

**compile a python extension**
+ dynamic loading
    * extension module is compiled into a shared lib or DLL(compile build option)
    * Python loads and initializes the module on the fly when meet 'import'
+ static linking
(C Extensions + Python =compile=> custom Python)
* the extension module is compiled into the Python core and become a new 'built-in' module
* typing 'import' simply initializes the module
** extension building tools **
hand write is extremely tedious and error-prone
+ stub generators(modulator)
+ automated tools(SWIG GRAD bgen)
+ distributed objects(ILU COBRA COM)
+ Extensions to Python itself(Extension classes, MESS)
+ Boost.Python ctypes SIP pyfort Pyrex

# SWIG(Simplified Wrapper and Interface Generator)
a compiler that turn ANSI C/C++ declarations into scripting language interface.
ANSI C/C++ declarations(.h .c .i) =SWIG=> python/perl/tcl/...(_wrap.c .py)
user import .py, _wrap.c is compiled in a shared library(.pyd).
+ arch
*.i -> Preprocessor -> C/C++ parser -full parse tree-> analysis modules -annotated parse tree-> code generator

