# Práctica 1: Seguridad de la información

## Objetivos de la práctica:
- Obtener manejo de comandos de la herramienta OpenSSL para la realización de operaciones criptográficas.
- Uso de algoritmos de resumen. 
- Uso de cifrado  simétrico y asimétrico.
- Generación y gestión de claves criptográficas.
- Utilización avanzada de sistemas de correo electrónico seguro.
- Gestión de certificados digitales y la configuración de un servidor web seguro. 

## Parte 1 - Utilización de OpenSSL

## 1.1 Generación y comprobación de Resúmenes 

### Una vez entendido el empleo del comando openssl-dgst y sus opciones básicas:
### -  Crear un archivo de texto legible de entre 150 y 200 caracteres

Para esto se ha creado un archivo de texto con el siguiente contenido: 

"La pesca al vivo es una técnica utilizada por muchos pescadores deportivos para capturar peces grandes y difíciles. Una de las especies más utilizadas como cebo vivo es la boga, un pez que se encuentra comúnmente en ríos y lagos de América del Sur.

La pesca al vivo con boga es muy efectiva porque los depredadores se sienten atraídos por el movimiento y el olor del cebo vivo. Para pescar con boga, es importante tener una red para atraparlas, ya que son difíciles de capturar con un anzuelo. Una vez que se ha capturado la cantidad necesaria de bogas, se pueden mantener vivas en un cubo o en una jaula sumergida en el agua hasta que se necesiten para la pesca.

Una vez que se tiene el cebo vivo, se debe seleccionar el anzuelo y la línea adecuados para la especie que se desea capturar. Los pescadores experimentados recomiendan utilizar anzuelos circulares, que minimizan el riesgo de que el pez se trague el anzuelo, lo que permite que se libere con mayor facilidad.

En conclusión, la pesca al vivo con boga es una técnica efectiva para capturar peces grandes y difíciles. Si se realiza de manera responsable y se liberan los peces que no se necesitan, puede ser una forma sostenible de pesca deportiva."


### - Aplicar un mínimo de CINCO algoritmos de resumen (SHA-1 y SHA-2 obligatorios) sobre ese archivo de texto y comprobar cómo varían los resúmenes obtenidos ante mínimas modificaciones del fichero.

Las funciones de resumen se utilizan en la autenticación de contraseñas y en el almacenamiento de contraseñas en sistemas informáticos. En lugar de almacenar las contraseñas en texto plano, se utiliza una función de resumen para crear un valor hash de la contraseña original y se almacena ese valor en su lugar. Cuando un usuario intenta iniciar sesión, la contraseña que ingresa se convierte en un valor hash y se compara con el valor hash almacenado en el sistema. Si los valores coinciden, se considera que la contraseña es auténtica y se permite el acceso.

El comando openssl dgst es una herramienta que se utiliza en la línea de comandos para calcular el valor hash de un archivo o mensaje utilizando diferentes algoritmos criptográficos de hash. Algunas de las opciones básicas que se pueden utilizar con este comando son:

- -md5, -sha1, -sha256, -sha512, entre otras: especifica el algoritmo de hash a utilizar. Por ejemplo, para calcular el valor hash utilizando el algoritmo SHA256, se puede utilizar la opción -sha256.

- -sign _archivo de clave privada_: especifica el archivo que contiene la clave privada que se utilizará para firmar el valor hash.

- -verify _archivo de clave pública_: especifica el archivo que contiene la clave pública que se utilizará para verificar la firma del valor hash.

- -hex: muestra el valor hash en formato hexadecimal.

- -binary: muestra el valor hash en formato binario.

- -out _archivo de salida_: especifica el archivo de salida donde se guardará el valor hash calculado.

- -salt: agrega un valor aleatorio (salt) al mensaje antes de calcular el valor hash. Esto ayuda a prevenir ataques de diccionario en la contraseña.

A continuación se calculan 5 funciones de has distintas del archivo de texto anteriormente creado como se ve  en la siguiente captura: 
![ejer1_1_a](./images/ejer1_1_a.PNG)

Ahora se modifica el texto ligeramente, se ha cambiado una U mayuscula a una u minúscula
![ejer1_1_b](./images/ejer1_1_b.PNG)

Y se vuelven a aplicar las funciones de resumen para comparar el resultado:
![ejer1_1_c](./images/ejer1_1_c.PNG)

Si se comparan las salidas obtenidas ahora con las anteriores se puede observar que no tienen ningun tipo de parecido, por ejemplo en md5 la primera salida fue "9e8214c21f0ee08a1532dfa574ea5eab" y tras modificar un caracter "e09c370d6b2aa4b54150b7c5bbfda02f".

## 1.2 Cifrado Simétrico de documentos
### Una vez entendido el empleo del comando openssl-enc para cifrar y descifrar, con diferentes algoritmos y modos de operación, el empleo de ficheros binarios y BASE64, la obtención de claves de contraseñas detallada en el estándar PKCS #5 (PBKDF1 y PBKDF2) y su aplicación a las claves de cifrado simétrico, vectores de inicialización y sal derivación de claves e “iv” a partir de contraseñas:

La opción "-enc" de OpenSSL se utiliza para cifrar y descifrar datos con el algoritmo de cifrado simétrico. 
Un algoritmo de cifrado simétrico es un tipo de cifrado que utiliza la misma clave para cifrar y descifrar los datos. Esto se hace diviendo el mensaje original en bloques y cifrándolo con la clave, posteriormente se realiza el mismo proceso para descifrarlo.

Los ficheros de texto son ficheros que contienen texto en formato legible para los seres humanos, los ficheros binarios son ficheros que contienen datos en formato binario no legible para los seres humanos y los ficheros Base64 son ficheros que contienen datos codificados en formato de Base64, para que puedan ser transmitidos por un canal que no puede transmitir datos binarios.

