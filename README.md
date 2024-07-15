# HTTP _Service_

HTTP _Service_ adalah _service_ yang bertugas untuk meng-_handle_ HTTP _Endpoint_ pada internal Lawang. _By default_, HTTP _Service_ menerapkan _circuit breaker_ per _backend/connection/host_ untuk menghindari _traffic flooding_ dan mengoptimalkan kinerja dari Lawang itu sendiri. HTTP Service juga menerapkan [zero trust](https://www.krakend.io/docs/design/zero-trust) untuk memastikan keamanan transmisi antara _client_ dan _backend/connection/host_.

HTTP _Service support Basic Auth, Single Key Auth (Api Key Only)_ maupun _Double Keys Auth (Api Key and Token)_, selain itu HTTP _Service_ juga mendukung _response transformation_ untuk memudahkan dalam memanipulasi _response_ dari _endpoint_. _Response transformation_ hanya dapat digunakan untuk _json response_.  

## _JSON to JSON Transformation (Response Transformation)_

JSON to JSON _Transformation_ berfungsi untuk memanipulasi _response_ pada HTTP _Service_ menggunakan spesifikasi sesuai dengan spesifikasi [GJson](https://github.com/tidwall/gjson) dengan _syntax_ sebagai berikut:

```json
[
	{
		"dest": "GSJON spec"
	}
]
```

Dan berikut adalah contoh penggunaan dari _response transformer_:

- _Response_ asli

```json
[
  {
    "id": "1",
    "name": "Google Pixel 6 Pro",
    "data": {
      "color": "Cloudy White",
      "capacity": "128 GB"
    }
  },
  {
    "id": "2",
    "name": "Apple iPhone 12 Mini, 256GB, Blue",
    "data": null
  },
  {
    "id": "3",
    "name": "Apple iPhone 12 Pro Max",
    "data": {
      "color": "Cloudy White",
      "capacity GB": 512
    }
  },
  {
    "id": "4",
    "name": "Apple iPhone 11, 64GB",
    "data": {
      "price": 389.99,
      "color": "Purple"
    }
  },
  {
    "id": "5",
    "name": "Samsung Galaxy Z Fold2",
    "data": {
      "price": 689.99,
      "color": "Brown"
    }
  },
  {
    "id": "6",
    "name": "Apple AirPods",
    "data": {
      "generation": "3rd",
      "price": 120
    }
  },
  {
    "id": "7",
    "name": "Apple MacBook Pro 16",
    "data": {
      "year": 2019,
      "price": 1849.99,
      "CPU model": "Intel Core i9",
      "Hard disk size": "1 TB"
    }
  },
  {
    "id": "8",
    "name": "Apple Watch Series 8",
    "data": {
      "Strap Colour": "Elderberry",
      "Case Size": "41mm"
    }
  },
  {
    "id": "9",
    "name": "Beats Studio3 Wireless",
    "data": {
      "Color": "Red",
      "Description": "High-performance wireless noise cancelling headphones"
    }
  },
  {
    "id": "10",
    "name": "Apple iPad Mini 5th Gen",
    "data": {
      "Capacity": "64 GB",
      "Screen size": 7.9
    }
  },
  {
    "id": "11",
    "name": "Apple iPad Mini 5th Gen",
    "data": {
      "Capacity": "254 GB",
      "Screen size": 7.9
    }
  },
  {
    "id": "12",
    "name": "Apple iPad Air",
    "data": {
      "Generation": "4th",
      "Price": "419.99",
      "Capacity": "64 GB"
    }
  },
  {
    "id": "13",
    "name": "Apple iPad Air",
    "data": {
      "Generation": "4th",
      "Price": "519.99",
      "Capacity": "256 GB"
    }
  }
]
```

- _Tranformer_

```json
[
    {
        "Id": "id",
        "Name": "name",
        "Type": "!Gadget",
        "Available": "!true",
        "Stock": "!100",
        "Price": "data.*ice",
        "Color": "data.*lor",
        "Capacity": "data.*city"
    }
]
```

- _Response_ akhir

```json
[
	{
		"Available": true,
		"Capacity": "128 GB",
		"Color": "Cloudy White",
		"Id": 1,
		"Name": "Google Pixel 6 Pro",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 2,
		"Name": "Apple iPhone 12 Mini, 256GB, Blue",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Cloudy White",
		"Id": 3,
		"Name": "Apple iPhone 12 Pro Max",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Purple",
		"Id": 4,
		"Name": "Apple iPhone 11, 64GB",
		"Price": 389.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Brown",
		"Id": 5,
		"Name": "Samsung Galaxy Z Fold2",
		"Price": 689.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 6,
		"Name": "Apple AirPods",
		"Price": 120,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 7,
		"Name": "Apple MacBook Pro 16",
		"Price": 1849.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "data.*lor",
		"Id": 8,
		"Name": "Apple Watch Series 8",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "data.*city",
		"Color": "Red",
		"Id": 9,
		"Name": "Beats Studio3 Wireless",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "64 GB",
		"Color": "data.*lor",
		"Id": 10,
		"Name": "Apple iPad Mini 5th Gen",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "254 GB",
		"Color": "data.*lor",
		"Id": 11,
		"Name": "Apple iPad Mini 5th Gen",
		"Price": "data.*ice",
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "64 GB",
		"Color": "data.*lor",
		"Id": 12,
		"Name": "Apple iPad Air",
		"Price": 419.99,
		"Stock": 100,
		"Type": "Gadget"
	},
	{
		"Available": true,
		"Capacity": "256 GB",
		"Color": "data.*lor",
		"Id": 13,
		"Name": "Apple iPad Air",
		"Price": 519.99,
		"Stock": 100,
		"Type": "Gadget"
	}
]
```

*NB: Jika Anda perlu lebih dari sekedar _response manipulations_, saya sarankan Anda menggunakan Javascript _Service_.*

# _Database Service_

_Database Service_ adalah _service_ yang bertugas untuk meng-_handle endpoint database_. _Database Service_ mem-_provide_ beberapa _driver database_ antara lain MySQL, PostgSQL, Oracle, dll yang dapat mengimplementasikan [`database/sql`](https://go.dev/wiki/SQLDrivers). Pada _Database Service_, hanya diperbolehkan melakukan _query_ `SELECT`.

# _File Service_

_File Service_ adalah _service_ yang bertugas untuk mengakses _file_ dalam _server_ secara aman. _File Service_ hanya dapat diakses menggunakan _credentials_ (_private endpoint_)

# Javascript _Service_

Javascript _Service_ adalah _service_ yang bertugas untuk meng-_handle_ _endpoint_ Javascript. Anda dapat melakukan apapun pada _service_ ini termasuk _HTTP Call_, _Database Query, Database Manipulation, Load File, Store File,_ dan pastinya memanipulasi _request_ maupun _response_. Javascript _Service_ diperuntukan untuk mengakomodir _custom logic_ serta untuk memudahkan dalam integrasi dengan sistem lain tanpa perlu membuat _middleware system_ antara Lawang dan _client_.

Javascript _Service_ menggunakan EcmaScript6 dengan beberapa penambahan _objects_ maupun _functions_ bawaan namun tidak mendukung untuk menggunakan EcmaScript6 _module_, sehingga `export` dan `import` _keywords_ tidak dapat digunakan pada Lawang.

## _Internal Objects_ dan _Functions_

Berikut adalah _internal objects_ dan _functions_ yang tersedia pada Lawang. Semua yang datang dari lawang baik *_objects_* maupun *_response functions_* akan dikirim dalam format `string` kecuali untuk tipe data primitif, sebagian berisi JSON _string_ sehingga perlu fungsi `JSON.parse()` untuk mengubah menjadi JSON _object_

```js
// string
$GT.uri
// string
$GT.method
// string
$GT.queries
// string
$GT.params
// string
$GT.headers
// string
$GT.payload

/**
 * Fungsi ini diload menggunakan plugin
 * 
 * payload: string
 **/
$GT.uuid(): string
$GT.md5(payload): string

/**
 * url: string
 * method: string
 * body: object
 * headers: object
 * timeout: int (in second)
 **/
$GT.request(method, url): string
$GT.request(method, url, body): string
$GT.request(method, url, timeout): string
$GT.request(method, url, body, headers): string
$GT.request(method, url, body, timeout): string
$GT.request(method, url, body, headers, timeout): string

$GT.get(url): string
$GT.get(url, headers): string
$GT.get(url, timeout): string
$GT.get(url, headers, timeout): string

$GT.post(url): string
$GT.post(url, body): string
$GT.post(url, timeout): string
$GT.post(url, body, headers): string
$GT.post(url, body, timeout): string
$GT.post(url, body, headers, timeout): string

$GT.put(url): string
$GT.put(url, body): string
$GT.put(url, timeout): string
$GT.put(url, body, headers): string
$GT.put(url, body, timeout): string
$GT.put(url, body, headers, timeout): string

$GT.patch(url): string
$GT.patch(url, body): string
$GT.patch(url, timeout): string
$GT.patch(url, body, headers): string
$GT.patch(url, body, timeout): string
$GT.patch(url, body, headers, timeout): string

$GT.delete(url): string 
$GT.delete(url, headers): string 
$GT.delete(url, timeout): string 
$GT.delete(url, headers, timeout): string 

/**
 * source: string
 * sql: string
 * table: string
 * identities: object
 * payload: object
 **/
$GT.query(source, sql): string 
$GT.save(source, table, payload): void
$GT.upsert(source, table, identities, payload): void
$GT.update(source, table, identities, payload): void

/**
 * Fungsi ini diload menggunakan plugin
 * 
 * payload: string
 * encoded: string
 * plainText: string
 * secret: string
 * privateKey: string
 * publicKey: string
 **/
$GT.hmac512(payload, secret): string
$GT.sha256_signing(payload, privateKey): string
$GT.sha256_verify(encoded, plainText, publicKey): bool

/**
 * Fungsi ini diload menggunakan plugin
 * 
 * xml: string
 * json: string
 * spec: string
 **/
$GT.xml2json(xml): string
$GT.transform(json, spec): string

/**
 * source: string
 * path: string
 * body: string
 **/
$GT.load_file(source, path): string
$GT.store_file(source, body, path): void

/**
 * body: object
 * code: int
 **/
$GT.response(body, code): void

```

## Javascript _Libraries_

Selain _internal objects_ dan _functions_, Lawang juga secara _default_ mendukung penggunaan _libraries_ Javascript berikut:

- [MomentJS](https://momentjs.com) : dapat dipanggil dengan _syntax_ `moment()`
