# Strong Encryption

This challenge was more like reverse engineering
We have to decrypt the given hash and find the password

```
 <?php

    // Decrypt -> 576e78697e65445c4a7c8033766770357c3960377460357360703a6f6982452f12f4712f4c769a75b33cb995fa169056168939a8b0b28eafe0d724f18dc4a7

    $flag="";

    function encrypt($str,$enKey){

        $strHex='';
        $Key='';
        $rKey=69;
        $tmpKey='';

        for($i=0;$i<strlen($enKey);$i++){
            $Key.=ord($enKey[$i])+$rKey;
            $tmpKey.=chr(ord($enKey[$i])+$rKey);
        }

        $rKeyHex=dechex($rKey);

        $enKeyHash = hash('sha256',$tmpKey);

        for ($i=0,$j=0; $i < strlen($str); $i++,$j++){
            if($j==strlen($Key)){
                $j=0;
            }
            $strHex .= dechex(ord($str[$i])+$Key[$j]);
        }
        $encTxt = $strHex.$rKeyHex.$enKeyHash;
        return $encTxt;
    }

    $encTxt = encrypt($flag, "VishwaCTF");

    echo $encTxt;

?>
```

From the given source we can see that here, the enrypted hash is a combination of `ciphered flag` + `key` + `sha256(tempkey)`

```php
$encTxt = $strHex.$rKeyHex.$enKeyHash;
```

Now we know that the last 64 bytes are just sha256 of the tempkey we don't need that and then 2 bytes of key so the useful part of the given hash are starting bytes.

I wrote this code to decrypt the flag

```py
hello = "576e78697e65445c4a7c8033766770357c3960377460357360703a6f6982452f12f4712f4c769a75b33cb995fa169056168939a8b0b28eafe0d724f18dc4a7"

strHex = hello[:-66] # excluding the sha256 64 bytes and 2 bytes of key

xxx = bytes(bytearray.fromhex(strHex))
print(strHex)
key = "155174184173188166136153139" # you can get the key from the source $key variable

flag = ""

for i, char in enumerate(xxx):
    flag += chr(char - int(key[i % len(key)]))

print(flag)
```

Boom we have our flag

```
576e78697e65445c4a7c8033766770357c3960377460357360703a6f6982
VishwaCTF{y0u_h4v3_4n_0p_m1nd}
```
