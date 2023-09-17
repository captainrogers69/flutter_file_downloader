# flutter_file_downloader

A simple flutter plugin that downloads all files types to downloads directory in all android devices.
When android 10 came out, privacy restrictions were changed in big way
and there are not enough flutter-related info about it,
so I came up with a simple ANDROID plugin to downloads any file type to downloads directory
Also it has callbacks and progress listeners with a very easy installation
Note: This plugin is not built for iOS, it will not effect it at all.

## Getting Started

First, make sure that you've added the permissions to your AndroidManifest.xml
```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

Add the following line to your pubspec.yaml  
``` flutter_file_downloader: ^1.1.3```

Next,  
    add the library import to your dart file,  
```
import 'package:flutter_file_downloader/flutter_file_downloader.dart';
```

Last step,  
    use the library easily in your code  
```
//You can download a single file
FileDownloader.downloadFile(
    url: _YOUR DOWNLOAD URL_,
    name: **OPTIONAL**, //THE FILE NAME AFTER DOWNLOADING,
    onProgress: (String fileName, double progress) {
      print('FILE fileName HAS PROGRESS $progress');
    },
    onDownloadCompleted: (String path) {
      print('FILE DOWNLOADED TO PATH: $path');
    },
    onDownloadError: (String error) {
      print('DOWNLOAD ERROR: $error');
    });
    
//Or download multiple files 
final List<File?> result = await FileDownloader.downloadFiles(
    urls: [
        'https://cdn.mos.cms.futurecdn.net/vChK6pTy3vN3KbYZ7UU7k3-320-80.jpg',
        'https://fansided.com/files/2015/10/cat.jpg',
    ],
    isParallel: true,   //if this is set to true, your download list will request to be downloaded all at once
                        //if your downloading queue fits them all, they all will start downloading
                        //if it's set to false, it will download every file individually
                        //default is true
    onAllDownloaded: () {
      //This callback will be fired when all files are downloaded
    });
    //This method will return a list of File? in the same order as the urls,
    //so if the url[2] failed to download, 
    //then result[2] will be null
    
//Also, you can enable or disable the log, this will help you track your download batches
FileDownloader.setLogEnabled(true);
//default is false

//You can set the number of consecutive downloads to help you preserving device's resources 
FileDownloader.setMaximumParallelDownloads(10);
    //This method will allow the plugin to download 10 files at a time
    //if you requested to download more than that, it will wait until a download is done to start another
    //default is 25, maximum is 25, minimum is 1
```

Also, you can change the download destination by passing downloadDestination,
this can by one of the enum's values:
- `DownloadDestinations.publicDownloads //(DEFAULT)` 
- `DownloadDestinations.appFiles`
## Example:
```
FileDownloader.downloadFile(
    url: url.text.trim(),
    name: name.text.trim(),
    downloadDestination: DownloadDestinations.appFiles,
);
```

And you can change download notifications to be:
- all enabled: progress, success & failed notifications will be delivered to the user
- all disabled: no notifications will be displayed to the user, this option require you to add a permission in the manifest `android.permission.DOWNLOAD_WITHOUT_NOTIFICATION`
- or the default, only progress notifications will appear to the user
this can by one of the enum's values:
- `NotificationType.all`
- `NotificationType.progressOnly //(DEFAULT)`
- `NotificationType.disabled`
## Example:
```
FileDownloader.downloadFile(
    url: url.text.trim(),
    name: name.text.trim(),
    notificationType: NotificationType.all,
);
```
<br></br>

## Examples:
**All callbacks are nullables, you can simply call** `FileDownloader.downloadFile(url: YOUR_URL);`
```
FileDownloader.downloadFile(
    url: "https://tinypng.com/images/social/website.jpg",
    name: "PANDA",
    onDownloadCompleted: (path) {
        final File file = File(path);
        //This will be the path of the downloaded file
    });
```
```
final List<File?> result = await FileDownloader.downloadFiles(
    urls: [
        'https://cdn.mos.cms.futurecdn.net/vChK6pTy3vN3KbYZ7UU7k3-320-80.jpg',
        'https://fansided.com/files/2015/10/cat.jpg',
    ],
    isParallel: false);
    
print('FILES: ${result.map((e) => e?.path).join(',\n')}');
```
```
final File? file = await FileDownloader.downloadFile(
    url: "https://tinypng.com/images/social/developer-api.jpg",
    name: "ANOTHER PANDA.jpg");

print('FILE: ${file?.path}');
```

You can also track the progress if you want to add a progress bar
```
final File? file = await FileDownloader.downloadFile(
    url: "https://tinypng.com/images/social/developer-api.jpg",
    name: "ANOTHER PANDA.jpg",
    onProgress: (String? fileName, double progress) {
        setState(() => _progress = progress);
    });

print('FILE: ${file?.path}');`
```

## Contributing
All contributions are welcome!

If you like this project then please click on the 🌟 it'll be appreciated or if you wanna add more epic stuff you can submit your pull request and it'll be gladly accepted 🙆‍♂️

or if you found any bug or issue do not hesitate opening an issue on github
