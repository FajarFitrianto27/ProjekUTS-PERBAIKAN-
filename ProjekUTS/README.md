# ProjekUTS
 UTS PBO - Muhammad Fajar Fitrianto_2210010748

 Kelas : 5B NonReg Banjarmasin



---

# Aplikasi Catatan Harian

Aplikasi Catatan Harian adalah sebuah aplikasi desktop berbasis Java yang memungkinkan pengguna untuk mencatat dan mengelola catatan harian. Aplikasi ini dibuat menggunakan **Java Swing** dan memiliki fitur tambahan seperti menyimpan catatan ke file dan mencetak catatan.

## Fitur Utama

1. **Tambah Catatan**: Menambahkan catatan baru dengan judul dan isi.
2. **Edit Catatan**: Memperbarui catatan yang sudah ada.
3. **Hapus Catatan**: Menghapus catatan yang tidak diperlukan.
4. **Simpan ke File**: Menyimpan semua catatan dalam bentuk file teks.
5. **Cetak Catatan**: Mencetak catatan langsung melalui printer.

## Tampilan Aplikasi
Aplikasi memiliki tampilan sederhana dengan antarmuka berbasis `JFrameForm` yang mencakup:
- `JTextField` untuk memasukkan judul catatan.
- `JTextArea` untuk menuliskan isi catatan.
- `JTable` untuk menampilkan daftar catatan yang sudah dibuat.
- Tombol aksi seperti `SIMPAN`, `EDIT`, `HAPUS`, `BUAT FILE`, dan `CETAK`.

## Cara Menggunakan

1. Jalankan aplikasi.
2. Isi `Judul` dan `Isi` pada kotak input.
3. Klik tombol `SIMPAN` untuk menyimpan catatan.
4. Pilih catatan di tabel untuk mengedit atau menghapusnya menggunakan tombol `EDIT` atau `HAPUS`.
5. Klik `BUAT FILE` untuk menyimpan semua catatan ke file teks.
6. Klik `CETAK` untuk mencetak semua catatan.

## Struktur Kode
### Pernyataan yang di gunakan
```
import javax.swing.*;                                       // Untuk komponen GUI seperti JFrame, JTable, JButton, JTextField, JTextArea, dll.
import javax.swing.table.DefaultTableModel;                 // Untuk model tabel yang digunakan dengan JTable, memungkinkan manipulasi data tabel.
import java.awt.event.*;                                    // Untuk menangani event seperti klik tombol atau aksi pengguna.
import java.util.ArrayList;                                 // Untuk menyimpan daftar catatan dalam struktur data dinamis.
import java.awt.Graphics;                                   // Untuk operasi grafis, terutama untuk pencetakan.
import java.awt.Graphics2D;                                 // Untuk manipulasi grafis tingkat lanjut, seperti translasi saat mencetak.
import java.awt.print.PageFormat;                           // Untuk menentukan format halaman cetak seperti margin dan orientasi.
import java.awt.print.Printable;                            // Antarmuka yang menentukan apa yang akan dicetak pada halaman.
import java.awt.print.PrinterException;                     // Untuk menangani kesalahan terkait pencetakan.
import java.awt.print.PrinterJob;                           // Untuk mengatur dan memulai pekerjaan pencetakan.
import java.io.File;                                        // Untuk operasi file, seperti menentukan lokasi file untuk menyimpan catatan.
import java.io.FileWriter;                                  // Untuk menulis data ke file dalam bentuk teks.
import javax.swing.JOptionPane;                             // Untuk menampilkan dialog pesan kepada pengguna, seperti informasi atau peringatan.

```

### Konstruktor
```java
public CatatanHarianForm() {
    initComponents();
    catatanIsi = new ArrayList<>();
    tableModel = new DefaultTableModel(new String[]{"Judul", "Isi"}, 0);
    jTable1.setModel(tableModel);
    jButton1.addActionListener(e -> simpanCatatan());
    jButton2.addActionListener(e -> editCatatan());
    jButton3.addActionListener(e -> hapusCatatan());
    btnSimpanKeFile.addActionListener(e -> simpanKeFile());
    btnCetak.addActionListener(e -> cetakCatatan());
}
```
- Menginisialisasi komponen GUI dan menghubungkan tombol dengan fungsi yang sesuai.

