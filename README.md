# BHJ-BytesChars

Page 118

Cont. from [75.KEP-WriteBin](https://github.com/Java-PJATK/75.KEP-WriteBin)

In the next example, we first read from an `InputStream` (connected just to `System.in`). Since this is a binary stream, invoking `read` returns consecutive bytes as integers, which we can cast to `char`. When the end of data is reached, `read` returns `-1` (note that `-1` does not correspond to any legal character). However, this will never happen with `System.in`, so we detect the LF character (`'\n'`) to stop reading.

A text may contain characters encoded on two or three bytes; such characters will not be read correctly when using this approach. To handle character encoding correctly, we can pass the `System.in` object to the constructor of `InputStreamReader`. This class acts as a translator, yielding an object that behaves as a text stream given a specific encoding.

Furthermore, we can pass the `InputStreamReader` object to the constructor of `BufferedReader`. By using `BufferedReader`, we can read characters much more efficiently. It also supports the very convenient `readLine` method, which allows us to read an entire line at once. This combination significantly simplifies reading text input, as we will see in one of the following examples.

## Listing 76 BHJ-BytesChars/BytesChars.java

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.IOException;
import java.io.Reader;
import static java.nio.charset.StandardCharsets.UTF_8;

public class BytesChars {
    public static void main(String[] args) {
        System.out.print("Type something and press enter ==> ");
        InputStream is = System.in;
        try {
            char c = ' ';
            while (true) {
                int i = is.read();
                c = (char) i;
                if (c == '\r' || c == '\n') break;
                System.out.printf("%3d ('", i);
                System.out.println(c + "') =>" +
                    " digit:" + Character.isDigit(c) +
                    " letter:" + Character.isLetter(c) +
                    " white:" + Character.isWhitespace(c));
            }
        } catch (IOException e) {
            e.printStackTrace();
            return;
        }

        // we don't close 'is' here, as this is the
        // standard input and we will use it later
        System.out.print("Type something again ==> ");
        try (
            Reader rd = new InputStreamReader(System.in, UTF_8)
        ) {
            char c = ' ';
            while (true) {
                int i = rd.read();
                c = (char) i;
                if (c == '\r' || c == '\n') break;
                System.out.printf("%#5x ('", i);
                System.out.println(c + "') =>" +
                    " digit:" + Character.isDigit(c) +
                    " letter:" + Character.isLetter(c) +
                    " white:" + Character.isWhitespace(c));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

Running the program, we can get:

```
Type something and press enter ==> Żółć
197 ('Å') => digit:false letter:true white:false
187 ('»') => digit:false letter:false white:false
195 ('Ã') => digit:false letter:true white:false
179 ('³') => digit:false letter:false white:false
197 ('Å') => digit:false letter:true white:false
130 ('') => digit:false letter:false white:false
196 ('Ä') => digit:false letter:true white:false
135 ('') => digit:false letter:false white:false

Type something again ==> Żółć
0x17b ('Ż') => digit:false letter:true white:false
0xf3 ('ó') => digit:false letter:true white:false
0x142 ('ł') => digit:false letter:true white:false
0x107 ('ć') => digit:false letter:true white:false
```

As we can see, in the first case, multi-byte characters (as in `Żółć`) are not read correctly. This is because (at least under Linux) the text from the console is in UTF-8 encoding, in which a single character may occupy 1, 2, 3, or even 4 bytes. The stream `System.in` is, however, a byte (binary) stream, so each read consumes one byte, which does not necessarily correspond to any character, as it may be only a part of a multi-byte character.

The situation is different in the second case — here we wrap `System.in` in `InputStreamReader` (which behaves as a text stream), passing also the correct encoding. The object of this wrapper (decorator) class behaves as a text stream, so each read consumes one character, no matter how many bytes it takes.
