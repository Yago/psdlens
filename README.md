#PSDLENS
##A simple command line tool to analyse PSD
 
[PSDLENS](https://github.com/Yago31/psdlens) use the amazing [PSD.rb library](http://cosmos.layervault.com/psdrb.html) to provide you multiple reports about Photoshop files. You just have to define location and the data type you want. **This is the end of painful Photoshop webdesign integration.**

<hr />

##Install
    
You have to install [PSD.rb](http://cosmos.layervault.com/psdrb.html) first to run [PSDLENS](https://github.com/Yago31/psdlens)

```
$ gem install psd
$ gem install psdlens
```
<hr />

##To use</h2>

To get some data from your PSD files, just define wich directory contains your files and the method you want to use.

```
$ psdlens [path] [method]
```
<hr />


##The path

He can be absolute or current using **dot**

```
$ psdlens /path/to/your/dir/ meta
# or
$ psdlens . meta
```
<hr />

##Methods

* **meta** : Display all informations like size, color mode or used fontString
* **text** : Display text content from all PSD files and return font-family, font-size and color
* **font** : Display only used font
* **content** : Display only text conent
* **report** : Create a complete json report for all PSD

To help you :

```
$ psdlens -h
```