Base64 es un formato de codificación utilizado para representar datos binarios en formato ASCII. En la codificación Base64, cada tres bytes de datos binarios se convierten en cuatro caracteres ASCII. Estos caracteres están seleccionados de un conjunto de 64 caracteres predefinidos, que incluyen letras mayúsculas y minúsculas, números y algunos símbolos.

PKCS #5 es un proceso que se utiliza para derivar una clave criptográfica a partir de una contraseña utilizando algoritmos de derivación de claves como PBKDF1 y PBKDF2. Este proceso se utiliza comúnmente para generar claves de cifrado simétrico a partir de contraseñas de usuario en la protección de datos y la autenticación de usuarios en línea.

PBKDF1 utiliza una función hash iterativa (como MD5 o SHA-1) para derivar la clave a partir de la contraseña y una semilla de bits. La semilla de bits se genera aleatoriamente para cada clave derivada, lo que garantiza que cada clave sea única. El proceso se realiza mediante la concatenación de la contraseña y la semilla de bits, y se aplica una función hash iterativa un número fijo de veces para generar la clave derivada.

PBKDF2 utiliza una función de hash criptográfica (como SHA-1, SHA-256 o SHA-512) para derivar la clave a partir de la contraseña y una semilla de bits, al igual que PBKDF1. La principal diferencia entre PBKDF2 y PBKDF1 es que PBKDF2 permite especificar el número de iteraciones de la función de hash que se realizan, lo que lo hace más resistente a los ataques de fuerza bruta.

El proceso de derivación de claves en PBKDF2 se realiza mediante la concatenación de la contraseña y la semilla de bits, y la aplicación de la función de hash un número configurable de veces (por ejemplo, 1000 iteraciones). Este proceso se repite hasta que se ha generado la cantidad de bits de clave necesarios.

PBKDF2 también incluye un parámetro opcional llamado "sal" (salt), que es una cadena de bits aleatoria que se utiliza para evitar que se generen las mismas claves para diferentes contraseñas. La sal se concatena con la contraseña antes de aplicar la función de hash, lo que hace que sea imposible precalcular la clave para una contraseña específica.

Modos de cifrado:
- ECB: Este es el modo de cifrado más simple y consiste en dividir el mensaje en bloques y cifrar cada bloque por separado utilizando la misma clave. Este modo de cifrado no es recomendable para cifrar grandes cantidades de datos, ya que no ofrece mucha seguridad. Además, si se utilizan bloques idénticos de datos, el cifrado resultante será idéntico, lo que puede revelar información sobre los datos originales.
- CBC: cada bloque de datos se cifra utilizando la clave y el bloque cifrado se combina con el bloque siguiente antes de ser cifrado. Esto significa que cada bloque cifrado depende del bloque anterior, lo que ofrece una mayor seguridad.
- CFB: comienza con un vector de inicialización que se cifra utilizando la clave y se combina con el primer bloque de datos antes de ser cifrado. El resultado de esta operación se convierte en el primer bloque cifrado y se utiliza para cifrar el siguiente bloque de datos. Este proceso se repite hasta que se cifran todos los bloques de datos.
- OFB: un bloque de datos se cifra utilizando la clave y el resultado se utiliza para cifrar un vector de inicialización. Este vector se combina con el siguiente bloque de datos antes de ser cifrado. El proceso se repite hasta que se cifran todos los bloques de datos.
- CTR: se utiliza un contador como entrada para el cifrado. El contador se cifra utilizando la clave y se combina con los datos antes de ser cifrados. Este modo de cifrado es útil para cifrar grandes cantidades de datos de forma eficiente.

El vector de inicialización como la sal son valores aleatorios que se utilizan en el cifrado de datos para aumentar la seguridad y proteger contra ciertos tipos de ataques. El IV se utiliza para asegurar que el cifrado de los mismos datos produzca diferentes resultados cada vez que se ejecuta el algoritmo, mientras que la sal se utiliza para generar claves diferentes para cada mensaje y proteger contra los ataques de diccionario.

### - Crear un archivo de texto legible de pequeño tamaño – entre 150 y 200 caracteres.

Se utilizará el archivo de texto anteriormente creado

### -  Cifrarlo (con salida en modo binario) con CINCO algoritmos simétricos (AES y TDES obligatorios en modo bloque y flujo y un cifrador de flujo –RC4 o similar-).

La contraseña usada en los cifrados es "tarea12"

<br>

**AES en modo bloque:**
```console
openssl enc -aes-128-cbc -salt -in texto.txt -out texto.aes-128-cbc -p  -pbkdf2
```
Captura:
![ejer1_2_a1](./images/ejer1_2_a1.PNG)

**AES en modo flujo:**
```console
openssl enc -aes-128-ctr -salt -in texto.txt -out texto.aes-128-ctr -p  -pbkdf2
```
Captura:
![ejer1_2_a2](./images/ejer1_2_a2.PNG)

**TDES en modo bloque:**
```console
 openssl enc -des-ede3-cbc -salt -in texto.txt -out texto.tdes-cbc -p -pbkdf2
```
Captura:
![ejer1_2_a3](./images/ejer1_2_a3.PNG)

**TDES en modo flujo:** 
```console
openssl enc -des-ede3-ofb -salt -in texto.txt -out texto.tdes-ofb -p -pbkdf2
```
Captura:
![ejer1_2_a4](./images/ejer1_2_a4.PNG)

**Chacha20 en modo flujo:**
```console
 openssl enc -rc4-chacha20  -salt -p -pbkdf2 -in texto.txt -out texto.rc4 
```
Captura:
![ejer1_2_a5](./images/ejer1_2_a5.PNG)


