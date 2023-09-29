# Why do we use Base64? 

Wikipedia says

Base64 encoding schemes are commonly used when there is a need to encode binary data that needs be stored and transferred over media that are designed to deal with textual data. 
This is to ensure that the data remains intact without modification during transport.

But is it not that data is always stored/transmitted in binary because the memory that our machines have store binary and it just depends how you interpret it? 
So, whether you encode the bit pattern 010011010110000101101110 as Man in ASCII or as TWFu in Base64, you are eventually going to store the same bit pattern.

If the ultimate encoding is in terms of zeros and ones and every machine and media can deal with them, how does it matter if the data is represented as ASCII or Base64?

What does it mean "media that are designed to deal with textual data"? They can deal with binary => they can deal with anything.


# Answer

Your first mistake is thinking that ASCII encoding and Base64 encoding are interchangeable. They are not. They are used for different purposes.

When you encode text in ASCII, you start with a text string and convert it to a sequence of bytes.
When you encode data in Base64, you start with a sequence of bytes and convert it to a text string.
To understand why Base64 was necessary in the first place we need a little history of computing.

Computers communicate in binary - 0s and 1s - but people typically want to communicate with more rich forms data such as text or images. 
In order to transfer this data between computers it first has to be encoded into 0s and 1s, sent, then decoded again. 
To take text as an example - there are many different ways to perform this encoding. 
It would be much simpler if we could all agree on a single encoding, but sadly this is not the case.

Originally a lot of different encodings were created (e.g. Baudot code) which used a different number of bits per character until eventually ASCII became a standard with 7 bits per character.
However most computers store binary data in bytes consisting of 8 bits each so ASCII is unsuitable for tranferring this type of data. Some systems would even wipe the most significant bit. 
Furthermore the difference in line ending encodings across systems mean that the ASCII character 10 and 13 were also sometimes modified.

To solve these problems Base64 encoding was introduced. 
This allows you to encode arbitrary bytes to bytes which are known to be safe to send without getting corrupted (ASCII alphanumeric characters and a couple of symbols). 
The disadvantage is that encoding the message using Base64 increases its length - every 3 bytes of data is encoded to 4 ASCII characters.

To send text reliably you can first encode to bytes using a text encoding of your choice (for example UTF-8) and 
then afterwards Base64 encode the resulting binary data into a text string that is safe to send encoded as ASCII. 
The receiver will have to reverse this process to recover the original message. 
This of course requires that the receiver knows which encodings were used, and this information often needs to be sent separately.

Historically it has been used to encode binary data in email messages where the email server might modify line-endings.
A more modern example is the use of Base64 encoding to embed image data directly in HTML source code. 
Here it is necessary to encode the data to avoid characters like '<' and '>' being interpreted as tags.

Here is a working example:

I wish to send a text message with two lines:

Hello
world!
If I send it as ASCII (or UTF-8) it will look like this:

72 101 108 108 111 10 119 111 114 108 100 33
The byte 10 is corrupted in some systems so we can base 64 encode these bytes as a Base64 string:

SGVsbG8Kd29ybGQh
Which when encoded using ASCII looks like this:

83 71 86 115 98 71 56 75 100 50 57 121 98 71 81 104
All the bytes here are known safe bytes, so there is very little chance that any system will corrupt this message. 
I can send this instead of my original message and let the receiver reverse the process to recover the original message.
