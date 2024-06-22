#include <iostream>
#include <vector>
#include <string>
#include <iomanip> // untuk format teks
#include <cstdlib> // untuk clear screen

using namespace std;

class ItemMenu {
public:
    string nama;
    double harga;

    ItemMenu(string n, double h) : nama(n), harga(h) {}
};

class Pesanan {
public:
    vector<ItemMenu> items;
    double total;

    Pesanan() : total(0.0) {}

    void tambahItem(const ItemMenu& item) {
        items.push_back(item);
        total += item.harga;
    }

    void tampilkanPesanan() const {
        cout << "\nDaftar Pesanan:" << "\n";
        for (size_t i = 0; i < items.size(); ++i) {
            cout << i + 1 << ". " << items[i].nama << " - Rp " << fixed << setprecision(2) << items[i].harga << "\n";
        }
        cout << "Total: Rp " << fixed << setprecision(2) << total << "\n";
    }
};

class Restoran {
private:
    vector<ItemMenu> menu;
    vector<Pesanan> daftarPesanan;

public:
    Restoran() {
        // Tambahkan beberapa item ke menu
        menu.push_back(ItemMenu("Nasi Goreng", 25000));
        menu.push_back(ItemMenu("Mie Goreng", 22000));
        menu.push_back(ItemMenu("Ayam Bakar", 30000));
        menu.push_back(ItemMenu("Es Teh Manis", 5000));
    }

    void tampilkanMenu() const {
        cout << "\nMenu Restoran:" << "\n";
        cout << "-----------------------------------------\n";
        cout << left << setw(20) << "Nama Item" << "Harga\n";
        cout << "-----------------------------------------\n";
        for (size_t i = 0; i < menu.size(); ++i) {
            cout << i + 1 << ". " << left << setw(20) << menu[i].nama << "Rp " << fixed << setprecision(2) << menu[i].harga << "\n";
        }
        cout << "-----------------------------------------\n";
    }

    void buatPesanan() {
        Pesanan pesanan;
        int pilihan;

        cout << "\nMasukkan nomor item dari menu untuk memesan (0 untuk selesai):" << "\n";
        tampilkanMenu();
        
        while (true) {
            cout << "Pilih item (0 untuk selesai): ";
            cin >> pilihan;
            if (pilihan == 0) break;
            if (pilihan < 1 || pilihan > menu.size()) {
                cout << "Pilihan tidak valid, coba lagi.\n";
                continue;
            }
            pesanan.tambahItem(menu[pilihan - 1]);
            cout << "Item ditambahkan ke pesanan.\n";
        }

        if (!pesanan.items.empty()) {
            daftarPesanan.push_back(pesanan);
            cout << "Pesanan berhasil dibuat.\n";
        } else {
            cout << "Pesanan kosong, tidak ada yang ditambahkan.\n";
        }
    }

    void tampilkanPesanan() const {
        if (daftarPesanan.empty()) {
            cout << "\nTidak ada pesanan.\n";
            return;
        }

        cout << "\nDaftar Semua Pesanan:" << "\n";
        for (size_t i = 0; i < daftarPesanan.size(); ++i) {
            cout << "\nPesanan " << i + 1 << ":"<< "\n";
            daftarPesanan[i].tampilkanPesanan();
            cout << "-------------------\n";
        }
    }
};

void clearScreen() {
    // Bersihkan layar
    #ifdef _WIN32
        system("cls");
    #else
        system("clear");
    #endif
}

void tampilkanHeader() {
    clearScreen();
    cout << "========================================="<< "\n";
    cout << "     Aplikasi Manajemen Pesanan Restoran   "<< "\n";
    cout << "========================================="<< "\n";
}

int main() {
    Restoran restoran;
    int pilihan;

    while (true) {
        tampilkanHeader();
        cout << "\nMenu Utama\n";
        cout << "1. Tampilkan Menu\n";
        cout << "2. Buat Pesanan\n";
        cout << "3. Tampilkan Semua Pesanan\n";
        cout << "4. Keluar\n";
        cout << "Pilih opsi: ";
        cin >> pilihan;

        switch (pilihan) {
            case 1:
                clearScreen();
                restoran.tampilkanMenu();
                break;
            case 2:
                clearScreen();
                restoran.buatPesanan();
                break;
            case 3:
                clearScreen();
                restoran.tampilkanPesanan();
                break;
            case 4:
                cout << "Terima kasih telah menggunakan aplikasi ini.\n";
                return 0;
            default:
                cout << "Pilihan tidak valid, coba lagi.\n";
        }

        cout << "Tekan Enter untuk melanjutkan...";
        cin.ignore();
        cin.get();
    }
}
