# Sugestão de como renomear arquivos no Drive (não testado) 

## Como Encontrar o ID da Pasta do Google Drive:

- Abra o Google Drive.
- Navegue até a pasta que contém as fotos que você quer renomear.
- Olhe a URL no seu navegador. O ID da pasta é a sequência de letras e números que aparece depois de folders/.
- Exemplo: Se a URL for https://drive.google.com/drive/folders/1a2b3c4d5e6f7g8h9i0j, o ID da pasta é 1a2b3c4d5e6f7g8h9i0j.

## Como Executar o Script:

- Crie uma nova Planilha Google.
- No menu, vá em Extensões > Apps Script.
- Apague todo o código que aparecer e cole o código acima.
- Clique no ícone de Salvar projeto.
- Recarregue a página da sua planilha.
- Um novo menu chamado "Renomear Arquivos" aparecerá na sua planilha.
- Clique em Renomear Arquivos > Iniciar.
- Uma caixa de diálogo aparecerá. Cole o ID da pasta que você copiou e clique em "OK".
- O script irá pedir autorização para acessar seu Google Drive. Conceda a permissão.
- Aguarde a mensagem de confirmação.


```js
function onOpen() {
  SpreadsheetApp.getUi()
      .createMenu('Renomear Arquivos')
      .addItem('Iniciar', 'renameFiles')
      .addToUi();
}

function renameFiles() {
  var ui = SpreadsheetApp.getUi();
  var result = ui.prompt(
      'Renomear Arquivos',
      'Por favor, cole o ID da pasta do Google Drive:',
      ui.ButtonSet.OK_CANCEL);

  var button = result.getSelectedButton();
  var folderId = result.getResponseText();

  if (button == ui.Button.OK) {
    try {
      var folder = DriveApp.getFolderById(folderId);
      var files = folder.getFiles();
      var count = 1;
      
      while (files.hasNext()) {
        var file = files.next();
        var extension = file.getName().split('.').pop();
        if (extension) {
          file.setName('CPF_' + count + '.' + extension);
          count++;
        }
      }
      
      ui.alert('Arquivos renomeados com sucesso!');
    } catch (e) {
      ui.alert('Ocorreu um erro. Verifique se o ID da pasta está correto e tente novamente.');
    }
  }
}
```
