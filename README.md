# pdv-geratrav-mov

Geratrab  
Movimento SDF 1  
Movimento SDF 2  
___
## geratrab SOMA

```bash
lnx_conv_XX
```
>XX = ZMAN Correspondente: CZ, CW, etc...

Opções:

>-s = Cria vários cupons (SDF 2)  
>-m Cria o arquivo de SOMA (SDF 1)  

Renomear o arquivo ZMV copiado para "**TH01HKXX.TRA**".  
SOMA SDF1, para enviar para a Cont\*er, renomear o arquivo convertido para  nome original do arquivo, mas com a extenção .ZAN
___

## Movimento SDF 1 e 2

>TAG: movimentosdf12.sh, movimento, cupom, SDF, converter  

- Acesso nos PDVs com sshpass (opcional):

Instalação via apt:
```bash
apt-get update
```
```bash
apt-get install sshpass
```

- Download do pacote, direto:  

>64 bits:  
```bash
wget -c http://br.archive.ubuntu.com/ubuntu/pool/universe/s/sshpass/sshpass_1.05-1_amd64.deb && dpkg -i sshpass*.deb
```
>32 bits:
```bash
wget -c http://br.archive.ubuntu.com/ubuntu/pool/universe/s/sshpass/sshpass_1.05-1_i386.deb && dpkg -i sshpass*.deb
```

- Exemplo de uso:
```bash
sshpass -p PWD ssh -o StrictHostKeyChecking=no USUÁRIO@192.168.169.203
```
- Dar permissão de execução e listar os arquivos ZMV
```bash
chmod +x movimentoSdf12.sh && ls -ltr *.ZMV
```
- Copiar .sh de outro PDV, caso já tenha, exemplo:
```bash
sshpass -p PWD scp -r USUÁRIO@192.168.169.204:/movimentoSdf12.sh . && chmod +x movimentoSdf12.sh && ls -ltr *.ZMV
```
- USANDO O SCRIPT:

Sintaxe do comando:  

>Arquivo.sh **DATA** **ECF** ARQUIVO**.ZMV**:  

Exemplo:
```bash
./movimentoSdf12.sh 16072021 112 3403TDCZ.ZMV
```
```bash
ls -d1 16072021_1*/
```
Na pasta MOVIMENTO, se quiser zipar TODAS as pastas em 1 ZIP, instale o comando zip e faça o comando:
```bash
apt-get install zip
```
```bash
zip -r ARQUIVO.zip $(ls -d1 16072021_1*/)
```
___
## PDV, Gerar Movimento SDF manualmente

Verificar data dos arquivos ZMV
```bash
ls -ltr *.ZMV
```
Verificar assinatura CODFON do PDV
```bash
cat ETIQUETL.TXT
```
Criar diretório com data e número do ECF e copiar o arquivo ZMV com conversor
```bash
mkdir 11062025_12
```
```bash
cp -a 0C02YGCZ.ZMV 11062025_12
```
```bash
cp -a lnx_conv_CZ* 11062025_12
```

Acessar o diretório e "converter" arquivo ZMV para arquivo TRA
```bash
cd 11062025_12
```
```bash
cp -a 0C02YGCZ.ZMV TH01HKCZ.TRA
```

Converter arquivo TRA para cupons de venda e arquivo de SOMA

>-s = Cria vários cupons (SDF 2)  
>-m Cria o arquivo de SOMA (SDF 1)  
```bash
./lnx_conv_CZ_u64 -s
```
```bash
./lnx_conv_CZ_u64 -m
```

Verificar data gravada nos arquivos de cupons do ECF
```bash
cut -c26-31 *.012
```
___
## Manager, Gerar Movimento SDF

\* Documentos eletrônicos em SDF: \*adm_geramovimento_manual.php\*  

O mesmo que usar o movimentoSdf12.sh (geratrab SOMA), só que pelo Manager.  
Ele cria um arquivo .ZAN na pasta especificada.  
Pode escolher entre SDF1 e SDF2  

- SDF1 = Cria o arquivo de SOMA (lnx_conv_XX -m)  
SOMA SDF1, para enviar para a Controller, renomear o arquivo convertido para  nome original do arquivo, mas com a extenção .ZAN  

- SDF2 = Cria vários cupons (lnx_conv_XX -s)  
Para quando a B\*S\* pede os arquivos de venda  
___

## Corrigir data do movimento sdf

Após usar o seguinte comando com a sua resposta:
```bash
 ./movimentoSdf12.sh 06122021 105 2X03XCCZ.ZMV
```
>“TRAB1.SDF” -> “2X03XCCZ.ZAN”  
>M330513.105  
>M330758.105  
>061221  

Pode-se usar o cut, que tem suporte à coluna:  
```bash
cut -c26-31 *.105
```
Então faz o comando mesclando com o sed:  
```bash
sed -i 's/061221/071221/' *.105 | cut -c26-31
```
```bash
sed -i 's/061221/071221/' TRAB1.SDF | cut -c26-31
```
```bash
sed -i 's/061221/071221/' 2X03XCCZ.ZAN | cut -c26-31
```

- Mais informações:

M019-Movimento-em-Banco-de-Dados.pdf  


- Zan_M43 – Item de nota fiscal no PDV:  

Codigo_Mercadoria = Coluna 33 tamanho 17 = cut 33-49 *.ECF  
Código da faixa de preço = Coluna 55 tamanho 3  
Quantidade de mercadorias vendidas = Coluna 75 tamanho 9 = 75-83  
Valor total vendido C 84 T 12 = 84-95  

