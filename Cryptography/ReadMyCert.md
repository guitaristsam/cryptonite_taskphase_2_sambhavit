# ReadMyCert
**Flag: picoCTF{read_mycert_5aeb0d4f}**
## My thought process and approach to the challenge:
Saw that there was an attached file called: `readmycert.csr` which contained:
```
-----BEGIN CERTIFICATE REQUEST-----
MIICpzCCAY8CAQAwPDEmMCQGA1UEAwwdcGljb0NURntyZWFkX215Y2VydF81YWVi
MGQ0Zn0xEjAQBgNVBCkMCWN0ZlBsYXllcjCCASIwDQYJKoZIhvcNAQEBBQADggEP
ADCCAQoCggEBAMCkf11rmV8rgqPvC2ZiPA6W+5RfOTwU6u3WpGvLA+2YFzocBPut
aATTxTPB+uaN2ZN3Z5J2CTFGmPzI4sUQfSqhZGuAqbfMyDDR8pRswmIYVJ6s0Apc
Toi7H8m3IShSbeE0pZUSIJpbK1a7V6lJqgwFMDI1qrgNhGgZaMA/l+d2J0vC3EYd
AijwSs8APcp6woWbFGYwdw5KaBsjn23oVz2G4h3/TmdB5g5e6Oq+kgi38NEpRDS0
ylXo9mUko3FqS4I6y9gOtDEI4uZaCJZuXHDmBpqZ04MfXbIVlHjF9NMOjDvXLonN
650oaANBm4bhBlgid0Fx48Z36tbtAVivZEcCAwEAAaAmMCQGCSqGSIb3DQEJDjEX
MBUwEwYDVR0lBAwwCgYIKwYBBQUHAwIwDQYJKoZIhvcNAQELBQADggEBAHZx6h9r
G/SE7RCoX6ndk5BOJprRiHpxOqPLAWcDyKHfStln0/HcQZzIrRVRsmoHiOmch+md
PBA1b+M5aj+3BWtPR9jOY4vht+ZmHAKa0WfQxwb2dBxsRPKTTDea0wN2u8BHLlSM
PbWPNuz+TKySL41xfwFuM4VN/ywn58GTvdb7HXgwNZCGgo2N1WhRq/dBMiagXMah
yb6gX4erugCu61T5tyD80hgsNBjaqyIdy/whRfC/Pmn3QHmdkqB5ZCPezwb2OLm4
5RDGv3WOB5q0BofoUGhVq757QE8qhL3oTvV2WlLoi3YWaZkJMCeR3vnH92cKC1Ov
FxdQuLOH8GMvl7U=
-----END CERTIFICATE REQUEST-----

```
Researched a bit about CSR files and came across https://www.globalsign.com/en/blog/what-is-a-certificate-signing-request-csr, which showed me a familar face,         
![image](https://github.com/user-attachments/assets/0a1fc5c2-44c1-4534-b7c4-d6e0e9870afa)         
Knew I was on the right track and need to find to way to read it, Researching a bit more I came across, https://www.sslshopper.com/csr-decoder.html, which helped me decode the text and find out information.           
![image](https://github.com/user-attachments/assets/ffb5b14b-998c-494e-ad92-488c48f88231)
This helped me get:
```
Common Name: picoCTF{read_mycert_5aeb0d4f}
```
which inturn helped me get the flag: picoCTF{read_mycert_5aeb0d4f}.

---

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1. Learned what a Certificate Signing Request (CSR) file is and how it's formatted with encoded data and how to read it.
2. Understood the role of the Common Name in certificates and its use in CSR files(Literally the flag in this case) 
---

##  Incorrect tangents I went on while solving :
1. Tried using decoder tools like dcode and quipquip.
