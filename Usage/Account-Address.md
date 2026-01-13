# Account


## Address Format

Use the public key P as the input, by SHA3 get the result H. The length of the public key is 64 bytes, SHA3 uses `Keccak256`. Use the last 20 bytes of H, and add three bytes of `0x43E552` in front of it, then the address comes out. 

basecheck process: first do `Keccak256` caculation to address to get h1, then do `Keccak256` to h1 to get h2, use the first 4 bytes as check to add it to the end of the address to get address||check, do base58 encode to address||check to get the final result.

character map: ALPHABET = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
