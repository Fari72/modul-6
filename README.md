###### Nama: Fari' Affrizal S.
###### Kelas: RPL 1
###### No.Absen: 25
## modul-6
###### INTERAKSI DATABASE DENGAN QUERY BUILDER

>Tugas implementasikan query builder dalam projek anda

Dalam tugas ini kita membuat database bisa terhubung dengan query builder, dalam penjelasan ini saya akan menunjukkan hasil dari pemahaman saya

kita langsung ke **folder** `App\Http\Controllers\BarangController`

syntax akan seperti dibawah ini
```
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use DB;
class BarangController extends Controller
{
    public function index()
    {
        $barang = DB::table('barang')
        ->get();
        return view('barang/home', compact('barang'));
    }

    public function store()
    {
       DB::table('barang')->insert([
            'nama' => 'Lampu',
            'id_kategori' => '1',
            'qty' => '12',
            'harga_beli' => 40000,
            'harga_jual' => 45000
        ]);
        echo "data berhasil disimpan";
    }

    public function update()
    {
        DB::table('barang')->where('id',3)->update([
            'nama' => 'Lampu',
            'id_kategori' => '1',
            'qty' => '12',
            'harga_beli' => 40000,
            'harga_jual' => 45000
        ]);
        echo 'Data berhasil diupdate';
    }

    public function delete()
    {
        DB::table('barang')->where('id',3)->delete();
        echo 'Data berhasil dihapus';
    }
}
```

lalu **folder** bagian `App\Http\Controllers\KategoriController`
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use DB;

class KategoriController extends Controller
{
    public function index()
    {
        $kategori = DB::table('kategori')
        ->get();
        Return view('kategori/index', compact('kategori'));
    }

    public function store()
    {
       DB::table('kategori')->insert([
            'nama' => 'mouse',
        ]);
        echo "data berhasil disimpan";
    }

    public function update()
    {
        DB::table('kategori')->where('id',2)->update([
            'nama' => 'komputer'
        ]);
        echo 'Data berhasil diupdate';
    }

    public function delete()
    {
        DB::table('kategori')->where('id',2)->delete();
        echo 'Data berhasil dihapus';
    }
}
```

<!-- 
Pada code diatas; 
public function index() untuk mendapatkan data
public function store() untuk menyimpan data
public function update() untuk memperbarui data
public function delete() untuk menghapus data
-->

Selanjutnya kita menambahkan code 
```
@foreach($barang as $key => $item) ... @endforeach
```
kita akan menambahkan codenya pada folder **barang** file `home.blade.php`

code akan seperti dibawah ini;
```@extends('layout.app')

@section('title')
    Data Barang
@endsection

@section('content')
<div class="table table-striped mt-5">
<table class="table table-striped">
    <thead>
    <tr>
            <th >No</th>
            <th>Nama</th>
            <th>Kategori</th>
            <th>QTY</th>
            <th>Harga Beli</th>
            <th>Harga Jual</th>
            <th>Aksi</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            @foreach($barang as $key => $item)
            <td width=2%>{{$key+1}}</td>
            <td>{{$item->nama}}</td>
            <td width=15%>{{$item->id_kategori}}</td>
            <td width=10%>{{$item->qty}}</td>
            <td width=15%>{{$item->harga_beli}}</td>
            <td width=15%>{{$item->harga_jual}}</td>
            <td>Hapus | Edit</td>
        </tr>
        @endforeach
    </tbody>
</div>
</table>
@endsection
```

kita akan menambahkan codenya pada folder **kategori** file `index.blade.php`

codenya akan seperti dibawah ini;
```
@extends('layout.app')

@section('title')
    Data Kategori
@endsection

@section('content')
<div class="mt-3">
    <table class="table table-striped">
    <thead>
        <tr>
            <th width=5%>No</th>
            <th>Nama</th>
            <th width=15%>Aksi</th>
        </tr>
    </thead>
    
    <tbody>
        @foreach($kategori as $key => $item)
            <td>{{$key+1}}</td>
            <td>{{$item->nama}}</td>
            <td>Hapus | Edit</td>
            
        </tr>
        @endforeach
        
    </tbody>
</div>
</table>
@endsection
```

Selanjutnya kita akan memanggil dari url browser

codenya pada **folder** `routes/web.php`

```
<?php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\BarangController;
use App\Http\Controllers\KategoriController;

Route::get('/halaman', function(){
    $title = 'Harry Pooter';
    $konten = 'harry pooter and the deathly hallows: part2';
    return view('konten.halaman', compact('title','konten'));
}); 

Route::get('/', [KategoriController::class, 'index']);

Route::get('/kategori', [KategoriController::class, 'index']);
Route::get('/kategori/store', [KategoriController::class, 'store']);
Route::get('/kategori/update', [KategoriController::class, 'update']);
Route::get('/kategori/delete', [KategoriController::class, 'delete']);

Route::get('/barang', [BarangController::class, 'index']);
Route::get('/barang/store', [BarangController::class, 'store']);
Route::get('/barang/update', [BarangController::class, 'update']);
Route::get('/barang/delete', [BarangController::class, 'delete']);
```
