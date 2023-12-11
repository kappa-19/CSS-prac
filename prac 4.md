// Bhavesh Balwani A-46 CSE
```c++
#include <bits/stdc++.h>
#include <vector>
class DES {
public:
std::vector<int> key = {1, 0, 1, 0, 0, 0, 0, 0, 1, 0}; // 10-bit key
std::vector<int> key1;
std::vector<int> key2;
std::vector<int> P10 = {3, 5, 2, 7, 4, 10, 1, 9, 8, 6};
std::vector<int> P8 = {6, 3, 7, 4, 8, 5, 10, 9};
std::vector<int> IP = {2, 6, 3, 1, 4, 8, 5, 7};
std::vector<int> EP = {4, 1, 2, 3, 2, 3, 4, 1};
std::vector<int> P4 = {2, 4, 3, 1};
std::vector<int> IP_inv = {4, 1, 3, 5, 7, 2, 8, 6};
std::vector<std::vector<int>> S0 = {{1, 0, 3, 2},
{3, 2, 1, 0},
{0, 2, 1, 3},
{3, 1, 3, 2}};
std::vector<std::vector<int>> S1 = {{0, 1, 2, 3},
{2, 0, 1, 3},
{3, 0, 1, 0},
{2, 1, 0, 3}};
std::vector<int> shift(std::vector<int> &ar, int n) {
while (n > 0) {
int temp = ar[0];
for (size_t i = 0; i < ar.size() - 1; i++) {
ar[i] = ar[i + 1];
}
ar[ar.size() - 1] = temp;
n--;
}
return ar;
}
void key_generation() {
std::vector<int> key_(10);
for (size_t i = 0; i < 10; i++) {
key_[i] = key[P10[i] - 1];
}
std::vector<int> Ls(5), Rs(5);
for (size_t i = 0; i < 5; i++) {
Ls[i] = key_[i];
Rs[i] = key_[i + 5];
}
std::vector<int> Ls_1 = shift(Ls, 1);
std::vector<int> Rs_1 = shift(Rs, 1);
for (size_t i = 0; i < 5; i++) {
key_[i] = Ls_1[i];
key_[i + 5] = Rs_1[i];
}
key1.resize(8);
for (size_t i = 0; i < 8; i++) {
key1[i] = key_[P8[i] - 1];
}
std::vector<int> Ls_2 = shift(Ls, 2);
std::vector<int> Rs_2 = shift(Rs, 2);
for (size_t i = 0; i < 5; i++) {
key_[i] = Ls_2[i];
key_[i + 5] = Rs_2[i];
}
key2.resize(8);
for (size_t i = 0; i < 8; i++) {
key2[i] = key_[P8[i] - 1];
}
std::cout << "Your Key-1 :" << std::endl;
for (size_t i = 0; i < 8; i++)
std::cout << key1[i] << " ";
std::cout << std::endl;
std::cout << "Your Key-2 :" << std::endl;
for (size_t i = 0; i < 8; i++)
std::cout << key2[i] << " ";
}
std::vector<int> function_(std::vector<int> &ar, std::vector<int> &key_) {
std::vector<int> l(4), r(4);
for (size_t i = 0; i < 4; i++) {
l[i] = ar[i];
r[i] = ar[i + 4];
}
std::vector<int> ep(8);
for (size_t i = 0; i < 8; i++) {
ep[i] = r[EP[i] - 1];
}
for (size_t i = 0; i < 8; i++) {
ar[i] = key_[i] ^ ep[i];
}
std::vector<int> l_1(4), r_1(4);
for (size_t i = 0; i < 4; i++) {
l_1[i] = ar[i];
r_1[i] = ar[i + 4];
}
int row, col, val;
row = std::stoi(std::to_string(l_1[0]) + std::to_string(l_1[3]), 0, 2);
col = std::stoi(std::to_string(l_1[1]) + std::to_string(l_1[2]), 0, 2);
val = S0[row][col];
std::string str_l = binary_(val);
row = std::stoi(std::to_string(r_1[0]) + std::to_string(r_1[3]), 0, 2);
col = std::stoi(std::to_string(r_1[1]) + std::to_string(r_1[2]), 0, 2);
val = S1[row][col];
std::string str_r = binary_(val);
std::vector<int> r_(4);
for (size_t i = 0; i < 2; i++) {
char c1 = str_l[i];
char c2 = str_r[i];
r_[i] = c1 - '0';
r_[i + 2] = c2 - '0';
}
std::vector<int> r_p4(4);
for (size_t i = 0; i < 4; i++) {
r_p4[i] = r_[P4[i] - 1];
}
for (size_t i = 0; i < 4; i++) {
l[i] = l[i] ^ r_p4[i];
}
std::vector<int> output(8);
for (size_t i = 0; i < 4; i++) {
output[i] = l[i];
output[i + 4] = r[i];
}
return output;
}
std::vector<int> swap(std::vector<int> &array, size_t n) {
std::vector<int> l(n), r(n);
for (size_t i = 0; i < n; i++) {
l[i] = array[i];
r[i] = array[i + n];
}
std::vector<int> output(2 * n);
for (size_t i = 0; i < n; i++) {
output[i] = r[i];
output[i + n] = l[i];
}
return output;
}
std::vector<int> encryption(std::vector<int> plaintext) {
std::vector<int> arr(8);
for (size_t i = 0; i < 8; i++) {
arr[i] = plaintext[IP[i] - 1];
}
std::vector<int> arr1 = function_(arr, key1);
std::vector<int> after_swap = swap(arr1, arr1.size() / 2);
std::vector<int> arr2 = function_(after_swap, key2);
std::vector<int> ciphertext(8);
for (size_t i = 0; i < 8; i++) {
ciphertext[i] = arr2[IP_inv[i] - 1];
}
return ciphertext;
}
std::string binary_(int val) {
if (val == 0)
return "00";
else if (val == 1)
return "01";
else if (val == 2)
return "10";
else
return "11";
}
std::vector<int> decryption(std::vector<int> ar) {
std::vector<int> arr(8);
for (size_t i = 0; i < 8; i++) {
arr[i] = ar[IP[i] - 1];
}
std::vector<int> arr1 = function_(arr, key2);
std::vector<int> after_swap = swap(arr1, arr1.size() / 2);
std::vector<int> arr2 = function_(after_swap, key1);
std::vector<int> decrypted(8);
for (size_t i = 0; i < 8; i++) {
decrypted[i] = arr2[IP_inv[i] - 1];
}
return decrypted;
}
};
int main() {
std::cout << std::endl;
std::cout << "Bhavesh Balwani A-46 CSE" << std::endl;
std::cout << std::endl;
DES obj;
obj.key_generation(); // Call the key generation function
std::vector<int> plaintext = {1, 0, 0, 1, 0, 1, 1, 1}; // 8-bit plaintext
std::cout << std::endl;
std::cout << "Your plain Text is :" << std::endl;
for (size_t i = 0; i < 8; i++) // Printing the plaintext
std::cout << plaintext[i] << " ";
std::vector<int> ciphertext = obj.encryption(plaintext);
std::cout << std::endl;
std::cout << "Your cipher Text is :" << std::endl; // Printing the ciphertext
for (size_t i = 0; i < 8; i++)
std::cout << ciphertext[i] << " ";
std::vector<int> decrypted = obj.decryption(ciphertext);
std::cout << std::endl;
std::cout << "Your decrypted Text is :" << std::endl; // Printing the decrypted text
for (size_t i = 0; i < 8; i++)
std::cout << decrypted[i] << " ";
return 0;
}
```
![[Pasted image 20231211231104.png]]