Membangung RESTful API untuk aplikasi catatan sederhana. Dimana aplikasi ini berfungsi untuk menyimpan(create), melihat(read), mengubah(update), menghapus(delete) catatan. Fungsionalitas ini dikenal sebagai CRUD.

Kriteria 1 - Web Server dapat menyimpan catatan
 Data cukup disimpan pada memory server dalam bentuk array JavaScript.
 
 Struktur dari objek catatan yang perlu disimpan oleh server:
    {
        id: string,
        title: string,
        createdAt: string,
        upadateAt: string,
        tags: array of string,
        body: string,
    }
 Sebagai contoh :
 {
    id: 'notes-V1StGXR8_Z5jdHi6B-myT',
    title: 'Sejarah JavaScript',
    createdAt: '2020-12-23T23:00:09.686Z',
    updatedAt: '2020-12-23T23:00:09.686Z',
    tags: ['NodeJS', 'JavaScript'],
    body: 'JavaScript pertama kali dikembangkan oleh Brendan Eich dari Netscape di bawah nama Mocha, yang nantinya namanya diganti menjadi LiveScript, dan akhirnya menjadi JavaScript. Navigator sebelumnya telah mendukung Java untuk lebih bisa dimanfaatkan para pemrogram yang non-Java.',
 }

 Dalam menyimpan atau menambahkan notes, client akan mengiriman permintaan ke path dan method tersebut dengan membawa data JSON berikut pada request body:
 {
    "title": "Judul Catatan",
    "tags":["Tag 1", "Tag 2"],
    "body": "Konten catatan"
 }

 path: '/notes'
 method: 'POST'
 response: 201 (created)
 data: {
    "status": "success",
    "message": "Catatan berhasil ditambahkan",
    "data": {
        "notedId": "V09YExygSUYogwWJ"
    }
 }

 Properti lainnya seperti id, createAt, dan updateAt harus diolah di sisi server.
 
 Nilai property noteId diambil dari properti id yang dibuat secara unik.

 Jika permintaan gagal, berikan status code 500 dan kembalikan dengan data JSON denganformat berikut:
 {
    "status": "error",
    "message": "Catatan gagal untuk ditambahkan"
 }

## Kriteria 2 - Web Server dapat menampilkan catatan
Ketika client melakuakn permintaan pada path '/notes' dengan method 'GET', maka server harus menembalikan status code 200(ok) serta seluruh data notes dalam bentuk array menggunakan JSON. Contoh :
{
  "status": "success",
  "data": {
    "notes": [
      {
        "id":"notes-V1StGXR8_Z5jdHi6B-myT",
        "title":"Catatan 1",
        "createdAt":"2020-12-23T23:00:09.686Z",
        "updatedAt":"2020-12-23T23:00:09.686Z",
        "tags":[
          "Tag 1",
          "Tag 2"
        ],
        "body":"Isi dari catatan 1"
      },
      {
        "id":"notes-V1StGXR8_98apmLk3mm1",
        "title":"Catatan 2",
        "createdAt":"2020-12-23T23:00:09.686Z",
        "updatedAt":"2020-12-23T23:00:09.686Z",
        "tags":[
          "Tag 1",
          "Tag 2"
        ],
        "body":"Isi dari catatan 2"
      }
    ]
  }
}

Jika belum ada catatan satupun pada array, serveri bisa mengembalikan data notes dengan ilai array kosong.
{
  "status": "success",
  "data": {
    "notes": []
  }
}

Selain itu, client bisa melakukan pmintaan untuk mendapatkan catatan secara spesifik menggunakan id melalui path '/notes/{id}' dengan meohd 'GET'. server harus mengembalikan status code 200 (ok) serta nilai satu object catatan dalam bentuk JSON.
{
  "status": "success",
  "data": {
    "note": {
      "id":"notes-V1StGXR8_Z5jdHi6B-myT",
      "title":"Catatan 1",
      "createdAt":"2020-12-23T23:00:09.686Z",
      "updatedAt":"2020-12-23T23:00:09.686Z",
      "tags":[
        "Tag 1",
        "Tag 2"
      ],
      "body":"Isi dari catatan 1"
    }
  }
}

Jika id tidak ditemukan server respons 404 dan data :
{
  "status": "fail",
  "message": "Catatan tidak ditemukan"
}

### Kriteria 3 - Web Server dapat mengubah catatan
 path : 'notes/{id}'
 method : 'PUT'
 data : {
    "title":"Judul Catatan Revisi",
    "tags":[
        "Tag 1",
        "Tag 2"
    ],
    "body":"Konten catatan"
 }

 respons berhasil code 200 (ok) dan membawa data :
 {
    "status": "success",
    "message": "Catatan berhasl diperbaharui"
 }

 Jika id tidak ditemukan maka serer response code 404 (not found) dan data json 
 {
    "status": "fail",
    "message": "Gagal memperbarui catatan. Id catatan tidak ditemukan"
 }

### Kriteria 4 - Web server dapat menghapus catatan
 path: '/notes/{id}',
 method: 'DELETE'
 response: 200 ok 
 data: {
    "status": "success",
    "message": "Catatan berhasil dihapus"
 }

 Jika id tidak ditemukan
 response: 404 (not found),
 data: {
    "status": "fail",
    "message": "Catatan gagal dihapus. Id catatan tidak ditemukan"
 }