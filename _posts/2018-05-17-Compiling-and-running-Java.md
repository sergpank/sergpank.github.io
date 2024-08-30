---
layout: post
title: Compiling and running Java appx via CommandLine
date: 2018-05-17
---  

# We have 3 Java files :

*com.main.Main.java*
```java
package com.main;

import com.bar.Bar;
import com.foo.Foo;

public class Main {

    public static void main(String[] args) {
        new Foo().pringFoo();
        new Bar().printBar();
    }
}
```

*com.foo.Foo.java*
```java
package com.foo;

public class Foo
{
  public void pringFoo()
  {
    System.out.println("FOO");
  }
}
```

*com.bar.Bar.java*
```java
package com.bar;

public class Bar
{
  public void printBar()
  {
    System.out.println("BAR");
  }
}
```

# Copile:  
```bash
# javac [options] [source files]:
javac -d <outpud-dir> <file-name>.java 
# Output directory should exist, all inner directories will be created automatically

# You are in the root of the project
# You want to compile all the classes that are necessary for Main.java to run
# Use -sourcepath flag!
# don't forget to create target/classes directory.
javac -d target/classes -sourcepath src src/com/main/Main.java
# Run with:
# java com.main.Main
# OR
# java com/main/Main

# If you need to compile all *.java files quick and dirty:

# Linux
$ find . -name "*.java" > sources.txt
$ javac -d target/classes @sources.txt

# Windows
> dir /s /B *.java > sources.txt
> javac -d target/classes @sources.txt
```

# Launch:  
```bash
# java [options] class [args]
java -DmyProp=myValue MyClass x 1

# In our case we have to cd to "target/classes" dir and execute the following command:
java com.main.Main
> FOO
> BAR

# If you are using jar depencies add ClassPath parameter starting with cur dir .:jar1:jar2:...
java -cp .:dependency.jar com.main.Main
...
```

Some nice tutrial - <https://dev.java/learn/jvm/tools/core/jar/>
