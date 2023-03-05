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

**RC4 en modo flujo:**
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

**RC4 en modo flujo:**
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

### Documentar el trabajo realizado, con ejemplos de los resultados obtenidos (valores binarios en hexadecimal, Base64 o en formato PEM) y profusión de cortes de volcados de pantalla. Abstenerse de mostrar en pantalla el volcado de binarios puros.