<br>

### -  Descifrarlos y comprobar el resultado  
<br>

**AES en modo bloque:**
```console
openssl enc -d -aes-128-cbc -in texto.aes-128-cbc -out descifrado.aes-128-cbc -pbkdf2
```
Captura:

![ejer1_2_b1](./images/ejer1_2_b1.PNG)

**AES en modo flujo:**
```console
openssl enc -d -aes-128-ctr -in texto.aes-128-ctr -out descifrado.aes-128-ctr -pbkdf2
```
Captura:
![ejer1_2_b2](./images/ejer1_2_b2.PNG)

**TDES en modo bloque:**
```console
openssl enc -d -des-ede3-cbc -in texto.tdes-cbc -out descifrado.tdes-cbc -pbkdf2
```
Captura:
![ejer1_2_b3](./images/ejer1_2_b3.PNG)

**TDES en modo flujo:**
```console
openssl enc -d -des-ede3-ofb -in texto.tdes-ofb -out descifrado.tdes-ofb -pbkdf2
```
Captura:
![ejer1_2_b4](./images/ejer1_2_b4.PNG)

**Chacha20 en modo flujo:**
```console
openssl enc -d -chacha20 -in texto.rc4 -out descifrado.rc4 -pbkdf2
```
Captura:
![ejer1_2_b5](./images/ejer1_2_b5.PNG)

### -  Explicar el tamaño de los diferentes ficheros cifrados en virtud del tamaño de bloque del cifrador (o no, si se cifra en flujo), y sabiendo que el empleo de sal añade 16 bits de más al inicio del fichero cifrado –Salted__XXXXXXXX-.
El tamaño de los archivos cifrados puede variar en función del tamaño del bloque del cifrador utilizado en cada caso. Al emplear la opción "-salt", se añaden 16 bytes extra al inicio del archivo cifrado.

<br>

Tamaños obtenidos

<br>

```console
-rw-r--r-- 1 root root  1248 Feb 28 11:32 texto.aes-128-cbc
-rw-r--r-- 1 root root  1240 Feb 28 11:42 texto.aes-128-ctr
-rw-r--r-- 1 root root  1240 Feb 28 12:05 texto.rc4
-rw-r--r-- 1 root root  1248 Feb 28 11:50 texto.tdes-cbc
-rw-r--r-- 1 root root  1240 Feb 28 12:01 texto.tdes-ofb
-rw-r--r-- 1 root root  1224 Feb 25 20:05 texto.txt
```
<br>

- En el caso de texto.aes-128-cbc, el tamaño de bloque es de 128 bits (16 bytes), y al agregar la sal (16 bytes), se tiene un total de 32 bytes adicionales al tamaño del archivo original. Por lo tanto, el tamaño esperado del archivo cifrado sería de al menos 1240 bytes (el tamaño del archivo original más 32 bytes). Sin embargo, en la salida del comando ls -l, se muestra que el archivo cifrado tiene un tamaño de 1248 bytes, esto es porque OpenSSL agrega bytes de relleno al final del archivo original para que el tamaño del archivo cifrado sea múltiplo del tamaño del bloque. Como el tamaño del archivo original es 1224 bytes y el tamaño del bloque es de 16 bytes, se necesitan 8 bytes de relleno adicionales al final del archivo para completar el último bloque de cifrado. 

- El archivo cifrado texto.aes-128-ctr , el tamaño de bloque es de 128 bits (16 bytes), y al agregar la sal (16 bytes), se tiene un total de 32 bytes adicionales al tamaño del archivo original. Por lo tanto, el tamaño esperado del archivo cifrado sería de al menos 1240 bytes (el tamaño del archivo original más 32 bytes). En la salida del comando ls -l, se muestra que el archivo cifrado tiene un tamaño de 1240 bytes, lo cual es exactamente el tamaño esperado.

- El archivo cifrado texto.rc4 al ser un cifrador de flujo, no hay un tamaño de bloque fijo. Sin embargo, se agrega la sal (16 bytes) al inicio del archivo cifrado. En la salida del comando ls -l, se muestra que el archivo cifrado tiene un tamaño de 1240 bytes, lo cual sugiere que no hay una sobrecarga significativa por el uso de este cifrador de flujo.

- El archivo cifrado texto.tdes-cbc  el tamaño de bloque es de 64 bits (8 bytes), y al agregar la sal (16 bytes), se tiene un total de 24 bytes adicionales al tamaño del archivo original. Por lo tanto, el tamaño esperado del archivo cifrado sería de al menos 1248 bytes (el tamaño del archivo original más 24 bytes). En la salida del comando ls -l, se muestra que el archivo cifrado tiene un tamaño de 1248 bytes, lo cual es exactamente el tamaño esperado.

- El archivo cifrado texto.tdes-ofb, el tamaño de bloque es de 64 bits (8 bytes), y al agregar la sal (16 bytes), se tiene un total de 24 bytes adicionales al tamaño del archivo original. Por lo tanto, el tamaño esperado del archivo cifrado sería de al menos 1240 bytes (el tamaño del archivo original más 24 bytes). En la salida del comando ls -l, se muestra que el archivo cifrado tiene un tamaño de 1240 bytes, lo cual es exactamente el tamaño esperado.



### -  cifrar un fichero con contraseña y descifrarlo NO con dicha contraseña, sino con su conjunto equivalente de clave (key), vector de inicialización (iv) y sal (salt).

<br>

Se usará el archivo de texto que se ha utilizado durante el resto de la práctica.

La contraseña usada para cifrar es "tarea12"

En primer lugar se cifra usando la opción p para imprimir los datos que necesitamos:

