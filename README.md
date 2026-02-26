#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

struct Kontak {
    string nama;
    string noTelepon;
    string email;
    string alamat;
};

int main() {
    vector<Kontak> daftarKontak;
    int pilihan;
    
    while(true) {
        cout << "\n=== Manajemen Kontak Telepon ===" << endl;
        cout << "1. Tambah Kontak" << endl;
        cout << "2. Lihat Semua Kontak" << endl;
        cout << "3. Cari Kontak" << endl;
        cout << "4. Edit Kontak" << endl;
        cout << "5. Hapus Kontak" << endl;
        cout << "6. Urutkan Kontak Berdasarkan Nama" << endl;
        cout << "7. Keluar" << endl;
        cout << "Pilihan: ";
        cin >> pilihan;
        cin.ignore();
        
        if(pilihan == 1) {
            Kontak kontak;
            cout << "Nama: ";
            getline(cin, kontak.nama);
            cout << "No. Telepon: ";
            getline(cin, kontak.noTelepon);
            cout << "Email: ";
            getline(cin, kontak.email);
            cout << "Alamat: ";
            getline(cin, kontak.alamat);
            daftarKontak.push_back(kontak);
            cout << "Kontak berhasil ditambahkan!" << endl;
        }
        else if(pilihan == 2) {
            if(daftarKontak.empty()) {
                cout << "Belum ada kontak!" << endl;
            } else {
                cout << "\n--- Daftar Kontak ---" << endl;
                for(int i = 0; i < daftarKontak.size(); i++) {
                    cout << (i+1) << ". " << daftarKontak[i].nama << endl;
                    cout << "   No. Telepon: " << daftarKontak[i].noTelepon << endl;
                    cout << "   Email: " << daftarKontak[i].email << endl;
                    cout << "   Alamat: " << daftarKontak[i].alamat << endl;
                    cout << "---" << endl;
                }
            }
        }
        else if(pilihan == 3) {
            string namaCari;
            cout << "Masukkan nama yang dicari: ";
            getline(cin, namaCari);
            bool found = false;
            for(int i = 0; i < daftarKontak.size(); i++) {
                if(daftarKontak[i].nama == namaCari) {
                    cout << "\nKontak Ditemukan!" << endl;
                    cout << "Nama: " << daftarKontak[i].nama << endl;
                    cout << "No. Telepon: " << daftarKontak[i].noTelepon << endl;
                    cout << "Email: " << daftarKontak[i].email << endl;
                    cout << "Alamat: " << daftarKontak[i].alamat << endl;
                    found = true;
                    break;
                }
            }
            if(!found) cout << "Kontak tidak ditemukan!" << endl;
        }
        else if(pilihan == 4) {
            string namaCari;
            cout << "Masukkan nama kontak yang akan diedit: ";
            getline(cin, namaCari);
            for(int i = 0; i < daftarKontak.size(); i++) {
                if(daftarKontak[i].nama == namaCari) {
                    cout << "No. Telepon Baru: ";
                    getline(cin, daftarKontak[i].noTelepon);
                    cout << "Email Baru: ";
                    getline(cin, daftarKontak[i].email);
                    cout << "Alamat Baru: ";
                    getline(cin, daftarKontak[i].alamat);
                    cout << "Kontak berhasil diupdate!" << endl;
                    break;
                }
            }
        }
        else if(pilihan == 5) {
            string namaCari;
            cout << "Masukkan nama kontak yang akan dihapus: ";
            getline(cin, namaCari);
            for(int i = 0; i < daftarKontak.size(); i++) {
                if(daftarKontak[i].nama == namaCari) {
                    daftarKontak.erase(daftarKontak.begin() + i);
                    cout << "Kontak berhasil dihapus!" << endl;
                    break;
                }
            }
        }
        else if(pilihan == 6) {
            sort(daftarKontak.begin(), daftarKontak.end(), 
                [](const Kontak &a, const Kontak &b) {
                    return a.nama < b.nama;
                });
            cout << "Kontak berhasil diurutkan!" << endl;
        }
        else if(pilihan == 7) {
            break;
        }
    }
    
    return 0;
}
