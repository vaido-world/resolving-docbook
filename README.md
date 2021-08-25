# resolving-docbook

## docbook catalog location
`/Data/Variable/lib/xml/catalog`



### Previous solution
```
sed -i '/delegatePublic/c\' /Data/Variable/lib/xml/catalog  
sed -i '/delegateSystem/c\' /Data/Variable/lib/xml/catalog  
sed -i '/delegateURI/c\' /Data/Variable/lib/xml/catalog  
Compile Docbook-xml-dtd
```


Example of docbook catalog  
http://xmlsoft.org/catalog.html

