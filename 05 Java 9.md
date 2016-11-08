# Java 9

## Modules
base: java.base (implicit, contains java.lang, ...)

What's a module:
* collection of code (packages) and data
* has a name
* tells what it needs (requires)
* tells what it provides (exports)

Classpath goes to hell. Now we can have module hell.

Now we'll use modulePath (-mp)

To run a Java app and specify the module path: `java -mp somewhere/mlib -m com.agtiledeveloper.user` (specified the module path and the module name)
The above assumes that the module was created as a jar in the mlib folder and has a main class (specified using `--main-class ...`)

When using a module, you have to declare what you want to use in that module (the requires)
Also the module we use must have a module-info.java:

```
module com.agiledeveloper {
    exports com.agiledeveloper.math;
}
```

On the above we export some package

```
module com.agiledeveloper.user {
    requires com.agiledeveloper;
}
```

On the above, we "import" what we are going to use.


You must pay attention when compiling the code, but also when running the code. At runtime we can get access violations, just like ClassDefNotFound errors..

So both the compiler and the runtime enforce access checks for modules.

## Modules and visibility
* readability: a module "reads" the modules it depends on
* exports
* public is not the same any more
* public + exports become visible outside the module
* public with no exports is not visible outside the module

Use jdeps to examine module dependencies: `jdeps -genmoduleinfo something/*.jar`

## Implied readability
To "re-export" dependencies you have to client modules:
```
module com.agiledeveloper.user {
    requires transitive com.agiledeveloper;
}
```

This passes on what you use to your users.

## Module versioning
For now, the dependencies don't use the module version.
Versions can be given to modules: jar -c -f ... --module-version 1.0.3 ...

`jar -p -f bidule.jar`: provides info about the jar (e.g., module version number, requires, exports, conceals, etc)

Can't have more than one of the same module in the module directory!! (e.g., can't have module v1 and v2 and have Java pick up the correct one)

## Targeted linking
Creating a targeted executable: use jlink: `jlink --modulepath mlib1:mlib2 --addmods mlib1 --addmod myapp`

jlink takes your modules and creates a java binary executable

`java -listmods` on a java executable created using jlink: contains only the modules we've chosen to add/use.

Creates very compact binaries (platform specific!). Benefit: no need to install the JVM on the target machine; just install the binaries generated by jlink!