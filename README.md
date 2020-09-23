﻿<div align="center">

## MP3 Archiver


</div>

### Description

This script will grab all the mp3's in a folder, get the Tags from them, ie.Title, Author, Album, Copyright, Comments, and will list them all on a page, in tables. Also it shows how to upload mp3's using a browser, and add them to that folder, which will then automatically add it to your list.
 
### More Info
 
You must input 3 variables.

$destination

$directory

$url

No side effects found as of yet.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Steve Oliver](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/steve-oliver.md)
**Level**          |Intermediate
**User Rating**    |4.9 (34 globes from 7 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Graphics/ Sound](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/graphics-sound__8-15.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/steve-oliver-mp3-archiver__8-249/archive/master.zip)





### Source Code

```
<?PHP
function SubmitSong($filename,$filename_name){
#Desination path that mp3s will be uploaded to.
$destination="/path/to/mp3/folder/";
if (ereg(".mp3",$filename_name)){
copy($filename,$destination.$filename_name);
echo "<h1>File Uploaded...</h1>";
echo "<b>$filename_name was uploaded succesfully.</b><br><br>";
echo "<a href=\"mp3.php\">Click here to go back.</a>";
}else{die("<h1>Not a Valid mp3 file...</h1>"); }
}
function main(){
###################################################################
#URL to directory containing your mp3s
$url="http://www.yoursite.com/mp3s/";
#actual location of your mp3 folder
$directory="path/to/mp3/folder/";
###################################################################
$count=0;
$handle=opendir($directory);
while (($filename = readdir($handle))!==false) {
if ($filename != "." && $filename != ".." && ereg(".mp3",$filename)) {
$count++;
echo "<b>Song Number: $count</b>";
getTag($filename,$directory,$url);}
}
closedir($handle);
####################################################################
#This part adds a submit form to allow people to upload mp3s to you.
#If they are on a slow connection the script will time out unless
#you change the timeout rate of your script.
?>
<form method="post" action="mp3.php" enctype="multipart/form-data">
Submit MP3:
<input type="file" name="filename" size="20" tabindex="5">
<input type="hidden" name="action" value="SubmitSong">
<input type="submit" value="Submit" tabindex="8">
</form><?
}
####################################################################
function getTag($filename,$directory,$url){
$filetitle=$filename;
$filename= $directory.$filename;
$fp=fopen($filename,"r");
fseek($fp,filesize($filename)-128);
$checktag=fread($fp,3);
echo "<table width='100%' border='1' bgcolor='#c0c0c0'><tr><td>";
echo "<b><a href=\"$url$filetitle\">$filetitle</a></b><br></tr></td><tr><tr><tr bgcolor='#FFFFFF'><td>";
if ($checktag=="TAG") {
echo "Size: ".number_format(filesize($filename)/1000,2)." KBytes<br>";
echo "Title: ".fread($fp,30)."<br>";
echo "Author: ".fread($fp,30)."<br>";
echo "Album: ".fread($fp,30)."<br>";
echo "Copyright: ".fread($fp,4)."<br>";
echo "Comments: ".fread($fp,30)."<br>";
echo "</td></tr></table><br>";
}else{
echo "Size: ".filesize($filename)." Bytes<br>";
echo "<font color=\"red\"><b>No Tag</b></font><br>";
echo "</td></tr></table><br>";
fclose($filename);}
}
switch ($action){
	default:
	main();
	break;
	case "SubmitSong":
	if ($filename=="none") {echo("<h1>No File Selected....</h1>"); break;}
	submitsong($filename,$filename_name);
	break;
}
?>
```

