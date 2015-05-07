# termshare

Quick and easy terminal sharing for getting quick help or pair sysadmin'ing. 

Share interactive control with a copilot and/or a readonly view of your terminal with others. Copilots and viewers can use the client or a web-based terminal.

## THIS IS JUST A FORK

For the original version please go to https://github.com/progrium/termshare

Lately i faced this issue:
```
panic: Post https://termsha.re:443/c4a9d55c-8038-4622-6fb5-a66105f61a36: x509: certificate signed by unknown authority
```

But a simple recompile solved my problem ...

## Installation

Here is the recompiled version which fixxes the cert issue. For Linux and OSX:
```
curl -L https://github.com/lalyos/termshare/releases/download/v0.2.0/termshare_v0.2.0_$(uname -s)_x86_64.tgz| tar -xz -C /usr/local/bin
```

## tl;dr

I spent almost a day to figure out, but still couldn't find out the root cause.
`termsha.re` is using HTTPS. Its certificate is signed with this chain:
- COMODO_RSA_Domain_Validation_Secure_Server_CA
- COMODO_RSA_Certification_Authority
- AddTrust_External_CA_Root

Looks like golang is having problem to accept it. Golfing itself doesn't have any cert storage its using 
it from the OS. In https://golang.org/src/crypto/x509/root_darwin.go you can see the source is: `/System/Library/Keychains/SystemRootCertificates.keychain`

### Is my keychain missing something?

So it seems like my OS X keychain's ca cert list is missing something. But:
- original `termshare` shows unknown authority.
- recompiled `termshare` connects fine.

Further mystery:
- I have an MacBookPro13 with OS X 10.10.3, where the original `termshare` works fine
- My new MacBookPro15 with OS X 10.10.3, the original `termshare` complains about  certificate signed by unknown authority
