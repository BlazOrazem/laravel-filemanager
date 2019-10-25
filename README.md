# Laravel Filemanager  

[![Latest Stable Version](https://poser.pugx.org/unisharp/laravel-filemanager/v/stable)](https://packagist.org/packages/unisharp/laravel-filemanager)
[![Total Downloads](https://poser.pugx.org/unisharp/laravel-filemanager/downloads)](https://packagist.org/packages/unisharp/laravel-filemanager)
[![License](https://poser.pugx.org/unisharp/laravel-filemanager/license)](https://packagist.org/packages/unisharp/laravel-filemanager)

This is a Laravel Filemanager (LFM) fork which I created because I required workable file manager for   
Laravel ^6.4 on PHP ^7.2. For the official documentation please refer to the original repository at  
[https://github.com/UniSharp/laravel-filemanager](https://github.com/UniSharp/laravel-filemanager).  
  
## Version  
  
This version is a forked tag **v1.9.2** which is the latest official v1.* tag. Not many things are modified,
only the Laravel 6.4 compatibility is handled.

## Require LFM in Composer  

1. Open and edit **composer.json** file in your Laravel ^6.4 project
2. Add a package to your `require` section:
```
"require": {
  "unisharp/laravel-filemanager": "dev-master",
  ...
},
```
3. After the `require` section add a `repositories` section (if you don't have it already) and add:
```
"repositories": [{  
  "type": "package",  
  "package": {  
    "name": "unisharp/laravel-filemanager",  
    "version": "dev-master",  
    "source": {  
      "url": "git@github.com:BlazOrazem/laravel-filemanager.git",  
      "type": "git",  
      "reference": "laravel-64"  
    },  
    "autoload": {  
      "psr-4": {  
        "UniSharp\\LaravelFilemanager\\": "src/"  
      }  
    },  
    "extra": {  
      "laravel": {  
        "providers": [  
          "UniSharp\\LaravelFilemanager\\LaravelFilemanagerServiceProvider"  
        ]  
      }  
    }  
  }  
}],
```
4. Save and close the file and run **composer update** in your preferred terminal.

## Publish the configuration file

After the Composer concludes with the installation, run the command:
```
php artisan vendor:publish --tag=lfm_config
```
and edit the file `/config/lfm.php` accordingly (configure your middlewares, URL prefix, etc.).

## Setup LFM routes

Open your routes file, eg. `/routes/web.php` and add lines:
```
// Laravel file manager
Route::get('/laravel-filemanager', '\UniSharp\LaravelFilemanager\Controllers\LfmController@show');
Route::post('/laravel-filemanager/upload', '\UniSharp\LaravelFilemanager\Controllers\UploadController@upload');
```
Now run the commands:
```
php artisan route:clear
php artisan config:clear
```

## Initialize LFM in TinyMCE 4

This LFM is working properly with TinyMCE version 4. The version on which was tested is 4.5.3 - 4.9.6.
To initialize it, you need to add some Javascript to your template and include it after the TinyMCE integration.
```
$(document).ready(function() {
  var editor_config = {  
    path_absolute : "/admin/",  
    selector: "textarea.wysiwyg-editor",  
    plugins: [  
      "advlist autolink lists link image charmap print preview hr anchor pagebreak",  
      "searchreplace wordcount visualblocks visualchars code fullscreen",  
      "insertdatetime media nonbreaking save table contextmenu directionality",  
      "emoticons template paste textcolor colorpicker textpattern"  
    ],  
    toolbar: "insertfile undo redo | styleselect | bold italic | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | link image media",  
    relative_urls: false,  
    file_browser_callback : function(field_name, url, type, win) {  
      var x = window.innerWidth || document.documentElement.clientWidth || document.getElementsByTagName('body')[0].clientWidth;  
      var y = window.innerHeight|| document.documentElement.clientHeight|| document.getElementsByTagName('body')[0].clientHeight;  
  
      var cmsURL = editor_config.path_absolute + 'laravel-filemanager?field_name=' + field_name;  
      if (type == 'image') {  
        cmsURL = cmsURL + "&type=Images";  
      } else {  
        cmsURL = cmsURL + "&type=Files";  
      }  
  
      tinyMCE.activeEditor.windowManager.open({  
        file : cmsURL,  
        title : 'Filemanager',  
        width : x * 0.8,  
        height : y * 0.8,  
        resizable : "yes",  
        close_previous : "no"  
      });  
    }  
  };  
  
  tinymce.init(editor_config);
});
```
