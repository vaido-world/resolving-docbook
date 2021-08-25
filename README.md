# resolving-docbook


Test: `Compile GTK-Doc 1.33.0`

## The error
```
checking whether stripping libraries is possible... yes
checking if libtool supports shared libraries... yes
checking whether to build shared libraries... yes
checking whether to build static libraries... yes
checking for pkg-config... /usr/bin/pkg-config
checking pkg-config is at least version 0.19... yes
checking for a Python interpreter with version >= 3.2... python3
checking for python3... /usr/bin/python3
checking for python3 version... 3.8
checking for python3 platform... linux
checking for python3 script directory... ${prefix}/lib/python3.8/site-packages
checking for python3 extension module directory... ${exec_prefix}/lib/python3.8/site-packages
checking for xsltproc... /usr/bin/xsltproc
checking for dblatex... no
checking for fop... no
configure: WARNING: neither dblatex nor fop found, so no pdf output from xml
checking for XML catalog... /Data/Variable/lib/xml/catalog
checking for xmlcatalog... /usr/bin/xmlcatalog
checking for DocBook XML DTD V4.3 in XML catalog... not found
configure: error: could not find DocBook XML DTD V4.3 in XML catalog
PrepareProgram: configure failed.
Compile: GTK-Doc 1.33.0 - Configuration failed.

```

## The Solution
```
sed -i '/delegatePublic/c\' /Data/Variable/lib/xml/catalog  
sed -i '/delegateSystem/c\' /Data/Variable/lib/xml/catalog  
sed -i '/delegateURI/c\' /Data/Variable/lib/xml/catalog  
bash  /Programs/DocBook-XML-DTD/4.5/Resources/PostInstall
```

Test: `Compile GTK-Doc 1.33.0`

## The Error
```
make[1]: Leaving directory '/Data/Compile/Sources/gtk-doc-1.33.0/tests'
make[1]: Entering directory '/Data/Compile/Sources/gtk-doc-1.33.0'
make[1]: Nothing to be done for 'all-am'.
make[1]: Leaving directory '/Data/Compile/Sources/gtk-doc-1.33.0'
git.mk: Generating .gitignore
Compile: Asserting that requirements are met...
SandboxInstall: unionfs is unavailable. Cannot proceed with the installation.
Compile: GTK-Doc 1.33.0 - Installation step failed.

```

## The solution 
```
InstallPackage https://gobolinux.org/packages/017/Fuse--2.9.7--x86_64.tar.bz2
InstallPackage https://gobolinux.org/packages/017/UnionFS-Fuse--2.1--x86_64.tar.bz2
Compile GTK-Doc 1.33.0
```


## Non-vital errors
```
checking for xsltproc... /usr/bin/xsltproc
checking for dblatex... no
checking for fop... no
configure: WARNING: neither dblatex nor fop found, so no pdf output from xml
checking for XML catalog... not found
checking for xmlcatalog... /usr/bin/xmlcatalog
checking for DocBook XML DTD V4.3 in XML catalog... not found
configure: error: could not find DocBook XML DTD V4.3 in XML catalog
```
These are probably dependencies that are not available in GoboLinux Remote Recipes Repository

## docbook catalog location
`/Data/Variable/lib/xml/catalog`

## DocBook
Post installation script for catalog generation  
`/Programs/DocBook-XML-DTD/4.5/Resources/PostInstall`

Postinstall takes content from this Data file.  
`/Programs/DocBook-XML-DTD/4.5/Resources/XmlCatalogData`

### Previous solution
This script removes every line that contains strings delegatePublic delegateSystem delegateURI.  
And Docbook-xml-dtd autogenerates them again on Compilation.  
```
#!/bin/bash
sed -i '/delegatePublic/c\' /Data/Variable/lib/xml/catalog  
sed -i '/delegateSystem/c\' /Data/Variable/lib/xml/catalog  
sed -i '/delegateURI/c\' /Data/Variable/lib/xml/catalog  
Compile Docbook-xml-dtd
```


Example of docbook catalog  
http://xmlsoft.org/catalog.html