```console
openssl enc -aes-128-cbc -salt -in texto.txt -out tarea1_2c.aes-128-cbc -p  -pbkdf2
```
Captura:
![ejer1_2_c1](./images/ejer1_2_c1.PNG)

Guardamos los datos impresos por pantalla: 
```console
salt=4DB5930EEBE1D9E9
key=27DE8AC58971C8732C2BA197854139AD
iv =34FC741EBC68B913143C2055EBB3B57C
```
Ahora se debe extraer la SALT para posteriormente descifrarlo:
```console
cat tarea1_2c.aes-128-cbc | dd ibs=16 obs=16 skip=1 > tarea1_2c.nosalt
```

Para descifrarlo se usa la opción -K(clave), -iv(vector de inicializacion), -S(salt):

```console
openssl enc -aes-128-cbc -d -in tarea1_2c.nosalt -K 27DE8AC58971C8732C2BA197854139AD -iv 34FC741EBC68B913143C2055EBB3B57C -S 4DB5930EEBE1D9E9 -out tarea1_2c.descifrado
```
Captura:
![ejer1_2_c2](./images/ejer1_2_c2.PNG)

## 1.3 Generación de claves asimétricas (pública-privada) y firmado de resúmenes

### • Generar un par de claves asimétricas RSA de 2048 bits.
La contraseña que se usará será "tarea13"

Primero se gennera la clave privada:
```console
 openssl genpkey -algorithm RSA -aes256 -out RSA.pem -pkeyopt rsa_keygen_bits:2048
```
Captura:
![ejer1_3_a1](./images/ejer1_3_a1.PNG)

A continuación se generará la clave pública a partir de la privada:
```console
 openssl pkey -in RSA.pem -pubout -out PRSA.pem
```
Captura:
![ejer1_3_a2](./images/ejer1_3_a2.PNG)

Clave Privada Generada:
```console
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIFLTBXBgkqhkiG9w0BBQ0wSjApBgkqhkiG9w0BBQwwHAQIvzBVAuSdE3cCAggA
MAwGCCqGSIb3DQIJBQAwHQYJYIZIAWUDBAEqBBCstHQyFQjKTFCPfZFaILPfBIIE
0IbpTV0Zu3+gK4Da2Ih8p5YX9Mirx/oLgcYBNF4jRw96WVMypvA1q7WyuBt3Yd1z
c47RsAbF4sw0TnXcLI+jM1IZAsCQ8FRcZadSaBbh+bc/tYlwGLOuWJsYX1KywDio
0zJcfp/vA55ueaGS/uvLMatoQu1cwiVhqpsCk7+HsQcmqg7srdnGUCb97RVEkL/2
VUvStjdbSgeVQwLKn03S/hnh0EaplmcO9ysfWHA/dtO8C9+n4nu3NwDspsQtBuY0
kuIj8HfqYi5aNbTIVlcoqsAgaPIGK6tFXVtQJh4HbGA74x4V7JX+ULJQTYPPON17
fkQFxyqWamS+ViKlYqOVM+hq4nFxSDecpz53pj/Fi3JPwfv0HYZXmmixqDihkihe
tSZvPV+3wWvTsRwwn38s9pfUzbd4Pcsz+exQSUD0m8RiVO5jxHeWCcxmXfm3x1Fa
+X/aPzKixAfu7j9U/wQXn7TYxK3PuoKdKzD2Xa3oDgINjhOzuy0XV/nHSAgpzpBl
q0h0oGA1OwV1hCfs94Ukhun5VlV/vSYH2XSteVGC9XaEpNgChHoMM9lvivdR03Cy
5ebFAInniHojpWTbCSRyjpZwBN4ZZap/OVNnY5fRt2tkQI0xjK5SpIxtnLDu+4bc
s9aSCGCpPmDHny1P26qXcS5SD2LqFPlpzitgiyM2PZvxMUeQNgmGT9UiG2gf+3Jp
Uxs1esitqKYdwMsmDNu3INlPiIOb26voP2DwCukykk08r/7MgG6uXMlCoHA6xlkQ
Dpp4ygwcC502VfKySocNarnHm4UQ1IGji+KKKRBnZDoqEWOdbSzhfhgakaw9OPWO
PL6SUgfkF2u7K2h/sUYs6Pqg+03EokOgHYfsjXCI/PDRrylONgN4R6o+UrkEsnS0
uWFo9s693GdWNe0rjH0MeqrYjKXAxjivhFP5kFWWVMQ0h7MczkyhtG/EBVqk+lgs
tQD9ywHApaGxn2DlKNHw9ehvifwgEhoNHgc48zG9e/98wYsXkm57zgfHBfiIYpDV
lGfZCAfyYkUE+/mWkhAX/wd41/FZ/cgGwheFR00Veg+yAF/7XQ4qIWWBSiQ3UU4y
N2bJ5c7g1bmnm+K5Ls1+RJelE2S5ajU2wa2UMjk8SlIPgPyUSrVDKnrDS8o5HcLk
p1LnicuuiGO6LI6EcDMh0PBsvOkvH761bG2A5P0m0XOW3s/DS4T4cNMYnnMzeDDq
2Ef2csgJ4TZpzYoC5OIpymQgdxK5E7wq+3PxvFMgLimssfQdgtNzTv7gnxO/EHUk
FoRHxwB30fSN7KfIDNuhDU8eHliqjq+7N49ErKpAydhQkY+56jADLeb/Q3FyJ1yA
wc6kstyQjxZRCzf92JVLqqpyVfjARTWi7MrVLqBUQBIzt9N1eejtR0WPy2cMetBd
CdPolNeFRfPHxGziu9rTe51ayEhs56pu6C39GMtvCYqPv9sWsgJhu6ZAYH/PWUfx
tiNbvhK445fX7U8BoeUaF9fsSt1ZcUaX8tnuAZ+G8kjtfdnzuII6dXyvGhtDX8NT
UHuyoDGV5I/qhomxoZWSmuF7dvznyBHQgV+Vi8r8cgLHSBkkovViTyHDZNnSK8Q8
mLp3vGQ73pc+ihcLTrC4EQna+1bCtaHhLktiWAODjyB6
-----END ENCRYPTED PRIVATE KEY-----
```
Clave Pública Generada:
```console
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA3HX5heHiuMfBOSH2d+yF
sAZZ11qJ+13oEbhno06nUILeB8WXrWSNSWMb0m2sR1tF+P67kk/odK7EcMuF39Sa
2kqX8FbaoMvJpviRLHCVwpG6cwLG/cog51wBNh7I30oB26wyOoe2ayDuB9pIrKzi
Surdnv4XUHUxXGbpiv17eD4AgDXLpyUMKACCxYfasPt6qA4tjgpJXDRWP7L8U6/1
PAh8bN9BQhJAm0OP5UbIQTA0obN9NwbRoF5aMepn7jHPpqMw+Sxllr+oxUa0v9t9
ziK1hhN5cgtROv2+0Q+wqXgrIzq8rc3oODi3N0gRbwyQ9A3GGeV3RCSCIBao+xlM
OwIDAQAB
-----END PUBLIC KEY-----
```
### • Exportar dicho par de claves (pública y privada) en formato PEM (textual) y DER (binario).Utilizar los comandos de conversión de PEM a DER y viceversa.
Conversión de la clave privada a DER:
```console
 openssl rsa -inform PEM -in RSA.pem -outform DER -out RSA.der
``` 
Captura:
![ejer1_3_b1](./images/ejer1_3_b1.PNG)

