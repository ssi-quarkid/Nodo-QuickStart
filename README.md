## Pasos para iniciar un nodo de Identidad Quarkid (DID Method: did:quarkid)

Como prerrequisito el nodo tiene componentes que deben ser iniciados en conjunto [Api-zkSync](https://github.com/gcba/api-zkSync/tree/master), [Api-Proxy](https://github.com/gcba/api-proxy/tree/master), mongodb e IPFS. 

## ¿Qué es la red de identidad Quarkid?
La red de identidad Quarkid se basa en *Sidetree*, una innovadora capa de escalabilidad
de protocolo de segunda capa que mejora la eficiencia y versatilidad de los
Identificadores Descentralizados (DIDs). Sidetree nos permite realizar operaciones
sobre DIDs, como creación, actualización y revocación, con una eficiencia que supera
con creces los límites de las implementaciones de primera capa.
La red QuarkID se ancla en [*zkSync*](https://zksync.io/), un protocolo de segunda capa seguro y escalable
para Ethereum. Aprovechamos las capacidades de *zkSync* para permitir transacciones
de bajo costo y alta velocidad, proporcionando así una infraestructura de utilidad
eficiente para nuestra red y su adop

## Descripción de la red de anclaje 
La red Quarkid está contruída sobre la base de la blockchain zkSync, con el objetivo de proporcionar estabilizar y eficiencia. También soporta otro tipo de redes, como RSK y cualquier ETH compatible. 
Esta arquitectura de red está diseñada para apoyar un alto volumen de operaciones DID con tiempos de confirmación de transacciones rápidas y costos de transacción mínimos.
Para permitir la interacción en la red, hemos desplegado dos contratos inteligentes en la blockchain zkSync, uno para la red principal (Mainnet) y otro para la red de pruebas (Tesnet). Estos contratos inteligentes facilitan la creación, actualización y revocación de DIDs en la red, además de proporcionar la funcionalidad necesaria para anclar y resolver DIDs utilizando Sidetree. 

### Red Mainnet 
Para la red mainnet, el contrato inteligente se puede encontrar en la siguiente dirección: 
- ZKSync: 0xe0055B74422Bec15cB1625792C4aA0beDcC61AA7
- Bloque: 2424399
- DID Method: did:quarkid
- Información de la red principal:
- Nombre de red: zkSync Era Mainnet
- URL de RPC: https://mainnet.era.zksync.io
- Identificación de la cadena: 324
- Símbolo de moneda: ETH
- URL del explorador de bloques: https://explorer.zksync.io/

### Red Tesnet
Para la red testnet, el contrato inteligente se puede encontrar en la siguiente dirección
ZKSync: 0x2535412fA22D9ad83384D7Ab7b636DDA37eFA872
Bloque: 505069
DID Method: did:quarkid:zksync
Estos datos son los que aseguran que se sincronice con la red de testnet de Quarkid
activa.
Información de la red de la red de prueba:
- Nombre de red: zkSync Era Testnet
- URL de RPC: https://sepolia.era.zksync.dev
- Identificación de la cadena: 280
- Símbolo de moneda: ETH
- URL del explorador de bloques: https://sepolia.explorer.zksync.io/

Para obtener fondos en la red zkSync, es necesario utilizar un puente (bridge) disponible para transferir Ether desde la red principal (Red Mainnet) o una red de prueba (Tesnet) como Sepolia, a la correspondiente (ZkSync). 

## Relación con el componente API Proxy
API proxy o también denominado reverse proxy es un componente diseñado para
funcionar como un punto de entrada a múltiples did methods con el objetivo de facilitar
su manejo y resolución ya que cada implementación tiene sus propias especificaciones
y dinámicas.
El componente funciona con *tres endpoints*:

### Primer endpoint
````
GET /mappings
````

Su respuesta nos brinda una lista de nodos disponibles para operar con Api Proxy.
Ejemplo:
````
{
    "list": [
         {
            "url":"https://URL_TO_api-zksync",
            "pattern": "did:quarkid",
            "behavior": 1
          }
    ]
}
````

### Segundo endpoint 
````
GET /resolve/:did
````

Recibe como parámetro un DID y nos da el poder de resolver un did-document, en este
caso el proxy retornara el documento variando del método consumido. Por ejemplo,
Api-zksync define cómo será la respuesta, atento a que este componente solo tiene la
funcion de proxy.

### Tercer endpoint
````
POST /create
````
Este endpoint recibe como parámetros el did-method a utilizar y la request. Al igual que
el anterior, su respuesta varía dependiendo del did-registry.

## Relación con el componente API zkSync
Es un componente que sigue las especificaciones de SideTree para interactuar con una
blockchain y gestionar registros descentralizados de manera eficiente. Utiliza:

### 1. MongoDB
Base de datos NoSQL orientado a documentos, que almacena datos en estructuras
flexibles tipo JSON llamadas BSON (Binary JSON). 

### 2. IPFS
IPFS, o el Sitema de Archivos Interplanetarios, es un protocolo y una red diseñados para crear un método de almacenamiento y acceso a archivos descentralizado y distribuído que busca conectar todos los dispositivos informáticos con el mismo sistema de archivos.

## Red zkSync
Como se aclaró anteriormente, esta solución de escabilidad para Ethereum utiliza pruebas de
conocimiento cero (Zero-Knowledge Proofs - zkProofs) para permitir transacciones
rápidas y seguras con tarifas significativamente más bajas que las de la red principal. La
tecnología "rollup" de zkSync agrupa cientos de transacciones fuera de la cadena de
Ethereum (off-chain) en un solo lote, y luego demuestra su validez en la cadena de
bloques principal (on-chain) con una sola prueba de conocimiento cero.

## Obtención de los componentes
### Pre-requisitos
1. Tener docker instalado en el sistema que se utilizará en la instalación.
2. Contar con acceso a Internet y permisos necesarios para descargar imágenes del registro de contenedores oficial de [Docker](https://hub.docker.com/u/quarkid)

### Pasos
Paso 1: Abrir Docker. Una vez autenticado, puede proceder a descargar las imágenes necesarias con el comando *docker pull*:
````
 # docker pull quarkid/api-proxy
 # docker pull quarkid/api-zksync
 # docker pull ipfs/kubo:latest
 # docker pull mongo:latest
````

Paso 2: Verificar las imágenes descargadas con *docker image ls*:
````
 REPOSITORY TAG IMAGE ID SIZE
 quarkid/api-proxy latest cb8941a6fbe4 16 hours ago 1.26GB
 quarkid/api-zksync latest 13e12acfd1c0 5 months ago 3.56GB
 ipfs/kubo latest 71f8fff78bb2 3 days ago 94.6MB
 mongo
````
### Configuración de variables de los componentes 

### API Proxy
1. Variable: NODE_1_URL
   - Valor: IP:PUERTO
   - Descripción: IP y Puerto del componente API Proxy
2. Variable: NODE_1_PATTERN
   - Valor: did:quarkid
   - Descripción: Did método que resuelve NODE_1_URL
1. Variable: NODE_1_BEHAVIOR
   - Valor: 1
   - Descripción: Activo

### API zkSync
A este componente se le deben configurar las siguientes variables:
1. Variable: DID_METHOD_NAME
   - Valor: quarkid
   - Descripción: Did metodo
2. Variable: CONTENT_ADDRESSABLE_STORE_SERVICE_URI
   - Valor: IP:PUERTO
   - Descripción: IP y Puerto del componente IPFS
3. Variable: DATABASE_NAME
   - Valor: -zksync-mainnet-v1
   - Descripción: Nombre de la base de datos
4. Variable: RPC_URL
   - Valor: https://mainnet.era.zksync.i
   - Descripción: URL de la red donde se va a anclar el DID
5. Variable: MONGO_DB_CONNECTION_STRING
   - Valor: mongodb://10.1.0.2:27017
   - Descripción: String de conexion al componente mongoDb
6. Variable: MAX_CONCURRENT_DOWNLOADS
   - Valor: 20
   - Descripción: Cantidad de operaciones a descargar por intervalo
7. Variable: OBSERVING_INTERVAL_IN_SECONDS
   - Valor: 30
   - Descripción: Tiempo de lectura en blockchain
8. Variable: BATCHING_INTERVAL_IN_SECONDS
   - Valor: 21600
   - Descripción: Tiempo de grabado en blockchain
9. Variable: BLOCKCHAIN_VERSION
   - Valor: latest 
   - Descripción: Versión de Blockchain
10. Variable: MODENA_ANCHO_CONTRACT
    - Valor: 0xe0055B74422Bec15cB1625792C4aA0beDcC61AA7
    - Descripción: Address del contrato inteligente de Quarkid
11. WALLET_PRIVATE_KEY:
    - Valor: XXX
    - Descripción: Clave privada de address con saldo de red zkSync
12. ACCOUNT_ADDRESS:
    - Valor: 0x9CAA73a4865fa9dbb125b6C7B2f03b6621234
    - Descripción: Address de la clave privada anterior
13. LEDGER_TYPE:
    - Valor: zksync | rsk | eth
    - Descripción: Red blockchain a utilizar 

### MongoDB
Este componente no tiene configuraciones adicionales.

### IPFS
Este componente no tiene configuraciones adicionales.

----------------------------------------------------------------------------------------------------------------------------
## Ejemplo de despliegue

Para simplicar, daremos un ejemplo del proceso de despliegue y gestión de
nuestra infraestructura de contenedores utilizando Docker Compose.

Los componentes fueron probados con éxito en las siguientes orquestadores:
1. Azure - AKS
2. Google - GKS
3. AWS - EKS
4. Docker-compose
5. Cualquier orquestador que emplee Docker como su tecnología de contenedorización.

Para realizar estas pruebas, usted debe tener configurado en Tesnet una address y debe transferir saldo para poder ejecturar las transacciones. 
````
ACCOUNT_ADDRESS==****
WALLET_PRIVATE_KEYS=***
````

Un ejemplo de archivo docker-compose.yml:
````
version: '3.7'
services:
api-sksync:
image: quarkid/api-zksync:latest
container_name: api-zksync-cont
ports:
- 8000:3000
restart: always
environment:
- DID_METHOD_NAME=quarkid:zksync
- CONTENT_ADDRESSABLE_STORE_SERVICE_URI=http://ipfs-quarkid:5001/
- DATABASE_NAME=zksync-testnet-v1
- RPC_URL=https://sepolia.era.zksync.dev
- MONGO_DB_CONNECTION_STRING=mongodb://mongo-quarkid:27017
- MAX_CONCURRENT_DOWNLOADS=20
- OBSERVING_INTERVAL_IN_SECONDS=30
- BATCHING_INTERVAL_IN_SECONDS=60
- STARTING_BLOCKCHAIN_TIME=505069
- BLOCKCHAIN_VERSION=latest
- MODENA_ANCHOR_CONTRACT=0x2535412fA22D9ad83384D7Ab7b636DDA37eFA872
- WALLET_PRIVATE_KEY=***
- ACCOUNT_ADDRESS=***
- LEDGER_TYPE=zksync

api-proxy:
 image: quarkid/api-proxy:latest
 container_name: api-proxy-cont
 ports:
- 8080:8080 #MODENA API port
 environment:
- NODE_1_URL=api-sksync:8000
- NODE_1_PATTERN=did:quarkid:zksync
- NODE_1_BEHAVIOR=1

mongo-quarkid:
 image: mongo
 restart: always
 expose:
- 27017
 ports:
- 27017:27017

ipfs:
 restart: always
 container_name: ipfs-quarkid
 image: ipfs/kubo
 environment:
- IPFS_PATH=/data/ipfs
 volumes:
- ./ipfs:/data/ipfs
 ports:
- 4001:4001
- 5001:5001
````
Al revisar los logs de api-zksync, usted notará que los primeros minutos se observa la sincronización con otros nodos. En algunos casos, se pueden generar mensajes del tipo {"code":"content_not_found"}, esto significa que otro nodo grabó una transacción en la red, pero no se abrió el IPFS para sincronizar la información. No afecta al nodo, solo que esos DIDs no van a poder resolverse. Por eso es importante que IPFS esté correctamente sincronizado. 


