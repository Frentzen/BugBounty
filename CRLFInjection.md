# Introdução

> O termo CRLF refere-se aos caracteres *Carriege Return (CR)*, em ASCII 13 ou /r, e *Line Feed (LF)*, em ASCII 10 ou /n. Eles são usados para definir o términio de um linha, entretanto, tratado diferentemente nos sistemas operacionais que são comumente usados.Por exemplo: nos sistemas Windows tanto CR quanto LF são necessários para o término de linha, já nos sistemas Linux/UNIX apenas o LF é requerido. 

> A injeção de vulnerabilidade CRLF Injection ocorre quando a aplicação não sanitiza a entrada fornecida pelo usuário.

Os servidores HTTP confiam nos terminadores de linha para verificar as seções da requisição, as URL encodam os respectivos caracteres com os hexadecimais correspondentes (%0D e %0A).

Essa vulnerabilidade pode ser um vetor inicial para outras mais performáticas como injeção de cabeçalhos e até mesmo SSRF.

# Exploração

```html
http://www.example.net/%0D%0ASet-Cookie:mycookie=myvalue
```
Neste exemplo os terminadores de linha são passados após a barra, mas também podem ser passados como parâmetros de uma URL correspondente a entrada de usuários:

```html
http://www.example.net/index.php?lang=en%0D%0ASet-Cookie:mycookie=myvalue
```