Conversión de la clave pública a DER:
```console
openssl rsa -inform PEM -pubin -in PRSA.pem -pubout -outform DER -out PRSA.der
``` 
Captura:
![ejer1_3_b2](./images/ejer1_3_b2.PNG)

Ahora se realizará la conversión inversamente y se comprobará:

Conversión de la clave privada a PEM:
```console
 openssl rsa -inform DER -in RSA.der -outform PEM -out RSA2.pem
``` 
Captura:
![ejer1_3_b3](./images/ejer1_3_b3.PNG)

Conversión de la clave pública a PEM:
```console
openssl rsa -inform DER -pubin -in PRSA.der -pubout -outform PEM -out PRSA2.pem
``` 
Captura:
![ejer1_3_b4](./images/ejer1_3_b4.PNG)

Se puede comprobar  que la clave pública obtenida es igual a la generada anteriormente:
```console
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA3HX5heHiuMfBOSH2d+yF
sAZZ11qJ+13oEbhno06nUILeB8WXrWSNSWMb0m2sR1tF+P67kk/odK7EcMuF39Sa
2kqX8FbaoMvJpviRLHCVwpG6cwLG/cog51wBNh7I30oB26wyOoe2ayDuB9pIrKzi
Surdnv4XUHUxXGbpiv17eD4AgDXLpyUMKACCxYfasPt6qA4tjgpJXDRWP7L8U6/1
PAh8bN9BQhJAm0OP5UbIQTA0obN9NwbRoF5aMepn7jHPpqMw+Sxllr+oxUa0v9t9
ziK1hhN5cgtROv2+0Q+wqXgrIzq8rc3oODi3N0gRbwyQ9A3GGeV3RCSCIBao+xlM
OwIDAQAB
-----END PUBLIC KEY-----
```

### • Con los dos pares de claves asimétricas creadas, firmar y comprobar la firma del resumen (con SHA-256) de un fichero de texto del apartado anterior.

Se crea el resumen del texto y se firma:
```console
openssl dgst -sha256 -sign RSA.pem -out firmado.rsa texto.txt
```
Captura:
![ejer1_3_c1](./images/ejer1_3_c1.PNG)

Se comprueba:
```console
openssl dgst -sha256 -verify PRSA.pem -signature firmado.rsa texto.txt
```
Captura:
![ejer1_3_c2](./images/ejer1_3_c2.PNG)

### • Generar dos claves DH con curva elíptica X25519 y demostrar que la combinación pública1- privada2 genera el mismo secreto que la combinación privada1-pública2.

Se generan las claves privadas:
```console
openssl genpkey -algorithm X25519 -out privada1.pem
openssl genpkey -algorithm X25519 -out privada2.pem
```
Se extraen las claves públicas:
```console
openssl pkey -in privada1.pem -pubout -out publica1.pem
openssl pkey -in privada2.pem -pubout -out publica2.pem
```
Se deriva un valor secreto usando la clave privada1 y la clave publica2:
```console
openssl pkeyutl -derive -inkey privada1.pem -peerkey publica2.pem -out valor1
```
Se deriva otro valor secreto usando la clave privada2 y la clave publica1:
```console
openssl pkeyutl -derive -inkey privada2.pem -peerkey publica1.pem -out valor2
```

Se comparan los secretos:
```console
cmp valor1 valor2
```
Captura:
![ejer1_3_d1](./images/ejer1_3_d1.PNG)


## 1.4 Cifrado Asimétrico de documentos

### En este apartado vamos a reproducir en una secuencia de operaciones el intercambio de información segura entre dos agentes utilizando cifrado simétrico, cifrado asimétrico de las claves simétricas yfirma digital (cifrado asimétrico de un resumen del documento original). Para ello, generaremos dos parejas de claves RSA que serán utilizadas por dos agentes (Ana y Berto) de forma que Ana construirá tres ficheros de texto a partir del fichero de texto original, el primero con el fichero cifrado, el segundo con las claves utilizadas y el tercero con la firma digital. Así, primero generaremos las dos claves:

