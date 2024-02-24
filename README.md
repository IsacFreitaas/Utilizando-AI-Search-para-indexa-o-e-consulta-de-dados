# Azure Cognitive Search: utilizando AI Search para fazer indexação e consulta de dados.


Imagine que trabalhamos em uma empresa de café multinacional, estilo Starbucks, e estamos recebendo muitas reclamações de clientes.

----------------------

### Parte 1.1- Criação de recursos.

----------------------

Antes de usarmos as ferramentas do Azure AI Search, precisamos criar dois recursos no portal da Azure para serem utilizados. Para isso, você precisa ter uma conta Microsft.

Pra criarmos os recursos, vamos entrar em "https://www.portal.azure.com" e clicar em "Criar um recurso".
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/65ad4ef6-f371-4662-98c4-041fb666006e" width="800px"/>
</div>

Agora, na caixa de pesquisas, você pesquisa "Azure AI Search", e seleciona o primeiro resultado.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/3cfeb942-6328-401e-a89a-92f67006faf5" width="800px"/>
</div>
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/8f079b21-40c6-465d-82f6-787bdf5f8fe3" width="800px"/>
</div>

Aqui, você deverá selecionar ou criar um grupo de recursos, definir a região, o nome e um preço.




No meu caso, vou selecionar um grupo de recuros já existente, chamado "LaboratórioAi900DIOLAB".



Descendo mais ou pouco vou criar o nome do meu recurso, que será "aisearch52".



Mais embaixo, recomendo que você você sempre selecione servidores de local americano. No meu caso, vou selecionar "East US".



Em "camada de preços" você vai aterar o tipo de preço para "Básico".



Então após tudo preenchido, ficou da seguinte forma:
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/bf92ddae-be5e-475d-8849-d14543728b1b" width="800px"/>
</div>

Após isso, você clica em revisar + criar, espera a validação, verifica se preencheu tudo corretamente e cria o recurso.



Então é só esperar a implantação e ir para o próximo passo.

----------------------

### Parte 1.2- Criação de recursos.

----------------------

Agora voltamos a página inicial do Azure, e vamos novamente em "Criar um recurso".

<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/40ca32a5-5a73-481d-ae32-9749aab93a94" width="800px"/>
</div>

Pesquisamos "Azure AI Services" ou somente "serviços cognitivos" e criamos, da mesma forma que o recurso anterior.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/9ea9a4e0-b210-4ef2-87f4-74f0a46a137a" width="800px"/>
</div>
*tipo de preço standard S0

----------------------

### Parte 1.3- Criação de conta de armazenamento.

----------------------

Agora precisaremos criar uma conta de armazenamento para nosso projeto.



Para isso, vamos a página inicial do portal da Azure e clicamos na opção lá em cima, escrito contas de armazenamento, e vamos em criar.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/480657a5-d1f8-4729-bd8e-9ba3ed3acee7" width="800px"/>
</div>

Agora na parte de criação, verifique se o resource group selecionado é o mesmo que selecionamos nos dois recursos anteriores.



Na parte de nome da conta de armazenamento, vou definir como "projetosearchaidio" (lembre-se que o nome da conta de armazenamento tem que ser um nome único, que nunca foi usado).



Região eu sempre coloco East US.



Em performance, recomendo que deixe em Standard, pois não mudará nada em nosso projeto.



E em redundância, selecione LRS (armazenamento com redundância local)



Então essas foram as configurações. Ficou da seguinte maneira:
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/dc57c643-7e7f-4090-b3d8-85366bdbebb6" width="800px"/>
</div>

Após isso você clica em "Examinar", espera a validação final, e clica em "Criar", e espera a implantação ser finalizada.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/274ef144-0e98-4085-bd5e-bf160a4e1b8f" width="800px"/>
</div>

Agora precisamos quebrar algumas regras de segurança de nossa conta de armazenamento para nosso projeto. Básicamente vamos liberar para acesso anônimo.



Para isso, assim que terminar a implantação, você clica em "Ir para o recurso" (ou volte novamente a parte de contas de armazenamento e localize a que criou para o projeto), e na aba lateral, desce até "Configuração".
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/42b8d185-7abd-41a8-afd6-4fe6475bf709" width="800px"/>
</div>

Agora vamos habilitar o acesso anônimo ao Blob e salvar, como é mostrado na imagem abaixo.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/58b94e72-7058-4f6d-9696-6f635ee826ef" width="800px"/>
</div>

----------------------

### Parte 1.4- Criação e configuração de contêiner na conta de armazenamento. 

----------------------

Agora precisamos criar um contêiner para armazenar os dados que iremos usar. Para isso voltamos para a página da nossa conta de armazenamento e vamos a aba contêiner.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/1c167040-7e10-4efa-83d3-86e523a70a36" width="800px"/>
</div>

Agora vamos criar um contêiner (utilize as mesmas configurações que eu pôr).
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/98824b79-4fa5-4b26-bcd0-cae4c3d9d1d7" width="800px"/>
</div>

----------------------

### Parte 1.5- Upload dos arquivos.

----------------------

Agora precisamos fazer o upload dos arquivos que vamos usar no AI Search pra fazer a indexação e consulta de dados. 




Para isso, eu deixei um link lá no final desse readme, com um link para a documentação oficial desse projeto.
<div align="center">
<img src="" width="800px"/>


Na etapa que estamos, vai ter um link para você baixar. É um arquivo zip com alguns documentos dentro.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/754db4df-1ce2-4f9a-8dae-e6d76fb75e4e" width="800px"/>



Você vai usar algum programa para descompactar esses arquivos. Então, na aba contêiner da nossa conta de armazenamento, você seleciona o contêiner que criamos e clica em upload, e seleciona os arquivos.
<div align="center">
<img src="https://github.com/IsacFreitaas/Utilizando-AI-Search-para-indexa-o-e-consulta-de-dados/assets/65254733/1088af84-b4aa-43e0-927d-bcd7593fdd20" width="800px"/>
  
----------------------

### Parte 2- Importando dados para o recurso de AI Search.

----------------------

Agora voltamos ao recurso que criamos inicialmente, e importamos os nossos dados da conta de armazenamento que importamos anteriormente os arquivos.
<div align="center">
<img src="" width="800px"/>



Obrigado por me acompanhar nestes testes até aqui!

--------------------------

Então é isso.

# Este foi o meu primeiro projeto de Language Studio da Microsoft Azure ML, em meus estudos em Ciência de Dados.

Obrigado pela atenção!
--------------------------
LINK para a Documentação oficial:



### 1- https://aka.ms/ai900-ai-search



**Qualquer dúvida sobre o projeto, ou caso precise de auxílio, fico a disposição. Entre em contato comigo por meio de minhas redes sociais (listadas abaixo ou em meu perfil).

--------------------------

Sobre mim:
# Isac Freitas
Estudante de Ciência de Dados e Inteligência Artificial.

### Me encontre:

Insta: @isac.sfreitas



Twitter: @isaczeitgeist
