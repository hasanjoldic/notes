# Base64

Base64 is a group of binary-to-text encoding schemes that represent binary data (more specifically, a sequence of 8-bit bytes) in sequences of 24 bits that can be represented by four 6-bit Base64 digits.

As with all binary-to-text encoding schemes, Base64 is designed to carry data stored in binary formats across channels that only reliably support text content. Base64 is particularly prevalent on the World Wide Web where one of its uses is the ability to embed image files or other binary assets inside textual assets such as HTML and CSS files.

Base64 is also widely used for sending e-mail attachments. This is required because SMTP – in its original form – was designed to transport 7-bit ASCII characters only.

**RFC 4648** uses `A-Z`, `a-z`, `0-9`, and the characters `+` and `/`.

Example

`Many hands make light work.`  
to  
`TWFueSBoYW5kcyBtYWtlIGxpZ2h0IHdvcmsu`  

The encoded value of `Man` is `TWFu`. Encoded in ASCII, the characters `M`, `a`, and `n` are stored as the byte values `77`, `97`, and `110`, which are the 8-bit binary values `01001101`, `01100001`, and `01101110`.  
These three values are joined together into a 24-bit string, producing `010011010110000101101110`. Groups of 6 bits (6 bits have a maximum of 2^6 = 64 different binary values) are converted into individual numbers from start to end (in this case, there are four numbers in a 24-bit string), which are then converted into their corresponding Base64 character values.

**As this example illustrates, Base64 encoding converts three octets into four encoded characters.**

> `=` padding characters might be added to make the last encoded block contain four Base64 characters.
