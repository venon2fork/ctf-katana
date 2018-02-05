CTF-Katana
===============

> John Hammond | February 1st, 2018

--------------------------


This repository, at the time of writing, will just host a listing of tools and commands that may help with CTF challenges. I hope to keep it as a "live document," and ideally it will not die out like the old "tools" page I had made ([https://github.com/USCGA/tools](https://github.com/USCGA/tools)).

Hopefully, at some point I will develop software that will run through a lot of the low-hanging fruit and simple command-line tools, generate a report and have all the output in one place.


---------------

Steganography
---------------------

* [`steghide`][steghide]

	A command-line tool typically used alongside a password or key, that could be uncovered some other way when solving a challenge. 

* [`zsteg`][zsteg]

	Command-line tool for use against Least Significant Bit steganography... unfortunately only works against PNG and BMP images.

* Morse Code

	Always test for this if you are seeing two distinct values... _it may not always be binary!_

* Whitespace

	Tabs and spaces could be representing 1's and 0's and treating them as a binary message... or, they could be whitespace done with [`snow`][snow] or an esoteric programming language interpreter: [http://ws2js.luilak.net/interpreter.html](http://ws2js.luilak.net/interpreter.html)

* [`snow`][snow]

	A command-line tool for whitespace steganography (see above).

* SONIC Visualizer (audio spectrum)

	Some classic challenges use an audio file to hide a flag or other sensitive stuff. SONIC visualizer easily shows you [spectrogram](https://en.wikipedia.org/wiki/Spectrogram). __If it sounds like there is random bleeps and bloops in the sound, try this tactic!__

* [Detect DTMF Tones]

	Audio frequencies common to a phone button, DTMF: [https://en.wikipedia.org/wiki/Dual-tone_multi-frequency_signaling](https://en.wikipedia.org/wiki/Dual-tone_multi-frequency_signaling).

* [`hipshot`][hipshot]

	A [Python] module to compress a video into a single standalone image, simulating a long-exposure photograph. Was used to steal a [QR code] visible in a video, displayed through "Star Wars" style text motion.	

* [QR code]

	A small square "barcode" image that holds data.

* [`zbarimg`][zbarimg]

	A command-line tool to quickly scan multiple forms of barcodes, [QR codes] included. Installed like so on a typical [Ubuntu] image: 

```
sudo apt install zbar-tools
```
	

Cryptography
-----------------

* XOR

	ANY text could be XOR'd. Techniques for this are Trey's code, and XORing the data against the known flag format. Typically it is given in just hex, but once it is decoded into raw binary data, it gives it keeps it's hex form (as in `\xde\xad\xbe\xef` etc..) Note that you can do easy XOR locally with Python like so (you need `pwntools` installed):

	``` python
	python >>> import pwn; pwn.xor("KEY", "RAW_BINARY_CIPHER")
	```


* Caesar Cipher

	The most classic shift cipher. Tons of online tools like this: [https://www.dcode.fr/caesar-cipher](https://www.dcode.fr/caesar-cipher) or use `caesar` as a command-line tool (`sudo apt install bsdgames`) and you can supply a key for it. Here's a one liner to try all letter positions:

	```
	cipher='jeoi{geiwev_gmtliv_ws_svmkmrep}' ; for i in {0..25}; do echo $cipher | caesar $i; done
	```

	__Be aware!__ Some challenges include punctuation in their shift! If this is the case, try to a shift within all 255 ASCII characters, not just 26 alphabetical letters!

* `caesar`

	A command-line caesar cipher tool (noted above) found in the `bsdgames` package.

* [Vigenere Cipher]

	[http://www.mygeocachingprofile.com/codebreaker.vigenerecipher.aspx](http://www.mygeocachingprofile.com/codebreaker.vigenerecipher.aspx), [https://www.guballa.de/vigenere-solver](https://www.guballa.de/vigenere-solver)

* Beaufourt Cipher

	[https://www.dcode.fr/beaufort-cipher](https://www.dcode.fr/beaufort-cipher)

* Transposition Cipher

* RSA: Classic RSA

	Variables typically given: `n`, `c`, `e`. _ALWAYS_ try and give to [http://factordb.com](http://factordb.com)

* RSA: Multi-prime RSA

* RSA: Weiner's Little D Attack

	The telltale sign for this kind of challenge is an enormously large `e` value. Typically `e` is either 65537 (0x10001) or `3` (like for a Chinese Remainder Theorem challenge)

* RSA: Chinese Remainder Attack

	These challenges can be spotted when given  mutiple `c` cipher texts and multiple `n` moduli. `e` must be the same number of given `c` and `n` pairs.

* Elgamal

* Affine Cipher

* Substitution Cipher

	[https://quipqiup.com/](https://quipqiup.com/)

* Railfence Cipher

	[http://rumkin.com/tools/cipher/railfence.php](http://rumkin.com/tools/cipher/railfence.php)


* [Playfair Cipher]

	racker: [http://bionsgadgets.appspot.com/ww_forms/playfair_ph_web_worker3.html](http://bionsgadgets.appspot.com/ww_forms/playfair_ph_web_worker3.html)

* Polybius Square

	[https://www.braingle.com/brainteasers/codes/polybius.php](https://www.braingle.com/brainteasers/codes/polybius.php)

* The Engima

	[http://enigma.louisedade.co.uk/enigma.html](http://enigma.louisedade.co.uk/enigma.html),
	[https://www.dcode.fr/enigma-machine-cipher](https://www.dcode.fr/enigma-machine-cipher)

* AES ECB

	The "blind SQL" of cryptography... leak the flag out by testing for characters just one byte away from the block length. 


Networking
---------------

* [Wireshark]

	The go-to tool for examining [`.pcap`][PCAP] files. 

* [Network Miner]

	Seriously cool tool that will try and scrape out images, files, credentials and other goods from [PCAP] and [PCAPNG] files.

* [PCAPNG]

	Not all tools like the [PCAPNG] file format... so you can convert them with an online tool [http://pcapng.com/](http://pcapng.com/) or from the command-line with the `editcap` command that comes with installing [Wireshark]:

```
editcap old_file.pcapng new_file.pcap
```

* [`tcpflow`][tcpflow]

	A command-line tool for reorganizing packets in a PCAP file and getting files out of them. __Typically it gives no output, but it creates the files in your current directory!__ 

```
tcpflow -r my_file.pcap
ls -1t | head -5 # see the last 5 recently modified files
```


PHP
------------

* Magic Hashes

	A common vulnerability in [PHP] that fakes hash "collisions..." where the `==` operator falls short in [PHP] type comparison, thinking everything that follows `0e` is considered scientific notation (and therefore 0). More valuable info can be found here: [https://github.com/spaze/hashes](https://github.com/spaze/hashes), but below are the most common breaks.

| Plaintext | MD5 Hash |
| --------- | -------- |
|240610708|0e462097431906509019562988736854|
|QLTHNDT|0e405967825401955372549139051580|
|QNKCDZO|0e830400451993494058024219903391|
|PJNPDWY|0e291529052894702774557631701704|
|NWWKITQ|0e763082070976038347657360817689|
|NOOPCJF|0e818888003657176127862245791911|
|MMHUWUV|0e701732711630150438129209816536|
|MAUXXQC|0e478478466848439040434801845361|
|IHKFRNS|0e256160682445802696926137988570|
|GZECLQZ|0e537612333747236407713628225676|
|GGHMVOE|0e362766013028313274586933780773|
|GEGHBXL|0e248776895502908863709684713578|
|EEIZDOI|0e782601363539291779881938479162|
|DYAXWCA|0e424759758842488633464374063001|
|DQWRASX|0e742373665639232907775599582643|
|BRTKUJZ|00e57640477961333848717747276704|
|ABJIHVY|0e755264355178451322893275696586|
|aaaXXAYW|0e540853622400160407992788832284|
|aabg7XSs|0e087386482136013740957780965295|
|aabC9RqS|0e041022518165728065344349536299|

| Plaintext | SHA1 Hash |
| --------- | --------- |
|aaroZmOk|0e66507019969427134894567494305185566735|
|aaK1STfY|0e76658526655756207688271159624026011393|
|aaO8zKZF|0e89257456677279068558073954252716165668|
|aa3OFF9m|0e36977786278517984959260394024281014729|


* `preg_replace`

	A bug in older versions of [PHP] where the user could get remote code execution
	
	[http://php.net/manual/en/function.preg-replace.php](http://php.net/manual/en/function.preg-replace.php)


* [`phpdc.phpr`][phpdc.phpr]

	A command-line tool to decode [`bcompiler`][bcompiler] compiled [PHP] code.


* [`php://filter` for Local File Inclusion](https://www.idontplaydarts.com/2011/02/using-php-filter-for-local-file-inclusion/)

	A bug in [PHP] where if GET HTTP variables in the URL are controlling the navigation of the web page, perhaps the source code is `include`-ing other files to be served to the user. This can be manipulated by using [PHP filters](http://php.net/manual/en/filters.php) to potentially retrieve source code. Example like so:

```
http://xqi.cc/index.php?m=php://filter/convert.base64-encode/resource=index
```


PDF Files
-------------

* `pdfinfo`	
	
	A command-line tool to get a basic synopsis of what the [PDF] file is.

* `pdfcrack`

	A comand-line tool to __recover a password from a PDF file.__ Supports dictionary wordlists and bruteforce.

* `pdfimages`

	A command-line tool, the first thing to reach for when given a PDF file. It extracts the images stored in a PDF file, but it needs the name of an output directory (that it will create for) to place the found images.

* [`pdfdetach`][pdfdetach]

	A command-line tool to extract files out of a [PDF]


Forensics
-----------

* `foremost`

	A command-line tool to carve files out of another file. Usage is `foremost [filename]` and it will create an `output` directory.

```
sudo apt install foremost
```

* `binwalk`

	A command-line tool to carve files out of another file. Usage to extract is `binwalk -e [filename]` and it will create a `_[filename]_extracted` directory.

```
sudo apt install binwalk
```


* [`hachoir-subfile`][hachoir-subfile]

	A command-line tool to carve out files of another file. Very similar to the other tools like `binwalk` and `foremost`, but always try everything!

Web
----------------

* `robots.txt`

	This file tries to hide webpages from web crawlers, like Google or Bing or Yahoo. A lot of sites try and use this mask sensitive files or folders, so it should always be some where you check during a CTF. [http://www.robotstxt.org/](http://www.robotstxt.org/)

* `/admin/`

	This directory is often found by directory scanning bruteforce tools, so I recommend just checking the directory on your own, as part of your own "low-hanging fruits" check.

* `/.git/`
	
	A classic CTF challenge is to leave a `git` repository live and available on a website. You can see this with `nmap -A` (or whatever specific script catches it) and just by trying to view that specific folder, `/.git/`. A good command-line tool for this is [`GitDumper.sh`][https://github.com/internetwache/GitTools], or just simply using [`wget`][wget].

* [`GitDumper.sh`][GitDumper.sh]
	
	A command-line tool that will automatically scrape and download a [git] repository hosted online with a given URL.

* [XSS]/[Cross-site scripting]

	[XSS Filter Evasion Cheat Sheet](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet). [Cross-site scripting], vulnerability where the user can control rendered [HTML] and ideally inject [JavaScript] code that could drive a browser to any other website or make any malicious network calls. Example test payload is as follows:

```
<IMG SRC=/ onerror="alert(String.fromCharCode(88,83,83))"></img>
```

* Cookie Catcher

	

* [`requestb.in`][https://requestb.in/]

	A free tool and online end-point that can be used to catch HTTP requests. Typically these are controlled and set by finding a [XSS] vulnerabilty.

* [`hookbin.com`][https://hookbin.com/]

	A free tool and online end-point that can be used to catch HTTP requests. Typically these are controlled and set by finding a [XSS] vulnerabilty.

* [`sqlmap`][sqlmap]

	A command-line tool written in [Python] to automatically detect and exploit vulnerable SQL injection points.	

* Flask Template Injection

	[https://nvisium.com/resources/blog/2015/12/07/injecting-flask.html](https://nvisium.com/resources/blog/2015/12/07/injecting-flask.html), [https://nvisium.com/resources/blog/2016/03/09/exploring-ssti-in-flask-jinja2.html](https://nvisium.com/resources/blog/2016/03/09/exploring-ssti-in-flask-jinja2.html), [https://nvisium.com/resources/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii.html](https://nvisium.com/resources/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii.html)

* Explicit SQL Injection



* Blind SQL Injection



* gobuster


* DirBuster

* `nikto`


* Burpsuite


Windows Executables
-------------

* [`pefile`][pefile]

	A [Python] module that examines the headers in a Windows [PE (Portable Executable)][PE] file. 

* [dnSpy]

	A [Windows] GUI tool to decompile and reverse engineer [.NET] binaries

* [PEiD][PEiD]

	A [Windows] tool to detect common packers, cryptors and compilers for [Windows][Windows] [PE][PE] 

* jetBrains .NET decompiler

* AutoIT

Python Reversing
------------

* [Easy Python Decompiler]

	A small `.exe` GUI application that will "decompile" [Python] bytecode, often seen in `.pyc` extension. The tool runs reliably on [Linux] with [Wine].



Java Reversing
----------




Android APK Reversing
-----------


VisualBasicScript Reversing
---------------------------




[steghide]: http://steghide.sourceforge.net/
[snow]: http://www.darkside.com.au/snow/
[cribdrag.py]: https://github.com/SpiderLabs/cribdrag
[cribdrag]: https://github.com/SpiderLabs/cribdrag
[pcap]: https://en.wikipedia.org/wiki/Pcap
[PCAP]: https://en.wikipedia.org/wiki/Pcap
[Wireshark]: https://www.wireshark.org/
[Network Miner]: http://www.netresec.com/?page=NetworkMiner
[PCAPNG]: https://github.com/pcapng/pcapng
[pcapng]: https://github.com/pcapng/pcapng
[pdfcrack]: http://pdfcrack.sourceforge.net/index.html
[GitDumper.sh]: https://github.com/internetwache/GitTools
[pefile]: https://github.com/erocarrera/pefile
[Python]: https://www.python.org/
[PE]: https://en.wikipedia.org/wiki/Portable_Executable
[Portable Executable]: https://en.wikipedia.org/wiki/Portable_Executable
[hipshot]: https://bitbucket.org/eliteraspberries/hipshot
[QR code]: https://en.wikipedia.org/wiki/QR_code
[QR codes]: https://en.wikipedia.org/wiki/QR_code
[QR]: https://en.wikipedia.org/wiki/QR_code
[zbarimg]: https://linux.die.net/man/1/zbarimg
[Linux]: https://en.wikipedia.org/wiki/Linux
[Ubuntu]: https://en.wikipedia.org/wiki/Ubuntu_(operating_system)
[Wine]: https://en.wikipedia.org/wiki/Wine_(software)
[Detect DTMF Tones]: http://dialabc.com/sound/detect/index.html
[dnSpy]: https://github.com/0xd4d/dnSpy
[Windows]: https://en.wikipedia.org/wiki/Microsoft_Windows
[.NET]: https://en.wikipedia.org/wiki/.NET_Framework
[Vigenere Cipher]: https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher
[PDF]: https://en.wikipedia.org/wiki/Portable_Document_Format
[Playfair Cipher]: https://en.wikipedia.org/wiki/Playfair_cipher
[phpdc.phpr]:https://github.com/lighttpd/xcache/blob/master/bin/phpdc.phpr
[bcompiler]: http://php.net/manual/en/book.bcompiler.php
[PHP]: https://en.wikipedia.org/wiki/PHP
[GET]: https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods
[pdfdetach]: https://www.systutorials.com/docs/linux/man/1-pdfdetach/
[sqlmap]: https://github.com/sqlmapproject/sqlmap
[hachoir-subfile]: https://pypi.python.org/pypi/hachoir-subfile/0.5.3
[wget]: https://en.wikipedia.org/wiki/Wget
[git]: https://git-scm.com/
[Cross-site scripting]: https://en.wikipedia.org/wiki/Cross-site_scripting
[XSS]: https://en.wikipedia.org/wiki/Cross-site_scripting
[HTML]: https://en.wikipedia.org/wiki/HTML
[JavaScript]: https://en.wikipedia.org/wiki/JavaScript
[PEiD]: https://www.aldeid.com/wiki/PEiD