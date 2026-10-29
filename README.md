/bang bam dung danh sch lien ket
#include <iostream>
using namespace std;

struct Node {
    int key;
    int value;
    Node *next = nullptr;
};

struct hashtable {
    Node *table[10] = {nullptr};

    int hashFunction(int key) {
        return key % 10;
    }

    void insert(int key, int value) {
        int idx = hashFunction(key);
        for (Node *p = table[idx]; p != nullptr; p = p->next) {
            if (p->key == key) {
                p->value = value;
                return;
            }
        }
        Node *q = new Node();
        q->key = key;
        q->value = value;
        q->next = table[idx];
        table[idx] = q;
    }

    int find(int key) {
        int idx = hashFunction(key); 
        for (Node *p = table[idx]; p != nullptr; p = p->next) {
            if (p->key == key) {
                return p->value;
            }
        }
        return -1; 
    }

    void remove(int key) {
        int idx = hashFunction(key);
        Node *p = table[idx];
        Node *pre = nullptr;

        while (p != nullptr && p->key != key) {
            pre = p;
            p = p->next;
        }

        if (p == nullptr) {
            return; // Không tìm thấy key để xóa
        }

        if (pre == nullptr) {
            table[idx] = p->next; 
        } else {
            pre->next = p->next;  
        }
        delete p; 
    }
};

int main() {
    hashtable ht;
    
    // Thêm dữ liệu
    ht.insert(5, 100);
    ht.insert(15, 200); 
    ht.insert(25, 300);

    // Tìm kiếm
    cout << "Tim key 5, gia tri: " << ht.find(5) << endl;
    cout << "Tim key 15, gia tri: " << ht.find(15) << endl;
    cout << "Tim key 99 (khong ton tai): " << ht.find(99) << endl;

    // Xóa phần tử
    ht.remove(5);
    cout << "Sau khi xoa key 5, tim lai key 5: " << ht.find(5) << endl;
    cout << "Kiem tra lai key 15 van con: " << ht.find(15) << endl;

    return 0;
}
