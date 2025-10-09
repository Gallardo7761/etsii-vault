# <mark style="background: #FFF3A3A6;">TEMA 1: Introducción a la Seguridad</mark>
## <mark style="background: #ADCCFFA6;">1. Conceptos Fundamentales</mark>
### <mark style="background: #FFB86CA6;">Seguridad Informática e Información</mark>
Seguridad informática protege infraestructuras tecnológicas y de comunicación, mientras que la seguridad de la información abarca cualquier soporte y las personas que la gestionan. El activo más importante es la información, que debe ser clasificada y protegida.
### <mark style="background: #FFB86CA6;">Gestión y Estrategias</mark>
Defensa en profundidad emplea capas de salvaguardas. La seguridad por oscuridad usa el secreto en los detalles. Security by design asume que el diseño puede ser conocido y la clave es el único secreto. Moving-target defense incrementa la dificultad introduciendo cambios continuos en la configuración.
### <mark style="background: #FFB86CA6;">Consecuencias de Mala Gestión</mark>
La falta de gobierno en seguridad implica riesgos legales, pérdida de datos, perjuicio reputacional, bloqueo de servicios y delitos como fraude, sabotaje, chantaje o suplantación de identidad.
## <mark style="background: #ADCCFFA6;">2. Requisitos de la Seguridad</mark>
- Integridad: evitar cambios no autorizados o accidentales.
- Confidencialidad: solo los autorizados acceden a información.
- Disponibilidad: acceso garantizado a usuarios legítimos.
- Identificación, autenticación y autorización como pilares del control de acceso.
- No repudio y auditoría para asegurar responsabilidades.
# <mark style="background: #FFF3A3A6;">TEMA 2: Integridad de la Información</mark>
## <mark style="background: #ADCCFFA6;">1. Definición y Objetivos</mark>
Prevenir, detectar y responder a ataques que comprometan la información. Implementar medidas para monitorización y respuesta en caso de incidentes.
## <mark style="background: #ADCCFFA6;">2. Hash y Funciones Resumen</mark>
Las funciones hash crean resúmenes únicos para detectar alteraciones. Los algoritmos clásicos incluyen MD5, SHA-1 y los más robustos: SHA-224, SHA-256, SHA-384, SHA-512 y SHA-3. MD5 y SHA-1 son vulnerables frente a colisiones hoy día.
### <mark style="background: #FFB86CA6;">Propiedades de un Hash Seguro</mark>
- Fácil cálculo
- Unidireccionalidad
- Resistencia a colisiones e inversión
## <mark style="background: #ADCCFFA6;">3. Implementación Práctica</mark>
Lenguajes modernos (Java, Python) proveen librerías para generar y verificar hashes. Ejemplos: MessageDigest en Java, hashlib en Python.
## <mark style="background: #ADCCFFA6;">4. Integridad en Almacenamiento</mark>
La monitorización de integridad de ficheros (Tripwire, VeriSys, Nikto) es clave para detectar cambios no autorizados. Se recomienda almacenar los resúmenes en lugares protegidos.
## <mark style="background: #ADCCFFA6;">5. Contraseñas y Salting</mark>
El uso de salt (valor aleatorio añadido a la contraseña antes del hash) previene ataques de diccionario y rainbow tables. Se recomienda salt variable. Funciones de key stretching como PBKDF2, Bcrypt y Scrypt aumentan la seguridad.
## <mark style="background: #ADCCFFA6;">6. Aplicaciones y Utilidades</mark>
- Cadena de custodia en análisis forense.
- Identificación rápida de archivos (Git, Dropbox).
- Antivirus y descarga segura de software.
- Blockchain y sellado de documentos digitales.
- Limpieza segura y recuperación de datos (Eraser, Data Shredder, Photorec).
# <mark style="background: #FFF3A3A6;">TEMA 3: Confidencialidad de la Información</mark>
## <mark style="background: #ADCCFFA6;">1. Cifrado Simétrico</mark>
Usa una clave secreta compartida. Ejemplos: DES, 3DES, Blowfish, AES, Camellia. AES es el estándar moderno, eficiente y robusto. Existen cifradores de bloque y flujo.
### <mark style="background: #FFB86CA6;">Modos de Operación</mark>
ECB cifra bloques independientemente (menos seguro), CBC usa XOR con el bloque anterior (más seguro), CFB y OFB adecuados para streaming. Los modos de relleno permiten cifrar mensajes no múltiplos del tamaño de bloque.
## <mark style="background: #ADCCFFA6;">2. Cifrado Asimétrico</mark>
Emplea un par de claves pública/privada (RSA, DSA, ElGamal, ECC). La clave pública puede ser divulgada libremente; la clave privada debe mantenerse secreta.
### <mark style="background: #FFB86CA6;">Características del Cifrado Asimétrico</mark>
- Más seguro para intercambio de claves, pero más lento y requiere claves más largas.
- Problemas: autenticidad de claves públicas, compromiso de la clave privada, pérdida y lentitud.
## <mark style="background: #ADCCFFA6;">3. Acuerdo de Claves y Protocolos</mark>
Protocolos como Diffie-Hellman permiten que dos partes acuerden una clave común sin compartirla explícitamente por el canal. Usados en VPN y otros entornos seguros.
## <mark style="background: #ADCCFFA6;">4. Protocolos Criptográficos</mark>
SSL/TLS y SSH combinan criptografía simétrica y asimétrica para autenticación y transmisión segura. WS utilizan XML Encryption para proteger mensajes a nivel de aplicación.
### <mark style="background: #FFB86CA6;">Implementación en Java y OpenSSL</mark>
Java proporciona clases específicas (Cipher, KeyPairGenerator, SecureRandom). OpenSSL es el toolkit estándar para SSL/TLS.
## <mark style="background: #ADCCFFA6;">5. Esteganografía</mark>
Técnica para ocultar información dentro de otros contenidos. Puede emplearse junto a la criptografía para incrementar la confidencialidad.