<br>

### • Generar una pareja de claves RSA en ficheros anapub.pem y anapriv.pem y otra pareja de claves del mismo tipo bertopub.pem y bertopriv.pem. 
### • Proteger ambas claves privadas con contraseña “anak” y “bertok” respectivamente.

Se generan primero las claves privadas utilizando las contraseñas propuestas:
```console
openssl genpkey -algorithm RSA -aes256 -out anapriv.pem -pkeyopt rsa_keygen_bits:2048
openssl genpkey -algorithm RSA -aes256 -out bertopriv.pem -pkeyopt rsa_keygen_bits:2048
```
A continuación se generan las claves públicas a partir de las privadas:
```console
openssl pkey -in anapriv.pem -pubout -out anapub.pem
openssl pkey -in bertopriv.pem -pubout -out bertopub.pem
```


<br>

### Se supone que Ana y Berto han intercambiado sus claves públicas. A continuación, realizaremos el trabajo de Ana:

### • Cifrar un fichero de texto mensaje.txt de los apartados anteriores con AES-256 en modo CBC, con clave y vector escogidos por el estudiante y sin sal, con salida en formato BASE64 a un fichero llamado cifrado.txt.
```console
#Se copia el texto de los apartados anteriores
cp texto.txt mensaje.txt 

#Se generan la clave y el vector aleatoriamente
openssl rand -hex 32 > clave
openssl rand -hex 16 > iv

#Se cifra el fichero 
openssl enc -aes-256-cbc -pbkdf2 -in mensaje.txt -out cifrado.txt -nosalt -base64 -K $(cat clave) -iv $(cat iv)
```
Captura:
![ejer1_4_1](./images/ejer1_4_1.PNG)
### • Crear un fichero binario con la concatenación de la clave y el vector utilizados, de nombre claves.txt y cifrarlo con la clave pública bertopub.pem, con salida en formato binario a un fichero claves.bin
```console
# Se usa el pipe con el comando xxd para que la salida sea binaria
echo $(cat clave)$(cat iv) | xxd -r -p > claves.txt
openssl pkeyutl -encrypt -pubin -inkey bertopub.pem -in claves.txt -out claves.bin
```
Captura:
![ejer1_4_2](./images/ejer1_4_2.PNG)
### •Convertir claves.bin a claves.txt en formato BASE64 
```console
openssl enc -a -in claves.bin -out claves64.txt
```
Captura:
![ejer1_4_3](./images/ejer1_4_3.PNG)
### • Obtener un resumen sha256 del fichero de texto original y firmarlo (es decir, cifrarlo con la clave privada anapriv.pem clave anak) , con salida en formato BASE64 a un fichero llamado firma.txt.
```console
openssl dgst -sha256 -binary  mensaje.txt >  resumen.txt
openssl pkeyutl -sign -in resumen.txt -inkey anapriv.pem -out firma.bin
openssl enc -base64 -in firma.bin -out firma.txt
```
Captura:
![ejer1_4_4](./images/ejer1_4_4.PNG)

<br>

### En este momento, Ana enviaría los tres ficheros obtenidos cifrado.txt, claves.txt y firma.txt (junto a la meta-información relativa a los algoritmos utilizados, cómo se concatenan clave y vector…) a Berto… que seremos nosotros mismos. Actuando como Berto, procederemos a:

### • Convertir el fichero claves.txt a fichero binario claves2.bin
```console
openssl base64 -d -in claves64.txt -out claves2.bin
```
### • Descifrar el fichero claves2.bin con la clave privada bertopriv.pem (contraseña bertok) y salida a un fichero claves2.txt (debería ser idéntico a claves.txt).
```console
openssl pkeyutl -decrypt -inkey bertopriv.pem -in claves2.bin -out claves2.txt 

#Se compara usando el comando cmp
cmp claves.txt claves2.txt
```
Captura:
![ejer1_4_5](./images/ejer1_4_5.PNG)

### • Extraer de claves2.txt la clave y el vector en formato hexadecimal, siguiendo la metainformación que le envió Ana junto con los tres ficheros.
```  console
head -c 32 claves2.txt | xxd -p -c 32 | tr -d '\n' > clave2
tail -c 16 claves2.txt | xxd -p -c 16 | tr -d '\n' > iv2
```
Captura:
![ejer1_4_6](./images/ejer1_4_6.PNG)

### • Descifrar el fichero cifrado.txt con la clave y el vector obtenidos y salida al fichero mensaje2.txt (que debería ser igual al fichero original mensaje.txt utilizado por Ana).
``` console 
openssl enc -aes-256-cbc -d -in cifrado.txt -out mensajedescifrado.txt -base64 -nosalt -K 23de99f465f3b3dbb53eb1625a92e5e25bd0ae63bc1e2e79bdb36428d929409c -iv 79928d8b9abb2ca82bbc5c78644f37bc -pbkdf2 
```
Captura:
![ejer1_4_7](./images/ejer1_4_7.PNG)

### • Verificar que el fichero firma.txt con la clave pública de Ana, anapub.pem coincide con un resumen sha256 del fichero mensaje2.txt.
``` Console 
openssl dgst -sha256 -binary -out resumen2.txt mensajedescifrado.txt
openssl pkeyutl -verify -in resumen2.txt -sigfile firma.bin -pubin -inkey anapub.pem
```
Captura:
![ejer1_4_8](./images/ejer1_4_8.PNG)
<br>

## 2.1 Envío, recepción y decodificación manual de mensajes S/MIME firmados y cifrados, empleando certificados creados por el estudiante y firmados con el certificado raíz de prácticas de esta asignatura.

