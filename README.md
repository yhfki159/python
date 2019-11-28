# python
# tempfile
主要有以下幾個函數：
tempfile.TemporaryFile
################################################################################
  
如何你的應用程式需要一個暫存檔案來存儲資料，但不需要同其他程式共用，那麼用TemporaryFile函數創建暫存檔案是最好的選擇。
  
其他的應用程式是無法找到或打開這個檔的，因為它並沒有引用檔案系統表。用這個函數創建的暫存檔案，關閉後會自動刪除。
``` python
import os
import tempfile

print 'Building a file name yourself:'
filename = '/tmp/guess_my_name.%s.txt' % os.getpid()
temp = open(filename, 'w+b')
try:
    print 'temp:', temp
    print 'temp.name:', temp.name
finally:
    temp.close()
    os.remove(filename)     # Clean up the temporary file yourself
 
print
print 'TemporaryFile:'
temp = tempfile.TemporaryFile()
try:
    print 'temp:', temp
    print 'temp.name:', temp.name
finally:
    temp.close()　　# Automatically cleans up the file
``` 
$ python tempfile_TemporaryFile.py  
這個例子說明了普通創建檔的方法與TemporaryFile()的不同之處  
注意：用TemporaryFile()創建的檔沒有檔案名  
  
Building a file name yourself:  
temp: <open file '/tmp/guess_my_name.14932.txt', mode 'w+b' at 0x1004481e0>  
temp.name: /tmp/guess_my_name.14932.txt  
TemporaryFile:  
temp: <open file '<fdopen>', mode 'w+b' at 0x1004486f0>  
temp.name: <fdopen>  
  
預設情況下使用w+b許可權創建檔，在任何平臺中都是如此，並且程式可以對它進行讀寫。 
################################################################################   
``` python
import os
import tempfile
 
temp = tempfile.TemporaryFile()
try:
    temp.write('Some data')
    temp.seek(0)
    print temp.read()
finally:
    temp.close()
``` 
$ python tempfile_TemporaryFile_binary.py  
寫入後，需要使用seek()，為了以後讀取資料。
################################################################################
  
如果你想讓檔以text模式運行，那麼在創建的時候要修改mode為'w+t'  
``` python
import tempfile
 
f = tempfile.TemporaryFile(mode='w+t')
try:
    f.writelines(['first\n', 'second\n'])
    f.seek(0)
    for line in f:
        print line.rstrip()
finally:
    f.close()
``` 
$ python tempfile_TemporaryFile_text.py  
################################################################################
