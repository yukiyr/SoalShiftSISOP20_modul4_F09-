LAPORAN RESMI
SISTEM OPERASI



KELOMPOK : F9

**Oleh:**

Yuki Yanuar Ratna

05111740000023

Rafif Ridho

05111840000058

**Asisten Pembimbing:**

Ibrahim Tamtama Adi

5111640000018

Departemen Teknik Infomatika

Fakultas Teknologi Elektro dan Informatika Cerdas

Institut Teknologi Sepuluh Nopember (ITS)

Surabaya

2020

**Soal**

1.	Enkripsi versi 1:

a. Jika sebuah direktori dibuat dengan awalan “encv1_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.

b. Jika sebuah direktori di-rename dengan awalan “encv1_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.

c. Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi adirektori tersebut akan terdekrip.

d. Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.

e. Semua file yang berada dalam direktori ter enkripsi menggunakan caesar cipher dengan key.

Misal kan ada file bernama “kelincilucu.jpg” dalam directory FOTO_PENTING, dan key yang dipakai adalah 10
“encv1_rahasia/FOTO_PENTING/kelincilucu.jpg” => “encv1_rahasia/ULlL@u]AlZA(/g7D.|_.Da_a.jpg
Note : Dalam penamaan file ‘/’ diabaikan, dan ekstensi tidak perlu di encrypt.

f. Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya.

**Jawaban :**

**Cara Pengerjaan**

**Encrypt**

```
char ext_cae[] = "9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_$

void encriptionLength(char* enc, int length) {
	if(strcmp(enc, ".") == 0 || strcmp(enc, "..") == 0)return;
	for(int i = length; i >= 0; i--){
		if(enc[i]=='/')break;
		if(enc[i]=='.'){
			length = i;
			break;
		}
	}
	int start = 0;
	for(int i = 0; i < length; i++){
		if(enc[i] == '/'){
			start = i+1;
			break;
		}
	}
    	for ( int i = start; i < length; i++) {
		if(enc[i]=='/')continue;
        	for (int j = 0; j < 87; j++) {
            		if(enc[i] == key[j]) {
                		enc[i] = key[(j+10) % 87];
                		break;
            		}
        	}
    	}
}
```

Melakukan enkripsi dengan caesar chiper dengan key yang dipakai 17. Jika sudah menemui titik maka fungsi akan berhenti karena ekstensi file tidak dienkripsi.

**Decrypt**

```
void decriptionLength(char * enc, int length){
	if(strcmp(enc, ".") == 0 || strcmp(enc, "..") == 0)return;
	if(strstr(enc, "/") == NULL)return;
	for(int i = length; i >= 0; i--){
		if(enc[i]=='/')break;
		if(enc[i]=='.'){
			length = i;
			break;
		}
	}
	int start = length;
	for(int i = 0; i < length; i++){
		if(enc[i] == '/'){
			start = i+1;
			break;
		}
	}
    	for ( int i = start; i < length; i++) {
		if(enc[i]=='/')continue;
        	for (int j = 0; j < 87; j++) {
            		if(enc[i] == key[j]) {
                		enc[i] = key[(j+77) % 87];
                		break;
            		}
        	}
    	}
}
```

Hampir sama seperti enkripsi, tetapi karena ini dekripsi maka beda di perhitungannya saja.

**Kendala Yang Dialami**

**Screenshot**

![Screenshot1](https://1.bp.blogspot.com/-e0i234grhx8/XqK2XWyUQhI/AAAAAAAAAzM/9dNK926UCLclAVXJcPvTFg2BFwd8MW84QCLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2020-04-24%2B17-45-29.png)

**Soal**

2.	Enkripsi versi 2:

a. Jika sebuah direktori dibuat dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.

b. Jika sebuah direktori di-rename dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.

c. Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi direktori tersebut akan terdekrip.

d. Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.

e. Pada enkripsi v2, file-file pada direktori asli akan menjadi bagian-bagian kecil sebesar 1024 bytes dan menjadi normal ketika diakses melalui filesystem rancangan jasir. Sebagai contoh, file File_Contoh.txt berukuran 5 kB pada direktori asli akan menjadi 5 file kecil yakni: File_Contoh.txt.000, File_Contoh.txt.001, File_Contoh.txt.002, File_Contoh.txt.003, dan File_Contoh.txt.004.

f. Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lain yang ada didalam direktori tersebut (rekursif).

**Jawaban :**

**Cara Pengerjaan**

Encrypt file

```
void encriptionTwo(char * path){
	FILE * file = fopen(path, "rb");
	int count = 0;
	char topath[1000];
	sprintf(topath, "%s.%03d", path, count);
	void * buffer = malloc(1024);
	while(1){
		size_t readSize = fread(buffer, 1, 1024, file);
		if(readSize == 0)break;
		FILE * op = fopen(topath, "w");
		fwrite(buffer, 1, readSize, op);
		fclose(op);
		count++;
		sprintf(topath, "%s.%03d", path, count);
	}
	free(buffer);
	fclose(file);
	remove(path);
}
```

Split file menjadi bagian kecil-kecil sebesar 1024kb.

Encrypt Folder

```
void encriptionTwoDir(char * dir){
	DIR *dp;
	struct dirent *de;
	dp = opendir(dir);
	if (dp == NULL)
		return;
	char dirPath[1000];
	char filePath[1000];
	while ((de = readdir(dp)) != NULL) {
		if(strcmp(de->d_name, ".") == 0 || strcmp(de->d_name, "..") == 0){
			continue;
		}
		if(de->d_type == DT_DIR){
			sprintf(dirPath, "%s/%s", dir, de->d_name);
			encriptionTwoDir(dirPath);
		}
		else if(de->d_type == DT_REG){
			sprintf(filePath, "%s/%s", dir, de->d_name);
			encriptionTwo(filePath);
		}
	}
	closedir(dp);
}
```

Jika folder akan looping terus sampai menemukan file untuk dienkripsi dengan fungsi sebelumnya.

**Kendala Yang Dialami**

Belum membuat decrypt dan fungsi encrypt masih belum bisa.

**Screenshot**

**Soal**

3.	Sinkronisasi direktori otomatis:

Tanpa mengurangi keumuman, misalkan suatu directory bernama dir akan tersinkronisasi dengan directory yang memiliki nama yang sama dengan awalan sync_ yaitu sync_dir. Persyaratan untuk sinkronisasi yaitu:

a. Kedua directory memiliki parent directory yang sama.

b. Kedua directory kosong atau memiliki isi yang sama. Dua directory dapat dikatakan memiliki isi yang sama jika memenuhi:

   - Nama dari setiap berkas di dalamnya sama.

   - Modified time dari setiap berkas di dalamnya tidak berselisih lebih dari 0.1 detik.

c. Sinkronisasi dilakukan ke seluruh isi dari kedua directory tersebut, tidak hanya di satu child directory saja.

d. Sinkronisasi mencakup pembuatan berkas/directory, penghapusan berkas/directory, dan pengubahan berkas/directory.

Jika persyaratan di atas terlanggar, maka kedua directory tersebut tidak akan tersinkronisasi lagi.

Implementasi dilarang menggunakan symbolic links dan thread.


**Jawaban :**

**Cara Pengerjaan**

**Kendala Yang Dialami**

Belum mengerjakan

**Screenshot**

**Soal**

4.	Log system:

a. Sebuah berkas nantinya akan terbentuk bernama "fs.log" di direktori *home* pengguna (/home/[user]/fs.log) yang berguna menyimpan daftar perintah system call yang telah dijalankan.

b. Agar nantinya pencatatan lebih rapi dan terstruktur, log akan dibagi menjadi beberapa level yaitu INFO dan WARNING.

c. Untuk log level WARNING, merupakan pencatatan log untuk syscall rmdir dan unlink.

d. Sisanya, akan dicatat dengan level INFO.

e. Format untuk logging yaitu:

```
[LEVEL]::[yy][mm][dd]-[HH]:[MM]:[SS]::[CMD]::[DESC ...]
```


LEVEL    : Level logging

yy        : Tahun dua digit

mm         : Bulan dua digit

dd         : Hari dua digit

HH         : Jam dua digit

MM         : Menit dua digit

SS         : Detik dua digit

CMD          : System call yang terpanggil

DESC      : Deskripsi tambahan (bisa lebih dari satu, dipisahkan dengan ::)


Contoh format logging nantinya seperti:

```
INFO::200419-18:29:28::MKDIR::/iz1
INFO::200419-18:29:33::CREAT::/iz1/yena.jpg
INFO::200419-18:29:33::RENAME::/iz1/yena.jpg::/iz1/yena.jpeg
```

**Jawaban :**

**Cara Pengerjaan**

**Level Warning**

```
void logWarning(char *input)
{
    FILE* fp;
    char data1[50];
    fp = fopen("/home/test/fs.log", "a");

    time_t raw;
    struct tm *timeinfo;
    char tanggal[40];
    time(&raw);
    timeinfo = localtime(&raw);

    strftime(tanggal, sizeof(tanggal), "%y%m%d-%H:%M:%S", timeinfo); 
    sprintf(data1,"WARNING::%s::%s\n", tanggal, input);
    fputs(data1, fp);

    fclose(fp);
}
```

Mencatat log ke dalam fs.log untuk syscall rmdir dan unlink. Ini merupakan log file level "WARNING".

**Level Info**

```
void logInfo(char *input)
{
    FILE* fp;
    char data1[50];
    fp = fopen("/home/test/fs.log", "a");

    time_t raw;
    struct tm *timeinfo;
    char tanggal[40];
    time(&raw);
    timeinfo = localtime(&raw);

    strftime(tanggal, sizeof(tanggal), "%y%m%d-%H:%M:%S", timeinfo); 
    sprintf(data1,"INFO::%s::%s\n", tanggal, input);
    fputs(data1, fp);

    fclose(fp);
}
```

Mencatat log ke dalam fs.log untuk syscall selain rmdir dan unlink. Ini merupakan log file level "INFO".

**Kendala Yang Dialami**

**Screenshot**

![Screenshot4](https://1.bp.blogspot.com/-U1k_7RPB7zc/XqK2XjntMfI/AAAAAAAAAzQ/JboXrpTuHQIRgrP0b2E3L147mZC3xZiTACLcBGAsYHQ/s1600/Screenshot%2Bfrom%2B2020-04-24%2B17-48-48.png)
