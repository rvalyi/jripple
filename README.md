JRipple - JRuby Rips Kettle
====

<table>
    <tr>
        <td><b>BY</b></td>
        <td><a href="http://www.akretion.com" title="Akretion - open source to spin the world"><img src="http://sites.google.com/site/assetserversite/_/rsrc/1257126470309/home/akretion_s.png" width="228px" height="124px" /></a></td>
        <td>
so far JRipple is a rewritte org.pentaho.di.trans.steps.scriptvalues_mod to use the JSR223 instead of old Rhino specific interface
        </td>
    </tr>
</table>


Why?
------------

Kettle + Scripting rocks. The script abililty of Kettle is among the most powerfull features, very handy to do flexible operations.
However, Kettle was limited to Javascript Rhino engine.


What works?
------------

Thanks to my rewrite it's now possible to use ['ANY' of the JSR223 languages: https://scripting.dev.java.net/](https://scripting.dev.java.net/), including JRuby, Jython, Groovy...
Secret goal: have a flexible killer [OpenERP](http://openerp.com/) Kettle connector that uses the [OOOR Connector](http://github.com/rvalyi/ooor) under the hood for unlimited flexibility.
See our [TerminatOOOR](http://github.com/rvalyi/terminatooor) Kettle plugin to extract and load data from/to OpenERP.
I tested reading and writing datas in Kettle with the following types, with both JRuby and Jython: Boolean, String, Integer, Number, Date. I'm not sure it works for the other types.
JRuby example:
<img src="http://docs.google.com/File?id=ajb639cjf9fb_10cvmpcsdq_b" />

Jython example:
<img src="http://docs.google.com/File?id=ajb639cjf9fb_11f6drj5cp_b" />


Requirements
------------

- Only tested with Kettle trunk (v4 dev)
- requires Java 6 minimum
- not yet packaged (waiting for a position from Pentaho: will they integrate my contrib (excellent occasion to clean up that 'not optimal code' or should we pakage it as an extra module), so currently you should recompile it yourself to use it.


How to compile
------------
- Get the jripple source using git or the download button on this page
- Get the Kettle trunk source code using SVN: http://community.pentaho.com/getthecode/
- Open the Kettle project with Eclipse
- Replace original classes in src/org/pentaho/di/trans/steps/scriptvalues_mod the patched classes 
- Compile using the command: ant

Don't forget to put a proper  jruby-complete.jar in extlib before running to have JRuby


Gotchas
------------

- I didn't re-activated the AddedValuesFunctions for now, it's mostly refactored already, but I don't know how to add them as global methods in the scripting scope using only JSR223 stuff, I propose to add them
as method of some object like 'Utility.foo', the decision is up to Pentaho.
- see TODO AKRETION in code: a few things not implemented or unsure
- script detection is based on step name extension rather than GUI element because adding a GUI element was too much hassle in crappy Kettle GUI code, meaning a JRuby script should be name script_name.jruby, for jython: script_name.jython
- loading gems can be done only if jruby-complete is in the path (extlib for instance) and if GEMS are in jruby-complete.jar!/META-INF/jruby.home/lib/ruby/gems/1.8
- I can't understand how "startup scripts" are expected to work in Kettle the original scripting step: however we definitely need a possibility for scripts to write in the global transformation scope (and not only access the line by line scope), that should enable to do something only once for the whole transfo and then reuse it for each line.


Hints
------------
- make sure you pasted all the required jar in your Kettle libext directory before trying to use your favorite scripting langage, for instance for Jython I had to paste here: antlr-2.7.7.jar  antlr-runtime-3.1.3.jar  asm-commons-3.1.jar  jython-dev.jar   antlr-3.1.3.jar  asm-3.1.jar  asm-util-3.1.jar
- if using JRuby, make sure to paste jruby-complete.jar in your Kettle libext directory. If using some gems, remember that JRuby packages gems in jruby-complete.jar/META-INF, see [http://kenai.com/projects/jruby/pages/JavaIntegration](http://kenai.com/projects/jruby/pages/JavaIntegration). Finally remember that a jar is just a zip, so you can re-package easily your required gems in jruby-complete to have them in Kettle.


Useful Resources
------------

[http://java.sun.com/developer/technicalArticles/J2SE/Desktop/scripting/](http://java.sun.com/developer/technicalArticles/J2SE/Desktop/scripting/)
[http://kenai.com/projects/jruby/pages/JavaIntegration](http://kenai.com/projects/jruby/pages/JavaIntegration)
[http://www.mozilla.org/rhino/tutorial.html](http://www.mozilla.org/rhino/tutorial.html)
[http://www.mozilla.org/rhino/apidocs/org/mozilla/javascript/Context.html](http://www.mozilla.org/rhino/apidocs/org/mozilla/javascript/Context.html)
[http://javalandscape.blogspot.com/2008/12/scripting-in-jdk6-jsr-223-part-1.htm](http://javalandscape.blogspot.com/2008/12/scripting-in-jdk6-jsr-223-part-1.html)