Title: Downloading and Opening PDF File in Angular Cordova Project
Published: 1/27/2018
Tags: ["Cordova", "PDF", "Angular"]

---

Rough sample code with opening a PDF file (or any file really) using Cordova. 

### Plugins

This code uses plugins [fileOpener2](https://github.com/pwlin/cordova-plugin-file-opener2) and [FileTranser](https://github.com/apache/cordova-plugin-file-transfer).

```Typescript
import { Injectable } from '@angular/core';
import { AuthService } from './auth.service';

declare var FileTransfer: any;

class CallbackToPromise<T> {
    promise: Promise<T>;
    doResolve: (T) => void;
    doReject: (any?) => void;
    constructor() {
        this.promise = new Promise<T>((doResolve, doReject) => {
            this.doResolve = doResolve;
            this.doReject = doReject;
        });
    }
}

@Injectable()
export class DocumentService {
  constructor(private authService: AuthService) {
  }

  downloadDocumentCordova(url: string, fileName: string): Promise<void> {

    const callbackToPromise = new CallbackToPromise<void>();
    const fileTransfer = new FileTransfer();
    const uri = encodeURI(url);
    const fileURL = this.replaceWhitespaceWithUnderscore(window['cordova'].file.dataDirectory + fileName);
    console.log('Starting file download: ' + fileURL);
    fileTransfer.download(
      uri,
      fileURL,
      (entry) => {
        console.log('File download complete: ' + entry.toURL());
        this.openDocumentCordova(entry.toURL());
        callbackToPromise.doResolve(null);
      },
      (error) => {
        console.log(`Error occurred downloading file. Code: ${error.code} Source: ${error.source} Target: ${error.target}`);
        callbackToPromise.doReject(error);
      },
      false,
      {
        headers: {
          'Authorization': `Bearer ${this.authService.token}`
        }
      }
    );
    return callbackToPromise.promise;
  }

  private replaceWhitespaceWithUnderscore(str: string) {
    return str.replace(/\s/g, '_', ); // iOS does not like spaces in filenames
  }

  private openDocumentCordova(file: string) {
    window['cordova'].plugins.fileOpener2.open(
      file,
      'application/pdf',
      {
        error: (err) => {
          console.log('Error opening file. Error status: ' + err.status + ' - Error message: ' + err.message);
        },
        success: () => {
          console.log('File opened successfully');
        }
      }
    );
  }
}

```
