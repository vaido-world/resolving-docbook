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
These are probably dependencies that are not available in GoboLinux Remote Recipes Repository.  
Also GTK-Doc seems to be more inclined to be used with `DocBook XML DTD V4.3` instead of what is preinstalled on GoboLinux `DocBook XML DTD V4.5`  
XML catalog actually exists, but maybe it is badly configured by GoboLinux maintainers

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


# After PostInstall Script 
```
root@LiveCD ~]cat /Data/Variable/lib/xml/catalog
<?xml version="1.0"?>
<!DOCTYPE catalog PUBLIC "-//OASIS//DTD Entity Resolution XML Catalog V1.0//EN" "http://www.oasis-open.org/committees/entity/release/1.0/catalog.dtd">
<catalog xmlns="urn:oasis:names:tc:entity:xmlns:xml:catalog">
  <rewriteSystem systemIdStartString="http://docbook.sourceforge.net/release/xsl/current/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteURI uriStartString="http://docbook.sourceforge.net/release/xsl/current/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteSystem systemIdStartString="http://docbook.sourceforge.net/release/xsl/1.79.1/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteURI uriStartString="http://docbook.sourceforge.net/release/xsl/1.79.1/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <delegatePublic publicIdStartString="-//OASIS//ENTITIES DocBook XML" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegatePublic publicIdStartString="-//OASIS//DTD DocBook XML" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.1.2" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.2" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.3" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.4" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.1.2" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.2" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.3" catalog="file:///Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.4" catalog="file:///Data/Variable/lib/xml/docbook"/>
</catalog>


```

# Before PostInstall Script 
It seems that `file:///GoboLinux/ISO/Output/ROLayer/` is incorrectly produced path by GoboLive 017 CD 

```
<?xml version="1.0"?>
<!DOCTYPE catalog PUBLIC "-//OASIS//DTD Entity Resolution XML Catalog V1.0//EN" "http://www.oasis-open.org/committees/entity/release/1.0/catalog.dtd">
<catalog xmlns="urn:oasis:names:tc:entity:xmlns:xml:catalog">
  <delegatePublic publicIdStartString="-//OASIS//ENTITIES DocBook XML" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegatePublic publicIdStartString="-//OASIS//DTD DocBook XML" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.1.2" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.2" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.3" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateSystem systemIdStartString="http://www.oasis-open.org/docbook/xml/4.4" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.1.2" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.2" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.3" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://www.oasis-open.org/docbook/xml/4.4" catalog="file:///GoboLinux/ISO/Output/ROLayer/Data/Variable/lib/xml/docbook"/>
  <delegateURI uriStartString="http://docbook.sourceforge.net/release/xsl/" catalog="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteSystem systemIdStartString="http://docbook.sourceforge.net/release/xsl/current/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteURI uriStartString="http://docbook.sourceforge.net/release/xsl/current/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteSystem systemIdStartString="http://docbook.sourceforge.net/release/xsl/1.79.1/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
  <rewriteURI uriStartString="http://docbook.sourceforge.net/release/xsl/1.79.1/" rewritePrefix="file:///Programs/DocBook-XSL-Stylesheets/1.79.1/share/xml/docbook/stylesheet/docbook-xsl/"/>
</catalog>

```
