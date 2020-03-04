# LinksForReferences

Image upload using Angular and PHP: [here](https://therichpost.com/angular-image-upload-with-preview-and-save-to-folder-with-php/)

1. Angular Code app.component.ts file

```
  import {Component} from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  public imagePath;
  constructor(private http: HttpClient) {}
  url: string;
  onSelectFile(event) { // called each time file input changes
    if (event.target.files && event.target.files[0]) {
      var reader = new FileReader();
           this.imagePath = event.target.files;
       for (const file of this.imagePath) {
       const formData = new FormData();
       formData.append('image', file, file.name);
           }
       const headers = new HttpHeaders();
           headers.append('Content-Type', 'multipart/form-data');
           headers.append('Accept', 'application/json');
       this.http.post('http://localhost/mypage.php', formData).subscribe( headers, console.log(file.name) );
    
       reader.readAsDataURL(event.target.files[0]); // read file as data url
      
       reader.onload = (event) => { // called once readAsDataURL is completed
      this.url = event.target.result;
      
      }
    }
  }
}
```

2. app.component.html file

```
<div class="jumbotron text-center">
  <h3>Angular6 Image Upload to php:</h3>
</div>
<div style="text-align:center">
<img [src]="url" height="200" *ngIf="url"> <br/>
<input type='file' (change)="onSelectFile($event)">
</div>
```

3. Here php file upload code and need to add your php file

```
<?php
move_uploaded_file($_FILES["image"]["tmp_name"], "img/" . $_FILES["image"]["name"]);
?>
```
