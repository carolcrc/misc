```js
// ATENÇÃO: Configure estas colunas antes de usar!
const COLUNA_CPF = 2;      // Mude para o número da coluna onde o CPF aparece (A=1, B=2, C=3, etc.)
const COLUNA_FOTO = 3;     // Mude para o número da coluna onde o link da foto aparece

function renomearFotoPeloCPF(e) {
  // Pega os dados da linha que acabou de ser adicionada pela resposta do formulário
  const valores = e.range.getValues()[0];
  
  // Pega o CPF e a URL da foto com base nos números das colunas que você configurou
  const cpf = valores[COLUNA_CPF - 1];
  const urlFoto = valores[COLUNA_FOTO - 1];
  
  // Verifica se o CPF e a URL da foto existem antes de continuar
  if (cpf && urlFoto) {
    try {
      const idFoto = extrairIdDoUrl(urlFoto);
      const arquivo = DriveApp.getFileById(idFoto);
      const extensao = arquivo.getName().split('.').pop();
      
      // Renomeia o arquivo para "CPF.extensao" (ex: 12345678900.jpg)
      arquivo.setName(cpf + '.' + extensao);
      
    } catch (erro) {
      // Registra um erro no histórico do script para você poder verificar se algo deu errado
      console.error("Erro ao renomear arquivo: " + erro.toString());
    }
  }
}

// Função auxiliar para pegar o ID do arquivo a partir da URL do Google Drive
function extrairIdDoUrl(url) {
  return url.match(/[-\w]{25,}/).toString();
}
```

## Como Configurar o Script

1. Abra a Planilha de Respostas:
Vá até o seu Google Forms, clique na aba "Respostas" e depois no ícone verde do Google Sheets para abrir a planilha onde os dados são salvos.

2. Abra o Editor de Scripts:
Com a planilha aberta, vá ao menu Extensões > Apps Script.
Apague todo o código que estiver no editor e cole o novo código acima.

3. Configure as Colunas:
Olhe a sua planilha de respostas e anote os números das colunas do CPF e da foto. A coluna A é 1, a B é 2, a C é 3, e assim por diante.
No topo do código, altere os números nas linhas const COLUNA_CPF = 2; e const COLUNA_FOTO = 3; para corresponder à sua planilha.
Exemplo: Se o CPF está na coluna C e a foto na coluna E, você deve alterar para const COLUNA_CPF = 3; e const COLUNA_FOTO = 5;.

4. Salve o Projeto:
Clique no ícone de Salvar projeto. Dê um nome ao projeto, como "Renomeador de fotos".

5. Crie o "Gatilho" Automático:
No menu à esquerda do editor de scripts, clique em Acionadores.
Clique no botão "+ Adicionar acionador" no canto inferior direito.
Configure a janela da seguinte forma:
Função a ser executada: renomearFotoPeloCPF
Escolha qual implantação deve ser executada: Principal
Selecione a origem do evento: Da planilha
Selecione o tipo de evento: No envio do formulário
Clique em Salvar.

6. Autorize o Script:
O Google pedirá permissão para que o script acesse seus arquivos e planilhas.
Escolha sua conta, clique em "Avançado", depois em "Acessar (nome do seu projeto)" e, por fim, em "Permitir".
