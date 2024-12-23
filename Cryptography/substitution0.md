# substitution0
**Flag: picoCTF{5UB5717U710N_3V0LU710N_357BF9FF}**
## My thought process and approach to the challenge:
There was an attached message file that contained:
```
ZGSOCXPQUYHMILERVTBWNAFJDK 

Qctcnrel Mcptzlo ztebc, fuwq z ptzac zlo bwzwcmd zut, zlo gtenpqw ic wqc gccwmc
xtei z pmzbb szbc ul fqusq uw fzb clsmebco. Uw fzb z gcznwuxnm bsztzgzcnb, zlo, zw
wqzw wuic, nlhlefl we lzwntzmubwb—ex sentbc z ptczw rtukc ul z bsuclwuxus reulw
ex aucf. Wqctc fctc wfe tenlo gmzsh brewb lczt elc cjwtciuwd ex wqc gzsh, zlo z
melp elc lczt wqc ewqct. Wqc bszmcb fctc cjsccoulpmd qzto zlo pmebbd, fuwq zmm wqc
zrrcztzlsc ex gntlubqco pemo. Wqc fcupqw ex wqc ulbcsw fzb actd tcizthzgmc, zlo,
wzhulp zmm wqulpb ulwe selbuoctzwuel, U senmo qztomd gmzic Ynruwct xet qub eruluel
tcbrcswulp uw.

Wqc xmzp ub: ruseSWX{5NG5717N710L_3A0MN710L_357GX9XX}
```
The last part `ruseSWX{5NG5717N710L_3A0MN710L_357GX9XX}` was definately a flag.      
The key given in the beginneing was enough for me to recognise that `ZGSOCXPQUYHMILERVTBWNAFJDK` had to be replaced with `ABCDEFGHIJKLMNOPQRSTUVWXYZ`              
I used an online Mono-alphabetic Substitution tool(https://www.dcode.fr/monoalphabetic-substitution), which helped me get the following text:     
![image](https://github.com/user-attachments/assets/a3263097-a19b-4d8b-ae34-f137acc61bc9)
```
ABCDEFGHIJKLMNOPQRSTUVWXYZ

Hereupon Legrand arose, with a grave and stately air, and brought me the beetle
from a glass case in which it was enclosed. It was a beautiful scarabaeus, and, at
that time, unknown to naturalists--of course a great prize in a scientific point
of view. There were two round black spots near one extremity of the back, and a
long one near the other. The scales were exceedingly hard and glossy, with all the
appearance of burnished gold. The weight of the insect was very remarkable, and,
taking all things into consideration, I could hardly blame Jupiter for his opinion
respecting it.

The flag is: picoCTF{5UB5717U710N_3V0LU710N_357BF9FF}
```
This helped me get the flag: picoCTF{5UB5717U710N_3V0LU710N_357BF9FF}.     

---

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge:
1. Learnt about how Mono-alphabetic Substitution Ciphers work
2. Improved usage of online tools like dcode.

---

##  Incorrect tangents I went on while solving :
1. None 
