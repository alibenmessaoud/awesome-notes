

# Java streams

For a long time, I used Java streams without a deep understanding until in a recent conversation about it! So, I decided to do a quick overview of Java streams.

First thing first, a stream is a sequence of an ordered data that provides i/o model and an abstraction of the detail of the manipulated data. Streams are unidirectional; even read or write and not both. 

Streams can interact with binary data or text. 

The base class to read binary data in Java is the `InputStream`*.* And the base class to read text data is called the `Reader`class. Both classes have `int read(..)`. 

The base class to write binary data in Java is the `OuputStream`*.* And the base class to write text data is called the `Writer`class. Both classes have `void write(..)`. 

Common Input/ OutputStream derived Classes

- InputStream => ByteArrayInputStream, PipedInputStream, FileInputStream

- OutputStream => ByteArrayOutputStream, PipedOutputStream, FileOutputStream

Common Reader/Writer Derived Classes

- Reader => CharArrayReader, StringReader, PipedReader, InputStreamReader (FileReader)

- Writer => CharArrayWriter, StringWriter, PipedWriter, InputStreamWriter (FileWriter)

Realities:

- Error handling: throw exception
- Cleanup: Need to close streams after finishing => Closeable + Try-With-Resources