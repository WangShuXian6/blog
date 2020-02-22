#### Angular 7 上传图片

>upload.component.html

```html
<div class="example-full-width">
                                <input placeholder="请上传公司logo" type="file"
                                       (change)="onFileUpload($event)" accept="image/*"
                                       name="logo" #selectFile>
                                <img [src]="imagePreview" >
                            </div>
```
>upload.component.ts

```ts
import {Component, OnInit, ChangeDetectionStrategy} from '@angular/core';
import {FormControl} from '@angular/forms';
import {ENTER, COMMA} from '@angular/cdk/keycodes';
import {Observable} from 'rxjs';
import {startWith, map} from 'rxjs/operators';
import {MatChipInputEvent} from '@angular/material';
import {MatSnackBar} from '@angular/material';
import {ImageService} from '../../services/image.service';

export class State {
    constructor(
        private imageService: ImageService) {
    }
}

@Component({
    selector: 'upload',
    templateUrl: './upload.component.html',
    styleUrls: ['./upload.component.scss'],
    changeDetection: ChangeDetectionStrategy.OnPush,
    styles: [`
        .example-container {
            display: flex;
            flex-direction: column;
        }

        .example-full-width {
            width: 100%;
        }

        .m-checkbox-inline > mat-checkbox {
            padding-right: 20px;
        }
    `]
})
export class UploadComponent implements OnInit {
    public model: any = {
        name: '',
        phone: '',
        email: '',
        company: '',
        logo: '',
        number: '',
        id: '',
        password: '',
        rePassword: ''
    };

    selectedFile: File;
    imagePreview: string;

    constructor(
        private imageService: ImageService
    ) {
    }

    ngOnInit() {
    }

    onFileUpload(event) {
        this.selectedFile = event.target.files[0];
        this.imageService.uploadImage(this.selectedFile)
            .subscribe((imageData) => {
                console.log('imageData--', imageData);
            });
        // const reader = new FileReader();
        // reader.onload = () => {
        //     this.imagePreview = reader.result;
        // };
        // reader.readAsDataURL(this.selectedFile);
    }
}
```

>image.service.ts

```ts
import {Injectable} from '@angular/core';
import {HttpClient} from '@angular/common/http';
import {ImageApi} from '../../config/api';

@Injectable({
    providedIn: 'root'
})
export class ImageService {

    constructor(private http: HttpClient) {
    }

    uploadImage(selectedFile) {
        // return this.http.post(ImageApi, selectedFile);
        const uploadFormData = new FormData();
        uploadFormData.append('file', selectedFile, selectedFile.name);
        return this.http.post(ImageApi, uploadFormData);
    }
}
```

>主app

>pages.module.ts

```ts
import {LayoutModule} from '../layout/layout.module';
import {NgModule} from '@angular/core';
import {CommonModule} from '@angular/common';
import {PagesRoutingModule} from './pages-routing.module';
import {PagesComponent} from './pages.component';
import {CoreModule} from '../../core/core.module';
import {FormsModule} from '@angular/forms';
import {HttpClientModule} from '@angular/common/http';

@NgModule({
    declarations: [
        PagesComponent,
    ],
    imports: [
        CommonModule,
        HttpClientModule,
        FormsModule,
        PagesRoutingModule,
        CoreModule,
        LayoutModule,
    ],
    providers: []
})
export class PagesModule {
}
```