### a) Descargar certificadoRaiz.crt y certificadoRaiz.key de la página de la asignatura

### b) Crear la clave de nuestro certificado (openssl genpkey ........) -> certificadoPersonal.key (hay que crear una contraseña que protege esta clave – ANOTARLA)
La contraseña usada sera tarea21
``` console
openssl genpkey -algorithm RSA -aes256 -out certificadoPersonal.key -pkeyopt rsa_keygen_bits:2048
```
Captura:
![ejer2_1_b](./images/ejer2_1_b.PNG)
### c) Crear el CSR de nuestro certificado (openssl req –new ........) -> certificadoPersonal.csr
``` console
openssl req -new -key certificadoPersonal.key -out certificadoPersonal.csr
```
Captura:
![ejer2_1_c](./images/ejer2_1_c.PNG)
### d) Crear nuestro certificado personal a partir del CSR, el certificado raíz y su clave, para ello habrá que introducir la contraseña SEGURIDAD que protege la clave de la raíz, el comando será del tipo (openssl x509 –req –days 365 ........) -> certificadoPersonal.crt
```console
openssl x509 -req -days 360 -in certificadoPersonal.csr -CA certificadoRaiz.crt -CAkey
certificadoRaiz.key -CAcreateserial -out certificadoPersonal.crt

# Para verificarlo:
openssl verify -CAfile certificadoRaiz.crt certificadoPersonal.crt
```
Captura:
![ejer2_1_d](./images/ejer2_1_d.PNG)
Captura:
![ejer2_1_d2](./images/ejer2_1_d2.PNG)
### e) El último apartado de la Wiki, 6.6: PEM a PKCS#12 explica cómo crear un fichero “inter.p12” a partir de certificadoIntermedio.crt y certificadoIntermedio.key, tendremos que hacer un comando similar, cambiando “Intermedio” por “Personal” para crear un fichero llamado certificadoPersonal.p12. Atención: el comando nos pedirá la clave que protege certificadoPersonal y a continuación otra clave (podría ser la misma) para proteger el fichero de salida, certificadoPersonal.p12 El comando será del tipo (openssl pkcs12 export ........) -> certificadoPersonal.p12
La clave usada será tarea21

```console
openssl pkcs12 -export -in certificadoPersonal.crt -inkey certificadoPersonal.key -out certificadoPersonal.p12
```
Captura:
![ejer2_1_e](./images/ejer2_1_e.PNG)

## 2.2 Instalación de los certificados en el cliente de correo e intercambio de certificados y correo firmado y cifrado con otro compañero  

### 2.2.1. Una vez creado nuestro certificado digital, hemos de configurar el cliente de correo electrónico Mozilla Thunderbird para que pueda firmar y cifrar mensajes con dicho certificado. Los correos a través de web (como el de la ULPGC) no permiten el uso de certificados, así que si no lo tenemos instalado, habrá que configurar previamente Mozilla Thunderbird con nuestro correo electrónico personal (no el de la ULPGC).

### 2.2.2. En este cliente de correo electrónico hemos de instalar tanto la parte pública del certificado raíz (fichero certificadoRaiz.crt), como nuestro certificado personal completo (fichero certificadoPersonal.p12) con la contraseña que le pusimos. En Thunderbird, hay que acceder a Herramientas/Opciones/Avanzado/Administrar certificados). Al Importar el certificado raíz, asignarle confianza para gestionar correo electrónico y verificar que se encuentra en el almacén de “Autoridades” (debe aparecer en primer lugar, ya que el nombre de la organización es “AA – Autoridad de prácticas FS/ASSI”. A continuación, hemos de importar nuestro certificadoPersonal.p12 e introducir la contraseña que lo protege. Verificar que nuestro certificado personal aparece en el almacén de “Sus certificados” o similar.

Captura:
![ejer2_2_2](./images/ejer2_2_2.PNG)
![ejer2_2_2](./images/ejer2_2_2b.PNG)

### 2.2.3. Con ambos certificados ya instalados, hay que configurar nuestra cuenta de correo electrónico para que utilice nuestro certificado personal, en Thunderbird eso se hace en Herramientas/Configuración de cuenta/Seguridad… al pulsar el botón de Seleccionar en las opciones de Firmado Digital y Cifrado debe aparecer seleccionado nuestro certificado personal que cargamos previamente. Importante: no seleccionar las opciones de firmar y cifrar siempre, es mejor decidir cuándo se envían mensajes firmados y cifrados antes de enviar cada mensaje.
Captura:
![ejer2_2_3](./images/ejer2_2_3.PNG)

### 2.2.4. Una vez configurado e instalado el certificado, podremos FIRMAR los mensajes que enviemos, pero para cifrar correo hacia un destinatario, es necesario disponer previamente de SU certificado personal (obviamente, sólo con la clave pública). Por tanto aconsejamos intercambiar mensajes sólo firmados con otro compañero y así nuestro programa de correo almacenará el certificado del compañero que acompaña a su mensaje cifrado. Una vez que verifiquemos que el certificado del compañero está en nuestro almacén de “Otras personas” ya podremos intercambiar mensajes cifrados y firmados.
![ejer2_2_4](./images/ejer2_2_4.PNG)


### 2.2.5. Enviar un mensaje con Asunto: (nombre y apellidos) a dicha dirección de correo (la del compañero) con un breve texto de salutación y sobre todo, con las opciones de firmado y cifrado activadas. 
![ejer2_2_43](./images/ejer2_2_43.PNG)

## 2.3 Decodificación con la utilidad “openssl smime” del mensaje cifrado y firmado que recibimos del compañero


