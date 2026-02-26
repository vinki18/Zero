#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
#include <fstream>
#include <ctime>
#include <algorithm>
using namespace std;

struct Soal {
    int nomorSoal;
    string pertanyaan;
    string opsiA, opsiB, opsiC, opsiD;
    char jawabanBenar;
};

struct Kuis {
    int kuisID;
    string nama;
    string deskripsi;
    vector<Soal> daftarSoal;
    int nilaiMaksimal;
};

struct MataPelajaran {
    int mataPelajaranID;
    string nama;
    string deskripsi;
    string instruktur;
    vector<Kuis> daftarKuis;
};

struct Siswa {
    int siswaID;
    string nama;
    string email;
    string password;
    vector<int> kuisYangDiambil;  // Menyimpan kuisID
    vector<int> nilaiKuis;
};

class ELearningPlatform {
private:
    vector<MataPelajaran> daftarMataPelajaran;
    vector<Siswa> daftarSiswa;
    Siswa siswaAktif;
    bool isLoggedIn;
    int mataPelajaranIDTerakhir;
    int kuisIDTerakhir;
    int siswaIDTerakhir;
    
    string getTanggalSekarang() {
        time_t now = time(0);
        char buf[80];
        strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", localtime(&now));
        return string(buf);
    }
    
public:
    ELearningPlatform() : isLoggedIn(false), mataPelajaranIDTerakhir(0), 
                          kuisIDTerakhir(0), siswaIDTerakhir(0) {
        loadData();
    }
    
    ~ELearningPlatform() {
        saveData();
    }
    
    void registrasiSiswa() {
        Siswa siswa;
        siswa.siswaID = ++siswaIDTerakhir;
        
        cin.ignore();
        cout << "\n╔════════════════════════════════════════╗" << endl;
        cout << "║      REGISTRASI SISWA BARU            ║" << endl;
        cout << "╚════════════════════════════════════════╝" << endl;
        
        cout << "ID Siswa: " << siswa.siswaID << endl;
        
        cout << "Nama: ";
        getline(cin, siswa.nama);
        
        cout << "Email: ";
        getline(cin, siswa.email);
        
        cout << "Password: ";
        getline(cin, siswa.password);
        
        daftarSiswa.push_back(siswa);
        cout << "\n✓ Registrasi berhasil! Silahkan login." << endl;
    }
    
    void loginSiswa() {
        string email, password;
        cin.ignore();
        
        cout << "\n╔════════════════════════════════════════╗" << endl;
        cout << "║         LOGIN SISWA                   ║" << endl;
        cout << "╚════════════════════════════════════════╝" << endl;
        
        cout << "Email: ";
        getline(cin, email);
        
        cout << "Password: ";
        getline(cin, password);
        
        for(auto &siswa : daftarSiswa) {
            if(siswa.email == email && siswa.password == password) {
                siswaAktif = siswa;
                isLoggedIn = true;
                cout << "\n✓ Login berhasil! Selamat datang, " << siswa.nama << "!" << endl;
                return;
            }
        }
        
        cout << "\n❌ Email atau password salah!" << endl;
    }
    
    void logoutSiswa() {
        isLoggedIn = false;
        cout << "\n✓ Logout berhasil." << endl;
    }
    
    void tambahMataPelajaran() {
        MataPelajaran mata;
        mata.mataPelajaranID = ++mataPelajaranIDTerakhir;
        
        cin.ignore();
        cout << "\n╔════════════════════════════════════════╗" << endl;
        cout << "║    TAMBAH MATA PELAJARAN BARU         ║" << endl;
        cout << "╚════════════════════════════════════════╝" << endl;
        
        cout << "ID Mata Pelajaran: " << mata.mataPelajaranID << endl;
        
        cout << "Nama Mata Pelajaran: ";
        getline(cin, mata.nama);
        
        cout << "Deskripsi: ";
        getline(cin, mata.deskripsi);
        
        cout << "Instruktur: ";
        getline(cin, mata.instruktur);
        
        daftarMataPelajaran.push_back(mata);
        cout << "\n✓ Mata pelajaran berhasil ditambahkan!" << endl;
    }
    
