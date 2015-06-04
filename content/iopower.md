+++
categories = ["general"]
date = "2015-05-27T17:52:47+02:00"
title = "IO Power"
+++
[Javadocs](../javadocs/iopower/) - [Download](https://github.com/manuel-hegner/power-libraries/releases)

IO Power is the first and really tiny library of the Power Libraries. It 
contains some simple helper method for opening Input- and Outputstreams. The 
main purpose of IO Power is to make opening streams, readers and writers shorter 
and more understandable.

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
try(BufferedWriter out = Out.file("test.txt").fromWriter(StandardCharsets.UTF_8)) {
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
try(BAObjectOutputStream out=Out.bytes().fromObjects()) {
	out.writeObject(someObject);
	bytes=out.toByteArray(); //this closes the stream automatically
}
~~~