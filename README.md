# BHJ-BytesChars

Page 118

Cont. from 75

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