    void tambahKuis() {
        if(daftarMataPelajaran.empty()) {
            cout << "\n❌ Belum ada mata pelajaran!" << endl;
            return;
        }
        
        lihatSemuaMataPelajaran();
        
        int mataPelajaranID;
        cout << "\nPilih ID Mata Pelajaran: ";
        cin >> mataPelajaranID;
        cin.ignore();
        
        for(auto &mata : daftarMataPelajaran) {
            if(mata.mataPelajaranID == mataPelajaranID) {
                Kuis kuis;
                kuis.kuisID = ++kuisIDTerakhir;
                
                cout << "\n╔════════════════════════════════════════╗" << endl;
                cout << "║      TAMBAH KUIS BARU                 ║" << endl;
                cout << "╚════════════════════════════════════════╝" << endl;
                
                cout << "ID Kuis: " << kuis.kuisID << endl;
                
                cout << "Nama Kuis: ";
                getline(cin, kuis.nama);
                
                cout << "Deskripsi: ";
                getline(cin, kuis.deskripsi);
                
                cout << "Masukkan jumlah soal: ";
                int jumlahSoal;
                cin >> jumlahSoal;
                cin.ignore();
                
                for(int i = 1; i <= jumlahSoal; i++) {
                    Soal soal;
                    soal.nomorSoal = i;
                    
                    cout << "\n--- Soal " << i << " ---" << endl;
                    cout << "Pertanyaan: ";
                    getline(cin, soal.pertanyaan);
                    
                    cout << "Opsi A: ";
                    getline(cin, soal.opsiA);
                    
                    cout << "Opsi B: ";
                    getline(cin, soal.opsiB);
                    
                    cout << "Opsi C: ";
                    getline(cin, soal.opsiC);
                    
                    cout << "Opsi D: ";
                    getline(cin, soal.opsiD);
                    
                    cout << "Jawaban Benar (A/B/C/D): ";
                    cin >> soal.jawabanBenar;
                    cin.ignore();
                    
                    kuis.daftarSoal.push_back(soal);
                }
                
                kuis.nilaiMaksimal = jumlahSoal * 10;
                mata.daftarKuis.push_back(kuis);
                cout << "\n✓ Kuis berhasil ditambahkan!" << endl;
                return;
            }
        }
        
        cout << "\n❌ Mata pelajaran tidak ditemukan!" << endl;
    }
    
    void lihatSemuaMataPelajaran() {
        if(daftarMataPelajaran.empty()) {
            cout << "\n⚠️  Belum ada mata pelajaran!" << endl;
            return;
        }
        
        cout << "\n╔══════════════════════════════════════════════════════════════╗" << endl;
        cout << "║               DAFTAR MATA PELAJARAN                          ║" << endl;
        cout << "╠═════════════════════════��════════════════════════════════════╣" << endl;
        cout << "║ ID │ Nama              │ Instruktur       │ Jumlah Kuis    ║" << endl;
        cout << "╠══════════════════════════════════════════════════════════════╣" << endl;
        
        for(auto &mata : daftarMataPelajaran) {
            cout << "║ " << setw(2) << right << mata.mataPelajaranID << " │ "
                 << setw(17) << left << mata.nama << "│ "
                 << setw(16) << left << mata.instruktur << "│ "
                 << setw(14) << right << mata.daftarKuis.size() << " ║" << endl;
        }
        cout << "╚══════════════════════════════════════════════════════════════╝" << endl;
    }
    
