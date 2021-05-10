# Writeup by Shrimpgo

- O arquivo challenge.png possui uma imagem e um zip. Descobri usando a ferramenta foremost;  

- A imagem possui pontos verdes em certos espaços. Indicativo de algo escondido nela;  

- O zip extraído necessita de uma senha para extrair seu conteúdo. Usando bruteforce, não foi possível achar a senha;  

- Na imagem inicial, utilizando a ferramenta exiftool, no campo "Adult Content Warning", há um código em base64. Convertido, dá "m3t4s3nha", mas usando a mesma pra descriptografar o zip, não funcionou, porém usando a senha codificada em base64, foi possível descriptografar o zip;  

- Uma nova imagem com o nome saldo-defcon-challenge.png surge. É um invoice do Renato recebendo um bounty;  

- Como não havia nada nos metadados e nem no conteúdo do arquivo, resolvi utilizar a ferramenta stegoveritas para observar se havia alguma marca d'água ou algo similar. Encontrei um QRCode e nele havia um link para o pastebin: [https://pastebin.com/Mmyt7GtH](https://pastebin.com/Mmyt7GtH);  

- Esse link dá uma pista de como a flag foi escondida, com um trecho do código. 

- Segue o código:  

```
...  

OFFSET = 16  

for c in hexflag:  

    for i in range(OFFSET, 1000, 36):  

        p = img.getpixel((i, 988))  

        img.putpixel((i, 988), (p[0], c ^ 137, p[2]))  

...  
```
  
- Lembrei da primeira imagem com os pequenos pixels simétricos verde. Agora fez sentido;  

- Criei um script em Python para obter a flag. 

- Segue o código:
  
```
from PIL import Image  
  
img = Image.open("test.png")  
flag = ""  
  
#OFFSET = 16  
#for c in hexflag:  
#  for i in range(OFFSET, 1000, 36):  
#     p = img.getpixel((i, 988))  
#     img.putpixel((i, 988), (p[0], c ^ 137, p[2]))  
  
OFFSET = 16  
  
for i in range(OFFSET, 1000, 36):  
 p = img.getpixel((i, 988))  
 flag += chr(p[1] ^ 137)  
print(flag)
```  

flag: DC5551{ST3G4n0_fL4g_dc5551}
