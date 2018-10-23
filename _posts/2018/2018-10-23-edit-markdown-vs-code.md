---
title:  "Mengubah plugin Markdown Shortcuts di Visual Studio Code"
date:   2018-10-23 9:00:00 +0800
short_desc : '<p>Do it however I like</p>'
image: 
tag:
 - tips
 - vscode
header_image: www/vscode/vscode_1.18_icon.svg
---

Visual Studio Code adalah editor teks yang sedang *in* beberapa tahun belakangan ini, berkompetisi dengan Sublime Text yang sudah tergolong pro. Perbedaannya, Visual Studio Code gratis sementara Sublime Text berbayar, US$ 80.

Di dalam editor ini, ada fasilitas plugin yang memungkinkan orang menambahkan fitur-fitur pada editor. Salah satu plugin yang saya <span title="Baru-baru ini" style="text-decoration:underline solid;">pakai</span> adalah Markdown Shortcuts oleh mdickin. Satu yang saya kurang suka dari plugin ini adalah penandaan *italic* dengan simbol *underscore*, sementara saya inginnya pakai *asterisk*. Namun, pagi ini saya dapat inspirasi untuk memodifikasi plugin ini.

VS Code *being* VS Code, seluruh strukturnya ditulis dengan Javascript. *It stands to reason that the plugin is also written in Javascript*.

Pergi ke `C:\Users\<yourusername>\.vscode\extensions\mdickin.markdown-shortcuts-0.8.1\lib` dan buka berkas *commands.js*. Cari fungsi ini:

```javascript
function toggleItalic() {
    return editorHelpers.surroundSelection('_', '_', toggleItalicPattern);
}
```

Lalu ganti saja simbol *underscore* (_) dengan simbol *asterisk* (*). Simpan. Tutup dan buka kembali editor Anda.

*Genieß es!*