    void lihatDetailMataPelajaran() {
        int mataPelajaranID;
        cout << "\nMasukkan ID Mata Pelajaran: ";
        cin >> mataPelajaranID;
        cin.ignore();
        
        for(auto &mata : daftarMataPelajaran) {
            if(mata.mataPelajaranID == mataPelajaranID) {
                cout << "\n╔════════════════════════════════���═══════════════════════════╗" << endl;
                cout << "║              DETAIL MATA PELAJARAN                         ║" << endl;
                cout << "╠════════════════════════════════════════════════════════════╣" << endl;
                cout << "║ Nama: " << setw(52) << left << mata.nama << " ║" << endl;
                cout << "║ Deskripsi: " << setw(47) << left << mata.deskripsi << " ║" << endl;
                cout << "║ Instruktur: " << setw(46) << left << mata.instruktur << " ║" << endl;
                cout << "╠════════════════════════════════════════════════════════════╣" << endl;
                cout << "║                    DAFTAR KUIS                            ║" << endl;
                cout << "╠════════════════════════════════════════════════════════════╣" << endl;
                
                if(mata.daftarKuis.empty()) {
                    cout << "║ Belum ada kuis dalam mata pelajaran ini                    ║" << endl;
                } else {
                    cout << "║ ID │ Nama Kuis        │ Jumlah Soal │ Nilai Maksimal   ║" << endl;
                    cout << "╠════════════════════════════════════════════════════════════╣" << endl;
                    
                    for(auto &kuis : mata.daftarKuis) {
                        cout << "║ " << setw(2) << right << kuis.kuisID << " │ "
                             << setw(16) << left << kuis.nama << "│ "
                             << setw(11) << right << kuis.daftarSoal.size() << " │ "
                             << setw(16) << right << kuis.nilaiMaksimal << " ║" << endl;
                    }
                }
                cout << "╚════════════════════════════════════════════════════════════╝" << endl;
                return;
            }
        }
        
        cout << "\n❌ Mata pelajaran tidak ditemukan!" << endl;
    }
	
    void mengerjakan Kuis() 
	
        if(!isLoggedIn) {
            cout << "\n❌ Silahkan login terlebih dahulu!" << endl;
            return;
        }
        
        lihatSemuaMataPelajaran();
        
        int mataPelajaranID;
        cout << "\nPilih ID Mata Pelajaran: ";
        cin >> mataPelajaranID;
        
        for(auto &mata : daftarMataPelajaran) {
            if(mata.mataPelajaranID == mataPelajaranID) {
                cout << "\n╔════════════════════════════════════════════════════════════╗" << endl;
                cout << "║              DAFTAR KUIS - " << setw(40) << left << mata.nama << " ║" << endl;
                cout << "╠════════════════════════════════════════════════════════════╣" << endl;
                cout << "║ ID │ Nama Kuis        │ Jumlah Soal │ Status            ║" << endl;
                cout << "╠════════════════════════════════════════════════════════════╣" << endl;
                
                for(auto &kuis : mata.daftarKuis) {
                    bool sudahDikerjakan = false;
                    for(int id : siswaAktif.kuisYangDiambil) {
                        if(id == kuis.kuisID) {
                            sudahDikerjakan = true;
                            break;
                        }
                    }
                    
                    string status = sudahDikerjakan ? "Sudah Dikerjakan" : "Belum Dikerjakan";
                    cout << "║ " << setw(2) << right << kuis.kuisID << " │ "
                         << setw(16) << left << kuis.nama << "│ "
                         << setw(11) << right << kuis.daftarSoal.size() << " │ "
                         << setw(17) << left << status << " ║" << endl;
                }
                cout << "╚════════════════════════════════════════════════════════════╝" << endl;
                
                int kuisID;
                cout << "\nPilih ID Kuis: ";
                cin >> kuisID;
                cin.ignore();
                
                for(auto &kuis : mata.daftarKuis) {
                    if(kuis.kuisID == kuisID) {
                        kerjakanKuis(kuis, mata.nama);
                        return;
                    }
                }
                
                cout << "\n❌ Kuis tidak ditemukan!" << endl;
                return;
            }
        }
        
