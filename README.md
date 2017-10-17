Internationalized Domain Names (IDNs) enable people around the world to use domain names in local languages and scripts. IDNs are formed using characters from different scripts, such as Arabic, Chinese, Cyrillic or Devanagari. These are encoded by the Unicode standard and used as allowed by relevant IDN protocols. These writing systems are encoded in multi-byte Unicode and are stored as ASCII strings using Punycode transcription. IDN’s were introduced in 1998 to support cultural groups across the world to experience a more open Internet experience  and are a known source for cyber-attacks. In February 2017, Google took countermeasures, limiting the use of IDN by banning certain characters and sharpening security protocols. Unfortunately, during my research in the past two months, I have found several malicious IDNs falling through the cracks.

Some characters from the Cyrillic alphabet still have strong resemblances to Western characters, making users of legitimate domain names vulnerable to so-called IDN homograph attacks. These IDN domains are typically in the format http://xn--[domain name].[extension]. When this link is opened in the browser the Punycode is automatically translated to display in the language-alphabet specified. 

For example: The domain name http://xn--80ak6aa92e.com used to translate to аррӏе.com. (Cyrillic)
Which certainly looks a lot like the legitimate domain apple.com (ASCII). 
Another example would be http://xn--j1akc.com which still translates to kpn.com

The following active domain names where encountered coincidently on the 23rd of September 2017 during regular security research.

![alt text](https://raw.githubusercontent.com/101sec/snort-idn/master/hsbc.PNG)

As a security professional I am bound by strict rules and regulations regarding my work and believe in responsible disclosure. Therefore, I have to state that I carefully researched this website using passive technology for the most part and relied solely on open-source information. I did not cause any damage or conducted any hacking activity while investigating this and did not harm HSBC or its customers in any way or form. The findings have been under responsible disclosure and will be released to the public shortly. 

In the mean time, I have made a proof of concept detection capability for Snort to detect IDN homograph attacks. 

