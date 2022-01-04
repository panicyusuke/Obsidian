# Python base64 transfomer

Base64のアルゴリズムについて軽く勉強してみた。
文字コードには対応していないが、基本的なロジックは完成。
規格や変換アルゴリズムはWikipediaを参考にした。  
デコードはサボった。  
Docstringもサボった。

# Base64の規格


1. 元データを6ビットずつに分割。
（6ビットに満たない分は0を追加して6ビットにする）
2. 各6ビットの値を変換表を使って4文字ずつ変換。
 (4文字に満たない分は = 記号を追加して4文字にする）
 
## 変換例（符号化）

`元データ`
### 文字列: 
`ABCDEFG`
### 16進表現: 
```41, 42, 43, 44, 45, 46, 47```
### 2進表現: 
```
0100 0001, 0100 0010, 0100 0011, 0100 0100, 0100 0101, 0100 0110, 0100 0111
```

1. 6ビットずつに分割
`010000 010100 001001 000011 010001 000100 010101 000110 010001 11`
2. 2ビット余るので、4ビット分0を追加して6ビットにする
`010000 010100 001001 000011 010001 000100 010101 000110 010001 110000`
3. 変換表により、4文字ずつ変換
`"QUJD", "REVG", "Rw"`
4. 2文字余るので、2文字分 = 記号を追加して4文字にする
`"QUJD", "REVG", "Rw=="`
5. Base64文字列
`"QUJDREVGRw=="`



## 変換表

|10進|2進|文字||10進|2進|文字||10進|2進|文字||10進|2進|文字|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|0|000000|A||16|010000|Q||32|100000|g||48|110000|w|
|1|000001|B||17|010001|R||33|100001|h||49|110001|x|
|2|000010|C||18|010010|S||34|100010|i||50|110010|y|
|3|000011|D||19|010011|T||35|100011|j||51|110011|z|
|4|000100|E||20|010100|U||36|100100|k||52|110100|0|
|5|000101|F||21|010101|V||37|100101|l||53|110101|1|
|6|000110|G||22|010110|W||38|100110|m||54|110110|2|
|7|000111|H||23|010111|X||39|100111|n||55|110111|3|
|8|001000|I||24|011000|Y||40|101000|o||56|111000|4|
|9|001001|J||25|011001|Z||41|101001|p||57|111001|5|
|10|001010|K||26|011010|a||42|101010|q||58|111010|6|
|11|001011|L||27|011011|b||43|101011|r||59|111011|7|
|12|001100|M||28|011100|c||44|101100|s||60|111100|8|
|13|001101|N||29|011101|d||45|101101|t||61|111101|9|
|14|001110|O||30|011110|e||46|101110|u||62|111110|+|
|15|001111|P||31|011111|f||47|101111|v||63|111111|/|



`table.json`
```json=
{
 "110000": "w", "110001": "x", "110101": "1", "110100": "0", 
 "010100": "U", "010101": "V", "001100": "M", "001101": "N",
 "011110": "e", "011111": "f", "001001": "J", "001000": "I",
 "011011": "b", "011010": "a", "000110": "G", "000111": "H",
 "000011": "D", "000010": "C", "100100": "k", "100101": "l",
 "111100": "8", "111101": "9", "100010": "i", "100011": "j",
 "101110": "u", "101111": "v", "111001": "5", "111000": "4", 
 "101011": "r", "101010": "q", "110011": "z", "110010": "y",
 "010010": "S", "010011": "T", "010111": "X", "010110": "W",
 "110110": "2", "110111": "3", "011000": "Y", "011001": "Z",
 "001111": "P", "001110": "O", "011101": "d", "011100": "c", 
 "001010": "K", "001011": "L", "101101": "t", "000000": "A",
 "000001": "B", "100111": "n", "100110": "m", "000101": "F",
 "000100": "E", "111111": "/", "111110": "+", "100001": "h", 
 "100000": "g", "010001": "R", "010000": "Q", "101100": "s", 
 "111010": "6", "111011": "7", "101000": "o", "101001": "p"
 }
```


### Code

```python=
import sys
import json

# Initialize
BINARY_TO_BASE64 = json.loads(open("table.json", "r").read())
BASE64_TO_BINARY = {v: k for k, v in BINARY_TO_BASE64.items()}


# Define function
def str2bin(input_data: str):
    """
    Args:
        input_data (str)
    Return:
        binary_data (binary)
    """
    binary_data = ""
    for s in input_data:
        binary_data += format(ord(s), "08b")
    return binary_data


def split(input_data: str, split_number: int, replace:str):
    """
    Args:
        input_data (str):
        split_number (int):
        replace woed (str):
    Returns:
        split_list (list):
    """
    split_list = []
    split_list = [input_data[i:i + split_number] for i in range(0, len(input_data), split_number)]
    mergine = (len(input_data) % split_number)
    if mergine:
        split_list[-1] = split_list[-1] + (replace * (split_number - mergine))
    return split_list


# Process
argvs = sys.argv
input_data = argvs[1]
print(f"{input_data=}\n")
# Encode
print("=========Encoding===========")
# Step 1 string to binary
b = f"{str2bin(input_data=input_data)}"
print(f"binary= {b}")
# Step 2 split binary and fill 0
splited = split(input_data=b, split_number=6, replace="0")
print(f"filled={splited}")

# Step 3 convert binary to Base64 format
converted = ""
for string in splited:
    converted += BINARY_TO_BASE64[string]
print(f"{converted=}")
# Step 4 fill blank to `=`
encoded = ""
for s in split(input_data=converted, split_number=4, replace="="):
    encoded += s

# Step 5 plint result
print(f"{encoded=}\n")

# Decode
print("=========Decoding======")
print(f"{encoded=}\n")
d = ""
for s in [i for i in encoded if i != "="]:
    d += BASE64_TO_BINARY[s]
decode = split(input_data=d, split_number=8, replace="")
print(f"filled= {decode}")
result = ""
for i, d in enumerate(decode):
    if len(d) != 8:
        del decode[i]
    result += chr(int(d, 2))
print(f"decoded= {result}")

```

### Usage

```bash

$python transfomer.py row_data
input_data='row_data'

=========Encoding===========
binary= 0111001001101111011101110101111101100100011000010111010001100001
filled=['011100', '100110', '111101', '110111', '010111', '110110', '010001', '100001', '011101', '000110', '000100']
converted='cm93X2RhdGE'
encoded='cm93X2RhdGE='

=========Decoding======
encoded='cm93X2RhdGE='

filled= ['01110010', '01101111', '01110111', '01011111', '01100100', '01100001', '01110100', '01100001', '00']
decoded= row_data


```
