# UploadToDriveUsingGoogleAPI
![Badge License](https://img.shields.io/badge/license-MIT-green)
![Badge Release](https://img.shields.io/badge/release-October-yellow)

This project has the aim to teach how to send local files to a folder on Google Drive, using Google Drive API, with <a href="https://console.cloud.google.com/">Google Cloud Console</a>

# ⚙️ Inicial Settings
For us to start the project, we need to create and configurate a project in Google Cloud Console, because without it we will not have access to the API. To do it follow the following steps:
* Create a project
* In the Enable APIs and services tab, enable Google Drive API
* In the same screen, click on Create credentials, selecting User Data, (click on next) filling all the mandatory fields, not filing with any scopes and in the OAuth Client ID part, select "Desktop app"
* Download the credentials, because they are necessary to the project operation.

In the code part, first of all, we must install the package <b>Google.Apis.Drive.v3</b> through NuGet, so we can execute the code.

# 🛠️ Project Operation
* I. We need to import the following libraries:
  
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
  
* II. Defining inicial parameters:
  * Here we declare, outside any method, 3 constants really necessary for the code operation, that are <b>sIdPastaDestino</b>, that contains the ID of the folder on Google Drive (https://drive.google.com/drive/folders/[Folder_ID]), <b>caminhoDoArquivoLocal</b>, that contains the path to the folder where the file is located, and finally <b>caminhoDasCredenciais</b>, where it tells us the path to the credentials downloaded in Inicial Settings.
 
         const string sIdPastaDestino = "ID of the folder on Google Drive";
         const string caminhoDoArquivoLocal = "Path to the folder where the file is located";
         const string caminhoDasCredenciais = "Path to the folder where the .json credential is located";

* III. Method <b>ObtemAutorizacao</b>:

  * In this method, we request authorization with the Google account owner of the .json credentials downloaded in the beginning of the documentation. The variable <b>tokenStorage</b>, will make the generated tokens be stored inside the folder %appdata%. This method will open a tab in your browser, and you should login with the account owner of the credentials. It will return the credentials.

* IV. Method <b>CriaDriveService</b>

  * The next step is to create a Drive Service, which, from the credentials generated in the previous step, will allow the application to do requests to the Google Drive API, in the name of the user that gave the necessary permissions during the login. It will return the service created.

* V. Method <b>CriarArquivoNoDrive</b>

  * This method has the objetive to encapsulate the metadata of a file that will be created in Google Drive. It allows us to define the name of the file and the destination folder, where the file will be stored. This part is <b>NOT</b> responsible for the upload of the files.
 
* VI. Method <b>UploadArquivoParaDrive</b>

  * Now, we do the upload of the files to Google Drive. Inicially we define the name of the file stored in our computer (variable <b>sNomeArquivoLocal</b>). After that, we will set the type of the file that we want to send, in this case PDF, how we can se in <b>"application/pdf"</b>. We will also need to define with which name, the files will be shown in Google Drive, and to do it we will create a string with the name <b>nomeDoArquivoNoDrive</b> in the Main method. It will return a link to the sent file.

* VII. Method <b>ExcluiArquivosNaPasta</b>

  * Method created to exclude all the files inside the folder in Google Drive.

* VIII. Method <b>Main</b>

  * Here is where the code will be executed. In my example, I follow the sequence of the created methods, with the option of exclusion of the files in order to clean the folder, and after that the upload is done to the folder.

        UserCredential credential = ObtemAutorização();
        var service = CriaDivreService(credential);
        string nomeDoArquivoNoDrive = "nomeArquivoNoDrive";
        var fileMetadata = CriaArquivoNoDrive(nomeDoArquivoNoDrive, sIdPastaDestino);
    
        //ExcluiArquivosNaPasta(service, sIdPastaDestino);
    
        string sLink = UploadArquivoParaDrive(service, fileMetadata, nomeDoArquivoNoDrive);

# ✔️ Used technologies
* C#

# Author
[<img loading="lazy" src="https://avatars.githubusercontent.com/u/98130340?v=4" width=115><br><sub>Guilherme Nascimento de Sousa</sub>](https://github.com/GuilhermeNSousa)
