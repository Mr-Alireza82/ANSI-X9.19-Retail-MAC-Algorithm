# ANSI-X9.19-Retail-MAC-Algorithm
## ANSI X9.19 Retail MAC Algorithm with mathmatical examples and java impementation code

MAC which stands for Message Authentication Code, works as a code which verifies Authority of incoming request.
for example in ISO8583 protocol, many of it's datas are raw so anybody can see them and use them, hackers can pass fake data to server. how server knows which request is real and which is fake? This is where a simple and short code can helps server to decide it.
> for example server knows request with code 4127 is valid so it only accepts requests which contain this code.

## if anyody can see the request and server only asnwers to a specified code then can't the hackers see the correct code and use it?
Yes! they actully can, that is why we dont use simple raw codes, instead each request code is calculated via complex mathematical operations beside a secrect key.

## Quick Overview
1. There are 2 data, Request Message, Secret Key1 and Key2 and Initial chaining value.
2. Divide request message into 8-byte blocks, if msg length is not multiple of 8-bytes then padd message to become multiple of 8-bytes.
3. Set a variable as chaining value, it vaalue for each chaning operation will be updated so it's size will stay the same(8-byte)
3. XOR each message block with the result of preceding chaining value, for first operation there is no calculated chaining value, so many standards get an initial chaining value as an argument, if not it's defualt value is usually 8-byte block of zeros(payment protocol defines it)
4. Encrypt the result with fisrt key using [DES Algorithm](https://example.com).
5. Do the same operation again until reaching last block.
6. Decrypt the final chaining value with second key.
7. Encrypt the result with first key again.
8. You get final 8-byte MAC Authentication code using ANSI X9.19 algorithms.

# Standard Terminoligis:
| Notation | Recommended name | Country |
|------|-----|---------|
| \(M_i\) | Message block | The \(i\)-th 8-byte part of the message/body |
| Bob | 30 | USA |
| Charlie | 22 | India |

## KEY
\(K_1\) and \(K_2\) are each 8-byte DES key components. In the common two-key version, they are supplied together as a 16-byte key.
