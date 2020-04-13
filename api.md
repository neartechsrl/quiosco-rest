QUIOSCO - REST API
------------------

Usuario: 		NTQuiosco
Clave: 			123456789
Autorización: 	Basic
ContentType:	application/json
CharSet: 		utf-8


## Métodos GET
     
##### GET /parametros

Recibe parámetros asignados al cliente

```
curl -L -X GET 'http://192.168.1.11:3000/parametros' -H 'Authorization: Basic TlRRdWlvc2NvOjEyMzQ1Njc4OQ=='
```

#### Ejemplo JSON

```json
{
    "monto_minimo_reparto": "1000",
    "codigo_transporte": "1",
    "nombre_transporte": "RETIRA CLIENTE",
    "transportes": [
        {
            "codigo_transporte": "2",
            "nombre_transporte": "ENTREGA REPARTO"
        }
    ]
}
```

##### GET /articulos 

Recibe todos los artículos

Parámetros ( ej: GET /articulos?sort_by=code-desc&page=2&q=pepe)
----------
sort_by			Ordenar artículos por un criterio en particular.
page 			Páginas a consultar ( 50 registros por página )
q				Filtro por descripción
id-folder		Carpeta de clasificador
binary_img		Indica si las imagenes vienen en formato binario o solo la uri. 1=Si, 0=No -> valor x defecto, se envía la uri.
clasificador	Indica si se filtra por carpetas clasificador parametrizadas. 1=Si, 0=No -> valor x defecto.
code			Filtro por código de artículo o código de barra

Criterios de Ordenamiento
-------------------------
code-asc	Ordenar por código ascendente
code-desc	Ordenar por código descendente
alpha-asc	Ordenar por descripción ascendente
alpha-desc	Ordenar por descripción descendente

```
curl -L -X GET 'http://192.168.1.11:3000/articulos?clasificador=1&q=ACERO&page=1' -H 'Authorization: Basic TlRRdWlvc2NvOjEyMzQ1Njc4OQ=='
curl -L -X GET 'http://192.168.1.11:3000/articulos?page=1&binary_img=0&id-folder=194' -H 'Authorization: Basic TlRRdWlvc2NvOjEyMzQ1Njc4OQ=='
```

#### Ejemplo JSON

```json
[
    {
        "codigo": "0001",
        "descripcion": "tv samsung 49",
        "adicional": "oled",
        "sinonimo": "tvs49",
        "codigo_barra": "00010010001",
        "unidad_medida_ventas": "UNI",
        "unidad_medida_stock": "UNI",
        "equivalencia": "1",
        "porcentaje_iva": "21.0",
        "porcentaje_ii": "0.0",
        "foto": "R0lGODlhIAAgAPIAAP...",
		"numero_lista": "1",
		"descripcion_lista": "mayorista",
		"precio": "25000.00",
		"incluye_iva": "1",
		"incluye_ii": "0"
    },
    {
        "codigo": "0002",
        "descripcion": "tv samsung 32",
        "adicional": "oled",
        "sinonimo": "tvs32",
        "codigo_barra": "00020020002",
        "unidad_medida_ventas": "UNI",
        "unidad_medida_stock": "UNI",
        "equivalencia": "1",
        "porcentaje_iva": "21.0",
        "porcentaje_ii": "0.0",
        "foto": "R0lGODlhIAAgAPIAAP...",
		"numero_lista": "1",
		"descripcion_lista": "mayorista",
		"precio": "15000.00",
		"incluye_iva": "1",
		"incluye_ii": "0"
    }

]
```

##### GET /clasificador

Parámetros ( ej: GET /clasificador?id-parent=2)
----------
id-parent	Carpetas del clasificador con idparent = nn

#### Ejemplo JSON

```json
[
    {
	"id_folder": "2",
	"descripcion": "Rubro",
	"id_parent": "1",
	"child_count": "3"
    },
    {
	"id_folder": "7",
	"descripcion": "Proveedor Extranjero",
	"id_parent": "1",
	"child_count": "2"
    }    
]
```

##### GET /arbol_clasificador

Devuelve todo el árbol del clasificador.

```
curl -L -X GET 'http://192.168.1.11:3000/arbol_clasificador' -H 'Authorization: Basic TlRRdWlvc2NvOjEyMzQ1Njc4OQ=='
```

#### Ejemplo JSON

```json
[
  {	
    "id_folder":"142",
    "descripcion":"Favoritos",
    "id_parent":"141",
    "child_count":"0"
  },
  {
    "id_folder":"143",
    "descripcion":"Metalurgia",
    "id_parent":"141",
    "child_count":"8"
  },
  {
    "id_folder":"164",
    "descripcion":"Angulos",
    "id_parent":"143",
    "child_count":"0"
  },
  {
    "id_folder":"169",
    "descripcion":"Chapas",
    "id_parent":"143",
    "child_count":"2"
  }  
]
```

##### POST /pedido ( grabar un pedido )

```
curl -L -X POST 'http://192.168.1.11:3000/pedido' -H 'Content-Type: application/json; charset=UTF-8' -H 'Authorization: Basic TlRRdWlvc2NvOjEyMzQ1Njc4OQ==' -H 'Content-Type: application/json' --data-raw '{
  "detalle":[
    {
      "codigo":"002600000080",
      "cantidad":"2",
      "precio":"56.128",
      "descuento":"0"
    },
    {
      "codigo":"002600000100",
      "cantidad":"1",
      "precio":"56.985",
      "descuento":"0"
    }
  ],
  "total":"1148.6436",
  "leyenda_1":"",
  "leyenda_2":"",
  "leyenda_3":"",
  "leyenda_4":"",
  "leyenda_5":"",
  "codigo_transporte":"1",
  "nombre":"",
  "domicilio":"",
  "documento":""
}
'
```

#### Ejemplo JSON

```json
{
	"total": "120.00",
	"leyenda_1": "",
	"leyenda_2": "",
	"leyenda_3": "",
	"leyenda_4": "",
	"leyenda_5": "",
	"codigo_transporte": "1",
	"nombre": "cliente ocacional",
	"domicilio": "mitre 123",
	"documento": "20304050",
	"detalle": [
		{
			"codigo": "0001",
			"cantidad": "1",
			"precio": "10.20",
			"descuento": "0.0"
		},
		{
			"codigo": "0002",
			"cantidad": "3",
			"precio": "15.20",
			"descuento": "5.0"
		}
	]
}
```

Respuesta Servidor

```json
{
  "numero_pedido":" 000000000017",
  "codigo_cliente":"000000",
  "nombre": "cliente ocacional",
  "domicilio": "mitre 123",
  "documento": "20304050",
  "total":"1270.50",
  "leyenda_1":"",
  "leyenda_2":"",
  "leyenda_3":"",
  "leyenda_4":"",
  "leyenda_5":""
  "numero_lista":"4", 
  "descripcion_lista": "LISTA EMPERSAS",
  "incluye_iva": "0",
  "incluye_ii": "0",
  "codigo_transporte": "1",
  "nombre_transporte": "RETIRA CLIENTE",
  "detalle":[
    {
      "codigo":"BARRA",
      "cantidad":"2",
      "precio":"35",
      "descuento":"0",
      "descripcion":"BARRA HIERRO ACERO 1020",
      "adicional":"",
      "sinonimo":"",
      "codigo_barra":"",
      "unidad_medida_ventas":"UNI",
      "unidad_medida_stock":"KILOGRAMOS",
      "equivalencia":"15",
      "porcentaje_iva":"21",
      "porcentaje_ii":"0",
      "precio_pan":"635.25"
    }    
  ]
}
```
