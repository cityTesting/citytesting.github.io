---
layout: post
title: Create function in zsh to change Java version
categories: [json, java]
---

Let's assume that we have installed zsh and two versions of Java on our computer (Java 17, Java11). To conveniently change the Java version we can create a function in the .zsh file and just run a command that will be equal to the name of the function and we will be able to change it.  

Normally, when installing Java, we add to the PATH environment variable this `PATH=$JAVA_HOME/bin`. This means that depending on the value of JAVA_HOME we will use one version or another.  

Therefore we would only have to:
- Edit .zsh file.
- Add at the end the following:
{% highlight java %}
function java11(){
export JAVA_HOME='/Library/Java/JavaVirtualMachines/jdk-11.0.12.jdk/Contents/Home'
java -version
}

function java17(){
export JAVA_HOME='/Library/Java/JavaVirtualMachines/azul-17.0.2/Contents/Home'
java -version
}

{% endhighlight %}  
With this if we execute java17 we will put Java 17 and java11 we will have Java 11 configured.

