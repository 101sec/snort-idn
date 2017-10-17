# IDN Homograph Attacks

Internationalized Domain Names (IDNs) enable people around the world to use domain names in local languages and scripts. IDNs are formed using characters from different scripts, such as Arabic, Chinese, Cyrillic or Devanagari. These are encoded by the Unicode standard and used as allowed by relevant IDN protocols. These writing systems are encoded in multi-byte Unicode and are stored as ASCII strings using Punycode transcription. IDN’s were introduced in 1998 to support cultural groups across the world to experience a more open Internet experience  and are a known source for cyber-attacks. In February 2017, Google took countermeasures, limiting the use of IDN by banning certain characters and sharpening security protocols. Unfortunately, during my research in the past two months, I have found several malicious IDNs falling through the cracks.

Some characters from the Cyrillic alphabet still have strong resemblances to Western characters, making users of legitimate domain names vulnerable to so-called IDN homograph attacks. These IDN domains are typically in the format http://xn--[domain name].[extension]. When this link is opened in the browser the Punycode is automatically translated to display in the language-alphabet specified. 
For example: The domain name http://xn--80ak6aa92e.com used to translate to аррӏе.com. (Cyrillic). Which certainly looks a lot like the legitimate domain apple.com (ASCII). 


![alt text](https://raw.githubusercontent.com/101sec/snort-idn/master/screenshots/8002.png)


Cybercriminals are using IDN domains despite stronger policies. The following active domain names where encountered coincidently on the 23rd of September 2017 during regular security research.  

- [hxxps://xn--b1a4a6c3v.com]
- [xn--hsb-spa.com]
- [hxxps://c2-domain.com]

After further analysis, this appeared to be a clickjacking botnet targeting customers and / or employees from the international HSBC bank. I decided to clone the website using a proxy for off-line analysis. The first thing that occurs to me is a blank index.html page containing nothing but the following base64 encoded string:
```
YmU4OTdiZWVlYjkzNTBlMTdjYjBjOGJkNGQ0ZWQ5Mzc5NzgxMmQ5NCAgLQo=
```
When we decode this string using a base64 decoder we are stuck with what appears to be a 40-byte SHA-1 hashed string:
```
be897beeeb9350e17cb0c8bd4d4ed93797812d94
```
Investigated if the above hash was already cracked using a variety of databases and wordlists. Unfortunately without luck so far.
After monitoring this page for a while it occurs to me that the string changes over-time, which could indicate botnet / command and control (c2) activity. After sending the report to Google and HSBC CERT, this is the result as of now:

![alt text](https://raw.githubusercontent.com/101sec/snort-idn/master/screenshots/hsbc2.PNG)

As a security professional I am bound by strict rules and regulations regarding my work and believe in responsible disclosure. Therefore, I have to state that I carefully researched this website using passive technology for the most part and relied solely on open-source information. I did not cause any damage or conducted any hacking activity while investigating this and did not harm HSBC or its customers in any way or form. The findings have been under responsible disclosure. Since HSBC did not request more time, I am allowed to share this with the public.

# Detecting IDN Homograph Attacks 
In the mean time, I have made a proof of concept detection capability for Snort to detect IDN homograph attacks. 

![alt text](https://raw.githubusercontent.com/101sec/snort-idn/master/screenshots/alert-ids.PNG)
