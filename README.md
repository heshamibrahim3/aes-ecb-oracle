AES ECB Challenge Writeup
I identified the problem: The server uses AES-128 in ECB mode. This mode is insecure because it is "deterministic," meaning the same 16 bytes of input always produce the same 16 bytes of encrypted output.

I used a "Byte-at-a-Time" strategy: Since the server adds the secret flag to the end of my input, I realized I could "leak" the flag one character at a time by carefully choosing how much text I sent.

I set up the alignment: I sent 15 "A"s to the server. This left only one empty space in the first 16-byte block, which the server filled with the first character of the flag. I saved the encrypted result of this block.

I performed a brute-force search: I wrote a script to send 15 "A"s plus a "guess" character (like A, B, C...). When my guess produced the exact same encrypted block as the server's version, I knew I had found the correct letter.

I repeated the process: After finding the first letter ('F'), I sent 14 "A"s. This pulled the first two letters of the flag into the first block. I kept repeating this "shifting" and "guessing" process for every character until the full flag was revealed.

I used Python to automate it: I ran a Python script on my MacBook to handle the hundreds of connections needed to guess every character. This allowed me to recover the flag quickly and accurately.

The Final Flag: FLAG{4cabf3a3e11462343f142227aa}