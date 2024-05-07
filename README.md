# UploadToDriveUsingGoogleAPI
![Badge License](https://img.shields.io/badge/license-MIT-green)
![Badge Release](https://img.shields.io/badge/release-October-yellow)

Este projeto tem como finalidade, ensinar como enviar arquivos locais para dentro de uma pasta no Google Drive, usando a Google Drive API, dentro do <a href="https://console.cloud.google.com/">Google Cloud Console</a>

# ⚙️ Configurações iniciais
Para podermos iniciar o projeto, precisamos inicialmente criar e configurar um projeto no Google Cloud Console, pois sem ele não teremos acesso à API. Para isso seguiremos os seguintes passos:
* Criar um projeto
* Na aba <b>Ativar APIs e serviços</b>, ative a Google Drive API
* Na mesma tela, clique em <b>Criar credenciais</b>, selecionando Dados do usuário, preenchendo todos os campos obrigatórios, não colocando escopos e em ID do Cliente OAuth, selecione "App para Computador"
* Baixe as credenciais, pois elas serão necessárias para o funcionamento do projeto.
* Na aba <b>OAuth consent screen</b>, publique o projeto.

Na parte do código, antes de tudo, precisamos baixar o pacote <b>Google.Apis.Drive.v3</b> através do NuGet, para conseguirmos executar o código.

# 🛠️ Funcionamento do projeto
* I. Precisamos importar as seguintes bibliotecas:
  
        using Google.Apis.Auth.OAuth2;
        using Google.Apis.Drive.v3;
        using Google.Apis.Drive.v3.Data;
        using Google.Apis.Services;
        using Google.Apis.Util.Store;
        using System;
        using System.IO;
        using System.Diagnostics;
        using System.Threading;
        using System.Text;
        using Google.Apis.Auth.OAuth2.Flows;
        using System.Net;
  
* II. Definindo parâmetros iniciais:
  * Aqui declaramos, fora de qualquer método, 3 constantes muito necessárias para o funcionamento do código, que são <b>sIdPastaDestino</b>, que contém o ID da pasta no Google Drive (https://drive.google.com/drive/folders/[Id da pasta]), <b>caminhoDoArquivoLocal</b>, que contém o caminho para a pasta onde o arquivo está localizado, e finalmente <b>caminhoDasCredenciais</b>, onde informa o caminho para as credenciais baixadas nas Configurações Iniciais.
    
         const string sIdPastaDestino = "Id da pasta desejada no Google Drive";
         const string caminhoDoArquivoLocal = "Caminho para a pasta em que o arquivo está localizado";
         const string caminhoDasCredenciais = "Caminho para a pasta em que a credencial em .json está localizada";

* III. Método <b>ObtemAutorizacao</b>:
  
  * Neste método, solicitamos autorização com a conta Google dona das credenciais .json baixadas no inínio da documentação. A variável <b>tokenStorage</b> fará com que os tokens gerados pelo método, sejam armazenados dentro da pasta %appdata%. Este método abrirá uma tela no navegador e você deverá logar com a conta dona das credenciais. Será retornado as credenciais.
  * Importante: Em "caminhoDasCredenciais", utilize o caminho "NomeDoProjeto\bin\Debug" para armazenar o arquivo JSON, e quando declarar a string, coloque somente o nome do arquivo com .json no final.

* IV. Método <b>CriaDriveService</b>

  * O próximo passo é criar um Drive Service, que, a partir das credenciais geradas no passo anterior, permitirá que a aplicação faça chamadas à API do Google Drive em nome do usuário que deu as permissões necessárias ao realizar o login. Será retornado o service criado.

* V. Método <b>CriarArquivoNoDrive</b>

  * Este método serve para encapsular a criação dos metadados de um arquivo que será criado no Google Drive. Ele permite definir o nome do arquivo e a pasta de destino onde o arquivo será colocado. Vale ressaltar que esta parte não é responsável pelo upload dos arquivos.
 
* VI. Método <b>UploadArquivoParaDrive</b>

  * Agora realizaremos o upload dos arquivos para o Drive. Inicialmente definiremos o nome do arquivo armazenado em nosso computador (variável <b>sNomeArquivoLocal</b>. Após isso vamos colocar o tipo de arquivo que queremos enviar, no caso PDF, como podemos ver entre aspas em <b>"application/pdf"</b>. Também precisaremos definir com qual nome os arquivos aparecerão no drive, para isso criaremos uma string com o nome de nomeDoArquivoNoDrive no método Main. Será retornado um link para o arquivo enviado.

* VII. Método <b>ExcluiArquivosNaPasta</b>

  * Método criado para excluão de arquivos dentro da pasta desejada.

* VIII. Método <b>Main</b>

  * Aqui é onde o código será executado. No meu exemplo, eu sigo a ordem dos métodos criados, com a opção de exclusão dos arquivos para limpeza da pasta, e depois é realizado o Upload para a pasta desejada

        UserCredential credential = ObtemAutorização();
        var service = CriaDivreService(credential);
        string nomeDoArquivoNoDrive = "nomeArquivoNoDrive";
        var fileMetadata = CriaArquivoNoDrive(nomeDoArquivoNoDrive, sIdPastaDestino);
    
        //ExcluiArquivosNaPasta(service, sIdPastaDestino);
    
        string sLink = UploadArquivoParaDrive(service, fileMetadata, nomeDoArquivoNoDrive);

# ✔️ Tecnologias utilizadas
* C#

# Autor
[<img loading="lazy" src="https://avatars.githubusercontent.com/u/98130340?v=4" width=115><br><sub>Guilherme Nascimento de Sousa</sub>](https://github.com/GuilhermeNSousa)
