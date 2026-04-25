import streamlit as st
import pandas as pd

# Konfigurasi Halaman
st.set_page_config(page_title="Sistem Penilaian Mahasiswa", layout="wide")

# --- 1. INISIALISASI DATABASE (Session State) ---
# Menggunakan session_state agar data tersimpan selama aplikasi berjalan
if 'db_nilai' not in st.session_state:
    st.session_state.db_nilai = []

if 'is_admin' not in st.session_state:
    st.session_state.is_admin = False

# --- 2. SIDEBAR NAVIGASI ---
st.sidebar.title("Menu Navigasi")
pilihan = st.sidebar.radio("Pilih Halaman:", ["Form Input (User)", "Dashboard Admin"])

# Tombol Logout jika sudah login admin
if st.session_state.is_admin:
    if st.sidebar.button("Log Out Admin"):
        st.session_state.is_admin = False
        st.rerun()

# --- 3. HALAMAN FORM INPUT (BISA DI AKSES SEMUA ORANG) ---
if pilihan == "Form Input (User)":
    st.title("📝 Form Input Nilai Mahasiswa")
    st.info("Silakan masukkan data Anda di bawah ini. Anda tidak perlu login untuk mengisi form.")

    with st.form("public_form", clear_on_submit=True):
        nama = st.text_input("Nama Lengkap")
        skor = st.number_input("Nilai/Skor (0-100)", min_value=0, max_value=100)
        submit = st.form_submit_button("Kirim Data")

        if submit:
            if nama:
                # --- RULE-BASED SYSTEM (Logika IF-THEN) ---
                if skor >= 85:
                    grade, ket = "A", "Lulus (Sangat Memuaskan)"
                elif skor >= 75:
                    grade, ket = "B", "Lulus (Memuaskan)"
                elif skor >= 60:
                    grade, ket = "C", "Lulus (Cukup)"
                elif skor >= 45:
                    grade, ket = "D", "Tidak Lulus (Kurang)"
                else:
                    grade, ket = "E", "Gagal"
                
                # Simpan ke Database
                st.session_state.db_nilai.append({
                    "Nama": nama,
                    "Nilai": skor,
                    "Grade": grade,
                    "Keterangan": ket
                })
                st.success(f"Terima kasih {nama}, data Anda telah berhasil dikirim!")
            else:
                st.error("Nama harus diisi!")

# --- 4. HALAMAN DASHBOARD ADMIN (HARUS LOGIN) ---
elif pilihan == "Dashboard Admin":
    st.title("🔐 Panel Kontrol Admin")

    if not st.session_state.is_admin:
        # Tampilkan Form Login jika belum login
        with st.form("login_admin"):
            st.subheader("Login Khusus Admin")
            username = st.text_input("Username")
            password = st.text_input("Password", type="password")
            btn_login = st.form_submit_button("Login")

            if btn_login:
                if username == "admin" and password == "123":
                    st.session_state.is_admin = True
                    st.success("Login Berhasil!")
                    st.rerun()
                else:
                    st.error("Username atau Password salah!")
    else:
        # Jika sudah login, tampilkan data CRUD
        st.subheader("📊 Data Nilai Masuk (CRUD)")
        
        if st.session_state.db_nilai:
            df = pd.DataFrame(st.session_state.db_nilai)
            st.dataframe(df, use_container_width=True)
            
            # Fitur Delete (CRUD - Delete)
            if st.button("Hapus Semua Data"):
                st.session_state.db_nilai = []
                st.rerun()
        else:
            st.warning("Belum ada data mahasiswa yang masuk.")
