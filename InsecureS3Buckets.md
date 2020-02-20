# Enumerando S3 Buckets

Encontrar possíveis *buckets* que possam servir de alvo é a primeira tarefa deste tipo de exploração, podem ser utilizar técnicas passivas de descoberta para não alertarmos o alvo sobre uma possível busca em seus servidores. A seguinte *dork* é útil para buscas dessas tecnologias em fontes abertas:

```
site:s3.amazonaws.com <target>
```

Mas é possível tambem automatizar esta tarefa utilizando a ferramenta [slurp](https://github.com/0xbharath/slurp) conforme trecho abaixo:

```
./slurp domain -t example.com
./slurp keyword -t example
```

Uma vez identificado possíveis *buckets* sobre domínios alvos deverá ser feito um reconhecimento sobre os níveis de permissão que estão atrelados aos mesmos para descobrir se há alguma má configuração de ACL que permita baixar ou escrever arquivos naquele *bucket*.

Antes de iniciar este processo crie uma conta na AWS e obtenha uma Access ID Key. Após isso será necessário configurar o cliente em sua máquina com os comandos:

```bash
apt install awscli
aws configure #configurar a key id
aws s3api get-bucker-acl --bucket example  #VERIFICAR ACLS
aws s3 ls s3://example      #LISTAR ARQUIVOS
aws s3 sync s3://example .  #SINCRONIZAR ARQUIVOS
```

