### 1.通过Environment获取的
Environment.getDataDirectory().getPath()  
获得根目录/data 内部存储路径

Environment.getDownloadCacheDirectory().getPath()  
获得缓存目录/cache

Environment.getExternalStorageDirectory().getPath()  
获得SD卡目录/mnt/sdcard（获取的是手机外置sd卡的路径）

Environment.getRootDirectory().getPath()  
获得系统目录/system  
**这些获取的都是应用外部路径。**

### 2.通过Context获取的
Context.getFilesDir().getPath()  
用于获取APP的files目录 /data/data/\<application package\>/files

Context.getCacheDir().getPath()  
用于获取APP的cache目录 /data/data/\<application package\>/cache目录

Context.getExternalFilesDir().getPath()  
用于获取APP的在SD卡中的cache目录/mnt/sdcard/Android/data/\<application package\>/files

Context.getExternalCacheDir().getPath()  
用于获取APP的在SD卡中的cache目录/mnt/sdcard/Android/data/\<application package\>/cache  
**后两个获取的是应用内部路径，即随着应用卸载会一起删除掉。**