### Fungsi-Fungsi Utama
- **Simpan Catatan**:
  Menambahkan catatan ke tabel dan menyimpan ke memori sementara.
  ```java
  private void simpanCatatan() {
      String judul = jTextField1.getText();
      String isi = jTextArea1.getText();
      if (!judul.isEmpty() && !isi.isEmpty()) {
          tableModel.addRow(new Object[]{judul, isi});
          jTextField1.setText("");
          jTextArea1.setText("");
          JOptionPane.showMessageDialog(this, "Catatan berhasil disimpan!");
      }
  }
  ```

- **Edit Catatan**:
  Memperbarui catatan yang dipilih di tabel.
  ```java
  private void editCatatan() {
      int selectedRow = jTable1.getSelectedRow();
      if (selectedRow != -1) {
          jTextField1.setText((String) tableModel.getValueAt(selectedRow, 0));
          jTextArea1.setText((String) tableModel.getValueAt(selectedRow, 1));
          tableModel.removeRow(selectedRow);
      }
  }
  ```

- **Hapus Catatan**:
  Menghapus catatan yang dipilih dari tabel.
  ```java
  private void hapusCatatan() {
      int selectedRow = jTable1.getSelectedRow();
      if (selectedRow != -1) {
          tableModel.removeRow(selectedRow);
      }
  }
  ```

- **Simpan ke File**:
  Menyimpan catatan ke file teks.
  ```java
  private void simpanKeFile() {
      JFileChooser fileChooser = new JFileChooser();
      if (fileChooser.showSaveDialog(this) == JFileChooser.APPROVE_OPTION) {
          File file = fileChooser.getSelectedFile();
          try (FileWriter writer = new FileWriter(file)) {
              for (int i = 0; i < tableModel.getRowCount(); i++) {
                  writer.write("Judul: " + tableModel.getValueAt(i, 0) + "\n");
                  writer.write("Isi: " + tableModel.getValueAt(i, 1) + "\n\n");
              }
              JOptionPane.showMessageDialog(this, "Catatan berhasil disimpan!");
          } catch (Exception e) {
              JOptionPane.showMessageDialog(this, "Gagal menyimpan: " + e.getMessage());
          }
      }
  }
  ```

- **Cetak Catatan**:
  Mencetak catatan ke printer.
  ```java
  private void cetakCatatan() {
      PrinterJob job = PrinterJob.getPrinterJob();
      job.setPrintable((graphics, pageFormat, pageIndex) -> {
          if (pageIndex > 0) return Printable.NO_SUCH_PAGE;
          Graphics2D g2d = (Graphics2D) graphics;
          g2d.translate(pageFormat.getImageableX(), pageFormat.getImageableY());
          int y = 20;
          for (int i = 0; i < tableModel.getRowCount(); i++) {
              g2d.drawString("Judul: " + tableModel.getValueAt(i, 0), 10, y);
              y += 15;
              g2d.drawString("Isi: " + tableModel.getValueAt(i, 1), 10, y);
              y += 30;
          }
          return Printable.PAGE_EXISTS;
      });
      if (job.printDialog()) {
          try {
              job.print();
          } catch (PrinterException e) {
              JOptionPane.showMessageDialog(this, "Gagal mencetak: " + e.getMessage());
          }
      }
  }
  ```

## Teknologi yang Digunakan
- `Java Swing` untuk antarmuka pengguna.
- `DefaultTableModel` untuk mengelola data tabel.
- `PrinterJob` untuk mencetak catatan.
- `FileWriter` untuk menyimpan catatan ke file teks.

## Cara Menjalankan
1. Clone atau unduh proyek ini.
2. Buka di IDE seperti NetBeans atau IntelliJ IDEA.
3. Jalankan file `CatatanHarianForm.java`.

---


