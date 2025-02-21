# ProjekUTS
 perbaikan dari java swing ke JFrameForm


---




---

## **🚀 Fitur Aplikasi**  
✅ **Tambah Catatan** – Simpan catatan dengan judul dan isi  
✅ **Edit Catatan** – Mengubah catatan yang sudah ada  
✅ **Hapus Catatan** – Menghapus catatan dari daftar  
✅ **Simpan ke File** – Menyimpan catatan ke file `.txt`  
✅ **Cetak Catatan** – Mencetak catatan langsung dari aplikasi  

---



---


---

## **📜 Cara Menggunakan**  
1️⃣ **Masukkan judul dan isi catatan**  
2️⃣ **Klik tombol "SIMPAN" untuk menambah catatan ke tabel**  
3️⃣ **Pilih catatan lalu klik "EDIT" untuk mengubahnya**  
4️⃣ **Pilih catatan lalu klik "HAPUS" untuk menghapusnya**  
5️⃣ **Klik "SIMPAN FILE" untuk menyimpan semua catatan ke `.txt`**  
6️⃣ **Klik "CETAK" untuk mencetak catatan ke printer**  

---

## **📌 Kode Utama (CatatanHarianForm.java)**
Berikut adalah **kode utama** dari aplikasi Catatan Harian yang digunakan di **NetBeans JFrame Form**:  

```java
import java.awt.event.*;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.print.PageFormat;
import java.awt.print.Printable;
import java.awt.print.PrinterException;
import java.awt.print.PrinterJob;
import java.io.File;
import java.io.FileWriter;
import java.util.ArrayList;

public class CatatanHarianForm extends javax.swing.JFrame {

    ublic CatatanHarianForm() {
        initComponents();
        setTitle("Catatan Harian");
    }

    @SuppressWarnings("unchecked")
    private void initComponents() {
        // Semua komponen GUI dibuat otomatis oleh NetBeans
    }

    private void btnSimpanActionPerformed(java.awt.event.ActionEvent evt) {                                          
        String judul = txtJudul.getText();
        String isi = txtIsi.getText();
        if (!judul.isEmpty() && !isi.isEmpty()) {
            DefaultTableModel model = (DefaultTableModel) tableCatatan.getModel();
            model.addRow(new Object[]{judul, isi});
            txtJudul.setText("");
            txtIsi.setText("");
            JOptionPane.showMessageDialog(this, "Catatan berhasil disimpan!");
        } else {
            JOptionPane.showMessageDialog(this, "Judul dan isi tidak boleh kosong!", "Peringatan", JOptionPane.WARNING_MESSAGE);
        }
    }

    private void btnEditActionPerformed(java.awt.event.ActionEvent evt) {                                         
        int selectedRow = tableCatatan.getSelectedRow();
    if (selectedRow != -1) {
        txtJudul.setText((String) tableCatatan.getValueAt(selectedRow, 0));
        txtIsi.setText((String) tableCatatan.getValueAt(selectedRow, 1));
        ((javax.swing.table.DefaultTableModel) tableCatatan.getModel()).removeRow(selectedRow);
    } else {
        javax.swing.JOptionPane.showMessageDialog(this, "Pilih catatan untuk diedit!", "Peringatan", javax.swing.JOptionPane.WARNING_MESSAGE);
    }
    }

    private void btnHapusActionPerformed(java.awt.event.ActionEvent evt) {                                         
      int selectedRow = tableCatatan.getSelectedRow();
    if (selectedRow != -1) {
        ((javax.swing.table.DefaultTableModel) tableCatatan.getModel()).removeRow(selectedRow);
        javax.swing.JOptionPane.showMessageDialog(this, "Catatan berhasil dihapus!");
    } else {
        javax.swing.JOptionPane.showMessageDialog(this, "Pilih catatan untuk dihapus!", "Peringatan", javax.swing.JOptionPane.WARNING_MESSAGE);
    }
    }

    private void btnSimpanKeFileActionPerformed(java.awt.event.ActionEvent evt) {                                                  
       try {
        javax.swing.JFileChooser fileChooser = new javax.swing.JFileChooser();
        if (fileChooser.showSaveDialog(this) == javax.swing.JFileChooser.APPROVE_OPTION) {
            java.io.File file = fileChooser.getSelectedFile();
            java.io.FileWriter writer = new java.io.FileWriter(file);
            javax.swing.table.DefaultTableModel model = (javax.swing.table.DefaultTableModel) tableCatatan.getModel();
            for (int i = 0; i < model.getRowCount(); i++) {
                writer.write("Judul: " + model.getValueAt(i, 0) + "\n");
                writer.write("Isi: " + model.getValueAt(i, 1) + "\n\n");
            }
            writer.close();
            javax.swing.JOptionPane.showMessageDialog(this, "Catatan berhasil disimpan ke file!");
        }
    } catch (Exception e) {
        javax.swing.JOptionPane.showMessageDialog(this, "Gagal menyimpan: " + e.getMessage(), "Error", javax.swing.JOptionPane.ERROR_MESSAGE);
    }
    }

    private void btnCetakActionPerformed(java.awt.event.ActionEvent evt) {                                          
       java.awt.print.PrinterJob job = java.awt.print.PrinterJob.getPrinterJob();
    job.setPrintable((java.awt.Graphics graphics, java.awt.print.PageFormat pageFormat, int pageIndex) -> {
        if (pageIndex > 0) return java.awt.print.Printable.NO_SUCH_PAGE;

        java.awt.Graphics2D g2d = (java.awt.Graphics2D) graphics;
        g2d.translate(pageFormat.getImageableX(), pageFormat.getImageableY());

        int y = 20;
        for (int i = 0; i < tableCatatan.getRowCount(); i++) {
            g2d.drawString("Judul: " + tableCatatan.getValueAt(i, 0), 10, y);
            y += 15;
            g2d.drawString("Isi: " + tableCatatan.getValueAt(i, 1), 10, y);
            y += 30;
        }
        return java.awt.print.Printable.PAGE_EXISTS;
    });

    if (job.printDialog()) {
        try {
            job.print();
        } catch (java.awt.print.PrinterException e) {
            javax.swing.JOptionPane.showMessageDialog(this, "Gagal mencetak: " + e.getMessage(), "Error", javax.swing.JOptionPane.ERROR_MESSAGE);
        }
    }
    }

    public static void main(String args[]) {
        java.awt.EventQueue.invokeLater(() -> new CatatanHarianForm().setVisible(true));
    }
}
```

---



---

## **📄 Lisensi**  

---

