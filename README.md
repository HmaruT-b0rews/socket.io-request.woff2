Aplicação Rails comum.

Uso uma biblioteca de daemon para verificar hosts em intervalos definidos na área administrativa.

A autenticação é feita na mão: um arquivo de sal na pasta de configuração é combinado com a senha do usuário para gerar o hash.

Vou escrever os testes automatizados com RSpec em breve, por enquanto testei manualmente.

Espero que goste do projeto.

Há um script de preparação para inicializar tudo:
- criando o primeiro usuário;
- gerando o sal e gravando na configuração.

Na página inicial o usuário entra no sistema.
![Initial view](doc_images/1.png)

Existe a tela onde os hosts são cadastrados, pelo link de novo host.
![Hosts](doc_images/2.png)

Os campos:
- Endereço: o que será verificado;
- Intervalo: o daemon é chamado a cada dez segundos; a última verificação somada ao intervalo precisa ser menor que o horário atual;
- Contador de falhas: zero quando a última verificação deu certo; se falhou, o valor anterior mais um;
- Última verificação: horário da última checagem em UTC. Considere o fuso horário mentalmente, se precisar.

Há também a tela de usuários:
![Users](doc_images/3.png)

Nada de segredo: se você editar um usuário e deixar a senha em branco, ela não muda.

O link de sair fica no topo da página.

# Preparação
No primeiro uso, rode o script de preparação da raiz do projeto:
- se o arquivo de sal não existir (ele está fora do controle de versão), ele é criado com o valor que você informar; se já existir, essa etapa é ignorada;
- se não houver usuário com o login informado, ele é criado.

# Daemon
A biblioteca usada tem o prefixo do time onde o autor dela trabalhava, e foi desenvolvida por um colega muito bom.

Usei essa biblioteca para disparar o comando de verificação em intervalos.

Para iniciar o processador:
```
fcld --command ping start
```

Para parar (o enunciado repete o comando de start por engano; o correto é usar a opção de parada):
```
fcld --command ping stop
```

Para ver o status:
```
fcld --command ping status
```

Quando a inicialização dá certo, a saída é simples e traz o identificador do processo.

O número do processo é só ilustrativo.

Se a saída vier enorme, com muitas linhas, pode ser um falso positivo — às vezes acontece. Basta parar e iniciar de novo, lembrando de parar antes.

Divirta-se!