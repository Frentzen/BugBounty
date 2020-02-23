# Introdução

A vulnerabilidade causada pelo *subdomain take over*, que em português significa assumir o controle domínio, ocorre quando há a delegação de zonas no serviço de DNS. Imagine que uma organização possua domínio ```hydra.net``` e que a mesma tenha o interesse de criar um e-commerce para seus serviços porém em um provedor específico para este tipo de serviço. Este provedor irá gerar uma instância para a organização solicitante porém com uma entrada de DNS própria, diferente do domínio da organização solicitante, ```ecloudcommercerinstance01.ecommerce.com```

Para um boa comunicação social daquela organização a mesma alocará uma entrada de DNS do tipo CNAME, criando uma espécie de alias, de um subdomínio de seu controle para o subdomínio gerado pela provedora que foi contratada. Após algum tempo este serviço não e renovado pela sua empresa e o subdomínio criado pela prestadora do servço é desativado porém a entrada CNAME no seu servidor de DNS não, então surge a vulnerabilidade supracitada.

# Ferramentas

As ferramentas abaixo são úteis para a identificaçõ deste tipo de vulnerabilidade:

https://github.com/michenriksen/aquatone

https://github.com/Ice3man543/SubOver

https://github.com/haccer/subjack

# Enumeração e Descoberta

A descoberta pode ser feita de forma passiva ou ativa. Na primeira são utilizadas técnicas sem interessão com o alvo buscando prioritariamente em bases de dados já ativas, técnica também chamada de *scrapping*. Para esta primeira parte será utilizada a ferramenta [Sublist3r](https://github.com/aboul3la/Sublist3r).

A instalação correta da ferramenta ocorre da seguinte forma:

```bash
git clone https://github.com/aboul3la/Sublist3r
cd Sublist3r
pip intall -r requirements.txt
```

A ferramenta mostra os seguintes resultados:

```bash
python3 sublist3r.py -d hackerone.com -t 50

                 ____        _     _ _     _   _____
                / ___| _   _| |__ | (_)___| |_|___ / _ __
                \___ \| | | | '_ \| | / __| __| |_ \| '__|
                 ___) | |_| | |_) | | \__ \ |_ ___) | |
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|

                # Coded By Ahmed Aboul-Ela - @aboul3la
    
[-] Enumerating subdomains now for hackerone.com
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
[-] Searching now in DNSdumpster..
[-] Searching now in Virustotal..
[-] Searching now in ThreatCrowd..
[-] Searching now in SSL Certificates..
[-] Searching now in PassiveDNS..
[-] Total Unique Subdomains Found: 15
www.hackerone.com
api.hackerone.com
go.hackerone.com
info.hackerone.com
api.hackerone.com
hackerone.com
www.hackerone.com
docs.hackerone.com
mta-sts.forwarding.hackerone.com
info.hackerone.com
links.hackerone.com
mta-sts.managed.hackerone.com
mta-sts.hackerone.com
a.ns.hackerone.com
b.ns.hackerone.com
support.hackerone.com
```
Com os subdomínios descobertos iremos utilizar a ferramenta [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness) para realizar uma captura de tela das páginas iniciais de todos os links fornecidos. A ferramenta irá gerar um reporte em *html* de fácil visualização para o operador.

```bash
./EyeWitness -f listadomain.txt -d out --headless
```

EyeWitness pode se tornar pesado em casos que existam muitos subdominios, neste caso pode ser adotada a ferramenta [meg](https://github.com/tomnomnom/meg) que envia requisições do tipo **GET** para a página e as armazena em um arquivo de texto. Isto torna o processo mais rápido e para identificação de anomalias poderá ser utilizado comandos *grep* para filtragem por palavras chaves.

```bash
meg -d 10 -c 200 / listadomain.txt
```
Desta forma poderá ser feita a descoberta manual deste tipo de vulnerabilidade, porém com a ferramenta [subjack](https://github.com/haccer/subjack) fica mais fácil a identificação uma vez que a mesma automatiza a detecção baseada em *fingerprints* conhecidos.

```bash
./subjack -w subdomains.txt -t 100 -timeout 30 -o results.txt -ssl
```

# Referências

https://www.hackerone.com/blog/Guide-Subdomain-Takeovers

https://github.com/EdOverflow/can-i-take-over-xyz

https://opendata.rapid7.com/sonar.fdns_v2/

https://gist.github.com/haccer/3698ff6927fc00c8fe533fc977f850f8

https://github.com/arkadiyt/bounty-targets-data/blob/master/data/domains.txt
