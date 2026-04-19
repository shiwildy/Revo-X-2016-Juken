# JRJX Decode Reference

Dokumen ini menjelaskan cara mengubah file `.Jrjx` menjadi angka map mentah.

## Format Overview

File `.Jrjx` tidak menyimpan angka dalam plain text. Struktur datanya:

1. Teks Base64
2. Hasil decode Base64 adalah ciphertext AES-CBC
3. Hasil decrypt adalah plain text UTF-8 berisi angka per baris

## Decode Procedure

1. Baca isi `.Jrjx` sebagai satu string.
2. Lakukan Base64 decode menjadi bytes ciphertext.
3. Bentuk key 16-byte dari string password `Felix`:
   - Konversi `Felix` ke UTF-8 bytes.
   - Salin ke buffer 16-byte.
   - Sisa buffer diisi `0x00`.
4. Lakukan AES-CBC decrypt dengan parameter:
   - Key: `buffer16`
   - IV: `buffer16`
   - KeySize: `128`
   - BlockSize: `128`
   - Padding: `PKCS7`
5. Konversi hasil decrypt ke UTF-8 text.
6. Split text per baris (`\r\n` atau `\n`).
7. Parse setiap baris non-kosong sebagai angka.

## Pseudocode

```text
jrjx_text = read_text("file.Jrjx").trim()
cipher_bytes = base64_decode(jrjx_text)

pwd = utf8("Felix")
key16 = byte[16] {0}
copy(min(len(pwd), 16), pwd -> key16)

plain_bytes = AES_CBC_Decrypt(
  data=cipher_bytes,
  key=key16,
  iv=key16,
  padding=PKCS7
)

plain_text = utf8_decode(plain_bytes)
numbers = []
for line in split_lines(plain_text):
  if line not empty:
    numbers.append(parse_number(line))
```

## Output Handling

Output decrypt berupa deret angka linear. Untuk menjadi tabel map, kelompokkan angka sesuai kontrak axis pada `REFERENCE_AXIS.md`.
