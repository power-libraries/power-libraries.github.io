---
categories:
- general
title: IO Power
---
[Javadocs](https://www.javadoc.io/doc/com.github.power-libraries/iopower/1.1.3) - [Download](http://search.maven.org/remotecontent?filepath=com/github/power-libraries/iopower/1.1.3/iopower-1.1.3.jar) - [Maven](http://search.maven.org/#artifactdetails|com.github.power-libraries|iopower|1.1.3|jar)

IO Power is the first library of Power Libraries. It contains some helper methods for opening Input- and OutputStreams and Readers and Writers. The main purpose of IO Power is to make opening streams, readers and writers less cluttered and simple to understand.

## Examples
### Writing UTF-8
in plain Java:

~~~java
try(BufferedWriter out = new BufferedWriter(new OutputStreamWriter(new FileWriter("test.txt"), StandardCharsets.UTF_8))) {
    out.write("Hello World");
}
~~~


with IO Power:

~~~java
try(BufferedWriter out = Out.file("test.txt").usingUTF8().asWriter()) {
    out.write("Hello World");
}
~~~


### Reading lines from a compressed file
in plain Java:

~~~java
try(BufferedReader in = new BufferedReader(new InputStreamReader(new GZipInputStream(new FileInputStream("test.txt.gz"))))) {
    String line;
    while((line=in.readLine())!=null)
    	System.out.println(line);
}
~~~


with IO Power:

~~~java
for(String line:In.file("test").decompress().readLines())
    System.out.println(line);
~~~

### Serializing Objects to bytes
in plain Java:

~~~java
ByteArrayOutputStream byteOut = new ByteArrayOutputStream();
try(ObjectOutputStream out = new ObjectOutputStream(byteOut);) {
    out.writeObject(someObject);
}
byte[] bytes=byteOut.toByteArray();
~~~

with IO Power:

~~~java
byte[] bytes;
try(BAObjectOutputStream out=Out.bytes().asObjects()) {
	out.writeObject(someObject);
	bytes=out.toByteArray(); //this closes the stream automatically
}
~~~

or even simpler

~~~java
byte[] bytes=Out.bytes().writeObject(someObject);
~~~
