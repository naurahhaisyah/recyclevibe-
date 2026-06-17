import tkinter as tk
from tkinter import ttk, messagebox

data_sampah = [
    {"jenis": "Plastik", "jumlah": 150, "metode": "Peleburan bijih plastik", "tanggal": "2026-06-01", "status": "Sudah Daur Ulang"},
    {"jenis": "Kertas", "jumlah": 80, "metode": "Bubur kertas (Pulping)", "tanggal": "2026-06-03", "status": "Belum Daur Ulang"},
    {"jenis": "Logam", "jumlah": 200, "metode": "Peleburan suhu tinggi", "tanggal": "2026-06-02", "status": "Sudah Daur Ulang"},
    {"jenis": "Kaca", "jumlah": 50, "metode": "Dihancurkan dan dicetak ulang", "tanggal": "2026-05-30", "status": "Belum Daur Ulang"},
]

THEMES = {
    "🌱 Earth": {"bg": "#F4F7F5", "fg": "#2C3E35", "card": "#FFFFFF"},
    "⚡ Cyber": {"bg": "#1A1A2E", "fg": "#00FFFF", "card": "#161625"},
    "🎀 Barbie": {"bg": "#FFF0F5", "fg": "#4A0E2E", "card": "#FFFFFF"}
}

class AplikasiSampah:
    def __init__(self, root):
        self.root = root
        self.root.title("RecycleVibe ♻️")
        self.root.geometry("1050x550")
        
        self.frame_atas = tk.Frame(root, pady=5)
        self.frame_atas.pack(fill="x", padx=15)
        
        tk.Label(self.frame_atas, text="Vibes:").pack(side="left", padx=5)
        self.cbo_theme = ttk.Combobox(self.frame_atas, values=list(THEMES.keys()), state="readonly", width=10)
        self.cbo_theme.current(0)
        self.cbo_theme.pack(side="left", padx=5)
        self.cbo_theme.bind("<<ComboboxSelected>>", self.ganti_tema)
        
        self.frame_input = tk.LabelFrame(root, text=" Form Input ", padx=15, pady=10)
        self.frame_input.pack(side="left", fill="y", padx=15, pady=10)
        
        self.entries = {}
        for label_text, key in [("Jenis Sampah:", "jenis"), ("Jumlah (kg):", "jumlah"), ("Metode:", "metode"), ("Tanggal:", "tanggal")]:
            tk.Label(self.frame_input, text=label_text).pack(anchor="w")
            self.entries[key] = tk.Entry(self.frame_input, width=22)
            self.entries[key].pack(pady=3)
        self.entries["tanggal"].insert(0, "2026-06-16")
        
        tk.Label(self.frame_input, text="Status:").pack(anchor="w")
        self.cbo_status = ttk.Combobox(self.frame_input, values=["Sudah Daur Ulang", "Belum Daur Ulang"], state="readonly", width=19)
        self.cbo_status.current(1)
        self.cbo_status.pack(pady=3)
        
        tk.Button(self.frame_input, text="Tambah", bg="#A3B19B", command=self.tambah).pack(fill="x", pady=5)
        tk.Button(self.frame_input, text="Ubah", bg="#7D9D9C", command=self.ubah).pack(fill="x", pady=3)
        tk.Button(self.frame_input, text="Hapus", bg="#E07A5F", command=self.hapus).pack(fill="x", pady=3)

        self.frame_kanan = tk.Frame(root)
        self.frame_kanan.pack(side="left", fill="both", expand=True, padx=(0, 15), pady=10)
        
        self.frame_tools = tk.Frame(self.frame_kanan)
        self.frame_tools.pack(fill="x", pady=(0, 5))
        
        self.ent_cari = tk.Entry(self.frame_tools, width=15)
        self.ent_cari.pack(side="left", padx=2)
        tk.Button(self.frame_tools, text="Cari", command=self.cari_data).pack(side="left", padx=2)
        
        self.cbo_sort = ttk.Combobox(self.frame_tools, values=["Jumlah", "Jenis", "Tanggal"], state="readonly", width=12)
        self.cbo_sort.current(0)
        self.cbo_sort.pack(side="left", padx=10)
        tk.Button(self.frame_tools, text="Sort", command=self.sort_data).pack(side="left", padx=2)
        
        columns = ("tanggal", "jenis", "jumlah", "metode", "status")
        self.tree = ttk.Treeview(self.frame_kanan, columns=columns, show="headings", height=13)
        for col in columns:
            self.tree.heading(col, text=col.capitalize())
            self.tree.column(col, width=110, anchor="center")
        self.tree.pack(fill="both", expand=True, side="left")
        
        scrollbar = ttk.Scrollbar(self.frame_kanan, orient="vertical", command=self.tree.yview)
        self.tree.configure(yscrollcommand=scrollbar.set)
        scrollbar.pack(fill="y", side="right")
        
        self.tree.bind("<<TreeviewSelect>>", self.ambil_data_klik)
        
        self.lbl_stat = tk.Label(root, text="", font=("Arial", 10, "bold"), bd=1, relief="sunken", anchor="w", padx=15)
        self.lbl_stat.pack(side="bottom", fill="x", padx=15, pady=10)
        
        self.refresh_tabel()
        self.ganti_tema()

    def refresh_tabel(self, sumber_data=None):
        for row in self.tree.get_children():
            self.tree.delete(row)
            
        target = sumber_data if sumber_data is not None else data_sampah
        for item in target:
            self.tree.insert("", "end", values=(item["tanggal"], item["jenis"], item["jumlah"], item["metode"], item["status"]))
            
        total = sum(i["jumlah"] for i in data_sampah)
        sudah = sum(i["jumlah"] for i in data_sampah if i["status"] == "Sudah Daur Ulang")
        self.lbl_stat.config(text=(
            f"Total Sampah: {total} kg\n"
            f"Sudah Daur Ulang: {sudah} kg\n"
            f"Belum Daur Ulang: {total - sudah} kg"
        ))

    def ganti_tema(self, event=None):
        t = THEMES[self.cbo_theme.get()]
        self.root.config(bg=t["bg"])
        for f in [self.frame_atas, self.frame_kanan, self.frame_tools]: f.config(bg=t["bg"])
        self.frame_input.config(bg=t["card"], fg=t["fg"])
        self.lbl_stat.config(bg=t["card"], fg=t["fg"])
        for w in self.frame_input.winfo_children():
            if isinstance(w, tk.Label): w.config(bg=t["card"], fg=t["fg"])

    def cari_data(self):
        kunci = self.ent_cari.get().strip().lower()
        if not kunci:
            self.refresh_tabel()
            return
            
        hasil_pencarian = []
        for item in data_sampah:
            if kunci in item["jenis"].lower():
                hasil_pencarian.append(item)
                
        self.refresh_tabel(sumber_data=hasil_pencarian)

    def sort_data(self):
        pilihan = self.cbo_sort.get()
        n = len(data_sampah)
    
        if pilihan == "Jumlah":
            for i in range(1, n):
                key_item = data_sampah[i]
                j = i - 1
                while j >= 0 and data_sampah[j]["jumlah"] > key_item["jumlah"]:
                    data_sampah[j + 1] = data_sampah[j]
                    j -= 1
                data_sampah[j + 1] = key_item

        elif pilihan == "Jenis":
            for i in range(n):
                min_idx = i
                for j in range(i + 1, n):
                    if data_sampah[j]["jenis"].lower() < data_sampah[min_idx]["jenis"].lower():
                        min_idx = j
                data_sampah[i], data_sampah[min_idx] = data_sampah[min_idx], data_sampah[i]
            
        elif pilihan == "Tanggal":
            for i in range(1, n):
                key_item = data_sampah[i]
                j = i - 1
                while j >= 0 and data_sampah[j]["tanggal"] < key_item["tanggal"]:
                    data_sampah[j + 1] = data_sampah[j]
                    j -= 1
                data_sampah[j + 1] = key_item

        self.refresh_tabel()

    def tambah(self):
        try:
            data_sampah.append({
                "jenis": self.entries["jenis"].get(), "jumlah": int(self.entries["jumlah"].get()),
                "metode": self.entries["metode"].get(), "tanggal": self.entries["tanggal"].get(), "status": self.cbo_status.get()
            })
            self.refresh_tabel()
        except ValueError:
            messagebox.showerror("Error", "Jumlah wajib angka!")

    def ambil_data_klik(self, event):
        selected = self.tree.focus()
        if selected:
            val = self.tree.item(selected, "values")
            for i, key in enumerate(["tanggal", "jenis", "jumlah", "metode"]):
                self.entries[key].delete(0, tk.END)
                self.entries[key].insert(0, val[i])
            self.cbo_status.set(val[4])

    def ubah(self):
        selected = self.tree.focus()
        if not selected: return
        val = self.tree.item(selected, "values")
        for item in data_sampah:
            if item["tanggal"] == val[0] and item["jenis"] == val[1]:
                try:
                    item.update({
                        "jenis": self.entries["jenis"].get(), "jumlah": int(self.entries["jumlah"].get()),
                        "metode": self.entries["metode"].get(), "tanggal": self.entries["tanggal"].get(), "status": self.cbo_status.get()
                    })
                    self.refresh_tabel()
                    break
                except ValueError:
                    messagebox.showerror("Error", "Jumlah wajib angka!")

    def hapus(self):
        selected = self.tree.focus()
        if not selected: return
        val = self.tree.item(selected, "values")
        global data_sampah
        data_sampah = [i for i in data_sampah if not (i["tanggal"] == val[0] and i["jenis"] == val[1])]
        self.refresh_tabel()

if __name__ == "__main__":
    root = tk.Tk()
    app = AplikasiSampah(root)
    root.mainloop()
