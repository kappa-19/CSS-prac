```c++
#include <bits/stdc++.h>
using namespace std;
string RandomString(int len)
{
char alpha[26] = { 'A', 'B', 'C', 'D', 'E', 'F', 'G',
'H', 'I', 'J', 'K', 'L', 'M', 'N',
'O', 'P', 'Q', 'R', 'S', 'T', 'U',
'V', 'W', 'X', 'Y', 'Z' };
string key = "";
for (int i = 0; i<len; i++)
key = key + alpha[rand() % 26];
return key;
}
string stringEncryption(string text, string key)
{
string cipherText = "";
int cipher[key.length()];
for (int i = 0; i < key.length(); i++) {
cipher[i] = text.at(i) - 'A' + key.at(i) - 'A';
cipher[i] = cipher[i] % 26;
}
for (int i = 0; i < key.length(); i++) {
int x = cipher[i] + 'A';
cipherText += (char)x;
}
return cipherText;
}
string stringDecryption(string s, string key)
{
string plainText = "";
int plain[key.length()];
for (int i = 0; i < key.length(); i++) {
plain[i] = s.at(i) - 'A' - (key.at(i) - 'A');
}
for (int i = 0; i < key.length(); i++) {
if (plain[i] < 0) {
plain[i] = plain[i] + 26;
}
}
for (int i = 0; i < key.length(); i++) {
int x = plain[i] + 'A';
plainText += (char)x;
}
return plainText;
}
int main()
{

string plainText;
getline(cin,plainText);
// string key;
// getline(cin,key);
int len = plainText.length();
for(int i=0; i<len; i++){
plainText[i] = toupper(plainText[i]);
}
string key = RandomString(len);
cout << "key - " << key << endl;
// if(plainText.length() != key.length()){
// cout << "Length is not same" << endl;
// return 0;
// }
string encryptedText = stringEncryption(plainText,key);
cout << "Cipher Text - " << encryptedText << endl;
cout << "Message - " << stringDecryption(encryptedText, key);
return 0;
}
```
OUTPUT :
![[images/Pasted image 20231211225752.png]]
![[images/Pasted image 20231211225806.png]]
