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

TAG: movimentosdf12.sh, movimento, cupom, SDF, converter

- Acesso nos PDVs com sshpass (opcional):

Instalação via apt:

apt-get update
apt-get install sshpass

Download do pacote, direto:

64 bits:

wget -c http://br.archive.ubuntu.com/ubuntu/pool/universe/s/sshpass/sshpass_1.05-1_amd64.deb && dpkg -i sshpass*.deb

32 bits:
wget -c http://br.archive.ubuntu.com/ubuntu/pool/universe/s/sshpass/sshpass_1.05-1_i386.deb && dpkg -i sshpass*.deb

Exemplo de uso:

sshpass -p PWD ssh -o StrictHostKeyChecking=no USUÁRIO@192.168.169.203

- Dar permissão de execução e listar os arquivos ZMV

chmod +x movimentoSdf12.sh && ls -ltr *.ZMV

- Copiar .sh de outro PDV, caso já tenha, exemplo:

sshpass -p PWD scp -r USUÁRIO@192.168.169.204:/movimentoSdf12.sh . && chmod +x movimentoSdf12.sh && ls -ltr *.ZMV

- USANDO O SCRIPT:

Sintaxe do comando:

Arquivo.sh DATA ECF ARQUIVO.ZMV:

Exemplo:

./movimentoSdf12.sh 16072021 112 3403TDCZ.ZMV
ls -d1 16072021_1*/

Na pasta MOVIMENTO, se quiser zipar TODAS as pastas em 1 ZIP só:

apt-get install zip
zip -r ARQUIVO.zip $(ls -d1 16072021_1*/)
___
## Manager, Gerar Movimento SDF

Controle > Interfaces > Documentos eletrônicos em SDF:
Pagina direto: /manager/adm_geramovimento_manual.php5

O mesmo que usar o movimentoSdf12.sh (geratrab SOMA), só que pelo Manager.
Ele cria um arquivo .ZAN na pasta especificada.
Pode escolher entre SDF1 e SDF2

SDF1 = Cria o arquivo de SOMA (lnx_conv_XX -m)
SOMA SDF1, para enviar para a Controller, renomear o arquivo convertido para  nome original do arquivo, mas com a extenção .ZAN

SDF2 = Cria vários cupons (lnx_conv_XX -s)
Para quando a B\*S\* pede os arquivos de venda
___

## Corrigir data do movimento sdf

Após usar o seguinte comando com a sua resposta:

 ./movimentoSdf12.sh 06122021 105 2X03XCCZ.ZMV
“TRAB1.SDF” -> “2X03XCCZ.ZAN”
M330513.105
M330758.105
061221

Pode-se usar o cut, que tem suporte à coluna:

cut -c26-31 *.105

Então faz o comando mesclando com o sed:

sed -i 's/061221/071221/' *.105 | cut -c26-31
sed -i 's/061221/071221/' TRAB1.SDF | cut -c26-31
sed -i 's/061221/071221/' 2X03XCCZ.ZAN | cut -c26-31

- Mais informações:

M019-Movimento-em-Banco-de-Dados.pdf
Zan_M43 – Item de nota fiscal no PDV:

- Mais colunas:

Codigo_Mercadoria = Coluna 33 tamanho 17 = cut 33-49 *.ECF
Código da faixa de preço = Coluna 55 tamanho 3
Quantidade de mercadorias vendidas = Coluna 75 tamanho 9 = 75-83
Valor total vendido C 84 T 12 = 84-95