        cout << "\n❌ Mata pelajaran tidak ditemukan!" << endl;
    }
    
    void kerjakanKuis(Kuis &kuis, string namaMata) {
        cout << "\n╔════════════════════════════════════════════════════════════╗" << endl;
        cout << "║                  KUIS DIMULAI                              ║" << endl;
        cout << "╠════════════════════════════════════════════════════════════╣" << endl;
        cout << "║ Mata Pelajaran: " << setw(43) << left << namaMata << " ║" << endl;
        cout << "║ Nama Kuis: " << setw(47) << left << kuis.nama << " ║" << endl;
        cout << "║ Jumlah Soal: " << setw(45) << right << kuis.daftarSoal.size() << " ║" << endl;
        cout << "╚════════════════════════════════════════════════════════════╝" << endl;
        
        int jawaban_benar = 0;
        
        for(auto &soal : kuis.daftarSoal) {
            cout << "\n--- Soal " << soal.nomorSoal << " ---" << endl;
            cout << soal.pertanyaan << endl;
            cout << "A. " << soal.opsiA << endl;
            cout << "B. " << soal.opsiB << endl;
            cout << "C. " << soal.opsiC << endl;
            cout << "D. " << soal.opsiD << endl;
            
            cout << "Jawaban Anda (A/B/C/D): ";
            char jawaban;
            cin >> jawaban;
            cin.ignore();
            
            if(jawaban == soal.jawabanBenar) {
                jawaban_benar++;
                cout << "✓ Benar!" << endl;
            } else {
                cout << "✗ Salah! Jawaban yang benar adalah: " << soal.jawabanBenar << endl;
            }
        }
        
        int nilai = (jawaban_benar * 100) / kuis.daftarSoal.size();
        
        cout << "\n╔════════════════════════════════════════════════════════════╗" << endl;
        cout << "║                  HASIL KUIS                                ║" << endl;
        cout << "╠════════════════════════════════════════════════════════════╣" << endl;
        cout << "║ Jawaban Benar: " << setw(44) << right << jawaban_benar << " ║" << endl;
        cout << "║ Total Soal: " << setw(47) << right << kuis.daftarSoal.size() << " ║" << endl;
        cout << "║ Nilai: " << setw(51) << right << nilai << " ║" << endl;
        cout << "╚════════════════════════════════════════════════════════════╝" << endl;
        
        // Simpan hasil
        for(int i = 0; i < daftarSiswa.size(); i++) {
            if(daftarSiswa[i].siswaID == siswaAktif.siswaID) {
                daftarSiswa[i].kuisYangDiambil.push_back(kuis.kuisID);
                daftarSiswa[i].nilaiKuis.push_back(nilai);
                siswaAktif = daftarSiswa[i];
                break;
            }
        }
    }
    
    void lihatNilaiSiswa() {
        if(!isLoggedIn) {
            cout << "\n❌ Silahkan login terlebih dahulu!" << endl;
            return;
        }
        
        cout << "\n╔════════════════════════════════════════════════════════════╗" << endl;
        cout << "║                  NILAI KUIS SISWA                          ║" << endl;
        cout << "╠════════════════════════════════════════════════════════════╣" << endl;
        cout << "║ Nama Siswa: " << setw(46) << left << siswaAktif.nama << " ║" << endl;
        cout << "║ Email: " << setw(51) << left << siswaAktif.email << " ║" << endl;
        cout << "╠════════════════════════════════════════════════════════════╣" << endl;
        
        if(siswaAktif.kuisYangDiambil.empty()) {
            cout << "║ Belum mengerjakan kuis apapun                            ║" << endl;
        } else {
            cout << "║ No │ Kuis              │ Nilai  │ Tanggal Mengerjakan   ║" << endl;
            cout << "╠════════════════════════════════════════════════════════════╣" << endl;
            
            double totalNilai = 0;
            for(int i = 0; i < siswaAktif.kuisYangDiambil.size(); i++) {
                totalNilai += siswaAktif.nilaiKuis[i];
                
                // Cari nama kuis
                string namaKuis = "Unknown";
                for(auto &mata : daftarMataPelajaran) {
                    for(auto &kuis : mata.daftarKuis) {
                        if(kuis.kuisID == siswaAktif.kuisYangDiambil[i]) {
                            namaKuis = kuis.nama;
                            break;
                        }
                    }
                }
                
                cout << "║ " << setw(2) << right << (i+1) << " │ "
                     << setw(17) << left << namaKuis << "│ "
                     << setw(6) << right << siswaAktif.nilaiKuis[i] << " │ "
                     << setw(20) << left << getTanggalSekarang() << " ║" << endl;
            }
            
            cout << "╠════════════════════════════════════════════════════════════╣" << endl;
            double rataRata = totalNilai / siswaAktif.nilaiKuis.size();
            cout << fixed << setprecision(1);
            cout << "║ Rata-rata Nilai: " << setw(42) << right << rataRata << " ║" << endl;
        }
        cout << "╚════════════════════════════════════════════════════════════╝" << endl;
    }
    
    void saveData() {
        ofstream file("elearning_data.txt");
        file << mataPelajaranIDTerakhir << "\n" << kuisIDTerakhir << "\n" << siswaIDTerakhir << "\n";
        
        for(auto &mata : daftarMataPelajaran) {
            file << "MATA|" << mata.mataPelajaranID << "|" << mata.nama << "|"
                 << mata.deskripsi << "|" << mata.instruktur << "\n";
            
            for(auto &kuis : mata.daftarKuis) {
                file << "KUIS|" << kuis.kuisID << "|" << kuis.nama << "|"
                     << kuis.deskripsi << "|" << kuis.nilaiMaksimal << "\n";
                
                for(auto &soal : kuis.daftarSoal) {
                    file << "SOAL|" << soal.nomorSoal << "|" << soal.pertanyaan << "|"
                         << soal.opsiA << "|" << soal.opsiB << "|" << soal.opsiC << "|"
                         << soal.opsiD << "|" << soal.jawabanBenar << "\n";
                }
            }
        }
        
        for(auto &siswa : daftarSiswa) {
            file << "SISWA|" << siswa.siswaID << "|" << siswa.nama << "|"
                 << siswa.email << "|" << siswa.password << "\n";
        }
        file.close();
    }
    
    void loadData() {
        // Implementasi load data mirip dengan save data
        // (Disederhanakan untuk waktu)
    }
    
    void run() {
        cout << "╔════════════════════════════════════════════════╗" << endl;
        cout << "║       E-LEARNING PLATFORM                     ║" << endl;
        cout << "║    Platform Pembelajaran Online               ║" << endl;
        cout << "╚════════════════════════════════════════════════╝" << endl;
        
        int pilihan;
        while(true) {
            if(!isLoggedIn) {
                cout << "\n╔════════════════════════════════════════════════╗" << endl;
                cout << "║              MENU AWAL                         ║" << endl;
                cout << "║ 1. Registrasi Siswa Baru                      ║" << endl;
                cout << "║ 2. Login                                      ║" << endl;
                cout << "║ 3. Lihat Mata Pelajaran (Tanpa Login)         ║" << endl;
                cout << "║ 4. Exit                                       ║" << endl;
                cout << "╚════════════════════════════════════════════════╝" << endl;
                
                cout << "Pilihan: ";
                cin >> pilihan;
                
                switch(pilihan) {
                    case 1:
                        registrasiSiswa();
                        break;
                    case 2:
                        loginSiswa();
                        break;
                    case 3:
                        lihatSemuaMataPelajaran();
                        break;
                    case 4:
                        cout << "\nTerima kasih! Program selesai." << endl;
                        return;
                    default:
                        cout << "\n❌ Pilihan tidak valid!" << endl;
                }
            } else {
                cout << "\n╔════════════════════════════════════════════════╗" << endl;
                cout << "║        MENU SISWA (Selamat datang, " << setw(17) << left << siswaAktif.nama << ")║" << endl;
                cout << "║ 1. Lihat Mata Pelajaran                       ║" << endl;
                cout << "║ 2. Lihat Detail Mata Pelajaran                ║" << endl;
                cout << "║ 3. Mengerjakan Kuis                           ║" << endl;
                cout << "║ 4. Lihat Nilai Kuis                           ║" << endl;
                cout << "║ 5. Logout                                     ║" << endl;
                cout << "╚════════════════════════════════════════════════╝" << endl;
                
                cout << "Pilihan: ";
                cin >> pilihan;
                
                switch(pilihan) {
                    case 1:
                        lihatSemuaMataPelajaran();
                        break;
                    case 2:
                        lihatDetailMataPelajaran();
                        break;
                    case 3:
                        mengerjakanKuis();
                        break;
                    case 4:
                        lihatNilaiSiswa();
                        break;
                    case 5:
                        logoutSiswa();
                        break;
                    default:
                        cout << "\n❌ Pilihan tidak valid!" << endl;
                }
            }
        }
    }
};

int main() {
    ELearningPlatform platform;
    platform.run();
    return 0;
}