### 2.3.1. Una vez el mensaje cifrado y firmado del compañero, se trata de decodificar ese mensaje desde la utilidad “openssl smime”. Para ello, hemos de guardar desde Thunderbird el mensaje completo como texto (seleccionar mensaje, botón de la derecha, Guardar cómo…), exportar nuestro certificado completo (o utilizar nuestro fichero certificadoPersonal.p12) y el certificado del compañero (Exportar en formato .crt) y procesar estos tres ficheros (mensaje de texto y los dos certificados) a través de la citada utilidad “openssl smime”.
Se guarda el certificado del compañero y el mensaje en formato eml:
![ejer2_3_1](./images/ejer2_3_1.PNG)
![ejer2_3_2](./images/ejer2_3_2.PNG)
### 2.3.2. El apartado 5.- S/MIME de la Wiki explica con detalle cómo procesar correos S/MIME desde línea de comandos openssl, lo que pedimos ahora está explicado en el apartado 5.2.4. – Descifrar mensaje cifrado y firmado de la citada Wiki. Obsérvese que el descifrado y la verificación de firma han de hacerse por separado. Documentar este proceso en detalle. 
Primero se debe convertir la clave personal del receptor a pem utilizando el siguiente comando:
``` console
openssl pkcs12 -in certificadoPersonal.p12 -legacy -out certificadoPersonal.pem 
``` 
A continuación se utiliza la funcionalidad de smime para descifrar el mensaje de correo:
``` console
openssl smime -decrypt -in Carlos\ Suárez\ Domínguez\ -\ \'Carlos\ Suárez\ Domínguez\'\ \(littleguitar.2001\@gmail.com\)\ -\ 2023-03-23\ 1429.eml  -recip certificadoPersonal.pem -out mensajedescifrado.txt
```

![ejer2_3_3](./images/ejer2_3_3.PNG)

Por ultimo se verifica la firma con el siguiente comando:
``` console
openssl smime -verify -in mensajedescifrado.txt -CAfile certificadoRaiz.crt
``` 
![ejer2_3_3](./images/ejer2_3_3b.PNG)


## 3 Configuración de un SERVIDOR WEB SEGURO (SHTTP) utilizando un certificado AUTOFIRMADO, con privilegios de Administrador en un servidor web – Apache, ISS, etc.
### Esta tercera parte de la práctica 1 consiste en la creación de un certificado AUTOFIRMADO de servidor Web, utilizando las herramientas de la utilidad openssl, tal y cómo se indica en el apartado de la Wiki 6.1 – Certificado Raíz Autofirmado; y en la instalación de dicho certificado en un servidor apache seguro, que responda a la dirección web https://www.ejemplo.com. Para ello, el campo “Sujeto” o “Common Name” del certificado ha de ser la cadena “www.ejemplo.com”.
### Una vez configurado el servidor seguro, se preparará una página HTML y se colocará como “index.html” en el directorio raíz del servidor, de forma que esta página será accedida de forma segura, por el puerto 443, desde un navegador web. Todo el proceso ha de ser documentado con profusión de ejemplos de comandos en línea, de ficheros de configuración (resumidos) y de volcados de pantalla.
### A diferencia de las anteriores, esta parte de la práctica requiere privilegios de administrador en la máquina en la que se pretenda configurar el servidor seguro. Podrá realizarse en una máquina Windows, Linux o Mac, virtual ó real, pero el servidor ha de ser un APACHE. El nombre del servidor seguro será “www.ejemplo.com”. Dado que no disponemos de control, desde el punto de vista del “Domain Name Service” o DNS, sobre el dominio de Internet “ejemplo.com”, tendremos que asegurarnos de que tanto el servidor como el programa cliente que utilicemos para realizar una conexión de prueba (que podrá ser en la misma máquina o en otra conectada en red) tienen definido el nombre www.ejemplo.com en su fichero de HOSTS. En una máquina Linux, este fichero se encuentra en /etc/hosts, mientras que en Windows su localización es “\windows\system32\drivers\etc\hosts” o similar.
### El objetivo de esta parte es conseguir que el estudiante resuelva un problema real, haciendo uso de los recursos de Internet. Por tanto, no se detallan las instrucciones. Precisamente lo que se pretende es que el estudiante redacte una breve pero clara y concisa descripción de los pasos a ser realizados y la incorpore en la memoria de la práctica.

Primero se debe instalar el Apache y mod_ssl:
``` console
sudo apt update
sudo apt install apache2
sudo ufw app list
sudo ufw allow 'Apache'
sudo a2enmod ssl

 ```
A continuacion se crea el directorio /etc/ssl/private, se accede y creamos el certificado
``` console
mkdir /etc/ssl/private
cd /etc/ssl/private
openssl req -new > new.ssl.csr
# La contraseña usada es tarea3
```
Captura:
![ejer3_1](./images/ejer3_1.PNG)

Se crea la clave privada
```console
 openssl rsa -in privkey.pem -out web.key
 openssl x509 -in web.csr -out web.cert -req -signkey web.key -days 365
```
Captura:
![ejer3_2](./images/ejer3_2.PNG)

A continuación se accede a la configuración de apache y se modifica para añadir los certificados con sus rutas:
``` console
vim /etc/apache2/sites-available/default-ssl.conf
 a2enmod ssl
 a2ensite default-ssl.conf
 service apache2 reload
```
Captura: 
![ejer3_6](./images/ejer3_6.PNG)

Ahora se personalizará el html del servidor:
``` console
cd /var/www/html
vim index.html
```
Captura:
![ejer3_4](./images/ejer3_4.PNG)
Captura:
![ejer3_5](./images/ejer3_5.PNG)
![ejer3_7](./images/ejer3_7.PNG)