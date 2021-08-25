# resolving-docbook

## docbook catalog location
`/Data/Variable/lib/xml/catalog`

## DocBook
Post installation script for catalog  
`/Programs/DocBook-XML-DTD/4.5/Resources/PostInstall`


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

