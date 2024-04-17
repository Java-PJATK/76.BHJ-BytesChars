# BHJ-BytesChars

Page 118

**Reading from InputStream**

When we read from an InputStream connected to System.in, we’re dealing with a binary stream. Invoking the read method, we obtain consecutive bytes (as int values) that can be cast to char. The end of data is indicated by the value -1, which is not a valid character. However, this end-of-data signal will not occur with System.in; instead, we detect the line feed character ('\n') to stop reading.

A potential issue arises with text containing characters encoded over two or three bytes—such characters will not be read correctly by a simple binary stream. To address this, we can pass the System.in object to the constructor of InputStreamReader. This acts as a ‘translator’, converting the binary stream into a text stream that respects a specific encoding.

For enhanced efficiency and convenience, we could further pass the InputStreamReader object to the constructor of BufferedReader. This allows us to read characters more efficiently and provides the readLine method, enabling us to read an entire line at once—a feature we’ll explore in subsequent examples.
